# AutoML for NLP in Azure ML

## What is AutoML for NLP?

**AutoML (Automated Machine Learning)** in Azure ML already supports tabular data and computer vision tasks. Now it also supports **NLP (Natural Language Processing)** — meaning you can train text-based AI models without manually choosing algorithms, tuning hyperparameters, or writing complex training code.

You give it:
- A labeled text dataset
- A task type
- A compute cluster

Azure ML does the rest — preprocessing, featurization (using BERT under the hood), training, and evaluation — all automated.

---

## The Three NLP Tasks Supported

AutoML for NLP currently supports three distinct text tasks:

### 1. Multi-Class Text Classification

**What it does:** Assigns **one label** from multiple possible labels to each text sample.

**Example:**
- Input: *"Tom Brady is a great quarterback"*
- Possible Labels: `Sports`, `Politics`, `Technology`, `Entertainment`
- Output: → `Sports`

**Dataset format (CSV):**
```
text,label
"I love watching Chicago Bulls games","NBA"
"Tom Brady is a great player","NFL"
"The rocket reached orbit successfully","Space"
```
Two columns: `text` (the input) and `label` (one class per row).

---

### 2. Multi-Label Text Classification

**What it does:** Assigns **one or more labels** to each text sample — a single text can belong to multiple categories simultaneously.

**Example:**
- Input: *"This romantic comedy made me laugh and cry"*
- Output: → `["Comedy", "Romance"]`

**Dataset format (CSV):**
```
text,label
"This is a Python tutorial for beginners","Python,Programming,Tutorial"
"Advanced deep learning with PyTorch","PyTorch,Deep Learning,Advanced"
```
Labels are comma-separated within a string, or as a Python list format.

---

### 3. Named Entity Recognition (NER)

**What it does:** Identifies and classifies **specific entities** within text — names, places, drug names, dates, organizations, etc.

**Example:**
- Input: *"Patient was prescribed Ibuprofen 400mg by Dr. Smith in New York"*
- Output: `Ibuprofen` → Drug, `400mg` → Dosage, `Dr. Smith` → Doctor, `New York` → Location

**Dataset format:**
Each word and its entity label are on separate rows, space-separated:
```
Hudson  B-LOC
Square  I-LOC
is      O
a       O
famous  O
place   O
```

Labels use **BIO tagging**:
- `B-` = Beginning of an entity
- `I-` = Inside (continuation of) an entity
- `O` = Outside any entity (not an entity)

---

## Data Requirements

Before training, your dataset must meet these minimums:

| Requirement | All NLP Tasks | Multi-Class/Multi-Label Specific | NER Specific |
|---|---|---|---|
| Minimum training samples | ≥ 50 rows | Each class must have multiple examples | Each entity type must appear multiple times |
| Validation format | Same structure as training data | Same column names and label format | Same BIO tagging format |
| Text file format | `.csv` or `.txt` | Text + label column | Word + tag per row |
| Label format | Consistent and clean | Double-quoted strings | Labels must start with `B-` or `I-` or `O` |

> **Key rule:** If your dataset has fewer than 50 samples, AutoML for NLP will not run.

---

## Language Support — 104 Languages

AutoML for NLP supports **104 languages** out of the box. The language is specified in the AutoML configuration:

| Language Setting | Backend Model Used |
|---|---|
| `en` (default — English) | BERT (base) |
| `de` (German) | BERT (German) |
| `multilingual` | Multilingual BERT |

**Why BERT?**
BERT (Bidirectional Encoder Representations from Transformers) is a state-of-the-art pretrained language model from Google. Instead of training from scratch, AutoML uses BERT as the **base model** and fine-tunes it on your specific dataset — this is called **transfer learning**. It dramatically reduces the amount of data and compute time needed to get a high-quality NLP model.

---

## How Distributed Training Works Here

> Important distinction from regular AutoML:

In **tabular AutoML**, Azure ML trains **many different algorithms** (LightGBM, XGBoost, RandomForest, etc.) and finds the best one.

In **NLP AutoML**, there is essentially **one model architecture** (BERT-based). The automation is in:
- Featurization pipeline setup
- Hyperparameter search (learning rate, epochs, batch size)
- Preprocessing steps

**Distributed training** in NLP AutoML works by **partitioning the training data** across multiple GPU nodes — each node trains on a portion of the data, and the gradients are aggregated. This speeds up the overall training without changing the model architecture.

```
Your Training Data (18,000 rows)
         ↓
  Split across N GPU nodes
  ┌──────────┐ ┌──────────┐ ┌──────────┐
  │ Node 1   │ │ Node 2   │ │ Node 3   │
  │ 6,000 rows│ │ 6,000 rows│ │ 6,000 rows│
  │ Training  │ │ Training  │ │ Training  │
  └──────────┘ └──────────┘ └──────────┘
         ↓ Gradients aggregated ↓
     Single trained BERT model
```

> **Use a GPU cluster** for NLP AutoML — CPU is too slow for BERT-based training.

---

## Step-by-Step Code Walkthrough

### The Use Case
Classify news posts into topics: **Baseball**, **Hockey**, **Graphics**, **Space**  
Dataset: **20 Newsgroups** (from scikit-learn) — ~18,000 posts across 20 topics  
Task: **Multi-Class Text Classification**

---

### Step 1: Import Libraries and Connect to Workspace

```python
from azureml.core import Workspace, Experiment, Dataset, Datastore
from azureml.train.automl import AutoMLConfig
from sklearn.datasets import fetch_20newsgroups
import pandas as pd
import os

# Connect to Azure ML Workspace
ws = Workspace.from_config()

# Name the experiment for tracking
experiment_name = "automl-nlp-text-multiclass"
experiment = Experiment(ws, experiment_name)

print(f"✅ Connected to workspace: {ws.name}")
print(f"✅ Experiment: {experiment_name}")
```

---

### Step 2: Create and Configure GPU Compute Cluster

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.exceptions import ComputeTargetException

CLUSTER_NAME = "gpu-nlp-cluster"

try:
    gpu_cluster = ComputeTarget(workspace=ws, name=CLUSTER_NAME)
    print("✅ Found existing GPU cluster")

except ComputeTargetException:
    config = AmlCompute.provisioning_configuration(
        vm_size="Standard_NC6",    # GPU VM — required for BERT training
        max_nodes=1                # Can increase for more distributed speed
    )
    gpu_cluster = ComputeTarget.create(ws, CLUSTER_NAME, config)
    gpu_cluster.wait_for_completion(show_output=True)
    print("✅ GPU cluster created")
```

> **Why GPU?** BERT has hundreds of millions of parameters. Training on CPU would take hours or days. A single GPU node reduces this to minutes.

---

### Step 3: Load and Prepare the Dataset

```python
# Load 20 Newsgroups — filter to 4 topics only
target_topics = ['rec.sport.baseball', 'rec.sport.hockey', 'comp.graphics', 'sci.space']

def prepare_dataset(subset, target_topics):
    """Fetch data, clean it, return a DataFrame with 'x' (text) and 'y' (label)."""
    data = fetch_20newsgroups(
        subset=subset,
        categories=target_topics,
        remove=('headers', 'footers', 'quotes')  # Remove metadata noise
    )
    df = pd.DataFrame({'x': data.data, 'y': data.target})
    df['x'] = df['x'].str.strip()     # Remove extra whitespace
    df = df[df['x'] != '']            # Remove blank posts
    return df

# Labels: 0=baseball, 1=hockey, 2=graphics, 3=space
train_df = prepare_dataset('train', target_topics).sample(200)    # 200 training rows
val_df   = prepare_dataset('test',  target_topics).sample(100)    # 100 validation rows
test_df  = prepare_dataset('test',  target_topics).sample(100)    # 100 test rows

print(f"Training rows:   {len(train_df)}")
print(f"Validation rows: {len(val_df)}")
print(f"Test rows:       {len(test_df)}")

# Preview the data
print(train_df.head(3))
```

**Output preview:**
```
                                                   x  y
0   Great game last night! The Bulls finally won...  0
1   Anyone know why the X graphics card drivers...  2
2   The shuttle launch was delayed again due to...  3
```

---

### Step 4: Save and Register Datasets in Azure ML

```python
# Save to local CSV files
os.makedirs('data', exist_ok=True)
train_df.to_csv('data/train.csv', index=False)
val_df.to_csv('data/val.csv',   index=False)
test_df.to_csv('data/test.csv', index=False)

# Upload to default Azure ML datastore
default_store = ws.get_default_datastore()

# Upload each split
default_store.upload('data/train.csv', target_path='nlp-data/train', overwrite=True)
default_store.upload('data/val.csv',   target_path='nlp-data/val',   overwrite=True)
default_store.upload('data/test.csv',  target_path='nlp-data/test',  overwrite=True)

# Register as Azure ML Tabular Datasets
train_dataset = Dataset.Tabular.from_delimited_files(
    path=(default_store, 'nlp-data/train')
).register(ws, name='nlp-train-dataset', create_new_version=True)

val_dataset = Dataset.Tabular.from_delimited_files(
    path=(default_store, 'nlp-data/val')
).register(ws, name='nlp-val-dataset', create_new_version=True)

test_dataset = Dataset.Tabular.from_delimited_files(
    path=(default_store, 'nlp-data/test')
).register(ws, name='nlp-test-dataset', create_new_version=True)

print("Datasets registered in Azure ML")
```

---

### Step 5: Configure AutoML for NLP

```python
from azureml.train.automl import AutoMLConfig

# Target and feature column names (must match CSV headers)
LABEL_COLUMN = 'y'    # The column AutoML will predict
TEXT_COLUMN  = 'x'    # The column containing text input

automl_config = AutoMLConfig(
    task='text-classification',          # NLP task type
                                         # Options: 'text-classification'
                                         #          'text-classification-multilabel'
                                         #          'text-ner'

    primary_metric='accuracy',           # Metric to optimize

    compute_target=gpu_cluster,          # Must be GPU for BERT

    training_data=train_dataset,         # Registered training dataset
    validation_data=val_dataset,         # Registered validation dataset

    label_column_name=LABEL_COLUMN,      # Which column is the target label

    # Language setting (default = English/BERT)
    # For other languages: featurization={'dataset_language': 'de'} for German
    # For multilingual:    featurization={'dataset_language': 'multilingual'}

    enable_dnn=True,                     # Enable deep neural network (BERT) featurization

    experiment_timeout_minutes=60,        # Max time to run the experiment
)

print("✅ AutoML NLP config defined")
```

---

### Step 6: Submit the Training Run

```python
# Submit the AutoML run
automl_run = experiment.submit(automl_config, show_output=True)
automl_run.wait_for_completion(show_output=True)

print("✅ AutoML NLP training complete!")
```

**What happens behind the scenes:**
```
1. Azure ML provisions the GPU cluster
2. BERT is loaded as the base pretrained model
3. Your training text is tokenized (split into subword tokens)
4. BERT is fine-tuned on your labeled data
5. The model is evaluated on validation data
6. Best model checkpoint is saved
7. A score.py scoring script is auto-generated
```

---

### Step 7: Retrieve Validation Metrics

```python
# Get the best run's metrics
best_run = automl_run.get_best_child()
metrics = best_run.get_metrics()

print("📊 Validation Metrics:")
for metric, value in metrics.items():
    print(f"  {metric}: {value}")
```

**Example output from the demo:**
```
📊 Validation Metrics:
  accuracy:           0.8182
  weighted_precision: 0.8201
  weighted_recall:    0.8182
  weighted_f1_score:  0.8178
```

~82% accuracy on a 4-class news classification task with only 200 training rows — achieved with zero manual model design.

---

### Step 8: Run Inference on the Test Dataset

AutoML automatically generates a `score.py` script with the best model. Use it to run predictions on the test set:

```python
from azureml.core import ScriptRunConfig, Environment

# Get the scoring script directory from the best run
best_run_id    = best_run.id
training_run_id = automl_run.id

# Arguments to pass to the scoring script
scoring_args = [
    "--run_id",          best_run_id,
    "--training_run_id", training_run_id,
    "--experiment_name", experiment_name,
    "--input_dataset",   "nlp-test-dataset",   # The registered test dataset name
    "--output_path",     "outputs/predictions.csv"
]

# Submit the scoring job
score_config = ScriptRunConfig(
    source_directory=".",
    script="score.py",          # Auto-generated by AutoML
    arguments=scoring_args,
    compute_target=gpu_cluster,
)

score_run = experiment.submit(score_config)
score_run.wait_for_completion(show_output=True)

print("Inference complete — predictions saved to outputs/predictions.csv")
```

---

### Step 9: Inspect the Predictions

```python
import pandas as pd

# Load the predictions output
predictions_df = pd.read_csv("outputs/predictions.csv")
print(predictions_df.head(10))
```

**Output structure:**
```
   x (text snippet)                      y (actual)  predicted_label  confidence
0  "The home run was incredible..."      0           0                0.94
1  "Shuttle launch was scrubbed..."      3           3                0.88
2  "OpenGL shader compilation error..."  2           2                0.91
3  "The goalie stopped every shot..."    1           3                0.61   ← wrong
```

Columns:
- `y` — actual label (ground truth)
- `predicted_label` — what the model predicted
- `confidence` — how confident the model was (0 to 1)

---

## Complete Flow Summary

```
Raw Text Data (news posts, scripts, medical notes, etc.)
         ↓
Prepare Dataset
  ├── Multi-class:  text + single label per row
  ├── Multi-label:  text + comma-separated labels per row
  └── NER:          word + BIO tag per word

         ↓
Register as Azure ML Datasets (train / val / test)
         ↓
Configure AutoMLConfig
  ├── task = 'text-classification' / 'text-classification-multilabel' / 'text-ner'
  ├── compute_target = GPU cluster
  ├── primary_metric = 'accuracy'
  └── enable_dnn = True  (activates BERT)

         ↓
Submit AutoML Run
  └── BERT fine-tuning runs on GPU cluster
  └── Validation metrics computed automatically
  └── score.py generated automatically

         ↓
Retrieve Best Model + Metrics
         ↓
Run Inference on Test Data using score.py
         ↓
Predictions CSV: [text | actual label | predicted label | confidence]
```

---

# Train & Score Thousands of ML Models in Parallel
### Many Models Solution — Azure ML | Concept Notes

---
---

## The Core Problem — Why One Model Is Not Always Enough

Traditional ML thinking: collect all data → train one model → deploy it everywhere.

This works fine when your data has **one unified pattern**. But real-world scenarios often have **many distinct patterns** that a single model cannot capture properly.

**Concrete examples where one model fails:**

| Scenario | Why One Model Fails |
|---|---|
| Retail demand forecasting across 4,000 stores | Store in Manhattan ≠ store in rural Texas — different customers, weather, habits |
| Wind turbine power output prediction | Each turbine sits in a different location with different wind conditions |
| Precision medicine / patient analytics | Each patient has a unique biological profile |
| YouTube viewer content preferences | What viewer A enjoys has nothing to do with viewer B |
| Orange juice sales by brand per store | Brand X in Store 100 has a completely different sales pattern than Brand X in Store 200 |

The solution: **train a dedicated ML model for each entity** — each store, each turbine, each patient. This is called the **Many Models** approach.

The challenge: doing this manually is impossible at scale. Training 12,000 models one at a time would take weeks and enormous manual effort.

Azure ML's **Many Models Solution** solves this by training all those models **simultaneously in parallel** across a cluster of compute nodes — and doing batch prediction the same way.

---

## The Architecture — How It Works

```
One Dataset with Many Groups
(e.g., Orange Juice Sales: 4,000 stores × 3 brands = 12,000 groups)
              ↓
     Partition / Split by Group
    ┌─────────────────────────────────────────────┐
    │ Store 1, Brand A │ Store 1, Brand B │ ...   │
    │ Store 2, Brand A │ Store 2, Brand B │ ...   │
    └─────────────────────────────────────────────┘
              ↓
   Compute Cluster (up to 20 nodes)
    ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐
    │  Node 1  │ │  Node 2  │ │  Node 3  │ │  Node 4  │
    │ Store 1A │ │ Store 1B │ │ Store 2A │ │ Store 2B │
    │ AutoML   │ │ AutoML   │ │ AutoML   │ │ AutoML   │
    │ → Model1 │ │ → Model2 │ │ → Model3 │ │ → Model4 │
    └──────────┘ └──────────┘ └──────────┘ └──────────┘
              ↓
   Models Registered in Azure ML Model Registry
    ┌─────────────────────────────────────────────┐
    │ model_store1_brandA  (algorithm: LightGBM)  │
    │ model_store1_brandB  (algorithm: XGBoost)   │
    │ model_store2_brandA  (algorithm: ElasticNet)│
    │ ...                                         │
    └─────────────────────────────────────────────┘
              ↓ (later, for batch prediction)
   Same Cluster, Each Node Uses Its Own Dedicated Model
    ┌──────────┐ ┌──────────┐ ┌──────────┐
    │  Node 1  │ │  Node 2  │ │  Node 3  │
    │ Model1   │ │ Model2   │ │ Model3   │
    │ predicts │ │ predicts │ │ predicts │
    │ Store 1A │ │ Store 1B │ │ Store 2A │
    └──────────┘ └──────────┘ └──────────┘
              ↓
   Combined Predictions Output (actual vs predicted per store/brand)
```

---

### Partitioning — The Heart of Many Models

Partitioning is how Azure ML splits your one big dataset into many smaller, group-specific datasets.

You define **partition columns** — the columns whose unique combinations define each group.

In the orange juice example:
- Partition column 1: `store` (which store)
- Partition column 2: `brand` (which juice brand)

Every unique combination of `(store, brand)` becomes its own dataset and gets its own model.

```
Full dataset (all stores, all brands)
         ↓  partition by [store, brand]
(Store1000, Dominick) → 102 rows → Model A
(Store1000, Tropicana) → 98 rows → Model B
(Store2000, Dominick) → 110 rows → Model C
...and so on for all combinations
```

### AutoML per Partition — Best Algorithm for Each Group

The really powerful part: for each partition, Azure ML runs **AutoML** independently. This means:
- Store 1 might get a LightGBM model (best for its data pattern)
- Store 2 might get an XGBoost model (best for its data pattern)
- Store 3 might get an Elastic Net model (best for its data pattern)

You don't choose the algorithm — AutoML finds the best one for each individual group automatically. Every model is the best possible model for its specific partition.

### Parallel Run Step — The Execution Engine

The `ParallelRunStep` is the Azure ML Pipeline step that powers Many Models. It:
- Takes all partitioned datasets as input
- Distributes them across nodes of a compute cluster
- Runs training (or scoring) on multiple partitions simultaneously
- Each node works independently — no coordination needed between nodes

This is different from distributed training (like with Ray/Dask) where multiple nodes cooperate on **one model**. Here, each node works on a completely **separate, independent model**.

---

## The Use Case — Orange Juice Sales Forecasting

**Goal:** Predict the weekly quantity of orange juice sold for each brand at each store.

**Dataset:** Azure Open Dataset — `OrangeJuiceSalesSimulated`
- ~4,000 stores
- 3 juice brands per store: Dominick, MinuteMaid, Tropicana
- Historical weekly sales data from 1992 onward
- Columns: `store`, `brand`, `weekStarting` (date), `quantity` (target), `price`, `revenue`

For the demo: 10 files (representing ~10 store/brand combinations) are used to keep training fast. The same approach scales to all 12,000 combinations without changing any code — just change the dataset.

---

## Step-by-Step Walkthrough

### Notebook Structure

The Many Models solution is organized into sequential notebooks:

```
00_Setup_AML_Workspace.ipynb     ← Connect, create cluster
01_Data_Preparation.ipynb        ← Download, split, upload, register data
02_AutoML_Training.ipynb         ← Configure and run parallel training
03_AutoML_Forecasting.ipynb      ← Batch prediction using trained models
```

---

### Notebook 00 — Workspace Setup & Compute Cluster

```python
from azureml.core import Workspace
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.exceptions import ComputeTargetException

# Connect to Azure ML Workspace
ws = Workspace.from_config()
print(f"Workspace: {ws.name}")

# Create a CPU compute cluster (up to 20 nodes for parallel training)
CLUSTER_NAME = "many-models-cluster"

try:
    cpu_cluster = ComputeTarget(workspace=ws, name=CLUSTER_NAME)
    print("Found existing cluster")

except ComputeTargetException:
    config = AmlCompute.provisioning_configuration(
        vm_size="STANDARD_DS3_V2",
        min_nodes=0,     # Scale to 0 when idle → save cost
        max_nodes=20     # Scale up to 20 nodes when all models train in parallel
    )
    cpu_cluster = ComputeTarget.create(ws, CLUSTER_NAME, config)
    cpu_cluster.wait_for_completion(show_output=True)
    print(f"Cluster created: {CLUSTER_NAME} (max 20 nodes)")
```

Why up to 20 nodes? Each node can train a separate model simultaneously. With 20 nodes, you can train 20 models at the same time. For 12,000 models, the cluster trains batches of 20 at a time, cycling through all of them.

---

### Notebook 01 — Data Preparation

#### Step 1: Download Orange Juice Data

```python
from azureml.opendatasets import OrangeJuiceSalesSimulated
import os

# Limit to 10 files for demo (change to None for all 12,000 combinations)
MAX_FILES = 10

oj_sales = OrangeJuiceSalesSimulated()
oj_sales_files = oj_sales.get_file_dataset()

# Download to local folder
os.makedirs("data/oj_sales", exist_ok=True)
oj_sales_files.download(target_path="data/oj_sales", overwrite=True)

print("Data downloaded")
print("Files downloaded:", os.listdir("data/oj_sales"))
```

Each downloaded file represents **one store + one brand** combination. For example:
- `Store1000_Dominick.csv` → Store 1000, Dominick brand
- `Store1000_Tropicana.csv` → Store 1000, Tropicana brand

File contents look like:
```
store   brand      weekStarting  quantity  price   revenue
1000    Dominick   1990-06-14    10        1.59    15.90
1000    Dominick   1990-06-21    13        1.59    20.67
...
```

#### Step 2: Split into Training and Inference Data

```python
import pandas as pd

def read_file(filepath):
    if filepath.endswith('.parquet'):
        return pd.read_parquet(filepath)
    return pd.read_csv(filepath)

def write_file(df, filepath):
    if filepath.endswith('.parquet'):
        df.to_parquet(filepath, index=False)
    else:
        df.to_csv(filepath, index=False)

# Create output folders
os.makedirs("data/training",   exist_ok=True)
os.makedirs("data/inference",  exist_ok=True)

# Split date: everything before this goes to training, after goes to inference
SPLIT_DATE = "1992-05-28"

for filename in os.listdir("data/oj_sales"):
    filepath = os.path.join("data/oj_sales", filename)
    df = read_file(filepath)

    # Convert date column
    df['weekStarting'] = pd.to_datetime(df['weekStarting'])

    # Split
    train_df = df[df['weekStarting'] < SPLIT_DATE]
    infer_df  = df[df['weekStarting'] >= SPLIT_DATE]

    # Write to respective folders
    write_file(train_df, f"data/training/{filename}")
    write_file(infer_df,  f"data/inference/{filename}")

print(f"Split complete")
print(f"   Training files: {len(os.listdir('data/training'))}")
print(f"   Inference files: {len(os.listdir('data/inference'))}")
```

For example, for Store1000/Dominick:
- Training data: 102 rows (historical weekly sales)
- Inference data: 19 rows (more recent weeks — to test predictions against actuals)

#### Step 3: Upload and Register in Azure ML

```python
from azureml.core import Dataset, Datastore

# Get the default datastore (blob storage)
default_store = ws.get_default_datastore()

# Upload training and inference data
default_store.upload("data/training",  target_path="oj-many-models/training",  overwrite=True)
default_store.upload("data/inference", target_path="oj-many-models/inference", overwrite=True)

print("Data uploaded to datastore")

# Register as Azure ML FileDatasets
# FileDataset (not Tabular) because each file is a separate model's data
train_dataset = Dataset.File.from_files(
    path=(default_store, "oj-many-models/training")
).register(ws, name="oj-training-small", create_new_version=True)

infer_dataset = Dataset.File.from_files(
    path=(default_store, "oj-many-models/inference")
).register(ws, name="oj-inference-small", create_new_version=True)

print(f"Registered: oj-training-small  (v{train_dataset.version})")
print(f"Registered: oj-inference-small (v{infer_dataset.version})")
```

Note: `Dataset.File` is used here (not `Dataset.Tabular`) because the Many Models solution treats each file as one partition — each file feeds one model.

---

### Notebook 02 — AutoML Training (Many Models)

#### Step 1: Define AutoML Configuration

```python
from azureml.train.automl import AutoMLConfig

automl_settings = AutoMLConfig(
    task='forecasting',                          # Predicting future values (time series)

    primary_metric='normalized_root_mean_squared_error',  # Metric AutoML optimizes for
                                                           # Lower NRMSE = better forecast

    iteration_timeout_minutes=10,                # Max time AutoML spends on one algorithm trial
    iterations=5,                                # Number of algorithm candidates to try per model
    experiment_timeout_hours=1,                  # Total max time for the whole experiment

    label_column_name='quantity',                # The column we want to predict

    n_cross_validations=3,                       # Cross-validation folds for model evaluation

    time_column_name='weekStarting',             # The datetime column (required for forecasting)

    max_horizon=20,                              # Predict 20 periods into the future
                                                 # (e.g., 20 weeks ahead)
)
```

Key parameters explained:

**`task='forecasting'`** — This is a time series forecasting task. AutoML will try algorithms like ARIMA, Prophet, LightGBM, ExponentialSmoothing, and more — all automatically.

**`time_column_name='weekStarting'`** — AutoML needs to know which column contains the time/date information to understand the temporal ordering of data.

**`max_horizon=20`** — How many steps into the future you want to predict beyond your training data. If your data ends in Week 50, the model will predict Weeks 51 through 70.

**`label_column_name='quantity'`** — The target column to predict. Every other column becomes a feature.

#### Step 2: Configure the Many Models Training Step

```python
from azureml.contrib.automl.pipeline.steps import AutoMLPipelineBuilder

# This is the key step that enables Many Models
many_models_train_step = AutoMLPipelineBuilder.get_many_models_train_step(
    experiment=experiment,
    automl_settings=automl_settings,
    train_data=train_dataset,              # The registered FileDataset with all partition files
    compute_target=cpu_cluster,            # The multi-node cluster

    partition_column_names=['store', 'brand'],  # How to group data into individual models
                                                 # Each unique (store, brand) pair = one model

    node_count=2,                          # How many cluster nodes to use simultaneously
    process_count_per_node=1,              # Parallel processes per node
    run_invocation_timeout=3700,           # Timeout per individual model training (seconds)
    max_concurrent_iterations=4,           # Max AutoML iterations running at the same time
)
```

**`partition_column_names`** is the most critical parameter. It tells the system: "Split the data into groups based on the unique values of these columns, and train a separate model for each group."

#### Step 3: Build and Submit the Training Pipeline

```python
from azureml.pipeline.core import Pipeline
from azureml.core import Experiment

# Wrap the many models step in an AML Pipeline
training_pipeline = Pipeline(ws, steps=[many_models_train_step])

# Create experiment for tracking
experiment = Experiment(ws, name="manymodel-training-pipeline")

# Submit — this triggers training of all models in parallel!
pipeline_run = experiment.submit(training_pipeline)
pipeline_run.wait_for_completion(show_output=True)

print("Many Models training complete!")
```

#### What Happens During Training

```
Pipeline Run starts
        ↓
Azure ML reads all files in the training FileDataset
        ↓
Groups files by (store, brand) → creates partitions
        ↓
Dispatches partitions to available cluster nodes
        ↓
Each node independently runs AutoML on its partition:
  - Tries LightGBM, XGBoost, ARIMA, Prophet, ElasticNet, etc.
  - Evaluates each using cross-validation (NRMSE)
  - Selects the best algorithm for that store/brand
  - Trains final model
  - Registers model in Azure ML Model Registry
        ↓
After all nodes finish their partitions, next batch dispatched
        ↓
Repeats until all partitions are trained
```

#### What You Get After Training

Going to Azure ML Studio → Models section shows all registered models:

```
model_store1000_dominick     → Algorithm: LightGBM        NRMSE: 0.23
model_store1000_tropicana    → Algorithm: ExponentialSmoothing  NRMSE: 0.31
model_store1000_minutemaid   → Algorithm: XGBoost         NRMSE: 0.28
model_store2000_dominick     → Algorithm: ElasticNet      NRMSE: 0.19
...
```

Each model is the **best possible algorithm for that specific store and brand** — not a one-size-fits-all model.

---

### Notebook 03 — Batch Prediction (Many Models Inference)

After all models are trained, you run batch predictions in parallel — each model predicts for its own partition simultaneously.

#### Step 1: Reference the Inference Dataset

```python
from azureml.core import Dataset

# Load the inference dataset registered earlier
infer_dataset = Dataset.get_by_name(ws, name='oj-inference-small')

print(f"Loaded inference dataset: {infer_dataset.name} v{infer_dataset.version}")
```

#### Step 2: Get the Training Run Details

The batch prediction step needs to know which training run produced the models to use.

```python
from azureml.core import Experiment, Run

# Get the experiment
training_experiment = Experiment(ws, "manymodel-training-pipeline")

# Get the specific run ID from the training pipeline run
TRAINING_RUN_ID = "your-training-run-id"  # Copy from the training experiment run details

training_run = Run(training_experiment, run_id=TRAINING_RUN_ID)
```

How to find the Run ID: Azure ML Studio → Experiments → manymodel-training-pipeline → click on the run → copy the Run ID shown in the run details.

#### Step 3: Configure and Submit Batch Prediction

```python
from azureml.contrib.automl.pipeline.steps import AutoMLPipelineBuilder

# Define the forecasting/scoring step
many_models_forecast_step = AutoMLPipelineBuilder.get_many_models_batch_inference_step(
    experiment=experiment,
    inference_data=infer_dataset,              # The data to run predictions on
    compute_target=cpu_cluster,

    partition_column_names=['store', 'brand'], # Must match training partition columns

    # Link back to the training run so the right models are used
    train_run_id=TRAINING_RUN_ID,
    train_experiment_name="manymodel-training-pipeline",

    node_count=2,                              # Nodes used for parallel prediction
    process_count_per_node=1,
    run_invocation_timeout=3700,

    output_file_name="predictions.csv"
)

# Build and submit the prediction pipeline
forecast_pipeline = Pipeline(ws, steps=[many_models_forecast_step])
forecast_experiment = Experiment(ws, name="manymodel-forecasting-pipeline")

forecast_run = forecast_experiment.submit(forecast_pipeline)
forecast_run.wait_for_completion(show_output=True)

print("Batch prediction complete!")
```

#### Step 4: Retrieve and Review Predictions

```python
import pandas as pd

# Download the output predictions
forecast_run.download_file(
    name="predictions.csv",
    output_file_path="outputs/predictions.csv"
)

# Load and preview results
predictions_df = pd.read_csv("outputs/predictions.csv")
print(predictions_df.head(10))
```

**Output format:**
```
store    brand      weekStarting  quantity  predicted_quantity
1000     Dominick   1992-05-28    10        11.3
1000     Dominick   1992-06-04    13        12.1
1000     Tropicana  1992-05-28    8         7.8
2000     Dominick   1992-05-28    22        20.4
...
```

Each row shows:
- `quantity` → the actual real value
- `predicted_quantity` → what the dedicated model for that store/brand predicted

---

## Scheduling for Continuous Retraining

Once you have both pipelines working, you can publish and schedule them for fully automated, recurring retraining and prediction.

```python
from azureml.pipeline.core.schedule import ScheduleRecurrence, Schedule, TimeUnit
from azureml.pipeline.core import PublishedPipeline

# Publish the training pipeline (gives it a REST endpoint)
published_training_pipeline = training_pipeline.publish(
    name="Many-Models-Training",
    description="Trains one model per store/brand combination",
    version="1.0"
)

# Schedule: retrain all models every month
monthly_retrain = Schedule.create(
    workspace=ws,
    name="monthly-many-models-retrain",
    pipeline_id=published_training_pipeline.id,
    experiment_name="manymodel-training-pipeline",
    recurrence=ScheduleRecurrence(frequency=TimeUnit.Month, interval=1)
)

print(f" Monthly retraining scheduled")
print(f"   Pipeline endpoint: {published_training_pipeline.endpoint}")
```

Or trigger on new data arrival:
```python
# Trigger retraining whenever new data lands in the datastore
data_trigger_schedule = Schedule.create(
    workspace=ws,
    name="data-driven-many-models-retrain",
    pipeline_id=published_training_pipeline.id,
    experiment_name="manymodel-training-pipeline",
    datastore=ws.get_default_datastore(),
    path_on_datastore="oj-many-models/training"   # Watch this folder for new files
)
```

---

## Complete Flow Summary

```
YOUR PROBLEM:
  Need one precise model per store / customer / turbine / patient
              ↓
APPROACH: Many Models on Azure ML
              ↓
┌── DATA PREP ───────────────────────────────────────────────┐
│ 1. Download raw data (all stores, all brands)              │
│ 2. Split into training (historical) + inference (recent)   │
│ 3. Upload to Azure Datastore                               │
│ 4. Register as FileDataset in Azure ML                     │
└────────────────────────────────────────────────────────────┘
              ↓
┌── TRAINING ────────────────────────────────────────────────┐
│ 5. Configure AutoML (task, metric, target column, horizon) │
│ 6. Define partition columns (store, brand)                 │
│ 7. Set up AutoMLPipelineBuilder many_models_train_step     │
│ 8. Submit Pipeline → cluster trains N models in parallel   │
│    Each node: AutoML finds best algorithm → registers model│
└────────────────────────────────────────────────────────────┘
              ↓
┌── RESULTS ─────────────────────────────────────────────────┐
│ 9. N models registered in Azure ML Model Registry          │
│    Each model is best-fit algorithm for its partition      │
└────────────────────────────────────────────────────────────┘
              ↓
┌── BATCH PREDICTION ────────────────────────────────────────┐
│ 10. Load inference FileDataset                             │
│ 11. Define many_models_batch_inference_step                │
│ 12. Submit forecast pipeline → parallel scoring            │
│     Each node uses its dedicated model to predict          │
│ 13. Output: predictions.csv (actual vs predicted per group)│
└────────────────────────────────────────────────────────────┘
              ↓
┌── AUTOMATION ──────────────────────────────────────────────┐
│ 14. Publish pipelines → REST endpoints                     │
│ 15. Schedule: time-based (monthly) or data-triggered       │
│     → fully automated retraining + prediction forever      │
└────────────────────────────────────────────────────────────┘
```
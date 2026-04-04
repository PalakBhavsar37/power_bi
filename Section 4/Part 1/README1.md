# Visualization & Report Basics

Report page is also known as canvas.

# Creating and Managing Visuals

## Build visual pane
- Click anywhere on the blank canvas and select a chart type from the Visualizations tab.
![alt text](image.png)
![alt text](image-1.png)

- Drag, drop, and move the newly generated visual anywhere on the report page.
![alt text](image-2.png)

- Use the Build visual tab to populate the chart by adding data columns (e.g.,In fields pane, selecting "Driver Name" from the Drivers table and "Points" from the Results table).
![alt text](image-3.png)
![alt text](image-4.png)
here, 'points' are aggregated.
Data values are often aggregated automatically (e.g., summing up the total points).

- Sort the visual quickly by clicking on the column headers (e.g., clicking "Points" to place the driver with the highest points at the top).
![alt text](image-5.png)

- Copy visuals by right-clicking and selecting Copy -> Copy visual, then holding Ctrl and clicking the canvas to paste. (here's a visual on top of first) Alternatively, use standard Ctrl + C and Ctrl + V keyboard shortcuts.
![alt text](image-6.png)

- Changing a visual's chart type (e.g., from a table to a bar chart) will change how it looks and the specific formatting options available to it.

# Formatting Visual Data (Format Visual Pane)

## format visual pane
![alt text](image-7.png)

- **Style Presets:** Apply quick visual themes from a dropdown list (e.g., Default, Minimal, or Sparse but first select the visual).
![alt text](image-8.png)
![alt text](image-9.png)

- **Grid Lines:** Toggle horizontal and vertical grid lines on or off.

Remove grid lines
![alt text](image-11.png)
![alt text](image-10.png)

Customize their specific color and line width.
![alt text](image-12.png)
![alt text](image-13.png)

Add vertical grid lines.
![alt text](image-14.png)
![alt text](image-15.png)

- **Borders:** Add distinct borders to the top, bottom, left, or right of the visual elements (e.g., adding a red border with a width of 2).
![alt text](image-16.png)
![alt text](image-17.png)

- **Values:** Customize the data rows by altering the font type, text size, text color, and background color.
![alt text](image-18.png)
![alt text](image-19.png)

- **Column Headers:** Adjust the font and size of the top header text independently from the values.
![alt text](image-20.png)
![alt text](image-21.png)

- **Totals:** Toggle the bottom totals row on or off. 
![alt text](image-22.png)
![alt text](image-23.png)

You can rename the label (e.g., "Total Points"), change the font size, apply italics, and adjust text color.
![alt text](image-24.png)
![alt text](image-25.png)

- **Specific Columns:** Format columns individually. For example, select the "Points" column to 

adjust display units (e.g., showing as thousands), 
![alt text](image-26.png)
![alt text](image-27.png)

change decimal places (e.g., setting it to 0), 
![alt text](image-28.png)
![alt text](image-29.png)

or change the text alignment.
![alt text](image-30.png)
![alt text](image-31.png)

- **Renaming Fields:** In the Build visual tab under Columns, right-click a field to rename it purely for presentation (e.g., changing "driver_name" to "Driver"). 
![alt text](image-42.png)
![alt text](image-43.png)
![alt text](image-44.png)
![alt text](image-45.png)

This updates the visual but leaves the underlying dataset entirely unchanged.
![alt text](image-46.png)

---

# General Settings & Global Formatting

**General Tab:** Located inside the format pane for overall container customizations.
![alt text](image-32.png)
![alt text](image-33.png)

- **Title:** Turn the chart title on, input custom text (e.g., "Total Points by Driver"), and modify the font size (e.g., changing from 30 to 20), italics, alignment (center), and background color.
![alt text](image-34.png)
![alt text](image-35.png)

- **Effects:** Apply a visual border to the entire chart container or add a drop shadow.

bg color
![alt text](image-36.png)
![alt text](image-37.png)

visual border
![alt text](image-38.png)
![alt text](image-39.png)

shadow
![alt text](image-40.png)
![alt text](image-41.png)

- **Analytics Tab:** Automatically generates data insights. Note that this feature is only available for certain chart types, not all basic visuals.
![alt text](image-47.png)

# Canvas and Page Formatting

**Format page tab**
Access page formatting by clicking on any empty, blank area of the canvas and opening the Format page tab under Visualizations.
![alt text](image-48.png)

- **Page Information:** Rename the specific report page (e.g., changing "Page 1" to "Driver Analysis").
![alt text](image-49.png)
![alt text](image-50.png)

- **Canvas Settings:** Alter the aspect ratio and overall dimensions of the report page.
![alt text](image-51.png)
![alt text](image-52.png)

- **Canvas Background:** Change the background color of the report. You must set the transparency level to 0% to make the color fully visible.
![alt text](image-53.png)
![alt text](image-54.png)

- **Images & Wallpaper:** Upload images to act as the canvas background or add a wallpaper to surround the report.

---

# View Tab, Selection Pane, and Alignment

## View Tab
![alt text](image-55.png)

click : selection
![alt text](image-56.png)
![alt text](image-58.png)

- **Selection Pane:** It manages all elements on the screen and features two distinct ordering systems: Layer order and Tab order.
![alt text](image-57.png)

- **Layer Order (Z-axis):** Determines which visuals are stacked on top of each other. Dragging a visual to the top of the list will ensure it overlaps everything beneath it.
![alt text](image-59.png)

current
![alt text](image-60.png)

If I take one visual and place it on top of other visual,
![alt text](image-61.png)
you can see that currently the layer order in this table which have moved to 'Total points by driver' is at the top.
![alt text](image-62.png)

If I change the ordering to the bottom, you can see now this visual, is on top.
![alt text](image-63.png)
![alt text](image-64.png)

- **Tab Order:** Dictates the exact sequence in which report elements are highlighted when a user navigates the dashboard using the Tab key.

current order
![alt text](image-65.png)

Click somewhere else on the screen and I just press tab
![alt text](image-66.png)
that's the first visual that gets selected.

![alt text](image-67.png) ![alt text](image-68.png)
That's the second visual

then the other.

-> change the order
![alt text](image-69.png)
press tab to see the order is different now.

- **Format Painter:** 
If you want all elements to have the same format in your report.
Click a fully formatted visual -> Home -> Format Painter icon -> click an unformatted visual to instantly duplicate all styling and formatting.
![alt text](image-70.png)
![alt text](image-71.png)
![alt text](image-72.png)

- **Themes:** Access the View tab dropdown to select from a Theme Gallery.
![alt text](image-73.png)
![alt text](image-74.png)
we can customize the current theme, browse your computer for theme files, and save custom themes.

---

## Format Tab
Copy the formatting and make all 3 visuals same.

- **Alignment Tools:** Select multiple elements, go to Format -> Align, and arrange them perfectly (e.g., Align Top, Distribute Horizontally, Distribute Vertically). This operates identically to PowerPoint.

![alt text](image-75.png)
![alt text](image-76.png)
![alt text](image-77.png)
![alt text](image-78.png)

Under Format, use actions like "Send backward", "Bring forward" to manually arrange overlapping visuals.

![alt text](image-79.png)
![alt text](image-80.png)
![alt text](image-81.png)

![alt text](image-82.png)
![alt text](image-83.png)

---

- **Gridlines & Snapping:** Turn on canvas gridlines to manually align objects. Enable "Snap to grid" to force visuals to lock precisely into grid intersections rather than moving freely.

-> Add Gridlines
View -> Tick Gridlines
![alt text](image-84.png)
![alt text](image-85.png)

this can help to align objects as well using these grid lines
![alt text](image-86.png)

Tick 'snap to grid'
![alt text](image-87.png)

- **Lock Objects:** Toggle this feature on to freeze all report elements in place, preventing accidental moving or resizing when clicking around the canvas.
![alt text](image-88.png)

---

# Table and Matrix Visuals

lets create a new page & rename it to 'Tables and Matrix' (by double clicking)
![alt text](image-89.png) ![alt text](image-90.png)

# Preparation & Layout

Before building visuals, it is helpful to set up canvas for easy alignment:

- Go to the View tab.
- Turn on Gridlines and select Snap to grid. This makes arranging and aligning multiple visualizations much easier.
![alt text](image-91.png)

# Table Visualizations

- **Creating a Table:** Click the Table icon in the Visualizations pane.

- **Adding Data:** Drag fields into the Columns section. For example: Constructor Name (from Constructors table) and Points (from Results table). Note that numeric fields like "Points" default to the Sum aggregation.
![alt text](image-92.png)
![alt text](image-93.png)
The table shows - number of points by each constructor 

- **Sorting Data:**
  - * Quick Sort: Click directly on a column header (like "Points") to alternate between ascending and descending order.

  first time click on 'points' column.
  ![alt text](image-94.png)

  first time click on 'points' column.
  ![alt text](image-95.png)

  - Menu Sort: Click the three dots (...) at the top right of the visual -> Sort descending or Sort ascending -> Choose the specific field to sort by.

  ![alt text](image-96.png)

- **Adding More Columns:** You can add additional fields, like Driver Name, and rearrange their order in the Build visual pane (e.g., Constructor > Driver > Points).
![alt text](image-97.png)
![alt text](image-98.png)
![alt text](image-99.png)

# Matrix Visualizations

![alt text](image-100.png)

![alt text](image-101.png)
drag 'driver_name' from columns to rows.
![alt text](image-102.png)

## Compare two Tables:

![alt text](image-103.png)

Tables display two-dimensional, flat data. Duplicate values are shown row by row rather than being grouped together.

A Matrix is more advanced than a standard Table. It makes it easier to display data meaningfully across multiple dimensions. The matrix automatically aggregates the data and enables you to drill down.

- **Hierarchy & Drill-down:** Matrices support a stepped layout. If you place Constructor Name and Driver Name in the Rows field, the matrix automatically groups the data. You can click the plus icon (+) next to a constructor (like Ferrari) to "drill down" and see the specific drivers (like Sebastian Vettel) and their individual points.

![alt text](image-104.png)
![alt text](image-105.png)

Meaning : Names of the drivers who've driven for that constructor. So you can see Sebastian Vettel has got the most points for Ferrari.

* Move driver_name from rows to columns. we now have a matrix.
![alt text](image-106.png)
![alt text](image-107.png)

We have drivers across the columns,  and construct_name down the rows, and the intersection would be the number of points for that constructor and that driver.

- **Cross-Tabulation:** If you move Driver Name to the Columns field and keep Constructor Name in Rows, the matrix creates a grid. The intersection of a row and column will show the points for that specific constructor-driver combination.

move driver_name back to rows again.
![alt text](image-108.png)

---

# Conditional Formatting

You can highlight specific values in your Tables or Matrices to make data easier to read at a glance.
(Format values to just select the column containing the values we'd like to format.)

-> **How to Apply:** Right-click the value field (e.g., Points) in the Build visual pane -> select Conditional Formatting.
![alt text](image-109.png)

- **Background Color:** Format cells based on rules or gradients.
![alt text](image-110.png)
![alt text](image-111.png)

-> Format cells based on their values.
select format style to 'Rules'.
![alt text](image-112.png)

**Example** If value is >= 500 and < 1000, set background to a specific color. 
![alt text](image-113.png)
![alt text](image-114.png)

- **Font Color:**
Format the Font Color : Higher the value -> Darker the color.
![alt text](image-115.png)
![alt text](image-116.png)
![alt text](image-117.png)

To remove conditional formatting:
Just right click on 'points' column -> remove conditional formatting.
![alt text](image-118.png)
![alt text](image-119.png)

- **Data Bars:** Similar to Excel, this adds an in-cell bar chart that visually represents how comparatively large the values are.

Right click on 'points' column -> Conditional formatting -> Data bars
![alt text](image-120.png)
![alt text](image-121.png)
![alt text](image-122.png)

- **Icons:** Adds symbols (like traffic lights or arrows) based on value thresholds.

- **To Remove:** Right-click the field -> Remove conditional formatting -> choose the specific format to clear.

---

# Sparklines

Sparklines are tiny charts shown directly within the cells of a Table or Matrix. That make it easy to see and compare trends quickly.

- **How to Add:** Right-click the value field (e.g., Points) -> Add sparkline.
  ![alt text](image-123.png)
  ![alt text](image-124.png)

Lets create a mini line show over time showing the points.

- **Setup:** You need a Y-axis (the value, like Points) and an X-axis (a date/time field, like Race Date).
![alt text](image-125.png)
This will show the value of the points arranged by date.

- **Result:** This creates a miniature line chart in the row. For example, you can visually compare when Alain Prost accumulated his points versus when Fernando Alonso started accumulating his, based on the shape of their individual sparklines.
![alt text](image-126.png)

here, Alain Prost raised a little bit earlier than Fernando Alonso because you can see that his points have been accumulated before Alonso start to accumulate points.
![alt text](image-127.png)

We got 'points by date' column created.
![alt text](image-128.png)
If you remove this, the sparkline will also disappear.

---

# Practice Assignment & Solution

## The Assignment:

![alt text](image-129.png)

- Create a Table showing Total Points (descending order) by Constructor Name and Driver Nationality.

- Create a Matrix visualization with the exact same columns.

- Add an appropriate title to both charts and rename the columns cleanly.

- Add conditional formatting (Icons) to the Points column in the Table based on these rules:

  - Red icon: Below 500 points.

  - Yellow icon: >= 500 and < 800 points.

  - Green icon: >= 800 points.


## Solution Steps:

- **Setup Visuals:** Swap out the Driver Name field for Driver Nationality in both the Table and the Matrix.

- **Observe the Matrix Advantage:** In the Matrix, you can sort Total Points in descending order while still keeping the data cleanly grouped by Constructor (e.g., seeing the point split of nationalities under Mercedes or Ferrari).

- **Rename Titles:** Go to Format visual -> General -> Title. Turn it on and rename to "Points by Constructor and Driver Nationality". Use the Format Painter to easily copy this title styling to the other visual.

- **Rename Columns:** In the Build visual pane, right-click the fields and rename them for presentation: "Constructor", "Driver Nationality", and "Total Points". (Note: Ensure the aggregation is set to Sum, not Average).

- **Apply Conditional Formatting:**
  - * Right-click Total Points on the Table -> Conditional Formatting -> Icons.

  - Crucial Step: Change the rule type from Percent to Number.

  - Enter the thresholds:

    - < 500 = Red

    - >= 500 and < 800 = Yellow

    - >= 800 (up to a very high maximum number) = Green.

---

# Bar and Column Charts

![alt text](image-130.png)

# Overview

Bar and Column charts are standard visualizations in Power BI used to compare specific numerical values across different categories.

- **Bar Chart:** Categories are arranged vertically (on the Y-axis), and the bars stretch horizontally.

- **Column Chart:** Categories are arranged horizontally (on the X-axis), and the columns stretch vertically.

(Note: Switching between a Bar chart and a Column chart simply rotates the visualization by 90 degrees).


# Types of Bar/Column Charts

When you look at the Visualizations pane, you will see several variations of these charts. Here is how they differ:

## 1. Stacked Bar Chart

In stacked bar chart, the rectangles are stacked on top of each other.

1) Add a stacked bar chart to the canvas.
![alt text](image-131.png)
![alt text](image-132.png)

2) X-axis: Constructor Name, Y-axis: Points
  ![alt text](image-133.png)
  ![alt text](image-134.png)
  ![alt text](image-135.png)

3) We can sort the chart in ascending or descending order of any specific fields.
![alt text](image-136.png)
![alt text](image-135.png)

4) add a data field to the legend
![alt text](image-137.png)

- **How it works:** This chart breaks down a single total value into sub-categories. The sub-categories are placed (stacked) on top of each other within a single bar or column.

- **Adding a Legend:** With a legend, we can add another categorical column, and the chart will update the visual to show the categories as per the legend.

To create the "stack," you must add a categorical field to the Legend section in the Build visual pane.

- **Example:**
  - * X-axis: Constructor Name
  - Y-axis: Points
  - Legend: Driver Nationality
  ![alt text](image-138.png)

- **Result:** You see one large bar representing Ferrari's total points (e.g., 9,292), but that single bar is divided into colored segments showing how many of those points were scored by Brazilian, British, or Italian drivers.

from (total ferrari)
![alt text](image-140.png)

to (shows the split by each different nationality, but the total should still be the same)
![alt text](image-139.png)
![alt text](image-141.png)

# Formatting the Legend

When you add a field to the Legend section to split your data:

- Go to the Format visual pane -> Legend.
  ![alt text](image-143.png)

- You can change the **position of the legend** (e.g., move it from the default position to the "Bottom Center" of the visual).
  ![alt text](image-144.png)
  ![alt text](image-142.png)

- You can adjust the text size, font, and color of the legend labels.

(Note: Concepts like Filtering, Tooltips, and Small Multiples apply to many different chart types, including Bar/Column charts, and are generally covered as standalone topics).

## 1.2 100% Stacked Bar Chart

Lets transform this chart to a 100% stacked bar chart.

![alt text](image-145.png)
![alt text](image-146.png)

all the rectangles for the constructors are the same size. This is because it shows the relative percentage of multiple data series in stacked Bar Charts with a total of each category equals 100%.

- **How it works:** Instead of showing the absolute raw numbers (like total points), this chart shows the relative percentage (or proportion) of the sub-categories.

- **Visual Difference:** Every single bar or column in the chart will be the exact same height/length, representing 100% of that category's total.

- **Example:** Using the Ferrari data above, the bar still shows the nationality split, but instead of showing raw points, it shows that "just under 13%" of Ferrari's points were scored by Brazilian drivers.
![alt text](image-147.png)

## 2. Stacked Column Chart

Change the chart to stacked column chart:
![alt text](image-148.png)
![alt text](image-149.png)

here, categories go along the bottom instead of the top. It just rotates at 90 degrees.

## 2.2. 100% Stacked Column Chart

![alt text](image-150.png)

## 3. Clustered Column Chart

clustered column chart just has the rectangles side by side as per the legend. Which is driver_nationality.

![alt text](image-151.png)
![alt text](image-153.png)
![alt text](image-152.png)

To make this clearer, we can apply sort.
![alt text](image-154.png)
![alt text](image-155.png)

click : apply filter
![alt text](image-156.png)
![alt text](image-157.png)

- **How it works:** Instead of stacking the sub-categories on top of each other, this chart places them side-by-side for each constructor.

- **Visual Difference:** If Ferrari had points scored by drivers of three different nationalities, you would see three separate, thinner columns grouped (clustered) together over the "Ferrari" label on the axis.

## 3.1 Clustered Bar Chart

![alt text](image-158.png)
![alt text](image-159.png)

---

# Practice Assignment & Solution

## The Assignment:

![alt text](image-160.png)

- Start with a blank canvas. Create either a Stacked Bar Chart or a Stacked Column Chart.

- Show the total count of races held in each country.

- Split the data by the specific name of the Grand Prix event.

- Arrange the visual in descending order of the total number of races.

- Ensure columns are renamed appropriately and the chart has a title.


## Solution Steps:

- **Select the Stacked Column Chart** from the Visualizations pane.

- **X-axis (Category):** Drag the Country field from the Circuits table into the X-axis. Rename the field in the visual pane to "Country" (capitalized).

- **Y-axis (Value):** We need to count the races. Drag the Grand Prix text field from the Races table into the Y-axis.

- **Key Concept:** Because Grand Prix is a text field, Power BI automatically defaults to a Count aggregation (you cannot Sum or Average text). Rename this field to "Total number of races".

- **Legend (Split):** To see the breakdown of the specific events within each country, drag the Grand Prix field into the Legend section as well. Rename it to "Grand Prix".

- **Sorting:** Click the three dots (...) -> Sort descending -> By Total number of races.

- **Result Analysis:** You should see that Italy has the most total events (~100). The Italy column will be stacked with segments showing the breakdown: roughly 71 occurrences of the Italian GP, 26 of the San Marino GP, and 1 of the Pescara GP.

- **Title:** Power BI usually auto-generates a title like "Total number of races by Country and Grand Prix," which is accurate, but you can customize it in the Format visual pane if desired.

---

# Legends

When using legends in your visualizations, you must be careful when selecting a column that contains a high number of unique (distinct) values. Legends have a strict, finite limit on the number of items they can display.

# How the Limitation Behaves

The legends can only show a certain number of values.

When a field is added to the Legend, Power BI loads the values sequentially. If the number of unique values in that column exceeds the visual's hidden limit:

- The legend will display items up to its maximum capacity.

- Once the limit is reached, it simply stops displaying any remaining values.

- The visual will silently drop the data associated with those omitted legend items, making your totals look incorrect or missing, even though the underlying data is fine.


# Real-World Example

![alt text](image-161.png)
![alt text](image-162.png)

Imagine a Column Chart showing the Total Number of Races (Y-axis) by Country (X-axis). The base totals are:

- Italy: 100 races

- Germany: 79 races

- UK: 75 races


## Scenario A: Safe Legend (Grand Prix)

If you add the Grand Prix column to the Legend, 
![alt text](image-163.png)
![alt text](image-164.png)

Convert visual converts to a Stacked Column Chart.
![alt text](image-165.png)
![alt text](image-166.png)

- The UK total remains a perfect 75 (split accurately into 1 occurrence of the 70th Anniversary GP, 71 of the British GP, and 3 European GPs).
![alt text](image-167.png)
![alt text](image-168.png)
![alt text](image-169.png)

- **Why it works:** The Grand Prix column only has 48 distinct values, which is well under the legend limit.


## Scenario B: Exceeding the Limit (Circuit Name)

If you swap out Grand Prix and put Circuit Name into the Legend instead, Italy and Germany might still look normal, but the UK's column will suddenly shrink, missing a massive amount of data.

![alt text](image-170.png)
![alt text](image-171.png)

- **Why it fails:** The Circuit_Name column has 79 distinct values. This exceeds the legend's maximum allowance. The specific circuits located in the UK happened to fall outside of that limit, so Power BI completely omitted them from the visual. Removing the legend immediately restores the UK's true total of 75. (Circuit's column has got more distinct values than the grand_prix column.)


# How to Verify Distinct Value Counts in Power Query

If you suspect a column is breaking your visual due to this legend limitation, you can check its exact number of unique values using the Transform Data menu.


## Steps to count distinct values:

- Open the Power Query Editor by clicking Transform Data.

- Navigate to the relevant table and select the column in question (e.g., Grand Prix in the Races table or Circuit Name in the Circuits table).

- Go to the Transform tab on the ribbon.

- Click Statistics, then select Count Distinct Values.
![alt text](image-172.png)
![alt text](image-173.png)

Power BI will return a single scalar number (e.g., showing 48 for Grand Prix and 79 for Circuits), confirming if you are dealing with a high-cardinality column.

(Note: Remember to delete this "Calculated Distinct Count" step from your Applied Steps pane to return to your normal data view!)

---

# Interactions (Cross-Highlighting & Cross-Filtering)

![alt text](image-174.png)

By default, all visualizations on a single Power BI report page are connected. When you click a data point (like a specific bar) on one visual, it automatically impacts all other visuals on that same page.

1) Add a Simple bar chart (stacked column chart)
![alt text](image-175.png)
![alt text](image-176.png)
![alt text](image-177.png)

2) Copy the visual & paste on right side
![alt text](image-178.png)

X : driver_name, Y : Points
![alt text](image-179.png)
![alt text](image-180.png)


- **Cross-Highlighting (Default behavior):** 

-- click on a constructor by selecting a bar in this visual
![alt text](image-181.png)

If you click "Mercedes" on a Bar Chart showing Points by Constructor, a second chart showing Points by Driver will dim all the unrelated data. The proportion of points that belong specifically to Mercedes drivers will remain in a solid, darker shade, allowing you to see their contribution relative to the whole.

- **Cross-Filtering:** Instead of just highlighting the proportion, the second visual completely filters out all unrelated data, resizing its axes to only show the drivers and points related to Mercedes.


# Editing Interactions

You can manually change how a specific visual reacts when you click on another visual.

## Steps to Edit:

- Select the "Source" visual (the one you will be clicking on) Ex. left side
![alt text](image-182.png)

- Go to the Format tab on the ribbon and click Edit interactions.
![alt text](image-183.png)

- Look at the top right corner of all the other visuals on the page. You will see three small icons appear:
![alt text](image-184.png)

  - **Filter (Funnel icon):** Changes the visual to filter out unrelated data completely.
  ![alt text](image-185.png)
  ![alt text](image-186.png)
  So now, instead of highlighting the points for drivers that are related to Mercedes, it actually filters for those specific drivers and for the points that they've scored for Mercedes.

  - Click Edit interactions again to hide the icons when you are finished setting your rules.

  -> You can check which filters are currently affecting a visual by hovering over or clicking the small filter icon that normally sits in the visual's header.

  - Click on the filter icon (right side) 
  ![alt text](image-187.png)
  here, see the filter that's affecting this visual - Mercedes (constructor name).

  - **Highlight (Pie chart icon):** Changes the visual to dim unrelated data and highlight the proportion (this is usually the default).

  - **None (Circle with a line icon):** Stops the interaction entirely. The visual will not change at all when you click the source visual.
  The (right side) visual is no longer affected by any selections made to this (left side) visual.
  ![alt text](image-188.png)

## Add filters into Report page

we can also use this interactivity to add filters into the report page.

> Lets filter the visualizations based on the construct_nationality.

1) Add a table visual
![alt text](image-189.png)
![alt text](image-190.png)
![alt text](image-191.png)
![alt text](image-192.png)

2) Click on British & see it highltghs all other visuals
![alt text](image-193.png)

See both visuals have the options for filter, highlight, none.
filter :
![alt text](image-194.png)

 
# Using Slicers

A Slicer is another way of filtering. A Slicer is a specific type of visual designed entirely for filtering a report page. It narrows the portion of the dataset shown in all other report visualizations.

![alt text](image-195.png)
![alt text](image-196.png)
![alt text](image-197.png)
![alt text](image-198.png)

> here, we can actually make multiple selections by 

1) Go to 'Format your visual'
![alt text](image-199.png)

2) Slicer settings -> Selection -> Multi select with Ctrl
![alt text](image-200.png)

![alt text](image-201.png)

With the slicer, you only have the option to filter, you can't highlight with the slicer.

- **Key Difference:** Unlike standard charts, Slicers only filter data; they cannot cross-highlight.

> We can change the layer.
list: ![alt text](image-202.png)
dropdown : ![alt text](image-203.png)

here, differnt field will have different options. we get different options depending on the type of the field.
> lets change conctructor_nationality with date :
![alt text](image-204.png)
list of unique dates : ![alt text](image-205.png)

or select dropdown
1) select between range (where we get a slider and then we can select some ranges)
![alt text](image-206.png)
![alt text](image-207.png)

2) select a relative date
![alt text](image-208.png)
![alt text](image-209.png)

3) from list
![alt text](image-210.png)
![alt text](image-211.png)

4) from dropdown
![alt text](image-212.png)
![alt text](image-213.png)

-> click on - clear selections

- make selection
![alt text](image-214.png)

- click clear selections icon
![alt text](image-215.png)
![alt text](image-216.png)

## Customize Slicers
![alt text](image-217.png)

lets add driver_name
![alt text](image-218.png)

list view: (its going all the way down)
![alt text](image-219.png)

Go to: Visualizations -> Format your visual -> Slicer settings -> set Orientation : Horizontal
![alt text](image-220.png)

Resize chart
![alt text](image-221.png)


# Slicer Formatting & Types

Depending on the type of data field you drop into the Slicer (Text, Date, Number), you get different layout options by clicking the down arrow on the Slicer header or looking in the Format visual -> Slicer settings pane.


## Text Fields (e.g., Nationality, Driver Name):

- **List:** A standard vertical checklist.

- **Dropdown:** A compact menu that drops down when clicked (great for saving space).

- **Horizontal (Tile) Layout:** Go to Format visual -> Slicer settings -> Orientation and change it from Vertical to Horizontal. This creates a grid of clickable buttons.

- **Multi-Select:** By default, clicking a new item replaces the previous selection. You must hold Ctrl while clicking to select multiple items.


## Date Fields (e.g., Race Date):

- **Between / Range:** Provides a slider bar with start and end dates.

- **Relative Date:** Allows selections like "Last 30 days" or "This Year."

- **List / Dropdown:** Standard specific date selection.


- **Clearing Selections:** Click the small eraser icon located on the top right of the slicer to clear all active filters.


# Syncing Slicers Across Pages

If you want the same filter to apply across multiple different pages in your report (e.g., filtering for "Alain Prost" on Page 1 automatically filters Page 2 as well), you need to sync the slicers.


## How to Sync:

- Copy an existing slicer (Ctrl + C or Right-click -> Copy visual).
![alt text](image-222.png)

- Go to a different page ('Bar and Column') and paste it (Ctrl + V).

- A prompt will immediately pop up asking if you want to "Sync visuals".
![alt text](image-223.png)

It asks if we want to sync (the copied slicer will sync with the original slicer)
So when I make a selection on any of the two slicers, they'll both automatically ensure that they have the same selection selected.

![alt text](image-224.png)


  - **Click Sync:** If you select "Alain Prost" on Page 1, the slicer on Page 2 will automatically update to "Alain Prost," and the data on Page 2 will be filtered accordingly.

  - **Click Don't Sync:** The slicer will be pasted as an independent visual. Selecting "Alain Prost" on Page 1 will have zero effect on Page 2.

(Note: You can manage all synced slicers in detail by going to the View tab and opening the Sync slicers pane).
# blinkit-grocery-sales-analytics
An interactive Power BI dashboard tracking FMCG retail sales, item distributions, and outlet performance metrics (₹1.20M in revenue) across 8,523 products.

Step 1: Create the Additional Core Business Measures
Right-click your 'BlinkIT Grocery Data' table, select New measure, and write these formula definitions:
1. Low Fat Sales (Segmented Revenue)
Calculates overall performance specifically for low-fat grocery items
.
Low Fat Sales = 
CALCULATE(
    [Total sales], 
    'BlinkIT Grocery Data'[Item Fat Content] = "Low Fat"
)
2. Regular Fat Sales (Segmented Revenue)
Tracks revenue contribution generated from regular-fat products
.
Regular Fat Sales = 
CALCULATE(
    [Total sales], 
    'BlinkIT Grocery Data'[Item Fat Content] = "Regular"
)
3. Item Sales % of Total (All-Filter Override)
Calculates the percentage contribution of any filtered item type (such as Fruits & Vegetables or Snack Foods) relative to all item sales
. It uses the ALL function to safely ignore local chart filters:
Item Sales % of Total = 
DIVIDE(
    [Total sales], 
    CALCULATE([Total sales], ALL('BlinkIT Grocery Data'[Item Type])), 
    0
)
Step 2: Expand Your Dynamic Field Parameter Table (metrics)
Using Power BI's modern field parameter table syntax, you can add these newly created calculations as selectable columns.
Replace your existing metrics table expression with this fully loaded version:
metrics = {
    ("Total sales", NAMEOF('BlinkIT Grocery Data'[Total sales]), 0),
    ("Avg Sales", NAMEOF('BlinkIT Grocery Data'[Avg Sales]), 1),
    ("NO of items", NAMEOF('BlinkIT Grocery Data'[NO of items]), 2),
    ("Avg Rating", NAMEOF('BlinkIT Grocery Data'[Avg Rating]), 3),
    ("Item Sales % of Total", NAMEOF('BlinkIT Grocery Data'[Item Sales % of Total]), 4),
    ("Low Fat Sales", NAMEOF('BlinkIT Grocery Data'[Low Fat Sales]), 5),
    ("Regular Fat Sales", NAMEOF('BlinkIT Grocery Data'[Regular Fat Sales]), 6)
}
Step 3: Format Your Measures for Professional UX
To make sure your numbers display beautifully across your dashboard card visuals and matrix tables
, highlight each measure in your Data pane and apply these standard formats using the Measure tools ribbon at the top of your screen:
Total sales, Low Fat Sales, and Regular Fat Sales: Format as Currency (₹) with 0 or 2 decimal places
.
Avg Sales: Format as a Decimal Number (or Whole Number)
.
NO of items: Format as a Whole Number
.
Avg Rating: Format as a Decimal Number with 1 decimal place
.
Item Sales % of Total: Format as a Percentage (%) with 2 decimal places.
How to use this dynamic metrics field on your report canvas 🎛️
Create a Slicer: Drag the metrics field you created in Step 2 directly onto an empty spot on your canvas and set the slicer style to Tile (or the new Button Slicer).
Configure Your Visuals: Take your Outlet Type matrix table
 and drag this metrics field into the Columns or Values bucket instead of hardcoding single metrics.
Watch the Magic Work: Now, when an executive clicks "Total sales" on your slicer, your table will dynamically change its columns to show sales
. If they click "Item Sales % of Total" or "Avg Rating", the entire table dynamically swaps to show those calculations instantly!

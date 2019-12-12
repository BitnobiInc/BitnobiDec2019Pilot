Bitnobi has a basic reporting package to let you visualize the data resulting from running a workflow. You can add one or more chart elements to the report, resize them and position them on the page. 
The menu icon in the lower right-hand corner allows you to:
* add a new chart, 
* save the current report contents, 
* get general info about the report, and 
* copy the URL for this report to the clipboard.

Each chart has a title bar displaying the chart title, a properties edit button and a trash icon. Hovering over the title will pop-up a tooltip containing the description entered for that chart. 

Here are the chart types currently available in the Reports canvas. Click on the Chart Type link to jump to an example:

| Chart Type | Comments |
|----|----|
| [Bar Chart](#bar-chart) | presents grouped data with rectangular bars with heights proportional to the values that they represent.|
| [Line Chart](#line-chart) | traditional x-y plot |
| [Map Chart](#map-chart) | uses Google Maps to display either heatmap or markers |
| [Multi-Bar Chart](#multi-bar-chart) | bar chart that groups bars based on a key field |
| [Multi-Series Line Chart](#multi-series-line-chart) | traditional x-y plot with multiple lines based on key field |
| [Pie Chart](#pie-chart) | displays % of total for each category |
| [Scatter Plot Chart](#tabular-chart) | displays data as bubbles on an x-y axis |
| [Tabular Chart](#tabular-chart) | displays data as a scrollable table |
| [Text Box](#text-box) | lets the user add text descriptions, HTML links and images to the report |

A note on data types supported by charts:
* the Y axis for all the bar and line charts must be numeric. 
* Bar and Multi-Bar will allow strings on the X axis.
* the X axis for the Multi-Series Line chart must be a date or numeric.
* for the Map Chart the lattitude and longitude columns must be numeric.

***

### Bar Chart

[[images/bar chart - movie box office receipts by genre.png]]

Here is a copy of the data used to create the above example: [[movies_data.csv]].  
Here are the parameters used to define the chart:

| Chart parameter| Parameter description | Data column or value used in example|
|----|----|----|
| Select X Field |data column to use on horizontal axis. | Genre |
| Enter X Axis Label | label for X axis | "Genre" |
| Select Y Field | data column to used for vertical axis. Must be numeric | Box Office (millions) |
| Enter Y Axis Label | label for Y axis | "Box Office (million $)" |
| Select Metric Field |how to aggregate Y data if there are multiple values for the same X value. can be sum, mean or count | sum |

***

### Line Chart

[[images/line chart - CD market share.png]]

The data used to create the above example started by importing [[music_industry_time_series.csv]] then 
doing a `Select where Media = CD`.
Here are the parameters used to define the chart:

| Chart parameter| Parameter description | Data column or value used in example|
|----|----|----|
|Select X Field | data column to use on horizontal axis.  | Year|
|Select X axis datatype field| data type for horizontal axis. Can be either Date or Number. |Date |
|Enter X Axis Label | label string for X axis | "Date" |
|Select Y Field | data column to used for vertical axis. Must be numeric |Market Share |
|Enter Y Axis Label | label string for X axis | "Market Share Percentage" |

***

### Map Chart

[[images/map chart - mlocation data.png]]

Here is a copy of the data used to create the above example: [[mlocation1.json]].  
Here are the parameters used to define the chart:

| Chart parameter| Parameter description | Data column used in example|
|----|----|----|
| Select Map Type: | how to mark the data points on the map. Can be markers or heatmap. | heatmap |
| Select Lattitude Field: | data column to use to provide lattitude. |lattitude |
| Select Longitude Field: | data column to use to provide longitude. |longitude |


***

### Multi-Bar Chart

[[images/multi-bar chart - financial sample 25.png]]

Here is a copy of the data used to create the above example: [[financial_sample_25.csv]].  
Here are the parameters used to define the chart:

| Chart parameter| Parameter description | Data column or value used in example|
|----|----|----|
| Select Key Field | data column to use to segregate data. | Country |
| Select X Field |data column to use on horizontal axis. | Product |
| Enter X Axis Label | label for X axis | "Product" |
| Select Y Field | data column to used for vertical axis. Must be numeric | Units Sold |
| Enter Y Axis Label | label for Y axis | "Units Sold" |
| Select Metric Field |how to aggregate Y data if there are multiple values for the same X value. can be sum, mean or count | sum |

***

### Multi-Series Line Chart

[[images/multi-series line chart - music sales by media type by date.png]]

Here is a copy of the data used to create the above example: [[music_industry_time_series.csv]].  
Here are the parameters used to define the chart:

| Chart parameter| Parameter description | Data column or value used in example|
|----|----|----|
|Select Key Field| data column to use for grouping data. A line with a different colour is generated for each group  | Media |
|Select X Field | data column to use on horizontal axis.  | Year|
|Select X axis datatype field| data type for horizontal axis. Can be either Date or Number. |Date |
|Enter X Axis Label | label string for X axis | "Date" |
|Select Y Field | data column to used for vertical axis. Must be numeric |Market Share |
|Enter Y Axis Label | label string for X axis | "Market Share Percentage" |
|Metric  | how to aggregate data if there are multiple values for the same X coordinate. can be Sum, Mean or Count | Mean|

***
### Pie Chart

[[images/pie chart - movies by genre.png]]

Here is a copy of the data used to create the above example: [[movies_data.csv]].  
Here are the parameters used to define the chart:

| Chart parameter| Parameter description | Data column or value used in example|
|----|----|----|
| | |
***

### Tabular Chart

[[images/tabular chart - movie box office receipts.png]]

Here is a copy of the data used to create the above example: [[movies_data.csv]].  
To minimize browser memory usage, this chart fetches a maximum of 1000 rows from the data source. 
It is possible to "page" through the entire data source using the arrow keys at the bottom of the chart.
***

### Text Box

[[images/text box - example.png]]

This lets the user add text descriptions, HTML links and images to the report.
The text can be rendered with a number of pre-defined types and formats.
Pressing `Preview` hides the editing bar.


# Create-Water-Main

# Point to Line Conversion Tool

This Python script connects points into a line feature. It takes the first entered row as the starting point and proceeds from there. You may need to adjust the order of rows to ensure the correct line is formed.

## Requirements

- Python 3.x
- ArcPy

## Usage

1. Set the workspace to the path of your geodatabase.
2. Modify the `feature_class` variable to match the name of your feature class.
3. Run the script.

## Code Overview

```python
# This line imports the arcpy module, which is the ArcPy library used for interacting with ArcGIS functionality in Python
import arcpy

# This line sets the workspace to the path of your geodatabase
arcpy.env.workspace = r"C:\Users\Nolan\Documents\ArcGIS\ArcGIS Pro Projects\DateDelete\DateCleanUpDelete\DateCleanUpDelete.gdb"

# This line assigns the name of the feature class you want to process to the feature class variable.
feature_class = "WaterMainPointConversion"

# Create a list to store the points
# This line creates an empty list called point_list to store the point objects extracted from the feature class
point_list = []

# Read the feature class and iterate through the points
with arcpy.da.SearchCursor(feature_class, ["SHAPE@XY"]) as cursor:
    for row in cursor:
        # Extract the x and y coordinates
        x, y = row[0]
        
        # Create a point object and append it to the list      
        point = arcpy.Point(x, y)
        point_list.append(point)

# Create a Polyline object from the points
polyline = arcpy.Polyline(arcpy.Array(point_list))

# This line sets the path for the output feature class where the line feature will be saved. 
output_fc = r"C:\Users\Nolan\Documents\ArcGIS\ArcGIS Pro Projects\DateDelete\DateCleanUpDelete\DateCleanUpDelete.gdb\WaterMainLineConversion"

# Create the feature class and insert the line feature
with arcpy.da.InsertCursor(output_fc, ["SHAPE@"]) as cursor:
    cursor.insertRow([polyline])

# Print the path to the output feature class
print("Line feature class created at:", output_fc)

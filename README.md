# Exploring 3D Data with ParaView
ParaView enables real time visualization and exploration of three dimensional data sets. The ParaView software is open-source and multi-platform and contains many tools for both qualitative and quantitative data analysis.  In this workshop we will explore an example data set and learn how to use a variety of ParaView's built-in tools.  Prior to the workshop, please install ParaView on your computer from their website: [https://www.paraview.org/](https://www.paraview.org/).  No prior knowledge or coding experience is required. 

The data here come from Pascal Paschos, formerly with NUIT RCS.

## Walkthrough

*Note*, you can use ParaView interactively, or through a built-in Python Shell: 
- View / Python Shell
- Tools / Start Trace (Select "Show Incremental Trace" to see in realtime)

We will use ParaView interactively:

1.	File / Open the Density.vtk and Temperature.vtk files.
2.	Turn the "eye" on to view Density.
3.	Change Representation to Volume, and check that the Coloring is set to Density (may already be set this way by default).
4.	Do similarly for Temperature.
5.	Change the Color Map for Density and/or Temperature.
    - Coloring / Edit / Choose Presets (folder with heart, on the right side of window)
    - I kept the blue-red for temperature but tweaked the curve a bit.
    - I changed the Density to green and inverted (white circle with black triangle).
    - Click Apply when you're happy with the color choices.
6.	Add a Slice to the Density (from the top bar).
    - click on Density.vtk and then click on slice icon above it.
    - Grabbing the plane (inside the red circle) moves the origin; grabbing the arrow tilts the plane (moving the normal).
    - When satisfied, click Apply.
7.	Hide the Density and Temperature, to view only the Slice.
8.	Add Contour to Slice (from the top bar).
    - In Value Range select "add a range of values" (icon that looks like a plot axis).
    - I chose from -2 to 2 with 20 linear steps.
    - (You may need/want to remove the initial value that was created by default, using the "-" button.)
    - Click Apply
9.	Add a Filters / Data Analysis / Calculator to Temperature
    - The original data in this file is log_10(Temperature).
    - I entered 10^Temperature, and saved as "Result Array Name" as "Linear".  (Click Apply.)
10.	Add Clip to Temperature - Calculator (from the top bar).
    - Copy the same positions from the Density Slice to the Temperature Clip.
    - You may want to move it down just a touch so that the contours a more visible.
    - Change Representation to Surface (if not already set by default).
    - Change Coloring to Linear (and back for comparison with the original logarithmic Temperature).
    - Click Apply.
11.	Add Filters / Data Analysis / Programmable Filter by selecting both Density and Temperature (Ctrl + click).
    - Add the following Script. (You may need to swap inputs order depending on how you loaded the data):
```
D = inputs[0].PointData['Density']
T = inputs[1].PointData['Temperature']
output.PointData.append(D*T, 'multiply')
output.PointData.append(D/T, 'divide')
output.PointData.append(D+T, 'add')
output.PointData.append(cos(T), 'cosT')
output.PointData.append(10**T, 'linearT')
output.PointData.append(10**D, 'linearD')
```
    - Change the "Representation" to "Volume" or "Surface"
    - Change the "Coloring" to one of these new arrays.
    - Apply a Threshold (or other) to this (from the top bar).
12.	File / Export Scene
    - I chose an eps format (this would need a lot of work to make it publication ready). 
    - You can also export to [x3dom](https://www.x3dom.org/), which is a format allowed by certain journals for interactive online figures .

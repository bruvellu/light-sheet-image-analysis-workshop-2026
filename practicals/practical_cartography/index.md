---
title: Tissue cartography using Blender
author: Bruno Vellutini
date: 05 January 2026
---

## Summary {#sec-summary}

Tissue cartography is the art of projecting a tridimensional surface, usually of a biological sample, into a 2D surface. This is useful to visualize and analyze complex 3D microscopy data.

The state-of-the-art approach to generate cartographic projections was the MATLAB-based ImSAnE toolbox [@Heemskerk2015-kv]. However, more recently, a Blender-based tool was released known as *Blender Tissue Cartography* [@Claussen2025-pv] ([code repository](https://github.com/nikolas-claussen/blender-tissue-cartography)). This tool has an [extensive documentation](https://nikolas-claussen.github.io/blender-tissue-cartography/) and several [in-depth tutorials](https://nikolas-claussen.github.io/blender-tissue-cartography/00_tissue_cartography_overview.html#tutorials) that I highly recommend anyone interested in tissue cartography to read.

The tutorial below is a simplified version of the Blender Tissue Cartography tutorials, focusing on one of the approaches. It shows how to generate cartographic projections from 3D lightsheet microscopy data using Fiji to inspect data, ilastik to segment the tissues, and Blender with the Blender Tissue Cartography add-on to create the projections.

If you want to generate cartographic projections using the MATLAB-based ImSAnE toolbox [@Heemskerk2015-kv], please check the [imsane-tutorial](https://github.com/bruvellu/imsane-tutorial) which explains how to set up and run the pipeline [@Vellutini2022-ya].

## Requirements {#sec-requirements}

- [Fiji](https://fiji.sc) [@Schindelin2012-di]
- [ilastik](https://www.ilastik.org) v1.4.1.post1 [@Berg2019-ab]
- [Blender](https://www.blender.org) v4.2.9 [@Blender-Foundation2002-jm]
- [Blender Tissue Cartography](https://github.com/nikolas-claussen/blender-tissue-cartography) (Blender add-on) [@Claussen2025-pv]
- `Drosophila_CAAX-mCherry.tif` dataset from Blender Tissue Cartography (available [here](https://github.com/nikolas-claussen/blender-tissue-cartography/blob/main/nbs/Tutorials/drosophila_example/)) [@Claussen2025-pv]

## Setup {#sec-setup}

### Download Blender Tissue Cartography

- Go to <https://github.com/nikolas-claussen/blender-tissue-cartography>.
- Press the green button named `Code` > `Download ZIP` to begin the download (or press [here](https://github.com/nikolas-claussen/blender-tissue-cartography/archive/refs/heads/main.zip)).
- Unzip the contents in your working directory.
- You should see a new directory named `blender-tissue-cartography`.
- Copy the file `Drosophila_CAAX-mCherry.tif` located at `blender-tissue-cartography/nbs/Tutorials/drosophila_example/` to your working directory.

### Download Blender

- Go to <https://download.blender.org/release/Blender4.2/>.
- Download Blender 4.2.9 (direct link for [Linux](https://download.blender.org/release/Blender4.2/blender-4.2.9-linux-x64.tar.xz)/[MacOS](https://download.blender.org/release/Blender4.2/blender-4.2.9-macos-x64.dmg)/[Windows](https://download.blender.org/release/Blender4.2/blender-4.2.9-windows-x64.msi)).
- Unzip the file into your working directory.
- You should see a new directory named `blender-4.2.9-linux-x64` (or similar for other systems).

Note that older or newer versions of Blender might not work. Use v4.2.9 as it is known to work with this tutorial.

### Install Blender Tissue Cartography

- Open the directory `blender-4.2.9-linux-x64`.
- Double-click the file `blender` to open the program.
- Go to `Edit` > `Preferences` > `Add-ons` > `Add-ons Settings` (down arrow) > `Install from Disk...`.
- Select the file `blender_tissue_cartography-1.0.0-linux_x64.zip` located in the directory `blender-tissue-cartography/blender_addon/`.
- Close Blender.

### Download ilastik

- Go to <https://www.ilastik.org/download>.
- Download ilastik 1.4.1.post1 (direct link for [Linux](https://files.ilastik.org/ilastik-1.4.1.post1-Linux.tar.bz2)/[MacOS](https://files.ilastik.org/ilastik-1.4.1.post1-OSX.zip)/[Windows](https://files.ilastik.org/ilastik-1.4.1.post1-win64.exe)).
- Unzip the file into your working directory.
- You should see a new directory named `ilastik-1.4.1.post1-Linux`.

### Download Fiji

- Go to <https://fiji.sc>.
- Choose `Distribution: Stable` then click the big download button.
- Unzip the file into your working directory.
- You should see a new directory named `fiji-stable-linux64-jdk`.

## Inspect dataset in Fiji {#sec-inspect-dataset}

Before starting, let's inspect the `Drosophila_CAAX-mCherry.tif` dataset in Fiji.

- Open the directory `fiji-stable-linux64-jdk/Fiji.app/` and double-click the `fiji-linux-x64` launcher.

![](media/01-fiji-open.png)

- Drag and drop `Drosophila_CAAX-mCherry.tif` in the Fiji window to open it.
- Scroll through the Z slices of the stack.

![](media/02-data-open.png)

- To get more information, activate the orthogonal views with `Image` > `Stacks` > `Orthogonal Views` (or `Ctrl+Shift+H`).

::: {layout-ncol=2}

![](media/03-orthogonal-open.png)

![](media/04-orthogonal-views.png)

:::

Explore the sample to understand well its shape. Try to figure out which side of the embryo is dorsal, which is ventral, and what is left/right. Also notice what are the characteristics of the tissues and of the background regions and think about the potential issues we might encounter with this dataset.

- Once done, close the orthogonal views and stack (leave Fiji open).

## Segment tissues in ilastik {#sec-segment-tissues}

The first step we need is to segment the stack in 3D to distinguish what is tissue and what is background. This is required to create a 3D mesh in Blender that has the shape of the sample. To accomplish that, we will use ilastik.

- Open the directory `ilastik-1.4.1.post1-Linux`, right-click the file `run_ilastik.sh`, and select `Run as a Program` to open ilastik.

![](media/05-ilastik-open.png)

### Create project

- Maximize the interface (we will need it).
- Under `Create New Project`, click on `Pixel Classification`.

![](media/06-ilastik-pixel.png)

The window `Create Ilastik Project` will open.

- Navigate to your working directory and click `Save`.

![](media/07-ilastik-project.png)

A file named `MyProject.ilp` will be created.

### Input Data

The ilastik interface is ready to define our input data.

- Under the `Raw Data` tab, click on `Add New...` > `Add separate Image(s)...`.

![](media/08-ilastik-add.png)

- Then, select the file `Drosophila_CAAX-mCherry.tif`.

![](media/09-ilastik-select.png)

ilastik will open the dataset in three orthogonal views: XY (blue), XZ (green), YZ (red).

![](media/10-ilastik-dataset.png)

Note, however, that the images are too dark; let's fix this.

- Right-click the dataset row and select `Edit properties...`.
- Change the value of `Normalize Display` to `True` and set the `Range` maximum value to `10000`.
- Press `OK`.

::: {layout-ncol=2}

![](media/11-ilastik-adjust.png)

![](media/12-ilastik-properties.png)

:::

The contrast will be much better now.

![](media/13-ilastik-adjusted.png)

### Feature Selection

Next we need to select the image features to take into account for the segmentation.

- On the left column press `2. Feature Selection`; the interface will update.
- Then press `Select Features...` to open the `Features` window.
- Select all the features and press `OK`.

::: {layout-ncol=2}

![](media/14-features-window.png)

![](media/15-features-selected.png)

:::

### Training

- On the left column, click `3. Training`.

A new toolbox will open underneath showing two labels, Label 1 (yellow) and Label 2 (blue), buttons for paint or eraser modes, a size drop-down menu, and a `Live Update` button.

![](media/16-training-open.png)

Now is a good time to get familiar with the basic ilastik commands.

- `Scroll forward`: go down through the slices of the orthogonal dimension.
- `Scroll backward`: go up through the slices of the orthogonal dimension.
- `Ctrl+Scroll forward`: zoom in.
- `Ctrl+Scroll backward`: zoom out.
- `Middle-click and hold`: drag view around.

Learn how to zoom in/out, go through the slices, and drag the view. If we are able to zoom in significantly, and reach the top or bottom of a view by dragging, we are ready for painting.

![](media/17-training-drag.png)

Our goal is to paint tissues in yellow and background in blue. But to accomplish that, we only need a few strokes at the right regions of the image.

- Begin by zooming in at the top region of the XZ (green) view.
- Select Label 2 (blue), change the size to `7`, and paint a line right above the tissue.
- Now select Label 1 (yellow) and paint the tissue immediately below the blue line.

![](media/18-training-paint.png)

These two simple lines are indicating to ilastik that all the image features in this region very close to the tissue correspond to "background", and that the image features of the tissue below correspond to "foreground". Putting the two lines adjacent to each other also helps ilastik to understand where the boundaries are.

- Since we want all of the tissue to be segmented (and not just the surface), also paint a line until the center of the embryo.

This is enough to get started. Based on these simple strokes, ilastik will learn and apply this to the entire dataset.

- To start the training, press the button `Live Update`.

![](media/19-training-live.png)

ilastik will overlay the current segmentation model over the image. We should see that most of the tissue regions are yellow and that the region around the embryo is more blue. The more vivid the color, the more confident the model is about that specific region.

Now start painting with simple strokes areas which are wrong or pale. For example the corners of the images are background and should be blue; any area inside the sample should be yellow; use different brush sizes if needed; or the eraser. This sample also has giant, super bright beads; they are not tissue, we want them blue.

Note that the segmentation model and overlay colors update upon each stroke, so we can see if what we did improved or worsened the segmentation. If it got worse, we can always erase the annotation.

![](media/20-training-better.png)

Be meticulous and pay special attention to the edges of the image. We do not want tissue (yellow) to be touching the border, because this will create a hole in the segmentation. The better the segmentation is, the better will be our visualization and cartographic projection.

There are ways that we can fix segmentation issues after converting it to a mesh, but they will not be covered in this tutorial. So, for this image, it is important to take care of the tip of the very top part of the sample because it is touching the edge.

- Use a size `1` brush to place a couple of blue lines at the very top.

![](media/21-training-tip.png)

- Go through the slices in each of the orthogonal views to fix any leftover segmentation uncertainties.

![](media/22-training-done.png)

The segmentation overlay should be showing clearly separated yellow and blue regions that match the embryo and background.

### Prediction Export

We can now export the segmentation prediction.

- On the left column, click on `4. Prediction Export`.
- Under `Export Settings` keep the `Source` value as `Probabilities`.

![](media/23-prediction-export.png)

- Then, press `Choose Export Image Settings...` to open the `Image Export Options` window.

![](media/24-prediction-options.png)

There are two options that we need to change.

- Under `Cutout Subregion` uncheck the row `c` (for channels) and change the stop value to `1`.

Since this image has only one channel, changing this option avoids loading a duplicated channel into Blender.

- Under `Output File Info` change the value of `Format` to `multipage tiff`.

This is required to be able to load the segmentation into Blender.

- Press `OK` to close the window.
- Then press the `Export All` button and wait...

When the prediction is done, a new file will appear in the working directory named `Drosophila_CAAX-mCherry_Probabilities.tiff`.

![](media/25-prediction-done.png)

- Open this file in Fiji to see how it looks before our next step in the tutorial.

![](media/26-fiji-probabilities.png)

- Now close the file and let's start with generating the actual projection.

## Import data to Blender {#sec-import-blender}

We can now import the image stack and segmentation probabilities into Blender.

### Open Blender

- Enter the directory `blender-4.2.9-linux-x64` and double-click the file `blender`.

![](media/27-blender-open.png)

Blender will open with a nice splash screen at the center.

- Click anywhere to close it.

![](media/28-blender-clean.png)

Notice at the top left region that we are in the `Layout` tab (important for later). There's a gray cube at the center, we want to get rid of it.

- In the top right panel under `Scene Collection` > `Collection`, right-click the `Cube` line and select `Delete`.

![](media/29-blender-cube.png)

Great. Let's focus now at the bottom right corner, it is busy, full of icons and menus. Don't get overwhelmed, we only need to select and use one of the modes.

- If not yet selected, click on the `Scene` icon (it is the white triangle with two circles) to activate this panel.
- Then locate the tab named `Tissue Cartography` at the bottom.

![](media/30-blender-btc.png)

- Scroll down and make the side panels wider to be able to read the options of `Tissue Cartography`.

![](media/31-blender-wider.png)

This is the main interface of the `Blender Tissue Cartography` add-on. It is through here that we will control most of the steps of this pipeline.

### Load sample

The first thing we need to do is to load the sample.

- Click on the folder icon in the `Tissue Cartography` > `File Path`, navigate to the working directory, and select the file `Drosophila_CAAX-mCherry.tiff`.

Tip: bookmark the directory for easy access in the future.

- Click `Accept`.

::: {layout-ncol=2}

![](media/32-blender-openpath.png)

![](media/33-blender-filepath.png)

:::

- Then press `Load .tiff file` to load it into Blender.

![](media/34-blender-loaded.png)

A new row will appear at the top right panel under `Scene Collection` > `Collection` named `Drosophila_CAAX-mCherry_BoundingBox` and the bounding box of the image stack will be visible in the main window as orange lines.

Only a portion of the bounding box is visible, but we want to see the whole thing. The controls to navigate the 3D space are at the top right corner of the main window. We have XYZ handles (red, green, blue), a zoom tool (magnifier lens), and a move tool (open hand).

- Click, hold, and drag any of these to move around.
- Clicking on the X, Y, or Z will reorient the sample along these axes (very useful).

![](media/35-blender-box.png)

Take some time to practice and finish by placing the bounding box at the center of the main window as in the image below.

### Load probabilities

Now let's load the probabilities files.

- Click on the folder icon of the `Tissue Cartography` > `Segmentation File Path`, navigate to the working directory, and select the file `Drosophila_CAAX-mCherry_Probabilities.tiff`.
- Click `Accept`.

![](media/36-blender-probabilities.png)

The file name will appear in the field, but before loading we want to adjust one parameter. Blender will take the segmentation probabilities and convert it into a tridimensional mesh. Generally, the raw segmentation is full of sharp angles, which will not look very nice when we map the image information onto the mesh for visualization. Therefore, it is generally a good idea to apply a degree of smoothing upon importing the segmentation.

We can control the smoothing in the small field below and to the right of `Segmentation File Path` named `S... 0.00`. It should read `Smoothing (µm)` but the panel is too narrow to show the full name.

- Click on it and set it to `1.0`.

::: {layout-ncol=2}

![](media/37-smoothing-zero.png)

![](media/38-smoothing-one.png)

:::

- Now click on `Get mesh(es) from binary segmentation .tiff file(s)` to generate the mesh (it takes a second).

![](media/39-blender-mesh.png)

A gray mesh shaped like our sample will appear inside the bounding box in the main window. Also notice that a new row appeared at the top right panel under `Scene Collection` > `Collection` named `Drosophila_CAAX-mCherry_Probabilities_c0`.

Congratulations! We have successfully generated a 3D mesh of the sample. That's already a powerful visualization method. Celebrate by exploring the sample. Rotate all around, zoom in to see details, and check how good the mesh is. Are there any holes or other artifacts?

### Apply shading

The mesh is nice, but it would be even better to see the actual image data overlaid on the mesh. We can accomplish that using the shading view and function of Blender.

- First, we need to activate the Shading workspace at the top right corner of the main window (an icon that looks like a pie chart).

![](media/40-shading-viewport.png)

Once clicked, the mesh will become almost white.

![](media/41-shading-blank.png)

Before applying the shading, there's one important parameter to set: `Vertex ... 0.00` or, in full, `Vertex Shader Normal Offset (µm)`.

![](media/42-offset-zero.png)

When this parameter is `0`, the image data that corresponds to the limits of the segmentation is applied onto the mesh. In this case, this is the surface of the sample which, in this case, does not have much information. The fluorescent signal of the tissue, in this case, is a few microns deeper. Therefore, we can use the offset parameter to adjust the exact layer to be applied to the mesh as shading.

- In this case, a value of `5` works well.

![](media/43-offset-five.png)

Every time we want to apply or refresh the shading, we need to select the bounding box and probabilities entries in the `Scene Collection` > `Collection` top right panel.

- We can do so by clicking on one and `Ctrl+Click` on the other to select both.

![](media/44-shading-boxprob.png)

- Finally, press `Initialize/refresh vertex shading` to apply the shading and wait...

![](media/45-shading-applied.png)

After a few seconds, we should see cell membranes overlaid onto the mesh.

- Take the chance to explore the sample again, now with some biological information projected into 3D!

## Generate projection {#sec-generate-projection}

We are ready to generate our first cartographic projection. This is done in the `UV Editing` workspace.

- Click on the `UV Editing` tab to activate it.

![](media/46-uv-workspace.png)

The workspace will be divided in two. On the left we will see the projected mesh and on the right it is the mesh of the original sample in 3D.

### Set up UV Editing {#sec-setup-uv}

Before starting, we need to make sure that the correct options are enabled.

- First, zoom out the right side window for the entire sample to be visible.

![](media/47-uv-sample.png)

- Then, disable the bounding box entry in the top right panel under `Scene Collection` > `Collection` by `Ctrl+Click` on the square nodes symbol on the left of the bounding box entry as shown below.

::: {layout-ncol=2}

![](media/48-uv-collection.png)

![](media/49-uv-disable.png)

:::

- Next, change the `Select Mode` from `Vertex` to `Face`.

::: {layout-ncol=2}

![](media/50-uv-vertex.png)

![](media/51-uv-face.png)

:::

In the right hand workspace, we should see the sample (not the bounding box) highlighted in light orange.

![](media/52-uv-ready.png)

We might accidentally click somewhere and the mesh will become black.

![](media/53-uv-all.png)

If this happens, worry not!

- Simply press `A` or click on `Select` > `All` to select all the mesh.

To finish the setup, we need to orient the sample properly for the projection, it matters!

- Click on the Y axis handle until the sample is oriented vertically with the narrower tip pointing upward.

![](media/54-uv-orient.png)

We are ready to project the mesh.

### Project mesh {#sec-project-mesh}

While the right side shows the sample, the left side shows how the projected mesh looks like; it is initially empty.

- To make the first projection, go to `UV` > `Cylinder Projection` to project the mesh over the curved wall of a cylinder.

![](media/55-uv-cylinder.png)

A crazy, palisade-like wall will appear.

![](media/56-uv-palisade.png)

But don't despair.

- Click on the tiny menu named `Cylinder Projection` that appeared at the bottom of the workspace.

The options for the cylinder projection will appear. There we can define the orientation of the axes and other options to change how the 3D mesh is transformed into a 2D surface. What we need for now is to contain the projection into the squared bounding area.

- Activate the checkbox `Scale to Bounds`.

![](media/57-uv-scale.png)

This will nicely limit the mesh to the projection area.

That's it. We have our first projected mesh. How good is it? Ideally, the mesh should occupy the entire projection area. Our projected mesh has a couple of portions slightly bulging outside the area, and we have an empty vertical portion on the right side. This could be fixed with some editing. However, for now, it looks good enough for a first try.

### Project data {#sec-project-data}

The next step is to project the actual image data onto this projected mesh surface.

- Change back to the `Layout` workspace, select both `Drosophila_CAAX-mCherry_BoundingBox` and `Drosophila_CAAX-mCherry_Probabilities_c0` under `Scene Collection` > `Collection`, and change the option `Normal Offs...` (`Normal Offsets (µm)`) to `5` to match the `Vertex Shader Normal Offset (µm)` option.

Note: `Normal Offsets (µm)` accepts a comma-separated list of values. We can put `0,1,2,3,4,5` to generate a projection with 6 slices representing onion layers deeper into the tissues.

- Then, click on `Create Projection`.

![](media/58-create-projection.png)

Wait... the interface might become unresponsive. If a dialog appears, click on `Wait` and wait. When done, the shading over the sample will blink. But, it should look very similar to how it was before (and if it doesn't, it is a sign that something went wrong).

### Check projection {#sec-check-projection}

- To visually inspect the projected data, change back to the `UV Editing` workspace.

![](media/59-uv-after.png)

- Zoom out on the left side window to see the entire projection.
- Then click anywhere outside the sample or projection to unselect the mesh.

![](media/60-uv-projected.png)

The sample mesh will become visible in black, and the cartographic projection should appear on the left side window. The orientation of the sample will match that of the projection (if the sample is upside down when projecting the mesh, the projected mesh will also be upside down).

So, what happened here? We projected the sample mesh to 2D using the cylinder approach. Then the add-on Blender Tissue Cartography used this projected mesh to create a projection of the image data from the original stack. Quite nice!

Every time we create a new projection, the projected data is stored as an image which is available for Blender to display as an image "data-block". We can see them by clicking on the picture icon in the top menu.

![](media/61-uv-datablock.png)

This first projection is named `Channel_0_Layer_0`. The next will be named `Channel_0_Layer_0.001`, `Channel_0_Layer_0.002`, and so on.

### Save projection {#sec-save-projection}

The projection now exists in Blender, but we need to export it to file.

- For that, go back to the `Layout` workspace, select both `Drosophila_CAAX-mCherry_BoundingBox` and `Drosophila_CAAX-mCherry_Probabilities_c0` under `Scene Collection` > `Collection` again, then click on `Save Projection`.

![](media/62-save-projection.png)

A `Blender File View` window will open.

- Navigate to the working directory.
- In the file name field put the name of the file with `_cylinder` appended to it, to read `Drosophila_CAAX-mCherry_cylinder.tif`.
- Then press `Save Projection`.

Three new files will appear in the working directory with `BakedData`, `BakedNormals`, and `BakedPositions` suffixes appended to the dataset filename.

![](media/63-projection-saved.png)

- `BakedData` shows the original image data projected on the surface.
- `BakedPositions` shows the original XYZ positions projected on the surface in RGB.
- `BakedNormals` shows the XYZ surface directions perpendicular to each point in RGB.

### Open projection {#sec-open-projection}

Let's open the files in Fiji for inspection.

- Drag and drop the files into Fiji.

::: {layout-ncol=3}

![](media/64-baked-data.png)

![](media/65-baked-normals.png)

![](media/66-baked-positions.png)

:::

Explore them in more detail, check with `Image` > `Color` > `Channels Tool...` (`Ctrl+Shift+Z`) how the individual channels look like. These files provide important information to reconstruct back the 3D information from the projected surface in downstream analyses.

## Optimize projection {#sec-optimize-projection}

Our initial projection is satisfactory, but there are many ways to optimize it to our specific needs. One immediate thing is to try a `Sphere Projection` instead of `Cylinder Projection`. They are quite similar, but I found the sphere projection to be more consistent and predictable and might work better for more spherical samples.

Another common use case is to be able to determine where the mesh will unwrap. This might be important for downstream analyses. In our sample, for example, the projection put the dorsal side on the left side (where a clump of germ cells are visible at the bottom) and the ventral side on the right side. However, let's say that I need for my analyses the dorsal side at the center of the projection.

To accomplish that we can manually mark a seam on our mesh to define the unwrapping position.

### Mark seam on mesh {#sec-mark-seam}

- Go to the `UV Editing` workspace and change the `Select Mode` to `Edge select`.

![](media/67-seam-edge.png)

In this mode, when we click on the mesh an edge is selected and when we subsequently `Ctrl+Click` on another edge, the shortest path between the two edges will be selected. Like this we can quickly select a line along the sample to mark the unwrapping position. What we want is to trace a line through the ventral side of the sample. This is the region opposite to the germ cell clump.

- Using the handle buttons, reorient the sample sideways with the ventral side facing us.

![](media/68-seam-zoom.png)

We will start by selecting an edge on one of the poles.

- Turn the sample to show one of the poles and zoom in to see the edges clearly.
- Then click on one edge at the center of the pole to select it.

![](media/69-seam-first.png)

- Zoom out slightly, turn the sample, and `Ctrl+Click` on another edge further away from the pole.
- A yellow line will appear connecting the pole with the current edge.

![](media/70-seam-second.png)

- Continue `Ctrl+Click` on edges along the sample until the opposite pole (don't worry if the line isn't perfectly straight).

::: {layout-ncol=3 layout-nrow=2}

![](media/71-seam-third.png)

![](media/72-seam-fourth.png)

![](media/73-seam-fifth.png)

![](media/74-seam-sixth.png)

![](media/75-seam-seventh.png)

![](media/76-seam-eighth.png)

:::

- Now press `Ctrl+E` to open a menu with edge options and select `Mark Seam`.

![](media/77-seam-mark.png)

The seam will be marked in red (below it's mixed with yellow line from the edge selection).

![](media/78-seam-done.png)

### Project mesh with seam {#sec-project-mesh-seam}

- Now reorient the sample vertically again with the narrow tip up and change the `Select Mode` back to `Face select`.

![](media/79-seam-orient.png)

- Press `A` to select all the mesh (they will turn orange).
- Go to `UV` > `Cylinder Projection`.
- Then check `Preserve Seams` in the option box.

![](media/80-seam-preserve.png)

As we can see, the projection changed. It is squeezed at the center of the projection area due to the very long protruding mesh at the top left and bottom right regions. Let's evaluate how good it is by projecting the data.

### Project data with seam {#sec-project-data-seam}

- Go to the `Layout` workspace, select both the bounding box and probabilities under `Scene Collection`, and press `Create Projection`; then, wait...

![](media/81-projection-seam.png)

- When done, go back to the `UV Editing` workspace and click anywhere outside the sample to unselect the mesh.

The new data projection should be visible on the left side.

- If not, click on the image data-block picture icon and select `Channel_0_Layer_0.001`.

![](media/82-projection-squeezed.png)

We have successfully changed the position of the unwrapping using the seam. The clump of germ cells is now at the center of the projection.

This projection is OK, but the contents are squeezed and it is not occupying the full bounding area. We can improve this by editing the projection mesh.

### Edit projected mesh {#sec-edit-projected-mesh}

- Press `A` to select the entire mesh.
- Select the tool `Transform` on the left side menu.

![](media/83-projection-transform.png)

- Hover the mouse on the right side edge to reveal the scale handle.

![](media/84-projection-scale.png)

- Drag the right side to the right to extend the orange mesh until the edge of the projection area.
- Then, drag the other side to extend the mesh to the left side.
- Finally, make adjustments so that the projected mesh is covering most of the projection area as shown below.

::: {layout-ncol=2}

![](media/85-projection-right.png)

![](media/86-projection-scaled.png)

:::

Now we can use the `Pinch` or `Grab` tools to edit the mesh at the corners, so that they are not clipped out. Or, we can leave them as is (they will be clipped out of the projection).

- Use `Pinch` to drag finer portions of the mesh to the corners.
- Use `Grab` to drag larger portions of the mesh to the corners.

::: {layout-ncol=3}

![](media/87-projection-pinch.png)

![](media/88-projection-grab.png)

![](media/89-projection-edited.png)

:::

At the end, the mesh should be filling more or less the projection area.

### Check edited projection {#sec-check-edited-mesh}

To check the new projection with the edited mesh, we need to re-create the image data projection onto the edited mesh.

- Go to the `Layout` workspace, select both the bounding box and probabilities under `Scene Collection`, and press `Create Projection`; then, wait...
- When done, switch to the `UV Editing` workspace and deselect the mesh and check the new projection (select the latest image data-block, likely `Channel_0_Layer_0.002`).

![](media/90-check-edited.png)

Despite the unevenness of the corners (next time we can improve our pinching and grabbing skills), the edited projection is better than the first one and the tissue is oriented the way we needed. Let's save the projection to disk.

- Go to the `Layout` workspace, select both the bounding box and probabilities entries under `Scene Collection` > `Collection`, and click on `Save Projection`.
- Then navigate to the working directory and give the file a different suffix.

![](media/91-save-edited.png)

- Finally, open the newly generated files in Fiji to investigate.

::: {layout-ncol=3}

![](media/92-baked-data.png)

![](media/93-baked-normals.png)

![](media/94-baked-positions.png)

:::

The projection is ready for image analyses.

There are several other use cases that are not covered in the current version of this tutorial. For example, how can we generate a projection with more layers, or create and register projections for different timepoints. But, if you are interested, these use cases are described in the [Blender Tissue Cartography](https://nikolas-claussen.github.io/blender-tissue-cartography/) paper and documentation [@Claussen2025-pv].

## References {#sec-references}

::: {#refs}
:::

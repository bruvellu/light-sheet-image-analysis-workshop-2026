---
title: Post-processing, segmentation and labelling
author: Marina Cuenca
date: 05 January 2026
---

## 2D segmentation in Fiji

In this exercise we will:

- Compare global and local thresholding methods and understand when each is useful.  
- Improve segmentation using background subtraction and Gaussian smoothing.  
- Refine masks using binary operations, including **Fill Holes** and **Kill Borders**.  
- Generate labeled objects using MorphoLibJ's Connected Components Labeling.  
- Extract object-level measurements (area, perimeter, circularity, etc.).  
- Visualize measurement values on labeled images.  
- Filter segmented objects based on size or other properties.  

These steps form a complete workflow:
**raw image → pre-processing → segmentation → cleanup → labeling → measurement → filtering**

We’ll use `MAX_Lund.tif` as the example image.

### 1. Open the image and inspect the histogram

1. Start **Fiji**.  
2. Open the image:  
   `File → Open… → MAX_Lund.tif`  
![Brightness/Contrast histogram](images/MAX_Lund.png)
3. Open the histogram and visualization tools:  
   `Image → Adjust → Brightness/Contrast…`

You should see a histogram like this:

![Brightness/Contrast histogram](images/Screenshot1.png)

**Idea:** Thresholding chooses an intensity value that separates “foreground” from “background”. The histogram shows how many pixels exist at each intensity.

### 2. Try all global Auto Threshold methods

Global thresholding applies **one single threshold** to the whole image.

1. With `MAX_Lund.tif` active, run:  
   `Image → Adjust → Auto Threshold…`
2. Choose:  
   - **Method:** *Try all*  
   - **White objects**  
3. Confirm.

Fiji generates a montage of all global methods:

![Global auto threshold montage](images/AutoThresh.png)

### 3. Prepare the image for Auto Local Threshold

Local thresholding requires **8-bit** input.

1. Select the original image:  
   `Window → MAX_Lund.tif`
2. Convert to 8-bit:  
   `Image → Type → 8-bit`

Now the image is ready for local methods.

### 4. Try all Auto Local Threshold methods

Local thresholding computes a threshold for each **local neighborhood**.

1. Run:  
   `Image → Adjust → Auto Local Threshold…`
2. Choose:  
   - **Method:** *Try all*  
   - **Radius:** 15  
   - **Parameter 1:** 0  
   - **Parameter 2:** 0  
   - **White objects**
3. Confirm.

![Local auto threshold montage](images/AutoLocalThresh1.png)

### 5. Improving segmentation with background subtraction and smoothing

Good segmentation often benefits from reducing background and noise first.

#### 5.1 Remove background and apply Gaussian blur

1. Open `MAX_Lund.png`.  
2. Subtract background:  
   `Process → Subtract Background…`
   - Rolling ball radius: **50**  
3. Smooth noise:  
   `Process → Filters → Gaussian Blur…`
   - Sigma: **1**

After preprocessing:

![Auto Local Threshold after preprocessing](images/AutoLocalThresh2.png)

### 6. Selecting a method: Local Otsu and binary cleanup

#### 6.1 Apply Local Otsu thresholding

1. Run:  
   `Image → Adjust → Auto Local Threshold…`
   - **Method:** Otsu  
   - **Radius:** 15  

**Local Otsu mask:**

![Local Otsu mask](images/MAX_Lund_Mask.png)

#### 6.2 Refine the mask with binary operations

2. Fill holes inside objects:  
   `Process → Binary → Fill Holes`
3. Slightly shrink objects:  
   `Process → Binary → Erode`
4. Optionally restore outlines:  
   `Edit → Draw`
5. Expand objects after erosion:  
   `Process → Binary → Dilate`

**Refined mask:**

![Refined mask](images/MAX_Lund_MaskErode.png)

#### 6.3 Remove border-touching objects

6. Remove partial objects touching image borders:  
   `Plugins → MorphoLibJ → Filtering → Kill Borders`  

![Filled + border-removed mask](images/MAX_Lund_Mask-killBorders.png)

### 7. Object labeling with MorphoLibJ

Convert each connected region into a uniquely labeled object:

`Plugins → MorphoLibJ → Label → Connected Components Labeling`

- Connectivity: **4**  
- Output type: **16-bit**

**Labeled objects:**

![Connected components labeling](images/MAX_Lund_Mask-killBorders-lbl.png)

### 8. Measuring object properties

Quantifying each object is often the main goal after segmentation.

Run measurements:

`Plugins → MorphoLibJ → Analyze → Analyze Regions`

This extracts a number of morphological associated features. We will select:

- Area  
- Pixel count  
- Perimeter  
- Circularity  
- Ellipse geometry  
- Bounding box  

![Morphometry table](images/Screenshot2.png)

### 9. Visualizing label properties

Visualize one measurement (e.g. area) mapped onto the label image:

`Plugins → MorphoLibJ → Label Images → Assign Measure to Label`

Choose **Area** and apply a colormap.

![Area visualization on labels](images/MAX_Lund_Mask-killBorders-lbl-Area.png)

Useful for:

- spotting unusually big/small objects  
- deciding filtering thresholds  
- checking measurement correctness  

### 10. Filtering objects by size

Remove small objects based on size:

`Plugins → MorphoLibJ → Label Images → Label Size Filtering`

- **Operation:** Lower Than  
- **Size threshold:** 100 px  

![Size filtering](images/MAX_Lund_Mask-killBorders-lbl-sizeFilt.png)

## 3D segmentation in Napari

In this exercise we will:

- Use Napari to open a 3D image
- Use Napari assistant to visualize a workflow for 3D image segmentation and labelling
- Use region props to quantify morphological parameters and make colorcoded plots

These steps form a complete workflow:  
**raw image → pre-processing → segmentation → cleanup → labeling → measurement → filtering**

We’ll use `Lund.tif` as the example image.

### 1. Open a 3D stack

Drag and drop the file or

`File → Open File`

![Brightness/Contrast histogram](images/Napari-load.png)

We will be able to see and explore the stack, and we can change to a 3D rendering with the option `Toogle 2D/3D view` in the lower left button pannel. We can also make orthogonal views by clicking the button to the right `Change order of the visible axis`.

In the right pannel we will see the Assistant plugin, where it suggests operations in the appropriate order. The amount of operations and options depends on your installed plugins. Some of them are redundant.

### 2. Remove background, binarization and labeling

Select `Remove Background → White top hat  → radius = 10`

![Brightness/Contrast histogram](images/Napari-bkg.png)

Then select `Binarize → Threshold Yen`, making sure to select the Result of White top-hat image.

![Brightness/Contrast histogram](images/Napari-thresh.png)

I recommend looking at the result in 3D.

![Brightness/Contrast histogram](images/Napari-3D.png)

Finally, we can select `Label → Connected component labeling`, make sure to select the Result of Threshold image. We can additionally select the `exclude on edges option`.

![Brightness/Contrast histogram](images/Napari-labels.png)

Some of them are stuck together. Let's try and fix that.

### 3. Fix labels

Let's select again the previous layer Result of Threshold. Then select `Process labels → Binary erosion → radius = 3`. This will reduce the objects of the binary segmentation.

![Brightness/Contrast histogram](images/Napari-erode.png)

Now let's recreate the labels `Label → Connected component labeling`, make sure to select the Result of Binary Erosion.

Then `Process labels → Expand Labels → radius = 3`. Explore the labels in 3D.

![Brightness/Contrast histogram](images/Napari-extend.png)

Now we can accurately measure morphological features of these labels. You can close the assistant pannel now.

### 4. Measure morphological properties

Select `Tools → Measure Tables → Object Features/Properties`. Here make sure to select the Result of Expanded Labels image. You can select different features, includding intensity features extracted from the raw data. After running a table should appear which can be exported in csv format. 

::: {layout-ncol=2}
![Brightness/Contrast histogram](images/Napari-table.png)
![Brightness/Contrast histogram](images/Napari-table2.png)

by double clicking any of the columns of this table, a new layer image will appear with colorcoded labels indicating the value of the selected measurement. Colormaps can be adjusted for preference.

![Brightness/Contrast histogram](images/Napari-colorcoded.png)


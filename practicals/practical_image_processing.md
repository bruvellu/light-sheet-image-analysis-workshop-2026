# Introduction to Thresholding in Fiji

In this exercise we will:

- Open an image in Fiji  
- Look at its histogram to relate intensities to thresholding  
- Apply all **global Auto Threshold** methods  
- Apply all **Auto Local Threshold** methods (after converting to 8-bit)  
- Visually compare the results

We’ll use `MAX_Lund.tif` as the example image.

---

## 1. Open the image and inspect the histogram

1. Start **Fiji**.
2. Open the image  
   `File → Open… → MAX_Lund.tif`
3. Open the Brightness & Contrast window:  
   `Image → Adjust → Brightness/Contrast…`

You should see a histogram like this:

![Brightness/Contrast histogram](Screenshot-2025-11-25-at-15.14.31.png)

**Idea:** thresholding is just choosing an intensity value that separates “foreground” (objects) from “background”. The histogram shows how many pixels have each intensity; a threshold is one vertical cut in this histogram.

---

## 2. Try all global Auto Threshold methods

Global thresholding uses **one single threshold value for the whole image**.

1. With `MAX_Lund.tif` active, run:  
   `Image → Adjust → Auto Threshold…`
2. In the dialog:
   - Choose `Method: Try all`
   - Set `White objects` (so objects will be white on black background)
   - Confirm.

Fiji will generate a montage showing the result of each global method, similar to:

![Global auto threshold montage](AutoThresh.tif)

Look at how different methods handle background, noise, and object boundaries. All of them use a **single** threshold value across the whole image.

---

## 3. Prepare the image for Auto Local Threshold

Local thresholding in Fiji requires an **8-bit** image.

1. Select the original image again:  
   `Window → MAX_Lund.tif`
2. Convert to 8-bit:  
   `Image → Type → 8-bit`  
   (If needed, enable scaling via `Edit → Options → Conversions…` and tick *Scale when converting*.)

Now the image is ready for local thresholding.

---

## 4. Try all Auto Local Threshold methods

Local thresholding computes a **different threshold in each neighborhood** (e.g. within a radius of 15 pixels). This adapts to uneven illumination and local contrast.

1. Run:  
   `Plugins → Auto Local Threshold…`  
   (or `Image → Adjust → Auto Local Threshold…` depending on your Fiji version)
2. In the dialog:
   - `Method: Try all`
   - `Radius: 15`
   - `Parameter 1: 0`
   - `Parameter 2: 0`
   - `White objects`
3. Confirm.

Fiji will generate a montage with all local methods:

![Local auto threshold montage](AutoLocalThresh1.tif)

Compare these results to the global methods. Local methods often segment structures better when the background is uneven or objects have varying brightness.

---

# Image processing, denoising, segmentation

Marina (1 hs)
- Segmentation, post-processing, Morphological operations, Labeling (MorphoLibJ in Fiji)
  
  https://knowhowspot.blog/fiji-morphological-segmentation-plugin-guide
- Napari assistant (MC I could do it based on RH talk - 30 min)


---
title: Cell tracking using Mastodon in Fiji
author: Bruno Vellutini
date: 05 January 2026
---

## Summary {#sec-summary}

This is a (very) basic tutorial on how to track cells using Mastodon [@Girstmair2025-qd] in Fiji [@Schindelin2012-di].

- Objectives: load/create Mastodon dataset, get familiar with navigating BigDataViewer and lineage windows, perform basic manual cell tracking with cell divisions, basic editing of lineages, try semi-automated detection and tracking, and some advanced analysis
- We will use the Mastodon dataset from Girstmair, J. (2024). Mastodon Auto-Tracking Demo on Parhyale hawaiensis Limb Development. Zenodo. https://doi.org/10.5281/zenodo.13944688
- The original data is from this paper https://elifesciences.org/articles/34410
- Mastodon has a detailed documentation. Please check it out for more details https://mastodon.readthedocs.io/

## Requirements {#sec-requirements}

- Parhyale dataset
- Fiji/ImageJ
- Mastodon

## Setup {#sec-setup}

### Download Dataset {#sec-setup-dataset}

Note! If you are following this during the course, the dataset has already been downloaded.

- Go to https://zenodo.org/records/13944688
- Click to download https://zenodo.org/records/13944688/files/Mastodon_Auto-Tracking_Demo_Ph-limb-dev.zip?download=1
- Wait. It's a 4.3GB ZIP file
- Move the file to a working directory
- Unzip `Mastodon_Auto-Tracking_Demo_Ph-limb-dev.zip`
- Wait. It'll be unzipped to 23GB

::: {layout-ncol=2}

![Your working directory after downloading and unzipping the dataset file.](media/01-dataset-zip.png)

![Contents of the dataset directory.](media/02-dataset-dir.png)

:::

### Download Fiji {#sec-setup-fiji}

- Go to https://fiji.sc
- Choose `Distribution: Stable`
- Click the big download button
- Copy `fiji-stable-linux64-jdk.zip` to working directory and unzip it
- Open the new directory `fiji-stable-linux64-jdk/Fiji.app/`
- Double-click on `fiji-linux-x64` launcher
- Fiji will open

![](media/03-fiji-dir.png)

### Install Mastodon {#sec-setup-mastodon}

- Click on `Help > Update...`

![](media/04-fiji-update.png)

- The updater will open and say Fiji is up-to-date
- Click `Manage Update Sites`


::: {layout-ncol=2}

![](media/05-fiji-up-to-date.png)

![](media/06-fiji-manage.png)

:::

- A window will open with a list of plugins available to install in Fiji

![](media/07-fiji-plugins.png)

- Search for "mastodon"

![](media/08-fiji-search.png)

- Several Mastodon related plugins will appear
- Click on the checkbox for `Mastodon`

![](media/09-fiji-mastodon.png)

- Click `Apply and Close` and then `Apply Changes`

![](media/10-fiji-apply.png)

- Wait... until the downloads are finished. Then, click `OK`

::: {layout-ncol=2}

![](media/11-fiji-download.png)

![](media/12-fiji-restart.png)

:::

- Restart Fiji (close window and double-click the launcher)

## Open Mastodon Project {#sec-open-project}

- In Fiji click on `Plugins > Tracking > Mastodon > Mastodon Launcher`

![](media/13-mastodon-launch.png)

- Mastodon Launcher window will open
- Click on "open Mastodon project" (top left) and "Open another project" (bottom right)

::: {layout-ncol=2}

![](media/14-mastodon-window.png)

![](media/15-mastodon-project.png)

:::

- Navigate to the directory `Mastodon_Auto-Tracking_Demo_Ph-limb-dev/`
- Select the file `Parhyale_LimbDev_30tps.mastodon`

![](media/16-mastodon-open.png)

- Several new windows will open (Console, Mastodon, BigDataViewer, TrackScheme, Data table)

![](media/17-mastodon-windows.png)

## Inspect the Dataset {#sec-inspect-dataset}

- Let's focus on the Mastodon window. Close Console, BigDataViewer, TrackScheme, Data table

![](media/18-mastodon-main.png)

- This is the main project menu from where you can open windows, set options, process data and save the project
- The most important buttons for this tutorial are `bdv` (BigDataViewer) and `trackscheme`
- Click on `bdv` and make the window larger

![](media/19-mastodon-bdv.png)

- Drag the timepoint slider at the bottom to see cells moving and dividing
- Using your acquired BigDataViewer skills, focus on the surface of the embryo
- If you get lost, press `Shift+Z` to re-orient the embryo
- Find a cell that divides before timepoint 15 and looks trackable
- Zoom on it using `Ctrl+Shift+scroll` and center it by holding the right button and dragging the mouse
- Use `Shift+scroll` to navigate through z and `M-N` to go through time

::: {layout-ncol=2}

![](media/20-nucleus-before.png)

![](media/21-nucleus-after.png)

:::

- You are ready to track

## Manual Tracking {#sec-manual-tracking}

- On the Mastodon project window, click on the `trackscheme` button
- The TrackScheme window will appear

![](media/22-trackscheme-window.png)

- Resize the bdv window to be side-by-side with the TrackScheme
- Click on the bdv window, set the timepoint to some frames before mitosis, `Shift+scroll` to find the center of the nucleus, put your mouse pointer there and press `A`
- A round magenta circle will appear over the nucleus and in the TrackScheme

![](media/23-manual-add.png)

- With your mouse over the circle, use `Shift+Q` and `Shift+E` to adjust the size of the spot to roughly the nucleus diameter

![](media/24-manual-resize.png)

- Zoom in on the new spot in the TrackScheme, hover and click on it and watch what happens in the bdv window

![](media/25-click-spot.png)

- Go back to the bdv, hover the pointer over the circle and hold the spacebar to adjust the position of the circle and the nucleus

![](media/26-adjust-position.png)

- Now let's add a second spot
- Hover the mouse inside the circle and hold `A`. This will advance to the next frame showing you the first spot in white dashed line and the second spot in white solid line with a white solid link between the two.

![](media/27-hold-a.png)

- Still holding `A`, position the second spot, then release `A` to create the new linked spot
- Check how the second spot and a link were created in the TrackScheme automatically

![](media/28-release-a.png)

- Continue to track the nucleus for a few more frames, until the frame immediately before division

- Note that when you click on a spot in the bdv window, the corresponding spot is highlighted in the TrackScheme window
- See what happens to the bdv when clicking the spots in TrackScheme... (nothing, unless it is the spot in view)
- Let's change that
- In the menu bar of bdv and TrackScheme windows there are locks 1, 2, 3. Click on lock 1 in both windows

![](media/29-lock-windows.png)

- Now click through spots in the TrackScheme; the view in the bdv will change to show the selected spot at the center

![](media/30-locked-behavior.png)

- Before we continue tracking the cell division, let's check one of the amazing Mastodon features
- Click on the `bdv` button in the Mastodon project window and another bdv window will open

![](media/31-bdv-new.png)

- Now activate lock 1 and click on one of the TrackScheme spots; both windows will be synchronized!

![](media/32-bdv-sync.png)

- Why is this useful for manual tracking?
- Adjust the view to center the spot in the second bdv window, press `Shift+Y`, and select a spot from the TrackScheme. Now we have both XY and ZY views of the same nucleus!

![](media/33-bdv-ortho.png)

- Which is great for tracking in 3D. You can check, for instance, that your spot is well centered in Z and adjust it in this window

![](media/34-bdv-division.png)

- Continue the tracking of one of the daughter cells. Select the last spot in the TrackScheme, go to the XY bdv, hover the mouse over the circle and hold `A`, move the spot, and release `A` to add it.
- Do it for a few frames
- Then go back to the pre-division spot and add a linked spot corresponding to the other daughter cell
- This will create the first branch of the lineage tree

![](media/35-add-split.png)

- Continue tracking the second daughter cell for a few frames

![](media/36-track-daughter.png)

- If you zoom out the TrackScheme view you will be able to see the full branched tree

![](media/37-tree-branch.png)

## Semi-Automated Tracking {#sec-semiauto-tracking}

- This mode will try to guess where the next nucleus is and automatically create the spots and links
- To start, choose a different nucleus to track and press `A` to add a new spot

![](media/38-semi-spot.png)

- Now, hovering the pointer above the spot press `Ctrl+T`
- A lineage will appear in the TrackScheme.

![](media/39-semi-auto.png)

- Check how accurate it is by clicking on the spots and watching their position relative to the nucleus in the bdv windows
- Try going further by hovering on a spot and pressing `Ctrl+T` to continue the semi-automated tracking. See how long you can go, how it behaves with cell divisions, and which cells work well with it and which don't

![](media/40-semi-more.png)

- There are many parameters that can be adjusted to tweak the semi-automated tracking behavior, check the documentation

## Automated Tracking {#sec-auto-tracking}

- The final part of this tutorial is to try automatic detection and linking of spots
- This is the dream: loading your data and getting out your lineage. However, in practice, it’s a lot messier. Cleaning, fixing, and curating the data is required to get a nice informative lineage
- We will use a simplified version of the protocol included in this dataset in the file `Protocol_Auto-Detection_Auto-Linking.docx`

![](media/41-auto-doc.png)

### Detection {#sec-auto-detection}

- In the Mastodon window, go to `Plugins > Tracking > Detection...`

![](media/42-auto-detect.png)

- Press `Next` twice (leave options as is)

::: {layout-ncol=2}

![](media/43-detect-info.png)

![](media/44-detect-roi.png)

:::

- Choose `Advanced DoG detector` and press `Next`
- Keep `Detect: bright blobs`, change `Estimated diameter` to `35px` and `Quality threshold` to `100`. `Behavior` should remain as `Add`

::: {layout-ncol=2}

![](media/45-detect-dog.png)

![](media/46-detect-params.png)

:::

- Click `Preview` and see how well the detection will work by exploring a new bdv window.

![](media/47-detect-preview.png)

- Do you observe too many false positives? You can change the diameter, for example, and try the preview again to see what happens to the detected spots
- Once satisfied, press `Next` and wait

![](media/48-detect-run.png)

- When the detection is done, press `Finish`

![](media/49-detect-done.png)

- The bdv windows and the TrackScheme will be showing a lot of new spots.
- Explore the spots in the BigDataViewer and TrackScheme windows. If you zoom in a lot you'll see the unlinked, individual spots per frame

![](media/50-detect-detail.png)

### Cleaning {#sec-auto-cleaning}

- Before we try to automatically link these spots, let's remove low quality detections
- On the Mastodon window click on `Table`, resize it to have more space, and resize the column `Detection q...` to show `Detection quality`

::: {layout-ncol=2}

![](media/51-table-open.png)

![](media/52-table-resize.png)

:::

- Click on `Detection quality` to sort the table

![](media/53-table-sort.png)

- Click on the first row to select it. Select all rows where `Detection quality` is <400
- Then click `Edit > Delete Selection`

![](media/54-table-delete.png)

- Close the table
- You can also manually delete obviously wrong spots by hovering and pressing `D`. Clean up the ones outside the embryo

### Linking {#sec-auto-linking}

- Now let's try linking
- In the main Mastodon window click on `Plugins > Tracking > Linking...`
- Keep `All spots` selected for all timepoints (0-29) and press `Next`
- Choose `Lap linker` and click `Next`

::: {layout-ncol=2}

![](media/55-link-timepoints.png)

![](media/56-link-lap.png)

:::

- Change the parameters to:
  - `Frame to frame linking`: `Max distance` to `40px`
  - `Gap closing`: `Max distance` to `60px` (keep others as is)
  - `Track division`: Check `Allow track division`, set `Max distance` to `40px`, and press `+` to add a `Feature penalty` and set it to `Center ch1` to `0.3`
- Press `Next` to start linking and wait... then press `Finish`

::: {layout-ncol=2}

![](media/57-link-params.png)

![](media/58-link-finish.png)

:::

- Note that there are now tracks in the bdv and TrackScheme windows. Explore them a bit

![](media/59-link-explore.png)

- Mastodon can calculate features (position, displacement, velocity, etc.) of individual spots, links and branches. Let's do that
- In the main Mastodon window press `compute features`. A Feature calculation window will open

![](media/60-compute-features.png)

- Press `compute` and wait... when it's done, close it.
- Note that now the tracks in the BigDataViewer is showing colored links 

![](media/61-computed-tracks.png)

- Open the table window from the main window
- It’ll be filled with computed features

![](media/62-computed-features.png)

## Basic Feature Visualization {#sec-feature-visualization}

- Finally, let's visualize the computed features that might be interesting or useful
- In the bdv window press `File > Preferences` to open the feature color coding visualization parameters

![](media/63-open-settings.png)

- On `Settings > Feature Color Modes` click on `Duplicate` (it'll generate a `Number of links (2)`) and then `Rename`. Rename it to `Velocity`

::: {layout-ncol=2}

![](media/64-colormode-settings.png)

![](media/65-colormode-duplicate.png)

:::

- On the `Coloring Spots` change `Read spot color from` to `Incoming link` and change `Feature` to `Link velocity`. Then click on `autoscale` in the range
- On the `Coloring Links` change `Read link color from` to `Link` and change `Feature` to `Link velocity`. Then also click on `autoscale`
- Click `Apply` (nothing will happen) then `OK`

![](media/66-colormode-params.png)

- On the bdv window press `View > Coloring > Velocity`

![](media/67-velocity-apply.png)

- The spots and links in the BigDataViewer window will change colors

![](media/68-velocity-colors.png)

- Do the same for the other bdv window and the TrackScheme

![](media/69-velocity-tracks.png)

- This gives a visual representation of cells which have a high displacement per frame. These might be artifacts in linking unrelated spots or, in a good processed lineage, reveal some biological process like cell migration

## Graph Plotting {#sec-graph-plotting}

- To finalize, a simple example of plotting
- Click on `grapher` in the main Mastodon window, a plot window will open

![](media/70-grapher-open.png)

- Press the lock 1 to lock the windows, select `Link velocity - outgoing` for X axis and `Detection quality` for Y axis and press `Plot`

![](media/71-grapher-plot.png)

- Find out if the spots with the highest link velocity are properly linked or if it is an artifact

![](media/72-grapher-analysis.png)

## References {#sec-references}

::: {#refs}
:::


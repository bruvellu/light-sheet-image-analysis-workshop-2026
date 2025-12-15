---
title: Cell tracking using Mastodon in Fiji
author: Bruno C. Vellutini
date: 05 January 2026
date-format: long
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

- Go to https://zenodo.org/records/13944688
- Click to download https://zenodo.org/records/13944688/files/Mastodon_Auto-Tracking_Demo_Ph-limb-dev.zip?download=1
- Wait. It's a 4.3GB ZIP file
- Move the file to a working directory
- Unzip `Mastodon_Auto-Tracking_Demo_Ph-limb-dev.zip`
- Wait. It'll be unzipped to 23GB

### Download Fiji {#sec-setup-fiji}

- Go to https://fiji.sc
- Choose `Distribution: Stable`
- Click the big download button
- Copy `fiji-stable-linux64-jdk.zip` to working directory and unzip it
- Open the new directory `fiji-stable-linux64-jdk/Fiji.app/`
- Double-click on `fiji-linux-x64` launcher
- Fiji will open

### Install Mastodon {#sec-setup-mastodon}

- Click on `Help > Update...`
- The updater will open and say Fiji is up-to-date
- Click `Manage Update Sites`
- List of plugins available to install in Fiji
- Search for "mastodon"
- Several Mastodon related plugins will appear
- Click on the checkbox for `Mastodon`
- Click `Apply and Close`
- Click `Apply Changes`
- Wait until downloads are finished
- Restart Fiji (close window and double-click the launcher)


::: {#fig-setup-fiji}

{{< video "media/Video1-Start_Fiji_and_install_Mastodon.mp4" >}}

Start Fiji and install Mastodon.

:::

## Open Mastodon Project {#sec-open-project}

- In Fiji click on `Plugins > Tracking > Mastodon > Mastodon Launcher`
- Mastodon Launcher window will open
- Click on "open Mastodon project" (top left) and "Open another project" (bottom right)
- Navigate to the directory `Mastodon_Auto-Tracking_Demo_Ph-limb-dev/`
- Select the file `Parhyale_LimbDev_30tps.mastodon`
- Several new windows will open (Console, Mastodon, BigDataViewer, TrackScheme, Data table)

## Inspect the Dataset {#sec-inspect-dataset}

- Let's focus on the Mastodon window. Close Console, BigDataViewer, TrackScheme, Data table
- This is the main project menu from where you can open windows, set options, process data and save the project
- The most important buttons for this tutorial are `bdv` (BigDataViewer) and `trackscheme`
- Click on `bdv` and make the window larger
- Drag the timepoint slider at the bottom to see cells moving and dividing
- Using your acquired BigDataViewer skills, focus on the surface of the embryo
- If you get lost, press `Shift+Z` to re-orient the embryo
- Find a cell that divides before timepoint 15 and looks trackable
- Zoom on it using `Ctrl+Shift+scroll` and center it by holding the right button and dragging the mouse
- Use `Shift+scroll` to navigate through z and `M-N` to go through time
- You are ready to track

## Manual Tracking {#sec-manual-tracking}

- On the Mastodon project window, click on the `trackscheme` button
- The TrackScheme window will appear
- Resize the bdv window to be side-by-side with the TrackScheme
- Click on the bdv window, set the timepoint to some frames before mitosis, `Shift+scroll` to find the center of the nucleus, put your mouse pointer there and press `A`
- A round magenta circle will appear over the nucleus and in the TrackScheme
- With your mouse over the circle, use `Shift+Q` and `Shift+E` to adjust the size of the spot to roughly the nucleus diameter
- Zoom in on the new spot in the TrackScheme, hover and click on it and watch what happens in the bdv window
- Go back to the bdv, hover the pointer over the circle and hold the spacebar to adjust the position of the circle and the nucleus
- Now let's add a second spot
- Hover the mouse inside the circle and hold `A`. This will advance to the next frame showing you the first spot in white dashed line and the second spot in white solid line with a white solid link between the two. Still holding `A`, position the second spot, then release `A` to create the new linked spot
- Check how the second spot and a link were created in the TrackScheme automatically
- Continue to track the nucleus for a few more frames, until the frame immediately before division
- Note that when you click on a spot in the bdv window, the corresponding spot is highlighted in the TrackScheme window
- See what happens to the bdv when clicking the spots in TrackScheme... (nothing, unless it is the spot in view)
- Let's change that
- In the menu bar of bdv and TrackScheme windows there are locks 1, 2, 3. Click on lock 1 in both windows
- Now click through spots in the TrackScheme; the view in the bdv will change to show the selected spot at the center
- Before we continue tracking the cell division, let's check one of the amazing Mastodon features
- Click on the `bdv` button in the Mastodon project window and another bdv window will open
- Now activate lock 1 and click on one of the TrackScheme spots; both windows will be synchronized!
- Why is this useful for manual tracking? Adjust the view to center the spot in the second bdv window, press `Shift+Y`, and select a spot from the TrackScheme. Now we have both XY and ZY views of the same nucleus! Which is great for tracking in 3D. You can check, for instance, that your spot is well centered in Z and adjust it in this window
- OK. Continue the tracking of one of the daughter cells. Select the last spot in the TrackScheme, go to the XY bdv, hover the mouse over the circle and hold `A`, move the spot, and release `A` to add it. Do it for a few frames
- Then go back to the pre-division spot and do the same for the other daughter cell
- This will create the first branch of the lineage tree (adjust the TrackScheme view to see it)

## Semi-Automated Tracking {#sec-semiauto-tracking}

- This mode will try to guess where the next nucleus is and automatically create the spots and links
- It requires you to set one spot, so let's pick a different nucleus. Choose a bright one you like
- Now, hovering above the circle press `Ctrl+T`
- A lineage will appear in the TrackScheme. Check how accurate it is by clicking on the spots and watching their position relative to the nucleus in the bdv windows
- Try going further by hovering on a spot and pressing `Ctrl+T` to continue the semi-automated tracking. See how long you can go, how it behaves with cell divisions, and which cells work well with it and which don't
- There are many parameters that can be adjusted to tweak the semi-automated tracking behavior, check the documentation

## Automatic Detection and Linking {#sec-auto-tracking}

- The final part of this tutorial is to try automatic detection and linking of spots
- This is the dream: loading your data and getting out your lineage. However, in practice, it’s a lot messier. Cleaning, fixing, and curating the data is required to get a nice informative lineage
- We will use a simplified version of the protocol included in this dataset in the file `Protocol_Auto-Detection_Auto-Linking.docx`
- In the Mastodon window, go to `Plugins > Tracking > Detection...`
- Press `Next` twice (leave options as is)
- Choose `Advanced DoG detector` and press `Next`
- Keep `Detect: bright blobs`, change `Estimated diameter` to `35px` and `Quality threshold` to `100`. `Behavior` should remain as `Add`
- Click `Preview` and see how well the detection will work by exploring a new bdv window. Do you observe too many false positives? You can change the diameter, for example, and try the preview again to see what happens to the detected spots
- Once satisfied, press `Next` and wait
- When the detection is done, press `Finish`
- The bdv windows and the TrackScheme will be showing all the spots. Explore a bit the bdv and TrackScheme. If you zoom in a lot you'll see the unlinked, individual spots per frame
- Before we try to automatically link these spots, let's remove low quality detections
- On the Mastodon window click on `Table`, resize it to have more space, and resize the column `Detection q...` to show `Detection quality`
- Click on `Detection quality` to sort the table
- Click on the first row to select it. Select all rows where `Detection quality` is <400
- Then click `Edit > Delete Selection`
- Close the table
- You can also manually delete obviously wrong spots by hovering and pressing `D`. Clean up the ones outside the embryo
- Now let's try linking
- In the main Mastodon window click on `Plugins > Tracking > Linking...`
- Keep `All spots` selected for all timepoints (0-29) and press `Next`
- Choose `Lap linker` and click `Next`
- Change the parameters to:
  - `Frame to frame linking`: `Max distance` to `40px`
  - `Gap closing`: `Max distance` to `60px` (keep others as is)
  - `Track division`: Check `Allow track division`, set `Max distance` to `40px`, and press `+` to add a `Feature penalty` and set it to `Center ch1` to `0.3`
- Press `Next` to start linking and wait... then press `Finish`
- Note that there are now tracks in the bdv and TrackScheme windows. Explore them a bit
- Mastodon can calculate features (position, displacement, velocity, etc.) of individual spots, links and branches. Let's do that
- In the main Mastodon window press `compute features`. A Feature calculation window will open
- Press `compute` and wait... when it's done, close it

## Basic Feature Visualization {#sec-feature-visualization}

- Finally, let's visualize the computed features that might be interesting or useful
- In the bdv window press `File > Preferences` to open the feature color coding visualization parameters
- On `Settings > Feature Color Modes` click on `Duplicate` (it'll generate a `Number of links (2)`) and then `Rename`. Rename it to `Velocity`
- On the `Coloring Spots` change `Read spot color from` to `Incoming link` and change `Feature` to `Link velocity`. Then click on `autoscale` in the range
- On the `Coloring Links` change `Read link color from` to `Link` and change `Feature` to `Link velocity`. Then also click on `autoscale`
- Click `Apply` (nothing will happen) then `OK`
- On the bdv window press `View > Coloring > Velocity`
- Do the same for the other bdv window and the TrackScheme
- This gives a visual representation of cells which have a high displacement per frame. These might be artifacts in linking unrelated spots or, in a good processed lineage, reveal some biological process like cell migration

## Graph Plotting {#sec-graph-plotting}

- To finalize, a simple example of plotting
- Click on `grapher` in the main Mastodon window, a plot window will open
- Press the lock 1 to lock the windows, select `Link velocity - outgoing` for X axis and `Detection quality` for Y axis and press `Plot`
- Find out if the spots with the highest link velocity are properly linked or if it is an artifact

## References {#sec-references}

::: {#refs}
:::

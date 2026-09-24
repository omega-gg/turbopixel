# turbopixel manual

turbopixel is a generative image compositor. It makes images on your own machine, and you compose
them: layer over layer, through masks and opacity. There is no brush and no pixel to push - unlike
a traditional image editor, it never works at the pixel level. A picture is built out of generated
images, the mask that cuts one out of its background, and the order and the opacity they are
stacked in.

Nothing is uploaded, nothing is tracked, and every image it makes lands in a folder you can open.

It has two screens. The **editor** is where you work: a canvas, a stack of layers, a prompt, and a
button that generates. The **Configure** screen is where you install what does the generating -
python, turboCLI, and one engine per kind of work. The three round buttons in the top left corner
move between them: the house opens the editor, the gear opens Configure, and the folder opens
the folder your images are written to.

## First run

On a fresh install the editor cannot generate yet - the line above the prompt says so and
`"Generate pixels"` stays grey. Open the Configure screen with the gear button and work down the
page.

**1. Pick a renderer.** The first block offers `"CPU"`, `"CUDA"` and `"Apple MPS"`. This is what
everything installed afterwards is built for, so it is worth getting right the first time.

| Choice | When to pick it |
| --- | --- |
| `"CPU"` | No dedicated graphics card. Works everywhere, and is the slowest. |
| `"CUDA"` | An Nvidia graphics card. |
| `"Apple MPS"` | An Apple Silicon Mac (M1 and later). |

The line under the buttons warns about the choice, for instance `"⚠️ Make sure you have a
compatible Nvidia CUDA GPU"`. Picking a renderer does nothing on its own: an `"Apply"` button
appears, and only applying records it. If turboCLI is already installed, applying also rebuilds
it for that renderer, and a second line says so.

**2. Install python.** The `"Installation"` section has two blocks. Each shows a coloured dot and a
state word - `"Checking"` while it looks, then `"Absent"` or `"Valid"` - and one button. Press
`"Install"` on `"python"` and wait for `"Valid"`.

**3. Install turboCLI.** The second block, `"Image generation engine built for CPU"` (or for
whichever renderer you applied). Its button stays grey until python is valid, and while a renderer
is picked but not applied. Press `"Install"`.

**4. Install an engine.** The `"Engines"` section appears once turboCLI is valid. The button at the
top of the list picks the category - `"Text to Image"`, `"Image to Image"` or `"Image to Mask"` -
and the list under it holds every engine of that category, the faint ones being the ones not
installed yet. Pick one, then press `"Install"` in the bottom right corner of its page.

An install downloads several gigabytes and can run for a long time. A line above the buttons says
`"🕓 Install in progress, this might take a long time. F1 for details"`, the state word reads
`"Installing"`, and the button becomes `"Abort"`. It is working even when nothing seems to move.
One install runs at a time: the rest of the page is locked while it does.

**5. Generate.** Go back to the editor with the house button, type something in the prompt at the
bottom - `"What do you want to generate ?"` - and press Enter. The first generation of a session
starts the generator and is slower than the ones after it.

What you get: a new layer holding the image, a file in the output folder, and a project saved by
itself. From there, the rest of this manual is the map.

## The editor

```
 +------------------------------------------------------------------------------------+
 | [home][gear][folder]              turbopixel                                        |
 | [ Untitled.tpx      ]                                          +-----------------+  |
 | [undo][redo]                                                   |  References     |  |
 | [hand][move]                                                   +-----------------+  |
 | [selection]                        canvas                      +-----------------+  |
 | +-----------------+                                            |  Settings       |  |
 | |  Selection      |                                            |                 |  |
 | +-----------------+                                            +-----------------+  |
 | +-----------------+                                            +-----------------+  |
 | |  Layers         |                                            | Text to Image   |  |
 | |                 |                                            | flux2-4b        |  |
 | +-----------------+         +------------------------+         | Generate pixels |  |
 | |  Properties     |         | What do you want to... |         +-----------------+  |
 | +-----------------+         +------------------------+                              |
 +------------------------------------------------------------------------------------+
```

- **Top left** - the three navigation buttons, then the project button carrying the name of the
  current project, then undo and redo, then the three tools.
- **Left column** - `"Selection"` (only while the selection tool is picked), `"Layers"`, and the
  `"Properties"` panel at the bottom.
- **Right column** - `"References"`, `"Settings"`, and the generate block in the corner: the mode
  button, the engine button under it, and `"Generate pixels"`.
- **Bottom centre** - the prompt. A line above it warns when something is missing, and while a
  generation runs a second line shows its progress.
- **Middle** - the canvas, on a checkerboard that stands for transparency. An empty document shows
  `"New layer"` and, under it, `"Drop or"` `"open"` `"an image file"` - where `"open"` is a link to
  the file dialog.

`Tab` hides the whole interface and leaves the canvas alone on screen. Press it again to bring the
panels back.

## The canvas and the tools

Three tool buttons sit under undo and redo, and one is always picked.

- **Hand** (`H`) - dragging moves the view. The wheel zooms, between a tenth and ten times the real
  size, and the arrow keys pan.
- **Move** (`V`) - dragging moves the current layer. The wheel, `+` and `-` scale it around its
  middle, and the arrow keys nudge it by one pixel, or sixteen with `Shift` held. A press that does
  not move is a click, and it picks the layer under the pointer - transparent pixels are seen
  through, and a layer inside a group picks the group.
- **Selection** (`F`) - dragging draws the selection frame.

Holding `Space` pans with any tool, and gives the tool back when you let go. Holding `Shift` while
zooming or scaling moves in smaller steps.

Nothing moves or scales when the current layer is hidden, or when the group holding it is.

## The selection frame

The frame decides what a generation reads and where its result lands. With a frame up, a run works
on that rectangle alone and the new layer is created on it. With no frame, runs are about the whole
canvas.

Drag with the selection tool to draw one. The sides snap to multiples of sixteen, which is what the
engines count in, and the frame never leaves the canvas. Clicking a layer row while the tool is
picked frames that row whole. Clicking the canvas clears the frame.

The frame belongs to the project, so it is still there when you reopen it - and it keeps steering
every run even when you cannot see it, because it is only drawn while the selection tool is picked.
If results keep landing in the same corner, pick the selection tool and click once on the canvas.

While the tool is picked, the `"Selection"` panel shows the frame as numbers: `w` and `h` with a
link button that keeps their ratio, then `x` and `y` with a reset that clears the frame.

## Layers

The `"Layers"` panel lists the layers, the top row being the layer drawn over the others. Each row
holds an eye that shows or hides it, a miniature of the canvas showing where the layer sits in it,
and its name. Clicking the row selects it, and the `"Properties"` panel follows.

Clicking a miniature opens the file in whatever your system opens images with. Hovering it shows a
larger preview beside the panel.

Names are given for you - `Background` for the first layer of a new project, then `Layer 1`,
`Layer 2`, and `Group 1`, `Group 2` for groups. A number already taken is never reused, and there
is no rename.

Drag a row up or down to reorder it. A coloured line shows where it will land; when that line is
indented, the row is going into the group above it.

Four buttons sit under the list:

- **Clone** - copies the current row right above itself and selects the copy. A group is copied
  with everything it holds.
- **Group** - adds an empty group over the current row.
- **Add** - adds an empty layer over the current row, inside the group that row lives in.
- **Remove** - deletes the current row, and a group takes its layers with it. It refuses to empty
  the document, so the last drawing layer stays.

`"Opacity"` is on the layer's page in the `"Properties"` panel: a slider, a number between 0.00 and
1.00, and a reset. At 0 the layer disappears from the canvas, is left out of renders, and stops
answering clicks - what is underneath it is picked instead.

## Groups

A group holds layers and draws nothing of its own. Moving it moves its layers, resizing it scales
them, hiding it hides them, its opacity fades them, and deleting it deletes them. A group's opacity
multiplies into its layers: a group at 0.5 holding a layer at 0.5 draws it at 0.25.

Drag a row rightwards past the indent to put it in a group, and leftwards to take it out. A group
cannot hold another group. The chevron on its row collapses it: its layers leave the list, and the
canvas keeps drawing them.

On a group the second tab of the `"Properties"` panel reads `"Group"` and holds the geometry and
the opacity only - a group has no image of its own, so no source and no fill buttons.

## The properties panel

Three tabs at the bottom left. Selecting a layer row jumps to the second one, unless you are on
`"Mask"`, which stays where it is.

**`"Canvas"`** - the size of the document. `"Geometry"` gives width and height with a link button
that keeps their ratio, and two buttons: `"Default"` puts the canvas back to 1024 x 768, and
`"Fit to layer"` takes the size of the current layer's image and makes that layer cover the canvas
whole. Sizes are between 1 and 8192, and a field commits when you leave it or press Enter, not as
you type.

**`"Layer"`** - everything the current layer carries.

- `"Source"` - the image file, as a thumbnail, a path you can type or paste, and a folder button
  opening `"Select an image"`. `"⚠️ Image file is not available"` appears under it when the file
  has moved or been deleted; the layer keeps pointing at it. When the layer has a mask, the right
  of the title says `"Applying mask..."`, `"Mask is active"` or `"Mask is hidden"`.
- `"Opacity"` - see above.
- `"Geometry"` - `w` and `h` with the same link button, then `x` and `y` with a reset that puts the
  layer back on the frame it was generated on, or on the canvas when it was never generated. The
  size of the image file itself is written on the right.
- `"Fit"`, `"Expand"`, `"Stretch"` - what the image does inside the layer box. `"Fit"` scales it to
  sit entirely inside, `"Expand"` scales it to cover and clips the overflow, `"Stretch"` distorts
  it to the box.

Picking an image never changes the layer's geometry: the file is fitted into the box the layer
already has.

**`"Mask"`** - the mask of the current layer and what it produces.

- `"Mask"` - the mask file, with a thumbnail, a path and a folder button. `"Applying mask..."`
  shows beside the title while the layer is being rebuilt.
- `"Composite"` - the layer seen through its mask. This is what the canvas draws. The field is read
  only, since the composite is built rather than chosen, and the eye button beside it hides the
  composite so the layer draws its original image again, keeping both files.
- `"Clear mask"` - drops the mask and the composite in one step.

Changing the mask - by a generation, the folder button or a typed path - rebuilds the composite by
itself.

## References

A reference is an image the generation looks at. Only `"Image to Image"` uses them, and the
`"References"` panel lists them on the right.

The first row is always `"Canvas"`. It stands for the picture you are working on, and it cannot be
removed, moved, or pushed down by another row. When it is checked, the run is given the canvas as
it stands - up to and including the row the result will land on, so the answer does not come back
on top of itself.

Every other row is a file. Each has a check that says whether it goes into the generation, a
thumbnail and the file's name. The `"+"` button adds an image, unchecked. The trash removes the
current one. Dragging reorders them, and the order matters: the engine is given them top to bottom.
The reset button beside `"+"` unchecks every image at once, leaving `"Canvas"` as it is.

The panel lets you add as many as you like, and a generation takes four images at most - the
canvas counted among them - so keep the checks down to four.

## Modes

The button at the top of the generate block opens the mode menu, and the mode says what the next
generation does.

| Mode | Key | Needs | Produces |
| --- | --- | --- | --- |
| `"Text to Image"` | `T` | An engine and a prompt | A new layer with the image |
| `"Image to Image"` | `I` | An engine, a prompt, a reference | A new layer with the image |
| `"Image to Mask"` | `M` | An engine and a layer with an image | A mask for that layer |
| `"Render"` | `R` | Nothing | A PNG of the canvas, in the output folder |

**`"Text to Image"`** writes the prompt into a picture. The new layer is created over the current
row, on the selection frame, or on the whole canvas when there is no frame.

**`"Image to Image"`** does the same but shows the engine what you already have. Check `"Canvas"`
to give it the picture you are editing, and add reference files for anything else. With nothing
checked, `"Generate pixels"` stays grey.

**`"Image to Mask"`** cuts the current layer out of its background. It takes no prompt - the
layer's own image is the input - and it produces a mask, which is then applied to give the layer
its composite. A frame limits the work to the framed part, and everything outside it becomes
transparent.

**`"Render"`** needs no engine and no prompt: it writes the canvas exactly as you see it, minus the
hidden layers, to a PNG in the output folder and says `"Render saved in: "` with the path.
`Ctrl+R` does it from any mode, even while a generation runs.

Each mode remembers its own engine, so switching between them is instant. Landing on an engine
brings its own `"Renderer"` and `"Inference"` into the `"Advanced"` page, since those are what it
was installed with. Your `"Seed"` is never touched.

The engine button under the mode button reads `"Checking engines"` while the list is being fetched
and `"No engine available"` when nothing is installed for that mode. It is disabled in both cases.

## The settings panel

Two tabs on the right, above the generate block. The first is named after the current mode, the
second is `"Advanced"`.

The mode page holds the size of what will be generated: a width and a height with a link button,
then a slider that multiplies the canvas size between a quarter and four times, with the multiplier
written beside it and a reset. Sizes land on multiples of sixteen; a size you type is taken as it
is. Under it, `"Seed"` reads `"Default"` until you give it a number, and its reset gives the
default back - the same seed and the same prompt generate the same image again.

Each mode adds its own:

- `"Text to Image"` and `"Image to Image"` show `"Generate on"` with the name of the layer the
  result will land over. Hovering it shows the canvas the way the run will see it.
- `"Image to Image"` says what it will send, for instance `"Canvas and 1 reference selected"` or
  `"No reference selected"`.
- `"Image to Mask"` shows `"Apply to"` with the layer being cut out, a `"Cutoff"` slider between 0
  and 255 - the higher it is, the fewer pixels the mask keeps - and, on the `mask` engine only,
  `"Default"` and `"Region"` buttons.
- `"Render"` has a single tab, `"Render image"`, holding the size alone.

The `"Advanced"` page holds `"Renderer"` - `"CPU"`, `"CUDA"` or `"MPS"` - and `"Inference"`, the
number of steps a generation takes. Both show `"Default"` until you change them, and both have a
reset. More steps are slower and can add detail.

These settings live for the session only: they follow the canvas size and are not kept in the
project.

## Generating

Press `"Generate pixels"`, or Enter in the prompt - `Shift+Enter` makes a new line there instead.
Pressing it closes the mode and engine menus.

The first generation of a session starts the generator, so it takes longer than the ones after it.
A progress line replaces the notice above the prompt, reading the mode and the percentage, with a
stop button on its right that cancels the run. The window stays usable throughout.

The result is written to the output folder and added as a new layer over the current row - inside
the group that row lives in. The layer takes the canvas size, with the image fitted into it.

Undo takes the generated layer back, but the file stays in the output folder: nothing you generate
is ever lost by an undo. The folder button in the top left corner opens that folder.

`"Generate pixels"` is grey when there is no engine, while a generation is running, when
`"Image to Image"` has nothing checked, and when `"Image to Mask"` has no usable layer. In
`"Text to Image"` and `"Image to Image"` it does nothing while the prompt is empty.

## When the app stops you

The line above the prompt says what is missing. It shows nothing while the checks are still
running, and nothing at all in `"Render"` mode.

| Notice | What to do |
| --- | --- |
| `"⚠️ No generator available. Go to the Configure page"` | Install an engine for this mode. |
| `"⚠️ The current layer has no image"` | Select a layer that holds an image. |
| `"⚠️ The selection is outside the current layer"` | Move the frame over the layer, or clear it. |
| Anything else ending in `"Configure page"` | Install python or turboCLI, see `First run`. |

## Projects and files

A project is a `.tpx` file. The button under the navigation shows the one you are in -
`Untitled.tpx` until you name it - and opens a menu:

- `"New"` - starts an empty project. From `Untitled.tpx`, which has nowhere else to live, it asks
  first: `"This will erase the current project"`, with `"Yes"` and `"No"`.
- `"Load"` - opens a `.tpx` from disk.
- `"Save as"` - gives the project a name and a place of your own choosing.

There is no Save button: every edit is written to the current project as you work, and the app
reopens the last one you were in when it starts. Undo goes back fifty steps, with the buttons
beside the project one or with `Ctrl+Z` and `Ctrl+Y`.

A project points at your images by their path rather than holding a copy, so moving or deleting one
breaks the layer that uses it - the layer then shows `"⚠️ Image file is not available"`.

Because they are ordinary files, you can edit them elsewhere: change a layer's image, a mask or a
reference in another program, save it, and turbopixel redraws by itself. Generated images, masks
and renders are written to turbopixel's own folders; the folder button opens the one holding the
images.

## The Configure screen

The gear button opens it, and the house button goes back to the editor. `First run` walks through
it in order; this is what each part does when you come back to it.

**Renderer.** `"CPU"`, `"CUDA"` and `"Apple MPS"`, with a warning line about the one picked.
Changing it shows `"Apply"`, and applying rebuilds an installed turboCLI for that renderer -
`"🚨 Applying reinstalls turboCLI for CUDA"` warns before you do. With turboCLI not installed, the
choice is simply what the next install uses.

**`"Installation"`.** `"python"` first, then `"turboCLI"` under it. Each block shows a dot and a
state - `"Checking"`, `"Absent"`, `"Valid"`, `"Installing"` or `"Removing"` - a refresh button that
looks again, and one button: `"Install"`, `"Remove"`, or `"Abort"` while it works. `"turboCLI"` is
grey until python is valid, and while a renderer is picked but not applied. Removing python cleans
turboCLI first, and neither removal deletes the models you downloaded. A title that reads `"Valid"`
opens its folder when clicked.

**`"Engines"`.** Hidden until turboCLI is valid. The button over the list picks the category, and
the list holds every engine of it - the faint ones are not installed. The page on the right shows
the engine you picked:

- Its name, then its state and a refresh button that reads the engine again, dropping any change
  you have not applied.
- `"Renderer"` for this engine alone, `"dtype"`, `"Inference"`, `"Offload"` and `"Data slicing"`.
  Each carries a line saying what it is for. Leave them as they are unless you know why you are
  changing them - the defaults are what the engine was installed with.
- `"ComfyUI folder to fetch the engine inside (optional)"`, on `comfy-` engines only. Point it at
  an existing ComfyUI install to reuse the model files already there instead of downloading them
  again. Left empty, turbopixel uses its own folder. A folder that does not exist keeps
  `"Install"` and `"Apply"` grey.
- In the corner, `"Install"` when the engine is absent, `"Delete"` when it is installed - which
  asks `"Are you sure ?"` first - and `"Abort"` while an install runs. `"Apply"` beside them stays
  grey until a setting differs from what the engine was installed with; applying records the new
  settings without downloading anything.

One install runs at a time. While one does, the category button, the list and the two blocks above
are locked, and the line above the buttons says it might take a while.

## Keyboard shortcuts

| Key | What it does |
| --- | --- |
| `Tab` | Shows or hides the interface |
| `Space` | Pans the view while held, whatever the tool |
| `H` | Hand tool |
| `V` | Move tool |
| `F` | Selection tool |
| `T` | `"Text to Image"` mode |
| `I` | `"Image to Image"` mode |
| `M` | `"Image to Mask"` mode |
| `R` | `"Render"` mode |
| `Ctrl+R` | Renders the canvas, from any mode |
| Arrows | Pans the view, moves the layer or the frame - sixteen pixels with `Shift` |
| `+` and `-` | Zooms, or scales the layer with the move tool |
| `Enter` | Generates, from the prompt - `Shift+Enter` makes a new line |
| `Ctrl+Z` | Undo |
| `Ctrl+Y` | Redo, and so does `Ctrl+Shift+Z` |

A field you are typing in takes the letter first, so the prompt is safe to type in.

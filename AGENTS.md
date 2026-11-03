# Agent notes - arya-comfyui-workflows

Public distribution of the ComfyUI **API-format** workflow JSONs used by the Arya
Image Gen plugin (`dsh-arya-image-gen`). Plain data: no build, no package, no
tests, no CI. Default branch is `master` - the rest of the suite uses `main`.

## Layout

Six graphs plus the README that documents them:

    Qwen_Image_2_1_Turbo_Wide_1344x768.json
    Qwen_Image_2_1_Turbo_Square_1024.json
    Qwen_Image_2_1_Turbo_Portrait_768x1344S.json
    Qwen_Image_2_1_Turbo_Edit.json
    Qwen_Image_2_1_Remove_Background.json
    Qwen_Image_2_1_Cutout_RGBA_1024.json

## Rules

1. **API-format only.** These were produced with ComfyUI's `Save (API Format)`;
   a workflow saved the ordinary way will not run through the plugin.
2. **The placeholders are the interface.** `{{prompt}}` is required and receives
   the user prompt, `{{seed}}` is a fresh random seed per call, and `{{image}}`
   appears exactly once inside a `LoadImage` - its presence is what marks a graph
   as edit-capable and lets it serve image-to-image and canvas selections.
   Removing or duplicating it changes what the plugin will let users do.
3. **Node roles are load-bearing.** Node 14 (`TextEncodeQwenImage21`) carries the
   prompt and reference images; node 20 is the `LoadImage` that receives
   `{{image}}`. Keep the `images.image_1` autogrow key when editing that link by
   hand.
4. **Update the README table whenever a file is added, removed, or changes its
   output size or capability.** That table is the only documentation users get,
   and it is what the plugin's picker descriptions mirror.
5. **Models are Qwen Image 2.1** (UNET, CLIP, VAE) at the revisions the graphs
   name. Exports came from ComfyUI 0.37.x; if a node is missing on another
   server, re-export there rather than guessing at node ids.
6. `*.zip` is gitignored: packs are build artifacts, never source.

## Relationship to the plugin

`dsh-arya-image-gen` keeps its own prefill pack in `docs/workflows/*.json`, under
different file names, generated into the plugin at build time. Nothing syncs the
two automatically - a graph change here does not reach the plugin, and a plugin
prefill change does not reach here. When a workflow should change in both, change
both.

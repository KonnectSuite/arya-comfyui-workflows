# Arya ComfyUI workflows

ComfyUI **API-format** workflows for the [Arya Image Gen](https://github.com/KonnectSuite/dsh-arya-image-gen) plugin. Each file becomes a selectable "workflow" in the plugin's chat pill, Studio picker, and comparison view.

Clone this on the machine that runs the plugin:

```bash
git clone https://github.com/KonnectSuite/arya-comfyui-workflows.git
```

Then in the plugin: **Settings → Plugins → Arya Image Gen → Workflows → Import workflow JSON**, one file at a time, and select the active workflow.

## Files

| File | Output | Capability |
|---|---|---|
| `Qwen_Image_2_1_Turbo_Wide_1344x768.json` | 1344×768 | Text to image |
| `Qwen_Image_2_1_Turbo_Square_1024.json` | 1024×1024 | Text to image |
| `Qwen_Image_2_1_Turbo_Portrait_768x1344S.json` | 768×1344 | Text to image |
| `Qwen_Image_2_1_Turbo_Edit.json` | source size | Text to image + image editing |
| `Qwen_Image_2_1_Remove_Background.json` | source size | Text to image + image editing |
| `Qwen_Image_2_1_Cutout_RGBA_1024.json` | 1024×1024 | Text to image (RGBA cutout) |

## Placeholders

- `{{prompt}}` — required; the plugin injects the user prompt.
- `{{seed}}` — a random seed per call.
- `{{image}}` — exactly once in a `LoadImage` input; marks a workflow as edit-capable so it can be used for **Image to image** and canvas selections.

## Requirements

- ComfyUI with the models these graphs load: **Qwen Image 2.1** (UNET, CLIP, VAE) at the revisions the workflows name.
- The workflows were exported from ComfyUI 0.37.x with **Save (API Format)**. Re-export from your own server if a node is missing.
- A remote ComfyUI works: point the plugin endpoint at that server and make sure it can reach these models; the plugin itself needs nothing installed locally.

## Editing

These are plain JSON graphs. Node 14 (`TextEncodeQwenImage21`) holds the prompt and reference images; node 20 is the `LoadImage` that receives `{{image}}`. Keep the `images.image_1` autogrow key when editing that link by hand.

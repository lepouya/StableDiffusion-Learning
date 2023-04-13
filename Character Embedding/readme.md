# Automatic1111 setup

## Launch script
```
set PYTHON=
set GIT=
set VENV_DIR=
set COMMANDLINE_ARGS=--xformers --opt-sdp-attention
set PYTORCH_CUDA_ALLOC_CONF=garbage_collection_threshold:0.9,max_split_size_mb:512

git pull
```

## Saving Images/Grids -> Images filename pattern
`[prompt_spaces] ([seed].[datetime])`

## User Interface -> Quichsettings list
`sd_model_checkpoint, sd_vae`

# Training photos

## Positive Prompt
> an extreme closeup front shot photo of %your character's look% (naked:1.3), %your character's body shape%, %your character's hairstyle%, (neutral gray background:1.3), neutral face expression

## Negative Prompt
> (gray hair, glasses, earrings, necklace, high heels, tattoo:1.3),

> (deformed, distorted, disfigured, mutated, mangled, disgusting, amputated:1.3), (text, frame, watermark, signature, copyright, username, error:1.4), poorly drawn, ugly, blurry,

> bad anatomy, wrong anatomy, extra limb, missing limb, floating limbs, (mutated hands and fingers:1.4), disconnected limbs,

## txt2img settings
* Sampling method: DPM++ 2M Karras
* Sampling steps: 30
* Restore Faces: On
* Tiling: Off
* Hires. fix: Off
* Width: 512
* Height: 512
* CFG Scale: 7
* Seed: -1
* Batch count: 2
* Batch size: 8

## Script
* `X/Y/Z Plot`
* X type: `Prompt S/R`
* X values:
> an extreme closeup, a medium closeup, a closeup, a medium shot, a full body
* Y type: `Prompt S/R`
* Y values:
> front shot, rear angle, side angle, shot from above, low angle shot
* Z type: `Nothing`
* Draw Legend
* Keep -1 for seeds

## Generate

## Filtering
* poor quality, messed up, cropped
* clothing, jewlery, tattos, accessories, effects
* wrong hair, face, body style
* mutations or weird angles
* anything that doesn't look like the character we want

Pare down to 25 good pics. Include different angles and zooms

## Renaming
* Remove things that belong to the character: the character's look; the character's body shape; the character's hairstyle
* Keep things that are in all pics but don't belong to the character: naked, neutral gray background
* Remove angles, they don't help training
* Keep zoom levels
* Include character's name in the prompt. e.g.
> a closeup photo of myCh4r4ct3rHere naked, neutral gray background (1234567890).png

## Zoom definitions
1) Can you see the knees? If so, it's probably "a full body" photo.
2) Can you see the waist? If so, it's probably "a medium shot" photo.
3) Can you see the breast area? If so, it's probably "a closeup" photo.
4) Can you see the neck area? If so, it's probably "a medium closeup" photo.
5) If none of the above, it's probably "an extreme closeup" photo.

# Embedding creation

## Character embedding creation
* Name: Choose a unique name that wouldn't appear in any other training. Use prefixes and numbers
* Initialization text: A specific starting point for training to apply everything it has learned. Like `woman`
* Number of vectors per token: 8-16 ish
* Overwrite Old Embedding

## Embedding setup
* Switch back for sd 1.5 base checkpoint model for training
* Embedding: char name
* Hypernetwork: none
* Embedding learning rate: `0.005:100, 1e-3:1000, 1e-5`
* Gradient Clipping: disabled
* Batch size: 1
* Gradient accumulation steps: 1
* Dataset directory: Where all the named and tagged images are
* Prompt template: your custom template. With code `[name], [filewords]`
* Width, Height: same as training pics
* Max steps: 2500
* Save image + embedding to log dir: every `100` steps
* Save images with embedding in PNG chunks
* Read parameters from txt2img tab when making previews
* latent sample method: `once`

## Custom testing prompts
Back in txt2img tab:
* Prompt: a closeup photo of %embedding name% in %sample scenario%
* Negative prompt: empty
* Sampling method: Euler A
* Sampling steps: 20
* Restore Faces: Off
* Tiling: Off
* Hires. fix: Off
* Width: 512
* Height: 512
* Batch count: 1
* Batch size: 1
* CFG Scale: 7
* Seed: -1
* ControlNet: off
* Script: None

## Train

# Validation

## Prepare embeddings
* Copy all generated embeddings to root-level embeddings folder
* `Show/hide extra networks` in txt2img tab and refresh, hide it again afterwards
* Switch back to the checkpoint/VAE that you want

## Comparison grid
* Prompt:
> a closeup photo of myCh4r4ct3rHere %in a simple situtation%
* Megative prompt: similar to the original generation
* Sampling method: DPM++ 2M Karras
* Sampling steps: 30
* Restore Faces: Off
* Tiling: Off
* Hires. fix: Off
* Width: 512
* Height: 512
* Batch count: 1
* Batch size: 4
* CFG Scale: 8 (note: higher than the default)
* Seed: 12345678 (note: different to the default)
* Script: `X/Y/Z Plot`
* X type: `Nothing`
* Y type: `Prompt S/R`
* Y values:
> myCh4r4ct3rHere, myCh4r4ct3rHere-20, myCh4r4ct3rHere-40, myCh4r4ct3rHere-60, myCh4r4ct3rHere-80, myCh4r4ct3rHere-100, myCh4r4ct3rHere-120, myCh4r4ct3rHere-140
* Z type: `Nothing`
* Draw Legend
* Do not Keep -1 for seeds
* Grid margins (px): 16 (note: different to the default)

## Fine tuning
* Find out which one is best
* switch to -5 increments and hone in on best one
* Rename the embedding to the one without numbers, delete all the extra embeddings

## Generate sample pics

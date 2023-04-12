# Training photos

### Images filename pattern
`[prompt_spaces] ([seed].[datetime])`

### Positive Prompt
> an extreme closeup front shot photo of %your character's look% (naked:1.3), %your character's body shape%, %your character's hairstyle%, (neutral gray background:1.3), neutral face expression

### Negative Prompt
> (gray hair, glasses, earrings, necklace, high heels, tattoo:1.3),

> (deformed, distorted, disfigured, mutated, mangled, disgusting, amputated:1.3), (text, frame, watermark, signature, copyright, username, error:1.4), poorly drawn, ugly, blurry,

> bad anatomy, wrong anatomy, extra limb, missing limb, floating limbs, (mutated hands and fingers:1.4), disconnected limbs,

### txt2img settings
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

### script
* X/Y/Z Plot
* X type: `Prompt S/R`
* X values:
> an extreme closeup, a medium closeup, a closeup, a medium shot, a full body
* Y type: `Prompt S/R`
* Y values:
> front shot, rear angle, side angle, shot from above, low angle shot
* Z type: `Nothing`
* Draw Legend
* Keep -1 for seeds


Comparisons and experimentation with different clips, models, vaes, and samplers.

Every image is replayable by drag and dropping it into ComfyUI

# CLIPTextEncode

## Positive

- Quality

  > (masterpiece), HDR, high quality, intricate, highly detailed, sharp focus, intricate details, hyperdetailed, low contrast,

- Photo realistic

  - **NOT** _for anime use_

  > 8K UHD, DSLR, film grain, Fujifilm XT3, soft lighting, (cinematic look), soothing tones, natural skin texture, looking at camera, (high detailed skin:1.2), RAW photo,

## Negative

- Quality

  > poorly drawn, blurry, (text), (frame), (watermark), (signature), (copyright:1.2), username, error, (depth of field),

- Deformities

  > (deformed, distorted, disfigured:1.4), bad anatomy, wrong anatomy, extra limb, missing limb, floating limbs, (mutated hands and fingers:1.4), disconnected limbs, mutation, mutated, amputation, cross eyed, (deformed iris, deformed pupils:1.4), (worst quality, low quality:1.4), JPEG artifacts, morbid, extra fingers, mutated hands, poorly drawn hands, poorly drawn face, dehydrated, bad proportions, gross proportions, fused fingers, too many fingers, long neck, deformed mouth, weird teeth, cropped face, clipped face, (extra leg, extra hand, mismatched hands, mismatched legs, extra feet, extra arms, missing hand, missing foot, bad arms, bad legs:1.4), long neck, (forehead mark), (sagging breasts),

- Faces
  > censor, censorship, ugly, close-up, cropped, out of frame, duplicate, cloned face,

# CheckpointLoaderSimple

### Lessons

- All of the results with `Anything 3`, `Anything 4`, and `AOM2` are reproducible with `AOM3` with some adjustments to sampling parameters
- AOM3 seems to be the most versatile one for Anime drawings. I won't do any realistic pics in this page
- AOM3A1 doesn't have any 3D effects. Looks like older anime and some game art.
- AOM3A2 has "oil painting" look. Very artistic.
- AOM3A3 has nice 3D effects and looks like modern anime

### Setup

- > Witch (girl) wearing long hooded red robe, long blonde hair, blue-green eyes,
- seed `7776483249880570000`

### Results

| AOM2                  | AOM3                  | AOM3A1                | AOM3A2                | AOM3A3                | Anything v3           | Anything v4.5         |
| --------------------- | --------------------- | --------------------- | --------------------- | --------------------- | --------------------- | --------------------- |
| ![](anime_00007_.png) | ![](anime_00002_.png) | ![](anime_00008_.png) | ![](anime_00009_.png) | ![](anime_00010_.png) | ![](anime_00071_.png) | ![](anime_00072_.png) |

# VAELoader

### Lessons

- `vae-ft-mse-840000-ema-pruned` is very vivid and colorful. `kl-f8-anime2` produces very similar results.
- The default VAE build into AOM looks a bit washed out. `Anything v3`'s default VAE is much closer to the two above.
- `orangemix` VAE is about half-way point. I would have expected this one to be the "built-in" VAE for AOM, but it doesn't to be the case for me.

### Setup

- AOM3
- 20 steps / 8 cfg

### Results

| default               | ft-mse-840000         | orangemix             | kl-f8-anime2          |
| --------------------- | --------------------- | --------------------- | --------------------- |
| ![](anime_00002_.png) | ![](anime_00003_.png) | ![](anime_00004_.png) | ![](anime_00005_.png) |

# KSampler

## **steps / cfg**

### Lessons

- There seems to be a clear minimum and maximum cfg value per # of steps. Values outside of that range can lead to really weird artefacts
- cfg merges details together and adds intricacies as it increases, but can lead to errors with lower step counts.
- More new noise and pieces are added with each step, but it becomes more important to have higher cfgs to detail them properly.
- Generation time increases linearly and by a large amount as steps increases, but changes very little as cfg increases. Using the right balance of cfg/steps can cut down on generation time a lot
- Output results of different anime models can be obtained by playing with these values.
- `steps = 3 * cfg` seems to be around the best ratio of foreground to background details. Higher steps put more focus on the foreground character

### Setup

- AOM3 + ft-mse-840000
- euler sampler + normal scheduler

### Results

|          | 2.0 cfg               | 3.0 cfg               | 4.0 cfg               | 5.0 cfg               | 6.0 cfg               | 7.0 cfg               | 8.0 cfg               | 10 cfg                | 12 cfg                | 15 cfg                |
| -------- | --------------------- | --------------------- | --------------------- | --------------------- | --------------------- | --------------------- | --------------------- | --------------------- | --------------------- | --------------------- |
| 5 steps  | ![](anime_00011_.png) | ![](anime_00012_.png) | ![](anime_00013_.png) | ![](anime_00014_.png) | ![](anime_00015_.png) | ![](anime_00016_.png) | ![](anime_00017_.png) | ![](anime_00053_.png) | ![](anime_00054_.png) | ![](anime_00055_.png) |
| 10 steps | ![](anime_00018_.png) | ![](anime_00019_.png) | ![](anime_00020_.png) | ![](anime_00021_.png) | ![](anime_00022_.png) | ![](anime_00023_.png) | ![](anime_00024_.png) | ![](anime_00056_.png) | ![](anime_00057_.png) | ![](anime_00058_.png) |
| 15 steps | ![](anime_00025_.png) | ![](anime_00026_.png) | ![](anime_00027_.png) | ![](anime_00028_.png) | ![](anime_00029_.png) | ![](anime_00030_.png) | ![](anime_00031_.png) | ![](anime_00059_.png) | ![](anime_00060_.png) | ![](anime_00061_.png) |
| 20 steps | ![](anime_00032_.png) | ![](anime_00033_.png) | ![](anime_00034_.png) | ![](anime_00035_.png) | ![](anime_00036_.png) | ![](anime_00037_.png) | ![](anime_00038_.png) | ![](anime_00062_.png) | ![](anime_00063_.png) | ![](anime_00064_.png) |
| 30 steps | ![](anime_00039_.png) | ![](anime_00040_.png) | ![](anime_00041_.png) | ![](anime_00042_.png) | ![](anime_00043_.png) | ![](anime_00044_.png) | ![](anime_00045_.png) | ![](anime_00065_.png) | ![](anime_00066_.png) | ![](anime_00067_.png) |
| 40 steps | ![](anime_00046_.png) | ![](anime_00047_.png) | ![](anime_00048_.png) | ![](anime_00049_.png) | ![](anime_00050_.png) | ![](anime_00051_.png) | ![](anime_00052_.png) | ![](anime_00068_.png) | ![](anime_00069_.png) | ![](anime_00070_.png) |

## sampler / scheduler

### Lessons

- `simple` scheduler and `dpm_fast` sampler keep giving the worst results. The time savings is roughly equivalent to `uni` samplers anyway, so it's not even worth it for quick generation
- There is a large variety of cartoonish vs. realism choices here, and many different art styles with some more flat and some more 3D. Most of the combinations have their own uses.
- Most samplers can be recreated in other samplers by varrying cfg and steps. This can be used to optimize generation time of the chosen style to whichever has lower # of steps.
  - e.g. 20 steps / 8 cfg / dpmpp_sde / ddim_uniform generates a similar result to 40 steps / 10 cfg / euler / normal, but in half the amount of time.

### Setup

- 20 steps + 8 cfg

### Results

|              | euler                 | euler_a               | heun                  | dpm_2                 | dpm_fast              | dpmpp_sde             | dpmpp_2m              | ddim                  | uni_pc                | uni_pc_bh2            |
| ------------ | --------------------- | --------------------- | --------------------- | --------------------- | --------------------- | --------------------- | --------------------- | --------------------- | --------------------- | --------------------- |
| simple       | ![](anime_00073_.png) | ![](anime_00074_.png) | ![](anime_00075_.png) | ![](anime_00076_.png) | ![](anime_00077_.png) | ![](anime_00078_.png) | ![](anime_00079_.png) | ![](anime_00080_.png) | ![](anime_00081_.png) | ![](anime_00082_.png) |
| normal       | ![](anime_00083_.png) | ![](anime_00084_.png) | ![](anime_00085_.png) | ![](anime_00086_.png) | ![](anime_00087_.png) | ![](anime_00088_.png) | ![](anime_00089_.png) | ![](anime_00090_.png) | ![](anime_00091_.png) | ![](anime_00092_.png) |
| karras       | ![](anime_00093_.png) | ![](anime_00094_.png) | ![](anime_00095_.png) | ![](anime_00096_.png) | ![](anime_00097_.png) | ![](anime_00098_.png) | ![](anime_00099_.png) | ![](anime_00100_.png) | ![](anime_00101_.png) | ![](anime_00102_.png) |
| ddim_uniform | ![](anime_00103_.png) | ![](anime_00104_.png) | ![](anime_00105_.png) | ![](anime_00106_.png) | ![](anime_00107_.png) | ![](anime_00108_.png) | ![](anime_00109_.png) | ![](anime_00110_.png) | ![](anime_00111_.png) | ![](anime_00112_.png) |

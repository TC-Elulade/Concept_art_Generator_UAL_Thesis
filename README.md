# Concept Art Generator (UAL Thesis)
This repository contains notebooks for using the concept art generator and a tutorial for those who would like to finetune their own stable diffusion model. Stable diffusion is the best method for text-to-image generation, so I chose this method for generative concept art as it allows the user to have more control.

## Important Links
If you would like to use the code to try out the generator on your computer or would like to follow the finetuning tutorial, please take note that a good portion of this project was done with a Huggingface account. Also, look through the following links for resources that could help you:

1. [Huggingface's Diffusers repository](https://github.com/huggingface/diffusers/tree/main/examples/text_to_image) for text-to-image finetuning script
2. [Captioned anime character dataset](https://huggingface.co/datasets/Christabelle/ai_anime_character_inspo)
3. [Finetuned concept art model](https://huggingface.co/Christabelle/sd_anime_concept_generator)

For the sketch skeletons in the finetuning notebook, simply change the number in the URL to get a different base (pick any number in the range of 1-18).

## Set-up Environment
You will need to clone the Diffusers git repository to finetune and use the model; you also need to install torch and accelerate libraries.

```
%pip install torch 
%pip install accelerate
```

```
!git clone https://github.com/huggingface/diffusers
%cd diffusers
%pip install .
%cd /notebooks/diffusers/examples/text_to_image
%pip install -r requirements.txt
```

```
!accelerate config default
```

To use the model to generate the images, just copy the code below and change it to your liking, or you can just follow the notebooks.
```
from huggingface_hub import model_info

# LoRA weights ~3 MB
model_path = "Christabelle/sd_anime_concept_generator"

info = model_info(model_path)
model_base = info.cardData["base_model"]
print(model_base)
```

```
import torch
from diffusers import StableDiffusionPipeline, DPMSolverMultistepScheduler, UniPCMultistepScheduler

pipe = StableDiffusionPipeline.from_pretrained(model_base, torch_dtype=torch.float16)
#pipe.scheduler = DPMSolverMultistepScheduler.from_config(pipe.scheduler.config)
#pipe.scheduler = UniPCMultistepScheduler.from_config(pipe.scheduler.config)
```
You can use either scheduler, UniPCMultistepScheduler is a lot faster as it is the most recent one at the time of writing this file.

```
from PIL import Image

pipe.unet.load_attn_procs(model_path)
pipe.to("cuda")

# Code adapted from:
# https://www.reddit.com/r/StableDiffusion/comments/wxba44/comment/ilqa7an/?utm_source=share&utm_medium=web2x&context=3
pipe.safety_checker = lambda images, **kwargs: (images, [False] * len(images))

prompt = "a man in a hunter outfit, white hair, dark skin, tan skin, red eyes, punk motif"
image = pipe(prompt, negative_prompt="monochrome,low res,poorly drawn face, mutated body parts, deformed body features, bad anatomy, worst quality, low quality",num_inference_steps=250).images[0]
image.save("magician.png")
image.show()
```

## Example Output
Here are some examples of the character designs you can produce with the concept art generator.

![a girl with long purple hair, ponytail, dark skin, star motif, punk motif](https://github.com/TC-Elulade/Concept_art_Generator_UAL_Thesis/blob/main/Sample%20pictures/star%20punk%2C%20dark%20skin.png)
![a man in a hunter outfit, white hair, red eyes, dark skin, punk motif!](https://github.com/TC-Elulade/Concept_art_Generator_UAL_Thesis/blob/main/Sample%20pictures/hunter%20dark%20skin.png)
![a girl with blue hair, long hair, holding sword, flower motif](https://github.com/TC-Elulade/Concept_art_Generator_UAL_Thesis/blob/main/Sample%20pictures/blue-sword-1.png)

The generator is also capable of generating multiple images from one prompt.
![a man or woman in a military uniform, mint hair, blue hair, moon motif](https://github.com/TC-Elulade/Concept_art_Generator_UAL_Thesis/blob/main/Sample%20pictures/moon%20miltary%20officers.png)


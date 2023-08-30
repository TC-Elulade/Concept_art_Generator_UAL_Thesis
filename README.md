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

## Example Output
Here are some examples of the character designs you can produce with the concept art generator.

![image info](images/image.png)
![image info](images/image.png)
![image info](images/image.png)
![image info](images/image.png)

# Making your own LoRA

As an AI art creator, generating charactors with stable patterns is a long desired feature. The outlook of the charator can vary a lot with a tiny change in the prompt, and it requires tedious post productions to "fix" those changes. Now with LoRA, a faster way to fine-tune the model to preserve the pattern is possible, and it gives us more control over the generated charactors.

The goal of this guide is to provide a reproducible way to train a LoRA model. 

Following the Lora from [yatoracat's tutorial](https://note.com/yatoracat/n/n3ecae7e0881f), 

- [Making your own LoRA](#making-your-own-lora)
  - [Prereqiuisites](#prereqiuisites)
  - [Making a Lora with 1 image](#making-a-lora-with-1-image)
    - [Prepare training image](#prepare-training-image)
  - [Making a Lora with multiple images (TBC)](#making-a-lora-with-multiple-images-tbc)
  - [FAQ](#faq)
    - [How can I get the the prompt and parameters from an unedit generated image?](#how-can-i-get-the-the-prompt-and-parameters-from-an-unedit-generated-image)


## Prereqiuisites

1. Basic knowledge to use [Stable Diffusion Web UI](https://github.com/AUTOMATIC1111/stable-diffusion-webui), you have used it to generate images before. It also means you have proper hardware to run software stack.
2. A Google account to use Colab.

## Making a Lora with 1 image

In this part, we will cover the steps to train a LoRA with one image of a red eye tiger, and see how the face look applies to a blue eye wolf.

![image](1-one-image-lora\train\train-output.png)

### Prepare training image

> You can use [the image in the repo](1-one-image-lora\train\train.png) and skip this part. Following are steps to recreate it.

We will use [Crosskemono 2.5 Model](https://civitai.com/models/11888?modelVersionId=47368) to generate the training image, it will also be used to train LoRA and generate final outputs.

- Download the model and put it in `models/StableDiffusion/` folder, and `VAE` in `models/VAE/` folder.
- Copy following contents into the "prompt" input in txt2img tag, click the arrow button under the "Generate" button to parse the it, and then click "Generate" button to generate the image.

    ```txt
    30 yrs old male tiger, front, empty background,
    full head, red eyes, silver ears, forehead patterns, hand paws at body side, sit, 1tail
    collar,
    Negative prompt: (boring_e621:1.0),(EasyNegative:0.8),(deformityv6:0.8),(bad-image-v2:0.8),(worst quality, low quality:1.4), (frame, border, film grain, greyscale:1.0),(text, signature, watermark, username:1.3), (female,girl,3D,realistic,CG,feral,nude,naked,animal:1.1), (bad anatomy:1.1), (bad hands, error, missing fingers), (extra digit, fewer digits),(bad face,bad nose,bad mouth,bad ear,bad eye,bad tail,bad eyebrow,bad eyelash,bad face:1.2),(different eye,different eyelash:1.1)
    Steps: 24, Sampler: DPM++ 2M Karras, CFG scale: 9, Seed: 3429169131, Size: 512x720, Model hash: 167e5bcb37, Model: crosskemonoFurryModel_crosskemono25
    ```

1. Test result

## Making a Lora with multiple images (TBC)

## FAQ

### How can I get the the prompt and parameters from an unedit generated image?

Open "PNG Info" tag in SD web UI, drag the image into the left region, and you will see the prompt and parameters in the right region if the image is generated by SD and has not been edited. Then there are option buttons below to reuse the prompt and parameters.
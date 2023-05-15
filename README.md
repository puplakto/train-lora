
This is an experimental tutorial inpired by **[yatoracat LoRA guide: 1枚の絵から設定資料風画像を作る](https://note.com/yatoracat/n/n3ecae7e0881f)**, and the goal is to provide a reproducible way to train a LoRA model. 

As a hobby AI art creator, generating charactors with stable patterns is a long desired feature of mine. The look of the charator can vary a lot with a tiny change in the prompt, and it requires tedious post productions to "fix" those changes. Now with LoRA, a faster way to fine-tune the model to preserve the pattern is possible, and it gives us more control over the generated charactors.

## Table of Contents

  - [Prereqiuisites](#prereqiuisites)
  - [Making a Lora with 1 image](#making-a-lora-with-1-image)
    - [Prepare training image](#prepare-training-image)
    - [Train LoRA](#train-lora)
    - [Test Result](#test-result)
  - [FAQ](#faq)
    - [How can I get the the prompt and parameters from an unedit generated image?](#how-can-i-get-the-the-prompt-and-parameters-from-an-unedit-generated-image)


## Prereqiuisites

1. Basic knowledge to use [Stable Diffusion Web UI](https://github.com/AUTOMATIC1111/stable-diffusion-webui), experiences of using it to generate images before. It also means the proper hardware to run software stack is ready.
2. A Google account to use Colab.

## Making a Lora with 1 image

In this part, we will cover the steps to train a LoRA with one image of a red eye tiger, and see how the face look applies to a blue eye wolf.

![image](https://github.com/puplakto/train-lora/blob/main/1-one-image-lora/train/train-output.png?raw=true)

### Prepare training image

> To skip this part, use [the image in the repo](1-one-image-lora\train\train.png) as training image. Following are steps to recreate it.

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

![train](https://github.com/puplakto/train-lora/blob/main/1-one-image-lora/train/train.png?raw=true)

### Train LoRA

I tried different approach and services available currently, and found [holostrawberry's training Guide on citiai](https://civitai.com/models/22530) is the most stable and reproducible one. It also saves the hustle to setup the local environment since it uses Google Colab. Following are brief steps to train our one image LoRA.

1. Open [Dataset Maker](https://colab.research.google.com/github/hollowstrawberry/kohya-colab/blob/main/Dataset_Maker.ipynb) to prepare project and tag image in it stores the training data needed in Google Drive. Following are values I used or modified in the Colab notebook. 
   - 1️⃣ Setup: give the project a name, eg. `lakto-lora`. Run the cell to create the project folder, it will ask for permission to access the Google Drive.
   - 2️⃣ Scrape images from Gelbooru: upload the image we generated in previous step in `Loras/lakto-lora/dataset` Google drive folder.
   - 3️⃣ Curate your images: skip this step, it's for finding duplicates and we only have one image.
   - 4️⃣ Tag your images: run the cell with default values, it uses Waifu Diffusion to generte tags. A txt file will be created along side the image.
   - 5️⃣ Curate your tags: add a `global_activation_tag` and it will be used to activate the LoRA in the prompt. I used `lakto_tiger` in this case. Leave the rest unchanged. 
   - The training dataset is now ready.

2. Open [Lora Trainer](https://colab.research.google.com/github/hollowstrawberry/kohya-colab/blob/main/Lora_Trainer.ipynb). There's one huge cell to setup hyper parameters. Change the field mentioned and leave the rest unchanged.
   - ▶️ Setup
     - Use the same `project name` and `folder_structure` as in the [Dataset Maker](https://colab.research.google.com/github/hollowstrawberry/kohya-colab/blob/main/Dataset_Maker.ipynb), it's where the training data is loaded.
     - For `optional_custom_training_model_url`, we use [Crosskemono 2.5 Model](https://civitai.com/models/11888?modelVersionId=47368) as training model as well. Here should be a direct download link to the model, I only following way to work and if anyone and a better idea to get the direct link easier please let me know: Download the model in civitai page and cancel it immediately, paste the direct download link from the browser's downoad managing page.
   - ▶️ Processing
     - Select `flip_aug`: it will flip the image horizontally.
   - Click "Run" button of the cell, training will start and take several minites to finish. The result wil be stored in the project `output` folder, `Loras/lakto-lora/output` in our case. Download `lakto-lora-10` and put it in `models/Lora/` folder.


## FAQ

### How can I get the the prompt and parameters from an unedit generated image?

Open "PNG Info" tag in SD web UI, drag the image into the left region, and you will see the prompt and parameters in the right region if the image is generated by SD and has not been edited. Then there are option buttons below to reuse the prompt and parameters.
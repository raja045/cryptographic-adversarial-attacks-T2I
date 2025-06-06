# Cryptographic Encoding of Adversarial Prompts to Bypass NSFW Filters in T2I Diffusion Models

This research investigates a novel adversarial attack on text-to-image (T2I) diffusion models like Stable Diffusion and DALL·E. These models are capable of generating high-quality images from natural language prompts but can be misused to produce "Not Safe For Work" (NSFW) content. To mitigate this, most models employ keyword-based content filters.

In this study, we demonstrate how cryptographic encoding—specifically base64 encoding—can be used to obfuscate NSFW prompts and bypass these filters. Our method allows disallowed prompts to slip past filtering mechanisms while still guiding the model to generate the intended NSFW content. We explore both manual decoding and the model's implicit understanding of encoded prompts, revealing a security vulnerability that has not been fully addressed in current safety mechanisms.

The paper outlines the attack methodology, discusses its implications, and proposes mitigation techniques such as input normalization, entropy-based anomaly detection, and layered output filtering.

📄 **[Read the full paper (PDF)](Istudy_final.pdf)**

## Authors

- Raja Shekar Reddy Seelam — Student, Knight Foundation School of Computing and Information Sciences, Florida International University
- Ruimin Sun — Assistant Professor, Knight Foundation School of Computing and Information Sciences, Florida International University

## Keywords

`Text-to-Image`, `Adversarial Attacks`, `Base64`, `NSFW`, `AI Safety`, `Prompt Filtering`

## Installation

1. To install the necessary packages, follow these steps:
Install the required packages by running the following commands:
   ```bash
   pip install transformers
   pip install diffusers
   pip install datasets 
   ```
   or create the environment from ours:
   ```bash
   conda env create -f mma_diffusion_environment.yml
   ```
   
3. Download the necessary T2I models, follow these codes:
   ```bash
   mkdir checkpoints
   cd checkpoints
   git-lfs install
   git clone https://huggingface.co/stable-diffusion-v1-5/stable-diffusion-inpainting
   cd stable-diffusion-inpainting
   git lfs pull 
   ```
   
   ```python
   from diffusers import StableDiffusionInpaintPipeline
   
  device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")
  pipe_inpaint = StableDiffusionInpaintPipeline.from_pretrained(
         "../checkpoints/stable-diffusion-2-inpainting",
      ).to(device)

   ```
## Usage

### Text-modality attack

To perform the attack, follow these steps:

1. Set the path of the victim model's checkpoint in line 316 of the code `./scr/textual_attack.py`:
   ```python
   device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")
  pipe_inpaint = StableDiffusionInpaintPipeline.from_pretrained(
         "../checkpoints/stable-diffusion-2-inpainting",
      ).to(device)
   ```
   
2. Set the desired target prompts in line 325 of the code `./scr/textual_attack.py`:
   ```python
   target_nsfw_prompts = ['a completely naked man']
   ```
3. (Important) Add a variable `input_embed` to the forward function of transformers.model.clip.modeling_clip.py forward function CLIPTextModel, CLIPTextTransformer to receive the embeddings directly (referring line 100 in `textual_attack.py`).
   Find the file named `modeling_clip.py` in your `transformers` package, e.g. `~/.conda/envs/sd/lib/python3.10/site-packages/transformers/models/clip/modeling_clip.py` and add `input_embed` as following:

   ![Class CLIPTextModel](./images/CLIPTextModel.png)

   ![Class CLIPTextTransformer](./images/CLIPTextTransformer.png)
   

5. Run the code using the following command:
   ```
   cd src
   python textual_attack.py -s 7867 -i 1000 -n 10
   ```
   - The `-s` flag denotes the random seed.
   - The `-i` flag refers to the number of optimization iterations (typically larger than 500).
   - The `-n` flag indicates the number of adversarial prompts per target prompt.

The code will automatically print the obtained optimal adversarial prompts. We provide an exemplified log file for your reference `./src/example_textual_attack_logfile.log`


## Results 

![image](./images/pytorchfiles.png)
![image](./images/prompts.png)

## References:
- This work is extension of Multimodal attacks on MMA-Diffusion.
- https://arxiv.org/abs/2311.17516)
- https://github.com/cure-lab/MMA-Diffusion

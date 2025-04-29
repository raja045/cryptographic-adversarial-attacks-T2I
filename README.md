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

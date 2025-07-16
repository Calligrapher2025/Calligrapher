# Calligrapher: Freestyle Text Image Customization

> **Calligrapher: Freestyle Text Image Customization**

<div align=center>
<img src="./docs/static/images/teaser.jpg" width=850px>
</div>

**Figure:** Photorealistic text image customization results produced by our proposed Calligrapher, which allows users to perform customization with diverse stylized images and text prompts.

<div align=center>
<img src="./docs/static/images/multilingual_samples.png" width=900px>
</div>

**Figure:** Multilingual freestyle text customization results are shown in the below figure, where tested languages and text are: Chinese (你好朋友/夏天来了), Korean (서예가), and Japanese (ナルト).

<div align=center>

## 🔗 **Links & Resources**

**[**[**📄 Project Page**](https://calligrapher2025.github.io/Calligrapher/)**]**
**[**[**🎥 Video**](https://youtu.be/FLSPphkylQE)**]**
**[**[**📦 Model & Data**](https://huggingface.co/Calligrapher2025/Calligrapher)**]**
**[**[**🤗 Hugging Face Demo**](https://huggingface.co/spaces/SahilCarterr/Calligrapher)**]**

</div>

## Summary

We introduce Calligrapher, a novel diffusion-based framework that innovatively integrates advanced text customization with artistic typography for digital calligraphy and design applications. Addressing the challenges of precise style control and data dependency in typographic customization, our framework supports text customization under various settings including self-reference, cross-reference, and non-text reference customization. By automating high-quality, visually consistent typography, Calligrapher empowers creative practitioners in digital art, branding, and contextual typographic design.

<div align=center>
<img src="./docs/static/images/framework.jpg" width=900px>
</div>

**Figure:** Training framework of Calligrapher, demonstrating the integration of localized style injection and diffusion-based learning.

## Environment Setup

We provide two ways to set up the environment:

### Using pip

Requires Python 3.10 + PyTorch 2.5.0 + CUDA. Install the required dependencies using:
```bash
pip install -r requirements.txt
```

### Using Conda

```bash
conda env create -f env.yml
conda activate calligrapher
```

## Pretrained Models & Data (Benchmark)

Before running the demos, please download the required pretrained models and test data.

Download the models and testing bench using huggingface_hub:
```python
from huggingface_hub import snapshot_download

# Download the base model FLUX.1-Fill-dev (granted access needed)
snapshot_download("black-forest-labs/FLUX.1-Fill-dev", token="your_token")

# Download SigLIP image encoder (this model can also be automatically downloaded when running the code)
snapshot_download("google/siglip-so400m-patch14-384")

# Download Calligrapher model and test data
snapshot_download("Calligrapher2025/Calligrapher")
```

Or manually download from:
[FLUX.1-Fill-dev](https://huggingface.co/black-forest-labs/FLUX.1-Fill-dev),
[SigLIP](https://huggingface.co/google/siglip-so400m-patch14-384),
and [Calligrapher](https://huggingface.co/Calligrapher2025/Calligrapher).

The Calligrapher repository hosted on Huggingface contains:
- `calligrapher.bin`: Model weights.
- `Calligrapher_bench_testing.zip`: Test dataset with examples for both self-reference customization and cross-reference customization scenarios. Additional reference images could also be found in it.

## Model Usage

### 1.Path Configuration

Before running the models, you need to configure the paths in `path_dict.json`:

- `data_dir`: Path to store the test dataset.
- `cli_save_dir`: Path to save results from command-line interface experiments.
- `gradio_save_dir`: Path to save results from Gradio interface experiments.
- `gradio_temp_dir`: Path to save temporary files.
- `base_model_path`: Path to the base model FLUX.1-Fill-dev.
- `image_encoder_path`: Path to the SigLIP image encoder model.
- `calligrapher_path`: Path to the Calligrapher model weights.

### 2. Gradio Demo Interface (Recommended)

We provide two Gradio demo interfaces:

1. Basic version:
```bash
python gradio_demo.py
```
When using this demo, in addition to uploading source and reference images, users also need to use the Draw button (brush control) in the Image Editing Panel to manually draw the mask.

2. Version supporting uploading custom inpainting masks:
```bash
python gradio_demo_upload_mask.py
```

This version includes pre-configured examples (e.g., at the bottom of the page) and is recommended for users to first understand how to use the model.

Below is a preview of the two aforementioned Gradio demo interfaces:

<div align=center>
<img src="./docs/static/images/gradio_preview.png" width=900px>
</div>



3. Version supporting multilingual text customization such as Chinese which supported by [TextFLUX](https://github.com/yyyyyxie/textflux). To use this gradio demo, first download [TextFLUX weights](https://huggingface.co/yyyyyxie/textflux-lora/blob/main/pytorch_lora_weights.safetensors) and configure the "textflux_path" entry in "path_dict.json". Then download [the font resource](https://github.com/yyyyyxie/textflux/blob/main/resource/font/Arial-Unicode-Regular.ttf) to "./resources/" and run:
```bash
python gradio_demo_multilingual.py
```



**✨User Tips:**
1. **Quality of multilingual generation.** The implementation strategy combines Calligrapher with the fine-tuned base model (textflux) without additional fine-tuning, please temper expectations regarding output quality.

2. **Speed vs Quality Trade-off.** Use fewer steps (e.g., 10-step which takes ~4s/image on a single A6000 GPU) for faster generation, but quality may be lower.

3. **Inpaint Position Freedom.** Inpainting positions are flexible - they don't necessarily need to match the original text locations in the input image.

4. **Iterative Editing.** Drag outputs from the gallery to the Image Editing Panel (clean the Editing Panel first) for quick refinements.

5. **Mask Optimization.** Adjust mask size/aspect ratio to match your desired content. The model tends to fill the masks, and harmonizes the generation with background in terms of color and lighting.

6. **Reference Image Tip.** White-background references improve style consistency - the encoder also considers background context of the given reference image.

7. **Resolution Balance.** Very high-resolution generation sometimes triggers spelling errors. 512/768px are recommended considering the model is trained under the resolution of 512.

### 3. Batch Testing (CLI)

We provide two python scripts for two text image customization modes:

1. Self-reference Customization:
```bash
python infer_calligrapher_self_custom.py
```

2. Cross-reference Customization:
```bash
python infer_calligrapher_cross_custom.py
```

## Additional Results


<div align=center>
<img src="./docs/static/images/application.jpg" width=900px>
</div>

**Figure:** Qualitative results of Calligrapher under various settings. We demonstrate text customization results respectively under settings of (a) self-reference, (b) cross-reference, and (c) non-text reference. Reference-based image generation results are also incorporated in (d).

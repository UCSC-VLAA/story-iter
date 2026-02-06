<p align="center">
  <img src="./docs/logo.png" height=150>
</p>



# Story-Iter: A Training-free Iterative Paradigm for Long Story Visualization
<span>
<a href="https://arxiv.org/abs/2410.06244"><img src="https://img.shields.io/badge/arXiv-2410.06244-b31b1b.svg" height=22.5></a>
<a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" height=22.5></a>  
<a href="https://jwmao1.github.io/storyadapter/"><img src="https://img.shields.io/badge/project-StoryAdapter-purple.svg" height=22.5></a>
<a href="https://colab.research.google.com/drive/1sFbw0XlCQ6DBRU3s2n_F2swtNmHoicM-?usp=sharing"><img src="https://colab.research.google.com/assets/colab-badge.svg" height=22.5></a>
</span>

Code for the paper [Story-Iter: A Training-free Iterative Paradigm for Long Story Visualization](https://arxiv.org/abs/2410.06244)

Note: This code base is still not complete. 

### About this repo:

The repository contains the official implementation of "Story-Iter".

## Introduction 🦖

> Story visualization, the task of generating coherent images based on a narrative, has seen significant advancements with the emergence of text-to-image models, particularly diffusion models. However, maintaining semantic consistency, generating high-quality fine-grained interactions, and ensuring computational feasibility remain challenging, especially in long story visualization (_i.e._, up to 100 frames). In this work, we introduce **Story-Iter**, a new training-free iterative paradigm to enhance long-story generation. Unlike existing methods that rely on fixed reference images to construct a complete story, our approach features a novel external \textbf{iterative paradigm}, extending beyond the internal iterative denoising steps of diffusion models, to continuously refine each generated image by incorporating all reference images from the previous round. To achieve this, we propose a plug-and-play, training-free **g**lobal **r**eference **c**ross-**a**ttention (**GRCA**) module, modeling all reference frames with global embeddings, ensuring semantic consistency in long sequences. By progressively incorporating holistic visual context and text constraints, our iterative paradigm enables precise generation with fine-grained interactions, optimizing the story visualization step-by-step. Extensive experiments in the official story visualization dataset and our long story benchmark demonstrate that Story-Iter's state-of-the-art performance in long-story visualization (up to 100 frames) excels in both semantic consistency and fine-grained interactions.

<br>

<img src="./docs/teaser-github.jpg" width="800"/>


## News 🚀
* **2024.10.10**: [Paper](https://arxiv.org/abs/2410.06244) is released on ArXiv.
* **2024.10.04**: Code released.
* **2026.01.27**: Fast version released, visualizing a 100-frame story over 10 iterations takes only 20 minutes.
* **2026.01.27**: ControlNet version released, supporting openPose skeletons as control signals.

## Framework 🤖 

> Story-Iter framework. Illustration of the proposed iterative paradigm, which consists of initialization, iterations in Story-Iter, and implementation of Global Reference Cross-Attention (GRCA).
Story-Iter first visualizes each image only based on the text prompt of the story and uses all results as reference images for the future round. 
In the iterative paradigm, Story-Iter inserts GRCA into SD. For the ith iteration of each image visualization, GRCA will aggregate the information flow of all reference images during the denoising process through cross-attention.
All results from this iteration will be used as a reference image to guide the dynamic update of the story visualization in the next iteration.

<br>

<img src="./docs/framework.jpg" width="1080"/>


## Quick Start 🔧

### Installation
The project is built with Python 3.10.14, PyTorch 2.2.2. CUDA 12.1, cuDNN 8.9.02
For installing, follow these instructions:
~~~
# git clone this repository
git clone https://github.com/UCSC-VLAA/Story-Iter.git
cd Story-Iter

# create new anaconda env
conda create -n StoryAdapter python=3.10
conda activate StoryAdapter 

# install packages
pip install -r requirements.txt
~~~

### Download the checkpoint
- downloading [RealVisXL_V4.0](https://huggingface.co/SG161222/RealVisXL_V4.0/tree/main) put it into "./RealVisXL_V4.0"
- downloading [clip_image_encoder](https://huggingface.co/h94/IP-Adapter/tree/main/sdxl_models/image_encoder) put it into "./IP-Adapter/sdxl_models/image_encoder"
- downloading [ip-adapter_sdxl](https://huggingface.co/h94/IP-Adapter/resolve/main/sdxl_models/ip-adapter_sdxl.bin?download=true) put it into "./IP-Adapter/sdxl_models/ip-adapter_sdxl.bin"

### Running Demo

~~~
python run.py --base_model_path your_path/RealVisXL_V4.0 --image_encoder_path your_path/IP-Adapter/sdxl_models/image_encoder --ip_ckpt your_path/IP-Adapter/sdxl_models/ip-adapter_sdxl.bin 
~~~

### Customized Running

~~~
python run.py --base_model_path your_path/RealVisXL_V4.0 --image_encoder_path your_path/IP-Adapter/sdxl_models/image_encoder --ip_ckpt your_path/IP-Adapter/sdxl_models/ip-adapter_sdxl.bin 
--story "your prompt1" "your prompt2" "your prompt3" ... "your promptN"
~~~
Note: Regarding custom stories, we suggest the template [Character Definition + Interaction Definition + Scene Definition] for better story visualization performance. For example, the Character Definition is "One man wearing yellow robe," the Interaction Definition is "dancing," and the Scene Definition is "the palace hall." So, the input prompt is "One man wearing yellow robe dancing in the palace hall."

## Performance 🎨

### Regular-length Story Visualization 
- downloading the [StorySalon](https://huggingface.co/datasets/haoningwu/StorySalon/resolve/main/testset.zip?download=true) test set."

| GIF1 | GIF2  | GIF3  |
| --- | --- | --- |
| <img src="./docs/our_005169.gif" alt="GIF 1" width="224"/>  | <img src="./docs/our_007016.gif" alt="GIF 2" width="224"/> | <img src="./docs/our_007137.gif" alt="GIF 3" width="224"/>  |

| GIF4 | GIF5  | GIF6  |
| --- | --- | --- |
| <img src="./docs/our_013804.gif" alt="GIF 4" width="224"/>  | <img src="./docs/our_015770.gif" alt="GIF 5" width="224"/> | <img src="./docs/our_000026.gif" alt="GIF 6" width="224"/>  |

| GIF7 | GIF8  | GIF9  |
| --- | --- | --- |
| <img src="./docs/our_012060.gif" alt="GIF 7" width="224"/>  | <img src="./docs/our_008614.gif" alt="GIF 8" width="224"/> | <img src="./docs/our_008710.gif" alt="GIF 9" width="224"/>  |


### Long Story Visualization 

<br>

<img src="./docs/comic1.png" width="1080"/>

<br>
<img src="./docs/comic7.png" width="1080"/>

<br>
<img src="./docs/comic3.png" width="1080"/>

### Running with Different Style
comic style:
~~~
python run.py --base_model_path your_path/RealVisXL_V4.0 --image_encoder_path your_path/IP-Adapter/sdxl_models/image_encoder --ip_ckpt your_path/IP-Adapter/sdxl_models/ip-adapter_sdxl.bin --style comic
~~~

<img src="./docs/style_comic.png" width="1080"/>

<be>

film style:
~~~
python run.py --base_model_path your_path/RealVisXL_V4.0 --image_encoder_path your_path/IP-Adapter/sdxl_models/image_encoder --ip_ckpt your_path/IP-Adapter/sdxl_models/ip-adapter_sdxl.bin --style film
~~~
<img src="./docs/style_film.png" width="1080"/>

<be>

realistic style:
~~~
python run.py --base_model_path your_path/RealVisXL_V4.0 --image_encoder_path your_path/IP-Adapter/sdxl_models/image_encoder --ip_ckpt your_path/IP-Adapter/sdxl_models/ip-adapter_sdxl.bin --style realistic
~~~

<img src="./docs/style_realistic.png" width="1080"/>

### Fast running with LCM
~~~
python run_fast.py --base_model_path your_path/RealVisXL_V4.0 --image_encoder_path your_path/IP-Adapter/sdxl_models/image_encoder --ip_ckpt your_path/IP-Adapter/sdxl_models/ip-adapter_sdxl.bin 
~~~

<img src="./docs/story_fast.jpg" width="1080"/>

### Running with ControlNet
~~~
python run_controlnet.py --base_model_path your_path/RealVisXL_V4.0 --image_encoder_path your_path/IP-Adapter/sdxl_models/image_encoder --ip_ckpt your_path/IP-Adapter/sdxl_models/ip-adapter_sdxl.bin --openpose_path your_path/openpose_root
~~~

<img src="./docs/controlnet.jpg" width="1080"/>

## Acknowledgement 🍻

Deeply appreciate these wonderful open source projects: [stablediffusion](https://github.com/Stability-AI/StableDiffusion), [clip](https://github.com/openai/CLIP), [ip-adapter](https://github.com/tencent-ailab/IP-Adapter), [storygen](https://github.com/haoningwu3639/StoryGen), [storydiffusion](https://github.com/HVision-NKU/StoryDiffusion), [theatergen](https://github.com/donahowe/TheaterGen), [timm](https://github.com/huggingface/pytorch-image-models).

## Citation 🔖

If you find this repository useful, please consider giving a star ⭐ and citation 🙈:

```
@misc{mao2024story_adapter,
  title={{Story-Adapter: A Training-free Iterative Framework for Long Story Visualization}},
  author={Mao, Jiawei and Huang, Xiaoke and Xie, Yunfei and Chang, Yuanqi and Hui, Mude and Xu, Bingjie and Zhou, Yuyin},
  journal={arXiv},
  volume={abs/2410.06244},
  year={2024},
}
```






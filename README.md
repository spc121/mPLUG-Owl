# mPLUG-Owl🦉: Modularization Empowers Large Language Models with Multimodality
Qinghao Ye*, Haiyang Xu*, Guohai Xu*, Jiabo Ye, Ming Yan†, Yiyang Zhou, Junyang Wang, Anwen Hu, Pengcheng Shi, Yaya Shi, Chenliang Li, Yuanhong Xu, Hehong Chen, Junfeng Tian, Qian Qi, Ji Zhang, Fei Huang

**DAMO Academy, Alibaba Group**

*Equal Contribution; † Corresponding Author

[![](assets/Demo-ModelScope-brightgreen.svg)](https://modelscope.cn/studios/damo/mPLUG-Owl/summary)
[![](assets/LICENSE-Apache%20License-blue.svg)](https://github.com/X-PLUG/mPLUG-Owl/blob/main/LICENSE)
[![](assets/Paper-PDF-orange.svg)](http://mm-chatgpt.oss-cn-zhangjiakou.aliyuncs.com/mplug_owl_demo/released_checkpoint/mPLUG_Owl_paper.pdf)
[![](assets/Paper-Arxiv-orange.svg)](https://arxiv.org/abs/2304.14178)

English | [简体中文](README_zh.md)
<hr>

<img src="http://mm-chatgpt.oss-cn-zhangjiakou.aliyuncs.com/mplug_owl_demo/released_checkpoint/sample.gif"  width="60%">


## Examples
![Training paradigm and model overview](assets/case_1.png "Training paradigm and model overview")
![Training paradigm and model overview](assets/case_2.png "Training paradigm and model overview")

## News

* We provide an [online demo](https://modelscope.cn/studios/damo/mPLUG-Owl/summary) on modelscope for the public to experience.
* We released code of mPLUG-Owl🦉 with its pre-trained and instruction tuning checkpoints.

## Spotlights
* A new training paradigm with a modularized design for large multi-modal language models.
* Learns visual knowledge while support multi-turn conversation consisting of different modalities.
* Observed abilities such as multi-image correlation and scene text understanding, vision-based document comprehension.
* Release a visually-related instruction evaluation set **OwlEval**.


![Training paradigm and model overview](assets/model.png "Training paradigm and model overview")

## Online Demo
[Demo of mPLUG-Owl on Modelscope](https://www.modelscope.cn/studios/damo/mPLUG-Owl/summary)

![](assets/modelscope.png)

## Checkpoints
|Model|Phase|Download link|
|-|-|-|
|mPLUG-Owl 7B|Pre-training|[Download link](http://mm-chatgpt.oss-cn-zhangjiakou.aliyuncs.com/mplug_owl_demo/released_checkpoint/pretrained.pth)|
|mPLUG-Owl 7B|Instruction tuning|[Download link](http://mm-chatgpt.oss-cn-zhangjiakou.aliyuncs.com/mplug_owl_demo/released_checkpoint/pretrained.pth)|
|Tokenizer model|N/A|[Download link](http://mm-chatgpt.oss-cn-zhangjiakou.aliyuncs.com/mplug_owl_demo/released_checkpoint/tokenizer.model)|
## Usage
### Install Requirements
Core library dependency:
* PyTorch=1.12.1
* transformers=4.28.1
* [Apex](https://github.com/NVIDIA/apex)
* einops
* icecream
* flask
* ruamel.yaml
* uvicorn 
* fastapi
* markdown2
* gradio

You can also refer to the exported Conda environment configuration file ```env.yaml``` to prepare your environments.
### Local Demo
We provide a script to deploy a simple demo in your local machine.
```Bash
python -m server_mplug.owl_demo --debug --port 6363 --checkpoint_path 'your checkpoint path' --tokenizer_path 'your tokenizer path'
```
### Inference
Build model, toknizer and processor.
```Python
from interface import get_model
model, tokenizer, img_processor = get_model(
        checkpoint_path='checkpoint path', tokenizer_path='tokenizer path')
```
Prepare model inputs.
```Python
# We use a human/AI template to organize the context as a multi-turn conversation.
# <image> denotes an image placehold.
prompts = [
'''The following is a conversation between a curious human and AI assistant. The assistant gives helpful, detailed, and polite answers to the user's questions.
Human: <image>
Human: Explain why this meme is funny.
AI: ''']

# The image paths should be placed in the image_list and kept in the same order as in the prompts.
# We support urls, local file paths and base64 string. You can custom the pre-process of images by modifying the mplug_owl.modeling_mplug_owl.ImageProcessor
image_list = ['https://xxx.com/image.jpg',]
```
Get response.
```Python
# generate kwargs (the same in transformers) can be passed in the do_generate()
from interface import do_generate
sentence = do_generate(prompts, image_list, model, tokenizer,
                               img_processor, max_length=512, top_k=5, do_sample=True)
```
## Performance Comparison
The comparison results of 50 single-turn responses (left) and 52 multi-turn responses (right) between mPLUG-Owl and baselines with manual evaluation metrics. A/B/C/D denote the rate of each response.
![Comparison Results](assets/mPLUG_Owl_compare_result_s&mturn.png)

## Coming Soon

- [ ] Instruction tuning code.
- [ ] Multi-lingustic support (e.g., Chinese, Japanese, Germen, French, etc.)
- [ ] A visually-related evaluation set **OwlEval** to comprehensively evaluate various models.

## Releated Projects

* [LLaMA](https://github.com/facebookresearch/llama). A open-source collection of state-of-the-art large pre-trained language models.
* [Baize](https://github.com/project-baize/baize-chatbot). An open-source chat model trained with LoRA on 100k dialogs generated by letting ChatGPT chat with itself.
* [Alpaca](https://github.com/tatsu-lab/stanford_alpaca). A fine-tuned model trained from a 7B LLaMA model on 52K instruction-following data.
* [LoRA](https://github.com/microsoft/LoRA). A plug-and-play module that can greatly reduce the number of trainable parameters for downstream tasks.
* [LLaVA](https://github.com/haotian-liu/LLaVA). A visual instruction tuned vision language model which achieves GPT4 level capabilities.
* [mPLUG](https://github.com/alibaba/AliceMind/tree/main/mPLUG). A vision-language foundation model for both cross-modal understanding and generation.
* [mPLUG-2](https://github.com/alibaba/AliceMind). A multimodal model with a modular design, which inspired our project.

## Citation
If you found this work useful, consider giving this repository a star and citing our paper as followed:
```
@article{ye2023mplugowl,
  title={mPLUG-Owl: Modularization Empowers Large Language Models with Multimodality},
  author={Qinghao Ye, Haiyang Xu, Guohai Xu, Jiabo Ye, Ming Yan†, Yiyang Zhou, Junyang Wang, Anwen Hu, Pengcheng Shi, Yaya Shi, Chenliang Li, Yuanhong Xu, Hehong Chen, Junfeng Tian, Qian Qi, Ji Zhang, Fei Huang},
  year={2023}
}
```
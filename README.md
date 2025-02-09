# GPTQLoRA: Efficient Finetuning of Quantized LLMs with GPTQ

[QLoRA](https://arxiv.org/abs/2305.14314) with [AutoGPTQ](https://github.com/PanQiWei/AutoGPTQ) for quantization

## License and Intended Use
I release the resources associated with GPTQLoRA finetuning in this repository under MIT license.

## Installation
```bash
conda create -n gptqlora python=3.8
conda activate gptqlora
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu117
git clone -b peft_integration https://github.com/PanQiWei/AutoGPTQ.git && cd AutoGPTQ
pip install .[triton]
cd ..
git clone https://github.com/timdettmers/bitsandbytes.git
cd bitsandbytes
# CUDA_VERSIONS in {110, 111, 112, 113, 114, 115, 116, 117, 118, 119, 120, 120}
# make argument in {cuda110, cuda11x, cuda12x}
# if you do not know what CUDA you have, try looking at the output of: python -m bitsandbytes
CUDA_VERSION=117 make cuda11x
python setup.py install
cd ..
pip install git+https://github.com/huggingface/transformers.git
pip install git+https://github.com/huggingface/peft.git
pip install git+https://github.com/huggingface/accelerate.git
pip install -r requirements.txt
pip install protobuf==3.20.*
```

## Getting Started
The `gptqlora.py` code is a starting point for finetuning and inference on various datasets.
Basic command for finetuning a baseline model on the Alpaca dataset:
```bash
python gptqlora.py --model_path <path>
```

For models larger than 13B, we recommend adjusting the learning rate:
```bash
python gptqlora.py –learning_rate 0.0001 --model_path <path>
```

The file structure of the model checkpoint is as follows:
```
(bnb) root@/root/qlora-main# ls llama-7b/
config.json             gptq_model-4bit-128g.bin  special_tokens_map.json  tokenizer_config.json
generation_config.json  quantize_config.json      tokenizer.model
```

## Quantization
Quantization is based on AutoGPTQ. Also, to run the code, you first need a model converted to GPTQ.

## Paged Optimizer
You can access the paged optimizer with the argument `--optim paged_adamw_32bit`

## Acknoledgements
This code is based on [QLoRA](https://github.com/artidoro/qlora).

This repo builds on the [Stanford Alpaca](https://github.com/tatsu-lab/stanford_alpaca) and [LMSYS FastChat](https://github.com/lm-sys/FastChat) repos.

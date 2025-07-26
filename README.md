<div align="center">

<h1>Retrieval-based-Voice-Conversion-WebUI</h1>
A simple and easy-to-use voice changer framework based on VITS<br><br>

[![madewithlove](https://img.shields.io/badge/made_with-%E2%9D%A4-red?style=for-the-badge&labelColor=orange
)](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI)

<img src="https://counter.seku.su/cmoe?name=rvc&theme=r34" /><br>

[![Open In Colab](https://img.shields.io/badge/Colab-F9AB00?style=for-the-badge&logo=googlecolab&color=525252)](https://colab.research.google.com/github/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/blob/main/Retrieval_based_Voice_Conversion_WebUI.ipynb)
[![Licence](https://img.shields.io/badge/LICENSE-MIT-green.svg?style=for-the-badge)](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/blob/main/LICENSE)
[![Huggingface](https://img.shields.io/badge/🤗%20-Spaces-yellow.svg?style=for-the-badge)](https://huggingface.co/lj1995/VoiceConversionWebUI/tree/main/)

[![Discord](https://img.shields.io/badge/RVC%20Developers-Discord-7289DA?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/HcsmBBGyVk)

[**Changelog**](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/blob/main/docs/Changelog_CN.md) | [**FAQ**](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/wiki/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98%E8%A7%A3%E7%AD%94) | [**AutoDL·5 cents training AI singer**](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/wiki/Autodl%E8%AE%AD%E7%BB%83RVC%C2%B7AI%E6%AD%8C%E6%89%8B%E6%95%99%E7%A8%8B) | [**Control Experiment Record**](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/wiki/Autodl%E8%AE%AD%E7%BB%83RVC%C2%B7AI%E6%AD%8C%E6%89%8B%E6%95%99%E7%A8%8 B](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/wiki/%E5%AF%B9%E7%85%A7%E5%AE%9E%E9%AA%8C%C2%B7%E5%AE%9E%E9%AA%8C%E8%AE%B0%E5%BD%95)) | [**Online Demo**](https://modelscope.cn/studios/FlowerCry/RVCv2demo)

[**English**](./docs/en/README.en.md) | [**Chinese Simplified**](./README.md) | [**Japanese**](./docs/jp/README.ja.md) | [**한국어**](./docs/kr/README.ko.md) ([**Korean**](./docs/kr/README.ko.han.md)) | [**Français**](./docs/fr/README.fr.md) | [**Türkçe**](./docs/tr/README.tr.md) | [**Português**](./docs/pt/README.pt.md)

</div>

> The base model is trained with nearly 50 hours of open source high-quality VCTK training set, so there is no copyright concern. Please feel free to use it.

> Please look forward to the base model of RVCv3, which has larger parameters, larger data, better results, and basically the same inference speed, and requires less training data.

<table>
<tr>
<td align="center">Training and reasoning interface</td>
<td align="center">Real-time voice changing interface</td>
</tr>
<tr>
<td align="center"><img src="https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/assets/129054828/092e5c12-0d49-4168-a590-0b0ef6a4f630"></td>
<td align="center"><img src="https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/assets/129054828/730b4114-8805-44a1-ab1a-04668f3c30a6"></td>
</tr>
<tr>
<td align="center">go-web.bat</td>
<td align="center">go-realtime-gui.bat</td>
</tr>
<tr>
<td align="center">You can freely choose the operation you want to perform. </td>
<td align="center">We have achieved end-to-end 170ms latency. If ASIO input and output devices are used, end-to-end 90ms latency can be achieved, but it is very dependent on hardware driver support. </td>
</tr>
</table>

## Introduction
This repository has the following features
+ Use top1 search to replace input source features with training set features to eliminate timbre leakage
+ Fast training even on relatively poor graphics cards
+ Good results can be obtained even with a small amount of data for training (it is recommended to collect at least 10 minutes of low-noise speech data)
+ The timbre can be changed by model fusion (with the help of ckpt-merge in the ckpt processing tab)
+ Simple and easy-to-use web interface
+ The UVR5 model can be called to quickly separate human voice and accompaniment
+ Use the most advanced [human voice pitch extraction algorithm InterSpeech2023-RMVPE](#reference project) to eliminate the mute problem. Best results (significantly) but faster and less resource-intensive than crepe_full
+ A-card and I-card acceleration support

Click here to view our [demo video](https://www.bilibili.com/video/BV1pm4y1z7Gm/) !

## Environment configuration
The following instructions need to be executed in an environment with Python version greater than 3.8.

### General methods for platforms such as Windows/Linux/MacOS
Choose one of the following methods.
#### 1. Install dependencies via pip
1. Install Pytorch and its core dependencies, skip if already installed. Reference: https://pytorch.org/get-started/locally/
```bash
pip install torch torchvision torchaudio
```
2. If it is a win system + Nvidia Ampere architecture (RTX30xx), according to the experience of #21, you need to specify the cuda version corresponding to pytorch
```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu117
```
3. Install the corresponding dependencies according to your graphics card
- N card
```bash
pip install -r requirements.txt
```
- A card/I card
```bash
pip install -r requirements-dml.txt
```
- A card ROCM(Linux)
```bash
pip install -r requirements-amd.txt
```
- I card IPEX(Linux)
```bash
pip install -r requirements-ipex.txt
```

#### 2. Install dependencies through poetry
Install the Poetry dependency management tool. Skip if already installed. Reference: https://python-poetry.org/docs/#installation
```bash
curl -sSL https://install.python-poetry.org | python3 -
```

When installing dependencies through Poetry, it is recommended to use Python 3.7-3.10. Other versions will conflict when installing llvmlite==0.39.0
```bash
poetry init -n
poetry env use "path to your python.exe"
poetry run pip install -r requirments.txt
```

### MacOS
You can install dependencies through `run.sh`
```bash
sh ./run.sh
```

## Other pre-model preparations
RVC requires some other pre-models for inference and training.

You can download these models from our [Hugging Face space](https://huggingface.co/lj1995/VoiceConversionWebUI/tree/main/).

### 1. Download assets
Below is a list of all the pre-models and other files required by RVC. You can find the script to download them in the `tools` folder.

- ./assets/hubert/hubert_base.pt

- ./assets/pretrained

- ./assets/uvr5_weights

If you want to use the v2 version of the model, you need to download it separately

- ./assets/pretrained_v2

### 2. Install ffmpeg
Skip if ffmpeg and ffprobe are already installed.

#### Ubuntu/Debian users
```bash
sudo apt install ffmpeg
```
#### MacOS users
```bash
brew install ffmpeg
```
#### Windows users
Place the downloaded file in the root directory.
- Download [ffmpeg.exe](https://huggingface.co/lj1995/VoiceConversionWebUI/blob/main/ffmpeg.exe)

- Download [ffprobe.exe](https://huggingface.co/lj1995/VoiceConversionWebUI/blob/main/ffprobe.exe)

### 3. Download the required files for the rmvpe human voice pitch extraction algorithm

If you want to use the latest RMVPE human voice pitch extraction algorithm, you need to download the pitch extraction model parameters and place them in the RVC root directory.

- Download [rmvpe.pt](https://huggingface.co/lj1995/VoiceConversionWebUI/blob/main/rmvpe.pt)

#### Download rmvpe's dml environment (optional, A card/I card users)

- Download [rmvpe.onnx](https://huggingface.co/lj1995/VoiceConversionWebUI/blob/main/rmvpe.onnx)

### 4. AMD graphics card Rocm (optional, Linux only)

If you want to run RVC based on AMD's Rocm technology on a Linux system, please first install the required drivers [here](https://rocm.docs.amd.com/en/latest/deploy/linux/os-native/install.html).

If you are using Arch Linux, you can use pacman to install the required drivers:
````
pacman -S rocm-hip-sdk rocm-opencl-sdk
````
For some graphics cards, you may need to configure the following environment variables (such as: RX6700XT):
````
export ROCM_PATH=/opt/rocm
export HSA_OVERRIDE_GFX_VERSION=10.3.0
````
Also make sure your current user is in the `render` and `video` user groups:
````
sudo usermod -aG render $USERNAME
sudo usermod -aG video $USERNAME
````

## Getting Started
### Direct Start
Use the following command to start the WebUI
```bash
python infer-web.py
```
If you previously used Poetry to install dependencies, you can start the WebUI in the following way
```bash
poetry run python infer-web.py
```

### Use the integration package
Download and unzip `RVC-beta.7z`
#### Windows users
Double-click `go-web.bat`
#### MacOS users
```bash
sh ./run.sh
```
### For I card users who need to use IPEX technology (Linux only)
```bash
source /opt/intel/oneapi/setvars.sh
```

## Reference projects
+ [ContentVec](https://github.com/auspicious3000/contentvec/)
+ [VITS](https://github.com/jaywalnut310/vits)
+ [HIFIGAN](https://github.com/jik876/hifi-gan)
+ [Gradio](https://github.com/gradio-app/gradio)
+ [FFmpeg](https://github.com/FFmpeg/FFmpeg)
+ [Ultimate Vocal Remover](https://github.com/Anjok07/ultimatevocalremovergui)
+ [audio-slicer](https://github.com/openvpi/audio-slicer)
+ [Vocal pitch extraction:RMVPE](https://github.com/Dream-High/RMVPE)
+ The pretrained model is trained and tested by [yxlllc](https://github.com/yxlllc/RMVPE) and [RVC-Boss](https://github.com/RVC-Boss).

## Thanks to all contributors for their efforts
<a href="https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/graphs/contributors" target="_blank">
<img src="https://contrib.rocks/image?repo=RVC-Project/Retrieval-based-Voice-Conversion-WebUI" />
</a>

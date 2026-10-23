# 视频出海翻译配音（VideoLingo）

> 篝火出品 · 收录于篝火 App 应用区 · 基于开源项目 [Huanshere/VideoLingo](https://github.com/Huanshere/VideoLingo)（Apache-2.0）

把视频做成字幕翻译加配音：语音识别、字幕切分与翻译、双语字幕压进视频，并可选生成配音。在浏览器里的 Streamlit 页面操作，也可粘贴 YouTube 链接下载。

## 下载与启动

到 [Releases](https://github.com/campfire-code/video-translate-dub/releases/latest) 下载 `video-translate-dub_版本号_portable.zip`，解压到一个**路径里没有中文和空格**的文件夹。

- **Windows**：双击「启动（Windows 双击）.bat」。
- **Mac**：右键「启动（Mac 双击）.command」→「打开」。

第一次启动会自动安装 uv、Python 3.12、依赖和 FFmpeg（上游用 static-ffmpeg 自动下载，不需要自己装 FFmpeg），完成后自动打开 http://localhost:8501 。以后启动很快。

## 前置条件与占用

- **大模型（必须自己准备）**：翻译和字幕切分要调用大模型。在页面左侧栏填写**接口地址、API Key、模型名**（OpenAI 兼容格式），也可直接编辑 `config.yaml`。模型需要能返回工作流要求的 JSON 结构，不合格时看 `output/gpt_log/error.json`。上游 README 推荐的接口地址是其合作方，篝火没有要求使用，请自行选择。
- **密钥保存位置**：填写后保存在本机解压目录下的 `config.yaml`（明文）。不要把含密钥的 `config.yaml` 发给别人或上传到仓库。篝火检查了上游代码，没有发现把密钥打印到日志的写法（按关键字检索，非逐行审计）。
- **语音识别**：默认在本机运行 Qwen3-ASR + ForcedAligner，首次使用会下载模型。Apple 芯片 Mac 走 MLX；Windows / Linux 有 NVIDIA 显卡会装 CUDA 版 PyTorch，没有显卡则用 CPU（更慢）；Intel Mac 用 CPU。也可在侧栏改用 ElevenLabs 或 MAI-Transcribe-2（音频会发给对应服务商并可能收费）。
- **配音**：可选 OpenAI、Fish Audio、SiliconFlow、GPT-SoVITS、Edge TTS、F5-TTS 等，部分需要对应服务的账号或自己部署，Edge TTS 不需要密钥。
- **首次安装耗时与占用空间**：本机实测（Apple 芯片 Mac，使用清华 PyPI 镜像，网络良好）：安装依赖约 6.5 分钟，`.venv` 约 2.1 GB。安装阶段**没有**下载语音识别模型，Qwen3-ASR-1.7B（8-bit）和对齐模型在第一次识别时才下载，其耗时和占用**未实测**（上游未标注大小）。Windows / NVIDIA 的 CUDA 版 PyTorch 更大，**未实测**。建议预留 10 GB 以上磁盘。
- **网络**：安装依赖、下载模型、下载 YouTube 视频都需要联网；启动脚本会询问是否使用国内 PyPI 镜像，模型和 YouTube 下载不走该镜像。
- 启动脚本已关闭 Streamlit 的使用统计，并只监听本机（127.0.0.1），局域网内其他电脑访问不到。
- 界面语言可在页面切换，默认配置为简体中文（`display_language: zh-CN`）。

## 怎么用

1. 启动后，在左侧栏填大模型接口、识别语言、目标语言。
2. 上传视频，或粘贴 YouTube 链接下载。
3. 依次执行识别、翻译、字幕生成，需要配音时再选配音方式并生成。
4. 结果在解压目录的 `output/` 下，包含字幕文件和合成后的视频。

## 已知限制（来自上游）

- 背景噪音会影响识别和时间戳；数字和符号的时间戳可能不准。
- 不会自动给不同说话人分配不同声音；混合语言的音频不保证每种语言都准确。
- 配音的自然度和对口型取决于翻译、TTS 服务和语速，请人工检查成品。
- 请只处理你有权使用和翻译的视频。

## 验收用例

| 操作 | 应得到 |
|---|---|
| 双击启动脚本，等待安装完成 | 浏览器自动打开 http://localhost:8501 的页面 |
| 填好大模型接口，上传一段 1 分钟内的视频，目标语言选简体中文，执行识别和翻译 | `output/` 下生成字幕文件，页面可预览双语字幕 |

## 和上游的区别

- 仅新增中文说明、`campfire/` 启动脚本和篝火发布流程（`tool.json`、`.github/workflows/release.yml`），代码与上游一致。
- 启动脚本用 uv 建 `.venv`（Python 3.12）后调用上游的 `installer.py`，再用 `streamlit run st.py` 启动；不使用上游的共享环境（`~/.venvs/videolingo`）。

## 归属与许可证

本仓库由**篝火**整理发布，是篝火 App 应用区收录的开源工具之一。软件全部功能与版权归 [Huanshere/VideoLingo](https://github.com/Huanshere/VideoLingo) 原作者所有，按 Apache-2.0 许可证再分发（见 `LICENSE`）。全部工具目录见 [toolkit-hub](https://github.com/campfire-code/toolkit-hub)。

---

以下为上游原始说明：

<div align="center">

<img src="/docs/logo.png" alt="VideoLingo Logo" height="140">

# Connect the World, Frame by Frame

<a href="https://trendshift.io/repositories/12200" target="_blank"><img src="https://trendshift.io/api/badge/repositories/12200" alt="Huanshere%2FVideoLingo | Trendshift" style="width: 250px; height: 55px;" width="250" height="55"/></a>

[**English**](/README.md)｜[**简体中文**](/translations/README.zh.md)｜[**繁體中文**](/translations/README.zh-TW.md)｜[**日本語**](/translations/README.ja.md)｜[**Español**](/translations/README.es.md)｜[**Русский**](/translations/README.ru.md)｜[**Français**](/translations/README.fr.md)

</div>

## 🌟 Overview ([Try VL Now!](https://videolingo.io))

VideoLingo combines speech recognition, subtitle translation, segmentation and dubbing in a Streamlit interface. It produces subtitle files and optionally subtitled or dubbed videos. Translation quality depends on the source audio, language and chosen models.

Key features:
- 🎥 YouTube video download via yt-dlp

- Word-level speech recognition and alignment with Qwen3-ASR + Qwen3-ForcedAligner

- **📝 NLP and AI-powered subtitle segmentation**

- **📚 Custom + AI-generated terminology for coherent translation**

- Direct translation with optional reflection and natural rewriting

- Subtitle segmentation with configurable length limits

- **🗣️ Dubbing with GPT-SoVITS, OpenAI, Edge TTS, and more**

- 🚀 One-click startup and processing in Streamlit

- 🌍 Multi-language support in Streamlit UI

- 📝 Detailed logging with progress resumption

- 🔍 Model searchbox with API auto-fetch — search and filter from your provider's full model list

- ⏯️ Task control — pause, resume, or stop processing at any step

The workflow combines transcription, translation, subtitle layout and dubbing in one project.

## 🎥 Demo

<table>
<tr>
<td width="33%">

### Dual Subtitles
---
https://github.com/user-attachments/assets/a5c3d8d1-2b29-4ba9-b0d0-25896829d951

</td>
<td width="33%">

### Cosy2 Voice Clone
---
https://github.com/user-attachments/assets/e065fe4c-3694-477f-b4d6-316917df7c0a

</td>
<td width="33%">

### GPT-SoVITS with my voice
---
https://github.com/user-attachments/assets/47d965b2-b4ab-4a0b-9d08-b49a7bf3508c

</td>
</tr>
</table>

### Language Support

**Input languages:**

🇺🇸 English 🤩 | 🇷🇺 Russian 😊 | 🇫🇷 French 🤩 | 🇩🇪 German 🤩 | 🇮🇹 Italian 🤩 | 🇪🇸 Spanish 🤩 | 🇯🇵 Japanese 😊 | 🇨🇳 Chinese 🤩

Dubbing languages depend on the selected TTS method.

## Installation

VideoLingo supports Windows, macOS (Apple Silicon / Intel), and Linux.

### Ask your local AI agent 🤖

If you use an AI agent that can operate your computer, send it this prompt:

> Install and launch GitHub's Huanshere/VideoLingo on my computer.

### Windows: one-click install 🎉

1. Download **Source code (zip)** from the [latest Release](https://github.com/Huanshere/VideoLingo/releases/latest), extract it to your Desktop or another folder, and open the folder.
2. Double-click `OneKeyStart.bat` and keep the window open. On the first run, it automatically installs uv, Python 3.12, the app dependencies, and FFmpeg. An internet connection is required.
3. After installation, VideoLingo opens automatically in your browser. Enter your API URL, key, and model in the sidebar to start using it.

### Install from source (Windows, macOS, Linux)

```bash
git clone https://github.com/Huanshere/VideoLingo.git && cd VideoLingo
uv run start.py
```

To start it later, run `uv run start.py` again from the VideoLingo folder. Apple Silicon uses MLX; Intel Macs use CPU recognition. By default, Intel Mac dubbing uses the new voice without the original background sound.

#### Docker (optional)

For a Linux NVIDIA container deployment, install Docker, a compatible GPU driver and the [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html). The image uses the same Python 3.12 setup and application dependencies, with CUDA 12.8.1/cu128 by default. See [Docker docs](/docs/pages/docs/docker.en-US.md) for the matched CUDA 12.6 alternative and persistence settings.

```bash
docker build -t videolingo .
docker run -d -p 8501:8501 --gpus all videolingo
```

## HTTP API (replaces Excel batch mode)

For agents and scripts, use the local HTTP API instead of the former Excel batch mode.
It shares the Streamlit pipeline and processes one operation at a time using `output/`.
Configure `config.yaml` and start it from the project root. The command also installs missing dependencies on first use:

```bash
uv run start.py --api
```

See the **[HTTP API guide](docs/api.md)** for input, processing, progress, downloads,
retries and serial batch processing. Interactive endpoint docs: [localhost:8000/docs](http://localhost:8000/docs).

## LLM, ASR and TTS providers
VideoLingo supports OpenAI-Like API format and various TTS interfaces:
- LLM: choose an OpenAI-compatible Chat Completions provider and model that can return the structured JSON required by the workflow. [OpenLux](https://www.openlux.ai/register?aff=wKYu) is recommended; set the API URL to `https://api.openlux.ai/v1`. Prefer GPT-6 Luna with model ID `gpt-6-luna` for best value, GPT-6 Sol with `gpt-6-sol` for better quality, or Claude Opus 5.5 with `claude-opus-5-5` for best quality. OpenLux relay rates are in the install docs. Configure the API URL, key and model in the sidebar.
- Speech recognition: run Qwen3-ASR + ForcedAligner locally (default), or choose ElevenLabs or MAI-Transcribe-2 in the sidebar. MAI supports an Azure Speech key or an OpenRouter key entered in the sidebar; audio is sent to the selected provider and may incur charges. WhisperX is not installed by the installer; to use it as a backend, follow [WhisperX (manual install)](docs/pages/docs/whisperx-manual.en-US.md).
- TTS: OpenAI, Fish Audio, SiliconFlow Fish/CosyVoice2, GPT-SoVITS, Edge TTS, F5-TTS and a custom adapter in `core/tts_backend/custom_tts.py`.

For detailed installation, LLM configuration, and usage instructions, please refer to the documentation: [English](/docs/pages/docs/start.en-US.md) | [中文](/docs/pages/docs/start.zh-CN.md)

## Current Limitations

1. Background noise and language-specific alignment models affect recognition and word timestamps. Vocal separation may help. Numbers and symbols may lack reliable word timings; inspect the resulting subtitles.

2. LLM output must satisfy the workflow's JSON structure. For failures, inspect `output/gpt_log/error.json`. Existing successful response caches and completed outputs can be reused on retry; changing the model alone does not regenerate every completed stage. Do not delete all output as the first troubleshooting step.

3. Dubbing quality and timing depend on translation, the TTS service and speech rate. Speed adjustment does not guarantee natural delivery or perfect synchronization.

4. Local recognition uses one primary recognition/alignment language per audio segment. Mixed-language speech is not guaranteed to retain accurate text and timing in every language.

5. The dubbing workflow does not automatically assign a separate voice to each speaker.

## 📄 License

This project is licensed under the Apache 2.0 License. Special thanks to the following open source projects for their contributions:

[Qwen3-ASR](https://github.com/Qwen/Qwen3-ASR), [MLX Audio](https://github.com/Blaizzy/mlx-audio), [whisperX](https://github.com/m-bain/whisperX), [yt-dlp](https://github.com/yt-dlp/yt-dlp), [json_repair](https://github.com/mangiucugna/json_repair), [BELLE](https://github.com/LianjiaTech/BELLE)

## 📬 Contact Me

- Submit [Issues](https://github.com/Huanshere/VideoLingo/issues) or [Pull Requests](https://github.com/Huanshere/VideoLingo/pulls) on GitHub
- DM me on Twitter: [@Huanshere](https://twitter.com/Huanshere)
- Email me at: team@videolingo.io

## ⭐ Star History

[![Star History Chart](https://api.star-history.com/svg?repos=Huanshere/VideoLingo&type=Timeline)](https://star-history.com/#Huanshere/VideoLingo&Timeline)

---

<p align="center">If you find VideoLingo helpful, please give me a ⭐️!</p>

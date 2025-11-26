![](images/logo.png)

---

[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/)
[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![Vue 3](https://img.shields.io/badge/vue-3.x-green.svg)](https://vuejs.org/)

# 红墨 - 小红书AI图文生成器

> 让传播不再需要门槛，让创作从未如此简单

![](images/index.gif)

<p align="center">
  <em>红墨首页</em>
</p>

<p align="center">
  <img src="images/showcase-grid.png" alt="使用红墨生成的各类小红书封面" width="600"/>
</p>

<p align="center">
  <em>使用红墨生成的各类小红书封面 - AI驱动，风格统一，文字准确</em>
</p>



## 写在前面

前段时间默子在 Linux.do 发了一个用 Nano banana Pro 做 PPT 的帖子,收获了 600 多个赞。很多人用🍌Nano banana Pro 去做产品宣传图、直接生成漫画等等。我就在想:**为什么不拿🍌2来做点更功利、更刺激的事情?**

于是就有了这个项目。一句话一张图片生成小红书图文

---

## ✨ 效果展示

### 输入一句话,就能生成完整的小红书图文

#### 提示词：秋季显白美甲（暗广一个：默子牌美甲），图片 是我的小红书主页。符合我的风格生成

#### 同时我还截图了我的小红书主页，包括我的头像，签名，背景，姓名什么的

![示例1](./images/example-1.png)

#### 然后等待10-20秒后，就会有每一页的大纲，大家可以根据的自己的需求去调整页面顺序（不建议），自定义每一个页面的内容（这个很建议）

![示例2](./images/example-2.png)

#### 首先生成的是封面页

![示例3](./images/example-3.png)

#### 然后稍等一会儿后，会生成后面的所有页面（这里是并发生成的所有页面（最高25个），如果大家的API供应商无法支持高并发的话，记得要去改一下设置）

![示例4](./images/example-4.png)

---

## 🏗️ 技术架构

### 后端
- **语言**: Python 3.11+
- **框架**: Flask
- **AI 模型**:
  - Gemini 3 (文案生成)
  - 🍌Nano banana Pro (图片生成)
- **包管理**: uv

### 前端
- **框架**: Vue 3 + TypeScript
- **构建**: Vite
- **状态管理**: Pinia

---

## ⚙️ Configuration & Deployment

This guide covers environment variables, local development setup, and Vercel deployment options.

### 1. Environment Variables

Configure these variables in your `.env` file (local) or Vercel Project Settings (production).

| Variable | Required | Default | Description | Where to Retrieve |
| :--- | :--- | :--- | :--- | :--- |
| `GOOGLE_CLOUD_API_KEY` | **Yes** | - | API Key for Google Gemini (Text & Image) | [Google AI Studio](https://aistudio.google.com/) |
| `IMAGE_API_KEY` | No | - | API Key for Custom Image Provider | Your provider dashboard |
| `TEXT_API_KEY` | No | - | API Key for Custom Text Provider | Your provider dashboard |
| `TEXT_API_BASE_URL` | No | `https://api.bltcy.ai` | Base URL for Custom Text Provider | Your provider docs |
| `FLASK_DEBUG` | No | `True` | Debug mode toggle | - |
| `FLASK_HOST` | No | `0.0.0.0` | Server host | - |
| `FLASK_PORT` | No | `12398` | Server port | - |
| `CORS_ORIGINS` | No | `http://localhost:5173...` | Allowed CORS origins | - |
| `OUTPUT_DIR` | No | `output` | Local image output directory | - |
| `STORAGE_BACKEND` | No | `local` | Storage backend (`local`, `vercel_blob`, `vercel_kv`) | - |
| `VERCEL_BLOB_READ_WRITE_TOKEN`| No | - | Vercel Blob Token | Vercel Dashboard (Storage) |
| `VERCEL_KV_REST_API_URL` | No | - | Vercel KV URL | Vercel Dashboard (Storage) |
| `VERCEL_KV_REST_API_TOKEN` | No | - | Vercel KV Token | Vercel Dashboard (Storage) |
| `IMAGE_PROVIDER` | No | `google_genai` | Active image provider name | - |

### 2. Image Provider Configuration Strategies

RedInk offers two ways to configure image providers:

| Feature | Option A: Config File (`image_providers.yaml`) | Option B: Env-Only |
| :--- | :--- | :--- |
| **Setup Complexity** | Higher (requires managing YAML file) | Lower (just env vars) |
| **Provider Support** | **All** (Custom, OpenAI, Google, etc.) | **Google Gemini Only** |
| **Flexibility** | High (detailed params per provider) | Low (defaults only) |
| **Deployment** | Must commit file or allow in `.gitignore` | Easy (just set envs) |
| **Best For** | Power users, Custom APIs, multiple providers | Quick start, Google users |

### 3. Local Development Setup

**Prerequisites:**
- Python 3.11+
- Node.js 18+ & pnpm
- [uv](https://github.com/astral-sh/uv) (Python package manager)

#### Option A: With `image_providers.yaml` (Recommended for Custom Providers)

1. **Clone the repository:**
   ```bash
   git clone https://github.com/HisMax/RedInk.git
   cd RedInk
   ```

2. **Configure Environment:**
   ```bash
   cp .env.example .env
   cp image_providers.yaml.example image_providers.yaml
   ```
   - Edit `.env` with your API keys.
   - Edit `image_providers.yaml` to configure your specific provider (e.g., `active_provider: image_api`).

3. **Install Dependencies & Run:**
   ```bash
   # Backend
   uv sync
   uv run python -m backend.app

   # Frontend (in a new terminal)
   cd frontend
   pnpm install
   pnpm dev
   ```

#### Option B: Env-Only (Quick Start / Google GenAI)

1. **Clone the repository:**
   ```bash
   git clone https://github.com/HisMax/RedInk.git
   cd RedInk
   ```

2. **Configure Environment:**
   ```bash
   cp .env.example .env
   ```
   - Edit `.env` and set `GOOGLE_CLOUD_API_KEY`.
   - **Skip** creating `image_providers.yaml`. The system will automatically default to Google Gemini configuration.

3. **Install Dependencies & Run:**
   ```bash
   # Backend
   uv sync
   uv run python -m backend.app

   # Frontend (in a new terminal)
   cd frontend
   pnpm install
   pnpm dev
   ```

### 4. Vercel Deployment

For detailed instructions, see [Vercel Deployment Guide](docs/vercel.md).

#### Method 1: Using Vercel Dashboard (Recommended)
1. Fork this repository.
2. Import project in Vercel Dashboard.
3. In **Environment Variables**, add the required keys (e.g., `GOOGLE_CLOUD_API_KEY`).
4. (Optional) Bind **Vercel KV** storage for persistence.
5. Deploy.

#### Method 2: Using `vercel.json` & CLI
1. Configure `vercel.json` (already present) for build settings.
2. Use Vercel CLI to deploy:
   ```bash
   npm i -g vercel
   vercel link
   vercel env pull .env.local  # Pull envs if needed
   vercel deploy
   ```
   *Note: Do not commit secrets to `vercel.json`.*

### 5. Troubleshooting

**Common Pitfalls:**

- **`image_providers.yaml` not found:** The system will fallback to the default Google Gemini configuration. If you are trying to use a custom provider, ensure the file exists and is readable.
- **API Key Errors:** Double-check that `GOOGLE_CLOUD_API_KEY` or `IMAGE_API_KEY` are set correctly in `.env` (local) or Vercel Environment Variables.
- **Vercel Storage:** If `STORAGE_BACKEND` is set to `vercel_kv` but no database is bound, the app will fail to save history. Check `KV_URL` presence.
- **Provider Mismatch:** If you set `IMAGE_PROVIDER=image_api` but didn't provide `image_providers.yaml`, the app will fail because `image_api` configuration is missing from the default fallback.

---

## 🎮 使用指南

### 基础使用
1. **输入主题**: 在首页输入想要创作的主题,如"如何在家做拿铁"
2. **生成大纲**: AI 自动生成 6-9 页的内容大纲
3. **编辑确认**: 可以编辑和调整每一页的描述
4. **生成图片**: 点击生成,实时查看进度
5. **下载使用**: 一键下载所有图片

### 进阶使用
- **上传参考图片**: 适合品牌方,保持品牌视觉风格
- **修改描述词**: 精确控制每一页的内容和构图
- **重新生成**: 对不满意的页面单独重新生成


## ⚠️ 注意事项

1. **API 配额限制**:
   - 注意 Gemini 和 Nano banana Pro 的调用配额
   - 建议使用支持高并发的 API 中转平台

2. **生成时间**:
   - 图片生成需要时间,请耐心等待（不要离开页面）

---

## 🤝 参与贡献

欢迎提交 Issue 和 Pull Request!

如果这个项目对你有帮助,欢迎给个 Star ⭐

### 未来计划
- [ ] 支持更多图片格式，例如一句话生成一套PPT什么的
- [ ] 历史记录管理优化
- [ ] 导出为各种格式(PDF、长图等)

---

## 交流讨论与赞助

- **GitHub Issues**: [https://github.com/HisMax/RedInk/issues](https://github.com/HisMax/RedInk/issues)

### 联系作者

- **Email**: histonemax@gmail.com
- **微信**: Histone2024（添加请注明来意）
- **GitHub**: [@HisMax](https://github.com/HisMax)

### 用爱发电，如果可以，请默子喝一杯☕️咖啡吧

<img src="images/coffee.jpg" alt="赞赏码" width="300"/>

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=HisMax/RedInk&type=Date)](https://star-history.com/#HisMax/RedInk&Date)

---

---

## 📄 开源协议

### 个人使用 - CC BY-NC-SA 4.0

本项目采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) 协议进行开源

**你可以自由地：**
- ✅ **个人使用** - 用于学习、研究、个人项目
- ✅ **分享** - 在任何媒介以任何形式复制、发行本作品
- ✅ **修改** - 修改、转换或以本作品为基础进行创作

**但需要遵守以下条款：**
- 📝 **署名** - 必须给出适当的署名，提供指向本协议的链接，同时标明是否对原始作品作了修改
- 🚫 **非商业性使用** - 不得将本作品用于商业目的
- 🔄 **相同方式共享** - 如果你修改、转换或以本作品为基础进行创作，你必须以相同的协议分发你的作品

### 商业授权

如果你希望将本项目用于**商业目的**（包括但不限于）：
- 提供付费服务
- 集成到商业产品
- 作为 SaaS 服务运营
- 其他盈利性用途

**请联系作者获取商业授权：**
- 📧 Email: histonemax@gmail.com
- 💬 微信: Histone2024（请注明"商业授权咨询"）

默子会根据你的具体使用场景提供灵活的商业授权方案。

---

### 免责声明

本软件按"原样"提供，不提供任何形式的明示或暗示担保，包括但不限于适销性、特定用途的适用性和非侵权性的担保。在任何情况下，作者或版权持有人均不对任何索赔、损害或其他责任负责。

---

## 🙏 致谢

- [Google Gemini](https://ai.google.dev/) - 强大的文案生成能力
- 图片生成服务提供商 - 惊艳的图片生成效果
- [Linux.do](https://linux.do/) - 优秀的开发者社区

---

## 👨‍💻 作者

**默子 (Histone)** - AI 创业者 | Python & 深度学习

- 🏠 位置: 中国杭州
- 🚀 状态: 创业中
- 💡 专注: Transformers、GANs、多模态AI
- 📧 Email: histonemax@gmail.com
- 💬 微信: Histone2024
- 🐙 GitHub: [@HisMax](https://github.com/HisMax)

*"让 AI 帮我们做更有创造力的事"*

---

**如果这个项目帮到了你,欢迎分享给更多人!** ⭐

有任何问题或建议,欢迎提 Issue 或者在 Linux.do 原帖讨论!

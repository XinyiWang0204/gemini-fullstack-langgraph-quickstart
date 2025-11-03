# 前后端运行指南

本文档记录了前后端的完整启动流程，已在实际环境中验证通过。

## 前置准备

### 1. 检查 Python 版本

项目要求 **Python 3.11+**，如果系统 Python 版本不足，需要先安装：

```bash
# 检查当前 Python 版本
python3 --version

# 如果没有 Python 3.11+，使用 Homebrew 安装（macOS）
brew install python@3.12
```

安装后，Python 3.12 将位于：`/opt/homebrew/bin/python3.12`

### 2. 准备 Gemini API Key

1. 访问 [Google AI Studio](https://makersuite.google.com/app/apikey) 创建 API Key
2. 建议命名：`langgraph-research-agent` 或 `gemini-fullstack-langgraph-research`

## 一、前端启动步骤

### 1. 安装前端依赖

```bash
cd frontend
npm install
```

### 2. 启动前端开发服务器

```bash
# 方式一：在 frontend 目录下
cd frontend
npm run dev

# 方式二：从项目根目录
make dev-frontend
```

前端将运行在：**http://localhost:5173**

访问应用页面：**http://localhost:5173/app**

## 二、后端启动步骤

### 1. 配置 API Key

在 `backend/` 目录下创建 `.env` 文件：

```bash
cd backend
echo 'GEMINI_API_KEY=YOUR_ACTUAL_API_KEY' > .env
```

**注意：** 将 `YOUR_ACTUAL_API_KEY` 替换为你的实际 API Key。

### 2. 创建 Python 虚拟环境

**重要：** 后端依赖必须安装在虚拟环境中。

```bash
cd backend

# 如果使用 Python 3.12（通过 Homebrew 安装）
/opt/homebrew/bin/python3.12 -m venv venv

# 或者如果系统 Python 已经是 3.11+
python3 -m venv venv
```

### 3. 激活虚拟环境并安装依赖

```bash
cd backend
source venv/bin/activate
pip install .
```

安装过程中会下载并安装所有依赖包（langgraph、langchain、google-genai 等）。

### 4. 启动后端开发服务器

```bash
cd backend
source venv/bin/activate  # 如果虚拟环境未激活
langgraph dev
```

后端将运行在：**http://127.0.0.1:2024**

启动时会自动打开 LangGraph UI 浏览器窗口。

## 三、同时启动前后端

### 方式一：使用 Makefile（推荐）

在项目根目录执行：

```bash
make dev
```

这会同时启动前后端开发服务器。

### 方式二：分别启动

**终端 1 - 前端：**
```bash
cd frontend
npm run dev
```

**终端 2 - 后端：**
```bash
cd backend
source venv/bin/activate
langgraph dev
```

## 四、完整启动流程（已验证）

以下是我们刚才实际执行的完整流程：

```bash
# 1. 安装 Python 3.12（如果系统版本不足）
brew install python@3.12

# 2. 配置后端 API Key
cd backend
echo 'GEMINI_API_KEY=YOUR_ACTUAL_API_KEY' > .env

# 3. 创建虚拟环境
/opt/homebrew/bin/python3.12 -m venv venv

# 4. 激活虚拟环境并安装依赖
source venv/bin/activate
pip install .

# 5. 安装前端依赖
cd ../frontend
npm install

# 6. 启动前端（终端 1）
npm run dev

# 7. 启动后端（终端 2）
cd ../backend
source venv/bin/activate
langgraph dev
```

## 五、访问地址

- **前端应用**：http://localhost:5173/app
- **后端 API**：http://127.0.0.1:2024
- **LangGraph UI**：会自动打开浏览器窗口

## 六、常见问题

### 1. Python 版本不足

**错误信息：**
```
ERROR: Package 'agent' requires a different Python: 3.9.6 not in '<4.0,>=3.11'
```

**解决方法：**
安装 Python 3.12：
```bash
brew install python@3.12
```

然后使用 `/opt/homebrew/bin/python3.12` 创建虚拟环境。

### 2. 依赖安装失败

**错误信息：**
```
error: externally-managed-environment
```

**解决方法：**
必须使用虚拟环境，不要直接在系统 Python 中安装：
```bash
python3.12 -m venv venv
source venv/bin/activate
pip install .
```

### 3. API Key 未配置

**错误信息：**
后端启动时提示找不到 API Key

**解决方法：**
确保 `backend/.env` 文件存在且包含：
```
GEMINI_API_KEY=YOUR_ACTUAL_API_KEY
```

## 七、停止服务器

- **前端**：在运行前端的终端按 `Ctrl + C`
- **后端**：在运行后端的终端按 `Ctrl + C`
- **虚拟环境**：执行 `deactivate` 退出虚拟环境

## 八、后续启动

如果依赖已安装，后续启动只需：

**前端：**
```bash
cd frontend
npm run dev
```

**后端：**
```bash
cd backend
source venv/bin/activate
langgraph dev
```

---

**最后更新：** 已验证流程，与 2025 年实际执行步骤一致。


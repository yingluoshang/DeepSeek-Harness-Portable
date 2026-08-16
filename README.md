# DeepSeek-Harness-Portable
https://pan.quark.cn/s/bda46a0161f6

# DeepSeek Harness 便捷版（dsh-portable）

一个 **免安装、可随身携带、U 盘直启** 的 DeepSeek Harness 便捷版应用。

内置了 DeepSeek Harness 的**全部源码与依赖**，以及**便携版 Node.js 运行时**，因此**无需在本机安装 Node.js、pnpm 或任何其它环境**，复制到任意 Windows 电脑或 U 盘即可直接运行。

---

## 一、特性

| 特性 | 说明 |
| --- | --- |
| 无需本地环境 | 内置便携版 Node.js（v24.19.0），不依赖本机 Node / pnpm / git |
| 依赖已内置 | DeepSeek Harness 的 node_modules 已随包携带 |
| U 盘直启 | 全部使用相对路径 + 运行时自动定位，换盘符/换电脑可直接运行 |
| 现代控制面板 | React + TypeScript + Vite + Tailwind CSS 构建的管理界面 |
| 后端管理 | Node.js + TypeScript + Express，一键启停 Harness |
| SQLite 存储 | 使用 Node 内置 node:sqlite，保存设置与启动记录 |
| 实时日志 | 面板内实时查看 Harness 启动/运行日志 |
| 中文使用说明 | 面板内置「说明」页 + 本文档 |

---

## 二、目录结构

    dsh-portable/
    ├─ harness/          DeepSeek Harness 源码与依赖（已内置）
    │   ├─ apps/          CLI 与 Web 前端
    │   ├─ packages/      全部插件包
    │   └─ node_modules/  全部依赖（已去符号链接化，兼容 exFAT 等文件系统）
    ├─ runtime/
    │   └─ node/
    │       └─ node.exe   便携版 Node.js 运行时
    ├─ manager/          控制面板（本便捷版应用）
    │   ├─ server/        后端源码（Express + SQLite）
    │   ├─ web/           前端源码（React + Vite + Tailwind）
    │   ├─ dist-server/   后端编译产物
    │   ├─ dist-web/      前端构建产物
    │   └─ node_modules/  后端运行依赖（express 等）
    ├─ home/             DSH 数据目录（会话 / 配置 / 凭据）
    ├─ data/             SQLite 数据库（设置 / 启动记录）
    ├─ logs/             运行日志（harness.log）
    ├─ start.bat         一键启动（英文提示）
    ├─ 启动.bat           一键启动（中文文件名入口）
    ├─ README.md         本说明文档
    └─ version.txt       版本信息

---

## 三、快速开始

1. **复制**：将整个 dsh-portable 文件夹复制到 U 盘或任意 Windows 电脑。**注意：需要使用 NTFS 文件系统**（程序内部使用了符号链接，exFAT / FAT32 不支持符号链接）。
2. **启动**：双击运行 start.bat（或 启动.bat）。
3. **自动运行**：脚本会自动启动控制面板，并自动启动 DeepSeek Harness，随后打开浏览器。
4. **进入 Harness**：在控制面板中点击「打开 Harness UI ↗」，即可进入 DeepSeek Harness 的 Web 界面。

默认地址：

- 控制面板：http://127.0.0.1:4100
- DeepSeek Harness Web UI：http://127.0.0.1:3080（可在面板「设置」中修改端口）

---

## 四、控制面板功能

- **运行状态**：实时显示 Harness 状态（运行中 / 启动中 / 已停止 / 失败）、端口、PID、运行时长。
- **一键启停**：启动 / 停止 / 重启 DeepSeek Harness。
- **打开 Harness UI**：一键打开 Harness 的浏览器界面。
- **启动日志**：实时滚动显示 Harness 启动与运行日志（SSE 推送）。
- **设置**：修改 Harness 端口、监听地址、是否自动启动、是否自动打开浏览器。
- **环境诊断**：检查便携版 Node.js、Harness 源码、依赖、前端构建产物、数据目录是否完整。
- **启动记录**：SQLite 保存的历史启动/停止记录。
- **使用说明**：面板内置中文说明。

---

## 五、常见问题

**Q1：需要联网吗？**
启动本身**无需联网**；但 DeepSeek Harness 运行时调用模型 API 需要联网，并需在 Harness 内配置模型凭据。

**Q2：端口被占用怎么办？**
在控制面板「设置」中修改端口，保存后重启 Harness。

**Q3：换了一台电脑 / 换了盘符还能用吗？**
可以。程序内部使用相对路径，并在每次启动时自动重新定位内部依赖（profile 的模块链接会自动修复）。

**Q4：为什么文件夹很大（约 1.5GB+）？**
因为内置了 DeepSeek Harness 的全部依赖（node_modules）与 Node.js 运行时，这是「免安装、离线可用」的代价。

**Q5：如何停止？**
关闭控制面板对应的命令行窗口即可（会自动停止 Harness 进程）；或在面板点击「停止」。

---

## 六、技术栈

| 层级 | 技术 |
| --- | --- |
| 前端 | React 18 · TypeScript · Vite 5 · Tailwind CSS 3 |
| 后端 | Node.js · TypeScript · Express 4 |
| 数据 | SQLite（Node 内置 node:sqlite，零原生依赖） |
| 运行时 | 便携版 Node.js v24.19.0 |
| 被托管应用 | DeepSeek Harness（deepseek-harness-master，0.1.0-rc.5） |

---

## 七、从源码重新构建（可选，面向开发者）

若需重新构建控制面板或更新 Harness：

    # 1. 更新 Harness（替换 harness/ 目录内容）
    # 2. 重新安装控制面板依赖并构建
    cd manager
    npm install
    npm run build        # 生成 dist-server + dist-web
    npm prune --omit=dev # 精简运行时依赖（可选）

便携版已随包附带构建产物，普通用户无需执行以上步骤。

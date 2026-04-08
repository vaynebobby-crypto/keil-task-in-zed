# keil-task-in-zed

# Zed-Keil-集成编译环境 使用说明

## 简介

本工具集旨在将 Keil C51/MDK-ARM 的编译、烧录和清理功能无缝集成到 Zed 编辑器中，让你无需离开 Zed 即可完成嵌入式项目的主要操作。

它通过 Zed 的"任务(Tasks)"功能调用批处理脚本，实现了项目文件的自动查找和命令的自动执行。

---

## 快速开始

### 方式 A：作为 Zed 扩展（推荐）

1. 在 Zed 扩展市场安装 **“Keil Task Runner”**（本仓库的扩展包）。
2. 在 Windows 环境变量中设置 `KEIL_PATH`（指向 `UV4.exe` 或 `UV5.exe`）。若未设置，脚本会回落到 `keil_task.bat` 内的默认值。
3. 打开你的 Keil 工程所在文件夹。
4. 按 `Ctrl + Shift + P` → 选择 `Tasks: Run`，挑选 `Keil: Build/Flash/Clean/...` 任务即可执行。

### 方式 B：手动复制 `.zed`（沿用旧方式）

1. 复制仓库里的 `.zed` 文件夹到任意 Keil 工程根目录。
2. 打开 `.zed/keil/keil_task.bat`，将文件顶部的 `KEIL_PATH` 默认值改成你的 Keil 安装路径（若未通过环境变量指定）。
3. 在 Zed 中打开该工程后，按 `Ctrl + Shift + P` → `Tasks: Run`，选择相应任务运行。

---

## 7大核心功能

### 1. 无缝编译集成
- **自动项目查找**: 自动在当前目录及子目录中查找 `.uvprojx` 或 `.uvproj` 文件
- **完整编译支持**: 支持增量编译、完全重新编译和烧录操作
- **实时输出显示**: 在 Zed 中直接显示 Keil 编译器的完整输出

### 2. 智能代码补全
- **自动生成 clangd 配置**: 针对嵌入式开发优化的配置选项
- **编译命令生成**: 使用 keil2clangd 工具生成真实的编译命令数据库
- **语法检查**: 实时显示语法错误和警告，减少低级错误

### 3. 版本控制优化
- **自动生成 .gitignore**: 包含常见的 Keil 中间文件和临时文件
- **多IDE支持**: 同时忽略 VSCode、JetBrains、AI工具等配置文件
- **团队协作**: 统一的忽略规则，避免提交不必要文件

### 4. 编码问题解决
- **批量UTF-8转换**: 一键将所有 `.c` 和 `.h` 文件转换为 UTF-8 编码
- **中文注释支持**: 解决中文注释在不同系统下的乱码问题
- **跨平台兼容**: 确保代码在不同操作系统下正确显示

### 5. 错误诊断与日志
- **详细错误信息**: 捕获并显示完整的编译错误和警告
- **日志文件保存**: 自动保存编译日志到 `keil_build_log.txt`
- **问题溯源**: 便于定位和分析编译问题

### 6. 项目管理辅助
- **多项目支持**: 当存在多个项目文件时提供选择界面
- **配置验证**: 自动检查 Keil 安装路径和项目文件有效性
- **路径处理**: 智能处理各种路径格式和特殊情况

### 7. 扩展性设计
- **模块化结构**: 每个功能独立，便于单独使用和维护
- **易于定制**: 可以通过修改 `tasks.json` 添加自定义任务
- **开源贡献**: 欢迎社区贡献代码和功能建议

---

## 原理简介

*   `.zed/tasks.json`: Zed 任务的定义文件。它告诉 Zed 命令面板中有哪些任务，以及每个任务应该执行什么命令。
*   `.zed/keil_task.bat`: 核心功能脚本。它会自动查找项目中的 `.uvprojx` 或 `.uvproj` 文件，并根据任务传来的参数（`build`, `flash` 等）调用 Keil 执行相应操作，同时捕获并显示 Keil 的原生输出。
*   `.zed/keilkilll.bat`: 一个简单的清理脚本，用于删除 Keil 编译产生的各种中间文件。
*   `.zed/clangd_config.bat`: clangd 配置脚本，自动生成 `.clangd` 和 `compile_commands.json` 文件。
*   `.zed/gitignore_config.bat`: .gitignore 生成脚本，创建适合 Keil 项目的 Git 忽略规则。
*   `.zed/keil_utf8conv.bat`: 编码转换脚本，将项目中的源文件转换为 UTF-8 编码。

---

## 常见问题解答

**问：任务执行失败，输出 `[ERROR] Keil 程序未在 "..." 找到`？**

**答：** 这是最常见的问题。请确认 `KEIL_PATH` 已正确指向 `UV4.exe/UV5.exe`（优先使用环境变量，其次是 `keil_task.bat` 中的默认值）。

**问：使用 clangd 配置后代码补全仍有问题？**

**答：** 自动生成的 `compile_commands.json` 是一个模板，可能需要根据你的项目实际配置进行调整。你可以手动编辑该文件，添加项目特定的编译标志和包含路径。

**问：UTF-8 编码转换后出现乱码？**

**答：** 如果原始文件不是 ANSI 编码，转换可能会出现问题。建议在转换前备份原始文件，或使用版本控制系统管理变更。如果出现问题，可以从备份恢复或使用文本编辑器手动调整编码。

---

## 资源链接

- **项目主页**: [GitHub - keil-task-in-zed](https://github.com/Yarrow-Cai/keil-task-in-zed)
- **Zed 编辑器官网**: [zed.dev](https://zed.dev/)
- **clangd 文档**: [clangd.llvm.org](https://clangd.llvm.org/)
- **Keil MDK 官方文档**: [developer.arm.com](https://developer.arm.com/tools-and-software/embedded/keil-mdk)

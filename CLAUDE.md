# CLAUDE.md

本文件为 Claude Code (claude.ai/code) 在此仓库中工作时提供指导。

## 项目概述

一个 JetBrains IDE 插件，在 IDE 内置终端窗口中直接运行 `claude` CLI（Claude Code）。使用 Java 17 和 IntelliJ Platform SDK 开发。

## 构建命令

```bash
# 构建插件（在 build/distributions/ 生成 ZIP 文件）
./gradlew buildPlugin

# 在沙盒 IDE 中运行插件进行测试
./gradlew runIde

# 验证插件有效性
./gradlew verifyPlugin

# 发布到 JetBrains Marketplace（需要 PUBLISH_TOKEN 环境变量）
./gradlew publishPlugin
```

## 测试

无自动化测试，采用手动测试：
1. 运行 `./gradlew runIde` 启动沙盒 IDE
2. 在沙盒中验证插件功能
3. 运行 `./gradlew verifyPlugin` 验证插件结构

## 架构

三个主要 Java 类位于 `src/main/java/com/hamdiwanis/claude/`：

- **ClaudeCodeToolWindowFactory** - 创建 "Claude Code" 工具窗口（锚定在右侧），使用 `TerminalView` 创建 `ShellTerminalWidget`，打开时自动运行 `claude` 命令
- **StartClaudeCodeAction** - 处理聚焦/重启操作（Ctrl+Shift+C）。普通点击聚焦窗口；Shift+点击重建终端会话
- **ClaudeCodeUtils** - 工具类，提供 shell 引号处理、主目录展开、命令执行和 IDE 通知功能

插件配置文件位于 `src/main/resources/META-INF/plugin.xml`。

## 关键依赖

- IntelliJ Platform 2023.2+（since-build: 232）
- Terminal 插件（`org.jetbrains.plugins.terminal`）
- Gradle IntelliJ Plugin 1.17.3

## 版本更新

发布新版本时，需更新以下位置的版本号：
- `build.gradle`（version 属性）
- `plugin.xml`（如有单独记录插件版本）

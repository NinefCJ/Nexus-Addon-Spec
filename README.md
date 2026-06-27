# Nexus Addon Spec

Nexus 命令助手拓展包格式规范与开发指南。

## 目录

- [快速开始](#快速开始)
- [文档](#文档)
- [示例](#示例)
- [格式校验](#格式校验)
- [贡献](#贡献)
- [许可证](#许可证)

## 快速开始

### 什么是拓展包？

拓展包是一个 JSON 文件，可以扩展 Nexus 命令助手的数据与功能。通过拓展包，你可以添加自定义的：

- 🧱 方块 ID 与翻译
- 🛠️ 物品 ID 与翻译
- 🔊 音效 ID 与翻译
- ✨ 粒子 ID 与翻译
- ⚡ 快捷命令
- 📝 命令模板

### 我想创建拓展包

1. 复制 [examples/template.json](examples/template.json)
2. 按照你的需求修改内容
3. 在 Nexus 中导入使用

详细教程请查看 [开发指南](docs/DEVELOPMENT.md)。

### 我想了解格式规范

完整的格式规范请查看 [SPEC.md](docs/SPEC.md)。

## 文档

| 文档 | 说明 |
|------|------|
| [格式规范](docs/SPEC.md) | 详细的字段说明、格式要求、命名规范 |
| [开发指南](docs/DEVELOPMENT.md) | 开发工作流、最佳实践、调试方法、常见问题 |

## 示例

| 示例文件 | 说明 |
|----------|------|
| [template.json](examples/template.json) | 空白模板，包含所有字段结构 |
| [minimal.json](examples/minimal.json) | 最小可行示例 |
| [basic-example.json](examples/basic-example.json) | 完整示例，包含所有类型的内容 |
| [redstone-toolkit.json](examples/redstone-toolkit.json) | 红石工具包示例 |

## 格式校验

本仓库提供 [JSON Schema](schema/addon.schema.json)，可用于自动化校验拓展包格式。

### 使用方法

**VS Code 中使用：**

在 JSON 文件顶部添加：
```json
{
  "$schema": "https://raw.githubusercontent.com/NinefCJ/Nexus-Addon-Spec/main/schema/addon.schema.json",
  ...
}
```

**在线校验：**

使用 [JSON Schema Validator](https://www.jsonschemavalidator.net/) 等在线工具。

**命令行校验：**

使用 `ajv` 等工具：
```bash
npx ajv-cli validate -s schema/addon.schema.json -d your-addon.json
```

## 项目结构

```
Nexus-Addon-Spec/
├── docs/
│   ├── SPEC.md          # 格式规范文档
│   └── DEVELOPMENT.md   # 开发指南
├── examples/
│   ├── template.json    # 空白模板
│   ├── minimal.json     # 最小示例
│   ├── basic-example.json  # 完整示例
│   └── redstone-toolkit.json # 红石工具包
├── schema/
│   └── addon.schema.json    # JSON Schema
└── README.md
```

## 版本

规范版本：**v1.0.0**

### 版本历史

- **v1.0.0** (2026-06-27)
  - 初始版本
  - 支持方块、物品、音效、粒子、命令、模板

## 贡献

欢迎贡献！你可以通过以下方式参与：

- 📝 改进文档
- 💡 提出新功能建议
- 🐛 报告问题
- ✅ 提交示例拓展包

请在 [Issues](https://github.com/NinefCJ/Nexus-Addon-Spec/issues) 中反馈。

## 相关项目

- [Nexus](https://github.com/NinefCJ/Nexus) - Nexus 命令助手主项目

## 许可证

[MIT License](LICENSE)

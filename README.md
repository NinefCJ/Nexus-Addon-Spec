# Nexus Addon Spec

Nexus 命令助手拓展包格式规范与完整开发指南。

## 目录

- [快速开始](#快速开始)
- [📚 文档体系](#-文档体系)
- [📦 示例](#-示例)
- [🛠️ 格式校验](#️-格式校验)
- [📁 项目结构](#-项目结构)
- [版本](#版本)
- [贡献](#贡献)
- [相关项目](#相关项目)
- [许可证](#许可证)

---

## 快速开始

### 什么是拓展包？

拓展包是一个 JSON 文件，可以扩展 Nexus 命令助手的数据与功能。通过拓展包，你可以添加自定义的：

| 类型 | 说明 | 补全场景 |
|------|------|----------|
| 🧱 方块 | 自定义方块 ID 与翻译 | `/setblock` `/fill` `/clone` |
| ⚔️ 物品 | 自定义物品 ID 与翻译 | `/give` `/replaceitem` `/clear` |
| 🔊 音效 | 自定义音效 ID 与翻译 | `/playsound` `/stopsound` |
| ✨ 粒子 | 自定义粒子 ID 与翻译 | `/particle` |
| ⚡ 快捷命令 | 一键插入的常用命令 | 快捷命令面板 |
| 📝 命令模板 | 可复用的命令模板 | 命令库 |

### 5 分钟创建第一个拓展包

```json
{
  "id": "your_name.my_first_addon",
  "name": "我的第一个拓展包",
  "version": "1.0.0",
  "author": "你的名字",
  "description": "这是我的第一个 Nexus 拓展包",
  "items": [
    {
      "id": "custom:ruby_sword",
      "name": "红宝石剑",
      "category": "武器",
      "description": "一把由红宝石打造的神剑"
    }
  ],
  "commands": [
    {
      "id": "give_ruby_sword",
      "command": "/give @s custom:ruby_sword",
      "name": "获得红宝石剑",
      "description": "给予自己一把红宝石剑",
      "category": "道具"
    }
  ]
}
```

保存为 `.json` 文件，在 Nexus 中导入即可使用。

### 下一步去哪看？

| 你想... | 去看这篇 |
|---------|----------|
| 📖 系统学习开发 | [开发指南](docs/DEVELOPMENT.md) |
| 📋 了解所有字段 | [格式规范](docs/SPEC.md) |
| 🐛 遇到问题了 | [调试指南](docs/DEBUGGING.md) |
| 🚀 准备发布了 | [发布指南](docs/PUBLISHING.md) |

---

## 📚 文档体系

完整的四篇文档，覆盖从入门到发布的全流程：

| 文档 | 适合谁 | 主要内容 |
|------|--------|----------|
| [开发指南](docs/DEVELOPMENT.md) | 所有开发者 | 四阶段工作流、最佳实践、VS Code 配置 |
| [格式规范](docs/SPEC.md) | 想深入了解格式 | 字段说明、命名规范、JSON Schema |
| [调试指南](docs/DEBUGGING.md) | 遇到问题时 | 常见错误排查、完整测试清单 |
| [发布指南](docs/PUBLISHING.md) | 准备发布时 | 版本规范、发布流程、更新日志 |

### 学习路径

1. **入门** → 读 [开发指南](docs/DEVELOPMENT.md)
2. **深入了解格式** → 读 [格式规范](docs/SPEC.md)
3. **遇到问题** → 读 [调试指南](docs/DEBUGGING.md)
4. **准备发布** → 读 [发布指南](docs/PUBLISHING.md)

---

## 📦 示例

| 文件 | 说明 | 适用场景 |
|------|------|----------|
| [template.json](examples/template.json) | 空白模板，含所有字段 | 从零开始 |
| [minimal.json](examples/minimal.json) | 最小可行示例 | 了解结构 |
| [basic-example.json](examples/basic-example.json) | 完整示例，含所有类型 | 学习用法 |

---

## 🛠️ 格式校验

本仓库提供 [JSON Schema](schema/addon.schema.json)，可用于自动化校验拓展包格式。

### VS Code 中使用

**方法一：在 JSON 文件中指定 Schema**

```json
{
  "$schema": "https://raw.githubusercontent.com/NinefCJ/Nexus-Addon-Spec/main/schema/addon.schema.json",
  "id": "your.addon",
  ...
}
```

**方法二：全局配置**

在 VS Code 设置中添加：

```json
{
  "json.schemas": [
    {
      "fileMatch": ["*.addon.json", "addon.json"],
      "url": "https://raw.githubusercontent.com/NinefCJ/Nexus-Addon-Spec/main/schema/addon.schema.json"
    }
  ]
}
```

配置后可以获得：
- ✅ 实时语法校验
- ✅ 字段智能提示
- ✅ 鼠标悬停查看说明

### 在线校验

使用 [JSON Schema Validator](https://www.jsonschemavalidator.net/) 等在线工具。

### 命令行校验

使用 `ajv` 等工具：

```bash
npx ajv-cli validate -s schema/addon.schema.json -d your-addon.json
```

---

## 📁 项目结构

```
Nexus-Addon-Spec/
├── docs/
│   ├── SPEC.md            # 格式规范文档
│   ├── DEVELOPMENT.md     # 开发指南（完整工作流）
│   ├── DEBUGGING.md       # 调试与测试指南
│   └── PUBLISHING.md      # 发布与版本管理
├── examples/
│   ├── template.json      # 空白模板
│   ├── minimal.json       # 最小示例
│   └── basic-example.json # 完整示例
├── schema/
│   └── addon.schema.json  # JSON Schema 验证文件
└── README.md
```

---

## 版本

规范版本：**v1.0.0**

### 版本历史

- **v1.0.0** (2026-06-27)
  - 初始版本
  - 支持方块、物品、音效、粒子、命令、模板
  - 完整的开发、调试、发布文档体系

---

## 贡献

欢迎贡献！你可以通过以下方式参与：

- 📝 改进文档
- 💡 提出新功能建议
- 🐛 报告问题
- ✅ 提交示例拓展包

请在 [Issues](https://github.com/NinefCJ/Nexus-Addon-Spec/issues) 中反馈。

---

## 相关项目

- [Nexus](https://github.com/NinefCJ/Nexus) - Nexus 命令助手主项目
- [Nexus-Addon-Spec](https://github.com/NinefCJ/Nexus-Addon-Spec) - 拓展包规范（本仓库）

---

## 许可证

[MIT License](LICENSE)

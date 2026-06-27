# Nexus 拓展包格式规范

**版本**: v1.0.0  
**最后更新**: 2026-06-27

## 目录

1. [概述](#概述)
2. [文件格式](#文件格式)
3. [根字段说明](#根字段说明)
4. [方块定义](#方块定义)
5. [物品定义](#物品定义)
6. [音效定义](#音效定义)
7. [粒子定义](#粒子定义)
8. [命令定义](#命令定义)
9. [模板定义](#模板定义)
10. [命名规范](#命名规范)
11. [示例](#示例)

---

## 概述

Nexus 拓展包使用 **JSON** 格式定义，用于扩展 Nexus 命令助手的数据与功能。每个拓展包都是一个独立的 JSON 对象，可以包含自定义的方块、物品、音效、粒子、快捷命令和命令模板。

## 文件格式

- **文件类型**: `.json`
- **编码**: UTF-8
- **文件名**: 建议与拓展包 ID 一致，例如 `my_addon.json`
- **大小限制**: 建议不超过 1MB

### 最小可行示例

```json
{
  "id": "example.my_addon",
  "name": "我的拓展包",
  "version": "1.0.0",
  "author": "作者名"
}
```

---

## 根字段说明

| 字段 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| `id` | string | ✅ | - | 拓展包唯一标识符 |
| `name` | string | ✅ | - | 拓展包显示名称 |
| `version` | string | ✅ | - | 语义化版本号 |
| `author` | string | ✅ | - | 作者名称 |
| `description` | string | ❌ | `""` | 拓展包描述 |
| `icon` | string | ❌ | `""` | 图标名称（预留） |
| `blocks` | array | ❌ | `[]` | 自定义方块列表 |
| `items` | array | ❌ | `[]` | 自定义物品列表 |
| `sounds` | array | ❌ | `[]` | 自定义音效列表 |
| `particles` | array | ❌ | `[]` | 自定义粒子列表 |
| `commands` | array | ❌ | `[]` | 快捷命令列表 |
| `templates` | array | ❌ | `[]` | 命令模板列表 |

### 字段详细说明

#### `id`

拓展包的唯一标识符，用于区分不同的拓展包。

- 格式要求：小写字母、数字、下划线，可用 `.` 分隔层级
- 长度：2 - 64 字符
- 推荐使用反向域名格式：`com.yourname.addonname`
- 示例：`com.example.magic_items`, `custom.addon_name`

#### `name`

拓展包的显示名称，将在 UI 中展示。

- 长度：1 - 32 字符

#### `version`

语义化版本号，格式为 `MAJOR.MINOR.PATCH`。

- `MAJOR`: 不兼容的 API 变更
- `MINOR`: 向下兼容的功能性新增
- `PATCH`: 向下兼容的问题修正

示例：`1.0.0`, `2.1.3`

#### `author`

作者名称，将在拓展包详情中显示。

- 长度：1 - 32 字符

#### `description`

拓展包的详细描述，说明拓展包的功能和内容。

- 长度：0 - 200 字符
- 默认值：空字符串

---

## 方块定义

`blocks` 数组中的每个对象代表一个自定义方块。

### 字段说明

| 字段 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| `id` | string | ✅ | - | 方块ID |
| `name` | string | ✅ | - | 方块中文名称 |
| `category` | string | ❌ | `"其他"` | 分类名称 |
| `description` | string | ❌ | `""` | 方块描述 |

### 字段详细说明

#### `id`

方块的唯一标识符，格式为 `命名空间:名称`。

- 命名空间：小写字母、数字、下划线
- 名称：小写字母、数字、下划线
- 示例：`minecraft:stone`, `custom:ruby_block`

#### `category`

方块所属分类，用于在方块库中分组显示。

- 长度：0 - 16 字符
- 推荐分类：建筑、自然、功能、装饰、红石、其他

### 示例

```json
{
  "id": "custom:ruby_block",
  "name": "红宝石块",
  "category": "建筑",
  "description": "由红宝石制成的装饰方块"
}
```

---

## 物品定义

`items` 数组中的每个对象代表一个自定义物品。

### 字段说明

| 字段 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| `id` | string | ✅ | - | 物品ID |
| `name` | string | ✅ | - | 物品中文名称 |
| `category` | string | ❌ | `"其他"` | 分类名称 |
| `description` | string | ❌ | `""` | 物品描述 |

### 字段详细说明

#### `id`

物品的唯一标识符，格式为 `命名空间:名称`。

- 命名空间：小写字母、数字、下划线
- 名称：小写字母、数字、下划线
- 示例：`minecraft:diamond_sword`, `custom:magic_wand`

#### `category`

物品所属分类，用于在物品库中分组显示。

- 长度：0 - 16 字符
- 推荐分类：武器、工具、防具、食物、材料、其他

### 示例

```json
{
  "id": "custom:magic_wand",
  "name": "魔法魔杖",
  "category": "工具",
  "description": "可以释放魔法的魔杖"
}
```

---

## 音效定义

`sounds` 数组中的每个对象代表一个自定义音效。

### 字段说明

| 字段 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| `id` | string | ✅ | - | 音效ID |
| `name` | string | ✅ | - | 音效中文名称 |
| `category` | string | ❌ | `"其他"` | 分类名称 |
| `description` | string | ❌ | `""` | 音效描述 |
| `volume` | string | ❌ | `"1"` | 默认音量 |
| `pitch` | string | ❌ | `"1"` | 默认音调 |

### 字段详细说明

#### `id`

音效的唯一标识符，格式为 `命名空间:路径`。

- 支持点分隔的路径格式
- 示例：`minecraft:block.note_block.harp`, `custom:magic_chime`

#### `volume`

音效的默认音量，用于 `/playsound` 命令生成。

- 字符串类型，值为数字
- 范围：0.0 - 100.0
- 默认值：`"1"`

#### `pitch`

音效的默认音调，用于 `/playsound` 命令生成。

- 字符串类型，值为数字
- 范围：0.0 - 2.0
- 默认值：`"1"`

### 示例

```json
{
  "id": "custom:magic_chime",
  "name": "魔法铃声",
  "category": "魔法",
  "description": "神奇的魔法音效",
  "volume": "1",
  "pitch": "1.2"
}
```

---

## 粒子定义

`particles` 数组中的每个对象代表一个自定义粒子。

### 字段说明

| 字段 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| `id` | string | ✅ | - | 粒子ID |
| `name` | string | ✅ | - | 粒子中文名称 |
| `category` | string | ❌ | `"其他"` | 分类名称 |
| `description` | string | ❌ | `""` | 粒子描述 |
| `hasColor` | boolean | ❌ | `true` | 是否支持颜色参数 |

### 字段详细说明

#### `id`

粒子的唯一标识符，格式为 `命名空间:名称`。

- 命名空间：小写字母、数字、下划线
- 名称：小写字母、数字、下划线
- 示例：`minecraft:heart`, `custom:star_sparkle`

#### `hasColor`

指示该粒子是否支持颜色参数（用于 `/particle` 命令生成）。

- `true`: 支持自定义颜色，生成命令时会包含颜色参数
- `false`: 不支持颜色参数

### 示例

```json
{
  "id": "custom:star_sparkle",
  "name": "星星闪光",
  "category": "魔法",
  "description": "闪亮的星星粒子效果",
  "hasColor": true
}
```

---

## 命令定义

`commands` 数组中的每个对象代表一个快捷命令，将出现在命令库中。

### 字段说明

| 字段 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| `id` | string | ✅ | - | 命令唯一ID |
| `command` | string | ✅ | - | 命令内容 |
| `name` | string | ✅ | - | 命令显示名称 |
| `description` | string | ❌ | `""` | 命令描述 |
| `category` | string | ❌ | `"其他"` | 分类名称 |

### 字段详细说明

#### `id`

命令的唯一标识符，格式为 `命名空间:名称`。

- 命名空间：小写字母、数字、下划线
- 名称：小写字母、数字、下划线
- 长度：3 - 64 字符
- 示例：`custom:fly_cmd`, `rs:clock`

#### `command`

命令的完整内容，必须以 `/` 开头。

- 必须以 `/` 开头
- 长度：2 - 500 字符
- 支持相对坐标 `~` `^`
- 支持目标选择器 `@s` `@p` `@a` `@e` `@r`

#### `category`

命令所属分类，用于在命令库中分组显示。

- 长度：0 - 16 字符
- 推荐分类：实用、建筑、管理、娱乐、红石、其他

### 示例

```json
{
  "id": "custom:fly_cmd",
  "command": "/effect @s levitation 10 1 true",
  "name": "飞行效果",
  "description": "给玩家10秒的漂浮效果",
  "category": "实用"
}
```

---

## 模板定义

`templates` 数组中的每个对象代表一个命令模板，将出现在模板生成器中。

模板的字段结构与 `commands` 完全相同，区别在于：
- `commands` 出现在命令库中，供快速复制使用
- `templates` 出现在模板生成器中，提供参数化编辑功能

### 示例

```json
{
  "id": "custom:house_tpl",
  "command": "/fill ~-5 ~ ~-5 ~5 ~5 ~5 stone hollow",
  "name": "快速造房",
  "description": "快速生成一个石屋框架",
  "category": "建筑"
}
```

---

## 命名规范

### ID 命名规范

所有 ID 字段都应遵循以下规范：

1. **全小写**: 使用小写字母、数字和下划线
2. **命名空间**: 使用 `命名空间:名称` 格式
3. **反向域名**: 推荐使用反向域名格式，如 `com.yourname.addon_name`
4. **禁止前缀**: 禁止使用 `minecraft:` 命名空间（可能与原版冲突）

### 推荐的命名空间

| 命名空间 | 用途 |
|----------|------|
| `minecraft` | 原版内容（禁止在自定义拓展包中使用） |
| `custom` | 通用自定义内容 |
| 个人/团队名 | 个人或团队开发的拓展包 |

### 分类命名建议

为了保持一致性，建议使用以下分类名称：

| 类型 | 推荐分类 |
|------|----------|
| 方块 | 建筑、自然、功能、装饰、红石、其他 |
| 物品 | 武器、工具、防具、食物、材料、其他 |
| 音效 | 环境、互动、敌对、友好、音乐、其他 |
| 粒子 | 效果、环境、魔法、红石、装饰、其他 |
| 命令 | 实用、建筑、管理、娱乐、红石、其他 |

---

## 示例

完整的拓展包示例请参考 [examples/basic-example.json](../examples/basic-example.json)。

```json
{
  "id": "com.example.magic_addon",
  "name": "魔法拓展包",
  "description": "添加各种魔法相关的物品和命令",
  "version": "1.0.0",
  "author": "魔法大师",
  "blocks": [
    {
      "id": "magic:crystal_block",
      "name": "水晶方块",
      "category": "建筑",
      "description": "闪闪发光的魔法水晶"
    }
  ],
  "items": [
    {
      "id": "magic:fire_staff",
      "name": "火焰法杖",
      "category": "武器",
      "description": "释放火球的法杖"
    }
  ],
  "commands": [
    {
      "id": "magic:heal",
      "command": "/effect @s instant_health 1 2",
      "name": "治疗术",
      "description": "立即恢复生命值",
      "category": "实用"
    }
  ]
}
```

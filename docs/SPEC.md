# Nexus 拓展包格式规范

**版本**: v2.0.0  
**最后更新**: 2026-06-30  
**适用**: Nexus v1.2.0+

---

## 目录

1. [概述](#概述)
2. [拓展包类型](#拓展包类型)
3. [文件格式](#文件格式)
4. [根字段说明](#根字段说明)
5. [方块定义](#方块定义)
6. [物品定义](#物品定义)
7. [音效定义](#音效定义)
8. [粒子定义](#粒子定义)
9. [动画定义](#动画定义)
10. [命令定义](#命令定义)
11. [模板定义](#模板定义)
12. [命令库拓展 (idlist + enums)](#命令库拓展)
13. [方块状态拓展](#方块状态拓展)
14. [JSON Schema 拓展](#json-schema-拓展)
15. [命名规范](#命名规范)
16. [最佳实践](#最佳实践)
17. [完整示例](#完整示例)

---

## 概述

Nexus 拓展包使用 **JSON** 格式定义，用于扩展 Nexus 命令助手的数据与功能。每个拓展包都是一个独立的 JSON 对象，根据类型不同可以包含不同的内容。

### 支持的拓展类型

| 类型 | 说明 | 适用场景 |
|------|------|----------|
| 简易拓展包 (simple) | 最基础的拓展包类型 | 添加自定义 ID、翻译、快捷命令 |
| 命令库拓展 (commandLib) | 提供完整的命令库和枚举数据 | 为模组/服务器添加完整命令支持 |
| 方块状态拓展 (blockStates) | 提供方块状态定义 | 为自定义方块添加状态提示 |
| JSON Schema 拓展 (jsonSchema) | 提供 JSON Schema 校验 | 为 NBT/JSON 命令提供智能提示 |
| 完整拓展包 (full) | 包含所有类型的拓展 | 大型模组的完整支持 |

### 兼容性说明

- v2.0.0 规范 **向下兼容** v1.x 版本的所有拓展包
- 新增字段均为可选，旧拓展包无需修改即可使用
- Nexus 应用会自动检测并兼容不同版本的拓展包

---

## 拓展包类型

### 1. 简易拓展包 (Simple Addon)

最基础的拓展包类型，用于添加自定义的 ID 翻译和快捷命令。

**适用场景**:
- 添加模组的方块/物品 ID 翻译
- 分享常用的快捷命令
- 自定义资源库内容

**核心字段**: `blocks`, `items`, `sounds`, `particles`, `animations`, `commands`, `templates`

### 2. 命令库拓展 (Command Library)

提供完整的命令定义和枚举数据，用于为模组或自定义服务器添加完整的命令支持。

**适用场景**:
- 为模组添加自定义命令
- 自定义服务器插件命令支持
- 完整的命令语法补全

**核心字段**: `idlist`, `enums`

### 3. 方块状态拓展 (Block States)

提供方块状态定义，用于 `/setblock`、`/fill` 等命令的方块状态提示。

**适用场景**:
- 为自定义方块添加状态提示
- 模组方块状态补全
- 命令方块状态智能提示

**核心字段**: `states.block`

### 4. JSON Schema 拓展 (JSON Schema)

提供 JSON Schema 定义，用于 NBT 数据、RawText 文本组件等 JSON 格式的智能提示。

**适用场景**:
- NBT 数据结构补全
- RawText 文本组件提示
- 自定义 JSON 格式校验

**核心字段**: `jsonSchema`

### 5. 完整拓展包 (Full Addon)

包含所有类型的完整拓展包，通常用于大型模组的完整支持。

**适用场景**:
- 大型模组的完整 Nexus 支持
- 自定义服务器的全面适配
- 多功能综合拓展

---

## 文件格式

### 基本要求

| 属性 | 值 | 说明 |
|------|-----|------|
| 文件类型 | `.json` | JSON 格式文件 |
| 编码 | UTF-8 | 必须使用 UTF-8 编码 |
| 文件名 | 建议与 ID 一致 | 如 `my_addon.json` |
| 大小限制 | 建议 ≤ 5MB | 过大可能影响加载性能 |

### 最小可行示例

```json
{
  "id": "example.my_addon",
  "name": "我的拓展包",
  "version": "1.0.0",
  "author": "作者名"
}
```

### 添加 Schema 引用

在 JSON 文件开头添加 `$schema` 字段，可以在编辑器中获得智能提示和实时校验：

```json
{
  "$schema": "https://raw.githubusercontent.com/NinefCJ/Nexus-Addon-Spec/main/schema/addon.schema.json",
  "id": "example.my_addon",
  ...
}
```

---

## 根字段说明

### 字段总览

| 字段 | 类型 | 必填 | 默认值 | 类型支持 | 说明 |
|------|------|------|--------|----------|------|
| `$schema` | string | ❌ | - | 所有 | JSON Schema 引用地址 |
| `$注释` | string | ❌ | - | 所有 | 开发者备注，不会被解析 |
| `id` | string | ❌* | - | 所有 | 拓展包唯一标识符 |
| `uuid` | string | ❌ | - | 高级 | UUID 唯一标识符 |
| `name` | string | ✅ | - | 所有 | 拓展包显示名称 |
| `version` | string/array | ✅ | - | 所有 | 语义化版本号 |
| `author` | string | ✅ | - | 所有 | 作者名称 |
| `description` | string | ❌ | `""` | 所有 | 拓展包描述 |
| `icon` | string | ❌ | `""` | 所有 | 图标（预留） |
| `require` | array | ❌ | `[]` | 高级 | 依赖的其他拓展包 |
| `packType` | string | ❌ | `"simple"` | 所有 | 拓展包类型 |
| `idlist` | array | ❌ | - | commandLib | ID 列表分类定义 |
| `enums` | object | ❌ | - | commandLib | 枚举值定义 |
| `jsonSchema` | object | ❌ | - | jsonSchema | JSON Schema 集合 |
| `states` | object | ❌ | - | blockStates | 方块状态定义 |
| `blocks` | array | ❌ | `[]` | 所有 | 自定义方块列表 |
| `items` | array | ❌ | `[]` | 所有 | 自定义物品列表 |
| `sounds` | array | ❌ | `[]` | 所有 | 自定义音效列表 |
| `particles` | array | ❌ | `[]` | 所有 | 自定义粒子列表 |
| `animations` | array | ❌ | `[]` | 所有 | 自定义动画列表 |
| `commands` | array | ❌ | `[]` | 所有 | 快捷命令列表 |
| `templates` | array | ❌ | `[]` | 所有 | 命令模板列表 |

> *`id` 字段在简易拓展包中可选，但强烈建议提供。命令库类型拓展包推荐使用 `uuid`。

---

### 字段详细说明

#### `id`

拓展包的唯一标识符，用于区分不同的拓展包。

- **格式要求**: 小写字母、数字、下划线，可用 `.` 分隔层级
- **长度**: 2 - 64 字符
- **推荐格式**: 反向域名格式 `com.yourname.addonname`
- **示例**: `com.example.magic_items`, `custom.addon_name`
- **注意**: 
  - 不能以数字开头
  - 不能包含大写字母
  - 建议使用与你的项目/域名相关的命名空间

#### `uuid`

拓展包的 UUID 唯一标识符，用于高级拓展包的依赖管理。

- **格式**: 标准 UUID v4 格式
- **示例**: `a1471826-7086-022e-cd02-8a5a317e825a`
- **生成方式**:
  - 在线工具: https://www.uuidgenerator.net/
  - 命令行: `uuidgen`
  - VS Code 插件: UUID Generator

#### `name`

拓展包的显示名称，将在 Nexus UI 中展示。

- **长度**: 1 - 64 字符
- **建议**: 简洁明了，能够体现拓展包的功能

#### `version`

语义化版本号，支持两种格式：

**字符串格式**（推荐简易拓展包使用）:
- 格式: `MAJOR.MINOR.PATCH`
- 示例: `"1.0.0"`, `"2.1.3"`

**数组格式**（推荐命令库类型使用）:
- 格式: `[MAJOR, MINOR, PATCH]`
- 示例: `[1, 0, 0]`, `[2, 1, 3]`

**版本号规则**:
- `MAJOR`: 不兼容的 API 变更（删除字段、修改字段含义）
- `MINOR`: 向下兼容的功能性新增（添加新字段、新类型）
- `PATCH`: 向下兼容的问题修正（修复错误、补充数据）

#### `author`

作者名称，将在拓展包详情中显示。

- **长度**: 1 - 64 字符
- **建议**: 使用你的 GitHub 用户名或团队名称

#### `description`

拓展包的详细描述，说明拓展包的功能和内容。

- **长度**: 0 - 1000 字符
- **建议**: 
  - 简述拓展包提供的功能
  - 说明适用的游戏版本
  - 列出主要特性

#### `require`

依赖的其他拓展包列表，当前拓展包需要这些拓展包先加载才能正常工作。

- **类型**: 字符串数组
- **值类型**: 拓展包的 UUID 或 ID
- **示例**:
  ```json
  "require": [
    "a1471826-7086-022e-cd02-8a5a317e825a",
    "com.example.base_addon"
  ]
  ```

#### `packType`

拓展包类型，用于告诉 Nexus 应用如何加载和使用这个拓展包。

- **可选值**: `simple`, `commandLib`, `blockStates`, `jsonSchema`, `full`
- **默认值**: `simple`

---

## 方块定义

`blocks` 数组中的每个对象代表一个自定义方块。

### 字段说明

| 字段 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| `id` | string | ✅ | - | 方块 ID |
| `name` | string | ✅ | - | 方块中文名称 |
| `category` | string | ❌ | `"其他"` | 分类名称 |
| `description` | string | ❌ | `""` | 方块描述 |
| `hardness` | number | ❌ | - | 硬度值 |
| `resistance` | number | ❌ | - | 爆炸抗性 |
| `lightLevel` | integer | ❌ | - | 光照等级 (0-15) |
| `tags` | array | ❌ | - | 标签列表 |

### 字段详细说明

#### `id`

方块的唯一标识符，格式为 `命名空间:名称`。

- **命名空间**: 小写字母、数字、下划线
- **名称**: 小写字母、数字、下划线、斜杠
- **示例**: `minecraft:stone`, `custom:ruby_block`, `my_mod:ores/ruby_ore`

#### `category`

方块所属分类，用于在方块库中分组显示。

- **长度**: 0 - 32 字符
- **推荐分类**: 建筑、自然、功能、装饰、红石、其他

**推荐分类详解**:

| 分类 | 适用方块 | 示例 |
|------|---------|------|
| 建筑 | 建筑用方块 | 石头、木板、砖块、玻璃 |
| 自然 | 自然生成的方块 | 泥土、草方块、矿石、树叶 |
| 功能 | 有特殊功能的方块 | 箱子、熔炉、命令方块、信标 |
| 装饰 | 装饰性方块 | 地毯、花盆、画、物品展示框 |
| 红石 | 红石相关方块 | 红石粉、中继器、比较器、活塞 |
| 其他 | 不属于以上分类 | - |

#### `hardness`

方块的硬度值，影响挖掘时间。

- **类型**: 数字
- **常见值**:
  - 0.0: 即时破坏（火把、花）
  - 0.5: 泥土、沙子
  - 1.5: 石头
  - 2.0: 深板岩
  - 3.0: 黑曜石
  - -1.0: 不可破坏（基岩）

#### `lightLevel`

方块的光照等级，决定方块发出的光的强度。

- **类型**: 整数
- **范围**: 0 - 15
- **常见值**:
  - 0: 大多数方块（不发光）
  - 7: 火把、灯笼
  - 15: 萤石、信标、熔岩

### 示例

```json
{
  "id": "custom:ruby_block",
  "name": "红宝石块",
  "category": "建筑",
  "description": "由红宝石制成的装饰方块，闪耀着红色的光芒",
  "hardness": 5.0,
  "resistance": 6.0,
  "lightLevel": 8,
  "tags": ["ruby", "gem", "decorative"]
}
```

---

## 物品定义

`items` 数组中的每个对象代表一个自定义物品。

### 字段说明

| 字段 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| `id` | string | ✅ | - | 物品 ID |
| `name` | string | ✅ | - | 物品中文名称 |
| `category` | string | ❌ | `"其他"` | 分类名称 |
| `description` | string | ❌ | `""` | 物品描述 |
| `maxStack` | integer | ❌ | - | 最大堆叠数 |
| `rarity` | string | ❌ | - | 稀有度 |
| `damage` | integer | ❌ | - | 耐久度/攻击力 |
| `tags` | array | ❌ | - | 标签列表 |

### 字段详细说明

#### `id`

物品的唯一标识符，格式为 `命名空间:名称`。

- **命名空间**: 小写字母、数字、下划线
- **名称**: 小写字母、数字、下划线、斜杠
- **示例**: `minecraft:diamond_sword`, `custom:magic_wand`, `my_mod:tools/fire_staff`

#### `category`

物品所属分类，用于在物品库中分组显示。

- **长度**: 0 - 32 字符
- **推荐分类**: 武器、工具、防具、食物、材料、其他

**推荐分类详解**:

| 分类 | 适用物品 | 示例 |
|------|---------|------|
| 武器 | 攻击性物品 | 剑、斧、弓、三叉戟 |
| 工具 | 功能性工具 | 镐、铲、斧、锄头、钓鱼竿 |
| 防具 | 可穿戴防具 | 头盔、胸甲、护腿、靴子 |
| 食物 | 可食用物品 | 牛排、面包、苹果、药水 |
| 材料 | 合成材料 | 钻石、铁锭、木棍、红石 |
| 其他 | 不属于以上分类 | - |

#### `maxStack`

物品的最大堆叠数量。

- **类型**: 整数
- **范围**: 1 - 64
- **常见值**:
  - 1: 工具、武器、防具（不可堆叠）
  - 16: 雪球、鸡蛋、末影珍珠
  - 64: 大多数物品

#### `rarity`

物品的稀有度，影响物品名称的颜色。

- **可选值**: `common`, `uncommon`, `rare`, `epic`
- **对应颜色**:
  - `common`: 白色（普通）
  - `uncommon`: 黄色（罕见）
  - `rare`: 青色（稀有）
  - `epic`: 紫色（史诗）

### 示例

```json
{
  "id": "custom:magic_wand",
  "name": "魔法魔杖",
  "category": "工具",
  "description": "可以释放魔法的神秘魔杖，需要魔力才能使用",
  "maxStack": 1,
  "rarity": "rare",
  "damage": 50,
  "tags": ["magic", "wand", "tool"]
}
```

---

## 音效定义

`sounds` 数组中的每个对象代表一个自定义音效。

### 字段说明

| 字段 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| `id` | string | ✅ | - | 音效 ID |
| `name` | string | ✅ | - | 音效中文名称 |
| `category` | string | ❌ | `"其他"` | 分类名称 |
| `description` | string | ❌ | `""` | 音效描述 |
| `volume` | string | ❌ | `"1"` | 默认音量 |
| `pitch` | string | ❌ | `"1"` | 默认音调 |
| `minVolume` | string | ❌ | `"0"` | 最小音量 |
| `soundCategory` | string | ❌ | - | 游戏内声音分类 |
| `tags` | array | ❌ | - | 标签列表 |

### 字段详细说明

#### `id`

音效的唯一标识符，格式为 `命名空间:路径`。

- 支持点分隔的路径格式
- **示例**: `minecraft:block.note_block.harp`, `custom:magic_chime`, `my_mod:ui.button_click`

#### `volume`

音效的默认音量，用于 `/playsound` 命令生成。

- **类型**: 字符串（数字形式）
- **范围**: 0.0 - 1000.0
- **默认值**: `"1"`
- **说明**: 
  - 1.0 为正常音量
  - 大于 1.0 时，可以在更远距离听到
  - 0.0 为静音

#### `pitch`

音效的默认音调，用于 `/playsound` 命令生成。

- **类型**: 字符串（数字形式）
- **范围**: 0.0 - 2.0
- **默认值**: `"1"`
- **说明**:
  - 1.0 为正常音调
  - 小于 1.0 为低沉音调
  - 大于 1.0 为尖锐音调

#### `soundCategory`

音效所属的游戏内声音分类，影响玩家的声音设置。

- **可选值**: `master`, `music`, `record`, `weather`, `block`, `hostile`, `friendly`, `player`, `ambient`, `voice`
- **说明**:
  - `master`: 主音量
  - `music`: 音乐
  - `record`: 唱片
  - `weather`: 天气
  - `block`: 方块
  - `hostile`: 敌对生物
  - `friendly`: 友好生物
  - `player`: 玩家
  - `ambient`: 环境
  - `voice`: 语音

### 示例

```json
{
  "id": "custom:magic_chime",
  "name": "魔法铃声",
  "category": "魔法",
  "description": "神奇的魔法音效，仿佛水晶在轻轻碰撞",
  "volume": "1",
  "pitch": "1.2",
  "minVolume": "0.5",
  "soundCategory": "player",
  "tags": ["magic", "chime", "ui"]
}
```

---

## 粒子定义

`particles` 数组中的每个对象代表一个自定义粒子。

### 字段说明

| 字段 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| `id` | string | ✅ | - | 粒子 ID |
| `name` | string | ✅ | - | 粒子中文名称 |
| `category` | string | ❌ | `"其他"` | 分类名称 |
| `description` | string | ❌ | `""` | 粒子描述 |
| `hasColor` | boolean | ❌ | `true` | 是否支持颜色参数 |
| `params` | array | ❌ | - | 粒子参数列表 |
| `tags` | array | ❌ | - | 标签列表 |

### 字段详细说明

#### `id`

粒子的唯一标识符，格式为 `命名空间:名称`。

- **命名空间**: 小写字母、数字、下划线
- **名称**: 小写字母、数字、下划线、斜杠
- **示例**: `minecraft:heart`, `custom:star_sparkle`, `my_mod:magic/sparkle`

#### `hasColor`

指示该粒子是否支持颜色参数（用于 `/particle` 命令生成）。

- `true`: 支持自定义颜色，生成命令时会包含颜色参数
- `false`: 不支持颜色参数

#### `params`

粒子的自定义参数列表，用于高级粒子效果。

每个参数对象包含：

| 字段 | 类型 | 说明 |
|------|------|------|
| `name` | string | 参数名称 |
| `type` | string | 参数类型：`int`, `float`, `string`, `vec3`, `color` |
| `default` | string | 默认值 |
| `description` | string | 参数描述 |

**参数类型说明**:
- `int`: 整数
- `float`: 浮点数
- `string`: 字符串
- `vec3`: 三维向量（x y z）
- `color`: 颜色（r g b）

### 示例

```json
{
  "id": "custom:star_sparkle",
  "name": "星星闪光",
  "category": "魔法",
  "description": "闪亮的星星粒子效果，闪烁着梦幻的光芒",
  "hasColor": true,
  "params": [
    {
      "name": "size",
      "type": "float",
      "default": "1.0",
      "description": "星星大小"
    },
    {
      "name": "speed",
      "type": "float",
      "default": "0.5",
      "description": "闪烁速度"
    }
  ],
  "tags": ["magic", "sparkle", "decorative"]
}
```

---

## 动画定义

`animations` 数组中的每个对象代表一个自定义动画。

### 字段说明

| 字段 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| `id` | string | ✅ | - | 动画 ID |
| `name` | string | ✅ | - | 动画中文名称 |
| `category` | string | ❌ | `"其他"` | 分类名称 |
| `description` | string | ❌ | `""` | 动画描述 |
| `targetType` | string | ❌ | `"entity"` | 动画作用目标类型 |
| `defaultController` | string | ❌ | `""` | 默认动画控制器 |
| `duration` | number | ❌ | - | 动画持续时间（秒） |
| `blendOutTime` | string | ❌ | `"0.2"` | 默认淡出时间 |
| `nextState` | string | ❌ | `""` | 动画结束后的下一个状态 |
| `stopExpression` | string | ❌ | `""` | 停止表达式 |
| `tags` | array | ❌ | - | 标签列表 |

### 字段详细说明

#### `id`

动画的唯一标识符，格式为 `命名空间:路径`。

- **命名空间**: 小写字母、数字、下划线
- **路径**: 小写字母、数字、下划线、点、斜杠
- **示例**: `minecraft:player.move`, `custom:magic_spell_cast`, `my_mod:entities/dragon/fly`

#### `category`

动画所属分类，用于在动画库中分组显示。

- **长度**: 0 - 32 字符
- **推荐分类**: 玩家、生物通用、攻击动作、日常动作、表情动作、载具、方块实体、特殊效果、其他

**推荐分类详解**:

| 分类 | 适用动画 | 示例 |
|------|---------|------|
| 玩家 | 玩家特有动画 | 待机、行走、跑步、跳跃、攻击 |
| 生物通用 | 大多数生物通用 | 死亡、受伤、出生、睡觉 |
| 攻击动作 | 攻击相关动画 | 挥砍、射击、法术施放、冲刺攻击 |
| 日常动作 | 日常行为动画 | 吃东西、走路、游泳、飞行 |
| 表情动作 | 表情和姿态 | 挥手、点头、摇头、庆祝 |
| 载具 | 载具相关动画 | 船划动、矿车移动、骑马 |
| 方块实体 | 方块实体动画 | 箱子开关、旗帜飘动、齿轮转动 |
| 特殊效果 | 特殊效果动画 | 变身、传送、召唤、爆炸 |
| 其他 | 不属于以上分类 | - |

#### `targetType`

动画作用的目标类型。

- **可选值**: `player`, `entity`, `block`, `all`
  - `player`: 仅玩家可用
  - `entity`: 实体（生物、玩家等）
  - `block`: 方块实体
  - `all`: 所有类型

#### `defaultController`

默认的动画控制器名称，用于生成 `/playanimation` 命令时的默认值。

- **示例**: `"controller.animation.player.move"`, `"controller.animation.generic.look_at_target"`

#### `duration`

动画的持续时间，以秒为单位。

- **类型**: 数字
- **示例**: `0.5`（0.5秒）, `2.0`（2秒）, `10.0`（10秒）

#### `blendOutTime`

动画的默认淡出时间，用于平滑过渡到下一个动画。

- **类型**: 字符串（数字形式）
- **默认值**: `"0.2"`
- **说明**: 值越大，淡出越平滑

#### `nextState`

动画结束后自动切换到的下一个状态。

- **类型**: 字符串
- **示例**: `"default"`, `"idle"`, `"walk"`

#### `stopExpression`

用于判断动画是否应该停止的 Molang 表达式。

- **类型**: 字符串
- **示例**:
  - `"!query.is_alive"` - 生物死亡时停止
  - `"query.on_ground"` - 触地时停止
  - `"variable.timer <= 0"` - 计时结束时停止

### 示例

```json
{
  "id": "custom:magic_spell_cast",
  "name": "施法动作",
  "category": "攻击动作",
  "description": "法师施法时的动作，抬手吟唱并释放魔法",
  "targetType": "entity",
  "defaultController": "controller.animation.generic.attack",
  "duration": 1.5,
  "blendOutTime": "0.3",
  "nextState": "idle",
  "stopExpression": "!query.is_alive",
  "tags": ["magic", "attack", "spell"]
}
```

---

## 命令定义

`commands` 数组中的每个对象代表一个快捷命令，将出现在命令库中。

### 字段说明

| 字段 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| `id` | string | ✅ | - | 命令唯一 ID |
| `command` | string | ✅ | - | 命令内容 |
| `name` | string | ✅ | - | 命令显示名称 |
| `description` | string | ❌ | `""` | 命令描述 |
| `category` | string | ❌ | `"其他"` | 分类名称 |
| `icon` | string | ❌ | `""` | 图标（预留） |
| `permissions` | array | ❌ | - | 所需权限 |
| `tags` | array | ❌ | - | 标签列表 |

### 字段详细说明

#### `id`

命令的唯一标识符，格式为 `命名空间:名称`。

- **命名空间**: 小写字母、数字、下划线
- **名称**: 小写字母、数字、下划线
- **长度**: 3 - 64 字符
- **示例**: `custom:fly_cmd`, `rs:clock`, `my_mod:teleport_spawn`

#### `command`

命令的完整内容，必须以 `/` 开头。

- **必须以 `/` 开头**
- **长度**: 2 - 2000 字符
- **支持的语法**:
  - 相对坐标: `~`, `~5`, `~-3`
  - 局部坐标: `^`, `^1`, `^-2`
  - 目标选择器: `@s`, `@p`, `@a`, `@e`, `@r`
  - 选择器参数: `@e[type=creeper,r=10]`
  - NBT 数据: `{Health:20f}`
  - JSON 文本: `{"rawtext":[{"text":"Hello"}]}`

#### `category`

命令所属分类，用于在命令库中分组显示。

- **长度**: 0 - 32 字符
- **推荐分类**: 实用、建筑、管理、娱乐、红石、其他

**推荐分类详解**:

| 分类 | 适用命令 | 示例 |
|------|---------|------|
| 实用 | 常用实用命令 | 传送、治疗、给予物品 |
| 建筑 | 建筑相关命令 | 填充、克隆、设置方块 |
| 管理 | 服务器管理 | 改模式、踢人、白名单 |
| 娱乐 | 娱乐趣味命令 | 召唤生物、播放音效、粒子效果 |
| 红石 | 红石相关 | 命令方块、红石块、检测 |
| 其他 | 不属于以上分类 | - |

### 命令编写最佳实践

1. **使用目标选择器而非固定玩家名**
   - ✅ `@s` (执行者自身)
   - ✅ `@p` (最近的玩家)
   - ✅ `@a` (所有玩家)
   - ❌ `Steve` (固定玩家名)

2. **使用相对坐标而非绝对坐标**
   - ✅ `~ ~ ~` (当前位置)
   - ✅ `~ ~1 ~` (上方一格)
   - ❌ `100 64 200` (固定坐标)

3. **简洁明了**
   - 命令名称简短且描述准确
   - 描述说明命令的用途和效果

### 示例

```json
{
  "id": "custom:fly_cmd",
  "command": "/effect @s levitation 10 1 true",
  "name": "飞行效果",
  "description": "给玩家10秒的漂浮效果，可以短暂飞行",
  "category": "实用",
  "permissions": ["operator"],
  "tags": ["flight", "effect", "utility"]
}
```

---

## 模板定义

`templates` 数组中的每个对象代表一个命令模板，将出现在模板生成器中。

模板的字段结构与 `commands` 完全相同，区别在于：

- **`commands`**: 出现在命令库中，供快速复制使用
- **`templates`**: 出现在模板生成器中，提供参数化编辑功能

### 使用场景

- 命令中有需要用户自定义的参数
- 复杂命令的分步构建
- 提供参数提示和默认值

### 示例

```json
{
  "id": "custom:house_tpl",
  "command": "/fill ~-5 ~ ~-5 ~5 ~5 ~5 stone hollow",
  "name": "快速造房",
  "description": "快速生成一个石屋框架，可以自定义大小和材料",
  "category": "建筑"
}
```

---

## 命令库拓展

命令库类型拓展包（`packType: "commandLib"`）用于提供完整的命令库和枚举数据。

### 核心概念

#### `idlist` - ID 列表分类

定义了枚举数据的分类展示，用于在 UI 中分组显示枚举类型。

每个 `idlist` 项包含：

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `name` | string | ✅ | 分类显示名称（中文） |
| `list` | string | ✅ | 对应的 `enums` 键名 |

**示例**:
```json
"idlist": [
  { "name": "方块", "list": "block" },
  { "name": "物品", "list": "item" },
  { "name": "实体", "list": "entity" },
  { "name": "效果", "list": "effect" }
]
```

#### `enums` - 枚举值定义

具体的枚举数据，键为枚举名（与 `idlist` 中的 `list` 对应），值为 ID-中文名称 的映射对象。

**格式**:
```json
"enums": {
  "block": {
    "minecraft:stone": "石头",
    "minecraft:dirt": "泥土",
    "minecraft:grass_block": "草方块"
  },
  "item": {
    "minecraft:diamond_sword": "钻石剑",
    "minecraft:apple": "苹果"
  }
}
```

### 支持的枚举类型

| 枚举名 | 说明 | 示例 ID |
|--------|------|---------|
| `block` | 方块 | `minecraft:stone` |
| `item` | 物品 | `minecraft:diamond_sword` |
| `entity` | 实体 | `minecraft:creeper` |
| `effect` | 状态效果 | `minecraft:speed` |
| `enchant` | 魔咒 | `minecraft:sharpness` |
| `biome` | 生物群系 | `minecraft:plains` |
| `gamerule` | 游戏规则 | `commandBlockOutput` |
| `sound` | 音效 | `minecraft:block.stone.break` |
| `particle` | 粒子 | `minecraft:heart` |
| `animation` | 动画 | `minecraft:player.move` |
| `fog` | 迷雾 | `minecraft:fog_basalt_deltas` |
| `structure` | 结构 | `minecraft:pillager_outpost` |
| `recipe` | 配方 | `minecraft:diamond_sword` |
| `damageCause` | 伤害类型 | `override` |
| `entityEvent` | 实体事件 | `minecraft:from_wandering_trader` |
| `entityFamily` | 实体族 | `mob` |
| `cameraPreset` | 摄像机预设 | `minecraft:free` |
| `hudElement` | HUD 界面元素 | `paper_doll` |
| `feature` | 地物 | `minecraft:tree` |
| `featureRule` | 地物规则 | `minecraft:oak_tree` |
| `lootTable` | 战利品表 | `minecraft:chests/simple_dungeon` |
| `entitySlot` | 实体槽位类型 | `slot.armor.head` |
| `inputPermission` | 操作输入权限 | `camera.enabled` |
| `animationController` | 动画控制器 | `controller.animation.player.move` |
| `particleEmitter` | 粒子发射器 | `minecraft:basic_flame_particle` |

### 完整示例

```json
{
  "$schema": "https://raw.githubusercontent.com/NinefCJ/Nexus-Addon-Spec/main/schema/addon.schema.json",
  "name": "示例命令库",
  "author": "示例作者",
  "version": [1, 0, 0],
  "uuid": "a1471826-7086-022e-cd02-8a5a317e825a",
  "description": "示例命令库拓展包，演示 idlist 和 enums 的用法",
  "packType": "commandLib",
  "idlist": [
    { "name": "方块", "list": "block" },
    { "name": "物品", "list": "item" },
    { "name": "效果", "list": "effect" }
  ],
  "enums": {
    "block": {
      "minecraft:stone": "石头",
      "minecraft:dirt": "泥土",
      "minecraft:grass_block": "草方块",
      "minecraft:oak_planks": "橡木木板",
      "minecraft:cobblestone": "圆石"
    },
    "item": {
      "minecraft:diamond_sword": "钻石剑",
      "minecraft:apple": "苹果",
      "minecraft:diamond_pickaxe": "钻石镐",
      "minecraft:bread": "面包"
    },
    "effect": {
      "minecraft:speed": "速度提升",
      "minecraft:strength": "力量",
      "minecraft:regeneration": "生命恢复",
      "minecraft:night_vision": "夜视"
    }
  }
}
```

---

## 方块状态拓展

方块状态类型拓展包（`packType: "blockStates"`）用于提供方块状态定义，增强 `/setblock`、`/fill` 等命令的智能提示。

### 核心结构

```json
"states": {
  "block": [
    {
      "context": ["方块ID1", "方块ID2"],
      "schema": {
        "状态名": {
          "name": "状态中文名",
          "type": "enum",
          "list": {
            "值1": "值1的中文说明",
            "值2": "值2的中文说明"
          }
        }
      }
    }
  ]
}
```

### 字段说明

#### `context`

适用的方块 ID 列表，定义了哪些方块适用这个状态 Schema。

- **类型**: 字符串数组
- **示例**: `["minecraft:stone_slab", "minecraft:oak_slab"]`

#### `schema`

方块状态的 Schema 定义，键为状态名，值为状态描述对象。

每个状态对象包含：

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `name` | string | ✅ | 状态的中文名称 |
| `type` | string | ✅ | 状态值类型：`enum`, `int`, `bool`, `string` |
| `list` | object | ❌ | 枚举值映射（type 为 enum 时需要） |

### 状态类型

| 类型 | 说明 | 示例 |
|------|------|------|
| `enum` | 枚举值（固定选项） | `{ "true": "开启", "false": "关闭" }` |
| `int` | 整数范围 | - |
| `bool` | 布尔值 | - |
| `string` | 字符串 | - |

### 完整示例

```json
{
  "name": "方块状态包",
  "author": "示例作者",
  "version": [1, 0, 0],
  "uuid": "4145a166-7f9e-4ee6-969e-b3b0582ee94f",
  "description": "方块状态拓展包，提供方块状态的中文提示",
  "packType": "blockStates",
  "states": {
    "block": [
      {
        "context": [
          "minecraft:oak_button",
          "minecraft:stone_button",
          "minecraft:wooden_button"
        ],
        "schema": {
          "button_pressed_bit": {
            "name": "按钮是否按下",
            "type": "enum",
            "list": {
              "false": "[默认] 按钮没有处于按下状态",
              "true": "按钮处于按下状态"
            }
          },
          "facing_direction": {
            "name": "方块朝向",
            "type": "enum",
            "list": {
              "0": "[默认] 方块朝向下方",
              "1": "方块朝向上方",
              "2": "方块朝向北方",
              "3": "方块朝向南方",
              "4": "方块朝向西方",
              "5": "方块朝向东方"
            }
          }
        }
      },
      {
        "context": [
          "minecraft:oak_door",
          "minecraft:iron_door",
          "minecraft:spruce_door"
        ],
        "schema": {
          "open_bit": {
            "name": "门是否已打开",
            "type": "enum",
            "list": {
              "false": "[默认] 门处于关闭状态",
              "true": "门处于打开状态"
            }
          },
          "upper_block_bit": {
            "name": "此方块是门的哪一部分",
            "type": "enum",
            "list": {
              "false": "[默认] 门的下半部分",
              "true": "门的上半部分"
            }
          },
          "direction": {
            "name": "门的朝向",
            "type": "enum",
            "list": {
              "0": "[默认] 南",
              "1": "西",
              "2": "北",
              "3": "东"
            }
          }
        }
      }
    ]
  }
}
```

---

## JSON Schema 拓展

JSON Schema 类型拓展包（`packType: "jsonSchema"`）用于提供 JSON Schema 定义，为 NBT 数据、RawText 文本组件等 JSON 格式提供智能提示和校验。

### 使用场景

- NBT 数据结构补全
- RawText 文本组件提示
- 自定义 JSON 格式校验
- 命令参数的 JSON 语法验证

### 核心结构

```json
"jsonSchema": {
  "schema名称": {
    "title": "Schema 标题",
    "description": "Schema 描述",
    "type": "object",
    "properties": {
      // Schema 定义
    }
  }
}
```

### 支持的 Schema 类型

| Schema 名 | 说明 | 适用命令 |
|-----------|------|---------|
| `rawtext` | RawText 文本组件 | `/tellraw`, `/titleraw`, `/say` |
| `nbt_entity` | 实体 NBT 数据 | `/summon`, `/data` |
| `nbt_block` | 方块 NBT 数据 | `/setblock`, `/fill` |
| `nbt_item` | 物品 NBT 数据 | `/give`, `/replaceitem` |
| `selector` | 选择器参数 | 所有使用选择器的命令 |

### 完整示例

```json
{
  "name": "JSON Schema 包",
  "author": "示例作者",
  "version": [1, 0, 0],
  "uuid": "a1471826-7086-022e-cd02-8a5a317e825a",
  "description": "提供 JSON Schema 验证功能",
  "packType": "jsonSchema",
  "jsonSchema": {
    "rawtext": {
      "title": "Minecraft RawText Schema",
      "description": "用于验证和补全 Minecraft 基岩版的文本组件",
      "type": "object",
      "properties": {
        "rawtext": {
          "type": "array",
          "items": {
            "oneOf": [
              {
                "type": "object",
                "properties": {
                  "text": { "type": "string" }
                },
                "required": ["text"]
              },
              {
                "type": "object",
                "properties": {
                  "translate": { "type": "string" },
                  "with": {
                    "oneOf": [
                      { "type": "array", "items": { "type": "string" } },
                      {
                        "type": "object",
                        "properties": {
                          "rawtext": { "$ref": "#/properties/rawtext" }
                        },
                        "required": ["rawtext"]
                      }
                    ]
                  }
                },
                "required": ["translate"]
              },
              {
                "type": "object",
                "properties": {
                  "score": {
                    "type": "object",
                    "properties": {
                      "name": { "type": "string" },
                      "objective": { "type": "string" }
                    },
                    "required": ["name", "objective"]
                  }
                },
                "required": ["score"]
              },
              {
                "type": "object",
                "properties": {
                  "selector": { "type": "string" }
                },
                "required": ["selector"]
              }
            ]
          }
        }
      },
      "required": ["rawtext"]
    }
  }
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
5. **见名知意**: ID 应该能够反映其用途和内容

### 推荐的命名空间

| 命名空间 | 用途 | 示例 |
|----------|------|------|
| `minecraft` | 原版内容（禁止在自定义拓展包中使用） | `minecraft:stone` |
| `custom` | 通用自定义内容 | `custom:my_block` |
| 个人/团队名 | 个人或团队开发的拓展包 | `alice:magic_items` |
| 模组名 | 对应模组的拓展包 | `my_mod:ruby_ore` |

### 分类命名建议

为了保持一致性，建议使用以下分类名称：

| 类型 | 推荐分类 |
|------|----------|
| 方块 | 建筑、自然、功能、装饰、红石、其他 |
| 物品 | 武器、工具、防具、食物、材料、其他 |
| 音效 | 环境、互动、敌对、友好、音乐、UI、其他 |
| 粒子 | 效果、环境、魔法、红石、装饰、其他 |
| 动画 | 玩家、生物通用、攻击动作、日常动作、表情动作、载具、方块实体、特殊效果、其他 |
| 命令 | 实用、建筑、管理、娱乐、红石、其他 |

### 文件命名建议

- 使用小写字母和下划线
- 与拓展包 ID 保持一致
- 使用 `.json` 后缀
- **示例**: `my_addon.json`, `com_example_magic.json`

---

## 最佳实践

### 开发流程

1. **规划**: 确定拓展包类型和内容范围
2. **编写**: 使用支持 JSON Schema 的编辑器编写
3. **校验**: 验证 JSON 格式和内容正确性
4. **测试**: 在 Nexus 中导入并测试功能
5. **发布**: 分享给他人使用

### 性能优化

1. **合理控制大小**
   - 简易拓展包建议 < 1MB
   - 大型拓展包可以分批制作
   
2. **避免重复数据**
   - 相同内容不要重复定义
   - 利用 `require` 字段建立依赖关系

3. **分类清晰**
   - 使用合理的分类名称
   - 保持分类的一致性

### 质量清单

发布前请检查以下项目：

- [ ] JSON 格式正确，无语法错误
- [ ] 所有必填字段已填写
- [ ] ID 命名规范且唯一
- [ ] 中文名称准确自然
- [ ] 分类合理且一致
- [ ] 描述清晰易懂
- [ ] 命令格式正确
- [ ] 版本号符合语义化规范
- [ ] 已在 Nexus 中测试通过

### 常见错误

| 错误 | 原因 | 解决方法 |
|------|------|---------|
| JSON 解析失败 | 语法错误（缺少逗号、引号等） | 使用 JSON 校验工具检查 |
| ID 格式错误 | 使用了大写字母或特殊字符 | 改为小写字母和下划线 |
| 命令不以 / 开头 | 忘记添加命令前缀 | 在 command 字段前添加 `/` |
| 加载后无内容 | 必填字段缺失或格式错误 | 检查 required 字段是否都有值 |
| 与原版冲突 | 使用了 `minecraft:` 命名空间 | 改用自定义命名空间 |

---

## 完整示例

### 简易拓展包完整示例

```json
{
  "$schema": "https://raw.githubusercontent.com/NinefCJ/Nexus-Addon-Spec/main/schema/addon.schema.json",
  "id": "com.example.magic_addon",
  "name": "魔法拓展包",
  "description": "添加各种魔法相关的方块、物品、音效和命令\n- 红宝石方块和工具\n- 魔法音效和粒子\n- 实用魔法命令",
  "version": "1.0.0",
  "author": "魔法大师",
  "packType": "simple",
  "blocks": [
    {
      "id": "magic:crystal_block",
      "name": "水晶方块",
      "category": "建筑",
      "description": "闪闪发光的魔法水晶方块，散发着柔和的光芒",
      "hardness": 3.0,
      "lightLevel": 7,
      "tags": ["crystal", "magic", "decoration"]
    },
    {
      "id": "magic:ruby_block",
      "name": "红宝石块",
      "category": "建筑",
      "description": "由红宝石制成的装饰方块，闪耀着红色的光芒",
      "hardness": 5.0,
      "lightLevel": 5,
      "tags": ["ruby", "gem", "decorative"]
    }
  ],
  "items": [
    {
      "id": "magic:fire_staff",
      "name": "火焰法杖",
      "category": "武器",
      "description": "可以释放火球的魔法法杖，消耗魔力使用",
      "maxStack": 1,
      "rarity": "rare",
      "damage": 100,
      "tags": ["fire", "magic", "weapon"]
    },
    {
      "id": "magic:magic_wand",
      "name": "魔法魔杖",
      "category": "工具",
      "description": "可以释放魔法的神秘魔杖，需要魔力才能使用",
      "maxStack": 1,
      "rarity": "rare",
      "tags": ["magic", "wand", "tool"]
    }
  ],
  "sounds": [
    {
      "id": "magic:magic_chime",
      "name": "魔法铃声",
      "category": "魔法",
      "description": "神奇的魔法音效，仿佛水晶在轻轻碰撞",
      "volume": "1",
      "pitch": "1.2",
      "soundCategory": "player",
      "tags": ["magic", "chime", "ui"]
    }
  ],
  "particles": [
    {
      "id": "magic:star_sparkle",
      "name": "星星闪光",
      "category": "魔法",
      "description": "闪亮的星星粒子效果，闪烁着梦幻的光芒",
      "hasColor": true,
      "tags": ["magic", "sparkle", "decorative"]
    }
  ],
  "animations": [
    {
      "id": "magic:spell_cast",
      "name": "施法动作",
      "category": "攻击动作",
      "description": "法师施法时的动作，抬手吟唱并释放魔法",
      "targetType": "entity",
      "defaultController": "controller.animation.generic.attack",
      "duration": 1.5,
      "blendOutTime": "0.3",
      "nextState": "idle",
      "tags": ["magic", "attack", "spell"]
    }
  ],
  "commands": [
    {
      "id": "magic:heal",
      "command": "/effect @s instant_health 1 2",
      "name": "治疗术",
      "description": "立即恢复生命值，相当于喝下一瓶瞬间治疗药水",
      "category": "实用",
      "tags": ["heal", "effect", "magic"]
    },
    {
      "id": "magic:fly",
      "command": "/effect @s levitation 10 1 true",
      "name": "飞行术",
      "description": "获得10秒漂浮效果，可以短暂飞行",
      "category": "实用",
      "tags": ["flight", "effect", "magic"]
    }
  ],
  "templates": [
    {
      "id": "magic:fireball_tpl",
      "command": "/summon fireball ~ ~1 ~ {Motion:[0.0,0.5,0.0]}",
      "name": "召唤火球",
      "description": "在玩家位置召唤一个火球，可以自定义方向和速度",
      "category": "攻击"
    }
  ]
}
```

---

## 版本历史

### v2.0.0 (2026-06-30)

**新增**:
- 新增动画类型支持（`animations` 字段）
- 新增命令库类型拓展包（`packType: "commandLib"`，`idlist` + `enums`）
- 新增方块状态类型拓展包（`packType: "blockStates"`，`states` 字段）
- 新增 JSON Schema 类型拓展包（`packType: "jsonSchema"`，`jsonSchema` 字段）
- 新增 `uuid` 字段（UUID 唯一标识符）
- 新增 `require` 字段（依赖管理）
- 新增 `packType` 字段（拓展包类型）
- 新增 `$注释` 字段（开发者备注）
- 扩展字段属性：`hardness`, `resistance`, `lightLevel`, `maxStack`, `rarity`, `duration`, `targetType` 等
- 新增 `tags` 字段支持（所有类型）

**变更**:
- 版本号支持数组格式（`[1, 0, 0]`）
- `additionalProperties` 改为 `true`（允许扩展字段）
- 部分字段长度限制放宽
- 完善各类型字段说明和示例
- 增加命名规范和最佳实践章节
- 新增 5 种拓展包类型的详细说明

**兼容性**:
- 完全向下兼容 v1.x 版本
- 所有新增字段均为可选
- 旧拓展包无需修改即可使用

### v1.1.0 (2026-06-27)

- 新增调试与测试指南
- 新增发布与版本管理文档
- 完善开发指南

### v1.0.0 (2026-06-27)

- 初始版本
- 支持方块、物品、音效、粒子、命令、模板

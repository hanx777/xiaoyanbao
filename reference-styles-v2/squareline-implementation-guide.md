# 参考风格第二版 — SquareLine / LVGL 实现规格

## 1. 通用页面结构

### 主页

- Header：`x=18, y=16, w=444, h=60`
- 四宫格：`x=18, y=88, w=444`
- 卡片：`216 × 176 px`
- 横向与纵向间距：`12 px`
- More 可见尺寸：约 `60 × 28 px`
- 实际点击区域建议：至少 `72 × 44 px`

### 详情页

- Back：`x=18, y=15, 42 × 42 px`
- Gauge：`x=90, y=67, 300 × 266 px`
- Reading Panel：`x=18, y=338, 444 × 124 px`

### Arc

- Rotation：`120°`
- 有效角度：`300°`
- 底部缺口：`60°`
- Knob：约 `13–18 px`
- Track 与 Indicator 根据主题选择圆头或平头
- 清除 `LV_OBJ_FLAG_CLICKABLE`

```text
progress = clamp((value - min) / (max - min), 0, 1)
angle = 120° + progress × 300°
```

Humidity `55.7%` 对应整条有效圆弧的 `55.7%`。

## 2. Editorial Ledger

### 视觉参数

- Screen：`#F7F4EF`
- Card：`#FAF8F4`
- Text：`#171512`
- Line：`#D8D2CA`
- 无圆角、无阴影
- Arc 宽度：`8 px`
- 标题与数字使用衬线字体
- 辅助文字使用无衬线字体

### SquareLine 建议

- 使用 `Playfair Display` 或相似衬线字体制作标题和数值
- 卡片用 1 px 细边框
- More 使用文字按钮和底部横线
- 避免大面积色块，仅用指标色强调图标、进度和圆弧

## 3. Swiss Grid

### 视觉参数

- Screen：`#F7F7F7`
- Card：`#FFFFFF`
- Text：`#050505`
- Red：`#E10600`
- Line：`#BFC1C3`
- 无圆角、无阴影
- Arc 宽度：`14 px`
- Arc 使用平头

### SquareLine 建议

- 所有组件严格对齐到网格
- 标题、按钮和指标名称使用 Bold
- 四个指标统一使用红色强调
- More 使用红底白字按钮
- 编号使用右上角 `29 × 20 px` 红色色块

## 4. Gentle Illustrative

### 视觉参数

- Screen：`#FFFAF6`
- Card：`#FFFFFF`
- Text：`#25213E`
- Primary：`#6C5CE7`
- Card Radius：`21 px`
- Arc 宽度：`14 px`
- Reading Panel Radius：`22 px`

指标色：

| 指标 | 色值 |
|---|---:|
| Temperature | `#F18A56` |
| Humidity | `#4EBFBB` |
| CO₂ | `#70B986` |
| PM2.5 | `#6C5CE7` |

### SquareLine 建议

- 插画装饰使用圆形、椭圆和两片叶子，不依赖复杂图片
- 可以把几何插画组合导出成小型透明 PNG
- 阴影较轻；低性能设备可删除阴影，仅保留浅色边框
- 图标底板使用 12–22 px 圆角

## 5. Signal Brutalism

### 视觉参数

- Screen：`#0C0D0D`
- Card：`#111313`
- Text：`#F1F4F2`
- Line：`#3A403E`
- Primary：`#00F5C3`
- 无圆角、无阴影
- Arc 宽度：`8 px`
- Arc 使用平头或方头

指标色：

| 指标 | 色值 |
|---|---:|
| Temperature | `#FFC700` |
| Humidity | `#00F5C3` |
| CO₂ | `#00D3A6` |
| PM2.5 | `#7D7BFF` |

### SquareLine 建议

- 标题和按钮统一大写
- 使用 1 px 灰色硬边框
- LIVE、More 和状态使用荧光色
- 背景网格可使用一张低透明度小型 Pattern 图片；性能有限时直接使用纯黑背景

## 6. 数据和状态

| 指标 | 示例值 | Min | Max | 状态 |
|---|---:|---:|---:|---|
| Temperature | `23.8 °C` | `0` | `40` | `Comfortable` |
| Humidity | `55.7 %` | `0` | `100` | `Comfortable` |
| CO₂ | `742 ppm` | `400` | `2000` | `Good` |
| PM2.5 | `18.4 µg/m³` | `0` | `150` | `Good` |

状态规则与项目第一版保持一致。

## 7. 页面数量

快速搭建可以建立五个 Screen：

```text
Home
TemperatureDetail
HumidityDetail
CO2Detail
PM25Detail
```

为了节省内存，更推荐：

```text
Home
SensorDetail
```

四个 More 按钮进入同一个详情页，并动态更新标题、颜色、图标、数值、单位、Arc 范围和状态。

## 8. 字体与单位

字体需包含：

```text
° ₂ µ ³ – /
```

如果资源不足，可使用：

```text
CO2
ug/m3
```

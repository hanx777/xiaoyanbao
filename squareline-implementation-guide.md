# 480 × 480 Indoor Monitor — SquareLine 实现规格

本方案包含三套完整视觉系统，每套均有：

- 1 个主页：Temperature、Humidity、CO₂、PM2.5 四个模块
- 4 个详情页：统一的不闭合圆弧、进度圆点、图标、实时值、状态和推荐范围
- 全部界面文案使用英文
- 画布固定为 `480 × 480 px`

## 1. 三套视觉方向

| 方案 | 定位 | 推荐用途 | 背景 | 卡片 | 主文字 | 字体建议 |
|---|---|---|---|---|---|---|
| ClearLab | 明亮、理性、清晰 | 仪器、医疗感、白天可读性优先 | `#F5F9FB` | `#FFFFFF` | `#173042` | Montserrat / Inter |
| Midnight Orbit | 深色、科技、专注 | 黑色面板、夜间显示、科技产品 | `#070B12` | `#101826` | `#F4F7FB` | Montserrat / Roboto Mono |
| Soft Habitat | 柔和、家居、安静 | 消费级家居产品、空气管家 | `#F6F2EA` | 指标浅色底 | `#27322E` | Nunito Sans / Montserrat |

共同指标色：

| 指标 | ClearLab | Midnight Orbit | Soft Habitat |
|---|---:|---:|---:|
| Temperature | `#F06B5E` | `#FFB15C` | `#D97B5F` |
| Humidity | `#2E86DE` | `#36D8F2` | `#5D9295` |
| CO₂ | `#00A47A` | `#55E0A3` | `#78916C` |
| PM2.5 | `#E39A24` | `#AA8DFF` | `#B18A4F` |

建议先选定一套主题，再将其颜色、圆角和字体样式保存为 SquareLine Styles，四个指标只替换 Accent Color。

## 2. 主页精确布局

### Header

- 安全边距：左右 `20 px`
- Header：`x=20, y=17, w=440, h=52`
- 主标题：约 `27 px / Semibold`
- 辅助说明：`10–11 px`
- LIVE 胶囊：约 `66 × 28 px`

### 四卡片网格

- 网格：`x=18, y=80, w=444, h=380`
- 单卡片：`216 × 184 px`
- 水平间距：`12 px`
- 垂直间距：`12 px`
- 两列坐标：`x=18`、`x=246`
- 两行坐标：`y=80`、`y=276`

单卡片建议对象：

1. Icon Container：`36 × 36 px`
2. Metric Name：`12–13 px`
3. Main Value：`37–39 px`
4. Unit：`12–14 px`
5. Mini Track：高 `4 px`
6. Status Dot + Status Label
7. More：可见尺寸 `64 × 30 px`

`More` 的实际触控区域建议至少 `72 × 44 px`。可以使用一个透明 Button 作为点击区，再把较小的可见胶囊放在其中央。

主页英文文案示例：

```text
Temperature   23.8 °C   Comfortable   More
Humidity      55.7 %    Comfortable   More
CO₂           742 ppm   Good          More
PM2.5         18.4 µg/m³ Good         More
```

## 3. 详情页精确布局

- Back Button：`x=20, y=16, 42 × 42 px`
- Title Group：从 `x=74` 开始
- LIVE：右上角，约 `66 × 28 px`
- Gauge Zone：`x=90, y=65, 300 × 270 px`
- Reading Panel：`x=20, y=338, 440 × 124 px`

### 不闭合圆弧

- 圆心：约 `x=240, y=190`
- 半径：约 `105 px`
- 轨道宽度：亮色/柔和主题 `12–13 px`，深色主题 `11 px`
- 有效角度：`300°`
- 缺口：底部 `60°`
- Rotation：`120°`
- Indicator 和 Track 均使用 Rounded Line Cap
- Knob：直径约 `14–18 px`，外侧增加与页面背景同色的描边

圆点位置公式：

```text
progress = clamp((value - min) / (max - min), 0, 1)
angle = 120° + progress × 300°
```

Humidity 示例：`55.7%`，因此圆点位于整条有效圆弧的 `55.7%` 处。

详情页内容层级：

```text
Humidity
Living Room · Live sensor

[open arc + droplet icon + 56%]
Comfort range 30–60 %

REAL-TIME HUMIDITY          Comfortable
55.7 %
Recommended · 30–60 %      Updated just now
```

## 4. 指标配置

| 指标 | 示例值 | Arc Min | Arc Max | 进度 | 状态 | 推荐文案 |
|---|---:|---:|---:|---:|---|---|
| Temperature | `23.8 °C` | `0` | `40` | `59.5%` | `Comfortable` | `Comfort range 20–26 °C` |
| Humidity | `55.7 %` | `0` | `100` | `55.7%` | `Comfortable` | `Comfort range 30–60 %` |
| CO₂ | `742 ppm` | `400` | `2000` | `21.4%` | `Good` | `Target below 800 ppm` |
| PM2.5 | `18.4 µg/m³` | `0` | `150` | `12.3%` | `Good` | `Good below 35 µg/m³` |

超过 Arc 最大值时：数字继续显示真实值，但 Arc 与 Knob 停在终点。

## 5. 默认状态规则

这些区间用于界面提示，可根据传感器规范或目标市场标准集中修改。

### Temperature

| 范围 | 英文状态 |
|---|---|
| `<16 °C` | `Cold` |
| `16–19.9 °C` | `Cool` |
| `20–26 °C` | `Comfortable` |
| `26.1–30 °C` | `Warm` |
| `>30 °C` | `Hot` |

### Humidity

| 范围 | 英文状态 |
|---|---|
| `<30%` | `Dry` |
| `30–60%` | `Comfortable` |
| `60.1–70%` | `Humid` |
| `>70%` | `Very Humid` |

### CO₂

| 范围 | 英文状态 |
|---|---|
| `≤800 ppm` | `Good` |
| `801–1000 ppm` | `Fair` |
| `1001–1500 ppm` | `Ventilate` |
| `>1500 ppm` | `Poor` |

### PM2.5

| 范围 | 英文状态 |
|---|---|
| `≤15 µg/m³` | `Excellent` |
| `15.1–35 µg/m³` | `Good` |
| `35.1–55 µg/m³` | `Moderate` |
| `55.1–75 µg/m³` | `Elevated` |
| `>75 µg/m³` | `Poor` |

状态颜色建议：

- Comfortable / Good / Excellent：绿色
- Cool / Dry：蓝色
- Fair / Warm / Humid / Moderate：黄色或琥珀色
- Hot / Poor / Very Humid / Ventilate：红色

## 6. SquareLine 对象结构

### 简单做法：5 个 Screen

```text
Home
TemperatureDetail
HumidityDetail
CO2Detail
PM25Detail
```

适合快速搭建。四个详情页先复制同一个模板，再替换颜色、标题、图标、范围和单位。

### 节省内存做法：2 个 Screen

```text
Home
SensorDetail
```

点击任意 More 时，只向 `SensorDetail` 注入当前指标的数据。这样只保留一个 Arc 和一套详情页对象，Flash/RAM 使用更少。

建议详情页对象树：

```text
SensorDetail
├─ BtnBack
├─ LabelTitle
├─ LabelRoom
├─ LivePill
├─ ArcValue
├─ IconContainer
│  └─ ImgMetric
├─ LabelRoundedValue
├─ LabelRangeMin
├─ LabelRangeHint
├─ LabelRangeMax
└─ ReadingPanel
   ├─ LabelRealtimeCaption
   ├─ StatusPill
   ├─ LabelValue
   ├─ LabelUnit
   ├─ LabelRecommended
   └─ LabelUpdated
```

## 7. LVGL / SquareLine 设置注意点

- Home、Detail 和所有容器关闭滚动：清除 `LV_OBJ_FLAG_SCROLLABLE`。
- Arc 仅用于显示：清除 `LV_OBJ_FLAG_CLICKABLE`，避免用户拖动传感器读数。
- More：进入详情可用 `MOVE_LEFT 180–220 ms`。
- Back：返回主页可用 `MOVE_RIGHT 180–220 ms`。
- 传感器建议每秒刷新一次；数值与 Arc 可用 `200–300 ms` 短动画平滑过渡。
- 不要在传感器中断或非 GUI 线程直接修改 LVGL 对象；先通过队列把数据传到 UI 任务。
- SquareLine 自动生成文件会被重新导出覆盖。状态判断、字符串格式化和传感器更新放入独立用户代码文件。

LVGL Arc 只接收整数时，可将带一位小数的指标放大 10 倍：

```text
Temperature: 23.8 °C -> value 238, range 0–400
Humidity:    55.7 %   -> value 557, range 0–1000
PM2.5:       18.4     -> value 184, range 0–1500
CO₂:         742      -> value 742, range 400–2000
```

## 8. 字体与图标

建议导入的字号：`10、12、14、21、27、39、46 px`。资源紧张时可合并为 `12、14、21、39、46 px`。

字体必须包含：

```text
° ₂ µ ³ –
```

如果字体或固件不支持这些字符，可退化为：

```text
CO2
ug/m3
```

图标保持同一套单色线框风格。推荐使用 A8 图标或可 recolor 的单色 PNG，以便四套指标色共用同一资源。

## 9. 选择建议

- 优先考虑可读性与通用性：选 ClearLab。
- 设备外壳和面板以黑色为主：选 Midnight Orbit。
- 产品定位偏家居、陪伴和舒适感：选 Soft Habitat。

HTML 原型中的每个 More 和 Back 都可以点击，用于检查完整页面流转。

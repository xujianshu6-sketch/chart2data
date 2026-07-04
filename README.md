<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)">
    <img alt="Chart2Data" src="data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNjAwIiBoZWlnaHQ9IjEwMCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48ZGVmcz48bGluZWFyR3JhZGllbnQgaWQ9ImciIHgxPSIwJSIgeTE9IjAlIiB4Mj0iMTAwJSIgeTI9IjEwMCUiPjxzdG9wIG9mZnNldD0iMCUiIHN0b3AtY29sb3I9IiM1OGE2ZmYiLz48c3RvcCBvZmZzZXQ9IjEwMCUiIHN0b3AtY29sb3I9IiNhMzcxZjciLz48L2xpbmVhckdyYWRpZW50PjwvZGVmcz48dGV4dCBmb250LWZhbWlseT0iLWFwcGxlLXN5c3RlbSxCbGlua01hY1N5c3RlbUZvbnQsU2Vnb2UgVUksc2Fucy1zZXJpZiIgZm9udC13ZWlnaHQ9IjgwMCIgZm9udC1zaXplPSI0MCIgZmlsbD0idXJsKCNnKSIgeD0iMzAwIiB5PSI2MCIgdGV4dC1hbmNob3I9Im1pZGRsZSI+Q2hhcnQyRGF0YTwvdGV4dD48L3N2Zz4=">
  </picture>
</p>

<p align="center">
  <strong>图表曲线 → 数值数据 → Excel 导出</strong><br>
  <sub>上传一张科研图表，自动提取曲线数据，支持手动采样与半自动颜色拾取</sub>
</p>

<p align="center">
  <a href="#-功能亮点"><strong>功能</strong></a> ·
  <a href="#-快速开始"><strong>使用</strong></a> ·
  <a href="#-技术架构"><strong>技术</strong></a> ·
  <a href="#-本地运行"><strong>本地</strong></a> ·
  <a href="#-在线体验"><strong>在线</strong></a>
</p>

---

## 解决的问题

在科研复现、Meta-Analysis、基准对比、竞品分析等场景中，我们常需要从已发表的论文图表中**反提取数值数据**。Chart2Data 是一个**纯浏览器端**工具，无需安装、无需后端、无需注册，直接上传图表图片即可完成从像素到数字的全流程转换。

### 支持场景

- **散点图 / 折线图** — 颜色拾取快速提取整条曲线
- **柱状图 / 条形图** — 手动逐点采样
- **多曲线叠加图** — 不同颜色可分别拾取
- **光谱图 / 色谱图** — 容忍较大像素带宽
- **显微镜 / SEM / TEM 图像标注** — 手动选点记录坐标
- **地图 / 地理轨迹图** — 建立地理坐标参考系后提取

---

## 功能亮点

### 1. 双模式数据提取

| 模式 | 原理 | 适合 |
|---|---|---|
| **手动选点** | 鼠标单击曲线上的关键点，带 4× 像素级放大镜精准定位 | 柱状图、离散点、精度要求高的场景 |
| **半自动颜色拾取** | 点击曲线采样目标颜色，一键扫描全图所有匹配像素并转为数据 | 折线图、连续曲线、大数据量提取 |

### 2. 四步标准化流程

```
[1] 上传图片    拖拽或点击上传 PNG/JPG/BMP/TIFF/WebP
[2] 坐标轴标定  点击 X/Y 轴起点和终点 → 输入实际数值 → 建立坐标系映射
[3] 提取数据    手动取点 / 半自动颜色拾取 / 框选删除误提取点
[4] 导出 Excel   等间距线性插值重采样 → 一键下载 .xlsx 文件
```

### 3. 精准标定系统

- **正交坐标映射**：X 轴和 Y 轴独立标定，不要求轴正交于原点
- **实时十字准星**：标定点可视化标记，误差一目了然
- **标定值回显**：每个标定点显示像素坐标 + 实际数值

### 4. 智能数据处理

- **自动 X 排序**：输出数据按 X 坐标升序排列
- **同值合并去重**：相同 X 附近的多个采集点取 Y 均值合并
- **等间距线性插值**：用户可设定输出点数（10-4000），均匀分布
- **实时数据预览**：前 10 行即时显示，验证提取质量

### 5. 交互优化

- ⚡ **容差滑块**：实时调节颜色匹配精度（RGB 空间距离阈值）
- 🖱️ **框选删除**：拖拽矩形框批量移除噪声像素
- 🔍 **4× 放大镜**：手动模式下像素级精确取样
- ⌨️ **键盘快捷键**：`C` 标定 | `M` 手动 | `S` 半自动 | `E` 导出 | `Ctrl+Z` 撤销
- 🌓 **暗色 UI + WebGL 背景**：毛玻璃面板 + 交互式 3D 棱镜着色器

---

## 技术架构

```
┌─────────────────────────────────────────────┐
│              浏览器端 (Client Only)          │
├─────────────────────────────────────────────┤
│  UI 层     │ 原生 DOM + CSS Glassmorphism   │
│  渲染层    │ Canvas 2D API (图像+叠加层)     │
│  背景层    │ WebGL Raymarching Shader       │
│  导出层    │ SheetJS (xlsx)                 │
│  数据处理  │ 纯 JS 线性插值 + 颜色空间匹配    │
├─────────────────────────────────────────────┤
│  零依赖 | 零后端 | 零注册 | 数据不出浏览器    │
└─────────────────────────────────────────────┘
```

**核心技术点：**

- **颜色匹配引擎**：RGB 欧几里得距离阈值 + 全图逐像素扫描，支持实时容差调节
- **坐标系重建**：基于 4 个标定点的正交线性映射，将像素坐标转为用户定义的实际量纲
- **Raymarching 背景**：GPU 着色器实时渲染 3D 棱镜，鼠标跟随旋转交互
- **数据后处理管线**：排序 → 分箱合并 → 等距插值 → 格式化输出

---

## 快速开始

### 在线使用

访问 **[在线地址](#)** 即可直接使用。不需要安装、注册或登录。

### 本地运行

```bash
git clone https://github.com/xujianshu6-sketch/chart2data.git
cd chart2data
# 直接用浏览器打开 index.html，或用任意静态服务器：
npx serve .
```

兼容所有现代浏览器（Chrome / Edge / Firefox / Safari）。

---

## 使用示例

### 提取一条折线图数据（半自动模式）

1. 拖入论文截图
2. 点击"开始标定"，在图上依次点 **X 轴起点 → X 轴终点 → Y 轴起点 → Y 轴终点**，每次输入对应的实际坐标值
3. 点击"拾取曲线颜色"，在曲线上点一下
4. 调节容差滑块直到匹配像素覆盖整条曲线，点击"确认提取"
5. 设置输出数据组数，点击"导出 Excel"

### 提取柱状图数据（手动模式）

1. 上传图片 → 标定坐标轴
2. 进入"手动选点"模式
3. 依次点击每个柱子的顶点（放大镜辅助定位）
4. 导出

---

## 项目结构

```
chart2data/
  index.html      # 完整应用（单文件，包含 CSS + JS + WebGL Shader）
  README.md
```

---

## 贡献

欢迎提 Issue 和 PR。

- 🐛 Bug 报告 → [Issues](https://github.com/xujianshu6-sketch/chart2data/issues)
- 💡 功能建议 → [Discussions](https://github.com/xujianshu6-sketch/chart2data/discussions)

---

## 许可

[MIT License](LICENSE) · 2026

---

<p align="center">
  <sub>如果这个工具对你有帮助，请给一个 ⭐ Star</sub>
</p>

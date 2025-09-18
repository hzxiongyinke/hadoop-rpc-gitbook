# 图表资源说明

本目录包含了Hadoop RPC分析文档中使用的所有Draw.io图表源文件。

## 📁 文件列表

| 文件名 | 描述 | 对应文档位置 |
|--------|------|-------------|
| `hadoop-rpc-ecosystem.drawio` | Hadoop生态系统RPC通信架构图 | `part1/chapter1/importance.md` |
| `rpc-call-architecture.drawio` | RPC调用完整架构图 | `part1/chapter1/rpc-concepts.md` |
| `analysis-framework.drawio` | 六维度分析框架图 | `part1/chapter1/framework.md` |

## 🎨 如何编辑图表

### 1. 打开Draw.io
访问 [https://app.diagrams.net/](https://app.diagrams.net/) 或使用桌面版Draw.io应用。

### 2. 导入图表
- 点击 **File** → **Open from** → **Device**
- 选择对应的 `.drawio` 文件
- 或者直接将文件拖拽到Draw.io界面中

### 3. 编辑图表
- 修改文本、颜色、布局等
- 添加或删除组件
- 调整连接线和样式

### 4. 导出为SVG
- 点击 **File** → **Export as** → **SVG**
- 保持默认设置，点击 **Export**
- 将导出的SVG文件保存到 `images/` 目录

### 5. 更新文档引用
确保文档中的图片路径正确指向导出的SVG文件：
```html
<img src="../../../images/your-diagram.svg" alt="图表描述" />
```

## 🔧 图表设计规范

### 颜色方案
- **主要组件**: `#f8cecc` (浅红色)
- **次要组件**: `#dae8fc` (浅蓝色)  
- **连接线**: `#d79b00` (橙色)
- **背景层**: `#e1d5e7` (浅紫色)
- **文本**: `#2c3e50` (深灰色)

### 字体规范
- **标题**: 14-16px, 粗体
- **组件标签**: 12px, 常规
- **说明文字**: 10-11px, 常规

### 布局原则
- 保持组件间距一致
- 使用网格对齐
- 连接线避免交叉
- 重要组件居中放置

## 📐 尺寸建议

- **画布大小**: 1169 x 827 (A4横向)
- **导出分辨率**: 300 DPI
- **最大宽度**: 1000px (适配网页显示)

## 🚀 快速操作指南

### 批量导出SVG
如果需要批量导出所有图表为SVG格式，可以使用以下步骤：

1. 逐个打开每个 `.drawio` 文件
2. 导出为SVG格式
3. 命名规则：`原文件名.svg`（去掉.drawio后缀）

### 图表版本管理
- 修改图表后，同时更新 `.drawio` 源文件和 `.svg` 导出文件
- 在git提交时包含两个文件
- 在文档中添加修改说明

## 💡 最佳实践

1. **保持源文件**: 始终保留 `.drawio` 源文件，便于后续修改
2. **统一风格**: 所有图表使用相同的颜色方案和字体
3. **清晰标注**: 为每个组件添加清晰的标签和说明
4. **适配移动端**: 确保图表在小屏幕上也能清晰显示
5. **定期更新**: 随着文档内容更新，及时更新相关图表

## 🔗 相关资源

- [Draw.io官方文档](https://www.diagrams.net/doc/)
- [SVG格式说明](https://developer.mozilla.org/en-US/docs/Web/SVG)
- [Web图表最佳实践](https://www.smashingmagazine.com/2017/09/designing-better-data-tables/)

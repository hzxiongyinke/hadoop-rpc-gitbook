# 图表资源说明

本目录包含了Hadoop RPC分析文档中使用的所有图表源文件，包括Draw.io图表和Mermaid时序图。

## 📁 文件列表

### 第1章 引言

| 文件名 | 描述 | 类型 | 对应文档位置 |
|--------|------|------|-------------|
| `tech-evolution.drawio` | 大数据时代技术演进图 | Draw.io | `chapter1.md` |
| `communication-challenges.drawio` | 分布式系统通信挑战对比图 | Draw.io | `chapter1.md` |
| `rpc-sequence-flow.md` | RPC调用完整流程时序图 | Mermaid | `chapter1.md` |
| `hadoop-ecosystem-rpc.drawio` | Hadoop生态系统RPC通信架构图 | Draw.io | `chapter1.md` |

### 第2章 Hadoop RPC概述

| 文件名 | 描述 | 类型 | 对应文档位置 |
|--------|------|------|-------------|
| `hadoop-rpc-architecture.drawio` | Hadoop RPC整体架构图 | Draw.io | `chapter2.md` |
| `rpc-framework-comparison.drawio` | RPC框架对比分析图 | Draw.io | `chapter2.md` |
| `rpc-engine-evolution.md` | RPC引擎演进时序图 | Mermaid | `chapter2.md` |
| `hadoop-rpc-protocols.drawio` | Hadoop生态系统RPC协议详解图 | Draw.io | `chapter2.md` |

### 第3章 RPC框架核心组件

| 文件名 | 描述 | 类型 | 对应文档位置 |
|--------|------|------|-------------|
| `rpc-core-components.drawio` | RPC核心组件关系图 | Draw.io | `chapter3.md` |
| `client-internal-architecture.drawio` | Client类内部架构图 | Draw.io | `chapter3.md` |
| `server-multithreaded-architecture.drawio` | Server多线程架构图 | Draw.io | `chapter3.md` |
| `rpc-complete-flow.md` | RPC调用完整流程时序图 | Mermaid | `chapter3.md` |

### 第4章 通信协议设计

| 文件名 | 描述 | 类型 | 对应文档位置 |
|--------|------|------|-------------|
| `protocol-stack-architecture.drawio` | 协议栈分层架构图 | Draw.io | `chapter4.md` |
| `message-format-details.drawio` | 消息格式详解图 | Draw.io | `chapter4.md` |
| `connection-lifecycle-management.drawio` | 连接生命周期管理图 | Draw.io | `chapter4.md` |
| `protocol-handshake-auth.md` | 协议握手与认证流程时序图 | Mermaid | `chapter4.md` |

### 第5章 序列化引擎演进

| 文件名 | 描述 | 类型 | 对应文档位置 |
|--------|------|------|-------------|
| `serialization-engine-evolution.drawio` | 序列化引擎演进历程图 | Draw.io | `chapter5.md` |
| `rpc-engines-architecture-comparison.drawio` | 三代引擎架构对比图 | Draw.io | `chapter5.md` |
| `serialization-performance-comparison.drawio` | 序列化性能对比图 | Draw.io | `chapter5.md` |

### 第6章 网络通信实现

| 文件名 | 描述 | 类型 | 对应文档位置 |
|--------|------|------|-------------|
| `socket-communication-architecture.drawio` | Socket通信基础架构图 | Draw.io | `chapter6.md` |
| `nio-implementation-mechanism.drawio` | NIO实现机制详解图 | Draw.io | `chapter6.md` |
| `connection-pool-management.drawio` | 连接池管理架构图 | Draw.io | `chapter6.md` |
| `data-transmission-optimization.drawio` | 数据传输优化策略图 | Draw.io | `chapter6.md` |
| `network-communication-flow.md` | 网络通信完整流程时序图 | Mermaid | `chapter6.md` |

### 原有图表

| 文件名 | 描述 | 类型 | 对应文档位置 |
|--------|------|------|-------------|
| `hadoop-rpc-ecosystem.drawio` | Hadoop生态系统RPC通信架构图 | Draw.io | `part1/chapter1/importance.md` |
| `rpc-call-architecture.drawio` | RPC调用完整架构图 | Draw.io | `part1/chapter1/rpc-concepts.md` |
| `analysis-framework.drawio` | 六维度分析框架图 | Draw.io | `part1/chapter1/framework.md` |

## 🎨 如何编辑图表

### Draw.io图表编辑

#### 1. 打开Draw.io
访问 [https://app.diagrams.net/](https://app.diagrams.net/) 或使用桌面版Draw.io应用。

#### 2. 导入图表
- 点击 **File** → **Open from** → **Device**
- 选择对应的 `.drawio` 文件
- 或者直接将文件拖拽到Draw.io界面中

#### 3. 编辑图表
- 修改文本、颜色、布局等
- 添加或删除组件
- 调整连接线和样式

#### 4. 导出为SVG
- 点击 **File** → **Export as** → **SVG**
- 保持默认设置，点击 **Export**
- 将导出的SVG文件保存到 `images/` 目录

### Mermaid图表编辑

#### 1. 编辑Mermaid代码
- 直接编辑 `.md` 文件中的Mermaid代码块
- 使用任何文本编辑器或Markdown编辑器

#### 2. 预览图表
- 使用支持Mermaid的编辑器（如Typora、VS Code + Mermaid插件）
- 或访问 [Mermaid Live Editor](https://mermaid.live/)

#### 3. 在文档中使用
- 直接在Markdown文档中引用
- Docsify会自动渲染Mermaid图表

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
6. **Mermaid优势**: 时序图使用Mermaid，便于版本控制和快速修改

## 🔗 相关资源

- [Draw.io官方文档](https://www.diagrams.net/doc/)
- [Mermaid官方文档](https://mermaid.js.org/)
- [SVG格式说明](https://developer.mozilla.org/en-US/docs/Web/SVG)
- [Web图表最佳实践](https://www.smashingmagazine.com/2017/09/designing-better-data-tables/)

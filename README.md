# Apple Design Skill

把 Apple 的设计原则转成可执行的中文界面设计与开发指引，重点支持网页手势、可中断弹簧动画、排版、半透明材质和 Liquid Glass 液态玻璃。

本项目是独立整理的资料与网页适配，非 Apple 官方发布。规则区分官方原则、特定课程案例和网页实现建议，并按任务加载详细参考。

## 安装与使用

在支持 Skill 的 Codex 环境中，可让内置安装技能安装本目录：

```text
$skill-installer https://github.com/MrRoam/apple-design-skill/tree/main/apple-design
```

也可将仓库中的 `apple-design` 目录复制到所用工具的技能目录，保留内部结构。

安装后，直接描述任务：

```text
$apple-design 为这个页面实现液态玻璃导航栏，保证文字清楚、交互自然。
```

```text
使用 apple-design 检查这个抽屉的拖拽、释放速度和动画中断，完成必要修改。
```

目标明确时直接工作，复用项目技术栈，按具体风险验证；普通网页布局不强制启用此技能。

## 内容

| 文件 | 用途 |
| --- | --- |
| [SKILL.md](apple-design/SKILL.md) | 核心判断、适用范围和资料入口 |
| [交互与动效](apple-design/references/interaction-motion.md) | 手势、惯性、速度交接、弹簧参数与中断 |
| [液态玻璃](apple-design/references/liquid-glass.md) | 材质边界、原生 API、网页近似与折射路径 |
| [设计基础与排版](apple-design/references/foundations-typography.md) | 八项原则、中文与混排、键盘和文字放大 |
| [官方资料索引](apple-design/references/sources.md) | Apple、Motion、W3C 和 MDN 参考 |
| [网页示例](apple-design/assets/liquid-glass-demo.html) | 无构建、无远程依赖的交互演示 |

## 网页示例

下载仓库后，在浏览器直接打开 `apple-design/assets/liquid-glass-demo.html`。可以切换晨光、暮色和夜色背景，调整背景位置，体验按压反馈与弹簧选择状态，也可以开启减少动态效果或提高对比度。

示例使用真实背景模糊和近似高光，未实现背景折射、形状融合或完整系统级光照，不等同于 Apple 系统的 Liquid Glass 渲染。需要更高还原度时，技能会指引按目标增加并验证光学效果。

## 设计依据与验证

主要依据 [Apple Design](https://developer.apple.com/design/)、[Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/)、[Designing Fluid Interfaces](https://developer.apple.com/videos/play/wwdc2018/803/) 和 [Meet Liquid Glass](https://developer.apple.com/videos/play/wwdc2025/219/)，具体出处见资料索引。官方资料核对日期：2026-10-02。

技能格式与内部资料链接已校验。网页示例在 Chrome 中检查了背景切换、快速重定向、键盘操作、滑块、系统偏好、无障碍开关、窄屏和文字放大。

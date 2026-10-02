# 官方资料索引

核对日期：2026-10-02。按任务查询，不要求通读全站；API、兼容性与库接口以当前文档及项目版本为准。

## Apple

| 资料 | 使用情境 |
| --- | --- |
| [Apple Design](https://developer.apple.com/design/) | 规范、资源、工具、课程入口 |
| [HIG](https://developer.apple.com/design/human-interface-guidelines/) | 按平台、组件查询，单个平台惯例不泛化 |
| [Materials](https://developer.apple.com/design/human-interface-guidelines/materials) | Liquid Glass、regular / clear、普通材质、可读性 |
| [Color](https://developer.apple.com/design/human-interface-guidelines/color) | 系统色、玻璃着色、状态与强调 |
| [Accessibility](https://developer.apple.com/design/human-interface-guidelines/accessibility) | 超出媒体查询的无障碍考虑 |
| [Design principles](https://developer.apple.com/design/human-interface-guidelines/design-principles) | 原则与权衡 |
| [Design Resources](https://developer.apple.com/design/resources/) | Figma、Sketch 模板等；按实际用途与条款使用 |
| [Designing Fluid Interfaces，WWDC18](https://developer.apple.com/videos/play/wwdc2018/803/) | 响应、中断、动量、弹性用途 |
| [Designing Audio-Haptic Experiences，WWDC19](https://developer.apple.com/videos/play/wwdc2019/810/) | 因果、协调、有用的反馈 |
| [The details of UI typography，WWDC20](https://developer.apple.com/videos/play/wwdc2020/10175/) | 字体、tracking、leading、Dynamic Type |
| [Meet Liquid Glass，WWDC25](https://developer.apple.com/videos/play/wwdc2025/219/) | 光学、环境适应、形变、材质边界 |
| [Get to know the new design system，WWDC25](https://developer.apple.com/videos/play/wwdc2025/356/) | 导航与整体设计关系 |
| [Principles of great design，WWDC26](https://developer.apple.com/videos/play/wwdc2026/250/) | 八项设计原则 |
| [Applying Liquid Glass to custom views](https://developer.apple.com/documentation/swiftui/applying-liquid-glass-to-custom-views) | SwiftUI 材质、容器、形变 API |
| [Adopting Liquid Glass](https://developer.apple.com/documentation/technologyoverviews/adopting-liquid-glass) | 原生应用采用新设计 |

课程参数是特定案例，当前 HIG 优先于旧课程简化示例。尤其 `clear` 的压暗，按背景与对比判断，不套统一不透明度。

## 网页

| 资料 | 使用重点 |
| --- | --- |
| [Motion animate](https://motion.dev/docs/animate) | 物理型 / 时间型弹簧、velocity、参数 |
| [CSS Transitions](https://www.w3.org/TR/css-transitions-1/#reversing) | 过渡可中断、反转，但不自动保证物理速度连续 |
| [MDN backdrop-filter](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/backdrop-filter) | 后景滤镜、backdrop root、兼容性 |
| [MDN feDisplacementMap](https://developer.mozilla.org/en-US/docs/Web/SVG/Reference/Element/feDisplacementMap) | 可验证的位移近似 |

中文规则、CSS 数值、Motion 参数和 HTML 演示是适配材料，不声称 Apple 为这些网页实现提供官方保证。

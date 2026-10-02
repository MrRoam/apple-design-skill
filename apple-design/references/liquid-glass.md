# 液态玻璃与网页适配

设计依据：[HIG Materials](https://developer.apple.com/design/human-interface-guidelines/materials)、[Meet Liquid Glass](https://developer.apple.com/videos/play/wwdc2025/219/)。原生 API 依据：[Applying Liquid Glass to custom views](https://developer.apple.com/documentation/swiftui/applying-liquid-glass-to-custom-views)。以下网页路径是适配建议，不等同 Apple 系统渲染。

## 先决定用在哪里

Liquid Glass 突出内容上方的功能层，优先用于关键导航、工具栏和控件。正文、列表、卡片保留稳定内容层；内容里的滑块、开关等短暂交互状态单独判断。避免玻璃叠玻璃与无差别使用。

| 材质 | 判断 |
| --- | --- |
| `regular` | 一般导航、较多文字或复杂背景时先考虑，具有保持可读性的适应行为 |
| `clear` | 富媒体背景上的控件，单独评估前景对比与必要的背景压暗 |
| 普通半透明材质 | 内容内部的结构区分，不必承诺 Liquid Glass 行为 |

不为“越透明越高级”而选 `clear`。当前 HIG 根据背景判断是否压暗，旧课程的压暗示例值不能成为所有场景的固定要求。

## 区分还原目标

| 层面 | 可观察效果 | 网页路径 |
| --- | --- | --- |
| 透光和背景 | 后方内容真实可见、可模糊 | 半透明填充与 `backdrop-filter` |
| 光学轮廓 | 背景在边缘弯曲、亮边随形状变化 | SVG 位移或可控场景着色器；描边只是近似高光 |
| 环境适应 | 背景变化时前景仍清楚 | 对比策略、分层遮罩和必要的色调变化 |
| 动态反馈 | 按压形变、发亮，可中断 | 小幅缩放、局部高光与保留速度的状态动画 |
| 形状过渡 | 展开、靠近时协调形态 | 共享几何过渡；独立模糊不会自动融合 |

只有模糊和透明的实现称为“半透明玻璃近似”。完整光学目标需检查背景折射、动态背景、形状变化与适应行为，不能只验一张截图。

## Apple 原生实现

先考虑系统组件。自定义 SwiftUI 控件可从以下模式开始，需确认 SDK、部署目标及 API 可用性：

```swift
Button("收藏") {
    // 执行用户动作
}
.buttonStyle(.glass)

Text("当前位置")
    .padding()
    .glassEffect(.regular.tint(.blue).interactive())
```

多个需协调、融合的形状考虑 `GlassEffectContainer`；形变按需要使用 `glassEffectID` 等 API，避免每个元素建独立容器。旧系统普通 Material 回退不冒充 Liquid Glass。

## 网页：可靠基础与渐进增强

复用项目技术栈，只给少量必要功能元素添加材质。以下值是网页起点，按背景、字号、设备调整：

```css
.glass {
  color: #172536;
  background: rgb(255 255 255 / 0.82);
  border: 1px solid rgb(255 255 255 / 0.7);
  border-radius: 999px;
  box-shadow: 0 8px 26px rgb(20 32 54 / 0.15);
}
@supports (backdrop-filter: blur(1px)) {
  .glass {
    background: rgb(255 255 255 / 0.5);
    backdrop-filter: blur(16px) saturate(125%);
  }
}
```

材质背景和高光位于前景之后，模糊、位移不施加到文字与图标上。圆角裁剪避免剪掉焦点环。`filter` 处理自身，`backdrop-filter` 处理后景；父元素透明度与滤镜会改变后景采样范围，模糊异常先查祖先和 backdrop root。

玻璃可以着色。关键动作用保留透光的色调或着色近似，不统一改实色遮挡，也不把所有控件都染成强调色。对比无法可靠保证时提高背景不透明度，不能仅靠字体加粗。

### 需要背景折射时

SVG `feDisplacementMap` 按映射图改变采样位置，可近似边缘透镜。中心保持中性位移，轮廓附近改变方向；映射匹配圆角和尺寸，避免把整个背景做成均匀波纹。

滤镜用于独立材质层或支持的背景滤镜路径，保留锐利前景。检查实际浏览器渲染、滤镜区域、裁剪和资源加载；`CSS.supports()` 只证明语法支持，不证明光学结果正确。

WebGL 等着色器适合可控制背景纹理的场景，不能自动获取任意 DOM 实时像素。不要用固定背景图的局部放大冒充整个页面真实折射。先证明采样方式可行，再增加动态光照。

## 示例与限制

[独立 HTML 示例](../assets/liquid-glass-demo.html)无需构建、无远程依赖：内容层保持普通材质，功能条有真实背景模糊、高光、按压反馈，选择状态用保留速度的数值弹簧；包含原生键盘控件及减少动态效果、提高对比度开关。

示例没有背景折射、形状融合或完整亮度自适应，不作为系统级复刻。复制时调整场景和文案，不把示例外观当所有产品的默认设计。

## 回退与验证

- `prefers-reduced-motion`：取消弹性和追随光效，保留直接反馈。
- `prefers-reduced-transparency`：提高不透明度、关闭模糊；不支持该偏好时仍有基础回退。
- `prefers-contrast`：稳定前景色、提高背景不透明度并加强必要边界。
- 滤镜不可用时用清楚的普通材质，避免整页重滤镜。

检查明暗、复杂及滚动背景中的文字与焦点；验证快速按压、重定向、窄屏和键盘，必要时看低性能设备。检查范围由实际项目决定。

技术参考：[backdrop-filter](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/backdrop-filter)、[feDisplacementMap](https://developer.mozilla.org/en-US/docs/Web/SVG/Reference/Element/feDisplacementMap)。

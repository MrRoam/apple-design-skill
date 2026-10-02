# 设计基础与排版

原则依据：[Principles of great design](https://developer.apple.com/videos/play/wwdc2026/250/)与 [HIG Design principles](https://developer.apple.com/design/human-interface-guidelines/design-principles)。原则用于权衡，不是必须逐项完成的问卷。

| 原则 | 对当前任务的判断 |
| --- | --- |
| Purpose | 解决什么问题，是否值得占用注意力？ |
| Agency | 是否可理解、可控制，有适当恢复方式？ |
| Responsibility | 是否服务用户，操作是否符合授权与风险？ |
| Familiarity | 外观、位置、行为是否符合平台经验？ |
| Flexibility | 不同设备、语言、字号和能力下是否可用？ |
| Simplicity | 信息是否恰好足够，是否减少操作与理解成本？ |
| Craft | 字体、色彩、对齐、反馈和性能是否可靠？ |
| Delight | 整体体验形成合适情绪，而非附加装饰？ |

控制靠近它影响的内容，分组符合任务关系。用户能理解位置、下一步与退出路径。导航名称按内容和习惯判断，不一概禁止 `Home` 等常见名称。

按动作提供状态、结果、警告或错误，问题在发生处解释；没有新信息或实际风险时，不加重复提示和确认。

## 排版

[The details of UI typography](https://developer.apple.com/videos/play/wwdc2020/10175/)强调字体、字号、tracking、leading 的关系。原生系统已有对应行为，“标题都加负字距”不是跨字体规则。

- 系统字体可以先用，但网页 `system-ui` 在不同系统结果不同，不保证是 San Francisco。
- `font-optical-sizing: auto` 需字体有光学尺寸轴，不替代选字体和实测。
- 优先默认字距，修改时按字体、字号、语言检查；中文与混排不直接照搬拉丁标题的负字距。
- 字号、字重、行距和间距共同建立层级。行距看字形、语言与密度，不套单一反比公式。
- 相对单位、灵活宽度、可换行布局适应文字放大，但不等于自动获得原生 Dynamic Type。

普通网页正文起点：

```css
body {
  font-family: system-ui, -apple-system, "Segoe UI", sans-serif;
  font-size: 1rem;
  line-height: 1.6;
}
h1 {
  font-size: clamp(2rem, 5vw, 4rem);
  line-height: 1.15;
  text-wrap: balance;
}
```

小字号不靠低对比表达次要关系，先尝试间距、位置与字重。透明背景按实际变化检查可读性。

## 无障碍与多感官

媒体偏好只是无障碍的一部分。检查语义、焦点、键盘替代、文字放大与触摸可达性；自定义滑块、拖拽、弹层不丢失浏览器已有能力。参见 [HIG Accessibility](https://developer.apple.com/design/human-interface-guidelines/accessibility)。

声音、触觉、视觉表达一致的原因、节奏与强度，只在有用时添加；按环境处理音频权限和振动支持，保留无需声音和触觉的操作路径。

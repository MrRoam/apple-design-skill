# 交互与动效

适用于直接操作、惯性、可中断动画。概念来源见 [Designing Fluid Interfaces](https://developer.apple.com/videos/play/wwdc2018/803/)；代码与工程取舍属于网页适配。

## 输入、反馈与取消

- 按下时突出控件，业务动作保留在提交事件。优先原生 `button` 与 `click`，保留键盘激活、移开取消和辅助技术支持。
- 拖拽保留抓取偏移，用 Pointer Events 与 `setPointerCapture()`；处理 `pointercancel` 和 `lostpointercapture`，清理手势状态。
- `touch-action` 按区域与方向设置。横向拖拽可保留纵向滚动；不要为局部控件禁掉整页滚动或缩放。
- 意图阈值区分滚动、点击和拖拽，按控件与设备调节。约 10 CSS px 可以试用，不能当成 Apple 固定规范。
- 用最近一小段位置、时间估计释放速度，处理停止后的陈旧样本与零时间间隔。位置统一为 CSS px，速度为 px/s。

## 拖拽与弹簧交接

拖拽时直接更新位置，不用缓动追赶指针。释放时以当前显示位置与速度启动弹簧；再次抓取时从画面上的位置接管，不从上次目标重新开始。

快速重定向保留当前速度。CSS transition 可以中断反转，但不等于物理速度连续；普通悬停和淡变通常不需要物理模拟。二维拖拽分别处理 X、Y 的位置与速度，避免丢失方向。

## 阻尼比不是阻尼系数

阻尼比 `ζ` 为无量纲比例。`ζ = 1` 表示临界阻尼，静止起步等常见条件下无振荡过冲；带初速度时仍需检查轨迹，不能保证所有初始条件下绝不越过目标。

物理弹簧常用质量 `m`、刚度 `k`、阻尼系数 `c`：

```text
ζ = c / (2 * sqrt(k * m))
c = 2 * ζ * sqrt(k * m)
```

不能把 Apple 的阻尼比 `1.0` 直接写成 Motion 的 `damping: 1`。例如 `mass: 1, stiffness: 225, damping: 30` 是临界阻尼的一组网页起点，不是 Apple 的统一参数。`response`、`duration`、`visualDuration` 的含义不同，不做数值一一对应。

### Motion 配置

[Motion 官方文档](https://motion.dev/docs/animate)区分物理型与时间型弹簧：物理配置考虑速度，`duration + bounce` 不自动继承手势速度。需要交接时采用物理配置，以项目版本为准。

```js
import { animate } from 'motion';

// xValue 是项目现有的数值 MotionValue；releaseVelocity 为 px/s。
animate(xValue, targetX, {
  type: 'spring',
  mass: 1,
  stiffness: 225,
  damping: 30,
  velocity: releaseVelocity,
});
```

重新抓取时停止控制器，以当前值接管；重新设定目标时保留当前速度。无需速度交接的独立进场动画可采用时间型弹簧。

某些 API 要求归一化速度，用速度除以剩余距离时防止接近零的距离导致除零；Motion 数值速度无需此归一化。

## 惯性目标与软边界

结合位置、速度、方向、可达状态选落点。不只按最近位置吸附，也不只看速度符号。原 Skill 的指数衰减投影可作候选模型：

```js
function projectDistance(velocityPxPerSecond, decayPerMillisecond = 0.998) {
  // 衰减率定义为每毫秒；必须 0 < decayPerMillisecond < 1。
  return (velocityPxPerSecond / 1000)
    * decayPerMillisecond / (1 - decayPerMillisecond);
}
```

`0.998` 是模型示例，不是浏览器原生滚动参数，也不代表所有 Apple 系统或第三方组件使用同一公式。限制可达目标，结合控件大小调整；已有组件行为合适时优先复用。

边界可逐渐增加阻力：

```js
function rubberband(overshoot, dimension, constant = 0.55) {
  // dimension > 0；返回保留方向的受限位移。
  return overshoot * dimension * constant
    / (dimension + constant * Math.abs(overshoot));
}
```

只在需要表达边界的操作中采用，不干扰浏览器已有滚动。

## 弹性、空间关系与性能

从无明显弹跳开始。惯性释放可以适度过冲；提示进一步操作也可以使用有功能的弹性，Apple 原课包含这两种用途。弹性不延迟操作、不妨碍内容跟踪。

展开位置关联触发器，退出路径通常与进入路径相容；流程改变时可以采用不同转场，不把路径对称当作硬约束。

优先动画 `transform` 与 `opacity`，其他有用途的属性按代价判断。避免逐帧交错读布局与写样式，`will-change` 不铺满页面。

弹簧循环使用 `requestAnimationFrame`，时间步按秒计算，处理切后台后的长间隔，结束时停止调度。减少动态效果时改即时更新或轻微淡变，指针跟随仍及时。

声音、触觉与视觉围绕同一动作协调，不要求所有反馈序列在同一帧开始；网页振动不等价于系统触觉。来源：[Designing Audio-Haptic Experiences](https://developer.apple.com/videos/play/wwdc2019/810/)。

针对改动检查按下反馈、移开取消、释放落点、中途抓取、快速反向、键盘替代及减少动态效果后的行为。

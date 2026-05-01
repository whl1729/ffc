我查了一轮，结论很明确：公开开源项目里，几乎没有成熟方案真正同时检测“人”和“绳”来做精确跳绳计数。现有项目绝大多数都只是“跳跃计数”。

比较有代表性的开源项目：

- chenwr727/RopeSkippingCounter 你现在这个项目同类方案，Python + OpenCV + MediaPipe，通过髋部/肩部关键点的上下位移计数。
  - 特点：实现简单、可复现。
  - 问题：不检测绳子，空跳也会计数。
  - 链接：https://github.com/chenwr727/RopeSkippingCounter

- aminnj/jumpcount
  - JavaScript + MediaPipe Pose，在浏览器里做人体姿态和垂直位移峰值检测。
  - 特点：实时性好、前端直接跑。
  - 问题：仍然只看人，不看绳子。
  - 链接：https://github.com/aminnj/jumpcount

- danimaaz/Jump-Rope-Counter
  - Python + OpenCV + Keras，用稠密光流提取整帧运动，再训练 CNN 分类“上升/下降/落地”。
  - 特点：比纯关键点法更接近“整体运动模式识别”。
  - 问题：虽然画面里可能包含绳子运动，但没有显式建模绳子是否过脚，本质仍不是“绳子状态检测”。
  - 链接：https://github.com/danimaaz/Jump-Rope-Counter

- foreverfaint/JumpRopeCounter
  - 安卓端，用音频计数，依赖绳子击地声。
  - 特点：某些环境下能反映绳子状态。
  - 问题：不是视觉方案，噪声敏感。
  - 链接：https://github.com/foreverfaint/JumpRopeCounter

- tyamazoe/JumpCounter / leCapi/RepCounter
  - 都是传感器/IMU方案。
  - 特点：工程上可用。
  - 问题：不是视觉检测，也不是“人+绳”联合视觉建模。
  - 链接1： https://github.com/tyamazoe/JumpCounter
  - 链接2：https://github.com/leCapi/RepCounter
                                                                                                                                     
和“绳子检测”最相关、但不是跳绳项目本身的开源基础设施：

- RMDLO/trackdlo
  - 这是做可变形线状物体跟踪的项目，目标是绳子、电线这类 DLO（Deformable Linear Objects）。
  - 它不是跳绳计数器，但如果要做真正的“绳子状态检测”，这是我查到最接近可复用的开源技术底座。
  - 链接：https://github.com/RMDLO/trackdlo

我的判断：

1. 现成可用的“人+绳联合精确计数”开源项目基本没有。
2. 现有开源实现主要分三类：
  - 人体姿态位移
  - 光流/动作分类
  - 音频或 IMU 传感器
3. 真正要做到精确计数，通常需要组合：
  - 人体姿态：判断是否起跳
  - 绳子跟踪：判断绳子是否完成一次有效过脚
  - 时序匹配：绳子过脚时刻与人体腾空时刻一致才计数

所以这块目前更像一个研究空白，而不是“已有成熟开源方案但你还没找到”。
如果你要，我下一步可以直接给你整理一个**“如何从当前这个项目演进到人+绳联合检测”的技术路线图**，包括可行模型、数据标注方式和最小可行实现。

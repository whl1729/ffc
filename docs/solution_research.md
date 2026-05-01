# 「花式足球教练」方案调研

## 2026-05-01

### 机器人动作训练方案调研

请问现在业界是怎么训练机器人学习人类动作的？
请介绍业界的训练方案及相关的开源项目或产品（如果有的话）

- [ChatGPT 的回答](./answers/humanoid_learning_chatgpt.md)
- [Gemini 的回答](./answers/humanoid_learning_gemini.md)
- [豆包的回答](./answers/humanoid_learning_doubao.md)
- [元宝的回答](./answers/humanoid_learning_yuanbao.md)
- [千问的回答](./answers/humanoid_learning_qianwen.md)
- [YanjieZe / awesome-humanoid-robot-learning](https://github.com/YanjieZe/awesome-humanoid-robot-learning)

### 人球共识方案调研

我想开发「花式足球教练」系统，这个系统需要同时识别人的动作和足球。
目前我调研到的 openpose, mmpose, mediapose 和 alphapose 貌似都只识别人的动作，不会同时识别其他物体。
请调查有哪些开源项目或产品满足「同时识别人的动作和物体」的要求。

- [ChatGPT 的回答](./answers/hoi_chatgpt.md)
- [Gemini 的回答](./answers/hoi_gemini.md)
- [豆包的回答](./answers/hoi_doubao.md)
- [元宝的回答](./answers/hoi_yuanbao.md)
- [千问的回答](./answers/hoi_qianwen.md)

## 2026-04-19

### 参考市面上的已有方案

目前可以参考以下问题的解决方案：

- 机器人学习人类动作
- AI 跳绳计数器
- 其他运动应用

#### AI 跳绳计数器方案调研

- [【超简单之50行代码】基于PaddleHub的跳绳AI计数器](https://aistudio.baidu.com/projectdetail/3936692)
- [chenwr727 / RopeSkippingCounter](https://github.com/chenwr727/RopeSkippingCounter)
- [danimaaz / Jump-Rope-Counter](https://github.com/danimaaz/Jump-Rope-Counter)

## 2026-04-15

### 动作对比方案调研

为了开发「花式足球教练」，我需要对比花式足球高手与普通人的动作的差异。
请问在对比人类动作差异领域，目前有哪些解决方案？
请介绍对应的研究团队、解决方案、适用场景、适用人群、成熟度及参考链接。
最好能介绍相关的开源项目。

回答：

- [豆包的回答](./answers/doubao_answer_action_comparison.md)
- [ChatGPT的回答](./answers/chatgpt_answer_action_comparison.md)
- [Gemini的回答](./answers/gemini_answer_action_comparison.md)

## 2026-04-12

### 「花式足球教练」方案对比

我准备开发一个「花式足球教练」系统，它的功能描述如下：

1. 这个系统包含至少一个摄像头，用于采集花式足球运动员的运动视频
2. 这个系统运行一个算法，其输入是花式足球运动视频，其输出是动作存在的问题及改进建议

我准备基于开源项目来进行二次开发，目前初步调研到 OpenPose, MMPose, MediaPipe 和 AlphaPose 等开源项目。
请从部署难度、动作识别精准度（特别是腿部动作）、二次开发难度等多个维度综合评估以上开源项目，并推荐最适合我的一个。

回答：

- [豆包的回答](./answers/doubao_answer_project_comparision.md)
- [Gemini 的回答](./answers/gemini_answer_project_comparison.md)

## 2026-04-06

### AI辅助运动市场方案提示词

请问现在AI辅助运动领域的发展现状是怎样的？请介绍最新的市场产品或科研成果。
所谓的「AI辅助运动」，是指利用AI技术帮助人们更好地练习舞蹈、武术、球类运动等各种技能。

- [ChatGPT的回答](./answers/chatgpt_answer_ai_aided_sport.md)
- [Gemini的回答](./answers/gemini_answer_ai_aided_sport.md)
- [豆包的回答](./answers/doubao_answer_ai_aided_sport.md)
- [元宝的回答](./answers/yuanbao_answer_ai_aided_sport.md)
- [千问的回答](./answers/qianwen_answer_ai_aided_sport.md)
- [智谱的回答](./answers/glm_answer_ai_aided_sport.md)

### 「花式足球教练」方案提示词

我准备开发一个「花式足球教练」系统，它的功能描述如下：

1. 这个系统包含至少一个摄像头，用于采集花式足球运动员的运动视频
2. 这个系统运行一个算法，其输入是花式足球运动视频，其输出是动作存在的问题及改进建议

我现在需要调研解决方案，请为我提供以下信息：

1. 目前有哪些开源项目支持类似功能？请提供参考链接
2. 如何开发这个系统？请提供可行的思路

回答：

- [ChatGPT的回答1](./answers/chatgpt_answer_ffc_solution_1.md)
- [ChatGPT的回答2](./answers/chatgpt_answer_ffc_solution_2.md)
- [Gemini的回答](./answers/gemini_answer_ffc_solution.md)
- [豆包的回答](./answers/doubao_answer_ffc_solution.md)
- [元宝的回答](./answers/yuanbao_answer_ffc_solution.md)
- [千问的回答](./answers/qianwen_answer_ffc_solution.md)
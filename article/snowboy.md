---
title: snowboy语音模型说明
date: 2019-01-01 20:11:22
categories:
  - 嵌入式
---

# snowboy 离线语音唤醒说明

- resources/models/snowboy.umdl：唤醒词为“snowboy”的通用模型。将 SetSensitivity 设置为 0.5，ApplyFrontend 设置为 false。
- resources/models/jarvis.umdl: 唤醒词为“Jarvis” (https://snowboy.kitt.ai/hotword/29) 的通用模型，其中包含了对应于“Jarvis”的两个唤醒词模型，所以需要设置两个 sensitivity。将 SetSensitivity 设置为 0.8,0.8，ApplyFrontend 设置为 true。
  -resources/models/smart_mirror.umdl: 唤醒词为“Smart Mirror” (https://snowboy.kitt.ai/hotword/47) 的通用模型。将 SetSensitivity 设置为 0.5，ApplyFrontend 设置为 false。
- resources/models/subex.umdl: 唤醒词为“Subex”(https://snowboy.kitt.ai/hotword/22014) 的通用模型。将 SetSensitivity 设置为 0.5，ApplyFrontend 设置为 true。
- resources/models/neoya.umdl: 唤醒词为“Neo ya”(https://snowboy.kitt.ai/hotword/22171) 的通用模型。其中包含了对应于“Neo ya”的两个>唤醒词模型，所以需要设置两个 sensitivity。将 SetSensitivity 设置为 0.7,0.7，ApplyFrontend 设置为 true。
- resources/models/hey_extreme.umdl: 唤醒词为“Hey Extreme” (https://snowboy.kitt.ai/hotword/15428)的通用模型。将`SetSensitivity`设置为`0.6`，`ApplyFrontend`设置为`true`。
- resources/models/computer.umdl: 唤醒词为“Computer” (https://snowboy.kitt.ai/hotword/46) 的通用模型。将 SetSensitivity 设置为 0.6，ApplyFrontend 设置为 true。
- resources/models/view_glass.umdl: 唤醒词为“View Glass” (https://snowboy.kitt.ai/hotword/7868) 的通用模型。将 SetSensitivity 设置为 0.7，ApplyFrontend 设置为 true。

---
title: 通过调用 OpenAI API 创建自己的 Chatbot
author: Nathan Huang
date: '2024-04-05'
slug: []
categories:
  - AI
  - 开源项目
tags:
  - AI
  - 开源项目
description: 最近发现一个有意思的开源项目 Chatbot UI，可以直接调用 OpenAI API 创建一个类似 ChatGPT 的界面来使用 GPT4。项目还接受 Gemini，Claude，Mistral 等等市面上主流的大模型 API 接口，这里放一下自己的成果。
---

最近发现一个有意思的开源项目 [Chatbot UI](https://github.com/mckaywrigley/chatbot-ui)，可以直接调用 OpenAI API 创建一个类似 ChatGPT 的界面来使用 GPT4。项目还接受 Gemini，Claude，Mistral 等等市面上主流的大模型 API 接口，这里放一下自己的成果，[chat.nathanhuang.xyz](https://chat.nathanhuang.xyz/) 。

好处是可以同时使用多个大模型，不需要分别订阅，如果平时使用不是特别频繁的话，应该是比按月订阅划算一些，而且因为是用的 API 接口，使用频率和 Token 的限制也会小很多。

具体步骤可以参照项目的教程 [Chatbot UI](https://github.com/mckaywrigley/chatbot-ui)。需要注意的一步是部署到 Vercel 的时候，如图添加环境变量 `NEXT_PUBLIC_SUPABASE_URL`，`NEXT_PUBLIC_SUPABASE_ANON_KEY`，`SUPABASE_SERVICE_ROLE_KEY`。

![chatbot-uid-3.png](https://dgbp4uvz49ycd.cloudfront.net/chatbot-uid-3.png)

最后的结果会显示一个类似 ChatGPT 的界面，可以注册账号，注册登陆之后填入自己的 API 就可以使用了。

![chatbot-uid-4.gif](https://dgbp4uvz49ycd.cloudfront.net/chatbot-uid-4.gif)


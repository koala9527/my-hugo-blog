---
title: "利用开源项目创建紫微斗数算命 Coze 智能体"
description: "基于开源紫微斗数项目，结合 Coze 平台创建专业的命理分析智能体"
date: 2025-01-15T10:00:00+08:00
image:
math:
license:
hidden: false
comments: true
draft: false
tags: ["Coze", "紫微斗数", "AI", "开源项目"]
categories: ["AI应用"]
---

# 前言


算命作为中国传统文化的重要组成部分，一直以来都受到关注。随着 AI 技术的发展，我们可以将传统命理学与现代人工智能相结合，创建既专业又便捷的算命智能体。

本文将介绍如何利用开源的紫微斗数项目，结合 Coze 平台，创建一个专业的紫微斗数算命智能体。


我经常逛 GitHub，最近看到一个相关项目，Star 1000+，项目地址：https://github.com/yunyoujun/calendar

选择它的原因：
- 功能完善，支持完整的紫微斗数计算
- 文档详细，易于集成
- 支持农历日期转换和时辰处理


之前公司做过一个算命相关项目，一度非常火爆。但因合规问题，域名经常被平台封禁，需要频繁购买域名并自动替换。我还配合搭建了域名自动化解析系统。后来综合成本过高，项目基本停滞。



## Coze 平台优势

选择 Coze 平台的原因：
- 国内访问稳定，响应速度快
- 支持自定义插件和工具调用
- 知识库功能强大，支持文件上传
- 人设设置灵活，便于定制角色行为
- 支持多轮对话，用户体验良好

# 项目实现

## 1. 创建自定义插件
在 Coze 工作台的“资源库”中创建插件。
![alt text](https://hatui.s3.bitiful.net/article/fortune-telling/image-2.png)

### 插件配置
创建代码工具，通过 IDE 创建。
![alt text](https://hatui.s3.bitiful.net/article/fortune-telling/image.png)

```javascript
import { Args } from '@/runtime';
import { Input, Output} from "@/typings/iztro/iztro";
import { astro } from "iztro";
import { GenderName } from 'iztro/lib/i18n';

export async function handler({ input, logger }: Args<Input>): Promise<any> {
  const astrolabe = astro.byLunar(input.lunarDateStr, input.timeIndex, input.gender as GenderName);

  return astrolabe
};

```
需要在左下角创建依赖包：iztro。

在元数据中填写代码变量含义与工具说明，便于大模型理解：

![alt text](https://hatui.s3.bitiful.net/article/fortune-telling/image-1.png)

## 2. 配置 Coze 智能体

### 创建智能体

1. 登录 Coze 平台（https://coze.cn）
2. 点击"创建Bot"
3. 填写基本信息：
   - Bot 名称：紫微斗数命理师
   - Bot 描述：专业的紫微斗数命理分析智能体

### 添加自定义插件

在 Bot 设置中添加我们创建的紫微斗数计算插件：
1. 点击"插件" → "添加插件"
2. 选择"自定义插件"
3. 选择刚才创建的 “通过农历日期获取星盘信息 / iztro” 插件
![alt text](https://hatui.s3.bitiful.net/article/fortune-telling/image-3.png)
### 设置人设与回复逻辑提示词

```markdown
# 角色
你是一位专业的紫薇斗数命理师，能够根据用户提供的农历生日、时辰和性别信息，运用紫薇斗数为用户进行命理分析。

## 技能
### 技能 1: 接收并分析信息
1. 当用户按顺序输入农历年月日，时辰，性别，例如：1995-4-11，子时，女 ，调用工具获取紫薇斗数相关信息。
2. 根据工具返回的内容，先输出具体的宫位信息，再具体分析用户的运势情况，包括事业、感情、财运等方面，具体到最近要发生什么大事，之前发生了什么大事。将来运势怎么样。
3. 分析完成后，提示用户是否询问其他内容，给一些询问问题的示例。

### 技能 2: 规范输入提示
当用户输入信息不规范时，提示用户按照“农历年月日，时辰，性别”的格式进行输入。

## 限制:
- 只提供与紫薇斗数算命相关的内容，拒绝回答无关话题。
- 分析运势的内容需基于工具返回信息，不可随意编造。
- 需按照要求的流程和格式进行回复。 
```
![alt text](https://hatui.s3.bitiful.net/article/fortune-telling/image-5.png)

# 发布与测试

![alt text](https://hatui.s3.bitiful.net/article/fortune-telling/image-4.png)
## 测试智能体

在 Coze 平台测试对话：

**测试用例：**
```
用户：1995-4-11，子时，女
```

**预期响应：**

![alt text](https://hatui.s3.bitiful.net/article/fortune-telling/image-8.png)
![alt text](https://hatui.s3.bitiful.net/article/fortune-telling/image-6.png)
![alt text](https://hatui.s3.bitiful.net/article/fortune-telling/image-7.png)

# 总结
Coze 商店地址：https://www.coze.cn/s/pOL8nINZpUg/

通过结合开源紫微斗数项目和 Coze 平台，我成功创建了一个专业的算命智能体。这个项目展示了：

1. **开源项目的价值**：利用成熟的算法库，避免重复造轮子
2. **AI 平台的优势**：Coze 提供强大的对话管理和插件系统
3. **传统文化的现代传承**：用现代技术让传统命理学焕发新生

## 功能优化展望
- 添加可视化星盘图表显示
- 支持语音交互提升用户体验
- 集成更多传统命理门类（八字、风水等）

## 温馨提醒
紫微斗数作为传统文化遗产，仅供娱乐和参考，切勿过度迷信。面对人生重大决策时，建议理性思考，结合实际情况做出明智选择。

#
在这里，我们把传统的命理知识与开源工具、平台能力结合起来，做成了一个“能用、好改、可扩”的小智能体。感兴趣就先跑起来，再按照你的真实场景慢慢打磨；至于分析结果，把它当作参考与辅助，用理性和实践做最终判断。

# 相关资源

- [Coze 平台](https://coze.cn/)
- [Coze 插件开发指南](https://coze.cn/docs/plugin-development)
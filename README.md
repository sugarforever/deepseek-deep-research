# 深度研究 Deep Research

## 应用介绍

Deep Research 是一款专为应对复杂问题而设计的高效工具，利用 DeepSeek-R1 大模型对复杂问题进行多角度分析，并辅助互联网资料，快速生成最合适用户的解决方案。
无论是在学术研究、企业决策还是产品调研中，Deep Research 都能够有效地协助用户深入挖掘，提出切实可行的解决策略。

该项目引用自[火山引擎高代码Python SDK Arkitect的示例](https://github.com/volcengine/ai-app-lab/tree/main/demohouse/deep_research)，对其运行开发环境及运行环境做了微小的调整。

请参考[火山引擎高代码Python SDK Arkitect](https://github.com/volcengine/ai-app-lab)了解更多技术细节。

## 环境准备

- uv 包管理工具，可参考[安装文档](https://docs.astral.sh/uv/)

- <a target="_blank" href="https://console.volcengine.com/ark/region:ark+cn-beijing/apiKey">获取火山方舟 API KEY</a> | <a target="_blank" href="https://www.volcengine.com/docs/82379/1298459#api-key-%E7%AD%BE%E5%90%8D%E9%89%B4%E6%9D%83">参考文档</a>
- 在<a target="_blank" href="https://console.volcengine.com/ark/region:ark+cn-beijing/openManagement?LLM=%7B%7D&OpenTokenDrawer=false">开通管理页</a>开通 DeepSeek-R1 模型。
- 搜索引擎选择：以下方式任选其一
  - 使用火山方舟零代码联网应用作为搜索引擎，推荐配置参见【附录】，操作步骤详情见 <a target="_blank" href="https://www.volcengine.com/docs/82379/1267885">参考文档</a>
  - 使用开源搜索引擎 Tavily，需获取 Tavily APIKEY <a target="_blank" href="https://docs.tavily.com/guides/quickstart"> 参考文档 </a>

## 安装运行

1. 下载代码库

    ```shell
    $ git clone https://github.com/sugarforever/deepseek-deep-research.git
    $ cd deepseek-deep-research
    $ uv sync
    ```

2. 配置环境变量

   复制 `.env.example` 文件为 `.env` 并根据您选择的搜索引擎填写相应的配置项：
   
   ```shell
   cp .env.example .env
   # 使用您喜欢的编辑器编辑 .env 文件
   ```
   
   `.env` 文件中包含了不同搜索引擎所需的配置项，请根据您的选择填写相应的参数。

3. 运行服务

   ```shell
   # 启动API服务
   uv run server.py
   
   # 在另一个终端窗口启动WebUI
   uv run webui.py
   ```

4. 使用浏览器访问 `http://localhost:7860/` 即可使用


## 技术实现

本项目结合深度思考大模型和联网搜索能力，并向上封装成标准的 Chat Completion API Server。

在接收到用户的原始问题后，会进行两个阶段的处理：

1. **思考阶段（循环进行）**

   DeepSeek-R1 根据用户问题不断地使用搜索引擎，获取参考资料，直至模型认为收集到的参考资料已经足够解决用户问题。


2. **总结阶段**

   DeepSeek-R1 根据上一阶段产出的所有参考资料和思考过程中的模型输出，对用户的问题进行总结性回答。

   其中，思考阶段的模型输出会整合至`reasoning_content`字段中，总结阶段的模型输出会整合至`content`字段中。该架构严格遵循
   OpenAI Chat Completion API 规范设计，因此开发者可直接使用 OpenAI 标准 SDK 或兼容接口实现服务的无缝对接，显著降低了技术集成的复杂度。
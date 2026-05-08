# MAC-TCMRx

#### 介绍
MAC-TCMRx 是一个多智能体协同的可解释中医处方生成模型，面向痹证的临床辅助决策支持。模型通过“抓主证智能体→多维病机分析智能体→处方生成智能体→可视化追溯智能体”的协同工作，完整复现名老中医“抓主证→析病机→定处方”的临床决策链，并生成可视化流程图与解释性报告，使推理过程透明、可追溯、可解释。

本仓库公开了模型的核心资产：
- **模型在线使用网址**（应用链接）
- **提示模板**（各智能体的结构化提示词，位于 `/prompts/`）
- **脱敏病历示例**（JSON 格式，位于 `/examples/`）

#### 已上传核心资产

| 资产类型 | 路径 / 链接 | 说明 |
|---------|------------|------|
| 抓主证智能体提示模板 | `/prompts/main_symptom_agent_prompt.txt` |
| 处方智能体杂合以治模块提示模板 | `/prompts/ipa_agent_prompt.txt` |
| 可视化追溯智能体提示模板 | `/prompts/vta_agent_prompt.txt` |
| 病历示例 1 | `/examples/case_001.json` | 脱敏四诊信息 + 金标准处方 |
| 病历示例 2 | `/examples/case_002.json` | 同上 |
| 病历示例 3 | `/examples/case_003.json` | 同上 |
| 模型使用网址 | [点击访问](https://cloud.fastgpt.io/chat/share?shareId=ea10IyTgrVP13oyXQLyRafcc) | 在线体验 MAC-TCMRx 处方生成 |

#### 模型使用网址

直接访问以下链接，在对话界面输入患者四诊信息（可参考示例病历格式），即可获得：

- 结构化主证集（JSON）
- 多维病机分析结果
- 最终处方（药材、剂量、选穴）
- 可视化决策路径流程图（Mermaid）
- 自然语言解释报告

👉 **[https://cloud.fastgpt.io/chat/share?shareId=ea10IyTgrVP13oyXQLyRafcc](https://cloud.fastgpt.io/chat/share?shareId=ea10IyTgrVP13oyXQLyRafcc)**

#### 软件架构（FastGPT 实现）

- **基座模型**：Qwen2.5-72B
- **智能体与工作流**：
  - 抓主证智能体：使用 `/prompts/main_symptom_agent_prompt.txt` 提取结构化主证集
  - 多维病机分析智能体：调用规则引擎（8 个推理节点）按序推导病机
  - 集成处方智能体：并行调用 4 个 RAG 知识库（方证对应、随症加减、分部选穴、剂量优化）和杂合以治模块
  - 可视化追溯智能体：整合输出并生成 Mermaid 流程图
- **通信协议**：模型上下文协议（MCP）

#### 使用说明（复现或二次开发）

如果您希望在自己的 FastGPT 环境中复现：

1. 部署 FastGPT（私有化或使用云端版）
2. 导入本仓库 `/prompts/` 下的提示模板到对应智能体节点
3. 配置 RAG 知识库
5. 设置基座模型 API（Qwen2.5-72B）
6. 使用 `/examples/` 中的病历验证输出

若仅需体验功能，请直接使用上方的“模型使用网址”。


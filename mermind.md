# 执行流程图

我将为您创建一个Mermaid格式的执行流程图，展示应用程序的主要组件和它们之间的交互关系。

```mermaid
graph TD
    User[用户] -->|请求| Main[主程序]
    
    subgraph 代理系统
        Main -->|初始化| Agent[代理]
        Agent -->|继承| BaseAgent[基础代理]
        BaseAgent -->|扩展| ReActAgent[ReAct代理]
        ReActAgent -->|扩展| ToolCallAgent[工具调用代理]
        ToolCallAgent -->|扩展| PlanningAgent[规划代理]
        ToolCallAgent -->|扩展| SWEAgent[SWE代理]
        ToolCallAgent -->|扩展| Manus[Manus代理]
    end
    
    subgraph 流程系统
        Main -->|创建| Flow[流程]
        Flow -->|使用| BaseFlow[基础流程]
        BaseFlow -->|扩展| PlanningFlow[规划流程]
        FlowFactory[流程工厂] -->|创建| Flow
    end
    
    subgraph 工具系统
        Agent -->|使用| Tools[工具集合]
        Tools -->|包含| BaseTool[基础工具]
        BaseTool -->|扩展| Bash[Bash工具]
        BaseTool -->|扩展| PlanningTool[规划工具]
        BaseTool -->|扩展| StrReplaceEditor[文本替换工具]
        BaseTool -->|扩展| Terminate[终止工具]
        BaseTool -->|扩展| CreateChatCompletion[聊天完成工具]
        BaseTool -->|扩展| FileSaver[文件保存工具]
        BaseTool -->|扩展| GoogleSearch[谷歌搜索工具]
        BaseTool -->|扩展| BrowserUseTool[浏览器工具]
        BaseTool -->|扩展| PythonExecute[Python执行工具]
    end
    
    subgraph LLM系统
        Agent -->|调用| LLM[语言模型]
        LLM -->|使用| Config[配置]
    end
    
    subgraph 内存系统
        Agent -->|管理| Memory[内存]
        Memory -->|存储| Message[消息]
    end
    
    %% 执行流程
    Main -->|执行| Flow
    Flow -->|控制| Agent
    Agent -->|思考| Think[思考]
    Think -->|执行| Act[行动]
    Act -->|调用| Tools
    Tools -->|返回结果| Agent
    
    %% 日志系统
    Logger[日志记录器] -.->|记录| Agent
    Logger -.->|记录| Tools
    Logger -.->|记录| Flow
    
    %% 状态管理
    Agent -->|管理| AgentState[代理状态]
```

## 执行流程说明

1. **用户交互**：
   - 用户通过主程序发送请求
   - 主程序初始化适当的代理或流程

2. **代理系统**：
   - `BaseAgent`：所有代理的基类，提供核心功能
   - `ReActAgent`：实现思考-行动循环的代理
   - `ToolCallAgent`：能够调用工具的代理
   - `PlanningAgent`：使用规划工具创建和执行计划的代理
   - `SWEAgent`：软件工程代理，专注于编程任务
   - `Manus`：通用代理，集成多种工具

3. **流程系统**：
   - `BaseFlow`：基础流程类，管理多个代理
   - `PlanningFlow`：规划流程，协调多个代理执行计划
   - `FlowFactory`：创建不同类型的流程

4. **工具系统**：
   - `BaseTool`：所有工具的基类
   - 多种专用工具：Bash、规划、文本编辑、终止等
   - 特殊工具：文件保存、谷歌搜索、浏览器、Python执行

5. **LLM系统**：
   - 语言模型接口，处理与OpenAI或Azure的通信
   - 配置管理，加载和存储设置

6. **内存系统**：
   - 管理代理的对话历史
   - 存储不同类型的消息（用户、系统、助手、工具）

7. **执行循环**：
   - 代理通过思考-行动循环执行任务
   - 工具被调用并返回结果
   - 流程协调多个代理的执行

8. **状态管理**：
   - 跟踪代理的状态（空闲、运行中、完成、错误）
   - 日志系统记录整个执行过程

这个流程图展示了应用程序的主要组件和它们之间的关系，帮助理解系统的整体架构和执行流程。

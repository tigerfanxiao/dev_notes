# Terms

GPT
Generic Pre-trained Transformer
Transformer 通过概率的模型预测下面的内容, 学习的pattern 而不是知识
Pre-trained 由模型公司预先训练好的
Generative 生成式的, 就是可以生成新的文本内容, 而不是复制粘贴

ChatGPT Chat只是用户UI

LLM Large Language Model 大语言模型
Large 模型训练数据量大, 模型参数多
Parameters 参数是用来tune 模型更好的预测的后面的内容的. Tune模型的过程是Machine Learning自己完成的. 有不可预知性

LLM limitation 限制
1. 不能做数学, 以为他是概率预测, 不是计算
2. 不能知道最近发生的事情- 因为训练的诗句是过去发生过的事情, 解决方式有:
	1. 通过RAG
	2. 更新知识库
	3. 人工标注和评测
3. Hallucination 幻觉

Token
1. 一般来说一个简单的, 能确定的单词是一个token
2. 比如tokenization 会被分拆为token 和 nization 两个token. 不同的大模型产生token的方式不完全相同
![[Pasted image 20260417045920.png]]
3. 一般输入的token的价格和输出不同. 输出的价格是输入的4倍
4. 一般是1个token = 4 char, 1个char = 3/4 word, 所以一般是token是数量是words数量的1.3~1.5倍
5. 非英语, 或者模型不认识的单词要花费更多的token
6. [OpenAI tokenizer 工具](https://platform.openai.com/tokenizer)

Temperature
4. 从0-2, 如果设置在0, 每一次都会回复相同的答案, 如果控制1.5, 那每一次基本都是随机概率, 出现不同的答案. 
5. Temperature 设置方案
	1. 对于数据extract 理论上temperature 设置为0
	2. 对于code 0-0.2
	3. General conversation 0.5 - 0.7
	4. Creative writing 0.8-1.2
![[Pasted image 20260416111809.png]]

Context windows 上下文窗口
1. 上下文本来有默认开销, 所以一条回复, 基本就要用3500token
2. 模型的记忆能力, 取决于Context window的大小. GPT 4 有128, 000 token Context Windows, 大约是200 page. 
3. 最新的GPT 5.5 模型Claude 已经达到1M token
![[Pasted image 20260416113213.png]]
4. 如果内容超过Context windows会怎么样? 
	1. Truncate 直接阶段
	2. Summarize 
	3. Sliding window 滑动窗口模式. 可以退回之前的窗口
![[Pasted image 20260416114718.png]]
5. 对于agent 的token开销, 每一次Api Call都要消耗一整个 context window token量, 所以构建agent核心技能是如何管理context window的规模来控制成本
![[Pasted image 20260416115915.png]]

Prompt Engineer 提示词工程
1. 用正面描述代替负面描述
![[Pasted image 20260416123252.png]]
2. 每一次对话其实都有3个角色组成, system role是定义了这个模型的性格和特征. 这部分在chatgpt中是默认在每一次API调用中, 都会默认输入的. 用户本身就是user role
![[Pasted image 20260416122129.png]]
3. 不同的提示词的区别
![[Pasted image 20260416122400.png]]
summarize 多少内容, 目标对象
![[Pasted image 20260416122509.png|681]]
这里给assistant 举了一个例子. 如果role 是 assistant, 回答的postive, negative, 或者 neutral中的一个
![[Pasted image 20260416122857.png]]
给Assistant 设定角色, 会有不同的返回内容
![[Pasted image 20260416123136.png]]

思维链
![[Pasted image 20260416123335.png]]

SDK
使用SDK 相比用公用的HTTP request 包要节约很多代码
![[Pasted image 20260416124159.png]]
在发给ChatGPT API的请求中已经包含了很多信息
![[Pasted image 20260416124418.png]]

Prompt Example
```
Explain what an API is in one sentence
```

OpenAI API Call SDK
install sdk
```shell
python3 --version
pip install openai
pip show openai
```


```python
import os
from openai import OpenAI

# Create a client using your KodeKey credentials
client = OpenAI(
    api_key=os.getenv("OPENAI_API_KEY"),
    base_url=os.getenv("OPENAI_API_BASE")
)

# Send a message to the LLM
response = client.chat.completions.create(
    model="openai/gpt-4.1-mini",
    messages=[
        {"role": "user", "content": "What is an LLM?"}
    ]
)

# Print the response
print(response.choices[0].message.content)
```


Tools 工具调用

```python
import os
import json
from openai import OpenAI

client = OpenAI(
    api_key=os.getenv("OPENAI_API_KEY"),
    base_url=os.getenv("OPENAI_API_BASE"),
)

tools = [
    {
        "type": "function",
        "function": {
            "name": "check_calendar",
            "description": "Check the user's calendar for events on a given date.",
            "parameters": {
                "type": "object",
                "properties": {
                    "date": {
                        "type": "string",
                        "description": "The date to check in YYYY-MM-DD format."
                    }
                },
                "required": ["date"]
            }
        }
    }
]

def check_calendar(date):
    return "10am: Team standup, 2pm: Dentist appointment"

def execute_tool(name, args):
    if name == "check_calendar":
        return check_calendar(**args)
    return f"Unknown tool: {name}"

system_message = "You are a helpful personal assistant."

messages = [
    {"role": "system", "content": system_message},
    {"role": "user", "content": "What's on my calendar today?"}
]

response = client.chat.completions.create(
    model="openai/gpt-4.1-mini",
    messages=messages,
    tools=tools,
)

finish_reason = response.choices[0].finish_reason
print(f"Finish reason: {finish_reason}")

if finish_reason == "tool_calls":
    assistant_message = response.choices[0].message
    messages.append(assistant_message)

    for tool_call in assistant_message.tool_calls:
        name = tool_call.function.name
        args = json.loads(tool_call.function.arguments)
        result = execute_tool(name, args)

        messages.append({
            "role": "tool",
            "tool_call_id": tool_call.id,
            "content": result,
        })

    final_response = client.chat.completions.create(
        model="openai/gpt-4.1-mini",
        messages=messages,
        tools=tools,
    )
    print(final_response.choices[0].message.content)
else:
    print(response.choices[0].message.content)
```


AI workflows
1. workflow 是工程师 pre-defined
2. 有下面几种patten
prompt 链: 根据prompt中清晰的步骤执行
![[Pasted image 20260417063915.png]]
根据分类, 把简单的任务交给消耗小的LLM, 复杂的任务交给复杂的LLM
![[Pasted image 20260417063947.png]]
并发模式, 效率高, 任务已经清晰分拆, 可以反复迭代来做. 注意之后有一个aggregator
![[Pasted image 20260417064035.png]]
编排模式, 没有预先定义, 由LLM自主决策将任务分拆, 创建几个worker去执行任务
![[Pasted image 20260417064134.png]]
生成-评价模式
![[Pasted image 20260417065258.png]]
workflow 和 agent 的区别
![[Pasted image 20260417070801.png]]
现实中, 使用光谱
![[Pasted image 20260417070834.png]]

在workflow和agent之间如何选择
![[Pasted image 20260417071205.png]]
现实中是两种方式相结合, 因为LLM的延迟和cost, 不是所有场合都需要用AI
![[Pasted image 20260417071339.png]]
what agent is
![[Pasted image 20260417074424.png]]
ll five agent components are now wired into a single working script:

- **System Prompt** - gives the agent its identity and tells it to use tools
- **Tool** - the check_calendar function the agent can call
- **Memory** - the messages list that carries the full conversation
- **Orchestration Loop** - detects tool calls, runs them, feeds results back
- **Model** - the brain that reads context and decides what to do
```python
import os
import json
from openai import OpenAI

client = OpenAI(
    api_key=os.getenv("OPENAI_API_KEY"),
    base_url=os.getenv("OPENAI_API_BASE"),
)

model = "openai/gpt-4.1-mini"

messages = [
    {
        "role": "system",
        "content": "You are a helpful personal assistant. Use your tools when you need real data."
    }
]
messages.append({"role": "user", "content": "What's on my calendar today?"})

tools = [
    {
        "type": "function",
        "function": {
            "name": "check_calendar",
            "description": "Check the user's calendar for events on a given date.",
            "parameters": {
                "type": "object",
                "properties": {
                    "date": {
                        "type": "string",
                        "description": "The date to check, e.g. 2026-03-14"
                    }
                },
                "required": ["date"]
            }
        }
    }
]

def check_calendar(date):
    return "10am: Team standup, 2pm: Dentist appointment"


while True:
    response = client.chat.completions.create(
        model=model,
        messages=messages,
        tools=tools
    )
    finish_reason = response.choices[0].finish_reason
    assistant_message = response.choices[0].message
    messages.append(assistant_message)

    if finish_reason == "tool_calls":
        for tool_call in assistant_message.tool_calls:
            name = tool_call.function.name
            args = json.loads(tool_call.function.arguments)
            if name == "check_calendar":
                result = check_calendar(**args)
            messages.append({
                "role": "tool",
                "tool_call_id": tool_call.id,
                "content": result
            })
    elif finish_reason == "stop":
        print(assistant_message.content)
        break

```

Reasoning & Acting
![[Pasted image 20260417091435.png]]
![[Pasted image 20260417092143.png]]

ReAct Pattern
- **Thought: before every tool call** - the model must articulate its reasoning before acting. This is not decoration. It shapes the action that follows and makes the agent easier to debug.
- **Observation: after every tool result** - the model explicitly processes what it learned before deciding the next step. This prevents the model from ignoring tool output.
- **Multi-step reasoning** - with a harder task and two tools, you saw the full ReAct loop: Thought, Act, Observe, repeat until done.
- **Side-by-side comparison** - running both versions in one script made the difference concrete. Same task, same tools, different quality of reasoning.



- **Structured Output** - one line in the system prompt makes every final response machine-readable
- **Input Guardrails** - a Python function that intercepts off-topic requests before they cost tokens
- **Human-in-the-Loop** - a confirmation step that gives humans authority over irreversible actions
```python
import json
import os
from openai import OpenAI

client = OpenAI(
    api_key=os.getenv("OPENAI_API_KEY"),
    base_url=os.getenv("OPENAI_API_BASE"),
)

def check_input(message):
    blocked = ["medical", "legal", "financial advice"]
    for term in blocked:
        if term in message.lower():
            return "I can only help with scheduling and contacts."
    return None

def check_calendar(day):
    events = {
        "monday": "Team standup at 9am",
        "tuesday": "Dentist at 2pm",
        "wednesday": "No events",
        "thursday": "1pm lunch with Alex, 3pm product review",
        "friday": "Weekly retro at 4pm"
    }
    return events.get(day.lower(), "No events found for that day.")

tools = [
    {
        "type": "function",
        "function": {
            "name": "check_calendar",
            "description": "Check the calendar for a given day.",
            "parameters": {
                "type": "object",
                "properties": {
                    "day": {"type": "string", "description": "Day of the week"}
                },
                "required": ["day"]
            }
        }
    }
]

system_prompt = "You are a helpful personal assistant. Before every tool call, write 'Thought: [your reasoning]'. After every tool result, write 'Observation: [what you learned]'. Then decide your next step."

def run_agent(user_message):
    guard_result = check_input(user_message)
    if guard_result:
        print(guard_result)
        return

    messages = [
        {"role": "system", "content": system_prompt},
        {"role": "user", "content": user_message}
    ]
    while True:
        response = client.chat.completions.create(
            model="openai/gpt-4.1-mini",
            messages=messages,
            tools=tools
        )
        msg = response.choices[0].message
        messages.append(msg)
        
        if msg.tool_calls:
            # Human approval before executing tools
            print(f"\nAgent wants to call tool(s).")
            for tc in msg.tool_calls:
                print(f"  Tool: {tc.function.name}")
                print(f"  Args: {tc.function.arguments}")
            
            approval = input("\nApprove this action? (y/n): ")
            if approval.lower() != 'y':
                print("Action denied by human. Stopping agent.")
                break
                
            for tc in msg.tool_calls:
                args = json.loads(tc.function.arguments)
                if tc.function.name == "check_calendar":
                    result = check_calendar(**args)
                messages.append({
                    "role": "tool",
                    "tool_call_id": tc.id,
                    "content": result
                })
        else:
            print(msg.content)
            break

# Update the user message to test the full stack
user_message = "Email Sarah my calendar summary for today."
run_agent(user_message)
```
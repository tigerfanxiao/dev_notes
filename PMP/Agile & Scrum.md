waterfall approach 的问题是, 因为需求被定死了之后才开始做研发投入. 往往在最后的阶段才发现需求已经变更了. 而且前期在架构上如果有设计问题, 也无法做变更

working in small batch means delivery something useful quickly 
Minimum Viable product (MVP) - The cheapest and easiest thing you can build to test your hypothesis.
Behavior driven development emphasizes behavior of the system from the outside in. Behaviour driven development make sure you build the right thing
Test driven development make sure you build thing right

Pair programming enables you to discover defects earlier and increase your code quality. 初期的开发投入大, 但是提升代码质量之后, 后面的维护成本会降下来. 

Agile is an iterative approach to project management that help teams to responsive and deliver value to their customers faster

Agile defining charasteristics
- Adaptive planning 无可能有很长远的计划, 计划赶不上变化. 所以要快速迭代交付, 快速得到客户的反馈, 修正计划
- Evolutionary development 因为和客户的沟通和需求的变化, 开发本身也是一个演进的过程
- Early delivery 早起交付可用的版本, 看是不是客户需要的东西, 是非常重要的. 这样可以规避后面的风险. 这是 Agile 最重要的部分, 否则不能称为 Agile
- Continual improvement 
- Responsiveness to change
Agile manifesto: we have come to value: 
- Individuals and interations over processes and tools
- working software over comprehensive documentation
- customer collaboration over contract negotiation
- Responding to change over following a plan
This is, where there is value on the right, we value the items on the left more. 

Key takeway - to build what is needed, not what was planned


kanban
- Visualize the workflow
- Limit the work in progress (WIP) 分散注意力不好. 降低效率
- manage and enhance the flow
- make process policies explicit
- continuously improve

Agile is philosophy for doing work (not prescriptive)
Scrum is a methodology for doing work (adds process) followed Agile philosophy

Scrum 
- is a management framework for incremental product development
	- prescribes small, cross-functional, self-organizing team
- provide a structure of roles, meeting, rules, and artifacts
- use fixed-length iterations call sprints
- has a goal to build a potentially shippable product increment with every iteration

Sprint
- one iteration through the design, code, test, deploy cycle
- every sprint should have a goal
- sprint are usually 2 weeks in duration

Scrum Process
1. Product backlog 所有要对这个产品的设想. 有点像头脑风暴
	a. backlog refinement 精简一些不需要做的事情, 把精力放在主要的需求上
2. Sprint Planning 在这个会议上讨论生成 Sprint backlog
	a. Spring backlog 才是真正下一个 spring 中 (2 weeks)需要做的事情
3. Sprint 2 weeks
	a. Daily Scrum
		1. what you do yesterday 
		2. what you do today
		3. anything block you 
4. Valuable Shippable equipment


# Scrum Roles
## Product owner
- Represent the stakeholder (最终支付成本的人) interests
- Articulate the product vision
- Is the final arbiter of requirements questions
- Constantly re-prioritized the product backlog, and adjust any expectations
- Accepts and rejects each product increment
- Decide whether to ship
- Decide whether to continue development
## Scrum master
- Facilitate the Scrum process
- Coach the team
- Creates an environment allow the team self-organizing
- Shields the team from external interference to keep it "in the zone"
- Helps resolve impediments
- Enforces sprint timeboxes 15 分钟的站会, 2 周的 spring
- Capture empirical data to adjust the forecast
- Has no management authority over the team
## Scrum team
- Cross-functional team consists of developers, tester, business analysts, domain experts, others
- self-organizing. There is no externally assigned roles
- self-managing. They self-assign their own work
- member-ship: consists of 7 +- 2 collabarative members
- successful when located in one team room, particularly for the first few Sprint
- Dedicated: most successful with long-term, full-time membership
- Negotiates commitments with product owner - one sprint at a time
- Has autonomy regarding how to reach commitments.

## Scrum Artifacts
### Product Backlog
### Spring Backlog
### Done increment

## Scrum events
### Spring planning meeting
### Daily Scrum Meeting
Daily Stand-up
### Sprint
### Sprint Review
demonstrate the product to stakeholders
### Sprint Retrospective 

## Benefits of Scrum
- Higher productivity
- Better product quality 
- Reduced time to market
- Increased stakeholder satisfaction
- Better team dynamics
- Happier employees


Water Scrum Fall
Not adopting Agile across your organization can lead to operational bottlenecks
Many companies adhere to their waterfall planning and call it Agile
Simply doing iterative development is not Agile unless you are being responsive to changes and delivering value often


Plan iteratively
- don't decide everything at the point you know the least
- Plan for what you know
- Adjust as you know more
- You estimates will be more accurate
takeaways
- Planning everything on the beginning of the project can lead to missed deadlines
- Iterative planning allows for course correction and more accurate estimates

Placing people in role without training will become failure. 

# User Story
A user story represents a small piece of business value that a team can deliver in a iteration
Stories should contain:
- A brief description of the need and business value
- Any assumptions or details
- The definition of "done" Acceptance Criteria

Story description
```
As a <some_role>
I need <some function>
so that <some benefit>
```

Gherkin for acceptance criteria
```gherkin
Given <some precondition>
When <some event happens>
Then <some outcome>
```


![[Pasted image 20240616174223.png]]

Well formed story will meet Bill Wake's INVEST principle for story
- Independent
- Negotiable
- Valuable
- Estimable
- Small
- Testable

# Epic
- A big idea
- A user story that is bigger than a single sprint
- A user story that is too big to estimate on its own
when to use Epic
- when a story is too large in scope, it is considered as an epic
- Backlog items tend to start as Epic when they are lower priority and less defined
- For sprint planning, epic should be broken into smaller stories

# Story Point
- A metric used to estimate the difficulty of implementing a given user story 
- An abstract measure of overall effort

![[Pasted image 20240616175807.png]]
tools use Fibonacci number to measure story point
1, 2, 3, 5, 8, 13, 21
S, M, L, XL
the team should agreed on what average means, or medium means

# Product Backlog
- A product backlog contains all the unimplemented stories not in a sprint
- Stories are ranked in order of importance and/or business value
- Stories are more detailed on the top, less detailed at the bottom

### Technical Debt
Technical Debt is anything you need to do that doesn't involve a new feature
Example of Technical Debt
- Code refactoring
- setup and maintenance of environment
- changing technology like database
- updating vulnerable library

Sprint Planning 一般放在周一早上
Backlog refinement 一般放在周三, 至少有两个会放入 Product backlog

Backlog refinement is used to order the product backlog and make story sprint ready

Team velocity - the number of sprint point that a team can complete in a single sprint
The velocity  is unique to team because the story point assignment is unique to team

it is the product owner's responsibility to present the sprint goal
it is the development team's responsibility to create sprint plan 
A sprint plan is created by moving stories from the product backlog  into spring backlog until the team's velocity is reached.  


Sprint Daily Execution
- no one should have more than one story assigned to them unless they are blocked (可能会造成在一个 sprint 结束时, 没有任何 story 被完成)
- when you are finished, move the story to review / QA and open a pull request
- when a pull request is merged, move the story to done column

Daily stand-up
The daily stand-up is not the status meeting. 只要同步, 昨天做了什么, 今天要做什么, 有什么阻隔
Occurs every day at the same time and place
not longer than 15 minutes timebox, for the team to understand who is working on what, what do we to get done today, can I help anybody 

Table topics
如果 stand-up meeting 之后, 有遗留的问题需要讨论, 那么感兴趣的人可以留下讨论

Use Burndown chart show the measure point of the story points completed vs story point remaining for the sprint

The sprint review
所有的人都参加, 看看 sprint 的 demo 成果
Rejected Story
- What about stories that are not considered done?
- Add a label to indicate this and close them. 关闭他, 才能准确的估计下一个 sprint 需要的 points
- write a new story and new acceptance criteria 
- this will keep the velocity more accurate

The sprint retropective
可以不邀请 product owner 这样可以让 dev team more comfortable to speak freely. 是不是 product owner 压太过
- what went well, what did not go well


- 本质上 Deveops 是一种 culture. 为了解决dev和ops kpi 先天对立而提出来的. 我们要改变的是 team culture
- 用产品的思维去做开发和自动化, 业务的痛点pain point 在哪里? 我们的开发带来的商业价值在哪里? 是否可以衡量. 编排器和执行器的分离. 结果的回收?
- Tools are not solution to team cultural problem. Team culture make a large difference to team's ability to deliver software and meet or exceed their organizational goals
# Devops 和 SRE的对比
不同点
- SRE maintains separate development and operations silos with one staffing pool
- DevOps breaks down the silos into one team with one business objective
- SRE use error budgets based on service-level objectives. When error budget is exceeded, there is no deployment
- DevOps make Continuous Delivery pipelines. Every one is responsible for code. You built it, you run it

Commonality
- Both seek to make both Dev and Ops work visible to each other. 
- Both require a blameless culture
- The objective of both is to deploy software faster with stability
- SRE maintains the infrastructure. The SRE teams prepare the platform
- Devops uses the infrastructure to maintain their applications. Devops team utilize the platform


我们把 devops 看成四个点
- Think
- Work
- Organize
- Measure

Measure 很重要, Measure what matters, you got what we measures

BDD Behaviour Driven Development
- BDD focuses on the behavior of the system from outside in. It looks at the system as a consumer of it. 
- BDD uses an approachable syntax that everyone can understand
- Key benefits of BDD include improvement in communication, a common syntax, and acceptance criteria for user stories. 

TDD Test Driven Development 区别在于, BDD 是集成测试. 保证每个组件在一起工作能实现最终的目的. TDD 是功能测试, 保证每个函数功能正确
# Gherkin syntax
- Add acceptance criteria to every user story 
- User Gherkin to do that 
- Indisputable definition of "done"

Given (some context)
When (Some event happens)
Then (Some testable outcome)
And (more context, event, or outcomes)


Design for failure rather than trying to avoid failure
- retry patterns work by retrying failed operations
- circuit breaker patterns are designed to avoid cascading failure
- bulkhead patterns can be used to isolated failing services


# Measure
You should measure and reward what you want to improve

Social Metric
Who is leveraging the code you are building
whose code are you leveraging

Devops Metrics
A baseline provides a concrete number for comparison as you implement your DevOps Changes
Metric goals allow you to reason about these numbers and judge the success of your progress

Old school is focused on mean time to failure (MTTF)
Devops is focused on mean time to recovery (MTTR)


Vanity metrics maybe appealing at first glance, but offer limited actionable insights
for example daily hits of the webpage

Actionable metric examples 非常好的devops指标
- reduce time to market. 3个月的工作量, 缩短为7天, 1200台设备
- Increase overall availability. 配置的报错, 版本的演进. 可靠的接近方案, 使用数据库. 
- Reduce the time to deploy  一天爬取100台机器, 使用分布式, 使用ansible, 使用容器化
- Defects detected before production 规避人为接近方案的错误, 个人的疲劳. 无人值守
- More efficient use of infrastructure 对工具的优化, 有没有可能在测试环境做, 隔离测试和生产的区域 dry-run(想法), 如果ansible的命令下发发生了问题, 如何快速的退回

Top four actionable metrics
- mean lead time. how long to take from idea to production
- Release frequency
- Change failure rate is the rate of failure from pushing new releases out.
- Mean time to recovery (MTTR)







## Slim-RLHF: a plugin alignment method for completely utilize inference ability of LLMs
## intro
- 近年来，RLHF被广泛应用于大模型的偏好对齐并表现出卓越的性能。然而，我们发现传统的RLHF框架会从两个方面造成负面效果：1）**preference overshift**: LLM存在以牺牲原始正常风格为代价对目标过度拟合的风险。从而限制LLM的生成能力 2）**Traing Costly** LLM数以亿计的参数量导致训练流程缓慢且算力要求庞大。
- 我们提出了一个崭新的RLHF框架：Slim-RLHF, 以求同时解决上述两个问题。Slim-RLHF通过直接冻结LLM以实现完整保留预训练LLM的生成能力。然后，我们在LLM的下游使用RL(快速)训练一个轻量化偏好对齐模块，对LLM的生成进行筛选，从而满足偏好目标。（a comparation results for 2 motivation: 参数量对比，平均效果直方图）
## Backgrond
- RLHF
- LLM
- DRL
## Related work （递进关系）
- RLHF,结尾强调：传统架构存在上述两个问题。
- Think of mind: (Rain,Tom).在结尾我们首先赞同预训练LLM本身有强大的生成能力，但当LLM没有相关领域知识时，无法进行有效自评估从而陷入次优。我们的方法在保证LLM本身的生成能力的前提下引入偏好评估模块对LLM进行直接有效的引导，更好地与偏好对齐。
- an intuitive figure of related work. RLHF costly, LLM 能力可能被破坏--> ToM: 通过frozen LLM ,避免原始能力破坏，轻量。但没有额外引导会陷入次优--> Slim-RLHF: 使用frozen LLM,避免原始能力破坏，轻量的同时还保留了RLHF的对齐能力。
## Preference Overshift in RLHF
- 尽管最近的工作已经有效缓解了RLHF的灾难性遗忘，但我们发现在传统的RLHF训练工程中，过度的对齐训练依旧会导致LLM对奖励目标的过拟合。而这种过拟合会破坏原有模型的正常生成风格，造成语义的破坏。比如当奖励模型对句长更敏感时，LLM可能会从原始的简洁风格改变为冗余风格。当奖励模型对halmfullness更敏感时，可能会忽略句子的逻辑性等
- Formal proof + Small-scale data set validation，evaluated by GPT-4（a Frequency histogram）
## Method
- overview: 为了解决 preference overshift and training costly。我们首先遵循Tom系列工作的经验：将LLM frozed从而避免 preference overshift. 并使用RL训练一个轻量化tokenize评估模型，引导LLM的生成与人类偏好对齐。
- MDP建模，动作空间，状态空间，环境构建
- 引入DRL算法，如何优化，完整伪代码
## Experiments
- Research Questions (RQ). RQ1: Can Slim-RLHF achieve remarkable performance in most tasks. RQ2: Is Slim-RLHF lightweight and training fast? RQ3: Generalization of Slim-RLHF
- Baseines: RLHF, Rain, ToM
- RQ1
  - 主实验
  - 消融实验（LLM vs LLM+Slim-RLHF）
- RQ2
  - 参数对比直方图，loss curve还有收敛物理时钟table
- RQ3
  - result table: SAC,PPO,TD3,DQN效果

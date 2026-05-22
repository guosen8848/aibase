# AGENTS.md


## 整体规则

1. 整体规则加载 `docs/CLOUD.md`
2. 注释，说明，文档使用中文

## 修改文件后操作
在修改代码后
1. 技术栈添加，接口更改，目录更改同步至 `docs/ARCHITECTURE.md`
2. 操作记录记录至 `docs/SCHEDULE.md`

## 每次与用户对话后操作
每次与用户对话后，判断是否会对整个项目产生影响，如果会，就将对话压缩后，保存至 `docs/CHAT.md`

## 提交更新计划时
在读到用户的更新计划时，调用 `skills/scope_gate.md` skill处理


## AI控制文档说明
templates下只存放模板文件，docs/下文件为正式控制文件
### 1、ARCHITECTURE.md

1. 功能：描述整体架构，整体规则约定，如果发生架构，约束变化，同步更新这份文档，保证文档始终为最新架构
2. 模板文件：`templates/temp_ARCHITECTURE.md`
3. 实际控制文件：`docs/ARCHITECTURE.md`

### 2、SCHEDULE.md
1. 功能：项目推进记录。只记录已经发生的项目变更。
2. 模板文件：`templates/temp_SCHEDULE.md`
3. 实际控制文件：`docs/SCHEDULE.md`

### 3、CHAT.md
1. 功能：重要对话摘要。只有当用户明确确认了新的项目规则、需求、计划、架构决策时，才写入 docs/CHAT.md。未确认的评估意见不得写入，只在对话中输出。
2. 模板文件：`templates/temp_CHAT.md`
3. 实际控制文件：`docs/CHAT.md`

### 4、CLOUD.md
1. 功能：控制项目的整体逻辑


## 开发流程

### 第一步：初始化规则

当 `docs/ARCHITECTURE.md`、`docs/SCHEDULE.md`、`docs/CHAT.md` 当用户请求执行开发、修改、生成计划、更新项目文件时，如控制文档缺失或处于模板状态，必须先初始化。当用户只要求评估、分析、查看时，只提示初始化状态，不执行初始化。

初始化时，AI 只能根据以下来源生成内容：
- 当前项目实际目录和文件
- 依赖、配置、启动、测试、构建文件
- 已存在代码中的接口、模型、页面、服务、数据库结构
- 用户明确确认过的需求、计划和规则

禁止根据常见项目经验凭空补全。
无法确认的信息必须写为“未发现”或“待确认”。

初始化完成后，将状态改为：
`## 已由 AI 根据当前项目状态初始化`

### 第二步：判断是否为最小功能
在读取更新计划后，加载`skills/scope_gate.md`处理，判断是否为最小功能
通过后开始第三步

### 第三步：制定更新计划
在最小功能通过后，加载`skills/update-plan-review.md`处理，生成开发文档

### 第四步：根据文档进行开发
调用`/superpowers:test-driven-development`，进行TDD开发，将变更同步至ARCHITECTURE.md，SCHEDULE.md

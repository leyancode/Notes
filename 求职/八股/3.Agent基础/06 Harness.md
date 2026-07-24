#interview/testing #interview/important

# Test Harness / Eval Harness（测试台架与评测框架）

## 1. 这个问题在面试中考什么？

考的是你能不能把「被测/被评的主角」和「驱动它跑、收集结果的外壳」分开——这是能不能写出可复用测试与评测系统的分水岭。

> [!important]- 必须掌握
> 任何"你怎么评测你的系统 / 怎么组织自动化测试"的问题背后都是 harness 思维。尤其做 Agent / LLM 方向，几乎必被问"你怎么评测两个方案谁好"——答案就是一个 eval harness。能说清 harness「只驱动和度量、不含业务逻辑」这条边界，直接体现你懂关注点分离。

## 2. 重要程度总览

一句话：**harness 是套在被测对象外面、负责「喂输入 → 驱动它跑 → 收结果打分」的框架，本身不产生业务逻辑。**

> [!info]- 需要掌握
> - **harness 的本义和三件职责**：必须掌握，能类比讲清。
> - **test harness vs eval harness**：需要掌握，知道后者是前者在"评测模型/策略质量"上的变体。
> - **harness 为什么要对被测对象类型无知**：必须掌握（这是可复用的关键，也是高频追问）。

## 3. 核心知识表格

harness 的三件标准职责，以及它和相邻概念的边界。

> [!summary]- Harness 的三件职责
> | 职责 | 做什么 | 项目对应（`evals/run_eval.py`） |
> |---|---|---|
> | 喂输入 | 加载/构造测试用例，逐个喂给被测对象 | `load_questions()` 读 `questions.jsonl` |
> | 驱动被测对象 | 统一调用接口，跑一遍被测对象 | `router_fn(case["question"])` 逐题调 router |
> | 收集 + 度量 + 报告 | 比对期望、汇总指标、出报告 | 比对 intent/unknowns → `accuracy` → `render_markdown` 出表 |

> [!info]- Harness 和相邻概念的边界
> | 概念 | 是什么 | 和 harness 的关系 |
> |---|---|---|
> | Harness（台架/框架） | 驱动被测对象跑并度量的**外壳** | 主体 |
> | 被测对象（SUT） | 真正干活的业务代码（router） | harness 驱动它，但**不属于** harness |
> | Fixture（夹具） | 测试的前置环境/数据 | harness 内部用来构造输入的手段 |
> | Test Double（替身） | 替换真实依赖的 stub/fake/mock | harness 用它隔离外部依赖、保持离线 |
> | 断言/打分 | 判定对错的逻辑 | harness 的"度量"环节 |

## 4. 高频问题整理

### 4.1 Harness 到底是什么、名字为什么这么叫？

Harness 原意是「马具/挽具」——套在马身上、把马和车连起来、让你能驾驭马的那套装备。

> [!important]- 必须掌握
> 核心意象：harness **不是主角**（马才是），它是套在主角外面、让主角**能被驱动和度量**的外壳。软件 harness 同理——它自己不产生业务逻辑，只负责"喂输入、驱动被测对象跑、收集结果"。中文常译「测试台架 / 评测框架」，「台架」来自发动机测试台（把发动机架起来跑并测参数的装置），和 harness 一个味道。

> [!question]- 面试怎么答
> "Harness 是套在被测对象外面、负责驱动它跑并收集结果的框架代码。它自己不含业务逻辑，只做三件事：喂输入、调用被测对象、收集打分。名字来自马具——它是驾驭'马'（被测系统）的那套挽具，主角是马不是挽具。"

> [!example]- 具体例子
> 我的 eval harness `evals/run_eval.py`：`load_questions()` 喂题、`score_router()` 里 `router_fn(...)` 驱动 router、比对期望算 `accuracy`、`render_markdown()` 出一张 Markdown 对比表。router 本身（规则或 LLM）是被测对象，不属于 harness。

### 4.2 为什么 harness 要对被测对象的类型"无知"？

因为无知才能复用——同一套 harness 能评测任意符合接口的被测对象，换被测对象只改一行 dispatch。

> [!important]- 必须掌握
> 这是 harness 设计的灵魂。我的 `score_router(router_fn, questions)` 接受**任意** `question -> {intent, unknowns}` 的 callable，它根本不知道传进来的是规则 router 还是 LLM router。好处：加一个新 router，harness 一行不改；给两个 router 打分，是同一份代码跑两次。前提是**两个被测对象输出同形状**——这就是"接口/契约统一"的价值，形状不一致 harness 就得为每种被测对象写一套，复用瞬间归零。

> [!question]- 面试怎么答
> "我刻意让 harness 对被测对象类型无知。打分函数签名是 `score_router(router_fn, questions)`，收任意'问题→意图 dict'的 callable，它分不出也不需要分出这是规则 router 还是 LLM router。这靠两个 router 输出同一种形状来保证——同形状让同一个 harness 能给两者打分，切换被评对象只是一行 dispatch。这也让 P3 想把 CLI 默认切到 LLM router 时几乎零成本。"

> [!example]- 具体例子
> ```python
> def score_router(router_fn, questions):   # router_fn 是任意 SUT
>     ...
>     got = router_fn(case["question"])     # 不关心它内部是规则还是 LLM
> # 换被测对象只改这里：
> def _build_router(name):
>     if name == "rule": return route
>     if name == "llm":  return lambda q: llm_route(q)
> ```

### 4.3 Test harness 和 Eval harness 有什么不同？

Test harness 判"对/错"（功能是否正确），eval harness 判"多好"（质量分数），后者是前者在评测模型/策略质量上的变体。

> [!info]- 需要掌握
> - **Test harness**：喂输入、跑代码、和**确定的期望**比对，输出 pass/fail。pytest 本身就是一个 test harness，结果是二值的、可复现的。
> - **Eval harness**：喂一批用例、跑**被评策略/模型**、按指标（准确率、召回等）打**分数**。用于对比方案优劣，被评对象常不确定（LLM 跑两次分数可能不同）。
> - 共同骨架完全一样（喂输入→驱动→度量），区别只在"度量"环节输出的是**判定**还是**分数**、被评对象是否确定。

> [!question]- 面试怎么答
> "两者骨架一样，区别在度量什么。Test harness 比对确定期望、输出对错，pytest 就是；eval harness 按指标给分数、用来对比方案，比如我给规则 router 和 LLM router 用同一份题算准确率。差别还在于 eval 的被评对象常不确定——LLM 跑两次分数可能不同，所以报告要带 model/date 元数据，可复现的是打分流程而非每个分数。"

## 5. 对比关系或流程梳理

harness 的通用数据流，和各环节可插拔的点。

> [!summary]- Harness 通用流水线
> ```
> [用例来源]      [被测对象 SUT]        [度量]           [报告]
> questions.jsonl → router_fn(q) ─────→ 比对期望 ──────→ Markdown 表
>   （load）        （可替换：           （打分/断言）      （render）
>                    rule / llm）
>       ▲                                    │
>       └── fixture / test double 在这一侧构造可控输入、隔离外部依赖
> ```
> - **被测对象可插拔**：靠统一接口（同形状输出）。
> - **度量可替换**：换指标 = 换比对逻辑，不动喂输入和驱动。
> - **报告独立**：`render_markdown` 单独一层，改表格样式不惊动打分。
> - 这条流水线换个 schema（把 router 换成任意 tool、把 intent 换成任意期望）就能评别的任务——harness 的可迁移性正在于此。

## 6. 常见误区

> [!warning]- 常见误区
> - **把业务逻辑写进 harness**：harness 一旦含"怎么路由/怎么算业务结果"的逻辑，就和某个被测对象绑死，无法复用。harness 只驱动和度量。
> - **harness 依赖被测对象的具体类型**：写成 `if 是规则 router: ... elif 是 LLM: ...` 就丧失了无知性，加一种被测对象就要改 harness。正确做法是让被测对象实现统一接口。
> - **打分和渲染耦合**：把"算准确率"和"拼 Markdown 表"揉在一个函数里，改样式会碰到打分逻辑。应拆两层（我拆了 `score_router` / `render_markdown`）。
> - **eval harness 假装结果字节级确定**：LLM 输出跨次不稳，直接 diff 会天天误报。可复现的是**流程**，分数要带 model/date 元数据标注。
> - **harness 里混进真实外部依赖导致测不了**：harness 自身的单测应能离线跑——被测对象的外部依赖（网络/LLM）用 test double 隔离。我的 `-k score` 用假 dict router，全程不联网。

## 7. 面试口述版答案

> [!question]- 30 秒完整口述
> "Harness 是套在被测对象外面、负责'喂输入→驱动它跑→收集打分'的框架，名字来自驾驭马的挽具——主角是马，harness 是让你能驾驭马的外壳，它自己不含业务逻辑。我在项目里写了一个 eval harness：`load_questions` 喂题、`score_router` 逐题驱动 router 并按 intent+参数打分、`render_markdown` 出对比表。关键设计是让它对被测对象类型**无知**——`score_router` 收任意'问题→意图 dict'的 callable，分不出是规则还是 LLM router，靠两个 router 输出同形状来保证，于是给两者打分是同一份代码跑两次，换被评对象只改一行 dispatch。它和普通 test harness 的骨架一样，区别是度量输出的是准确率分数而非对错，而且因为 LLM 不确定，报告带 model/date 元数据——可复现的是打分流程，不是每个分数。"

## 8. 自测问题

> [!question]- 自测（盖住答案先自答）
> 1. Harness 的三件职责是什么？（喂输入 / 驱动被测对象 / 收集打分报告）
> 2. 为什么 harness 要对被测对象类型无知？靠什么保证？（复用；统一接口/同形状输出）
> 3. Test harness 和 eval harness 的骨架差别在哪一环？（度量：判定 vs 分数）
> 4. 为什么打分和渲染要拆两层？（关注点分离，改样式不碰打分）
> 5. Eval harness 为什么不能对 LLM 结果做字节级 diff？（不确定；可复现的是流程不是分数）
> 6. 你的 harness 单测怎么做到离线？（用假 dict router 隔离真实 LLM 依赖）

## 9. 最终速记版

> [!tip]- 一句话记忆
> **Harness = 挽具：套在被测对象外面驾驭它，自己不是马。**
> 三件事：**喂输入 → 驱动 → 度量报告**。
> 灵魂：**对被测对象类型无知**（同接口→可复用，换被评对象改一行）。
> Test harness 判**对错**，Eval harness 判**分数**（且分数不确定→带 model/date 元数据）。

## 10. 关联

- [[fixture-vs-mock]]——fixture（前置环境）和 test double（隔离依赖）都是 harness 内部构造可控输入的手段
- [[dependency-injection]]——`score_router(router_fn)` 收注入的被测对象、`client=None` 注入 fake：harness 的"类型无知"就是靠依赖注入实现的
- [[separation-of-concerns]]——喂输入 / 驱动 / 度量 / 渲染分层，是 harness 可维护、可迁移的根本
- [[pytest-basics]]——pytest 本身就是一个 test harness，`@fixture` + 断言 + 报告
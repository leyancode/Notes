# Python 字符串切分：split / rsplit / splitlines / partition / join

切分解决的问题是：**把一行文本拆成有意义的几段**——日志行、配置行 `key=value`、CSV 行、路径、多行文本。Python 给了五个方法，区别只在三个维度：**从哪头开始数、切几刀、拿什么当分隔符**。选错方法本身不报错，只是安静地给你一个形状不对的列表——所以这一族的坑几乎全是静默的。

---

## 思路

`split` 做的事：拿分隔符在字符串上找位置，**在每个位置切一刀**，返回被刀分开的那些段。

关键认知：**返回的段数 = 刀数 + 1**。`"a,b,c"` 里有 2 个逗号，切 2 刀，得 3 段。由此推出两条：分隔符在开头或结尾时，那一侧会得到**空串**（因为刀外面还有一段，只不过是空的）；分隔符一个都没找到时，返回的是**只含原字符串的单元素列表**，不是空列表。

> [!tip]- 核心思路
> **`s.split(sep)` = 在每个 `sep` 处切一刀，返回所有段（含空段）。段数 = 刀数 + 1。**
>
> ```python
> "a,b,c".split(",")     # ['a', 'b', 'c']    2 刀 → 3 段
> ",a,".split(",")       # ['', 'a', '']      2 刀 → 3 段，两头是空串
> "abc".split(",")       # ['abc']            0 刀 → 1 段，没找到也给你一段
> ```
>
> 判断依据：先数分隔符出现了几次，答案的长度就该是那个数加一。对不上就是你想错了。

---

## 1. 导入与创建

全部是 `str` 的自带方法，**不需要任何 import**。只有"多种分隔符"这种复杂情形才需要 `import re`。

> [!success]- 基础语法
> ```python
> s = "2026-07-27 ERROR disk full"
>
> s.split()             # ['2026-07-27', 'ERROR', 'disk', 'full']   按空白
> s.split(" ", 1)       # ['2026-07-27', 'ERROR disk full']         只切第一刀
> s.rsplit(" ", 1)      # ['2026-07-27 ERROR disk', 'full']         只切最后一刀
> s.partition(" ")      # ('2026-07-27', ' ', 'ERROR disk full')    永远 3 元组
> " ".join(["a", "b"])  # 'a b'                                     逆操作
> ```

---

## 2. 核心操作

| 操作 | 作用 | 注意事项 |
| --- | --- | --- |
| `s.split()` | **无参**：按任意连续空白切，自动丢掉空段 | 与 `split(" ")` **行为不同**，见易错点一 |
| `s.split(sep)` | 按 `sep` 切到底 | 空段会**保留** |
| `s.split(sep, n)` | 从左边最多切 `n` 刀 | 结果最多 `n+1` 段 |
| `s.rsplit(sep, n)` | 从**右边**最多切 `n` 刀 | **不带 `n` 时与 `split` 完全一样** |
| `s.splitlines()` | 按行切 | 处理 `\n` / `\r\n`；末尾换行不产生空串 |
| `s.partition(sep)` | 在**第一个** `sep` 处切一刀 | 永远返回 3 元组 `(前, sep, 后)` |
| `s.rpartition(sep)` | 在**最后一个** `sep` 处切一刀 | 同上 |
| `sep.join(parts)` | 逆操作：把若干段拼回去 | 元素必须全是 `str` |

> [!example]- 最小示例
> ```python
> line = "user=alice"
> key, _, value = line.partition("=")
> print(key, value)        # user alice
> ```

---

## 3. 关键行为与边界

`split()` 不带参数是一个**特例**，不是"默认分隔符是空格"那么简单：它把**任意连续空白**（空格、制表符、换行）当作一个分隔符，并且**丢掉首尾空白产生的空段**。带参数的版本则严格按你给的分隔符逐个切，一个空段都不省。

> [!tip]- 为什么这样做
> 两种行为服务两种意图。切"自然语言的词"时，用户敲了几个空格是随意的，你要的是词本身 → 无参版替你把噪音清干净。切"结构化数据"时（CSV、`a::b::c`），**空段是有含义的**——CSV 里 `a,,c` 表示中间那列是空值，如果被吞掉，后面所有列都会错位。所以带参版一个都不能丢。
>
> 一句话：**无参 = 我要词，带参 = 我要字段。**

> [!warning]- 边界条件
> - **空串的两种结果不对称**：`"".split()` 得到 `[]`，但 `"".split(",")` 得到 `['']`（长度 1！）。读文件遇到空行时这个差别会直接决定你要不要判空。
> - **分隔符不能是空串**：`"abc".split("")` 抛 `ValueError: empty separator`。想拆成单字符用 `list("abc")`。
> - **`maxsplit` 数的是刀，不是段**：想拿到 2 段就传 `1`。
> - **`rsplit` 不带 `maxsplit` 毫无意义**：切到底的话从左从右结果一模一样，`"a/b/c".rsplit("/") == "a/b/c".split("/")` 为 `True`。**`rsplit` 永远要配 `maxsplit` 用**，否则你写它只是在骗自己。
> - **`join` 只吃字符串**：`",".join([1, 2])` 抛 `TypeError`。要先转：`",".join(str(x) for x in nums)`。

---

## 4. 实例一：解析 `key=value`（`partition` 的主场）

配置行、HTTP 头、查询参数都是这个形状。难点在于**值里面可能也有分隔符**。

> [!success]- 完整实现
> ```python
> line = "url=http://example.com/?a=1&b=2"
>
> line.split("=")        # ❌ ['url', 'http://example.com/?a', '1&b', '2'] 值被切碎
> line.split("=", 1)     # ✅ ['url', 'http://example.com/?a=1&b=2']
>
> key, sep, value = line.partition("=")   # ✅ 更好
> print(key)             # url
> print(value)           # http://example.com/?a=1&b=2
> ```

`partition` 比 `split(sep, 1)` 好在**返回长度恒定为 3**，可以直接解包成三个变量，不用先判断"到底切出来几段"。没找到分隔符时它也给三个：

```python
"noequals".partition("=")     # ('noequals', '', '')
```

中间那个 `sep` 位正好可以当"到底有没有找到"的判据——空串就是没找到。而 `split("=", 1)` 在没找到时返回 `['noequals']`，你一解包成两个变量就 `ValueError`。

**主流程**：拿到一行 → `partition` → 用中间位判断合法性 → 两边各自 `.strip()`。最容易写错的位置：忘了 `maxsplit` 而用 `split("=")` 去解包，测试数据里恰好没有第二个 `=`，于是**测试全绿、上线炸**。

---

## 5. 实例二：从右边切——取最后一段

场景的共同点是：**前面有多少段不确定，但我只要最后那一块**。

> [!success]- 完整实现
> ```python
> "archive.tar.gz".rsplit(".", 1)      # ['archive.tar', 'gz']   ✅ 扩展名
> "archive.tar.gz".split(".", 1)       # ['archive', 'tar.gz']   ❌ 切错头了
>
> "2026-07-27 ERROR disk full".rsplit(" ", 1)[-1]    # 'full'
>
> # 更省事：rpartition 直接给你最后一段
> "archive.tar.gz".rpartition(".")[-1]               # 'gz'
> ```

⚠️ **但路径不要用 `split`**：`Path(s).name` 才是正解，它顺带处理了结尾斜杠、Windows 分隔符这些边界。实测差异：

```python
from pathlib import Path
Path("/var/log/").name              # 'log'
"/var/log/".rsplit("/", 1)[-1]      # ''   ← 结尾斜杠让它切出空串
```

**判据**：切的是"路径"就用 `pathlib`；切的是"恰好长得像路径的普通字符串"才用 `rsplit`。

---

## 6. 实例三：按行处理（`splitlines` 与 JSONL）

> [!success]- 完整实现
> ```python
> import json
>
> raw = '{"seq": 1}\n{"seq": 2}\n'          # 文件通常以换行结尾
>
> raw.split("\n")        # ❌ ['{"seq": 1}', '{"seq": 2}', '']  末尾多个空串
> raw.splitlines()       # ✅ ['{"seq": 1}', '{"seq": 2}']
>
> rows = [json.loads(line) for line in raw.splitlines()]
> ```

那个空串会让 `json.loads("")` 抛 `JSONDecodeError`——**"文件末尾有没有换行"这种事不该影响你的解析逻辑**，`splitlines` 替你抹平了。它同时认 `\n`、`\r\n`、`\r`，跨平台的文本也不用管。

> [!warning]- `splitlines` 认的换行符比你以为的多
> 它还会在 `\v`（垂直制表符）、`\f`（换页）、`\x1c`–`\x1e`、`\x85`、` ` 等字符处断行。处理**受信任的结构化数据**（如 `json.dumps` 的输出，控制字符已被转义）没问题；处理**用户可控的原始文本**时，如果只想认 `\n`，就得显式 `split("\n")` 再自己过滤空行。

**本项目** —— [`evals/run_eval.py:41`](../../evals/run_eval.py) 和 [`tests/test_tracing.py`](../../tests/test_tracing.py) 读 JSONL 都走 `splitlines()`；[`hpc_agent/report.py:41`](../../hpc_agent/report.py) 用 `"\n".join(lines)` 反向拼 Markdown。**读用 `splitlines`、写用 `join`，是同一组操作的两头。**

---

## 7. 与相近写法怎么选

| 需求 | 推荐写法 | 原因 |
| --- | --- | --- |
| 按空白切成词 | `s.split()` | 自动处理连续空白和首尾 |
| 固定分隔符、要全部字段 | `s.split(sep)` | 空字段会保留，不会错位 |
| `key=value`，值里可能有分隔符 | `s.partition(sep)` | 定长 3 元组，可直接解包 |
| 只要最后一段 | `s.rpartition(sep)[-1]` | 比 `rsplit(sep, 1)[-1]` 更直白 |
| **路径**的最后一段 / 目录 / 扩展名 | `Path(s).name` / `.parent` / `.suffix` | 处理了所有路径边界 |
| 多行文本按行 | `s.splitlines()` | 抹平末尾换行与平台差异 |
| 多种分隔符 | `re.split(r"[,;]\s*", s)` | 单一分隔符做不到 |
| **CSV** | `csv` 模块 | 引号、转义、字段内换行，`split(",")` 全处理不了 |
| 拆成单个字符 | `list(s)` | `split("")` 是 ValueError |
| 拼回去 | `sep.join(parts)` | 比循环加法快且清晰 |

---

## 8. 常见易错点

> [!warning]- 易错点
> - **`split()` 和 `split(" ")` 不是一回事**（最高频）：
>   ```python
>   "a  b".split()        # ['a', 'b']         连续空格算一个分隔符
>   "a  b".split(" ")     # ['a', '', 'b']     两个空格 = 2 刀 = 3 段
>   " a ".split()         # ['a']
>   " a ".split(" ")      # ['', 'a', '']
>   ```
>   **想按词切就别传参数。** 传了 `" "` 等于宣布"每一个空格都有结构意义"。
> - **空串的不对称**：`"".split()` → `[]`，`"".split(",")` → `['']`。逐行处理时空行会给你一个含空串的列表，`for` 循环照跑不误，然后在下游炸。
> - **`rsplit` 不带 `maxsplit` 等于没写**：`"a/b/c".rsplit("/")` 和 `split("/")` 结果相同。见过有人以为 `rsplit` 会"反着排"——不会，顺序永远是原文顺序。
> - **`maxsplit` 数刀不数段**：想要 2 段传 `1`。传 `0` 是**一刀不切**（`"a b".split(" ", 0)` → `['a b']`），传负数才是不限制。
> - **解包前不确认长度**（实测报错原文）：
>   ```python
>   k, v = "a=1=2".split("=")      # ValueError: too many values to unpack (expected 2)
>   k, v = "noequals".split("=")   # ValueError: not enough values to unpack (expected 2, got 1)
>   ```
>   **要么 `maxsplit=1`，要么用 `partition`。** 这类崩溃全都发生在"测试数据恰好规整、真实数据不规整"的时候。
> - **`join` 的调用方向反了**：是 `"分隔符".join(列表)`，不是 `列表.join("分隔符")`（后者 `AttributeError`）。记法：**分隔符要出现在每两段之间，所以由它主导**。
> - **`join` 遇到非字符串**：`",".join([1, 2])` → `TypeError: sequence item 0: expected str instance, int found`。
> - **用 `split(",")` 解析 CSV**：字段里带引号包裹的逗号（`"上海, 中国"`）会被切开。**用 `csv` 模块。**
> - **末尾换行制造幽灵空行**：`"a\nb\n".split("\n")` → `['a', 'b', '']`。用 `splitlines()`。

---

## 复杂度

> [!summary]- 复杂度
> - `split` / `rsplit` / `splitlines` / `partition`：时间 $O(n)$，扫一遍字符串；空间 $O(n)$，所有段加起来就是原串长度。
> - `join`：$O(n)$，$n$ 为总长度——它会先算总长再一次性分配内存。
> - **反例**：循环里 `result += part + sep` 是 $O(n^2)$（字符串不可变，每次都复制一份新的）。**拼字符串一律用 `join`。**

---

## 一句话总结

`split` 在每个分隔符处切一刀、返回"刀数 + 1"段；**无参版为切词而生（吞掉空白噪音），带参版为切字段而生（一个空段都不丢）**；只关心第一刀或最后一刀时用 `partition` / `rpartition` 拿定长三元组，按行用 `splitlines`，路径交给 `pathlib`，拼回去永远用 `join`。

---

## 模板归纳

看到以下信号时，对应到这些写法：

- "按空格拆成词" → `s.split()`，**不传参数**
- "`key=value`，值里可能有等号" → `s.partition("=")`
- "只要扩展名 / 最后一段" → `s.rpartition(sep)[-1]`；是路径就换 `Path(s).name`
- "逐行读文件 / JSONL" → `s.splitlines()`，别用 `split("\n")`
- "把列表拼成一行" → `sep.join(...)`，元素先转 `str`

```python
# 定长解包，不用判长度
key, sep, value = line.partition("=")
if not sep:
    raise ValueError(f"缺少 '=': {line!r}")

# 只切最后一刀
head, _, tail = path_like.rpartition("/")

# 逐行 → 结构化 → 拼回去
rows = [json.loads(l) for l in raw.splitlines()]
text = "\n".join(render(r) for r in rows)
```

关联笔记：[python-string.md](0.%20python-string.md)（字符串处理总入口：`strip`/大小写/查找/替换/f-string)
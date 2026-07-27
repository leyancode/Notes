# Python 字符串数字判定：isdigit / isdecimal / isnumeric 与 is_integer

用于判断"一个字符串/浮点是不是我想要的那种数"。算法题里凡是**解析数字输入、校验 token、字符串转整数（atoi）、判断有效数字**时都会用到——核心是它们各自的"认字范围"不同，且**通过判定 ≠ `int()` 一定转得动**。

来源：task_010 `intent_from_tool_call` 的 `unknowns` 兜底（2026-07-21）。

---

## 思路

模型/用户给的 `unknowns` 可能是 `1000000`、`"1000000"`、`1000000.0`、`"100万"`、`True`……要在**转 int 之前**先判断"这货能不能安全转"。不判断直接 `int(x)`：遇到 `"abc"`/`"100万"` 直接抛 `ValueError`，遇到 `True` 会默默变成 `1`（bool 是 int 子类）——**默默算错比报错更危险**。

这些 `is...()` 方法就是"转换前的守门员"。但守门员有强弱之分，选错会漏人。

> [!tip]- 核心思路
> 三个字符串数字方法按"认字范围"层层包含：
>
> ```python
> "7".isdecimal()   # 最严：纯十进制数字
> "²".isdigit()     # 中：+ 上标下标（isdecimal ⊆ isdigit）
> "½".isnumeric()   # 最宽：+ 一切表数字义的字符（isdigit ⊆ isnumeric）
> (7.0).is_integer() # float 专用：小数部分是不是 0
> ```
>
> 不变量：`isdecimal ⊆ isdigit ⊆ isnumeric`；而 `int()` 能转的集合和 `isdigit` **两个方向都不重合**。

---

## 1. 导入与创建

**全部无需导入**——它们是 `str` / `float` 类型自带的方法，不是某个库的函数。调用形式永远是 `值.方法()`。

> [!success]- 基础语法
> ```python
> # 无 import
> "7".isdigit()        # str 的方法
> (7.0).is_integer()   # float 的方法
> # (7.0).isdigit()    # ❌ AttributeError：float 没有 isdigit
> ```

---

## 2. 核心操作

| 操作 | 作用 | 注意事项 |
| --- | --- | --- |
| `s.isdecimal()` | 是否全为十进制数字字符 | 最严；`O(len(s))` 扫全串 |
| `s.isdigit()` | isdecimal + 上标下标(`²³`) | `"²".isdigit()` 为 True 但 `int("²")` 抛错 |
| `s.isnumeric()` | isdigit + 分数/中文/罗马数字等 | `"一".isnumeric()`、`"½".isnumeric()` 均 True |
| `f.is_integer()` | float 小数部分是否为 0 | `O(1)`；`int` 在 3.12+ 也有 |
| `s.strip()` | 去首尾空白 | `int()` 会自动 strip，`isdigit()` 不会 |

> [!example]- 最小示例
> ```python
> for s in ["7", "²", "一", "½", "-5", "  7"]:
>     print(repr(s), s.isdigit(), s.isnumeric())
> # '7'   True  True
> # '²'   True  True
> # '一'  False True
> # '½'   False True
> # '-5'  False False   ← 负号不算数字字符
> # '  7' False False   ← 空格不算
> ```

---

## 3. 关键行为与边界

最容易被误解的是"`isdigit()` 为 True 就等于 `int()` 转得动"——**错**，两个方向都能反例。

> [!tip]- 为什么这样做
> `isdigit()` 判的是"每个字符是否为数字字符"，是**逐字符**语义；`int()` 判的是"整串能否解析为整数"，还额外接受首尾空白和正负号。两者规则不同，所以：
> - `"²".isdigit()` True，但 `int("²")` → `ValueError`（上标不是可解析整数）
> - `"-5".isdigit()` False，但 `int("-5")` → `-5`（负号 isdigit 不认，int 认）

> [!warning]- 边界条件
> - **空字符串**：`"".isdigit()` / `isdecimal` / `isnumeric` **全为 False**（对所有 `is...()` 都是 False），判空要单独处理。
> - **bool 陷阱**：`isinstance(True, int)` 为 True，做数值兜底时必须**先挡 bool**，否则 `True→1`、`False→0` 默默算错。
> - **中文/全角数字**：`"一".isnumeric()` True 但 `int("一")` 抛错——想支持要另写解析（本项目规则 router 的 `_cn_to_int`）。
> - **非 ASCII 数字**：`"๗"`(泰文7) `isdigit()` True 且 `int("๗")` 居然等于 `7`——算法题里判 ASCII 数字别用 `isdigit()`。

---

## 4. 实例一：安全地"字符串/数值 → 正整数或 None"

输入是来源不可信的值（LLM tool call 的参数），目标是转成正整数，转不了就返回 `None`（宁可追问，不默默算错）。选 `isdigit()` 而非直接 `int()`，是因为要**顺带挡掉负数和空格**（规模必为正整数）。

> [!success]- 完整实现
> ```python
> def to_positive_int(value):
>     if isinstance(value, bool):          # 先挡 bool，否则 True→1
>         return None
>     if isinstance(value, int):
>         return value
>     if isinstance(value, float):
>         return int(value) if value.is_integer() else None   # 7.0 救，7.5 不救
>     if isinstance(value, str):
>         s = value.strip()
>         return int(s) if s.isdigit() else None               # "7" 救，"-5"/"² "/"abc" 不救
>     return None
>
>
> print(to_positive_int("1000000"))  # 1000000
> print(to_positive_int(1000000.0))  # 1000000
> print(to_positive_int(True))       # None
> print(to_positive_int("100万"))    # None
> ```
最容易写错的位置：把 `isinstance(value, bool)` 放到 `isinstance(value, int)` 后面——那样 `True` 会先命中 int 分支，前功尽弃。**bool 判断必须在 int 之前**。

---

## 5. 实例二：算法题——实现 `myAtoi`（LeetCode 8 简化版）

把前导空格后的连续数字解析成整数。这里刻意**不用 `s.isdigit()`** 判字符，而用 `'0' <= c <= '9'`——因为 `"²".isdigit()` 是 True，会污染结果；算法题里判 ASCII 数字，区间比较才是稳的。

> [!success]- 完整实现
> ```python
> def my_atoi(s: str) -> int:
>     i, n = 0, len(s)
>     while i < n and s[i] == " ":          # 跳过前导空格
>         i += 1
>     sign = 1
>     if i < n and s[i] in "+-":            # 可选符号
>         sign = -1 if s[i] == "-" else 1
>         i += 1
>     num = 0
>     while i < n and "0" <= s[i] <= "9":   # 用区间而非 isdigit()！
>         num = num * 10 + (ord(s[i]) - ord("0"))
>         i += 1
>     return sign * num
>
>
> print(my_atoi("   -42abc"))  # -42
> print(my_atoi("4²2"))        # 4  ← 若用 s[i].isdigit() 会误吞 ²
> ```

> [!summary]- 当前实例复杂度
> - 时间复杂度：$O(n)$，一次线性扫描字符串。
> - 空间复杂度：$O(1)$，只用了几个指针/累加变量，返回值不算额外空间。

---

## 6. 与相近写法怎么选

| 需求 | 推荐写法 | 原因 |
| --- | --- | --- |
| 判"这串能否转整数"（含负号/空格） | `try: int(s) except ValueError` | int() 的规则就是解析规则，最贴合 |
| 判"每个字符都是十进制数字" | `s.isdecimal()` | 最严，排除上标/分数/中文 |
| 算法题判单个 **ASCII** 数字字符 | `'0' <= c <= '9'` | 避开 `isdigit()` 的 `²`/他国数字坑 |
| 判 float 有没有小数部分 | `f.is_integer()` | 专用且 $O(1)$ |
| 只想要正整数、顺带排除负数空格 | `s.strip().isdigit()` | 比 int() 更严，符合"正整数"约束 |

只保留会影响选择的差异：`isnumeric()` 几乎只在明确要支持分数/罗马数字时才用，日常别选它当"是不是数字"的判据。

---

## 7. 常见易错点

> [!warning]- 易错点
> - **bool 当 int 漏判**：`isinstance(True, int)` 为 True。`int` 分支必须放在 `bool` 分支**之后**，否则 `True→1` 默默算错。
> - **`isdigit()` 当成 `int()` 的等价校验**：`"²"`/`"๗"` 能过 isdigit 但 `int("²")` 抛错；`"-5"`/`"  7"` 过不了 isdigit 但 int 转得动。两者不等价。
> - **忘了空串**：`"".isdigit()` 是 False，`int("")` 抛错——空输入要显式处理。
> - **`is_integer()` 用在 int 上（旧版本）**：Python 3.12 前 `int` 没有 `is_integer()`，`(7).is_integer()` 会 AttributeError；跨版本代码用 `isinstance(x, int)` 更稳。

---

## 复杂度

> [!summary]- 复杂度
> - `s.isdigit()` / `isdecimal()` / `isnumeric()`：$O(len(s))$，逐字符扫描。
> - `int(s)`：$O(len(s))$，同样要读完整串。
> - `f.is_integer()`：$O(1)$，只看浮点的小数部分。
> - 额外空间：以上均 $O(1)$，不分配与输入等长的结构。

---

## 一句话总结

字符串数字判定三姐妹按范围 `isdecimal ⊆ isdigit ⊆ isnumeric`、float 用 `is_integer()`；**触发信号是"转 int 前要不要先校验"**，最稳写法是「先挡 bool → int 直取 → float 靠 `is_integer()` → str 靠 `strip().isdigit()`（或直接 `try int`）」。

---

## 模板归纳

看到以下信号时，优先想到本知识点：
- 要把不可信的字符串/数值**转成整数**，且转不了要优雅兜底而非崩溃
- 判断"这个字符/字符串是不是数字"（atoi、有效数字、token 校验）
- 输入可能混入 bool、浮点、全角/中文数字等**伪装成数字**的东西

```python
# 可直接复用的最小模板：安全转正整数或 None
def to_positive_int(value):
    if isinstance(value, bool):
        return None
    if isinstance(value, int):
        return value
    if isinstance(value, float):
        return int(value) if value.is_integer() else None
    if isinstance(value, str):
        s = value.strip()
        return int(s) if s.isdigit() else None
    return None
```

相关笔记：[[python-list-building]]（同为"Python 数据处理实战"系列）。
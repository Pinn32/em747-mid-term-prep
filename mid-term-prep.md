---
start: 2026-03-18
tags: 
    - r
    - dv
---

# 🔗 Quicklinks

> [dply函数](#函数速览)
> [regex](#regex)
> [常用图表](#🔹%20常用%20Geom%20图表)
> [常用颜色](#🔹%20常用颜色)


# 🔴 提前准备

| 步骤 | 操作 |
|------|------|
| 安装 LaTeX | `tinytex::install_tinytex()` （否则无法渲染 PDF） |
| 填写作者信息 | `author: "..."` 中的 `___` 改成自己信息 |
| setup 块 | 保持原样，确保 `include=FALSE`，`echo=TRUE` |
| 答题代码块 | 把 `eval=FALSE` 改成 `eval=TRUE`，`??` 改成答案 |
| 查函数用法 | RStudio Help 面板搜索函数名 |



---

# 🟢 所有 Package

| 分类 | 包名 | 主要功能 |
|------|------|---------|
| 基础 | `tidyverse` | 核心包集合 (readr, dplyr, stringr, tidyr, tibble, ggplot2, lubridate) |
| 基础 | `readxl` | `read_excel(path, sheet=2, range="B1:D10")` |
| 文本分析 | `tidytext` | `unnest_tokens()` `cast_dtm()` `get_sentiments()` |
| 文本分析 | `janeaustenr` | `austen_books()` 示例数据集 |
| 可视化 | `patchwork` | 图表并排: `p1 + p2` (并列) / `p1 / p2` (上下) |
| 可视化 | `ggthemes` | `theme_economist()` `theme_wsj()` `theme_tufte()` |
| 配色 | `RColorBrewer` | `display.brewer.all()` `scale_fill_brewer(palette="Set2")` |
| 配色 | `wesanderson` | `wes_palette("GrandBudapest1", n=3)` |
| 配色 | `NatParksPalettes` | `natparks.pals("Yellowstone", n=3)` |
| 词云 | `wordcloud2` | `wordcloud2(data, size, color, shape, backgroundColor)` |
| 词云 | `htmlwidgets` | `saveWidget(cloud, "wordcloud.html", selfcontained=FALSE)` |
| 词云 | `webshot` | `install_phantomjs()` / `webshot("x.html", "x.png", delay=5)` |
| 统计建模 | `broom` | `tidy(model, conf.int=TRUE)` `glance(model)` |
| 统计建模 | `ggeffects` | `ggpredict(model, terms="x")` |
| 统计建模 | `performance` | `check_model(model)` |

---

# 🟢 L1-R Basics

## 🔹 基础数据类型转换

| 类型 | 示例代码 | 输出 |
|------|----------|------|
| 字符 | `as.character(123)` | `"123"` |
| 整数 | `as.integer("123")` | `123` |
| 小数 | `as.numeric("3.5")` | `3.5` |
| 布尔值 | `as.logical(0)` | `FALSE` |
|  | `as.logical(1)` | `TRUE` |
|  | `as.logical(123)` | `TRUE` |
|  | `as.logical(3.5)` | `TRUE` |
|  | `as.logical("123")` | <code><span class="red">NA</span></code> |

> 任何非 0 数值 👉🏻 TRUE
> 只有数值可以转 logical  
> 字符转 logical 👉🏻 NA

## 🔹 Factor

| 项目 | 说明 |
|------|------|
| 示例代码 | `my_vector <- c(1,2,1,1,2)`<br>`my_factor <- factor(my_vector, levels = c(2,1))` |
| 输出 | `1 2 1 1 2`<br>`Levels: 2 1` |

> 默认情况下，levels 按出现顺序（1 → 2）；  
> 手动指定 `levels = c(2,1)` 可控制因子水平顺序。

## 🔹 Date

### 系统日期 & 时间

| 函数 | 作用 | 示例输出 |
|------|------|---------|
| `Sys.Date()` | 当前系统日期 | `"2026-03-20"` |
| `Sys.time()` | 当前系统时间 | `"2026-03-20 11:27:07 EDT"` |

### 转换文本为 Date（`as.Date()`）

| 情况 | 代码 |
|------|------|
| 标准格式（无需 format） | `as.Date("2026-03-20")` |
| 其他格式（须指定 format） | `as.Date("2026/03/20", format="%Y/%m/%d")` |

> `%Y`年 `%m`月 `%d`日 `%H`时 `%M`分 `%S`秒
> `class()` → `"Date"`，`typeof()` → `"double"`

### Lubridate

| 类别 | 函数 |
|------|------|
| 文本转日期 | `ymd()` `mdy()` `dmy()` `ymd_hms()` `mdy_hms()` |
| 提取部件 | `year()` `month()` `day()` `hour()` `minute()` `second()` |
| 当前时间 | `today()` `now()` |

> 函数名即格式：`mdy("03-20-2026")` → `"2026-03-20"`（输入须与函数名顺序匹配）
> `year()` `month()` `day()` 返回**数字**，不再是 Date 对象

## 🔹 DataFrame

| 函数 | 作用 |
|------|------|
| `head(df, n=6)` | 前 n 行 |
| `tail(df, n=6)` | 后 n 行 |
| `dim(df)` | 行数 & 列数 |
| `nrow(df)` / `ncol(df)` | 行数 / 列数 |
| `str(df)` / `glimpse(df)` | 结构（`glimpse` 是 dplyr 函数） |
| `summary(df)` | 数字列: mean/min/Q1/median/Q3/max；非数字: length/class/mode |

## 🔹 Function

| 概念 | 语法 | 说明 |
|------|------|------|
| 定义 | `f <- function(a, b) { ... }` | 用 `<-` 赋值 |
| 调用 | `f(1, 2)` 或 `f(a=1, b=2)` | 可按位置或名称传参 |
| 默认参数 | `function(a, b = 10)` | 调用时可省略有默认值的参数 |
| 返回值 | `return(x)` 或直接写 `x`（最后一行） | 不写 `return()` 默认返回最后表达式 |
| 匿名函数 | `\(x) x * 2` | 等价于 `function(x) x * 2`，常用于 map |

```r
# 默认参数
greet <- function(name, greeting = "Hello") {
  paste(greeting, name)
}
greet("Alice")          # "Hello Alice"
greet("Alice", "Hi")   # "Hi Alice"

# 匿名函数（配合 sapply / map）
sapply(1:3, \(x) x^2)  # 1 4 9
```

> 函数内部变量是==局部作用域==，不影响外部环境；外部变量函数内可读但不可改（除非用 `<<-`）



---

# 🟢 L2-Data Wrangling

## 🔹 dplyr

```r
data <- read_csv("URL或文件夹地址")  # readr 包，需 tidyverse
```

### 函数速览

| 函数 | 作用 | 示例 |
|------|------|------|
| `glimpse(df)` | 查看结构（≈ `str()`） | `glimpse(data)` |
| `filter(条件)` | 保留满足条件的行 | `filter(age > 20 & score < 80)` |
| `select(列)` | 选列 | `select(col1, col1:col3, -col3)` |
| `mutate(新列=表达式)` | 添加/修改列 | `mutate(perc = col1/col2*100)` |
| `group_by(列)` | 分组（配合 summarize/mutate） | `group_by(age)` |
| `summarize(...)` | 聚合汇总，压缩行数 | `summarize(avg=mean(score,na.rm=T), .groups="drop")` |
| `arrange(列)` | 排序（默认升序） | `arrange(age, desc(score))` |
| `rename(新=旧)` | 重命名列（不新建列） | `rename(new_col = old_col)` |

> `group_by` + `summarize`：必须加 `.groups="drop"`；只保留汇总列，行数压缩
> `group_by` + `mutate`：保留所有列且行数不变，之后须 `ungroup()`
> `mean(condition)` → 满足条件的**比例**；`sum(condition)` → 满足条件的**个数**

### filter() 逻辑运算符

| Operator | 说明 |
|----------|------|
| `!` `&` <code>\|</code> | not / and / or |
| `==` `!=` | 向量相等 / 不等 |
| `>` `>=` `<` `<=` | 大于 / 大于等于 / 小于 / 小于等于 |
| `%in%` | 左元素是否存在于右向量 (非向量比较) |

> `%in%` 判断元素存在性；`==` 按位置匹配（长度不等会循环）

### select() 辅助函数

| 写法 | 效果 |
|------|------|
| <code>select(col1<span class="red">:</span>col3)</code> | 选 col1 到 col3 |
| <code>select(<span class="red">-</span>col3)</code> | 排除 col3 |
| <code>select(<span class="red">contains</span>()"x"))</code> | 列名含 "x" |
| <code>select(<span class="red">starts_with</span>("c"))</code> | 列名以 "c" 开头 |
| <code>select(<span class="red">ends_with</span>("3"))</code> | 列名以 "3" 结尾 |
| <code>select(<span class="red">everything</span>())</code> | 全选 |

### summarize() 聚合函数

| 类别 | 函数 | 说明 |
|------|------|------|
| 统计 | `mean()` `sd()` `var()` `min()` `max()` `median()` `sum()` | 均需 `na.rm=TRUE` |
| 计数 | `n()` | 计行数（无 na.rm） |
| 去重计数 | `n_distinct(col)` | 计列中类别数 |

### pivot_longer / pivot_wider

```r
# 这两个是 tidyr 的函数

# 变长（宽→长）
data |> pivot_longer(
  cols = c(male_score, female_score),
  names_to = "gender",
  values_to = "score",
  names_pattern = "(.*)_.*"  # 只保留括号内 → male / female
)

# 变宽（长→宽），多值列
data |> pivot_wider(
  names_from = gender,
  values_from = c(score, rank),
  names_glue = "{gender}_{.value}"  # {gender}=类名, {.value}=值列名
)

# 变宽，多分组列
data |> pivot_wider(
  names_from = c(gender, level),
  values_from = score,
  names_sep = "_"   # → male_low, female_low, male_medium ...
)
```

## 🔹 Joining

| 函数 | 作用 |
|------|------|
| `left_join()` | 保留左表所有行，右表匹配补全 |
| `right_join()` | 保留右表所有行 |
| `inner_join()` | 只保留两表都有的行 |
| `full_join()` | 保留所有行 |
| `anti_join()` | 保留左表中==不匹配==右表的行 |

```r
data |> left_join(all_words, by = c("data_word" = "word"))
data |> anti_join(stop_words, by = "word")
```

> dplyr 函数
> join 后缺失值自动补 `NA`；列名不同时用 `by = c("左列名" = "右列名")` 映射

## 🔹 Stringr 文本操作

### Regex

| Pattern | Meaning | Example | Matches |
|:---------:|---------|---------|---------|
| `.` | 任意单个字符 | `"a.c"` | "abc", "a1c", "a c" |
| `^` | 字符串开头 | `"^The"` | "The dog" 但不匹配 "See The dog" |
| `$` | 字符串结尾 | `"end$"` | "the end" 但不匹配 "endless" |
| `*` | 前一个字符出现零次或多次 | `"ab*c"` | "ac", "abc", "abbc" |
| `+` | 前一个字符出现一次或多次 | `"ab+c"` | "abc", "abbc" 但不匹配 "ac" |
| `?` | 前一个字符出现零次或一次 | `"colou?r"` | "color", "colour" |
| `\\` | 转义特殊字符 | `"\\."` | 字面意义的句点 "." |

| Pattern | Meaning | Example |
|---------|---------|---------|
| `[abc]` | 匹配 a、b 或 c | `"[aeiou]"` 匹配元音 |
| `[a-z]` | 匹配任意小写字母 | |
| `[A-Z]` | 匹配任意大写字母 | |
| `[0-9]` | 匹配任意数字 | 等同于 `\\d` |
| `[^abc]` | 匹配除 a、b、c 以外的任意字符 | `[^0-9]` 匹配非数字 |

```r
[:punct:]⁠  标点符号
# ⁠! " # $ % & ' ( ) * + , - . / : ; < = > ? @ [ \ ] ^ _ ` { | } ~⁠
```

| Shorthand | Meaning | Equivalent |
|-----------|---------|------------|
| `\\d` | 任意数字 | `[0-9]` |
| `\\D` | 任意非数字 | `[^0-9]` |
| `\\w` | 任意单词字符 | `[a-zA-Z0-9_]` |
| `\\W` | 任意非单词字符 | `[^a-zA-Z0-9_]` |
| `\\s` | 任意空白字符 | 空格、制表符、换行 |
| `\\S` | 任意非空白字符 | |

### ==常用 regex==

| Task | Pattern | Example |
|------|---------|---------|
| Email | `[\\w.-]+@[\\w.-]+\\.\\w+` | `user@example.com` |
| Phone (US) | `\\d{3}[-.\\s]?\\d{3}[-.\\s]?\\d{4}` | 555-123-4567 |
| URL | `https?://[\\w./]+` | `https://example.com` |
| Twitter handle | `@\\w+` | @username |
| Hashtag | `#\\w+` | `#DataScience` |
| Date (YYYY-MM-DD) | `\\d{4}-\\d{2}-\\d{2}` | 2024-01-15 |
| Time (HH:MM) | `\\d{2}:\\d{2}` | 14:30 |

### stringr 函数

| Function | Purpose | Example |
|----------|---------|---------|
| `str_detect()` | 是否存在匹配（返回 TRUE/FALSE） | `str_detect(x, "pattern")` |
| `str_subset()` | 返回匹配的元素 | `str_subset(x, "pattern")` |
| `str_extract()` | 提取第一个匹配 | `str_extract(x, "pattern")` |
| `str_extract_all()` | 提取所有匹配 | `str_extract_all(x, "pattern")` |
| `str_replace()` | 替换第一个匹配 | `str_replace(x, "pattern", "replacement")` |
| `str_replace_all()` | 替换所有匹配 | `str_replace_all(x, "pattern", "replacement")` |
| `str_match()` | 提取第一个匹配的分组 | `str_match(x, "pattern")` |
| `str_count()` | 统计匹配次数 | `str_count(x, "pattern")` |
| `str_split()` | 按模式拆分字符串 | `str_split(x, "pattern")` |

---

# 🟢 L3-Ggplot2

## 🔹 Ggplot 基础语法

**注：用 <code><span class='red'>+</span></code> 连接，不用 <code><span class='red'>|></span></code>**

```r
ggplot(df, aes(x = , y = )) +
	geom_foo() +
	labs() +
	theme()
```

## 🔹 常用 Geom 图表

| Geom Function | Description | Key Aesthetics |
|---------------|-------------|----------------|
| `geom_point()` | Scatter plot | `x`, `y`, `color`, `size`, `shape` |
| `geom_line()` | Line plot | `x`, `y`, `color`, `linetype` |
| `geom_bar()` | Bar chart (counts) | `x`, `fill`, `position` ('dodge', 'stack', 'fill', 'identity') |
| `geom_col()` | Bar chart (values) | `x`, `y`, `fill`, `position` |
| `geom_histogram()` | Histogram | `x`, `fill`, `binwidth`, `bins` (bin数量) |
| `geom_boxplot()` | Box plot | `x`, `y`, `fill` |
| `geom_smooth()` | Trend line | `x`, `y`, `method="lm"`, `se=TRUE`, `color` |
| `geom_density()` | Density curve | `x`, `fill`, `color`, `alpha` |
| `geom_text()` | Add text | `x`, `y`, `label` |
| `geom_tile()` | Heatmap | `x`, `y`, `fill` |
| `geom_errorbar()` | Error Bar | `aes(ymin= mean+sd, ymax= mean-sd)`, `width=2` |

> 注：line 要和 point 一起用, `geom_line() + geom_point()`

| Geom Function | Description | Key Aesthetics |
|---------------|-------------|----------------|
| `geom_area()` | Area under a line | `x`, `y`, `fill`, `alpha` |
| `geom_violin()` | Violin plot | `x`, `y`, `fill` |
| `geom_jitter()` | Jittered points | `x`, `y`, `color`, `width`, `height` |
| `geom_tile()` | Heatmap tiles | `x`, `y`, `fill` |
| `geom_segment()` | Line segments | `x`, `xend`, `y`, `yend` |
| `geom_abline()` | Reference line | `slope`, `intercept` |
| `geom_hline()` | Horizontal reference line | `yintercept` |
| `geom_vline()` | Vertical reference line | `xintercept` |

## 🔹 可映射参数

| Aesthetic | Description | Used With |
|-----------|-------------|-----------|
| `color` | 点线颜色 | 散点图，折线图，文字 |
| `fill` | 内部颜色 | 柱状图，箱线图，面积图 |
| `alpha` | 不透明度 (0-1) | 所有图 |
| `size` | 点大小/线宽 | 散点图，折线图，气泡图 |
| `shape` | 点形状 | 散点图 |
| `linetype` | 线型 | 折线图，面积图 |
| `stroke` | 描边粗细 | 散点图（shape 21–25 空心点） |

> 映射参数的时候可以用 `factor(num_var)` 来给 num_var 分组

| ID | Viz | Shape Name |
|:---:|:---:|-----------|
| 0 | □ | square open |
| 1 | ○ | circle open |
| 2 | △ | triangle open |
| 15 | ■ | square filled |
| 16 | ● | circle filled |
| 17 | ▲ | triangle filled |
| 18 | ◆ | diamond filled |

## 🔹 常用图表 Snippets

### 旋转x标签 45deg

```r
# rotate xlab 45deg
theme(
  axis.text.x = element_text(
    angle = 45,  # rotate xlab 45deg anticlockwise
    hjust = 1    # horizontally adjust xlab
  )
)
```

### 调色盘

```r
# color brewer
scale_fill_brewer(palette = "Set2")
```

### 移除图例

```r
# remove legend
theme(legend.position = "none")
```

### 转置坐标

```r
# flip coordinates
coord_flip()  # horiz plot
```

### 自定义颜色

```r
# manual color scale
scale_color_manual(
	values = c("FALSE" = "steelblue", "TRUE" = "red"),
	labels = c("Normal", "Outlier")
)
```

### 添加统计量 ink

```r
# add statistic ink
stat_summary(
	fun = mean, geom = "point", 
	shape = 18, size = 3, color = "red"
)
```



## 🔹 常用颜色

| 颜色名称 | 预览 | 颜色名称 | 预览 |
|---------|:----:|---------|:----:|
| pink | <span style="color:pink">████</span> | beige | <span style="color:beige">████</span> |
| lightpink | <span style="color:lightpink">████</span> | darkseagreen | <span style="color:darkseagreen">████</span> |
| lightcoral | <span style="color:lightcoral">████</span> | mediumseagreen | <span style="color:mediumseagreen">████</span> |
| palevioletred | <span style="color:palevioletred">████</span> | forestgreen | <span style="color:forestgreen">████</span> |
| rosybrown | <span style="color:rosybrown">████</span> | green | <span style="color:green">████</span> |
| indianred | <span style="color:indianred">████</span> | seagreen | <span style="color:seagreen">████</span> |
| firebrick | <span style="color:firebrick">████</span> | teal | <span style="color:teal">████</span> |
| tomato | <span style="color:tomato">████</span> | lightseagreen | <span style="color:lightseagreen">████</span> |
| coral | <span style="color:coral">████</span> | mediumaquamarine | <span style="color:mediumaquamarine">████</span> |
| salmon | <span style="color:salmon">████</span> | powderblue | <span style="color:powderblue">████</span> |
| lightsalmon | <span style="color:lightsalmon">████</span> | lightblue | <span style="color:lightblue">████</span> |
| sandybrown | <span style="color:sandybrown">████</span> | skyblue | <span style="color:skyblue">████</span> |
| orange | <span style="color:orange">████</span> | lightskyblue | <span style="color:lightskyblue">████</span> |
| peru | <span style="color:peru">████</span> | lightsteelblue | <span style="color:lightsteelblue">████</span> |
| tan | <span style="color:tan">████</span> | steelblue | <span style="color:steelblue">████</span> |
| wheat | <span style="color:wheat">████</span> | mediumpurple | <span style="color:mediumpurple">████</span> |
| bisque | <span style="color:bisque">████</span> | plum | <span style="color:plum">████</span> |
| linen | <span style="color:linen">████</span> | thistle | <span style="color:thistle">████</span> |


---

# 🟢 L4-TidyText

## 🔹 获取按书/章节分类的文本

```r
library(janeaustenr)
# austen_books() 里包含了简奥斯汀的所有书籍的所有行,一共只有两列, book, text

original_books <- austen_books() |> 
	group_by(book) |> 
	mutate(
		linenumber = row_number(),
		chapter = cumsum(
			str_detect(
				text, 
				regex("^chapter [\\divxlc]", ignore_case = TRUE))
			)
	) |> 
	ungroup()
```

> `regex(^chapter [\\divxlc], ignore_case = TRUE)` 是章节名, 其中 ivxlc 是罗马数字
> `str_detect()` 看是否有出现过匹配值, 返回 TRUE / FALSE
> `cumsum()` 用来累积 TRUE, 即数章节标题出现了累积几次, 即当前所在章节


## 🔹 Tokenize

```r
# unnest_tokens() 语法
unnest_tokens(tbl, output, input, token = "words", ...)
```

```r
# tokenize
data |> unnest_tokens(word, text)  
	# input = text列 (段落)
	# output = word列（单个词，新生成的列）
	# 默认 token = "words"
```

```r
# 查看词频最高的前10个词
data |> 
	unnest_tokens(word, text) |>
	count(word, sort = TRUE) |>
	head(10)
```

## 🔹 移除停用词 (stopwords)

```r
# 停用词
tidytext::stop_words
```

```r
# 移除停用词
data_clean <- data |> 
  anti_join(stop_words, by = "word")
```

## 🔹 关键词上下文查找 (KWIC: Keyword in Context)

查找所有 text 中包含特定 keyword，且 keyword 前后有特定 context pattern 的行

| 参数 | 说明 |
|------|------|
| `data` | 使用的数据 |
| `text_col` | 使用的 text 列名 |
| `keyword` | 关键词 |
| `window` | 关键词前后字符数（包括空格） |

> 列名参数要用 `{{ 列名 }}` 来表示 (tidy eval)，使函数内可接受裸列名

```r
# 定义KWIC函数
kwic <- function(data, text_col, keyword, window = 10) {
    pattern <- paste0(".{0,", window, "}", keyword, ".{0,", window, "}")
    data |>
        filter(str_detect({{ text_col }}, keyword)) |>
	    mutate(context = str_extract({{ text_col }}, pattern)) |>
	    select(context)
}
```

## 🔹 词频矩阵 (DTM: Document Term Matrix)

DTM 是文本的矩阵化表示：**行** = 文档，**列** = 词，**值** = 词频

|        | love | family | money | marriage | happy |
|--------|------|--------|-------|----------|-------|
| Book 1 | 45   | 23     | 12    | 67       | 34    |
| Book 2 | 32   | 45     | 56    | 23       | 12    |
| Book 3 | 67   | 12     | 8     | 89       | 45    |

> TDM（词-文档矩阵）是 DTM 的转置，行列互换；`tidytext` 中一般生成 DTM

==**词频分析工作流**==
原始文本 → 分成单个词 (tokenize) → 移除停用词 (remove stopwords) 
→ 计数 (count) → 生成词频矩阵 (DTM)

```r
# DTM 工作流
tokenized <- original_books |> 
  unnest_tokens(word, text)
# 第二步: 移除停用词
cleaned <- tokenized |> 
  anti_join(stop_words, by = "word")
# 第三步: 计数
word_counts <- cleaned |> 
  count(book, word, sort = TRUE)
# 第四步: 生成 DTM
book_dtm <- word_counts |> 
  cast_dtm(document = book, term = word, value = n)
```

```r
# 查看 summary of DTM
book_dtm

<<DocumentTermMatrix (documents: 6, terms: 13914)>>
Non-/sparse entries: 37224/46260
Sparsity           : 55%
Maximal term length: 19
Weighting          : term frequency (tf)

# non-sparse entries: 非0值,整个dtm里有三万多个频率非0的单元格
# sparse entries: 四万多个0单元格
# sparsity: 整个dtm中有55%的单元格是0值
# weighting: term frequency (tf) 指dtm单元格中的值是什么,这里是词频(tf)
```

> 注: 整个 `book_dtm` 中有太多词了 (三万多个), 不可能展示完整 matrix, 只看总结

==**词频矩阵转回成 tidy dataframe**==

```r
tidy_dtm <- tidy(book_dtm)
```

### 词频矩阵 (DTM) 热力图

```r
# 可视化词频矩阵 dtm

# 找出 top10 词频的词
top_10_words <- word_counts |> 
	group_by(word) |> 
	summarize(total = sum(n)) |> 
	slice_max(total, n = 10) |> 
	pull(word)
# 过滤只包含 top10 词的 data
dtm_subset <- word_counts |> 
	filter(word %in% top_10_words)

# 画热力图
ggplot(dtm_subset, aes(x = word, y = book, fill = n)) +
	geom_tile(color = "white") +
	geom_text(aes(label = n), color = "white", size = 3) +
	scale_fill_gradient(low = "steelblue", high = "darkred") +
	labs(
		title = "Document-Term Matrix Heatmap",
		subtitle = "Top 10 words across Jane Austen books",
		x = "Term",
		y = "Document",
		fill = "Frequency"
	) +
	theme_minimal() +
	theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

![[Pasted image 20260321150000.png]]

### 词频 (TF) 柱状图分面网格

```r
ggplot(top_words, aes(x = reorder(word, n), y = n, fill = book)) +
	geom_col(show.legend = FALSE) +
	coord_flip() +
	facet_wrap(~ book, scales = "free_y") +
	labs(
		title = "Top 5 Words in Each Jane Austen Book",
		x = "Word",
		y = "Frequency"
	) +
	theme_minimal()
```

![[Pasted image 20260321150413.png]]

## 🔹 TF-IDF 词频–逆文档频率
(Term Frequency - Inverse Document Frequency)

$$
\text{TF-IDF} = 
	\text{TF} \times 
	\log \left( 
		\frac{ \text{Total Documents} }
		{ \text{Documents containing term} }
	\right)
$$

| 指标 | 含义 |
|------|------|
| TF（词频） | 词在当前文档中出现的频率 |
| IDF（逆文档频率） | 词在所有文档中的稀有程度 |
| TF-IDF 高 | 在当前文档频繁出现，但在其他文档罕见 → ==该词对本文档有区分性== |
| TF-IDF 低 | 在所有文档中普遍出现 → 区分度低（如停用词） |

==**计算 TF-IDF**==

`bind_tf_idf(tbl, term, document, n)`

```r
book_tfidf <- original_books |> 
	unnest_tokens(word, text) |> 
	count(book, word, sort = TRUE) |> 
	bind_tf_idf(word, book, n)
```

## 🔹 词云 Word Clouds

```r
# 第一步: 导入包
library(wordcloud2)

# 第二步: 准备词频数据
word_freq <- original_books |>
	unnest_tokens(word, text) |>
	anti_join(stop_words, by = "word") |>
	count(word, sort = TRUE)

# 第三步: 画词云
wordcloud2(data = word_freq, size = 0.5)
```

### 自定义词云样式

| 参数 | 说明 | 示例 |
|------|------|------|
| `size` | 词语大小缩放比例 | `size = 0.5` |
| `color` | 配色方案 | `color = "random-light"` |
| `backgroundColor` | 背景颜色 | `backgroundColor = "black"` |
| `shape` | 词云形状 | `shape = "circle"` 或 `"star"` |
| `minRotation`, `maxRotation` | 词语旋转角度范围 | `minRotation = -pi/4` |

### 导出词云

```r
library(htmlwidgets)
library(webshot)
webshot::install_phantomjs()

# 第一步: 创建词云
my_cloud <- wordcloud2(word_freq, size = 1)
# 第二步: 导出 HTML
saveWidget(my_cloud, "wordcloud.html", selfcontained = FALSE)
# 第三步: 导出 PNG
webshot("wordcloud.html", "wordcloud.png", delay = 5)
```

---

# 🟢 L5-Descriptive Statistics

## 🔹 Central Tendency

```r
mean()
median()
# 众数 (mode)
get_mode <- function(x) {
	unique_x <- unique(x)
	unique_x[which.max(tabulate(match(x, unique_x)))]
}
summary()
```

| 指标 | 适用场景 | 对异常值敏感? |
|------|----------|--------------|
| 均值 `mean` | 对称分布、连续数据 | 是 |
| 中位数 `median` | 偏态分布、有序数据 | 否 |
| 众数 `mode` | 类别数据、找最常见值 | 否 |

## 🔹 Spread

```r
range()
max() - min()
diff(range())      # 等于 max - min

var()              # variance 方差
sd(); sqrt(var())  # 标准差
```

> `range()` 对异常值极度敏感

```r
# 这两个都可以 na.rm=TRUE
quantile(x=列, probs=分位百分比)
IQR(x=列)
```

## 🔹 Correlation

```r
cor(df$col1, df$col2)
cor.test(df$col1, df$col2) # p<.05 且 |r|>.6 的有关联
```

### Correlation Heatmap

```r
# Create correlation matrix
cor_matrix <- cor(num_var)

# Convert to long dataframe format for ggplot
cor_long <- cor_matrix |> 
  as.data.frame() |>  # convert matrix to dataframe
  rownames_to_column("var1") |> 
  pivot_longer(-var1, names_to = "var2", values_to = "correlation")

# Heatmap
ggplot(cor_long, aes(x = var1, y = var2, fill = correlation)) +
  geom_tile(color = "white") +
  geom_text(aes(label = round(correlation, 2)), color = "black", size = 4) +
  scale_fill_gradient2(
    low = "steelblue", mid = "white", high = "firebrick", 
    midpoint = 0, limits = c(-1, 1)
  ) +
  labs(
    title = "Correlation Heatmap",
    x = "", y = "",
    fill = "Correlation"
  ) +
  # theme_minimal() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

![[Pasted image 20260322153026.png]]

## 🔹 Cross-Tabulations

```r
ggplot(df, aes(x = type1, y = value, fill = type2)) + 
	geom_col(position="dodge") + # bar分组并列排布
	scale_fill_brewer(palette = "Set2") +
	labs(
		title = "Barplot of value by type1 and type2",
		x = "type1",
		y = "value",
		fill = "type2"
	)
```

---

# 🟢 L6-Inferential Statistics

## 🔹 假设检验 Hypothesis Testing

| 函数 | 用途 | 关键参数 |
|------|------|---------|
| `t.test(y ~ x, data)` | 独立样本 t 检验 (==只能用于 2 组 cat==) | `paired=TRUE` 配对检验 |
| `aov(y ~ x, data)` | 单因素方差分析 (3+ 组) | — |
| `summary(anova_result)` | 查看 ANOVA F 值与 p 值 (不显示配对) | — |
| `TukeyHSD(anova_result)` | 事后检验 (Tukey HSD)，查看==配对差异== | — |

> **决策规则：** p < 0.05 → 拒绝 H₀；p ≥ 0.05 → 无法拒绝 H₀。
> ==永远不要说 "accept H₀" / "prove null"==，只能说 "fail to reject H₀"。

**t-test 输出解读：**

| 字段 | 含义 |
|------|------|
| `t` | t 统计量 |
| `df` | 自由度 |
| `p-value` | 观察到此差异的概率（若 H₀ 为真） |
| `95% CI` | 真实均值差异的 95% 置信区间 |
| `mean in group X` | 各组均值 |

**可视化 t-test / ANOVA snippet：**

```r
# Boxplot + 均值点
ggplot(df, aes(x = group_var, y = outcome, fill = group_var)) +
  geom_boxplot(alpha = 0.7) +
  stat_summary(fun = mean, geom = "point", shape = 18, size = 4, color = "black") +
  theme_minimal() + theme(legend.position = "none")

# ANOVA subtitle 取值方式 (anova_result 是 list)
paste("F =", round(summary(anova_result)[[1]]$`F value`[1], 2))
```

---

## 🔹 回归 Regression

| 函数 | 用途 | 示例 |
|------|------|------|
| `lm(y ~ x, data)` | 简单线性回归 | `lm(GPA ~ Study_Hours, data=df)` |
| `lm(y ~ x1 + x2, data)` | 多元线性回归 | `lm(GPA ~ Study_Hours + college, data=df)` |
| `summary(model)` | 查看系数、R²、F 统计量 | — |
| `predict(model, newdata)` | 预测新值 | `predict(m, data.frame(x=8))` |
| `tidy(model, conf.int=TRUE)` | `broom` 整洁系数表 | 含 `conf.low`, `conf.high` |
| `ggpredict(model, terms="x")` | `ggeffects` 边际效应 | `plot(effects_hours)` |

> **Dummy variable：** 含 k 个水平的 cat 变量，R 自动生成 k-1 个 dummy，==第一个字母顺序的类别为参考组==。
> 手动改参考组：`factor(col, levels = c("新参考", ...))`，再重新拟合模型。

**输出解读：**

| 字段 | 含义 |
|------|------|
| `Intercept` | 所有预测变量 = 0 时的预测 Y |
| `β` (slope) | X 每增加 1 单位，Y 的变化量（控制其他变量） |
| `R²` | 模型解释的 Y 方差比例 |
| `Adjusted R²` | 校正预测变量数量后的 R² |
| `F-statistic` | 整体模型显著性 |

**系数图 (Coefficient Plot) snippet：**

```r
library(broom)
coef_data <- tidy(model, conf.int = TRUE) |> filter(term != "(Intercept)")

ggplot(coef_data, aes(x = estimate, y = reorder(term, estimate))) +
  geom_point(size = 3, color = "steelblue") +
  geom_errorbarh(aes(xmin = conf.low, xmax = conf.high), height = 0.2) +
  geom_vline(xintercept = 0, linetype = "dashed", color = "red") +
  theme_minimal()
```

> ==CI 穿过 0 → 不显著 (p > .05)；不穿过 0 → 显著==。离 0 越远，效应越强。

---

## 🔹 广义线性模型 GLM (Generalized Linear Model)

> GLM 部分标注 🔴no need for quiz / exam，了解即可。

| 结果类型 | 分布 | R 函数 | 示例 |
|---------|------|--------|------|
| 连续型 | Normal | `lm()` | GPA、收入 |
| 二元 (0/1) | Binomial | `glm(family=binomial)` | 通过/失败 |
| 计数 | Poisson | `glm(family=poisson)` | 事件次数 |

```r
# Poisson 回归示例
poisson_model <- glm(count ~ x, family = poisson, data = df)
exp(coef(poisson_model))  # 转换为 Incident Rate Ratio (原始尺度)
```

---

## 🔹 交互作用 Interaction Terms

| 函数 | 用途 |
|------|------|
| `lm(y ~ x1 * x2, data)` | 包含交互项（同时包含主效应和交互效应） |
| `ggpredict(model, terms = c("x1", "x2"))` | 可视化交互效应（边际效应图） |

```r
interaction_model <- lm(GPA ~ Study_Hours * Stress_Level, data = df)

effects_int <- ggpredict(interaction_model, terms = c("Study_Hours", "Stress_Level"))
plot(effects_int)
```

**交互图判读：**

| 线型 | 含义 |
|------|------|
| 平行线 | 无交互效应，X 对 Y 的影响在各组一致 |
| 发散/收敛 | 存在交互，效应随调节变量变化 |
| 交叉线 | 强交互，效应方向在不同组间反转 |

> `x1 * x2` 等价于 `x1 + x2 + x1:x2`，自动包含主效应。
> 交互项系数 = 两组斜率之差（相对于参考组）。

---

[↑回到顶部](#🔗%20Quicklinks)


---
start: 2026-03-25
tags:
    - r
    - dv
    - da
    - ds
---


```r
question answered: 21 (1-20, 26, 27, -11) (11没写, 1分)
score: 24/40

26,2
27,2


```


1. 不要用 q <- ??, 因为如果有人没写完就 knit pdf 会报错, 整个 rmd 脚本无法从头到尾运行
2. 最后的检查function两个都有问题, 因为 knit pdf 必须用 latex, 而所有输出的结果都必须是可以转换成 character 的才能被 latex 转换
3. 用 `output: pdf_document` 前必须先 install latex module (好像是 tinylatex 之类的)
4. 所以不要用 `output: pdf_document` , 可以用 html 然后 print pdf

# Tips
1. 他让你 add new column, 是后续需要用的, 
   所以必须要 `data <- data |> mutate()...`
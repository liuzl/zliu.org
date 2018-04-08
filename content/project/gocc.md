+++
# Date this page was created.
date = "2018-02-16"

# Project title.
title = "gocc: Golang version OpenCC 繁簡轉換"

# Project summary to display on homepage.
summary = "gocc is a golang port of OpenCC([Open Chinese Convert 開放中文轉換](https://github.com/BYVoid/OpenCC/)) which is a project for conversion between Traditional and Simplified Chinese developed by [BYVoid](https://www.byvoid.com/)."

# Optional image to display on homepage (relative to `static/img/` folder).
image_preview = "bubbles.jpg"

# Tags: can be used for filtering projects.
# Example: `tags = ["machine-learning", "deep-learning"]`
tags = ["Chinese", "NLP"]

# Optional external URL for project (replaces project detail page).
external_link = "https://github.com/liuzl/gocc"

# Does the project detail page use math formatting?
math = false

# Optional featured image (relative to `static/img/` folder).
#[header]
#image = "headers/bubbles-wide.jpg"
#caption = "My caption :smile:"

highlight_languages = ["go"]

+++

## Introduction 介紹
gocc is a golang port of OpenCC([Open Chinese Convert 開放中文轉換](https://github.com/BYVoid/OpenCC/)) which is a project for conversion between Traditional and Simplified Chinese developed by [BYVoid](https://www.byvoid.com/).

gocc stands for "**Go**lang version Open**CC**", it is a total rewrite version of OpenCC by Go. It just borrows the dict files and config files of OpenCC, so it may not produce the same output with the original OpenCC.

## Installation 安裝
### 1, golang package
```sh
go get github.com/liuzl/gocc
```
### 2, Command Line
```sh
git clone https://github.com/liuzl/gocc
cd gocc/cmd
make install
gocc --help
echo "我们是工农子弟兵" | gocc
#我們是工農子弟兵
```

## Usage 使用
```go
package main

import (
    "fmt"
    "github.com/liuzl/gocc"
    "log"
)

func main() {
    s2t, err := gocc.New("s2t")
    if err != nil {
        log.Fatal(err)
    }
    in := `自然语言处理是人工智能领域中的一个重要方向。`
    out, err := s2t.Convert(in)
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("%s\n%s\n", in, out)
    //自然语言处理是人工智能领域中的一个重要方向。
    //自然語言處理是人工智能領域中的一個重要方向。
}
```
## Conversions
* `s2t` Simplified Chinese to Traditional Chinese
* `t2s` Traditional Chinese to Simplified Chinese
* `s2tw` Simplified Chinese to Traditional Chinese (Taiwan Standard)
* `tw2s` Traditional Chinese (Taiwan Standard) to Simplified Chinese
* `s2hk` Simplified Chinese to Traditional Chinese (Hong Kong Standard)
* `hk2s` Traditional Chinese (Hong Kong Standard) to Simplified Chinese
* `s2twp` Simplified Chinese to Traditional Chinese (Taiwan Standard) with Taiwanese idiom
* `tw2sp` Traditional Chinese (Taiwan Standard) to Simplified Chinese with Mainland Chinese idiom
* `t2tw` Traditional Chinese (OpenCC Standard) to Taiwan Standard
* `t2hk` Traditional Chinese (OpenCC Standard) to Hong Kong Standard

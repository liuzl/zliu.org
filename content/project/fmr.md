+++
# Date this page was created.
date = "2017-04-27"

# Project title.
title = "FMR: functional meaning representation"

# Project summary to display on homepage.
summary = "Parsing all context free grammars using Earley's algorithm in Golang."

# Optional image to display on homepage (relative to `static/img/` folder).
image_preview = "bubbles.jpg"

# Tags: can be used for filtering projects.
# Example: `tags = ["machine-learning", "deep-learning"]`
tags = ["nlp", "nlu", "semantic parsing"]

# Optional external URL for project (replaces project detail page).
external_link = "https://github.com/liuzl/fmr"

# Does the project detail page use math formatting?
math = false

# Optional featured image (relative to `static/img/` folder).
#[header]
#image = "headers/bubbles-wide.jpg"
#caption = "My caption :smile:"

+++

* 语义理解平台/语音助手（2013-今，腾讯/搜狗）
  * 复合实体识别（CRF识别/规则识别/词表识别）
  * 基于语言模型的查询意图理解
  * **基于语义模板的查询意图识别和语义信息抽取**
      * 通用CFG文法解析器：高效Earley算法的实现
      * 意图识别，槽位填充
  * 查询意图排序，基于候选意图结果的终判模块
  * 下游分类别知识库系统/垂直搜索系统

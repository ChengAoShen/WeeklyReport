---
marp: true
size: 16:9
theme: am_green
paginate: true
# footer: ChengAo Shen
---

<!-- _class: cover_e -->
<!-- _header: ![SICAU logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/SICAU_logo.png) -->
<!-- _footer: ![SICAU brand](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/SICAU_brand.png) -->
<!-- _paginate: "" -->

# 汇报模版

## 深度视觉农业实验室

ChengAo Shen
Jul 18, 2024

---

<!-- _class: toc_b -->
<!-- _header: 目录<br>CONTENTS<br>![SICAU logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/SICAU_logo.png)-->
<!-- _footer: "" -->
<!-- _paginate: "" -->

- [关于模板](#3)
- [页面分栏与列表分列](#20)
- [引用、链接和引用盒子](#38)

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->
## 1. 关于模板

---

<!-- _class: navbar -->
<!-- _header: \ ***深度视觉农业实验室*** **关于模板** *页面分栏与列表分列* *引用、链接和引用盒子* -->
## 1. 关于模板

- 模版来源于**初虹**开发的[Awesome Marp](https://github.com/favourhong/Awesome-Marp/tree/main)项目，该项目主要目的是为单调的[Marp](https://marp.app/)提供更多功能。
- 本人在其基础上针对于四川农业大学进行了二次开发和删减，将其主要特征进行固定，便于本校同学直接进行使用。
- 原始Awesome Marp 支持 38 个自定义样式，使用时需在页面指定（如 `<!-- _class: trans -->`）：

| 封面页    | 目录页  | 列表        | 引用盒子    | 其他 1                        | 其他 2                                                          |
| --------- | ------- | ----------- | ----------- | ----------------------------- | --------------------------------------------------------------- |
| `cover_a` | `toc_a` | `cols-2`    | `bq-black`  | 过渡页面 `trans`              | 图表等的标题 `caption`                                          |
| `cover_b` | `toc_b` | `cols-2-64` | `bq-purple` | 最后一页 `lastpage`           | 非嵌套无序列表的毛玻璃效果 `fglass`                             |
| `cover_c` |         | `cols-2-73` | `bq-red`    | 导航栏 `navbar`               | 脚注：`footnote`                                                |
| `cover_d` |         | `cols-3`    | `bq-blue`   | 标题固定+无底色 `fixedtitleA` | 调节字体大小：`tinytext`/`smalltext`/<br>`largetext`/`hugetext` |
| `cover_e` |         | `cols-2-46` | `bq-green`  | 标题固定+有底色 `fixedtitleB` |                                                                 |
|           |         | `cols-2-37` |             | 两列有序列表 `cols2_ol_sq/ci`                              |                                                                 |
|           |         | `rows-2`    |             | 两列无序列表 `cols_ul_sq/ci`                              |                                                                 |
|           |         | `pin-3`            |             | 单列有序列表 `col1_ul_sq/ci`                              |                                                                 |

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->
## 2. 页面分栏与列表分列

---

<!-- _class: navbar fixedtitleA -->
## 2.1 页面分栏与列表分列：页面分栏
<!-- _header: \ ***深度视觉农业实验室*** *关于模板* **页面分栏与列表分列** *引用、链接和引用盒子* -->
- Awesome Marp 提供了 8 种页面分栏方式，分别为：
  - `cols-2`：[两列分栏，五五平分](#23)
  - `cols-2-64`：[两列分栏，六四分](#24)
  - `cols-2-73`：[两列分栏，七三分](#25)
  - `cols-2-46`：[两列分栏，四六分](#26)
  - `cols-2-37`：[两列分栏，三七分](#27)
  - `cols-3`：[三列分栏，平分](#28)
  - `rows-2`：[两行分栏](#29)
  - `pin-3`：[品字型分栏](#30)

- 如果某一栏为图片，可以将 `class=ldiv` 换成 `class=limg`，这样能够实现图片的垂直居中对齐呢（`class=ldiv` 为居上对齐）
---

<!-- _class: navbar fixedtitle -->
<!-- _header: \ ***深度视觉农业实验室*** *关于模板* **页面分栏与列表分列** *引用、链接和引用盒子* -->

## 2.1 页面分栏与列表分列：页面分栏

- 以 `<!-- _class: cols-2 -->` 为例，Markdown 的源码为：

```markdown
<!-- _class: cols-2 -->  
<div class=ldiv>  

第一列（左侧栏）的内容在这里

内容可以是普通纯文本，可以是列表，也可以是引用块、链接、图片等
</div>

<div class=rdiv>

第二列（右侧栏）的内容在这里
</div>
```

- 如果是分三栏（`<!-- _class: cols-3 -->`），还需要再增加 `<div class="mdiv"></div>` 标签

---

<!-- _class: navbar fixedtitleA -->
<!-- _header: \ ***深度视觉农业实验室*** *关于模板* **页面分栏与列表分列** *引用、链接和引用盒子* -->
## 2.2 页面分栏与列表分列：列表分列

Awesome Marp v1.3 提供了 6 种列表分列的方式，分别为：

- `cols2_ol_sq`：呈现效果为[有序列表 + 方形序号](#32)
- `cols2_ol_ci`：呈现效果为[有序列表 + 圆形序号](#33)
- `cols2_ul_sq`：呈现效果为[无序列表 + 方形序号](#34)
- `cols2_ul_ci`：呈现效果为[无序列表 + 圆形序号](#35)
- `col1_ul_sq`：呈现效果为[有序列表 + 方形序号](#36)
- `col1_ul_ci`：呈现效应为[有序列表 + 圆形序号](#37)

---
<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->
## 3. 引用、链接和 Callouts

---
<!-- _class: navbar fixedtitleA -->
<!-- _header: \ ***深度视觉农业实验室*** *关于模板* *页面分栏与列表分列* **引用、链接和引用盒子** -->
## 3. 引用、链接和 Callouts

- 引用的呈现效果为：

> 合成控制法 (Synthetic Control Method) 最早由 Abadie and Gardeazabal (2003) 提出，用来研究西班牙巴斯克地区恐怖活动的经济成本，属于案例研究范畴 (Case Study)。

- 链接的呈现效果：
  - [经管数据清洗与 Stata 实战：三大地级市数据库和 CSMAR 上市公司数据](https://mp.weixin.qq.com/s/D0cYVPJJsNiu61GcYwV6cg)
  - [Stata 基础：从论文文件夹体系的建立说起](https://mp.weixin.qq.com/s?__biz=MzkwOTE3NDExOQ==&mid=2247486489&idx=1&sn=2eb51e85a01541c7a552a9434e087512&scene=21#wechat_redirect)
- Callouts 是 Awesome Marp 提供的自定义的样式，有 5 种颜色可选：
  - [紫色](#40)：`bq-purple`
  - [蓝色](#41)：`bq-blue`
  - [绿色](#42)：`bq-green`
  - [红色](#43)：`bq-red`
  - [黑色](#44)：`bq-black`


---
title: new draft
tags:
---

Front-matter
Front-matter 是 markdown 文件最上方以 --- 分隔的區域，用於指定個別檔案的變數。

Page Front-matter 用於頁面配置
Post Front-matter 用於文章頁配置
如果標注可選的參數，可根據自己需要添加，不用全部都寫在markdown裏

Page Front-matter
MARKDOWN
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
---
title:
date:
updated:
type:
comments:
description:
keywords:
top_img:
mathjax:
katex:
aside:
aplayer:
highlight_shrink:
---
寫法	解釋
title	【必需】頁面標題
date	【必需】頁面創建日期
type	【必需】標籤、分類和友情鏈接三個頁面需要配置
updated	【可選】頁面更新日期
description	【可選】頁面描述
keywords	【可選】頁面關鍵字
comments	【可選】顯示頁面評論模塊(默認 true)
top_img	【可選】頁面頂部圖片
mathjax	【可選】顯示mathjax(當設置mathjax的per_page: false時，才需要配置，默認 false)
katex	【可選】顯示katex(當設置katex的per_page: false時，才需要配置，默認 false)
aside	【可選】顯示側邊欄 (默認 true)
aplayer	【可選】在需要的頁面加載aplayer的js和css,請參考文章下面的音樂 配置
highlight_shrink	【可選】配置代碼框是否展開(true/false)(默認為設置中highlight_shrink的配置)
Post Front-matter
MARKDOWN
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
---
title:
date:
updated:
tags:
categories:
keywords:
description:
top_img:
comments:
cover:
toc:
toc_number:
toc_style_simple:
copyright:
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
mathjax:
katex:
aplayer:
highlight_shrink:
aside:
---
寫法	解釋
title	【必需】文章標題
date	【必需】文章創建日期
updated	【可選】文章更新日期
tags	【可選】文章標籤
categories	【可選】文章分類
keywords	【可選】文章關鍵字
description	【可選】文章描述
top_img	【可選】文章頂部圖片
cover	【可選】文章縮略圖(如果沒有設置top_img,文章頁頂部將顯示縮略圖，可設為false/圖片地址/留空)
comments	【可選】顯示文章評論模塊(默認 true)
toc	【可選】顯示文章TOC(默認為設置中toc的enable配置)
toc_number	【可選】顯示toc_number(默認為設置中toc的number配置)
toc_style_simple	【可選】顯示 toc 簡潔模式
copyright	【可選】顯示文章版權模塊(默認為設置中post_copyright的enable配置)
copyright_author	【可選】文章版權模塊的文章作者
copyright_author_href	【可選】文章版權模塊的文章作者鏈接
copyright_url	【可選】文章版權模塊的文章連結鏈接
copyright_info	【可選】文章版權模塊的版權聲明文字
mathjax	【可選】顯示mathjax(當設置mathjax的per_page: false時，才需要配置，默認 false)
katex	【可選】顯示katex(當設置katex的per_page: false時，才需要配置，默認 false)
aplayer	【可選】在需要的頁面加載aplayer的js和css,請參考文章下面的音樂 配置
highlight_shrink	【可選】配置代碼框是否展開(true/false)(默認為設置中highlight_shrink的配置)
aside	【可選】顯示側邊欄 (默認 true)
標籤頁
前往你的 Hexo 博客的根目錄

輸入hexo new page tags

你會找到source/tags/index.md這個文件

修改這個文件：

記得添加 type: "tags"

MARKDOWN
1
2
3
4
5
---
title: 標籤
date: 2018-01-05 00:00:00
type: "tags"
---
分類頁
前往你的 Hexo 博客的根目錄

輸入hexo new page categories

你會找到source/categories/index.md這個文件

修改這個文件：

記得添加 type: "categories"

MARKDOWN
1
2
3
4
5
---
title: 分類
date: 2018-01-05 00:00:00
type: "categories"
---
友情鏈接
為你的博客創建一個友情鏈接！

創建友情鏈接頁面
前往你的 Hexo 博客的根目錄

輸入 hexo new page link

你會找到source/link/index.md這個文件

修改這個文件：

記得添加 type: "link"

MARKDOWN
1
2
3
4
5
---
title: 友情鏈接
date: 2018-06-07 22:17:49
type: "link"
---
友情鏈接添加
本地生成
遠程拉取
在Hexo博客目錄中的source/_data（如果沒有 _data 文件夾，請自行創建），創建一個文件link.yml

YML
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
- class_name: 友情鏈接
  class_desc: 那些人，那些事
  link_list:
    - name: Hexo
      link: https://hexo.io/zh-tw/
      avatar: https://d33wubrfki0l68.cloudfront.net/6657ba50e702d84afb32fe846bed54fba1a77add/827ae/logo.svg
      descr: 快速、簡單且強大的網誌框架

- class_name: 網站
  class_desc: 值得推薦的網站
  link_list:
    - name: Youtube
      link: https://www.youtube.com/
      avatar: https://i.loli.net/2020/05/14/9ZkGg8v3azHJfM1.png
      descr: 視頻網站
    - name: Weibo
      link: https://www.weibo.com/
      avatar: https://i.loli.net/2020/05/14/TLJBum386vcnI1P.png
      descr: 中國最大社交分享平臺
    - name: Twitter
      link: https://twitter.com/
      avatar: https://i.loli.net/2020/05/14/5VyHPQqR6LWF39a.png
      descr: 社交分享平臺
      class_name 和 class_desc 支持 html 格式書寫，如不需要，也可以留空。


友情鏈接界面設置
由 2.2.0 起，友情鏈接界面可以由用戶自己自定義，只需要在友情鏈接的md檔設置就行，以普通的Markdown格式書寫。

圖庫
圖庫頁面只是普通的頁面，你只需要hexo n page xxxxx 創建你的頁面就行

然後使用標簽外掛 galleryGroup，具體用法請查看對應的內容。

YAML
1
2
3
4
5
<div class="gallery-group-main">
{% galleryGroup '壁紙' '收藏的一些壁紙' '/Gallery/wallpaper' https://i.loli.net/2019/11/10/T7Mu8Aod3egmC4Q.png %}
{% galleryGroup '漫威' '關於漫威的圖片' '/Gallery/marvel' https://i.loli.net/2019/12/25/8t97aVlp4hgyBGu.jpg %}
{% galleryGroup 'OH MY GIRL' '關於OH MY GIRL的圖片' '/Gallery/ohmygirl' https://i.loli.net/2019/12/25/hOqbQ3BIwa6KWpo.jpg %}
</div>


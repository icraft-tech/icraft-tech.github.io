---
layout: post
title:  "github pageでブログ作成"
date:   2017-11-08 20:40:35 -0700
tags:
- Web
---

##github pageでブログ作成

本サイトは、アイクラフト株式会社のブログ・サイトとして、
新たにサイトを作成致しました。
記念すべき最初の投稿では、本ページの構築手順についてお話したいと思います。

本サイトを'github pages'と'Jekyll'の2つのツールを使い、ブログを作りました。
要は、wordpressの静的ジェネレータ版といったところです。
まずは、以下で2つのツールを紹介します。


{% highlight ruby %}
root/
├ _config.yml----------------------------(設定ファイル)
├  index.html-----------------------(最初に読み込むhtml)
├ cssーscreen.css-------------------------(cssファイル)
├ _post                              (mdで書かれた記事)
├ _layoutsーpage.html----------------------(レイアウト)
├ _includes----------------------------(分割されたhtml)
　　　　　├ top.html        
　　　　　├ body.html
　　　　　└ header.html
{% endhighlight %}

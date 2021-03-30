---
layout: post
title: このサイトでの投稿について
tags: markdown
categories: common
---


* TOC
{:toc}

# マークダウンの書き方

このサイトでの投稿はMarkdownでなされ、各Markdownページは`YYYY-MM-DD-ページタイトル.md`というファイルである。
（YYYYは投稿年、MM、DDは投稿月と投稿日。ページタイトルは英語）

mdファイルのはじめに
```
---
layout: post
title: このサイトでの投稿について
tags: markdown
categories: markdown
---
```
を書かなければならない。（上のはこのページのもの）

```
---
layout: post
title: VIM
tags: VIM Linux
categories: special
published: false
---
```
のように`published: false`とすれば公開されない（下書き状態）。

ページのURLは`ベースURL/上で設定したcategories/mdファイル名で設定したページタイトル/`となるので、***ページタイトルは重複しないようにしなければならない。***

そして右側にあるTocに
# テストヘッダ
## テストヘッダ２
などの`#`で記すヘッダをのせるために
```
*TOC
{:toc}
```
を書かなければならない。

**つまり毎回**

```
---
layout: post
title: このサイトでの投稿について
tags: markdown
categories: markdown
---

*TOC
{:toc}
```
**を書かなければならない。**

<br><br>

---

# よく使うマークダウン

数字や式は
```
数字
`3.141592653`
式
`x^2+y^2=1`
```
とすることで
数字
`3.141592653`
式
`x^2+y^2=1`
となる。

また、

---

のような区切り線は
```
---
```
でできる。

<br>

**ふと文字**は
```
**ふと文字**
```

<br>

* 好きな哲章：玄田哲章
* 好きな運昇：石塚運昇
* 好きな万丈：銀河万丈

のような箇条書きは

```
* 好きな哲章：玄田哲章
* 好きな運昇：石塚運昇
* 好きな万丈：銀河万丈
```

でできる。

<br>

>『B-伝説! バトルビーダマン』（ビーレジェンド バトルビーダマン）は、犬木栄治による漫画作品。およびこれを原作としたアニメなどのメディアミックス作品。

出典:<a href="https://ja.wikipedia.org/wiki/B-%E4%BC%9D%E8%AA%AC!_%E3%83%90%E3%83%88%E3%83%AB%E3%83%93%E3%83%BC%E3%83%80%E3%83%9E%E3%83%B3">フリー百科事典『ウィキペディア（Wikipedia）』</a>

のような引用、リンクは

```
>『B-伝説! バトルビーダマン』（ビーレジェンド バトルビーダマン）は、犬木栄治による漫画作品。およびこれを原作としたアニメなどのメディアミックス作品。

出典:<a href="https://ja.wikipedia.org/wiki/B-%E4%BC%9D%E8%AA%AC!_%E3%83%90%E3%83%88%E3%83%AB%E3%83%93%E3%83%BC%E3%83%80%E3%83%9E%E3%83%B3">フリー百科事典『ウィキペディア（Wikipedia）』</a>
```

で可能。

<br>

画像は

`/static/img/`に入れて

```
![bitmap][bitmap]

[bitmap]: {*{"/bitmap.jpg" | prepend: site.imgrepo }*} ←*を消す
```

とすることで、以下のように挿入できる。

```
![image-title](画像のURL)
```

URLにすることで外部の画像も貼り付け可能。

![bitmap][bitmap]

[bitmap]: {{"/bitmap.jpg" | prepend: site.imgrepo }}

<br><br>

---
# ここまでのスクリプト

<a href="https://raw.githubusercontent.com/AriNie/TripleBrain/master/_posts/2021-1-28-about-posting.md">このページのスクリプト（Github）</a>
---
layout: post
title: 競プロにおける二項係数を使ったテクニック # ページタイトル
tags: atcoder algorithm c# # タグ
categories: atcoder # カテゴリ. AtCoderの解説記事は atcoder で.
published: true # ここは必ずtrueにしてください.
---


* TOC
{:toc}

Author: Non　<!-- 自分の名前 -->

<!-- ↓↓↓↓↓ 記事内容 ↓↓↓↓↓ -->

# はじめに

競技プログラミングの問題として二項係数を大きい素数（例えば$10^7$）で割るという問題がおおくあり、単純に計算すると値が非常に大きくなり、32bit, 64bitでは間に合わない。そして計算量も$O(n)$となってしまう。そこで以下で述べるテクニックで値の爆発、計算量の問題を解決する。

<br>

# ${}_n C_k$についてのアルゴリズム

## $k<=n<=10^7$で$P$が素数のとき

1. $i$の会場を$P$で割ったあまりを`fact[i]`としてその逆元を`fact_inv[i]`、$i$の逆元を`inv[i]`としておく。

2. $i$が$0$から$n$のときまで`fact[i]`、`fact_inv[i]`、`inv[i]`を計算しておく。

3. ${}_n C_k$を素数$P$で割ったあまりを`fact[n] * fact_inv[k] * fact_inv[n - k] / P`とする。

1.の逆元を求める際は

- `fact[i + 1]`: $(i+1)!\equiv i!\times (i+1) (mod P)$

- `inv[i + 1]`: $(i+1)^-1 \equiv -(P mod (i+1))^-1 \times [\frac{P}{i+1}]$

- `fact_inv[I+1]`: $((i+1)!)^-1\equiv (i!)^-1 \times (i+1)^-1 (mod P)$

### C#での実装

- 逆元を求める部分

```csharp
for (int i = 2; i < 10000000; i++) {
    fact[i] = fact[i - 1] * i % P;
    inv[i] = P - inv[P % i] * (P / i) % P;
    fact_inv[i] = fact_inv[i - 1] * inv[i] % P;
}
```

- 全体

```csharp
using System;
using System.Collections.Generic;

const int MOD = (int)1e9 + 7;

List<long> fact, fact_inv, inv;

void init_nCk(int SIZE) {
    fact = new List<long>(new long[SIZE + 5]);
    fact_inv = new List<long>(new long[SIZE + 5]);
    inv = new List<long>(new long[SIZE + 5]);
    fact[0] = fact[1] = 1;
    fact_inv[0] = fact_inv[0] = 1;
    inv[1] = 1;
    for (int i = 2; i < SIZE + 5; ++i) {
        fact[i] = fact[i - 1] * i % MOD;
        inv[i] = MOD - inv[MOD % i] * (MOD / i) % MOD;
        fact_inv[i] = fact_inv[i - 1] * inv[i] % MOD;
    }
}
```

## $n$が非常に大きい（$k<=10^7, n<=10^9$）とき

1. $i$が$0$から$k$のときまで、`fact_inv[i]`、`inv[i]`を計算しておく。

2. ${}_n C_k = \frac{n}{1} \times \frac{n-1}{2} \times ... \times \frac{n-k+1}{k}$を`fact_inv[k]`と`inv[i]`を利用して計算する。

2.の計算については

- 階乗の逆元`fact_inv[k]`: $(k!)^-1$を利用して${}_n C_k$を

    - $\frac{n}{1} \times \frac{n -1}{2} \times ... \times \frac{n-k+1}{k} = n(n-1)(n-2)...(n-k+1) \times $ `fact_inv[k]`

- nが固定値のとき

    -  `Com[k]`: ${}_n C_k$の値

    として`Com[[i]` $=$  `Com[i-1]` $\times \frac{n-i+1}{i}$

### C#での実装

- nが固定値でないとき

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
const int MOD = (int)1e9 + 7;

List<long> fact_inv, inv;

void init_nCk(int SIZE) {
    fact_inv = new List<long>(new long[SIZE + 5]);
    inv = new List<long>(new long[SIZE + 5]);
    fact_inv[0] = fact_inv[0] = 1;
    inv[1] = 1;
    for (int i = 2; i < SIZE + 5; ++i) {
        inv[i] = MOD - inv[MOD % i] * (MOD / i) % MOD;
        fact_inv[i] = fact_inv[i - 1] * inv[i] % MOD;
    }
}
```

- nが固定値のとき

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
const int MOD = (int)1e9 + 7;

List<long> fact_inv, inv, Com;

void init_nCk(int n, int SIZE) {
    fact_inv = new List<long>(new long[SIZE + 5]);
    inv = new List<long>(new long[SIZE + 5]);
    fact_inv[0] = fact_inv[0] = 1;
    inv[1] = 1;
    for (int i = 2; i < SIZE + 5; ++i) {
        inv[i] = MOD - inv[MOD % i] * (MOD / i) % MOD;
        fact_inv[i] = fact_inv[i - 1] * inv[i] % MOD;
    }
    Com = new List<long>(new long[SIZE + 5]);
    Com[0] = 1;
    for (int i = 1; i < SIZE + 5; ++i) {
        Com[i] = Com[i - 1] * ((n - i + 1) * inv[i] % MOD) % MOD;
    }
}
```

## 逆元を使用した二項係数の計算の仕組み

ここでは

${}_n C_k \equiv n! \dot (k!)^-1 \dot ((n-k)!)^-1 (mod P)$

特に$i^-1 \equiv -(P mod i)^-1 \times [\frac{P}{i}] (mod P)$

を示す。

まず、$P=[\frac{P}{i}] \times i + (P mod i)$ について$P$を法とすると

$0 \equiv [\frac{P}{i}] \times i + (P mod i) (mod P)$

全体に$i^-1$をかけて

$0 \equiv [\frac{P}{i}] + (P mod i) \times i^-1 (mod P)$

$(P mod i) \times i^-1 \equiv -[\frac{P}{i}] (mod P)$

$i^-1 \equiv -(P mod i) ^-1 \times [\frac{P}{i}] (mod P)$

となり、示される。

これをプログラミングすると

`inv[i] = P - inv[P % i] * (P / i) % P`

となる。

<br>







[foobarpiyopiyo]:{{"/foo/bar/piyo/piyo"|prepend:site.url}}


---
layout: post
title: "Visual Studio code を使ってみた"
date: 2015-05-07 10:25:50
categories: VisualStudioCode
---
## ダウンロード
-[VisulStudioCode](https://code.visualstudio.com/)

## インストール後

### 中華フォントを直す

なんでか知りませんが、中華フォントなので直します。

> %USERPROFILE%\AppData\Local\Code\app-0.1.0\resources\app\client\vs\monaco\ui\workbench\native\native.main.css

font-familyに中華フォントが大量に設定されているので直す。
アイコンフォントの指定をやっている時もあるので非常に紛らわしい。

### 動作どうなのよ？
軽くていいですが、カスタマイズの幅も狭いので痛し痒し。

### 見つけたバグ

日本語変換を行うと何故かHomeをおした時と同じ挙動をする。
日本語→変換の直後に入力すると全部消える。

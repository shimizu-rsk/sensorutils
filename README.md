# sensorutils

センサデータのロード関数やその他便利な関数群の共有リポジトリ。

こうしたほうが使いやすくない等の相談は issues や slack、直接言うなど気軽に行いたい。

## CAUTION

`core.py` を破壊的変更したため `dataset` 等のアップデートが必要！

## ファイル構成について

特に指定はないけどジャンル別にすると使用するときに使いやすいかも。
各ファイルに関して長くなったら分割、ファイル分けなどを行うこと。

* datasets  : データセットのロード関数
* core      : センサデータ処理関数（主に `preprocessing` を担当。肥大化したら適宜分離）
* stats     : 統計的処理
    * 可能なら `pandas` の `goupby` や `rolling` 関数を用いたほうが良い
* metrics   : センサデータの評価関数
    * 可能なら `sklearn.metrics` を使用したほうが良い
* doc       : ドキュメントファイルを格納
* tests     : テストコードを格納
    * `python -m unittest discover tests` で実行できるようにしておく？

詳細は `docs` ディレクトリへ

## usage

`sensorutils` をインポートすると `core.py` と `dataset` が自動的に読み込まれる。

インポートしたときに `doc` と `test` が補完に表示されるがインポートする意味はない。

```python
import sensorutils
import sensorutils.datasets
```

* [sensorutils.datasetの使い方](doc/samples)

詳細は `docs` ディレクトリへ

## requirement

**共通**
* python 3
    * 3.8 以外でも動くように設計しているが、エラーが出たら教えてください
* numpy
* pandas
* scipy

## Git 関連

サブモジュールを扱うときのコマンド（適宜引数は変えること）

サブモジュールとして clone する方法。
```bash
# すでにリポジトリが存在し、サブモジュールとして追加する場合
git submodule add https://github.com/haselab-dev/sensorutils.git
# サブモジュールの clone をし忘れた場合
git submodule update --init --recursive
# サブモジュールと同時にリポジトリを clone する場合
git clone --recursive [リポジトリの url など]
```

サブモジュールの更新を受け取るとき
```bash
git submodule foreach git fetch
git submodule foreach git merge origin/master
# 上二つの代わりに下でも可能
git submodule update --remote --merge
```

サブモジュールの更新をした時
```bash
git submodule foreach git add .
git submodule foreach git commit -m "2 on parent"
git submodule foreach git push
# このようにしてサブモジュールのリモートを更新したのち
git add .
git commit -m "update child 2"
git push --recurse-submodules=check
# 元のリポジトリのリモートの更新もする（確認）
```

サブモジュールが `Detached HEAD` になったときの対処
```bash
git submodule foreach git status #HEADのリビジョン名(SHA-1)を調べる
git submodule foreach git checkout master
git submodule foreach git merge <HEADのSHA-1> #rebse
git submodule foreach git push
```

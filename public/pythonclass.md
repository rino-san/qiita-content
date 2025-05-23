---
title: 'Pythonクラス入門:学生時代の脳筋コードを卒業するための再学習メモ'
tags:
  - Python
private: false
updated_at: '2025-05-19T12:59:40+09:00'
id: 741411d64e9dbec08276
organization_url_name: null
slide: false
ignorePublish: true
---
# 背景
本記事では、学生時代にクラス設計を深く考えずに脳筋コードばかり書いていた自分への反省を込めて、Pythonの「クラス」「継承」「オーバーライド」などの基本概念を整理します。

# 「学生時代の脳筋コードを卒業するための再学習メモ」シリーズのゴール
- 学生時代になんとなく学んでなんとなくは理解しているが、人に目的や意義が説明できなかったり実装できなかったりする技術を自分の知識・スキルとして定着すること

# この記事のゴール
- クラスに関する基礎概念を整理して、Pythonで簡易な実装ができるようになること

# 主要な知識
※ 今回の記事で中華料理を題材にしたクラス構造をベースにしています。
## クラスとインスタンス
- クラスは設計図、インスタンスはその設計図から作られる具体的なオブジェクトです。
- 以下の例は、中国料理というクラスを定義しています。中国料理には地域ごとに多種多様な味、作り方、辛さがあり、それらを定義するためのクラスです。

```python
class ChineseDish:
  cuisine_type = "中国料理"
  def __init__(self, name, region, spicy_level):
    self.name = name
    self.region = region
    self.spicy_level = spicy_level
  def describe(self):
    print(f"{self.name}は{self.cuisine_type}の代表的な料理です。")
    print(f"発祥地: {self.region}")
    print(f"辛さレベル: {self.spicy_level}")

```


## コンストラクタとは
- コンストラクタとは、クラスがインスタンス化されたときに実行される特別なメソッド`__init__`のことを指します。
- 主に初期値の設定に使われます。
- `super().__init__()`を使うと、親クラスのコンストラクタも呼び出すことができます。
```python
dish = ChineseDish("担々麺","四川","4")
dish.describe()
```
### 出力例
```
担々麺は中国料理の代表的な料理です。
発祥地: 四川
辛さレベル: 4
```


## 継承とは
- 既存のクラスの機能（メソッド）を引き継いで、新たなクラスを定義できます。
- Pythonでは`class 子クラス（親クラス）: `という構文で継承を行います。


```python
class SichuanDish(ChineseDish):
  def __init__(self,name,region):
    super().__init__(name,region,spicy_level=4)
```

## オーバーライドとは
- オーバーライドとは、親クラスから継承したメソッドを**子クラスで同じ名前で再定義して振る舞いを変える**ことです。
- Pythonでは、子クラスでオーバーライドされたメソッドが**優先的に呼ばれます。
- ただし、
- `super()`を使って明示的に親のメソッドを呼び出すこともできます。


```python
class SichuanDish(ChineseDish):
  def describe(self):
      print(f"{self.name} は四川料理（激辛！）です。")
      super().describe()
```

### 出力例
```
担々麺 は四川料理（激辛！）です。
担々麺は中国料理の代表的な料理です。
発祥地: 四川
辛さレベル: 4
```


## カプセル化とは
- カプセル化とは、**オブジェクトの内部状態（データ）を外部から直接アクセスできないようにして、メソッドを通じて操作させる**設計手法です。（実装TODO）
- Pythonでは、`_変数名`や`__変数名`のように名前にアンダースコアをつけて外部からアクセスしにくくします。

```python
print(dish.get_spicy_level())
class ChineseDish:
    ...
    def get_spicy_level(self):
        return self._spicy_level
```



## 多態性とは
- 多態性（ポリモーフィズム）とは、**親クラス型の変数を使ってこクラスのオブジェクトを扱える**性質のことです。
- 例えば、`ChineseFood`クラスを継承した`SichuanFood`と`CantoneseFood`がそれぞれ`set_spicy_level`メソッドを持っていれば、共通の`ChineseFood`型としてそれらを扱うことができます。



```python
dishes = [
    ChineseDish("饺子", "中国", 1),
    SichuanDish("四川麻婆豆腐", "四川")
]

for d in dishes:
    d.describe()  

```



## super()の使い方
`super()`は、親クラスのメソッドを呼び出す際に使います。オーバーライドした上で、元の動作も活かしたいときに便利です。

```python
class SpicyDish(ChineseDish):
    def __init__(self, name, region):
        super().__init__(name, region, spicy_level=5)
        print("これは超激辛料理です！")
```


## is-aの関係

- is-a関係とは、クラス設計の原則であり、**子クラスが親クラスの一種（a kind of）であること**を意味します。
- 例えば、`SichuanFood`は`ChineseFood`の一種である。→ `Sichuan food is a Chinese food.`.





## 継承を制限する方法
- どのようなクラスでも継承できるわけではなく、意図的に継承を防ぐ設計も可能です。
- `typing.final`で継承・オーバーライドを禁止することができます。


```python
from typing import final

@final
class Base:
  pass

# 以下はType Chekerから警告が出る。
# class Derived(Base): 
#   pass
```



## 再学習する中で生じた疑問
- オブジェクト指向はどういう場面で使われるのか？
  - 状態管理、拡張性のある設計、大規模開発に有効。
- アンダースコアの意味は？
  - `_name`: 内部利用の慣習。
  - `__name`: 名前修飾でサブクラスからの誤使用防止。
  - `__init__`; Pythonが自動で呼び出すマジックメソッド

- selfとは？
  - **インスタンス自身**を指すキーワード。インスタンス変数やメソッドにアクセスするために使います。

# 参考文献
- すっきりわかるJava入門第二部を参考にしつつPython的に解釈しました。

---
title: '【解決策】Google Colabでmoviepyを使用時のImageMagickエラー: 正しいインストール手順'
tags:
  - Python
  - moviepy
  - GoogleColaboratory
private: false
updated_at: '2024-01-12T21:25:11+09:00'
id: abd4b6f2fd5e5f53bfc9
organization_url_name: null
slide: false
ignorePublish: false
---



moviepyで動画に字幕を入れようとしたとき、
ImageMagickのエラーで躓いたためメモとして残しておきます。

## 該当コード
```python
txt_clip = TextClip("テキスト")
```


## エラー内容
```bash
OSError: MoviePy Error: creation of None failed because of the following error:

[Errno 2] No such file or directory: 'C:\\\\Program Files\\\\ImageMagick-7.0.8-Q16\\\\magick.exe'.

.This error can be due to the fact that ImageMagick is not installed on your computer, or (for Windows users) that you didn't specify the path to the ImageMagick binary in file conf.py, or that the path you specified is incorrect
```


## 解決策
Google Colabのランタイムを再起動し、以下のコードを実行すると無事終わりました。

```bash
apt update
apt install imagemagick
apt install ffmpeg
pip3 install moviepy
sed -i '/<policy domain="path" rights="none" pattern="@\*"/d' /etc/ImageMagick-6/policy.xml  
```



## ポイント

- `moviepy[optional]`としてインストールすることで、ImageMagickを含むすべての依存関係を一括でインストールできます。
- `ffmpeg`や`imagemagick`は`moviepy`よりも前にインストールする必要があります。
- `'/<policy domain="path" rights="none" pattern="@\*"/d'`というポリシーを削除することで、エラーを回避できます。

最初は日本語で調べていましたが、なかなか解決できなかったため
[GithubのIssue][1]の解決策をひたすら試し、[該当コメント][2]によってようやく解決しました。
感謝。



### 参考

- [MoviePy 公式ドキュメント](https://pypi.org/project/moviepy/)
- [GitHub Issue: ImageMagick Error](https://github.com/Zulko/moviepy/issues/693)
- [GitHub Issue Comment](https://github.com/Zulko/moviepy/issues/693#issuecomment-622997875)

[1]:https://github.com/Zulko/moviepy/issues/693
[2]:https://github.com/Zulko/moviepy/issues/693#issuecomment-622997875

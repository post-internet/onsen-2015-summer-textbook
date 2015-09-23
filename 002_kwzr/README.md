# Artistic Styleで遊ぼう!

## 雑なまとめ
[A Neural Algorithm of Artistic Style](http://arxiv.org/abs/1508.06576)で遊びます. [Torchの実装](https://github.com/jcjohnson/neural-style)をDockerfile化したのでKitematicなどで[kwzr/neural-style](https://hub.docker.com/r/kwzr/neural-style)を検索してCREATEします. VOLUMEのmodelsディレクトリにVGG\_ILSVRC\_19\_layersを落としてきて入れます. imagesディレクトリにスタイル画像とコンテンツ画像を入れます. おそらくVMのメモリが足りないので頑張って4GBくらいに増やします. 
EXECボタンを押して以下のコマンドを入力します. 
```
$ bash
$ th neural_style.lua -style_image images/style.jpg -content_image images/contents.jpg -image_size 64 -gpu -1 -output_image outputs/out.png
```

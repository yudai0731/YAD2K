# forkした後にやったこと

original : https://github.com/allanzelener/YAD2K

## 実行環境
python 3.6  
tensorflow 1.6.0  
keras==2.0.3  
protobuf==3.2.0  
pydot-ng==1.0.0  
theano==0.9.0  
h5py==2.10.0
numpy  
pillow

## 1. .weights(重みのデータ), .cfg(アーキテクチャ)をダウンロードする.
2つのファイルは<a href="https://pjreddie.com/darknet/yolo/">YOLO: Real-Time Object Detection</a>のPerformance on the COCO DatasetのYOLOv2 608x608	からダウンロードできる. このファイルをforkしたディレクトリ直下に配置する.

## 2. generic_utils.pyの修正
kerasのgeneric_utils.pyがpath指定でErrorを出す問題を解決する. envs>Lib>site-packages>keras>utils>generic_utils.pyの次の部分を修正する.
```python
code = marshal.dumps(func.__code__).decode('raw_unicode_escape')
を次のように修正
code = marshal.dumps(func.__code__).replace(b'\\',b'/').decode('raw_unicode_escape')
```

## 3. モデル生成
モデルを生成する. 次のコマンドを実行する. 実行に成功するとsaveしたという趣旨のコンソールとともに, model_data/yolo.h5が生成される
```
$ python yad2k.py yolo.cfg yolo.weights model_data/yolo.h5
```

## 4. 物体検出の実行
次のコマンドを実行すると, /iamgesの画像が物体検出され, 結果がimageoutに格納される. 
```
$ python test_yolo.py -o ./imagesout model_data/yolo.h5
```
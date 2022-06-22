# gesturenet-on-tao-toolkit
gesturenet-on-tao-toolkit は、NVIDIA TAO TOOLKIT を用いて GestureNet の AIモデル最適化を行うマイクロサービスです。  

## 動作環境
- NVIDIA 
    - TAO TOOLKIT
- GestureNet
- Docker
- TensorRT Runtime

## GestureNetについて
GestureNet は、画像内の手のジェスチャーを分類し、カテゴリラベルを返すAIモデルです。  

## 動作手順

### engineファイルの生成
GestureNet のAIモデルをデバイスに最適化するため、GestureNet の .etlt ファイルを engine file に変換します。
engine fileへの変換は、Makefile に記載された以下のコマンドにより実行できます。

```
tao-convert:
	docker exec -it gesturenet-tao-toolkit tao-converter -k nvidia_tlt -p input_1,1x3x160x160,8x3x160x160,8x3x160x160 \
		-t fp16 -d 3,160,160 -e /app/src/gesturenet.engine /app/src/model.etlt
```

## 相互依存関係にあるマイクロサービス  
本マイクロサービスで最適化された GestureNet の AIモデルを Deep Stream 上で動作させる手順は、[gesturenet-on-deepstream](https://github.com/latonaio/gesturenet-on-deepstream)を参照してください。  

## engineファイルについて
engineファイルである gesturenet.engine は、[gesturenet-on-deepstream](https://github.com/latonaio/gesturenet-on-deepstream)と共通のファイルであり、本レポジトリで作成した engineファイルを、当該リポジトリで使用しています。

## 演算について
本レポジトリでは、ニューラルネットワークのモデルにおいて、エッジコンピューティング環境での演算スループット効率を高めるため、FP16(半精度浮動小数点)を使用しています。  
浮動小数点値の変更は、Makefileの以下の部分を変更し、engineファイルを生成してください。

```
tao-convert:
	docker exec -it gesturenet-tao-toolkit tao-converter -k nvidia_tlt -p input_1,1x3x160x160,8x3x160x160,8x3x160x160 \
		-t fp16 -d 3,160,160 -e /app/src/gesturenet.engine /app/src/model.etlt
```

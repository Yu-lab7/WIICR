# WIICR

このプログラムは、天気予報や気温、湿度などの情報を基に、適切な服装を推薦するシステムです。ユーザーはアンケート形式で自分の体感温度や好みを入力し、その情報を基に最適な服装を提案します。また、リスク情報やアドバイスも提供します。

## 主要技術

| 言語・フレームワーク   | バージョン |
| -------------------- | ---------- |
| processing           | 4.3        |
| python               | 3.13.0     |

## pythonの環境構築

pythonの環境構築をする方法は以下に示されています。

```
https://www.python.jp/install/windows/install.html
```

## 権利記載

#### VOICEVOX:春日部つむぎ

## 注意点

#### 現在の温湿度などからリスクを表示するモジュールは時間の関係上、夏と冬のリスクしか表示されません。ご了承ください。

## プログラムの流れ

1. **ユーザー情報入力**: ユーザーは、ユーザーはフォームにニックネーム、暑がり/寒がりの選択、情報を伝える朝と夜の時間を入力します。
2. **データ処理**: APIを叩いた結果を元に、現在地の温度、湿度、天気を表示します。また、それらの情報と入力されたデータを基にPythonスクリプトが適切な服装や、考えられるリスクを計算します。
3. **結果表示**: 計算結果を基に、適切な服装やリスク情報、アドバイスを表示します。
4. **音声再生** 指定した時間に、情報を音声で再生します。

## 設定(必ず実行してください)

#### 1.ライブラリのインストール

pythonスクリプトで使用されているライブラリをインストールしてください。

```
pip install -r requirements.txt
```

#### 2.voicevoxのダウンロード

voicevoxをダウンロードしてください。利用許諾をしっかり見てください。

```
https://voicevox.hiroshiba.jp/
```

#### 3.APIキーの設定

openweathermapのAPIキーとipgeolocationのAPIキーを以下のサイトから取得し、processingのスクリプトがある位置にAPIKEY.pdeを作成してください。

#### ipgeolocation
```
https://ipgeolocation.io/ 
```

#### openweathermap

```
https://openweathermap.org/
```

作成したAPIKEY.pdeに以下のコードを貼り付けて保存後、取得したAPIキーをWEATHER_API_KEYでは""の中に、ipgeolocationではapikey=の後に入れてください。

```
final String WEATHER_API_KEY = "";
final String ipgeolocation_API_KEY = "https://api.ipgeolocation.io/ipgeo?apiKey=";
```

## 実行方法

#### processing.exeから本プログラムを起動する方法を説明する。

#### 1.voicevoxの起動(起動状態を維持してください)

#### 2.processing.exeを起動

#### 3.このような画面が表示される(はず)

![image](https://github.com/user-attachments/assets/cf41bb83-8546-4a35-baf7-01bae90876b5)

#### 4.左上の"ファイル"をクリックし、"開く"をクリックする。その際、本プログラムが格納されているディレクトリをクリックする。

![image2](https://github.com/user-attachments/assets/76604607-e323-4d06-8381-79aa4bf02cac)

#### 5.左上の再生ボタンをクリックし、実行ボタンをクリックする。

![image3](https://github.com/user-attachments/assets/908a8258-eda1-4ea8-af49-37b4c8f39eaf)

#### [本番環境(Arduinoを接続した状態)で実行する場合] 6. WIICR.pdeのdebugModeをfalseに設定する。

![2024-12-15](https://github.com/user-attachments/assets/dcfc2615-e2d7-48b9-9d58-d0e7c9d7039b)

## ディレクトリの説明
```
.
├─.vscode
├─data       
│  ├─ad
│  ├─heavy
│  ├─humidity
│  ├─risk
│  ├─temperature
│  └─weather
└─python
    ├─csv
    ├─model
    ├─output
    └─__pycache__

```

## 各モジュールの説明(.pde)

### WIICR.pde

- **役割** メインの描画ループを管理し、各モジュールの描画を行います。また、時間に応じて適切な処理を実行します。
- **主要な役割**
  - 各モジュールの描画を管理
  - 時間に応じて処理を実行
  - ユーザー情報を入力する画面で取得した時間に応じて、音声を作成、再生するプログラムを実行

### Initialize.pde

- **役割** メインの描画ループを管理し、各モジュールの描画を行う。また、時間に応じて適切な処理を実行する。
- **主要な役割**
  - 必要な画像やデータの読み込み
  - 初期設定の実行
  - 各モジュールの初期化

### M_Data.pde

- **役割** 日付と時間の管理を行う。
- **主な役割**
  - 現在の時間を取得
  - 曜日を特定
  - 土日祝日を特定
  - 日付を描画

### M_FullImage.pde

- **役割** 背景を作成する。
- **主な役割** 
  - 指定した画像を背景にする

### M_generateVoicevox.pde

- **役割** voicevoxの音声を呼ぶ
- **主な役割**
  - voicevoxの音声を作成、再生する。

### M_LaunchingScreen.pde

- **役割** プログラムの起動時に表示されるランチャースクリーンを管理する。各種データの初期化状況を表示し、ユーザーに現在の進行状況を知らせる。
- **主な役割**
  - 各種データの初期化状況を表示
  - 初期化が完了した項目を緑色で表示し、未完了の項目を黒色で表示

### M_Location.pde

- **役割** 現在地を描画
- **主な役割**
  - 現在地を描画

### M_pageControl.pde

- **役割**  ページコントロールモジュールを管理する。複数のページがある場合に、現在のページを示すインジケーターを画面上に表示します。
- **主な役割**
  - 現在のページを示すインジゲーターを描画
  - ページごとに異なる色のインジゲーターを描画

### M_ProgressBar.pde

- **役割** 現在の進行状況を示すプログレスバーを描画する。
- **主な役割** 
   - プログレスバーの進行率を計算し、画面上に表示

### M_Survey.pde

- **役割**  form.pyを実行してデータを取得します。ユーザーが入力した情報を基に、適切な変数にデータを設定します。
- **主な役割** 
   - form.pyを実行
   - 取得したデータのスケーリング

### M_tomorrowWeather.pde

- **役割** 明日の天気を取得します。
- **主な役割** 明日の天気を取得

### M_tts.pde

- **役割** ttsAbout.pyまたはttsAbout2.pyを実行し、音声を作成、再生する。
- **主な役割** 
   - pythonスクリプトを実行し、テキストを音声に変換
   - pythonスクリプトの出力を処理

### RM_Humidity.pde

- **役割** 部屋の湿度を描画
- **主な役割**
   - Arduinoから取得した湿度を描画

### RM_recommendClothes.pde

- **役割** 推薦された服装を表示するモジュールを管理する。AIの処理を取得し、画面に表示する。
- **主な役割** 
   - suggestClothes.pyを実行し、AIの処理を取得
   - 取得した情報を画面に表示

### RM_recommendHeavyOutfit.pde

- **役割** 推薦されたグッズを表示するモジュールを管理する。AIの処理を取得し、画面に表示する。
- **主な役割**
   - suggestClothes2.pyを実行し、AIの処理を取得
   - 取得した情報を画面に表示

### RM_riskShow.pde

- **役割** 室内外の温度差から想定されるリスクを表示する。
- **主な役割**
   - リスク情報の計算
   - リスク情報とアドバイスの表示
   - リスク情報に基づいた画像の表示

### RM_rightWrite.pde

- **役割** 権利関係の記載
- **主な役割**
   - 権利関係の記載

### RM_Temperature.pde

- **役割** 部屋の温度を描画
- **主な役割**
   - Arduinoから取得した温度を描画

### RM_Weather.pde

- **役割**　天気情報を表示するモジュールを管理する。現在の天気、気温、湿度などの情報を取得し、画面上に表示する
- **主な役割**
   - 天気情報の取得
   - 天気情報の表示
   - 天気情報に基づいたアイコンの表示

## 各モジュールの説明(.py)

### form.py

- **役割** 初回起動時に、ユーザーからの入力を受け付けるフォームを表示する。取得したデータを変数に代入し、出力する。
- **主な役割**
  - ニックネーム、暑がり/寒がりの選択、朝と夜の時間を入力するフィールドを提供
  - 入力内容の検証
  - 入力拒否ボタンを押すと全てのプログラムを強制終了

### generateHeavyOutfit.py

- **役割**　グッズの推薦を行うためのデータを生成し、csvファイルに保存する。気温やユーザーの体感温度に基づいて適切なグッズを決定する。
- **主な役割**
  - AI作成のためのデータを作成
  - 作成したデータは、csvディレクトリにheavyOutfit_recommendations.csvに保存

### generateOutfit.py

- **役割**　服装の推薦を行うためのデータを生成し、csvファイルに保存する。気温やユーザーの体感温度に基づいて適切な服装を決定する。
- **主な役割**
  - AI作成のためのデータを作成
  - 作成したデータは、csvディレクトリにoutfit_recommendations.csvに保存

### makeModel.py

- **役割** 　服装を推薦するモデルを作成する。csvファイルからデータを読み込み、ランダムフォレストでトレーニング、モデルを保存する。
- **主な役割**
  - outfit_recommendations.csvからデータを読み込み
  - 特徴量とターゲットの設定
  - データの前処理
  - データのトレーニングセットとテストセットへの分割
  - ランダムフォレストでモデルの作成
  - トレーニングされたモデルをmodelディレクトリにoutfit_model.pklとして保存

### makeModel2.py

- **役割** 　グッズを推薦するモデルを作成する。csvファイルからデータを読み込み、ランダムフォレストでトレーニング、モデルを保存する。
- **主な役割**
  - outfit_recommendations.csvからデータを読み込み
  - 特徴量とターゲットの設定
  - データの前処理
  - データのトレーニングセットとテストセットへの分割
  - ランダムフォレストでモデルの作成
  - トレーニングされたモデルをmodelディレクトリにoutfit_model2.pklとして保存

### openWav.py

- **役割** wavを自動で再生する。
- **主な役割**
  - 指定したwavファイルを自動で再生
  
### suggestClothes.py

- **役割** 事前にトレーニングされたモデルを使用して、入力されたデータに基づいて最適な服装を推薦する。
- **主な役割**
  - トレーニングされたモデルを読み込み
  - 入力されたデータに基づいて服装を推薦

### suggestClothes2.py

- **役割** 事前にトレーニングされたモデルを使用して、入力されたデータに基づいて最適なグッズを推薦する。
- **主な役割**
  - トレーニングされたモデルを読み込み
  - 入力されたデータに基づいてグッズを推薦

### ttsAbout.py

- **役割**  天気情報やリスク情報、アドバイスなどをテキストから音声に変換し、音声ファイルとして保存する。
- **主な役割** 
  - テキストから音声合成のためのクエリを作成
  - クエリを元に音声データを生成
  - 生成された音声データをファイルとして保存

### ttsAbout2.py

- **主な役割** 天気情報やリスク情報、アドバイスなどをテキストから音声に変換し、音声ファイルとして保存する。ttsAbout.pyと似ているが、追加の服装情報であるoutfit2を含むテキストを生成する。
- **主な役割** 
  - テキストから音声合成のためのクエリを作成
  - クエリを元に音声データを生成
  - 生成された音声データをファイルとして保存


## 作成するにあたって参考にしたサイト

```
1. LIXIL,室温が体に影響を与える『ヒートショック』とは？症状や対策、なりやすい人,
https://www.lixil.co.jp/reform/gensai/column/column_vol05/#:~:text=%E3%83%92%E3%83%BC%E3%83%88%E3%82%B7%E3%83%A7%E3%83%83%E3%82%AF%E3%81%AF10%E5%BA%A6,%E6%84%9F%E3%81%98%E3%82%8B%E3%81%93%E3%81%A8%E3%81%AF%E3%81%82%E3%82%8A%E3%81%BE%E3%81%9B%E3%82%93%E3%81%8B%EF%BC%9F,(閲覧日 2024/12/12)

2. NHK,寒い時期 “ヒートショック”に注意！ 具体的な対策は,
https://www3.nhk.or.jp/sapporo-news/20241203/7000071751.html,(閲覧日 2024/12/12)

3. WeatherNews,服装と気温の関係,
春ファッションを楽しむために知っておきたいこと,
https://weathernews.jp/s/topics/202103/310055/,(閲覧日 2024/12/13)

4. 川原浩揮,「暑がりさん」と「寒がりさん」服装の目安ご紹介！ 寒暖差激しい“ジグザグ”な一週間に注意　真夏日→気温急落→夏日でも朝晩ヒンヤリ,FNNプライムオンライン,
https://www.fnn.jp/articles/-/769161?display=full,(閲覧日 2024/12/13)

5. mitsumizo(三溝 英司),ProcessBuilderクラスの使い方,
https://qiita.com/mitsumizo/items/836ce2e00e91c33fcf95(閲覧日 2024/12/13)

6. パナソニック株式会社 コミュニケーションデザインセンター,9割が家庭で「体感温度の違いを感じたことがある」パナソニック エアーマイスターが教える、暑がり・寒がりが同室で過ごすコツ 〜暑がり・寒がりにそれぞれおすすめのエアコン最新機能は？〜,
https://panasonic.jp/aircon/air_letter/news/sensible_temperature.html,(閲覧日 2024/12/15)

7. にいがたマイホーム応援マガジン,室内の快適な温度とは？WHOは18℃以上を強く勧告！,https://www.toskenhome.com/blog/who18/#toc3 (閲覧日 2024/12/17)
```






# 自動車シミュレータの実行
![picture1](https://github.com/utagoeinc/Carsimulator/blob/master/1.PNG)
![picture2](https://github.com/utagoeinc/Carsimulator/blob/master/2.PNG)
![picture3](https://github.com/utagoeinc/Carsimulator/blob/master/3.PNG)
自動車シミュレータの[AirSim](https://microsoft.github.io/AirSim/)を自作したMAPの上で実行しました。

1. AirSimについて
2. 実行環境
3. 環境構築
   - VS2017とUnreal Engineのインストール
   - Download source code
   - AirSimのビルド
   - プラグインのインストール
4. 付録


## 1.AirSimについて

AirSimはMicrosoftが開発しているオープンソースの車とドローンのシミュレーターです。Unreal Engine上で動き、LinuxとWindowsで実行できます。APIを利用してセンサーのデータを取得したり、車やドローンの操作ができます。センサーはカメラやGPS,Magnetometre、Lidarなど豊富にあり、学習データを簡単にとることができます。

## 2.実行環境

- Windows 10 Home 1709
- Visual Studio 2017 Community
- Unreal Engine 4.20.3

## 3.環境構築

### 3.1 Install Visual Studio 2017 and Unreal Engine

AirSimはバイナリ版が公開されていますが、マップがすでに用意されているものしか使えません。自作のマップの上で動かすにはUnreal EngineのAirSim Pluginのビルドをする必要があります。Unreal EngineのマーケットプレイスからアセットをダウンロードするのはWindowsでしかできないのでこの環境になりました。さらにビルドスクリプトの内部でPowerShell 5.0の機能を使っています。なのでWindows10を使うかPowerShellを5.0にアップデートしてください。またVisual Studioは2019が最新版ですが2017でしかビルドできないので気をつけてください。またAirSimの公式ドキュメントにはUnreal Engine version 18が指定されていますが、アセットの整合性の関係でversion 20で使っていますが、現段階ではちゃんと動いています。

- Use Windows 10 or PowerShell 5.0
- Use VS2017 only

### 3.2 Download source code

AirSimにはバージョンが1.2.1,1.2.0等がありますがVC++の内部でエラーが発生しビルドすることができませんでした。git等でmasterをダウンロードしてください。


### 3.3 Build AirSim

ここでビルドの前に行うことが２つあります。(On 09 May 2019)
１．"Half.h"のエンコードを使って"utf-8" から "utf-8(with BOM)"へ変更してください。僕はVScodeを使いました。ファイルは"airsim-master\airlib\deps\eigen3\eigen\src\core\arch\cuda"にあります。
２．VS2017で"Airsim.sln"を開いて、ソリューションエクスプローラーを右クリックしてプロジェクトの再ターゲットを選んでください。v141へアップグレードを選んで再ターゲットします。

3. "build.cmd"を実行してください。

### 3.4 Install AirSim Plugin

以下は[公式のドキュメント](https://microsoft.github.io/AirSim/docs/unreal_custenv/)と同じです。

1. AirSimを使いたいプロジェクトをUnreal Engineで開いて、File-New c++ class より空のc++ class を作ってください。親のクラスはなし、ファイル名は適当で大丈夫です。
2. Unreal Engineを閉じて、Unreal Engineのプロジェクトフォルダにある"Project.uproject"をeditorで開いて   次のコードのように追加してください。

```json:project.uproject

{
    "FileVersion": 3,
    "EngineAssociation": "4.18",
    "Category": "Samples",
    "Description": "",
    "Modules": [
        {
            "Name": "LandscapeMountains",
            "Type": "Runtime",
            "LoadingPhase": "Default",
            "AdditionalDependencies": [
                "AirSim"
            ]
        }
    ],
    "TargetPlatforms": [
        "MacNoEditor",
        "WindowsNoEditor"
    ],
    "Plugins": [
        {
            "Name": "AirSim",
            "Enabled": true
        }
    ]
}

```

3. Unreal Engineを開いてWorldsetting -> GameMode OvverrideにてAirSimGameModeを選びます。
4. Play AirSim！


### 4. 付録

1. 今回はWindowsを使いましたが、Linux（Ubuntu18）でビルドしたところ公式のドキュメント通りでうまくいきました。もしマーケットプレイスを使わないならそちらでビルドすることをお勧めします。

2. Windowsでビルドする際にDebugとReleaseの整合性が合わないとエラーが出た場合は"build.cmd --Release"でうまくいきます。
3. user\Docments\Airsim\settings.jsonを編集することで車の出現位置、マルチエージェント、カメラの追加等の様々なことができます。車にカメラを追加したとき、UE4がクラッシュしました。これはカメラの(x,y,z)のみを指定すると(pitch,yaw,roll)に値が入らないためで、すべて０に指定してあげると解消しました。

### Reference 
1. [Airsim](https://github.com/Microsoft/AirSim)
2. [AirSimをUnreal Engine, Visual Studio 2017でビルドする(Windows編)](https://qiita.com/thrzn41/items/32171647c15c8da37f31)
3. [Microsoft PowerShell Document](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.archive/Expand-Archive?view=powershell-5.0)

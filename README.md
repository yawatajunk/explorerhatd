# NAME
Explorerhatd

# Overview
ラズベリーパイ用Explorer Hatをコマンドラインからコントロールするコマンド群です。

# Description
Explorer Hatは、5V入力が可能な入力ポートx4、出力ポートx4、タッチパッドx8、LEDx4及び小型ブレッドボードが備わったラズパイ用Hatです。
Python用モジュールをpipでインストールすれば、簡単にコントロールできます。
```
$ sudo pip install explorerhat    # python2用
$ sudo pip3 install explorerhat   # python3用
```

1例として、次のPythonスクリプト`test.py`を実行して青LEDを点灯させてみましょう。
```test.py
import explorerhat

explorerhat.light.blue.on()
```

青色のLEDが一瞬だけ点灯し、すぐに消えてしまいました。そのとき、ターミナルに表示された内容は次のとおりです。

```
$ python3 test.py
Explorer HAT Basic detected...

Explorer HAT exiting cleanly, please wait...
Stopping flashy things...
Stopping user tasks...
Cleaning up...
Goodbye!
```

Pythonスクリプトが終了するとき、"Cleaning up..."、つまり初期化されて、その結果、点いた青LEDがあっという間に消えてしまったようです。
よって、LEDを点灯させ続けたいときには、Pythonスクリプトを持続させなければなりません。ちょっと使いにくい。  
そこで、Explorer Hatをコントロールするバックグラウンドプログラムをラズパイに常駐させることにしました。もちろん、デーモン化してもOKです。
また、そのプログラム経由でHatのI/Oをコントロールするコマンド群を作成しました。

# Requirement
* Raspberry Pi
* Explorer Hat


# Install
## Explorer Hat用Pyhonモジュール
Python2とPython3用をインストールします。

```
$ pip install explorerhat
$ pip3 install explorerhat
```

## explorerhatd (本リポジトリ)
インストールするディレクトリを作ります。例えば`~/tools/`。
そこに、本リポジトリをクローンします。

```
$ mkdir ~/tools
$ cd ~/tools
$ git clone https://github.com/yawatajunk/explorerhatd.git
$ git checkout 0.1a
```

# Contents
次のファイルがインストールされます。
* explorerhatd: ラズパイに常駐させるExplorer Hatを制御するPythonスクリプト
* ehlight: LEDを制御するコマンド
* ehtouch: タッチパッドの状態を取得するコマンド
* ehinput: 入力ポートの状態を取得するコマンド
* ehoutput: 出力ポートを制御するコマンド
* LICENCE: ライセンス
* README.md: 本ドキュメント

# Usage
## explorerhatd
次のコマンドでバックグラウンドで実行します。
```
$ ~/tools/explorerhatd/explorerhatd &
```
または、ラズパイを起動したときに、自動的に起動させてデーモン化するのも良いでしょう。

## 各コマンドの使用例
`~/tools/explorerhatd`にパスが通っていることを前提に各コマンドの使用例を次に示します。

### LED
```
$ ehlight --green 1 --blue 1 --red 1 --yellow 1   # 全LED点灯
$ ehlight -g 0 -b 0 -r 0 -y 0                     # 全LED消灯
```

### タッチパッド
```
$ ehtouch -ch 0   # '-ch 0'で全チャンネルのタッチ状態を取得。
                  # 1&3にタッチなら、'00000101'(2進数)より、5(10進数)が得られる。
$ ehtouch -ch 1   # 特定のチャンネルの状態を取得するには1〜8を設定。
$ ehtouch         # "ehtouch -ch 0"と同一。全チャンネルの状態を取得。
```

### 入力ポート
```
$ ehinput    # 全入力ポートの状態を取得する。
             #    例: port1 = 0, port2 = 0, port3 = 1, port4 = 1のとき
             #       '1100'(2進数)より、12(10進数)が得られる。
```

### 出力ポート
```
$ ehoutput --port1 1  # 出力ポート1: high
$ ehoutput -p1 0      # 出力ポート1: low
```

# History
0.1a: 初版

# Reference
[Raspberry Pi](https://www.raspberrypi.org)  
[Explorer Hat](https://github.com/pimoroni/explorer-hat)

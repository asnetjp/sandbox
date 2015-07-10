#ADBコマンド使いこなしてますか？？
最近はAndroidのTVアプリを開発を行っている、開発部の梶原です。  
今回はAndroidのADBコマンドについて簡単にまとめていきたいと思います。  
どうでしょうかみなさんしっかりと使いこなしています？  
adb install以外にも開発時に使えるコマンドがたくさんそろっていますので参考にしてくださいね。  

##接続している端末の確認
```
dkajiwara:app dkajiwara$ adb devices
List of devices attached
192.168.56.101:5555	device
YT910ZK7PB	device
```

##特定の端末に接続する
特定の端末に接続するときには
```
$ adb -s YT910ZK7PB
```

##APKのインストール・アップデート・アンインストール
これはもう言わずもがなですね
###インストール
```
$ adb install MyApp.apk
```
###アップデート
```
$ adb install -r MyApp.apk
```
-rを付けることで、データは消されずに上書きインストール出来ます。

###アンインストール
```
$ adb uninstall com.sample.myapp
```
com.sample.myappには特定のパッケージ名を入れてください
##データの削除
```
$ adb shell pm clear com.sample.myapp
```
##ログの出力
```
$ adb shell logcat
```
logcatにはいくつかオプションを付けることが可能です

|オプション|説明|
|:---|:---|
|-v time|出力形式を指定してログ出力|
|-c |ログクリア|

あと、標準出力に出力されるので下記のようにリダイレクト
```
$ adb shell logcat -v time > ~/Desktop/test.log
```
##ADBの起動・停止
ADBが何故か繋がらなくなる時によくやるやつですね  
```
# 起動
$ adb start-server
# 停止
$ adb kill-server
```
##Dump系いろいろ
ほげほげほげほげ
##キーイベント
ほげぇほげぇほげぇ
##Contentproviderにアクセス
ほげぇほげぇ
##パッケージ列挙
ほげぇ
##おまけ
###adb shellで入った後の処理をバッチで行う
最近たまにbatを書いたりすることがあって、その時によくやる方法を載せておきます。 shellの中で行う操作を記述したファイルと、実行用のbatファイルを用意します。
下記は自分が開発で使っているログ出力するバッチファイルです
```hoge.bat
adb shell < test.sh
```
```test.sh
am ....
am ....
am ....
```

いかがでしたでしょうか？意外と

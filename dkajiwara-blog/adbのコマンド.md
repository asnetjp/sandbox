#開発中によく使用するadbコマンド
開発部の梶原です。  
adbコマンドを使いこなしていますか？  

SDKの中にはadb install以外にも開発時に使えるコマンドがたくさんそろっていますので参考にしてみてください。  


##adbのPathを通す
まずは、ADBを使うためにパスを通しましょう。
###Mac
下記のように、.bash_profileを編集してパスを通してください。
```
$ vi .bash_profile
#ANDROIDのパス
ANDROID_HOME=/Users/dkajiwara/android-sdk-macosx
export PATH=$PATH:${ANDROID_HOME}/platform-tools

$ source ~/.bashrc
$ adb version
Android Debug Bridge version 1.0.32
```

###Windows
windowsを使用している方は下記等を参考にしてパスを通してください。  
参考：Windows 8.1でadbコマンドを使えるようにする簡単な方法を紹介
http://gadget-drawer.net/2014/05/windows-8-1-adb-command/


##接続している端末を確認する
```
$ adb devices
List of devices attached
192.168.56.101:5555	device
YT910ZK7PB	device
```

##特定の端末に接続する
-sオプションで一覧で確認した端末にアクセス出来ます。
```
$ adb -s YT910ZK7PB
```

##APKのインストール・アップデート・アンインストール
これはもう言わずもがなですが、端末にアプリをインストールするときに使用します。
###インストール
```
$ adb install MyApp.apk
```
###アップデート
```
$ adb install -r MyApp.apk
```
-rオプションを付けることで、データは消されずに上書きインストール出来ます。

###アンインストール
アンインストールする際にはアプリのパッケージ名を指定します。
```
$ adb uninstall com.sample.myapp
```
##アプリのデータを削除する
```
$ adb shell pm clear com.sample.myapp
```
##ログの出力
```
$ adb shell logcat
# 日付も表示
$ adb shell logcat -v time
# ログのクリア
$ adb logcat -c
```
下記のようにリダイレクトして適当なところに出力することが出来ます。
```
$ adb shell logcat -v time > ~/Desktop/test.log
```
##ADBの起動・停止する
adbがつながらなくなったり、端末を認識しなくなった場合に、一度adbを再起動させることで復帰出来る場合があります。
```
# 起動
$ adb start-server
# 停止
$ adb kill-server
```
##Intentを投げる
```
# 明示的Intentを投げる
$ adb shell am start -n com.sample.myapp/.MyAppActivity
# 暗黙的Intentを投げる
$ adb shell am start -a android.intent.action.VIEW -d http://xxxx.xxxx

# サービスの起動 (Intentの指定方法はActivityと同じ)
$ adb shell am startservice ...
# ブロードキャストの送信 (Intentの指定方法はActivityと同じ)
$ adb shell am broadcast ...
```
明示的Intentを投げるときは*-n アプリのパッケージ名/Activity*
暗黙的Intentを投げるときは*-a Action名*

##インストール済みのapkのversionを確認する
```
$ adb shell dumpsys package com.android.email
~省略~
Packages:
  Package [com.android.email] (42721300):
    versionCode=600010 targetSdk=19
    versionName=6.1
    applicationInfo=ApplicationInfo{4261a488 com.android.email}

```
##View階層の確認する
```shell
$ adb shell dumpsys activity top
TASK com.google.android.gm id=62
  ACTIVITY com.google.android.gm/.ComposeActivityGmail 42011f98 pid=4333
    Local FragmentActivity 42f3d0e0 State:
      mCreated=truemResumed=true mStopped=false mReallyStopped=false
      mLoadersStarted=true
    FragmentManager misc state:
      mActivity=com.google.android.gm.ComposeActivityGmail@42f3d0e0
      mContainer=android.support.v4.app.n@436f5cc8
      mCurState=5 mStateSaved=false mDestroyed=false
    View Hierarchy:
      com.android.internal.policy.impl.PhoneWindow$DecorView{42efe2f0 V.E..... ... 0,0-720,1184}
        android.widget.LinearLayout{42f16570 V.E..... ... 0,0-720,1184}
          android.view.ViewStub{42ff0e70 G.E..... ... 0,0-0,0 #1020328}
          android.widget.FrameLayout{4357c578 V.E..... ... 0,50-720,1184}
            android.support.v7.internal.widget.ActionBarOverlayLayout{43304068 V.ED.... ... 0,0-720,1134 #7f0f009b app:id/decor_content_parent}
```
##AlarmManagerでセットしたタイマーを確認する
```shell
$ adb shell dumpsys alarm
RTC_WAKEUP #6: Alarm{44cecb00 type 0 com.google.android.gms}
  type=0 whenElapsed=2667655216 when=+29d22h22m58s745ms window=0 repeatInterval=0 count=0
  operation=PendingIntent{42025980: PendingIntentRecord{4319de80 com.google.android.gms broadcastIntent}}
```
##ContentProviderにアクセス
※Android4.1.1以上で使用可能です
```
$ adb shell content query --uri content://media/external/images/media
Row: 0 _id=1592, _data=/storage/emulated/0/DCIM/100ANDRO/DSC_0001.JPG, _size=4072815, _display_name=DSC_0001.JPG, mime_type=image/jpeg, title=DSC_0001, date_added=1433051343, date_modified=1433051343, description=NULL, picasa_id=NULL, isprivate=NULL, latitude=35.71935, longitude=139.77538, datetaken=-1467734462, orientation=90, mini_thumb_magic=-1370636478, bucket_id=-413971691, bucket_display_name=100ANDRO, width=3840, height=2160
```
##インストールされているパッケージの一覧を表示
```
$ adb shell pm list package
```

いかがだったでしょうか、今回はAndroidのADBコマンドについてよく使うものをまとめてみました。
今回紹介したもの以外にもまだあるので使って試してみてください。

##参考URL
Android開発コマンド  
https://sites.google.com/site/memo73737/android-dev/android-dev-command

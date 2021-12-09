# mydnsddnsclient
取得したドメインをMyDNSのダイナミックDNS(DDNS)に登録して利用するためのクライアントスクリプトです。
Linuxで動作します。
グローバルIPアドレスが固定されていない自宅サーバでの利用を想定しています。

## 特徴
* Linuxで動作します。(Python3スクリプトです）
* IPv4のみに対応しています。
* 【公式手順と比較してのポイント】エラー発生時やIPアドレス更新時にメール通知することができます。
* 複数アカウントに対応しています。
* 定期強制更新が可能です。
* 【公式手順と比較してのポイント】現在のグローバルIPアドレスが前回実行と異なるときに、DNSレコードを現在のグローバルIPアドレスに更新します。このことによりサーバ上で高頻度で実行しても、DDNSサービスには負担をかけません。(グローバルIPを調べるサービスには負担をかけるのでご注意ください)

## システム要件
* python3以上
* requestsモジュール

## インストール方法
1. requestsモジュールをインストールします。
```
# pip install requests
```
2. スクリプトを任意の場所に配置します。
```
# cp mydnsddnsclient.py /usr/local/mydnsddnsclient/bin/mydnsddnsclient.py
```
3. パーミッションを700に設定します(認証情報を記載するため）
```
# chmod 700 /usr/local/mydnsddnsclient/bin/mydnsddnsclient.py
```
4. mydnsddnsclient.pyにMyDNSの認証情報の設定を記載します。任意でメール通知のためのメールサーバとその認証情報を記載します。
```
# vi /usr/local/mydnsddnsclient/bin/mydnsddnsclient.py
```
```
# MyDNSのユーザ名、パスワード
# -書式-
#   USER:PASSWORD,USER:PASSWORD,....
# -例-
#   dns000001:PaSsW0rd
#   dns000001:PaSsW0rd,dns000002:PaSsW0rd,dns000003:PaSsW0rd
USER_PASSWORD_LIST="dns000001:PaSsW0rd"

# 必要なときのみ実行する(False/True)
#  有効にするとIPアドレスに変化があったときのみ、アップデートを実行します。
#  ただしIPアドレスに継続して変化が無いときはDAYS_FORCE_UPDATEで指定する日数後に
#  強制アップデートを実行します。
#  IPアドレスチェックで警告になる場合は無効にしてください。
ENABLE_EXEC_ONLY_WHEN_NEEDED=True
# 強制的にDDNSを更新する日数間隔
DAYS_FORCE_UPDATE=1

#--[簡易実装]メール通知----------
#  上記ENABLE_EXEC_ONLY_WHEN_NEEDED=Falseの場合は失敗時のみメール送信
# メールサーバホスト名（空値の場合、メール通知は無効）
MAIL_HOST="192.168.1.10"
# メールサーバポート番号
MAIL_PORT="25"
# メールサーバユーザ名（空値の場合、認証実施しない）
MAIL_USER=""
MAIL_PASSWORD=""
# 通知メールFromメールアドレス
MAIL_FROM="test@testtest.com"
# 通知メールToメールアドレス
MAIL_TO="test@testtest.com"
# 通知メール件名
MAIL_SUBJECT="MyDNS DDNS Client(Non-Formula)"

```
5.crontabに登録します。
```
# cat <<EOF > /etc/cron.d/mydnsddnsclient
0,15,30,45 * * * * root /usr/local/mydnsddnsclient/bin/mydnsddnsclient.py
EOF
```

## 使い方
* 上記通り、cronに登録して実行します。
* ログは/var/log/mydnsddnsclient.logに出力します。(何かあった時しか出力しません)
* IPアドレスの記録と前回更新日時は/var/lib/mydnsddnsclient.txtに保存しています。
* LOG_VERBOSE=Trueにすると冗長ログを出力します。（実行ごとの開始終了を出力します）

## 注意
* 非公式かつ無償ですので自己責任でご使用ください。いかなる損害が発生しても責任は負いません。
* 必要に応じてログはログローテーションしてください。
* 利用者は日本人しか想定していません。（なので日本語で書いています）
* ソースはcoolな書き方をしていませんので参考になりません。

## Autor
* 作成者：ちぢぴー(chidipy)
* E-mail：webmaster@chidipy.jpn.com

## ライセンス
MITライセンスです。


# BC2ODPT-BUS
BC2ODPT: BlockChain-based Certification of Open Data for Public Transportation
BC2ODPTはブロックチェーン(BC)に公共交通オープンデータ(ODPT)の リアルタイムAPIから得られるデータを保存していくことで，その存在を証明するアプリケーションです。

# アプリケーションの説明
私達はこのアプリを実現する為に必要なBCを管理するコアノードと，APIにアクセスしてデータを取得した後にコアノードに転送するクライアントノードの２つを作成しました。

BCを用いることで複数のコアノードがネットワークを介して通信を行い, ODPTをBCに保存します. 
BCの改ざん耐性を利用することで, リアルタイムAPIでは時とともに流れて消えていた動的データをBC上に永続保存します.
これを過去の交通状況の記録として再利用します.

このアプリの応用例として遅延証明があります.
電車やバスの運行状況や遅延情報をBCに記録することで, 過去の遅延情報が確認できます. これとSuica等のICカード乗車券の情報を組み合わせることで, 過去に遅延した電車やバスに乗っていたことを証明できます. 
　現在は駅毎に紙媒体で配布している遅延証明書を電子化するだけでなく, 配布する必要もなくなります.BCとODPTを利用して, 遅延証明が欲しい利用者自身が自分で電子データを揃えて必要な時に必要な人に提出する方式を提案します.
公共交通オープンデータは公共性が高い為, 限られた公共交通機関がBCを管理するよりも, 不特定多数のコアノードがBCを管理する方が信頼度が向上します. 
　従って, コアノードを管理するインセンティブが不可欠です. BC上のデータから貢献度を計算してスコア化し, それに応じて公共交通機関が独自に発行するポイントを付与することができれば, インセンティブとして機能すると考えます.

BC2ODPTはBC(BlockChain)とODPTを組み合わせることで便利な公共交通を実現する第一歩を初めて踏み出した作品です。

アイデアについてのドキュメント
https://docs.google.com/document/d/1MoxBF5Mhwtya_5IbWbNOSFZIeyDcnu13YGX97S03bW8/edit?usp=sharing

# 実行可能環境
Python 3.6以上
必要モジュール: requirements.txt に記載
実行方法
各ディレクトリ説明
中身コアノードとしての役割をServerNodeA 新規参加コアノードとしてServerNodeB, APIデータを取得し送信する役割としてClientNodeA,Bという３つのnodeをディレクトリごとに設定しています。
ノードを増やしたい場合, ディレクトリごとコピーし, 各Node*/server2.py・client*/client_*.pyにて,接続先IP・PORTの設定をすることでノードを増やすことが可能です。

各種nodeの動き
ServerNodeA/server1
中心コアノードとしての役割。

ClientNodeA/client_...py
serverに対してデータを送信

ServerNodeB/server2
新規参加nodeとしての役割。

簡単な実行
nodeA/server1.pyを実行することで、中身コアノードが立ち上がります。このとき、ServerCore({PORT})内にて起動ホストPORTを設定します。

clientA/client_while.pyを実行することで、テストデータを起動し続けるかぎり接続先サーバーに送信し続けます。接続先の設定をClientCore({ホストport}, '{接続先IP}', {接続先PORT})にて行います。

ローカルで実行する場合
中心コアノードの接続先IPが分からない場合, ServerNodeA/server1.pyを実行時に
2行目にServer IP address is set to ... {実行プログラムのIP} と, 起動したサーバーのIPが表示されるため,
表示された入力することで接続をすることができます。

新規接続のサーバーNodeBを起動する場合, ServerNodeB/server2.pyを実行します.
こちらの接続設定も, clientと同様に、ServerCore({ホストport}, '{接続先IP}', {接続先port})と設定を行います.
その後,ServerNodeとの同期が開始されます. DB2Memory Valid Check OK !!!!というメッセージが出力されば, ブロックチェーンの同期が完了します.
もし, ServerNodeBディレクトリ内のdb/ldb/に同期されたはずのデータが無い場合, nodeBを再起動することで正常に動作する場合があります.

# パラメータ設定
# 各nodeA, nodeB, clientA ディレクトリ内のsetting.pyにて各種設定が可能。

# DEBUG: True時、printによる処理を実行する
# DIFFICULTY: ブロックの生成難易度（高いほど生成に時間がかかる4~6が適正か）
# API_KEY: テスト用のものを用意しているが、原則変えるのを推奨
# HOST_PORT: 起動PORT
# CONNECT_IP: 接続先IP
# CONNECT_PORT: 接続先PORT

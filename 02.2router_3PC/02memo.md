
# 目標

PC2台、ルーター２台を作成する


スタティックルーティングの設定を行う


# 作業メモ

## 1. 配線

配線箇所（仮）|接続元機器|接続元I/P|接続先機器|接続先I/P
----------|--------|--------|-------------------------------|---------
Router0-Router1|Router0|GigabitEthernet0/1|Router1|GigabitEthernet0/1
PC0-Router0|Router0|GigabitEthernet0/0|PC0|FastEthernet0|
PC1-Router1|Router1|GigabitEthernet0/0|PC1|FastEthernet0|
PC2-Router1|Router1|GigabitEthernet0/2|PC2|FastEthernet0|

※I/P インターフェイス＆ポート
## 1.5 IPアドレスまとめ


## 2.Router0側の設定

以下コマンドを実行

```bash

// Gigaにipアドレス割当

// スタティックルートを作成

```

## 3.Router1側の設定

以下コマンドを実行

```bash
// Gigaにipアドレス割当

// スタティックルートを作成

```

## 4.PC0側の設定

以下設定を行う。


## 5.PC1側の設定

以下設定を行う。




# 確認作業

## 1.PC0から確認

## 2.PC1から確認

# 参考
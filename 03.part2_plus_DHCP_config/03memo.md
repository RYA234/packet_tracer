
# 目標

Router0 Router1のDHCPサービスを有効化します。
クライアントのPCにIPアドレス　サブネットマスク　デフォルトゲートウェイを割り当てます。



# 作業メモ

## 1. 前の引き継ぎ
03.2router_3Pcのpktファイルをコピペします。


## 2.Router0側の設定

以下コマンドを実行

```bash
Router0>en
Router0>config t

# dhcpプールを作成
Router0(config)>ip dhcp pool LanUse0
# ネットワークアドレスの設定
Router0(dhcp-config)>network 192.168.1.0 255.255.255.0 

# デフォルトゲートウェイの設定
Router0(dhcp-config)>default-router 192.168.1.254
Router0(dhcp-config)>exit
Router0(config)>exit

# DHCPサーバーの設定を確認
Router0>show running-config | begin dhcp


```


## 3.Router1側の設定

以下コマンドを実行

```bash
Router1>en
Router1>config t

# dhcpプールを作成
Router1(config)>ip dhcp pool LanUser1
# ネットワークアドレスの設定
Router1(dhcp-config)>network 192.166.1.0 255.255.255.0 

# デフォルトゲートウェイの設定
Router1(dhcp-config)>default-router 192.166.1.254

# リース期間を設定　12時間ごと
Router1(dhcp-config)>lease 0 12
Router1(dhcp-config)>exit
Router1(config)>exit

# DHCPサーバーの設定を確認
Router1>show running-config | begin dhcp


```

## 4.PC0側の設定

以下GUIで設定をする。
IPアドレス設定
DHCPに設定する


## 5.PC1側の設定
以下GUIで設定をする。
IPアドレス設定
DHCPに設定する


# 確認作業

## 1.PC0から確認

PC1につながるか確認


```bash
C:\>tracert 192.166.1.2

Tracing route to 192.166.1.2 over a maximum of 30 hops: 

  1   0 ms      0 ms      0 ms      192.168.1.254   #Router0 Giga0/0
  2   0 ms      0 ms      0 ms      192.170.1.250   #Router1 Giga0/1
  3   0 ms      0 ms      0 ms      192.166.1.2     #PC1

Trace complete.

```

## 2.PC1から確認

PC0につながるか確認

```bash
C:\>tracert 192.168.1.1

Tracing route to 192.168.1.1 over a maximum of 30 hops: 

  1   0 ms      0 ms      0 ms      192.166.1.254   #Router1 Giga0/0
  2   0 ms      0 ms      1 ms      192.170.1.254   #Router0 Giga0/1
  3   0 ms      0 ms      0 ms      192.168.1.1     #PC0

Trace complete.


```

# 参考
徹底攻略Cisco CCENT/CCNA Routing & Switching教科書ICND1編［100-105J］［200-125J］V3.0対応 徹底攻略シリーズ

pp465-470　DHCPサーバについて
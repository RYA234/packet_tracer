
# 目標

PC2台、ルーター２台を作成する


スタティックルーティングの設定を行う

DHCP無し


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


Router0>en
Router0>config t

// Giga0/0にipアドレス割当
Router0(config)>interface GigabitEthernet0/0

Router0(config-if)>ip address 192.168.1.254 255.255.255.0

Router0(config-if)>no shutdown

Router0(config-if)>exit

// Giga0/1にipアドレス割当
Router0(config)>interface GigabitEthernet0/1

Router0(config-if)>ip address 192.170.1.254 255.255.255.0

Router0(config-if)>no shutdown

Router0(config-if)>exit


// スタティックルートを作成

Router0(config)>ip route 192.166.1.0  255.255.255.0 192.170.1.250 
Router0(config) > exit
// ルーティングテーブルを表示して確認
Router0 > show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

S    192.166.1.0/24 [1/0] via 192.170.1.250
     192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.1.0/24 is directly connected, GigabitEthernet0/0
L       192.168.1.254/32 is directly connected, GigabitEthernet0/0
     192.170.1.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.170.1.0/24 is directly connected, GigabitEthernet0/1
L       192.170.1.254/32 is directly connected, GigabitEthernet0/1
```

## 3.Router1側の設定

以下コマンドを実行

```bash
Router1>en
Router1>config t

// Giga0/0にipアドレス割当
Router1(config)>interface GigabitEthernet0/0

Router1(config-if)>ip address 192.166.1.254 255.255.255.0

Router1(config-if)>no shutdown

Router1(config-if)>exit

// Giga0/1にipアドレス割当
Router1(config)>interface GigabitEthernet0/1

Router1(config-if)>ip address 192.170.1.250 255.255.255.0

Router1(config-if)>no shutdown

Router1(config-if)>exit


// スタティックルートを作成

Router1(config) >ip route 192.168.1.0  255.255.255.0 192.170.1.254 

// ルーティングテーブルを表示して確認
Router1 > show ip route 

Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

     192.166.1.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.166.1.0/24 is directly connected, GigabitEthernet0/0
L       192.166.1.254/32 is directly connected, GigabitEthernet0/0
S    192.168.1.0/24 [1/0] via 192.170.1.254
     192.170.1.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.170.1.0/24 is directly connected, GigabitEthernet0/1
L       192.170.1.250/32 is directly connected, GigabitEthernet0/1

```

## 4.PC0側の設定

以下GUIで設定をする。(DHCPは切る)

IPアドレス　192.168.1.1

サブネットマスク:255.255.255.0

デフォルトゲートウェイ：192.168.1.254


## 5.PC1側の設定

以下GUIで設定をする。(DHCPは切る)

IPアドレス　192.166.1.1

サブネットマスク:255.255.255.0

デフォルトゲートウェイ：192.166.1.254




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
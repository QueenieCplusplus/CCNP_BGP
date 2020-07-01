# CCNP BGP

邊界閘道器路由協定

BGP 是 EGP 的改良。
僅適用於 ISP 之間，抑或是超大型企業與 ISP 之間的連接，故不常見。

# AS

自治系統，全名為 Autonomous System。AS 的功能在一定範圍內，使用單一技術（IGP 包含 RIP 靜態路由、不支援可變動長度子網路遮罩的 IGP、僅有 cisco 有的 EIGRP、可支援多品牌和多層級架構的 OSPF）抑或是多個 IGP 方式來來管理一群路由器（IGP），
其相關技術亦可參考 CIDR （c lass IP 位置的配置）。
https://github.com/QueenieCplusplus/CCNP_IP#cidr--supernet

然而透過 BGP，仍然可以得到單一路由協定的路由資訊。

          AS 100                           AS 200


            R 群- R-gw-----------------gw-R -R群
                   |                    |
                  BGP                  BGP
                  
                    \                  /
                     \                /
                      \              /
                      
                             BGP
                             

* ASN

自治系統號碼是由 IANA 組織所管理的。
亞洲區各國 ASN 申請單位是 Asia Pacific - NIC 組織。
號碼共有 2 ^ 16 = 65535 個，其中私有網路使用的 ASN 為 64512 ~ 65535 區間。 

* BGP Requirements

不同 AS 間的頻寬要大，不然 BGP 無法運作，另外 BGP 的路由器的能力（需要較高的處理器和較多的記憶體，最好是 128 MB RAM）也要能勝任任務。

# BGP 

  在 ASs 間，運用 Routing Policy 路由政策決定封包的路徑。另外，BGP 屬於 Distance Vector 距離向量的路由協定。
  仰賴 TCP port 179 (預設) 傳送訊息。
  
  * BGP 的路徑選擇
  
  BGP 是 ASs 間，用來接收和更新路徑的主要協定，由於互聯網路是由多的 ASs 所組成，常會有許多路徑可達到同一網段的情形，
  因此 BGP 需要一定程序來選擇最佳路徑。
  
  依據的屬性，請詳封包標頭（路徑屬性）的解析。
  
  (1) 同一 As 內，『同步化』開啟情況下，方才會送出路徑資訊。
  
  (2) Next Hops 需要 reachable 方才會使用這條路徑，故需要使用 IGP 協定。
  
  (3) 選擇高權值 Weight 的路徑。
  
  (4)
  
  (5)
  
  (6)
  
  (7)
  
  (8)
  
  (9)
  
  (10)
  
  (11)
  
  
# BPG Packet Header 

解讀 BGP 封包標頭 (路徑屬性)

* Synch 同步化

* Weight 權值

  權值區間為 0 ~ 65535，設定路由的預設權值為 32768，未設定則預設為 0。
  倘若有多條路徑可達到相同目的地時，可採用 Wight 屬性。

(to be continued...)

# BPG CLI setup

解析 BGP 設定指令

(to be continued...)


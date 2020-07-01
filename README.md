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
  
  因為 BGP 能區分出不同 AS 路徑和 同一 AS 路徑（分別採用 EBGP 和 IBGP ），細節詳見 Next-Hops 屬性。
  
  依據的屬性，請詳封包標頭（路徑屬性）的解析。
  
            (1) 同一 As 內，『同步化』開啟情況下，方才會送出路徑資訊。否則詳見 10。

            (2) Next-Hop 需要 reachable 方才會使用這條路徑，故需要使用 IGP 協定。

            (3) 選擇高權值 Weight 的路徑。

            (4) 若權值相等，則採用較高的 Local Preference。

            (5) 若有相同 Local Prefreance，則採用本身產生的路徑的 Router。

            (6) 若有相同 Local Prefreance，且都非本身產生的路徑的 Router，則選擇最短的 AS-Path。

            (7) 若有相同 AS-Path，則選擇最小 Origin Code，基本上 IGP < EGP < Incomplete。

            (8) 若有相同 Origin Code，則選擇最小 MED 值。

            (9) 若 MED 相同，則選則 EBGP，才選擇 IBGP。

            (10) 倘若 Async 關閉，則只有 IGP 協定可使用，則會選擇最短 BGP Next-Hop 路徑。

            (11) 最後是最久路徑，在選擇最小的鄰接 BGP Router ID, 通常是最高的 IP 位址代表。
  
  
# BPG Packet Header 

解讀 BGP 封包標頭主要欄位

* Open 是否打開

* Update 路由更新訊息

           （重要訊息請詳 Path Attribute。）

* Notification （錯誤）通知
 
* Keepalive 存活時間

            可供判定鄰近路由器是否存活即其網段是否可達到。
            Hold Time 最好是 Keep Alive 的三倍時間。
            不用太頻繁，避免耗損 CPU。

# Path Attributes 

解讀 BGP 封包標頭欄位 Update 內的路徑屬性。

* Synch 同步化

* Next-Hops 下一跳

             因為 BGP 能區分出不同 AS 路徑和 同一 AS 路徑（分別採用 EBGP 和 IBGP ），
             故此屬性的使用上也有所不同。

* Weight 權值

            權值區間為 0 ~ 65535，設定路由的預設權值為 32768，未設定則預設為 0。
            倘若有多條路徑可達到相同目的地時，可採用 Wight 屬性。
  
* Local Preference 在地化偏好

            此屬性是在同一 AS 內才會被傳送，使用的是 IBGP 協定，目的是通往相同目的網段的路徑們，從中選擇一個喜好的程度。
            數值越高，受喜愛程度越大。
  
* AS Path 自治區路徑

            此屬性由一連串 ASN 所組成，用來提供到達目的地網路的路徑，此路徑的行程是由最早產生這路徑的 AS，
            會將 ASN 加入到 AS-Path 屬性中，收到路徑的 AS 在送出路徑時，都會將自己的 ASN 號碼加入到此清單中。

            為了避免迴路 Loop，清單中若有自己的 ASN，就不會繼續更新路徑了！（不會重複放自己的 ASN 至清單中）
  
* Origin 路徑起源

          Origin Code 代表路徑的起源，即此 AS 內產生路由更新的來源，資訊都在 BGP 的 Update 中。
          三種：
          
          IGP
          
          EGP
          
          Incomplete
          
* Auto Aggregate  (非強制性)

           倘若 Update 資訊中，集合起來的路徑無法辨識路徑來源，就不會加入此屬性。
           倘若真的無法辨識路徑來源，等同資料的遺失，此時會選擇較少指定的路徑，加上此屬性，方才送出訊息。

* Community (非強制性)

          (to be continued...)

# BPG CLI setup

解析 BGP 設定指令

         (to be continued...)


# CCNP BGP

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

不同 AS 間的頻寬要大，不然 BGP 無法運作，另外 BGP 的路由器的能力也要能勝任任務。

# BGP 

  在 ASs 間，運用 Routing Policy 路由政策決定封包的路徑。

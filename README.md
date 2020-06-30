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
                             
                             

# BGP 

  在 ASs 間，運用 Routing Policy 路由政策決定封包的路徑。

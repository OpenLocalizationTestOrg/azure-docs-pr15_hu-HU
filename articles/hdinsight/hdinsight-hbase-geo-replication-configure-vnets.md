<properties 
   pageTitle="Virtuális hálózatok között a virtuális Magánhálózati kapcsolat beállítása |} Microsoft Azure" 
   description="Megtudhatja, hogyan állíthatja be VPN-kapcsolatok és a tartomány névfeloldás Azure virtuális hálózatok között, és hogyan konfigurálható HBase geo-replikáció." 
   services="hdinsight,virtual-network" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="06/28/2016"
   ms.author="jgao"/>

# <a name="configure-a-vpn-connection-between-two-azure-virtual-networks"></a>A virtuális Magánhálózati kapcsolat két Azure virtuális hálózatok között konfigurálása  

> [AZURE.SELECTOR]
- [Virtuális Magánhálózati kapcsolatok konfigurálása](hdinsight-hbase-geo-replication-configure-vnets.md)
- [A DNS konfigurálása](hdinsight-hbase-geo-replication-configure-dns.md)
- [HBase replikáció konfigurálása](hdinsight-hbase-geo-replication.md) 

Azure virtuális hálózati hely közötti kapcsolat VPN átjáró szolgáltatással Ipsec/egy biztonságos alagutas biztosítására használja. A VNets lehet a különböző előfizetések és különböző területei. Több elem webhely konfigurációk VNet kommunikációt VNet is össze. Vannak VNet connectivity az VNet számos oka lehet:

- Régió geo-redundancia és geo-jelenléti Cross 
- Erős elkülönítési határa területi több szálon futó alkalmazások 
- Idegen előfizetését, a szervezet közötti kommunikáció Azure-ban

További tudnivalókért lásd: a [Configure egy VNet VNet kapcsolatra](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). 

Azt a videó megtekintése:

> [AZURE.VIDEO configure-the-vpn-connectivity-between-two-azure-virtual-networks]

Ebben az oktatóanyagban része a [sorozat] [ hdinsight-hbase-replication] HBase geo replikációs létrehozásával. 

- A virtuális Magánhálózati kapcsolat (ebben az oktatóanyagban) virtuális hálózatok között konfigurálása
- [A virtuális hálózatok DNS konfigurálása][hdinsight-hbase-geo-replication-dns]
- [HBase geo replikáció konfigurálása][hdinsight-hbase-geo-replication]

Az alábbi ábra bemutatja a két virtuális hálózat, ebben az oktatóanyagban létrehoznia:

![HDInsight HBase replikációs virtuális hálózatdiagram][img-vnet-diagram]
 

##<a name="prerequisites"></a>Előfeltételek
Ebben az oktatóanyagban megkezdése előtt a következőket kell rendelkeznie:

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Azure PowerShell munkaállomás**.

    PowerShell-parancsfájlokat futtatásához győződjön meg arról, hogy csatlakozva van az Azure előfizetés az alábbi parancsmag használatával:

        Add-AzureAccount

    Ha több Azure előfizetéssel rendelkezik, használja a következő parancsmagot az aktuális előfizetés beállítása:

        Select-AzureSubscription <AzureSubscriptionName>
        
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


>[AZURE.NOTE] Azure service és virtuális gép nevének egyedinek kell lennie. Ebben az oktatóanyagban használt név Contoso-[Azure Service/virtuális neve]-[Európai Unió / USA-beli]. Ha például a Contoso-VNet-Európai Unió az Észak-Európa adatközpont; az Azure virtuális hálózati A Contoso-DNS-hu a DNS-kiszolgáló virtuális a kelet-Amerikai Egyesült Államok adatközponthoz. A saját szereplő nevekkel együtt kell lyncen.
 

##<a name="create-two-azure-vnets"></a>Hozzon létre két Azure VNets



**A Contoso-VNet-Európai Unió nevű Észak-Európában virtuális hálózat létrehozása**

1.  Jelentkezzen be az [Azure klasszikus portál][azure-portal].
2.  Kattintson az **Új**, a **hálózati szolgáltatások**, a **virtuális hálózati**, **egyéni létrehozása**.
3.  Írja be:

    - **Név**: Contoso-VNet-Európai Unió
    - **Hely**: Észak-Európa

        Ebben az oktatóanyagban Észak-Európa és kelet-Amerikai Egyesült Államok adatközpontokkal használja. Megadhatja, hogy a saját adatközpontokkal.
4.  Írja be:

    - **DNS-kiszolgáló**: (hagyja üresen) 
    
        A saját DNS-kiszolgáló névfeloldásra virtuális hálózatok belül lesz szüksége. Használata Azure által biztosított névfeloldás, és mikor érdemes használni a saját DNS-kiszolgáló mikor további tudnivalókért lásd: [Name felbontás (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md). VNets közötti névfeloldás konfigurálása című témakör nyújt tájékoztatást [DNS konfigurálása Azure virtuális hálózatok között][hdinsight-hbase-dns].
  
    - **Virtuális Magánhálózat pont-webhely konfigurálása**: (nincs bejelölve)

        Webhely-pont ebben az esetben nem érvényes.

    - **A webhely VPN konfigurálása**: (nincs bejelölve)
    
        A webhely virtuális Magánhálózati kapcsolatot az Azure virtuális hálózati a kelet-Amerikai Egyesült Államok adatközpontban fog konfigurálni.
5.  Írja be:

    -   **Cím terület indítása IP**: 10.1.0.0
    -   **Cím terület CIDR**: / 16
    -   **Alhálózat-1 indítása IP**: 10.1.0.0
    -   **CIDR alhálózat-1**: / 24

    A címterületet nem is az Amerikai Egyesült Államok virtuális hálózati átfedést.  

**A Contoso-VNet-Európai Unió nevű nyugati Európában virtuális hálózat létrehozása**

- Ismételje meg az utolsó eljárást, az alábbi értékeket:

    - **Név**: Contoso-VNet-Amerikai Egyesült Államok
    - **Hely**: Kelet-Amerikai Egyesült Államok
     
    - **DNS-kiszolgáló**: (hagyja üresen)
    - **Virtuális Magánhálózat pont-webhely konfigurálása**: (nincs bejelölve)
    - **A webhely VPN konfigurálása**: (nincs bejelölve)
     
    - **Cím terület indítása IP**: 10.2.0.0
    - **Cím terület CIDR**: / 16
    - **Alhálózat-1 indítása IP**: 10.2.0.0
    - **CIDR alhálózat-1**: / 24

















##<a name="configure-a-vpn-connection-between-the-two-vnets"></a>A virtuális Magánhálózati kapcsolat között a két VNets konfigurálása

###<a name="create-local-networks"></a>Hozzon létre helyi hálózatok

Amikor létrehoz egy VNet VNet konfiguráción, kell minden VNet azonosítása egymással a helyi hálózaton helyként konfigurálása. Ebben a részben egy helyi hálózatként fogja minden VNet konfigurálása. A helyi hálózat ossza meg a megfelelő VNet az azonos IP cím szóközöket.

![Azure virtuális Magánhálózati webhely konfiguráció – azure helyi hálózat konfigurálása][img-vnet-lnet-diagram]


**Hozzon létre egy helyi hálózaton nevű a Contoso-LNet-Európai Unió egyező a Contoso-VNet-Európai Unió hálózati címterület használatára**

1. Az Azure klasszikus portálról kattintson az **Új**, a **Hálózati szolgáltatások**, a **Virtuális hálózat**, a **Helyi hálózaton hozzáadása**.
3. Írja be:

    - **Név**: Contoso-LNet-Európai Unió
    - **Virtuális Magánhálózati eszköz IP-címe**: 192.168.0.1 (Ez a cím frissülnek később)

        A tényleges külső IP-cím általában VPN-eszközhöz szeretné használni. VNet való VNet konfigurációk a virtuális Magánhálózati gateway IP-címe lesz használhatja. Tekintve, hogy nem létrehozta a virtuális Magánhálózati átjárók a két VNets még, akkor írja be a arbitary IP-címet, és térjen vissza az szolgáló fix it.
4.  Írja be:

    - **Cím terület indítása IP:** 10.1.0.0
    - **Cím terület CIDR:** /16
    
    Meg kell felelnie pontosan a tartományt, amelyet Ön korábban megadott Contoso-VNet-Európai Unió.

**Hozzon létre egy helyi hálózaton nevű a Contoso-LNet-Amerikai Egyesült Államok a Contoso-VNet-hu hálózati címterületet egyező**

- Ismételje meg az alábbi paraméterekkel utolsó eljárást:

    - **Név**: Contoso-LNet-Amerikai Egyesült Államok
    - **Virtuális Magánhálózati eszköz IP-címe**: 192.168.0.1 (Ez a cím frissülnek később)
     
    - **Cím terület indítása IP**: 10.2.0.0
    - **Cím terület CIDR**: / 16


###<a name="create-vpn-gateways"></a>Virtuális Magánhálózati átjárók létrehozása

Ebben a konfigurációban két részből áll. Először állítsa be a helyi hálózaton VNet hely közötti kapcsolatot, és ezt követően hozhatja létre a dinamikus útválasztási VPN. A VNet VNet dinamikus útválasztási VPN adatai az Azure virtuális Magánhálózati átjárók szükséges. Azure statikus útválasztási VPN adatai nem támogatottak.

**A Contoso-LNet-hu Contoso-VNet-Európai Unió hely közötti kapcsolat beállítása**

1.  Az Azure klasszikus portálról kattintson a bal oldali ablaktáblában **hálózatok**
2.  Kattintson a **Contoso-VNet-Európai Unió**.
3.  Kattintson a **CONFIGUE** fülre.
4.  Jelölje be a **Csatlakozás helyi hálózathoz**.
5.  A **Helyi hálózaton**jelölje ki a **Contoso-LNet-hu**.
6.  Kattintson a **Hozzáadás átjáró alhálózat** a virtuális hálózati szóközöket szakaszban.
7.  Kattintson a **MENTÉS**gombra.
8.  Kattintson az **OK gombra** kattintva erősítse meg.


**A Contoso-VNet-Európai Unió VPN átjáró létrehozásához**

1.  Az Azure klasszikus portálról kattintson az **IRÁNYÍTÓPULT** lapon.
4.  Kattintson az **ÁTJÁRÓ létrehozása** a lap alján lévő gombra, és kattintson a **Dinamikus útválasztás**.
5.  Kattintson az **Igen gombra** kattintva erősítse meg. Figyelje meg a lapon az átjáró ábra sárga változik, és megjelenik a átjáró létrehozása. Általában elég körülbelül 15 percet, hogy az átjáró létrehozásához.

    Az átjáró állapotának megváltozásakor való csatlakozás minden Gateway IP-címe lesz látható az irányítópult. Jegyezze fel az IP-címet, amely megfelel az egyes VNet ügyelve nem arra, hogy keverjen ki őket. Ezek az IP-címek a helyőrző IP-címek a virtuális Magánhálózati eszköz a helyi hálózat szerkesztésekor használt.

6.  Másolat készítése a **GATEWAY IP-címet**. Akkor használja a virtuális Magánhálózati gateway IP-cím beállítása Contoso-VNet-Európai Unió a következő szakaszban.

**A Contoso-VNet-Európai Unió VPN átjáró létrehozásához**

- Ismételje meg az utolsó két eljárás Contoso-LNet-Európai Unió és a virtuális Magánhálózati az átjárók létrehozása Contoso-VNet-USA-beli hely közötti kapcsolatot Contoso-Vnet – hu-hoz szükséges. Ha végzett, a Contoso-VNet-hu VPN gateway IP-címe lesz.


### <a name="set-the-vpn-device-ip-addresses-for-local-networks"></a>A virtuális Magánhálózati eszköz IP-címek a helyi hálózat beállítása
A legutóbbi csoportban az egyes a VNets hoz létre egy virtuális Magánhálózati átjárót. Az IP-címek a virtuális Magánhálózati átjárók nyissa meg. Most már meg is visszatérhet helyi hálózaton VPN eszköz IP-címek konfigurálása.

**A Contoso-LNet-Európai Unió a virtuális Magánhálózati eszköz IP-cím beállítása** 

1.  Az Azure klasszikus portálról a bal oldali ablaktáblában kattintson a **hálózatok** hivatkozásra.
2.  Kattintson a **Helyi hálózat** tetején.
3.  Kattintson a **Contoso-LNet-Európai Unió**, és kattintson a **SZERKESZTÉS** alján.
4.  Frissítse a **virtuális Magánhálózati eszköz IP-címet**.  Ez az a cím, letölthető Contoso-VNET-Európai Unió IRÁNYÍTÓPULT lapja.
5.  Kattintson a jobb gombbal.
6.  Kattintson az ellenőrzés gombra.

**A virtuális Magánhálózati eszköz IP-cím beállítása a Contoso-LNet-Amerikai Egyesült Államok** 

- Ismételje meg a virtuális Magánhálózati eszköz IP-cím beállítása a Contoso-LNet-hu az utolsó eljárást.

###<a name="set-vnet-gateway-keys"></a>VNet átjáró billentyűk beállítása

Az Vnet átjárók megosztott billentyűvel a virtuális hálózatok közötti kapcsolatok hitelesítést végezni. A kulcs nem állítható be az Azure klasszikus portálról. A PowerShell vagy .NET SDK kell használnia.

**A billentyűk beállítása**

1. A munkaállomás nyissa meg a **Windows PowerShell ISE** vagy a Windows PowerShell-konzol.
2. Frissítse a paraméterek a követés parancsfájl, és indítsa el:

        Add-AuzreAccount
        Select-AzureSubscription -[AzureSubscriptionName]
        Set-AzureVNetGatewayKey -VNetName ContosoVNet-EU -LocalNetworkSiteName Contoso-LNet-US -SharedKey A1b2C3D4
        Set-AzureVNetGatewayKey -VNetName ContosoVNet-US -LocalNetworkSiteName Contoso-LNet-EU -SharedKey A1b2C3D4 


##<a name="check-the-vpn-connection"></a>Jelölje be a virtuális Magánhálózati kapcsolat 

A VNets rendszerbe állított bármely VMs nélkül is használhatja a virtuális vizuális Hálódiagram VNet Irányítópultlap az Azure klasszikus portálon a kapcsolat állapotának ellenőrzése:

![HDInsight HBase replikációs virtuális hálózati virtuális Magánhálózati kapcsolat állapota][img-vpn-status]
  



##<a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban megtanulta beállításáról a virtuális Magánhálózati kapcsolat két Azure virtuális hálózatok között van. Az adatsor más két cikkeket terjed ki:

- [Konfigurálja a DNS-Azure virtuális hálózatok között][hdinsight-hbase-geo-replication-dns]
- [HBase geo replikáció konfigurálása][hdinsight-hbase-geo-replication]



[hdinsight-hbase-geo-replication-dns]: hdinsight-hbase-geo-replication-configure-dns.md
[hdinsight-hbase-geo-replication]: hdinsight-hbase-geo-replication.md


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com

[powershell-install]: ../install-configure-powershell.md



[hdinsight-hbase-replication]: hdinsight-hbase-geo-replication.md
[hdinsight-hbase-dns]: hdinsight-hbase-geo-replication-configure-dns.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-diagram.png
[img-vnet-lnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-lnet-diagram.png
[img-vpn-status]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-status.png 

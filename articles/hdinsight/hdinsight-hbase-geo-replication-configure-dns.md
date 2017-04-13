<properties 
   pageTitle="Konfigurálja a DNS-Azure virtuális hálózatok között |} Microsoft Azure" 
   description="Megtudhatja, hogyan állíthatja be virtuális Magánhálózati kapcsolatot, és a tartomány névfeloldás virtuális hálózatok között, és hogyan konfigurálható HBase geo-replikáció." 
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

# <a name="configure-dns-between-two-azure-virtual-networks"></a>Konfigurálja a DNS-Azure virtuális hálózatok között

> [AZURE.SELECTOR]
- [Virtuális Magánhálózati kapcsolatok konfigurálása](../hdinsight-hbase-geo-replication-configure-vnets.md)
- [A DNS konfigurálása](hdinsight-hbase-geo-replication-configure-dns.md)
- [HBase replikáció konfigurálása](hdinsight-hbase-geo-replication.md) 


Megtudhatja, hogy miként vehet fel, és állítsa be a DNS-kiszolgálók Azure virtuális hálózatok kezelése névfeloldás belül, és a virtuális hálózatok között.

Ebben az oktatóanyagban része a második a [sorozat] [ hdinsight-hbase-geo-replication] HBase geo replikációs létrehozásával kapcsolatban:

- [Virtuális hálózatok között egy virtuális Magánhálózati kapcsolatok konfigurálása][hdinsight-hbase-geo-replication-vnet]
- A virtuális hálózatok (ebben az oktatóanyagban) DNS konfigurálása
- [HBase geo replikáció konfigurálása][hdinsight-hbase-geo-replication]


Az alábbi ábra szemlélteti a két virtuális hálózat [beállítása a virtuális Magánhálózati kapcsolat két virtuális hálózatok között]a létrehozott[hdinsight-hbase-geo-replication-vnet]:

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

- **Két Azure virtuális hálózati a virtuális Magánhálózati kapcsolatot**.  Útmutatásért lásd: [a virtuális Magánhálózati kapcsolat két Azure virtuális hálózatok között konfigurálása][hdinsight-hbase-geo-replication-vnet].

>[AZURE.NOTE] Azure service és virtuális gép nevének egyedinek kell lennie. Ebben az oktatóanyagban használt név Contoso-[Azure Service/virtuális neve]-[Európai Unió / USA-beli]. Ha például a Contoso-VNet-Európai Unió az Észak-Európa adatközpont; az Azure virtuális hálózati A Contoso-DNS-hu a DNS-kiszolgálót az kelet-Amerikai Egyesült Államok adatközpontban virtuális. A saját szereplő nevekkel együtt kell lyncen.
 
 
##<a name="create-azure-virtual-machines-to-be-used-as-dns-servers"></a>A DNS-kiszolgálók helyőrzőként Azure virtuális gépeken futó létrehozása

**Hozzon létre egy virtuális gép belül Contoso-VNet-Európai Unió, nevű a Contoso-DNS-Európai Unió**

1.  Kattintson a **Új** **számítja ki**, **virtuális gép**, **FROM GYŰJTEMÉNY**.
2.  Válassza a **Windows Server 2012 R2 adatközponthoz**.
3.  Írja be:
    - **Virtuális számítógépnév**: Contoso-DNS-Európai Unió
    - **Új felhasználó nevét**: 
    - **Új jelszó**: 
4.  Írja be:
    - **FELHŐBELI SZOLGÁLTATÁSTÓL**: hozzon létre egy új, felhőalapú szolgáltatásba
    - **Régió/AFFINITÁS csoport/virtuális hálózati**: (Contoso-VNet-Európai Unió kiválasztása)
    - **Virtuális hálózati ALHÁLÓZAT**: alhálózat-1
    - **TÁRTERÜLET-fiók**: automatikusan generált tárterület-fiók
    
        A felhőalapú szolgáltatás neve megegyezik a virtuális számítógépnév lesz. Ebben az esetben ez Contoso-DNS-Európai Unió. Az ezt követő virtuális gépeken futó lehet választhatja a azonos felhőalapú szolgáltatást használhatja.  Az összes a virtuális gépeken futó felhőalapú ugyanezt a oszthat meg a virtuális ugyanabba a hálózatba, és a tartomány utótag.

        A tárterület-fiókot a virtuális gép képfájl tárolására szolgál. 
    - **VÉGPONT**: (görgessen le, és válassza a **DNS**) 

A virtuális gép létrehozását követően keresse meg a belső IP és a külső IP.

1.  Kattintson a **Contoso-DNS-Európai Unió**virtuális gép nevére.
2.  Az **Irányítópult**elemre.
3.  Írja le:
    - NYILVÁNOS VIRTUÁLIS IP-CÍM
    - BELSŐ IP-CÍM


**Hozzon létre egy virtuális gép Contoso-VNet-hu belül nevű a Contoso-DNS-Amerikai Egyesült Államok** 

- Ismételje meg ugyanezt az eljárást, az alábbi értékeket egy virtuális gép létrehozásához:
    - VIRTUÁLIS számítógép neve: A Contoso-DNS-hu
    - RÉGIÓ/AFFINITÁS csoport/virtuális hálózati: jelölje be a Contoso-VNET-Amerikai Egyesült Államok
    - VIRTUÁLIS hálózati ALHÁLÓZAT: Alhálózat-1
    - TÁRTERÜLET-fiók: Automatikusan generált tároló fiókot használ.
    - VÉGPONT: (DNS kiválasztása)

##<a name="set-static-ip-addresses-for-the-two-virtual-machines"></a>A két virtuális gépeken futó statikus IP-címeket beállítása

A DNS-kiszolgálók statikus IP-címek szükséges.  Ez a lépés nem végezhető el az Azure klasszikus portálról. Azure PowerShell fogja használni.

**A két virtuális gépeken futó statikus IP-cím beállítása**

1. Nyissa meg a Windows PowerShell ISE.
2. A következő parancsmagok futtatásával.  

        Add-AzureAccount
        Select-AzureSubscription [YourAzureSubscriptionName]
        
        Get-AzureVM -ServiceName Contoso-DNS-EU -Name Contoso-DNS-EU | Set-AzureStaticVNetIP -IPAddress 10.1.0.4 | Update-AzureVM
        Get-AzureVM -ServiceName Contoso-DNS-US -Name Contoso-DNS-US | Set-AzureStaticVNetIP -IPAddress 10.2.0.4 | Update-AzureVM 

    ServiceName a felhőalapú szolgáltatás neve. Mivel a DNS-kiszolgálót az első virtuális gép a felhőalapú szolgáltatás, a felhőalapú szolgáltatás neve megegyezik a virtuális számítógép nevét.

    Előfordulhat, hogy módosítania kell a a neveket, amelyekhez rendelkezik a megfelelő ServiceName és nevét.


##<a name="add-the-dns-server-role-the-two-virtual-machines"></a>A DNS-kiszolgáló szerepkör a két virtuális gépeken futó hozzáadása

**A Contoso-DNS-Európai Unió adja meg a DNS-kiszolgálói szerepkör**

1.  Az Azure klasszikus portálról kattintson a **virtuális gépeken futó** a bal oldalon. 
2.  Kattintson a **Contoso-DNS-Európai Unió**.
3.  Kattintson az **IRÁNYÍTÓPULT** tetején.
4.  Kattintson a **Csatlakozás** lentről felfelé, és kövesse az utasításokat követve csatlakoztassa a virtuális géphez RDP keresztül.
2.  Az RDP-munkamenet belül kattintson a bal alsó nyissa meg a kezdőképernyőn Windows gombjára.
3.  Kattintson a **Kiszolgálókezelő** csempére.
4.  Kattintson a **szerepkörök és szolgáltatások hozzáadása**gombra.
5.  Kattintson a **Tovább** gombra
6.  Jelölje ki a **szerepköralapú vagy a szolgáltatás-alapú telepítés**, és kattintson a **Tovább gombra**.
7.  Jelölje be a DNS virtuális gép (meg kell kiemelten már), és kattintson a **Tovább**gombra.
8.  Jelölje be a **DNS-kiszolgáló**.
9.  Kattintson a **Szolgáltatás hozzáadása**elemre, és kattintson a **Folytatás**gombra.
10. Kattintson a **Tovább** gombra háromszor, és kattintson a **telepítés**gombra. 

**A DNS-kiszolgálói szerepkör hozzáadása a Contoso-DNS-Amerikai Egyesült Államok**

- Ismételje meg a DNS-szerepkör hozzáadása a **Contoso-DNS-hu**.

##<a name="assign-dns-servers-to-the-virtual-networks"></a>A DNS-kiszolgálók hozzárendelése a virtuális hálózatok

**A két DNS-kiszolgálók rögzítése**

1.  Az Azure klasszikus portálról kattintson az **Új**, **Hálózati szolgáltatások**, **Virtuális hálózati**, **REGISZTRÁLHATJA a DNS-kiszolgáló**.
2.  Írja be:
    - **Név**: Contoso-DNS-Európai Unió
    - **DNS-kiszolgáló IP-címe**: 10.1.0.4 – az IP-cím a DNS server virtuális gép IP-cím hozzá kell.
     
3.  Ismételje meg a folyamat Contoso-DNS-hu regisztrálása az alábbi beállításokat:
    - **Név**: Contoso-DNS-Amerikai Egyesült Államok
    - **DNS-kiszolgáló IP-címe**: 10.2.0.4

**A két DNS-kiszolgálók hozzárendelése a két virtuális hálózatok**

1.  A klasszikus portál bal oldali ablaktáblában kattintson a **hálózat** elemre.
2.  Kattintson a **Contoso-VNet-Európai Unió**.
3.  Kattintson a **KONFIGURÁLÁS**gombra.
4.  A **DNS-kiszolgálók** szakaszban jelölje be a **Contoso-DNS-Európai Unió** .
5.  Kattintson a **MENTÉS** a lap alján lévő gombra, és kattintson az **Igen gombra** kattintva erősítse meg.
6.  A **Contoso-DNS-hu** DNS-kiszolgáló hozzárendelése a **Contoso-VNet-hu** virtuális hálózat kell ismételni.

Az összes a virtuális gépeken futó a virtuális hálózatok telepített frissítése a DNS-kiszolgáló konfigurációja újra kell indítani.

**A virtuális gépeken futó indítani**

1. Az Azure klasszikus portálról kattintson a **virtuális gépeken futó** a bal oldalon.
2. Kattintson a **Contoso-DNS-Európai Unió**.
3. Kattintson az **Irányítópult** tetején.
4. Kattintson az **Újraindítás** alján.
5. Ismételje meg ezeket a lépéseket az újraindításhoz **Contoso-DNS-hu**.


##<a name="configure-dns-conditional-forwarders"></a>Feltételes továbbítókat konfigurálása

A DNS-kiszolgáló minden virtuális hálózaton csak oldható meg a DNS-nevek, hogy virtuális hálózaton belül. Szeretne beállítani egy feltételes továbbító, mutasson a partner DNS-kiszolgáló neve megoldások a peer virtuális hálózaton.

Feltételes továbbító konfigurálásához kell tudnia a két DNS-kiszolgálók a tartomány mértékegységeket. A DNS-mértékegységeket a virtuális gépeken futó létrehozásakor használt Cloud Services beállításaitól függően eltérő lehet. Minden DNS-utótag a virtuális hálózaton használt fel kell vennie egy feltételes továbbító. 

**A tartomány mértékegységeket két DNS-kiszolgálók keresése**

1. A **Contoso-DNS-Európai Unió**RDP.
2. Nyissa meg a Windows PowerShell-konzol vagy a parancssor parancsot.
3. Futtassa az **ipconfig**, és jegyezze fel a **kapcsolatot-specifikus DNS-utótag**.
4. Ne zárja be az RDP-munkamenet, akkor is szüksége lesz rá. 
5. Ismételje meg ezeket a lépéseket megtudhatja, hogy a **Contoso-DNS-hu**a **kapcsolat-specifikus DNS-utótag** .


**DNS-továbbítókat konfigurálása**
 
1.  Az a **Contoso-DNS-Európai Unió**RDP-munkamenet kattintson a Windows-billentyűt, a bal alsó sarokban.
2.  Kattintson a **Felügyeleti eszközök**.
3.  Kattintson a **DNS**elemre.
4.  A bal oldali ablaktáblában bontsa ki a **DSN**, **Contoso-DNS-Európai Unió**.
5.  Írja be az alábbi adatokat:
    - **DNS-tartomány**: Adja meg a DNS-utótag a Contoso-DNS-amerikai. Példa: Contoso-DNS-US.b5.internal.cloudapp.net.
    - **A fő kiszolgálók IP-címe**: Adja meg a 10.2.0.4, amely a Contoso-DNS-hu 's IP-címe.
6.  Nyomja le az **ENTER BILLENTYŰT**, és kattintson **az OK**gombra.  Most már lesz a Contoso-DNS-hu 's IP-címet a Contoso-DNS-Európai Unió megoldható.
7.  Ismételje meg a DNS-továbbító hozzáadása a DNS-szolgáltatás az alábbi értékeket a Contoso-DNS-hu virtuális gépen:
    - **DNS-tartomány**: Adja meg a DNS-utótag Contoso-DNS-Unió. 
    - **A fő kiszolgálók IP-címe**: Adja meg a 10.2.0.4, amely a Contoso-DNS-Európai Unió 's IP-címe.

##<a name="test-the-name-resolution-across-the-virtual-networks"></a>A névfeloldás tesztelje a virtuális hálózatok között

Most már állomásnév tesztelheti a virtuális hálózatok között. Ping tűzfal blokkolja alapértelmezés szerint.  Az nslookup használatával a DNS server virtuális gépeken futó (FQDN kell használni) megoldása a peer hálózatok.  


##<a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban megtanulta névfeloldás beállításáról a virtuális Magánhálózati kapcsolat virtuális hálózatok között van. Az adatsor más két cikkeket terjed ki:

- [A virtuális Magánhálózati kapcsolat két Azure virtuális hálózatok között konfigurálása][hdinsight-hbase-geo-replication-vnet]
- [HBase geo replikáció konfigurálása][hdinsight-hbase-geo-replication]



[hdinsight-hbase-geo-replication]: hdinsight-hbase-geo-replication.md
[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[powershell-install]: powershell-install-configure.md

[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-DNS/HDInsight.HBase.VPN.diagram.png 

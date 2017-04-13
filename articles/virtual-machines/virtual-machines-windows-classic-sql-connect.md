<properties
    pageTitle="Csatlakozás SQL Server virtuális géphez (klasszikus) |} Microsoft Azure"
    description="Megtudhatja, hogy miként csatlakozhat az Azure virtuális gépen futó SQL Server. Ez a témakör a klasszikus telepítési modellt használja. Az esetek különböznek egymástól attól függően, hogy a hálózati konfigurációja, és hol található az ügyfélnek."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="jroth" />

# <a name="connect-to-a-sql-server-virtual-machine-on-azure-classic-deployment"></a>Csatlakozás egy SQL Server virtuális gép Azure (klasszikus telepítés)

> [AZURE.SELECTOR]
- [Erőforrás-kezelő](virtual-machines-windows-sql-connect.md)
- [Klasszikus](virtual-machines-windows-classic-sql-connect.md)

## <a name="overview"></a>– Áttekintés

Ez a témakör ismerteti, hogyan csatlakozhat az Azure virtuális-gépen futó SQL Server-példányt. Néhány [Általános kapcsolódási esetek](#connection-scenarios) foglalkozik és a majd az [SQL Server-Azure virtuális való csatlakozással konfigurálása a lépések részletes](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm)biztosít.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ha az erőforrás-kezelő VMs használja, olvassa el a [Csatlakozás az SQL Server virtuális gép a Azure erőforrás-kezelő használatával](virtual-machines-windows-sql-connect.md).

## <a name="connection-scenarios"></a>Kapcsolat felhasználási területei

Egy ügyfél csatlakozik egy virtuális gépen futó SQL Server módja eltérő attól függően, hogy az ügyfél és a számítógép/hálózati konfigurációja helyét. Ez a helyzet a következők:

- [Csatlakozás SQL Server az azonos felhőszolgáltatásában](#connect-to-sql-server-in-the-same-cloud-service)
- [Csatlakozás SQL Server az interneten](#connect-to-sql-server-over-the-internet)
- [Csatlakozás SQL Server virtuális ugyanabba a hálózatba](#connect-to-sql-server-in-the-same-virtual-network)

>[AZURE.NOTE] Módszerekben való csatlakozás előtt hajtsa végre a [kapcsolat konfigurálásához ez a cikk lépéseit](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

### <a name="connect-to-sql-server-in-the-same-cloud-service"></a>Csatlakozás SQL Server az azonos felhőszolgáltatásában

Az azonos felhőszolgáltatásában több virtuális gépeken futó hozhat létre. Ebben az esetben a virtuális gépeken futó érteni, hogy megtudhatja, [hogy miként kapcsolódás virtuális gépeken futó virtuális hálózati vagy felhőalapú szolgáltatást](virtual-machines-windows-classic-connect-vms.md#connect-vms-in-a-standalone-cloud-service). Ebben az esetben van, amikor az ügyfél egy virtuális gépen ugyanazt a felhőalapú szolgáltatást egy másik virtuális gépen futó SQL-kiszolgálóhoz való csatlakozáshoz.

Ebben az esetben csatlakoztathatja virtuális **nevét** (a **Számítógép neve** vagy **Hostname (állomásnév)** a portálon is látható). Ez a létrehozása során a virtuális megadott név. Például az SQL-virtuális **mysqlvm**elnevezett, ha egy ügyfél virtuális az azonos felhőszolgáltatásában segítségével a következő kapcsolati karakterlánc csatlakozni:

    "Server=mysqlvm;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

### <a name="connect-to-sql-server-over-the-internet"></a>Csatlakozás SQL Server az interneten

Ha az internetről az SQL Server adatbázismotort csatlakozni szeretne, létre kell hoznia egy virtuális gép végpontot bejövő TCP kommunikáció. Azure konfigurációs ebben a lépésben egy olyan portot elérhető a virtuális géphez bejövő TCP-port forgalom irányítja.

Csatlakozás az interneten keresztül, a virtuális DNS-neve és a virtuális végpont portszámot (Ez a cikk későbbi részében konfigurált) kell használnia. Keresse meg a DNS, nyissa meg az Azure-portált, és válassza ki a **virtuális gépeken futó (klasszikus)**. Ezután jelölje ki a virtuális gépet. Az **Áttekintés** szakaszban megjelenik a **DNS-nevét** .

Például érdemes megvizsgálni **mysqlvm** nevű **mysqlvm7777.cloudapp.net** a DNS-neve és a virtuális végpont **57500**klasszikus virtuális géphez. Feltételezve, hogy helyesen konfigurált kapcsolatok, a következő kapcsolati karakterlánc felhasználható a virtuális gép elérése bárhonnan az interneten:

    "Server=mycloudservice.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Bár ez lehetővé teszi, hogy csatlakozási ügyfelek számára az interneten keresztül, ez nem jelenti azt, hogy bárki csatlakozhat az SQL Server. Kívüli ügyfelek kell helyes a felhasználónév és jelszó. A biztonság fokozása nem használja a jól ismert port 1433 a nyilvános virtuális gép végpontot. Ha lehetséges, fontolja-vezérlés és forgalmának korlátozása csak az ügyfelek a végpont teszi lehetővé. Hozzáférés-vezérlési listák használata végpontok, tanulmányozza [a vezérlés végpont kezelése](virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint).

>[AZURE.NOTE] Fontos, hogy vegye figyelembe, hogy ez a módszer is kommunikálhatnak SQL Server, az Azure adatközponthoz kimenő adatainak van fizetnie normál [kimenő adatátvitel vonatkozó árak](https://azure.microsoft.com/pricing/details/data-transfers/)használatakor.

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Csatlakozás SQL Server virtuális ugyanabba a hálózatba

[Virtuális hálózati](../virtual-network/virtual-networks-overview.md) további forgatókönyveket teszi lehetővé. VMs csatlakoztathatja virtuális hálózatához, még akkor is, ha adott VMs különböző felhőszolgáltatások szerepel. És a [webhely VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md)egy hibrid architektúra VMs kapcsolódó hálózatok a helyszíni és gépek hozhat létre.

Virtuális hálózatok is lehetővé teszi az Azure VMs tartományhoz való csatlakozásra. Ez a csak úgy lehet az SQL Server Windows-hitelesítést használ. A más esetben a kapcsolat SQL hitelesítés szükséges, a felhasználóneveket és jelszavakat.

Ha a tartomány és a Windows-hitelesítés konfigurálása fogja, nem kell a cikkben ismertetett lépésekkel hozhatja létre a nyilvános végpont vagy SQL-alapú hitelesítés és bejelentkezések konfigurálása. Ebben az esetben is kapcsolódni lehet az SQL Server-példányt a kapcsolati karakterláncot az SQL Server virtuális nevének megadásával. A következő példa feltételezi, hogy a Windows-hitelesítés is konfigurált és, hogy a felhasználó hozzáférést kapott az SQL Server-példányt szeretne.

    "Server=mysqlvm;Integrated Security=true"

## <a name="steps-for-configuring-sql-server-connectivity-in-an-azure-vm"></a>Az SQL Server-kapcsolat beállítása az Azure virtuális lépései

Az alábbi lépésekkel csatlakozhat az SQL Server-példányt az SQL Server Management Studio (SSMS) interneten keresztüli szemléltetik. Azonban ezeket a lépéseket, hogy az alkalmazások, mind a helyszíni futtatása és Azure-ban elérhető az SQL Server virtuális gép alkalmazni.

Az SQL Server-példány csatlakozhat egy másik virtuális vagy az internetről, mielőtt a az alábbi szakaszokban ismertetett módon az alábbi műveleteket kell elvégeznie:

- [A virtuális gép TCP végpont létrehozása](#create-a-tcp-endpoint-for-the-virtual-machine)
- [Nyissa meg a Windows tűzfal TCP-portok](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [SQL Server-figyelni a TCP protokollt konfigurálása](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [Az SQL Server vegyes módú hitelesítés konfigurálása](#configure-sql-server-for-mixed-mode-authentication)
- [SQL Server-hitelesítés bejelentkezési létrehozása](#create-sql-server-authentication-logins)
- [A virtuális gép DNS nevének megállapításához](#determine-the-dns-name-of-the-virtual-machine)
- [Csatlakozás a adatbázismotor másik számítógépről](#connect-to-the-database-engine-from-another-computer)

Kapcsolat elérési az alábbi ábra szerint van összefoglalva:

![Csatlakozás SQL Server virtuális géphez](../../includes/media/virtual-machines-sql-server-connection-steps/SQLServerinVMConnectionMap.png)

[AZURE.INCLUDE [Connect to SQL Server in a VM Classic TCP Endpoint](../../includes/virtual-machines-sql-server-connection-steps-classic-tcp-endpoint.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Classic Steps](../../includes/virtual-machines-sql-server-connection-steps-classic.md)]

## <a name="next-steps"></a>Következő lépések

Ha is szeretné használni AlwaysOn rendelkezésre állási csoportok magas rendelkezésre állásának és a helyreállítás, fontolja meg figyelő végrehajtása. Adatbázis az ügyfélgépek hogyan kapcsolódnak közvetlenül az egyik az SQL Server-példányok helyett a figyelő. A figyelő ügyfelek az elérhetőség csoportjának elsődleges replika irányítja. További tudnivalókért lásd: a [Configure egy ILB figyelő AlwaysOn elérhetősége csoportok Azure-ban](virtual-machines-windows-classic-ps-sql-int-listener.md).

Fontos áttekintheti a biztonsággal kapcsolatos gyakorlati tanácsok az SQL Server Azure virtuális-gépen futó. További tudnivalókért olvassa el a [Biztonsági megfontolások az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-security.md)című témakört.

[Tallózás a tanulási javaslat](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) az SQL Server Azure virtuális gépeken. 

Futó SQL Server Azure VMs kapcsolódó témakörök [A Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-server-iaas-overview.md)talál.

<properties
    pageTitle="Csatlakozás SQL Server virtuális géphez (erőforrás-kezelő) |} Microsoft Azure"
    description="Megtudhatja, hogy miként csatlakozhat az Azure virtuális gépen futó SQL Server. Ez a témakör a klasszikus telepítési modellt használja. Az esetek különböznek egymástól attól függően, hogy a hálózati konfigurációja, és hol található az ügyfélnek."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"    
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/21/2016"
    ms.author="jroth" />

# <a name="connect-to-a-sql-server-virtual-machine-on-azure-resource-manager"></a>Csatlakozás egy SQL Server virtuális gép Azure (erőforrás-kezelő)

> [AZURE.SELECTOR]
- [Erőforrás-kezelő](virtual-machines-windows-sql-connect.md)
- [Klasszikus](virtual-machines-windows-classic-sql-connect.md)

## <a name="overview"></a>– Áttekintés

Ez a témakör ismerteti, hogyan csatlakozhat az Azure virtuális-gépen futó SQL Server-példányt. Néhány [Általános kapcsolódási esetek](#connection-scenarios) foglalkozik és a majd az [SQL Server-Azure virtuális való csatlakozással konfigurálása a lépések részletes](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm)biztosít.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klasszikus telepítési modell. Ez a cikk klasszikus verzióinak megtekintéséhez, olvassa el a [Csatlakozás az SQL Server virtuális gép a Azure klasszikus](virtual-machines-windows-classic-sql-connect.md).

Ha egy teljes segédlet kiépítési és csatlakozási inkább lenne, nézze meg [virtuális gép SQL Server Azure a kiépítési](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="connection-scenarios"></a>Kapcsolat felhasználási területei

Egy ügyfél csatlakozik egy virtuális gépen futó SQL Server módja eltérő attól függően, hogy az ügyfél és a számítógép/hálózati konfigurációja helyét. Ez a helyzet a következők:

- [Csatlakozás SQL Server az interneten](#connect-to-sql-server-over-the-internet)
- [Csatlakozás SQL Server virtuális ugyanabba a hálózatba](#connect-to-sql-server-in-the-same-virtual-network)

### <a name="connect-to-sql-server-over-the-internet"></a>Csatlakozás SQL Server az interneten

Ha az internetről az SQL Server adatbázismotort csatlakozni szeretne, több lépésből áll szükséges karakterek, például a tűzfal konfigurálása SQL-hitelesítés engedélyezése és beállítása a hálózati biztonsági csoportnak rendelkeznie kell a hálózati biztonsági csoport szabály port 1433 TCP forgalmának engedélyezésére.

A portál hozhatók létre SQL Server-virtuális gép képként, az erőforrás-kezelő használatakor kijelölésekor **nyilvános** SQL kapcsolódási beállítással ezeket a lépéseket végzett meg:

![Nyilvános SQL kapcsolati lehetőséget kiépítési során](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

Ha ez nem volt egy kiépítési alatt, majd manuálisan beállíthatja a virtuális gépeken futó SQL Server és a [kézi beállításához kapcsolódási Ez a cikk lépéseit](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm)követve.

>[AZURE.NOTE] Az SQL Server Express edition virtuális gép elem képével automatikusan nem engedélyezi a TCP/IP protokollt. Express Edition kell használnia az SQL Server Configuration Manager ahhoz, hogy [manuálisan a TCP/IP protokollt](#configure-sql-server-to-listen-on-the-tcp-protocol) a virtuális létrehozása után.

Miután ez megtörtént, bármilyen ügyfél internet-hozzáféréssel rendelkező csatlakozhat az SQL Server-példányt a virtuális gép nyilvános IP-címét, vagy a DNS-címke az IP-címet rendelt megadásával. Ha az SQL Server port 1433, nem kell a kapcsolati karakterláncban adja meg.

    "Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Bár ez lehetővé teszi, hogy csatlakozási ügyfelek számára az interneten keresztül, ez nem jelenti azt, hogy bárki csatlakozhat az SQL Server. Kívüli ügyfelek kell helyes a felhasználónév és jelszó. A biztonság fokozása elkerülheti a jól ismert port 1433. Például ha az SQL Server-port 1500 beállítva, és megfelelő tűzfalat, és a hálózati biztonsági csoport szabályok létrehozott, sikerült csatlakozni, a portszámot hozzáfűzi a kiszolgáló nevét, az alábbi példának megfelelően:

    "Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

>[AZURE.NOTE] Fontos, hogy vegye figyelembe, hogy ez a módszer is kommunikálhatnak SQL Server, az Azure adatközponthoz kimenő adatainak van fizetnie normál [kimenő adatátvitel vonatkozó árak](https://azure.microsoft.com/pricing/details/data-transfers/)használatakor.

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Csatlakozás SQL Server virtuális ugyanabba a hálózatba

[Virtuális hálózati](../virtual-network/virtual-networks-overview.md) további forgatókönyveket teszi lehetővé. VMs csatlakoztathatja virtuális hálózatához, még akkor is, ha adott VMs különböző erőforráscsoport szerepel. És a [webhely VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md)egy hibrid architektúra VMs kapcsolódó hálózatok a helyszíni és gépek hozhat létre.

Virtuális hálózatok is lehetővé teszi az Azure VMs tartományhoz való csatlakozásra. Ez a csak úgy lehet az SQL Server Windows-hitelesítést használ. A más esetben a kapcsolat SQL hitelesítés szükséges, a felhasználóneveket és jelszavakat.

A portál hozhatók létre SQL Server-virtuális gép képként, az erőforrás-kezelő használatakor a megfelelő tűzfalszabályokat kommunikáció a virtuális hálózaton esetén beállítása ki **saját** az SQL-kapcsolati lehetőséget. Ha ez nem volt egy kiépítési alatt, majd manuálisan beállíthatja a virtuális gépeken futó SQL Server és a [kézi beállításához kapcsolódási Ez a cikk lépéseit](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm)követve. De a tartomány és a Windows-hitelesítés konfigurálása szeretné, ha nem kell a cikkben ismertetett lépésekkel hozhatja létre SQL-alapú hitelesítés és bejelentkezések konfigurálása. Is szükségtelen hálózati biztonsági csoport hozzáférési szabályok konfigurálása az interneten keresztül.

>[AZURE.NOTE] Az SQL Server Express edition virtuális gép elem képével automatikusan nem engedélyezi a TCP/IP protokollt. Express Edition kell használnia az SQL Server Configuration Manager ahhoz, hogy [manuálisan a TCP/IP protokollt](#configure-sql-server-to-listen-on-the-tcp-protocol) a virtuális létrehozása után.

Feltételezve, hogy a virtuális hálózata DNS van beállítva, akkor csatlakozhat az SQL Server-példányt az SQL Server virtuális számítógép neve a kapcsolati karakterláncban megadásával. Az alábbi példa is feltételezi, hogy a Windows-hitelesítés is konfigurált és, hogy a felhasználó hozzáférést kapott az SQL Server-példányt.

    "Server=mysqlvm;Integrated Security=true"

Figyelje meg, hogy ebben az esetben is megadhatja a virtuális IP-címét.

## <a name="steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm"></a>Az SQL Server-kapcsolat manuális beállítása az Azure virtuális lépései

Az alábbi lépéseket a manuális beállítás a kapcsolatot az SQL Server-példányt, és tetszés szerint csatlakoztassa az SQL Server Management Studio (SSMS) interneten keresztüli szemléltetik. Figyelje meg, hogy ezeket a lépéseket számos végzett meg, akkor jelölje be az SQL Server csatlakozási beállítások közül a megfelelőket a portálon.

Az SQL Server-példány csatlakozhat egy másik virtuális vagy az internetről, mielőtt a az alábbi szakaszokban ismertetett módon az alábbi műveleteket kell elvégeznie:

- [Nyissa meg a Windows tűzfal TCP-portok](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [SQL Server-figyelni a TCP protokollt konfigurálása](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [Az SQL Server vegyes módú hitelesítés konfigurálása](#configure-sql-server-for-mixed-mode-authentication)
- [SQL Server-hitelesítés bejelentkezési létrehozása](#create-sql-server-authentication-logins)
- [A hálózati biztonsági csoport a bejövő szabályok beállítása](#configure-a-network-security-group-inbound-rule-for-the-vm)
- [A nyilvános IP-cím DNS címke beállítása](#configure-a-dns-label-for-the-public-ip-address)
- [Csatlakozás a adatbázismotor másik számítógépről](#connect-to-the-database-engine-from-another-computer)

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager-nsg-rule.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Következő lépések

Kiépítési kapcsolódási lépések együtt útmutatást talál további [virtuális gép SQL Server Azure a kiépítési](virtual-machines-windows-portal-sql-server-provision.md).

[Tallózás a tanulási javaslat](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) az SQL Server Azure virtuális gépeken.

Futó SQL Server Azure VMs kapcsolódó témakörök [A Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-server-iaas-overview.md)talál.

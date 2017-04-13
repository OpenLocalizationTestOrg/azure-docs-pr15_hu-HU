<properties
    pageTitle="Biztonsági megfontolások az SQL Server Azure-ban |} Microsoft Azure"
    description="Ez a témakör a klasszikus telepítési modell készült erőforrások hivatkozik, és az általános útmutató biztonságossá tehető az SQL Server-Azure virtuális gépen futó."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
   editor=""    
   tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="06/24/2016"
    ms.author="jroth" />

# <a name="security-considerations-for-sql-server-in-azure-virtual-machines"></a>Azure virtuális gépeken futó SQL Server biztonsági kérdései
 
Ez a témakör általános biztonsági útmutatásokat, amelyek meghatározzák az SQL Server-példányok biztonságos hozzáférés az Azure virtuális segítségével. Azonban jobb védelme az SQL Server-adatbázis példányaiban Azure-ban, javasoljuk, hogy az Azure mellett a biztonsággal kapcsolatos gyakorlati tanácsok a hagyományos helyszíni biztonsági tanácsok végrehajtása.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Az SQL Server biztonsági eljárások kapcsolatos további tudnivalókért lásd: [az SQL Server 2008 R2 biztonsággal kapcsolatos gyakorlati tanácsok - műveleti és felügyeleti feladatok](http://download.microsoft.com/download/1/2/A/12ABE102-4427-4335-B989-5DA579A4D29D/SQL_Server_2008_R2_Security_Best_Practice_Whitepaper.docx)

Azure megfeleljenek a több iparágban előírásokat és, amelyekkel már egy virtuális gépen futó SQL Server-kompatibilis megoldást összeállításához szabványoknak. Kérdések a szabályozási megfelelőségről az Azure tudnivalókért lásd: [Azure az Adatvédelmi központ](https://azure.microsoft.com/support/trust-center/).

Az alábbiakban felsoroljuk biztonsági ajánlások kell figyelembe venni, konfigurálásakor és csatlakoztatása az SQL Server-Azure virtuális példányát.

## <a name="considerations-for-managing-accounts"></a>Fiókok kezelése kapcsolatos szempontok

- Hozzon létre egy egyedi helyi rendszergazdafiók, hogy a **rendszergazda**neve nem.

- Összetett erős jelszavak használata az összes fiókhoz. Hogyan hozhat létre olyan erős jelszót, lásd: a [tippek olyan erős jelszavak létrehozása a](http://windows.microsoft.com/en-us/windows-vista/Tips-for-creating-a-strong-password) cikk további információt.

- Alapértelmezés szerint Azure SQL Server virtuális gép a telepítés során kijelöli a Windows-hitelesítés használatával. Ezért a **rendszergazdai** bejelentkezési le van tiltva, és a jelszó beállítása úgy van-e hozzárendelve. Azt a **rendszergazdai** bejelentkezési kell ajánlott nem használt vagy nem engedélyezett. Az alábbiakban alternatív stratégiák egy SQL-bejelentkezés van szükség:
    - Hozzon létre egy rendszergazdák tagsággal rendelkező SQL-fiókkal.
    - **Rendszergazdai** bejelentkezési kell használnia, ha a bejelentkezési engedélyezése, és nevezze át, és hozzárendelése egy új jelszót.
    - Mind a beállításokat, amely a korábban említett előírhatja a hitelesítési módot **az SQL Server**és a Windows-hitelesítési módban. További tudnivalókért olvassa el a [Kiszolgálói hitelesítési mód váltása](https://msdn.microsoft.com/library/ms188670.aspx)című témakört.

## <a name="considerations-for-securing-connections-to-azure-virtual-machine"></a>Azure virtuális géphez kapcsolatok biztonságossá tétele kapcsolatos szempontok

- Célszerű lehet felügyelni a virtuális gépeken futó helyett nyilvános RDP-portok [Azure virtuális hálózati](../virtual-network/virtual-networks-overview.md) használatával.

- [Hálózati biztonsági csoport](../virtual-network/virtual-networks-nsg.md) (NSG) használatával lehetővé teszi, vagy megtagadja a hálózati forgalmat a virtuális géphez. Ha szeretne egy NSG használja, és már zárólap vezérlés már a helyükön vannak, először távolítsa el az vezérlés végpontot. Ehhez olvashat olvassa el a [Kezelése Access (-szabályozási listák) megismerkedhet a PowerShell használatával végpontok](../virtual-network/virtual-networks-acl-powershell.md)című témakört.

- Végpontok használatakor bármely végpontja a virtuális gépen eltávolítani, ha nem használja őket. Hozzáférés-vezérlési listák használata végpontok, tanulmányozza [a vezérlés végpont kezelése](../virtual-network/virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint).

- Az SQL Server adatbázismotor az Azure virtuális gépeken futó egy példányának engedélyezése egy titkosított kapcsolat lehetőséget. Állítsa be az SQL server-példányt aláírt tanúsítvány. További tudnivalókért lásd: a [Titkosított kapcsolatok engedélyezése a adatbázismotor](https://msdn.microsoft.com/library/ms191192.aspx) és a [Kapcsolati karakterlánc szintaxisát](https://msdn.microsoft.com/library/ms254500.aspx).

- A virtuális gépeken futó kell érhető el, csak egy adott hálózatról, ha bizonyos IP-címek vagy hálózati alhálózat való hozzáférés korlátozása a Windows tűzfal segítségével.

## <a name="next-steps"></a>Következő lépések

Ha érdeklik is ajánlott eljárások megalkotása a teljesítményt, olvassa el a [Teljesítmény gyakorlati tanácsok az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-performance.md)című témakört.

Futó SQL Server Azure VMs kapcsolódó témakörök olvassa el a [SQL Server Azure virtuális gépeken futó – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md)című témakört.

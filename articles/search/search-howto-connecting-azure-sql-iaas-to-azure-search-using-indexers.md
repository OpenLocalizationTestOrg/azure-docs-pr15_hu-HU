<properties 
    pageTitle="Az SQL Server Azure keresési indexkiszolgáló a kapcsolat beállítása az Azure virtuális gépen |} Microsoft Azure |} Indexelő" 
    description="Titkosított kapcsolat engedélyezése és beállítása a kapcsolatok engedélyezésére SQL Server-Azure virtuális gépen (virtuális) az Azure Search indexkiszolgáló a tűzfalat." 
    services="search" 
    documentationCenter="" 
    authors="jack4it" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="09/26/2016" 
    ms.author="jackma"/>

# <a name="configure-a-connection-from-an-azure-search-indexer-to-sql-server-on-an-azure-vm"></a>Az SQL Server Azure keresési indexkiszolgáló a kapcsolat beállítása a az Azure virtuális

[Csatlakozás Azure SQL-adatbázis használata az indexelő Azure keresés](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md#frequently-asked-questions)amint Azure keresési által támogatott **SQL Server Azure VMs** (vagy **SQL Azure-VMs** rövid) elleni indexelő létrehozása, de van néhány biztonsággal kapcsolatos előfeltételek első gondoskodik. 

**a tevékenység időtartama:** 30 perc, a virtuális tanúsítvány feltételezve, már telepítve.

## <a name="enable-encrypted-connections"></a>Titkosított kapcsolat engedélyezése

Azure keresési titkosított csatorna szükséges összes indexelő kérelem nyilvános internet-kapcsolaton keresztül. Ez a szakasz lépéseit, hogy mindez működjön sorolja fel.

1. Jelölje be a tulajdonságok a tanúsítvány ellenőrzéséhez tulajdonosának neve a Azure virtuális teljes tartománynevét (FQDN). Egy CertUtils például, vagy a tanúsítványok beépülő modul segítségével tulajdonságainak megtekintése. A teljesen minősített tartománynév szakaszából a virtuális szolgáltatás lap Essentials **nyilvános IP-cím/DNS címke neve** mezőben az [Azure portál](https://portal.azure.com/)elérheti.

    - Az **Erőforrás-kezelő** újabb sablonnal létrehozott VMs, a teljesen minősített tartománynév van formázva `<your-VM-name>.<region>.cloudapp.azure.com`. 

    - A program létrehoz egy **Klasszikus** virtuális régebbi VMs, a teljesen minősített tartománynév van formázva `<your-cloud-service-name.cloudapp.net>`. 

2. Állítsa be az SQL Server használatával a Beállításszerkesztő (regedit) tanúsítvány használatát. 

    Bár az SQL Server Configuration Manager gyakran használják ezt a feladatot, az ebben az esetben nem használható. Az importált tanúsítvány meg nem találja, mert a teljes Tartománynevét az Azure virtuális gép nem egyezik meg a virtuális (azonosítja a tartomány vagy a helyi számítógépen, vagy a hálózati tartományt, amelyhez csatlakozott) által meghatározott teljes Tartománynevét. Nevek nem egyeznek meg, amikor a regedit segítségével adja meg azt a tanúsítványt.

    - A regedit parancsot, keresse meg azt az alábbi beállításkulcs: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\[MSSQL13.MSSQLSERVER]\MSSQLServer\SuperSocketNetLib\Certificate`.
     
    A `[MSSQL13.MSSQLSERVER]` rész verzió és a példány függ. 

    - A **tanúsítvány** kulcs értékét állítsa az SSL-tanúsítvány a virtuális importált **ujjlenyomat** .

    Nincsenek több lehetőség közül választhat az ujjlenyomatot, néhány jobban, mint másoknak. A **tanúsítványok** beépülő modulból MMC másolni, ha, fog valószínűleg vegyen fel egy kezdő karakter [támogatási cikkben ismertetett módon](https://support.microsoft.com/kb/2023869/), amely hibát eredményez, amikor megpróbálja kapcsolat láthatatlan. Több lehetséges megoldások léteznek javítása a probléma megoldásához. A legegyszerűbb, hogy a backspace fölé, és a megerősítéshez írja be az első karakterének helye a ujjlenyomat szeretné eltávolítani, az első karakter kulcsmező a regedit parancsot. Egy másik eszközt használni azt is megteheti, másolja a vágólapra az ujjlenyomatot.

3. A szolgáltatás fiókjához engedélyek kiosztása. 

    Győződjön meg arról, hogy az SQL Server-szolgáltatási fiók engedélyt megfelelő titkos kulcs az SSL-tanúsítvány. Ha ezt a lépést, eltéveszthetők, SQL Server nem indul el. Ehhez a feladathoz használhatja a **tanúsítványok** beépülő modul vagy **CertUtils** .

4. Indítsa újra az SQL Server szolgáltatást.

## <a name="configure-sql-server-connectivity-in-the-vm"></a>A virtuális az SQL Server-kapcsolatok konfigurálása

A titkosított kapcsolatot követel meg az Azure keresés beállítása után nincsenek további beállítási lépéseket az SQL Server Azure VMs belső. Ha még nem tette meg, következő lépésként befejezéséhez konfigurációval, ezek a cikkek egyikét:

- **Erőforrás-kezelő** virtuális olvassa el a [Csatlakozás az SQL Server virtuális gép a Azure erőforrás-kezelő használatával](../virtual-machines/virtual-machines-windows-sql-connect.md). 

- A **Klasszikus** virtuális olvassa el a [Csatlakozás az SQL Server virtuális gép a Azure klasszikus](../virtual-machines/virtual-machines-windows-classic-sql-connect.md).

Különösen olvassa el az egyes cikk "az interneten keresztül kapcsolódó" című szakasza.

## <a name="configure-the-network-security-group-nsg"></a>A hálózati biztonsági csoport (NSG) beállítása

Még nem a NSG és a megfelelő Azure végpont vagy beállítása Access vezérlőelem-lista (vezérlés) más feleknek megkönnyítése a Azure virtuális szokatlan. De elemeket ez előtt a saját alkalmazások logika csatlakozhat az SQL Azure virtuális engedélyezése valószínűleg. Nem különbözik, az SQL Azure virtuális az Azure keresési kapcsolatot. 

Az alábbi hivatkozásokat NSG konfigurálásról virtuális telepítési útmutatást. Ezeket az utasításokat követve Azure keresési zárólap IP-címének alapuló vezérlés használja.

> [AZURE.NOTE] Hátteréről [Mi az hálózati biztonsági csoport?](../virtual-network/virtual-networks-nsg.md)

- Az **Erőforrás-kezelő** virtuális [létrehozásának](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)módját ARM telepítésekhez NSGs. 

- A **Klasszikus** virtuális megtudhatja, [hogy miként létrehozása klasszikus telepítésekhez NSGs](../virtual-network/virtual-networks-create-nsg-classic-ps.md).

IP-címek jelenthet néhány kihívásokkal kapcsolatban, egyszerűen megoldása a Ha tartsa szem előtt, a kibocsátás és a lehetséges megoldásokat alkalmazhatja. A fennmaradó szakaszokban javaslatok problémák megoldásához IP-címek a vezérlés kezelésére.

#### <a name="restrict-access-to-the-search-service-ip-address"></a>A keresési szolgáltatás IP-cím való hozzáférés korlátozása

Nyomatékosan javasoljuk, hogy korlátozza-e az access a vezérlés minden kapcsolat kérelmek széles nyissa meg az SQL Azure-VMs tétele helyett a keresési szolgáltatásának IP-címét. Egyszerűen láthatja, hogy az IP-cím által pingelés teljes Tartománynevét (például `<your-search-service-name>.search.windows.net`) a keresési szolgáltatás.

#### <a name="managing-ip-address-fluctuations"></a>IP-cím ingadozások kezelése

Ha a keresési szolgáltatás csak egy keresési egység (Ez azt jelenti, hogy több kópia és egy partíciót) van, az IP-cím változik, a keresési szolgáltatás IP-címet egy meglévő vezérlés érvénytelenítsék rutinszerű szolgáltatás az újraindítás során.

A későbbi kapcsolódási hibák elkerülése érdekében egy úgy, hogy egynél több kópia és egy partíciót Azure keresés használata. Ha így nő a költség, de azt is megoldja a IP-cím problémát. Azure keresés IP-címek nem változik, ha egynél több keresési egységhez van.

A második megközelítés, ha engedélyezi a csatlakozást sikertelen, és konfigurálja a NSG a hozzáférés-vezérlési listák újra. Átlagosan is a várt IP-címek és néhány hetenként módosítása. Az ügyfelek, akik ellenőrzött indexelés ritka kombinálásával esetében ezt a megközelítést életképes lehet.

A harmadik életképes (de nem kifejezetten biztonságos) megközelítés, megadhatja az IP-címtartományokat az Azure terület, ahol a keresési szolgáltatás már kiépítve. IP-tartományokat, amelyből nyilvános IP-címek rendelt Azure erőforrások listája [Azure adatközpont IP-címtartományai](https://www.microsoft.com/download/details.aspx?id=41653)van közzétéve. 

#### <a name="include-the-azure-search-portal-ip-addresses"></a>Az Azure keresési portál IP-címeket tartalmazza.

Indexkiszolgáló létrehozása az Azure portal szolgáltatást használ, ha Azure keresési portál logika is hozzáférésre van szüksége az SQL Azure virtuális létrehozási idejének során. Azure keresési portál IP-címek található pingelése által `stamp2.search.ext.azure.com`.

## <a name="next-steps"></a>Következő lépések

Beállításokkal az útból most már a adhatja meg az SQL Server Azure virtuális Azure keresési indexkiszolgáló adatforrásként. [Csatlakozás Azure SQL-adatbázis használata az indexelő Azure keresés](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md) talál további információt.

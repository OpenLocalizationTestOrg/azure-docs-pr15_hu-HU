<properties
    pageTitle="Azure virtuális – klasszikus mindig rendelkezésre állási csoport beállítása"
    description="Állandó elérhetősége-ös csoport létrehozása az Azure virtuális gépeken futó. Ebben az oktatóanyagban elsősorban a felhasználói felület és eszközöket használ, nem pedig a parancsfájlok."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="mikeray" />

# <a name="configure-always-on-availability-group-in-azure-vm---classic"></a>Azure virtuális – klasszikus mindig rendelkezésre állási csoport beállítása

> [AZURE.SELECTOR]
- [Erőforrás-kezelő: sablon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Erőforrás-kezelő: kézi](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Klasszikus: felhasználói felület](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Klasszikus: PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

> [AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Ebben az oktatóanyagban végpontok közötti bemutatja, hogyan használata SQL Server mindig Azure virtuális gépeken futó rendelkezésre állási csoportok végrehajtásához.

Az oktatóanyag végén az SQL Server mindig a megoldást Azure-ban a következő elemekből áll:

- Több alhálózat, beleértve a előtér és a háttér-alhálózat tartalmazó virtuális hálózaton

- A tartományvezérlőnek Active Directory (AD) tartománnyal

- Két SQL Server VMs a háttéradatbázist az alhálózathoz rendszerbe, és az Active Directory tartományhoz

- A 3-csomópont WSFC fürtre csomópont többsége kvórum modell

- Az elérhetőség adatbázis két szinkron jóváhagyás kópiákkal egy rendelkezésre állási csoport

Az alábbi ábrán a megoldást grafikus ábrázolása.

![Teszt labor architektúra AG Azure-ban](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC791912.png)

Figyelje meg, hogy ez a beállítás egy lehetséges. Két-replika elérhetősége csoport VMs számát méretűre például annak érdekében, hogy a tartományvezérlőnek a kvórum tanúsító fájlmegosztás a 2-csomópont WSFC fürt használatával számítási óra Azure-ban a mentés. Ez a módszer virtuális számának csökkenti a fenti konfiguráció egyet.

Ebben az oktatóanyagban feltételezi, hogy a következőket:

- Már rendelkezik egy Azure-fiók.

- A virtuális gép gyűjteményből a felhasználói felülettel klasszikus SQL Server virtuális kiépítését már tudja.

- Már rendelkezik egy mindig a elérhetőség csoportok tömör megértése. További tudnivalókért lásd: [Mindig a elérhetősége csoportok (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

>[AZURE.NOTE] Ha érdeklik a csoportokkal mindig a elérhetőségét a SharePointtal, is megjelenik a [Konfigurálása SQL Server 2012 mindig tovább rendelkezésre állási csoportok a SharePoint 2013-hoz](https://technet.microsoft.com/library/jj715261.aspx).

## <a name="create-the-virtual-network-and-domain-controller-server"></a>A virtuális hálózati és a tartomány vezérlő kiszolgáló létrehozása

Új Azure próbaverzió fiókkal kell kezdeni. Amikor befejezte a fiók beállítása, akkor a kezdőképernyő közötti különbség az Azure klasszikus portál kell lennie.

1. Az **Új** gombra a lap bal alsó sarkában alább látható módon.

    ![Kattintson az új a portálon](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665511.gif)

1. **Hálózati szolgáltatások**, majd kattintson a **Virtuális hálózat,** gombmenü **Egyéni létrehozása**, alább látható módon.

    ![Virtuális hálózat létrehozása](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665512.gif)

1. **Virtuális hálózati létrehozása** párbeszédpanelen hozzon létre egy új virtuális hálózati szerint az oldalak között az alábbi beállításokkal verziószámú. 

  	|Lap|Beállítások|
|---|---|
|Virtuális hálózati részletei|**NÉV = ContosoNET**<br/>**RÉGIÓ = US nyugati**|
|A DNS-kiszolgálók és a virtuális Magánhálózati kapcsolat|Nincs lehetőség|
|Virtuális hálózati cím szóköz|Az alábbi képernyőképen látható beállítások: ![Virtuális hálózat létrehozása](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784620.png)|

1. Ezután hozzon létre a virtuális, akkor használja a tartományvezérlőnek (Adatközpont). Kattintson ismét **Új** , majd **számítja ki**, majd **virtuális gép**, majd kattintson **A gyűjtemény**alább látható módon.

    ![Hozzon létre egy virtuális](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784621.png)

1. **A virtuális gép létrehozása** párbeszédpanelen állítsa be egy új virtuális szerint az oldalak között az alábbi beállításokkal verziószámú. 

  	|Lap|Beállítások|
|---|---|
|Jelölje ki a virtuális gép operációs rendszer|A Windows Server 2012 R2 adatközponthoz|
|Virtuális gép konfigurálása|**Kiadás verziója dátuma** = (legújabb)<br/>**Virtuális számítógépnév** = ContosoDC<br/>**RÉTEG** = STANDARD<br/>**Méret** = A2 (2 magmintákat)<br/>**Új felhasználó nevét** = AzureAdmin<br/>**Új jelszó** = Contoso! 000<br/>**Megerősítés** = Contoso! 000|
|Virtuális gép konfigurálása|**FELHŐBELI SZOLGÁLTATÁSTÓL** = egy új felhőszolgáltatásba létrehozása<br/>**FELHŐALAPÚ szolgáltatás tartománynév** = egy egyedi felhőalapú szolgáltatás neve<br/>**DNS-név** = egy egyedi nevet (ex: ContosoDC123)<br/>**Régió/AFFINITÁS csoport/virtuális hálózati** = ContosoNET<br/>**Virtuális hálózati ALHÁLÓZAT** = Back(10.10.2.0/24)<br/>**TÁRTERÜLET-fiók** = az automatikusan generált tárterület-fiók használata<br/>**Elérhetőség beállítása** = (nincs)|
|Virtuális gép beállításai|Alapértelmezett értékek használata|

Miután befejezte az új virtuális beállítását, megvárja, amíg a virtuális provsioned lesz. Ezt a folyamatot befejezéséhez egy kis időt vesz igénybe, és az Azure klasszikus portált a **virtuális gép** fülre kattint, láthatja a ContosoDC ciklus államok a **Kezdő (létesítése)** **Leállítva**, **indítása**, **futó (létesítése)**, és végül **lépéseket**.

Az Adatközpont kiszolgáló sikeresen most már kiépítve. Ezután az Active Directory tartományi fog konfigurálni a Adatközpont kiszolgálón.

## <a name="configure-the-domain-controller"></a>A tartományvezérlőnek konfigurálása

Az alábbi lépéseket a ContosoDC gép konfigurál vallalat.kontraktor.hu tartományvezérlőnek.

1. Jelölje ki a **ContosoDC** gép a portálon. Az **Irányítópult** lapon kattintson a **Csatlakozás** távoli asztali Access-RDP-fájl megnyitása parancsra.

    ![Csatlakozás Vritual géphez](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784622.png)

1. Jelentkezzen be a beállított rendszergazdai fiók (**\AzureAdmin**) és jelszavával (**Contoso! 000**).

1. Alapértelmezés szerint a **Kiszolgáló Manager** irányítópultjával lehet megjeleníteni.

1. Kattintson a **Hozzáadás szerepkörök és szolgáltatások** hivatkozásra az irányítópulton.

    ![Kiszolgáló Explorer szerepkörök hozzáadása](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784623.png)

1. Kattintson a **Tovább gombra** , amíg el nem a **Kiszolgálói szerepkörök** területen.

1. Jelölje ki az **Active Directory tartományi szolgáltatások** és a **DNS-kiszolgáló** szerepkörök. Amikor a rendszer kéri, adja hozzá bármilyen további funkciókkal követel meg ezeket a szerepköröket.

    >[AZURE.NOTE] Egy adatérvényesítési figyelmeztetés, amely nincs statikus IP-cím jelenik meg. Ha a konfiguráció tesztelése esetén kattintson a Tovább gombra. Gyártási esetek [a statikus IP-cím, a tartomány vezérlő gép beállítása PowerShell-lel](./virtual-network/virtual-networks-reserved-private-ip.md).

    ![Adja hozzá a szerepkörök párbeszédpanel](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784624.png)

1. Kattintson a **Tovább gombra** , amíg el nem éri a **megerősítést kérő** szakaszban. Jelölje be a **automatikusan, ha szükséges, indítsa újra a célként megadott kiszolgáló** jelölőnégyzetet.

1. Kattintson a **telepítés**gombra.

1. Után a szolgáltatások telepítésével, lépjen vissza az **Kiszolgálókezelő** irányítópulton.

1. Válassza a bal oldali ablaktáblában kattintson az új **Active Directory tartományi szolgáltatások** lehetőséget.

1. Kattintson a sárga figyelmeztetés sávon a **több** hivatkozásra.

    ![Active Directory Tartományi párbeszédpanel a DNS Server virtuális](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784625.png)

1. Tartozó **művelet** oszlopban az **Összes kiszolgálón tevékenység adatai** párbeszédpanel, kattintson az **Előléptetés a tartományvezérlőnek a kiszolgáló**.

1. Az **Active Directory tartományi szolgáltatások konfigurációs varázslója**a következő értékeket használja:

  	|Lap|A beállítás|
|---|---|
|Telepítés konfigurálása|**Hozzáadás új erdő** kijelölt =<br/>**Legfelső szintű tartománynév** vallalat.kontraktor.hu =|
|Tartomány vezérlő beállítások|**Jelszó** = Contoso! 000<br/>**Jelszó megerősítése** = Contoso! 000|

1. Kattintson a **Tovább gombra** a varázsló többi lapjával folyamatát. A **Előfeltételek ellenőrzése** lapon ellenőrizze, hogy a következő üzenet jelenik meg: **minden előfeltétel ellenőrzés átadott sikeresen**. Figyelje meg, hogy minden esetben figyelmeztető üzenetek érdemes áttekintenie, de a telepítés folytatásához lehetséges.

1. Kattintson a **telepítés**gombra. A **ContosoDC** virtuális gép automatikusan újraindul.

## <a name="configure-domain-accounts"></a>Tartományi fiókok konfigurálása

A következő lépésekkel konfigurálása az Active Directory (AD) fiókok későbbi felhasználás céljából.

1. Jelentkezzen be az **ContosoDC** számítógépre.

1. A **Kiszolgálókezelő** válassza az **eszközök pontot** , és kattintson az **Active Directory felügyeleti központ**gombra.

    ![Active Directory felügyeleti központ](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784626.png)

1. A az **Active Directory felügyeleti központ** választó **corp (helyi)** a bal oldali ablaktáblából.

1. A **feladatok** jobb oldali ablaktáblában válassza az **Új** , és válassza a **felhasználó**. Használja az alábbi beállításokat:

  	|A beállítás|Érték|
|---|---|
|**Utónév**|Telepítés|
|**Felhasználói SamAccountName**|Telepítés|
|**Jelszó**|A Contoso! 000|
|**Jelszó megerősítése**|A Contoso! 000|
|**Más jelszóbeállításokat**|Kijelölt|
|**Jelszó sohasem jár le**|Jelölőnégyzet|

1. Kattintson az **OK gombra** a **telepítés** felhasználó létrehozása. Ez a fiók konfigurálása a Feladatátvevőfürt és az elérhetőség csoport lesz.

1. Hozzon létre két további felhasználók ezeket a lépéseket a: **CORP\SQLSvc1** és **CORP\SQLSvc2**. Ezekben a fiókokban az SQL Server-példányok használandó. Ezután meg kell adnia **CORP\Install** a Windows szolgáltatás feladatátvevő fürtözés (WSFC) beállításához szükséges engedélyekkel.

1. Az **Active Directory felügyeleti központ** **corp (helyi)** , a bal oldali ablaktáblában válassza. A **feladatok** jobb oldali ablaktáblában válassza a **Tulajdonságok parancsot**.

    ![CORP felhasználói tulajdonságai](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784627.png)

1. Jelölje ki a **bővítményeket**, és kattintson a **Biztonság** lapon a **Speciális** gombra.

1. A **corp speciális biztonsági beállítások** párbeszédpanel. Kattintson a **hozzáadása**gombra.

1. **A rendszerbiztonsági tag kiválasztása**elemre. Keresse meg a **CORP\Install**. Kattintson az **OK gombra**.

1. Jelölje ki az **összes tulajdonság olvasása** és a **számítógép létrehozása objektumok** engedélyeket.

    ![Corp felhasználói engedélyek](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784628.png)

1. Kattintson az **OK gombra**, és kattintson ismét az **OK gombra** . A corp tulajdonságok ablak bezárásához.

Most, hogy végzett konfigurálása az Active Directory és a felhasználó objektumok, hozzon létre három SQL Server VMs, és vegye fel őket a tartományba.

## <a name="create-the-sql-server-vms"></a>Az SQL Server VMs létrehozása

Ezután hozzon létre három VMs, beleértve a WSFC csomópont és két SQL Server VMs. Minden a VMs létrehozása, térjen vissza az Azure klasszikus portált, kattintson **Új**, **számítja ki**, **virtuális gép**, majd kattintson **A gyűjtemény**. Kattintson a sablonok használatával az alábbi táblázat a VMs létrehozásához segítséget.

|Lap|VM1|VM2|VM3|
|---|---|---|---|
|Jelölje ki a virtuális gép operációs rendszer|**A Windows Server 2012 R2 adatközponthoz**|**Az SQL Server 2014-es RTM Enterprise**|**Az SQL Server 2014-es RTM Enterprise**|
|Virtuális gép konfigurálása|**Kiadás verziója dátuma** = (legújabb)<br/>**Virtuális számítógépnév** = ContosoWSFCNode<br/>**RÉTEG** = STANDARD<br/>**Méret** = A2 (2 magmintákat)<br/>**Új felhasználó nevét** = AzureAdmin<br/>**Új jelszó** = Contoso! 000<br/>**Megerősítés** = Contoso! 000|**Kiadás verziója dátuma** = (legújabb)<br/>**Virtuális számítógépnév** = ContosoSQL1<br/>**RÉTEG** = STANDARD<br/>**Méret** = A3 (4 magmintákat)<br/>**Új felhasználó nevét** = AzureAdmin<br/>**Új jelszó** = Contoso! 000<br/>**Megerősítés** = Contoso! 000|**Kiadás verziója dátuma** = (legújabb)<br/>**Virtuális számítógépnév** = ContosoSQL2<br/>**RÉTEG** = STANDARD<br/>**Méret** = A3 (4 magmintákat)<br/>**Új felhasználó nevét** = AzureAdmin<br/>**Új jelszó** = Contoso! 000<br/>**Megerősítés** = Contoso! 000|
|Virtuális gép konfigurálása|**FELHŐBELI SZOLGÁLTATÁSTÓL** = korábban létrehozott egyedi felhőalapú szolgáltatás DNS-neve (ex: ContosoDC123)<br/>**Régió/AFFINITÁS csoport/virtuális hálózati** = ContosoNET<br/>**Virtuális hálózati ALHÁLÓZAT** = Back(10.10.2.0/24)<br/>**TÁRTERÜLET-fiók** = az automatikusan generált tárterület-fiók használata<br/>**Elérhetőség beállítása** létrehozása az elérhetőségét = beállítása<br/>**Elérhetőség SET NAME** = SQLHADR|**FELHŐBELI SZOLGÁLTATÁSTÓL** = korábban létrehozott egyedi felhőalapú szolgáltatás DNS-neve (ex: ContosoDC123)<br/>**Régió/AFFINITÁS csoport/virtuális hálózati** = ContosoNET<br/>**Virtuális hálózati ALHÁLÓZAT** = Back(10.10.2.0/24)<br/>**TÁRTERÜLET-fiók** = az automatikusan generált tárterület-fiók használata<br/>**Elérhetőség beállítása** = SQLHADR (is beállíthatja a gép létrehozása után állítsa elérhetőségét. Az összes három gépek kell-e kiosztani SQLHADR elérhetősége készletébe.)|**FELHŐBELI SZOLGÁLTATÁSTÓL** = korábban létrehozott egyedi felhőalapú szolgáltatás DNS-neve (ex: ContosoDC123)<br/>**Régió/AFFINITÁS csoport/virtuális hálózati** = ContosoNET<br/>**Virtuális hálózati ALHÁLÓZAT** = Back(10.10.2.0/24)<br/>**TÁRTERÜLET-fiók** = az automatikusan generált tárterület-fiók használata<br/>**Elérhetőség beállítása** = SQLHADR (is beállíthatja a gép létrehozása után állítsa elérhetőségét. Az összes három gépek kell-e kiosztani SQLHADR elérhetősége készletébe.)|
|Virtuális gép beállításai|Alapértelmezett értékek használata|Alapértelmezett értékek használata|Alapértelmezett értékek használata|

<br/>

>[AZURE.NOTE] Korábbi beállításainak szabványos réteg virtuális gépeken futó, javításukhoz javasolt, mert egyszerű réteg gépek nem támogatják a terheléselosztás végpontok egy rendelkezésre állási csoport hallgatók később létrehozásához szükséges. A gép méretű javasolt itt is, az Azure VMs rendelkezésre állási csoportok teszteléshez van kialakítva. A legjobb teljesítmény elérése érdekében a termelési munkaterhelésekből olvassa el a az SQL Server gépi méretét és a [Teljesítmény gyakorlati tanácsok az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-performance.md)-konfigurálás javaslatok című témakört.

Miután teljesen kiépített környezetet nyújt a három VMs, szeretne vegye fel őket a **vallalat.kontraktor.hu** tartományba és a gép CORP\Install rendszergazdai engedélyek. Ehhez az egyes a három VMs kövesse az alábbi lépéseket.

1. Első lépésként módosítsa a használni kívánt DNS-kiszolgáló címét. Kezdje azzal, hogy letölti-e minden virtuális távoli asztali (RDP) fájlt a helyi könyvtár, jelölje ki a virtuális a listában, és kattintson a **Csatlakozás** gombra. Jelölje ki a virtuális, kattintson az első cellát a sorban, de alább látható módon.

    ![Töltse le a RDP-fájlt](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664953.jpg)

1. Indítsa el a letöltött RDP-fájlt, és jelentkezzen be a virtuális, használja a beállított rendszergazdai fiók (**BUILTIN\AzureAdmin**) és jelszavával (**Contoso! 000**).

1. Miután bejelentkezett, meg kell jelennie a **Kiszolgálókezelő** irányítópult. A bal oldali ablaktáblában kattintson a **Helyi kiszolgálóra** .

1. Jelölje ki a **IPv4-cím DHCP, IPv6 engedélyezve által megadott** hivatkozást.

1. A **Hálózati kapcsolat** ablakban válassza ki a hálózat ikonjára.

    ![A virtuális előnyben részesített DNS-kiszolgáló módosítása](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784629.png)

1. A parancssávon, kattintson **a kapcsolat beállításainak módosítása** (attól függően, hogy az ablak méretének, lehet, ha a dupla jobb nyílra kattintva ez a parancs látható).

1. Jelölje ki az **Internet Protocol 4-es verziója (TCP/IPv4)** , és válassza a Tulajdonságok parancsot.

1. Jelölje be a következő DNS-kiszolgáló címeket, és adja meg a **10.10.2.4** az **előnyben részesített DNS-kiszolgáló**.

1. A cím **10.10.2.4** a címet egy virtuális az Azure virtuális hálózatban 10.10.2.0/24 alhálózat rendelt, hogy virtuális pedig **ContosoDC**. Annak ellenőrzéséhez, **ContosoDC**IP-címet, használja az **nslookup contosodc** a parancssorba, alább látható módon.

    ![Adatközpont IP-címének NSLOOKUP használatával](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664954.jpg)

1. Kattintson a O**K** , majd **bezárása gombra** a módosítások véglegesítése gombra. Most már tud csatlakozni a virtuális **vallalat.kontraktor.hu**szeretne.

1. Vissza a **Helyi kiszolgáló** ablakában kattintson a **munkacsoport** hivatkozásra.

1. A **Számítógép neve** csoportban kattintson a **Módosítás**gombra.

1. Jelölje be a **tartomány** jelölőnégyzetet, és írja be a **vallalat.kontraktor.hu** a szövegmezőbe. Kattintson az **OK gombra**.

1. A **Windows biztonsági** előugró párbeszédpanel, adja meg az alapértelmezett tartomány rendszergazdai fiók (**CORP\AzureAdmin**) és a jelszó hitelesítő adatait (**Contoso! 000**).

1. Amikor megjelenik a "Üdvözöljük vallalat.kontraktor.hu tartomány" üzenet, kattintson az **OK gombra**.

1. Kattintson a **Bezárás**gombra, és kattintson az előugró panelen kattintson az **Újraindítás** gombra.

### <a name="add-the-corpinstall-user-as-an-administrator-on-each-vm"></a>Adja hozzá a Corp\Install felhasználó minden virtuális rendszergazdaként:

1. A virtuális újraindításáig várja meg, majd indítsa el újra a RDP-fájl jelentkezzen be a virtuális **BUILTIN\AzureAdmin** -fiókkal.

1. A **Kiszolgálókezelő** válassza az **eszközök pontot**, és kattintson a **Számítógép-kezelés**.

    ![A számítógép-kezelés](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784630.png)

1. A **Számítógép-kezelés** ablakban bontsa ki a **helyi felhasználók és csoportok**lehetőséget, és válassza a **csoportok**.

1. Kattintson duplán a **rendszergazdák** csoportba.

1. A **Rendszergazdák tulajdonságok** párbeszédpanelen kattintson a **Hozzáadás** gombra.

1. Adja meg a felhasználó **CORP\Install**, és kattintson **az OK**gombra. A hitelesítő adatait kérő használja a **AzureAdmin** fiókot a **Contoso! 000** jelszót.

1. Kattintson az **OK gombra** a **Rendszergazda tulajdonságai** párbeszédpanel bezárásához.

### <a name="add-the-failover-clustering-feature-to-each-vm"></a>A **Feladatátvételét** szolgáltatás hozzáadása az egyes virtuális.

1. A **Kiszolgálókezelő** irányítópult kattintson a **Hozzáadás szerepkörök és szolgáltatások**elemre.

1. A **szerepkör hozzáadása és szolgáltatások varázslót**kattintson a **Tovább** amíg el nem a **szolgáltatások** lapon.

1. Jelölje ki a **feladatátvételét**. Amikor a rendszer kéri, adja hozzá minden függő szolgáltatást.

    ![Szolgáltatás a virtuális fürt feladatátvevő hozzáadása](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784631.png)

1. Kattintson a **Tovább**gombra, és kattintson a **telepítés** a **Megerősítés** lapon.

1. A szolgáltatás **Feladatátvételét** telepítés befejezése után kattintson a **Bezárás**gombra.

1. Jelentkezzen ki a virtuális.

1. Ismételje meg a lépéseket, ebben a részben mindhárom kiszolgáló – **ContosoWSFCNode** **ContosoSQL1**és **ContosoSQL2**.

Az SQL Server VMs most kiépített környezetet és fut, de vannak telepítve SQL Server-alapértelmezett beállításokkal.

## <a name="create-the-wsfc-cluster"></a>A WSFC fürt létrehozása

Ebben a részben hoz létre a WSFC fürt ugyanis az elérhetőség csoport később hoz létre. Most, amelyet elkészült kell az alábbiak szerint használhatja a WSFC fürt a három VMs mindegyike:

- Teljesen kiépítéstől Azure-ban

- A tartomány illesztett virtuális

- A helyi Rendszergazdák csoport hozzáadott **CORP\Install**

- A feladatátvételét szolgáltatás hozzáadása

A minden virtuális Előfeltételek mind olyan, a WSFC fürthöz bekapcsolódás előtt.

Emellett látható, hogy az Azure virtuális hálózati verziókban nem ugyanúgy, mint egy helyszíni hálózaton. Meg kell hozza létre a következő sorrendben:

1. Hozzon létre egy egyetlen csomópontot fürt (**ContosoSQL1**) csomópontok egyik.

1. Módosítsa a nem használt IP-cím (**10.10.2.101**) fürt IP-címet.

1. Jelenítse meg a fürt nevének online.

1. Adja hozzá a csomópontok (**ContosoSQL2** és **ContosoWSFCNode**).

Kövesse ezeket a feladatokat, amely teljesen beállítja a fürt elvégezheti az alábbi lépéseket.

1. Indítsa el az RDP-fájlt az **ContosoSQL1** , és jelentkezzen be a tartományi fiók **CORP\Install**.

1. A **Kiszolgálókezelő** irányítópult válassza az **eszközök pontot**, és válassza a **Feladatátvevőfürt-kezelő**.

1. A bal oldali ablaktáblában kattintson a jobb gombbal a **Feladatátvevőfürt-kezelő**, és kattintson a **Létrehozás fürt**, alább látható módon.

    ![Csoport létrehozása](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784632.png)

1. Fürt létrehozása varázsló hozhat létre egy csomópont-fürt verziószámú a lapokról, az alábbi beállításokat:

  	|Lap|Beállítások|
|---|---|
|Mielőtt elkezdené|Alapértelmezett értékek használata|
|Jelölje ki a kiszolgálón|Írja be a **kiszolgáló nevének megadása** **ContosoSQL1** , és kattintson a **Hozzáadás** gombra|
|Figyelmeztetés adatérvényesítési|Jelölje ki a **én nincs szükség a Microsoft támogatási a fürt, és ezért nem szeretne az érvényesítési vizsgálatok futtatása szám. Ha tovább, továbbra is a fürt létrehozása**.|
|Hozzáférési pontot a fürt felügyelete|Típus **Cluster1** **fürt** nevében|
|Megerősítést kérő|Alapértelmezett használata, kivéve ha tárhelyek használ. Olvassa el a következő a táblázat megjegyzést.|

    >[AZURE.WARNING] Ha [Tárhelyek](https://technet.microsoft.com/library/hh831739), amely több lemezre csoportosítja tároló készletek, használja törölni kell a jelet az **összes jogosult tárterület a fürthöz** jelölőnégyzetet a **Megerősítés** lapon. Nem törli a jelet a jelölőnégyzetből, ha a leválasztott a fürtképző során a virtuális lemez kell. Emiatt is nem fognak megjelenni a lemez Manager vagy Explorer mindaddig, amíg a tárhelyek törlődjenek fürt, és azt a PowerShell használatá visszazárni.

1. A bal oldali ablaktáblában bontsa ki a **Feladatátvevőfürt-kezelő**, és válassza a **Cluster1.corp.contoso.com**.

1. A központi paneljén lévő görgessen le a **Fürt Core források** részben, és bontsa ki a **neve: Clutser1** részleteket. Meg kell jelennie a **nevét** és **IP-cím** erőforrások **hibás** állapotú. Az IP-cím erőforrás a nem online állapotba, mert a fürt van-e hozzárendelve ugyanazt a címet, mint a a gép magát, vagyis egy ismétlődő címet.

1. Kattintson a jobb gombbal az **IP-cím** sikertelen erőforrás, és válassza a **Tulajdonságok parancsot**.

    ![Fürt tulajdonságai](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784633.png)

1. Jelölje be a **Statikus IP-címet** , és adja meg a **10.10.2.101** az a cím mezőbe. Kattintson az **OK gombra**.

1. A **Fürt alapvető erőforrások** csoportban kattintson a jobb gombbal **neve: Cluster1** , és kattintson az **Online állapotba**. Ezután várja meg mindkét erőforrás online állapotban. Amikor a fürt név erőforrás online állapotba kerül, új AD fiókkal rendelkező frissíti az Adatközpont kiszolgáló. Ehhez a fiókhoz AD a csoportosított csoport rendelkezésre állási szolgáltatás később futtatandó lesz.

1. Végül a fürt hozzáadja a többi csomópontot. A böngésző fában **Cluster1.corp.contoso.com** gombbal, és válassza a **Csomópont hozzáadása**, alább látható módon.

    ![Adja hozzá csomópontot a fürthöz](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784634.png)

1. A **Csomópont hozzáadása varázsló**kattintson a **Tovább**gombra. **Válassza a kiszolgálók** lapon hozzáadása **ContosoSQL2** és **ContosoWSFCNode** a listához **kiszolgáló nevének megadása** írja be a kiszolgáló nevét, majd kattintson a **Hozzáadás**gombra kattintva. Ha végzett, kattintson a **Tovább**gombra.

1. Az **Ellenőrzési figyelmeztetés** lapon kattintson a **nem** gombra (a termelési forgatókönyvben, kell végezze el az érvényességi teszteket). Kattintson a **Tovább gombra**.

1. A **megerősítő** lapon kattintson a **Tovább** gombra a csomópontok hozzáadása.

    >[AZURE.WARNING] Ha [Tárhelyek](https://technet.microsoft.com/library/hh831739), amely több lemezre csoportosítja tároló készletek, használja törölni kell a jelet az **összes jogosult tárterület a fürthöz** jelölőnégyzetet. Nem törli a jelet a jelölőnégyzetből, ha a leválasztott a fürtképző során a virtuális lemez kell. Emiatt is nem fognak megjelenni a lemez Manager vagy Explorer mindaddig, amíg a tárhelyek törlődjenek fürt, és azt a PowerShell használatá visszazárni.

1. Miután a csomópontok bekerülnek a fürthöz, kattintson a **Befejezés gombra**. Feladatátvevőfürt-kezelő most jelenjen meg, hogy a fürt három csomópontok és sorolja fel azokat a **csomópontok** tárolóban.

1. Jelentkezzen ki a távoli asztali kapcsolat.

## <a name="prepare-the-sql-server-instances-for-availability-group"></a>Az SQL Server-példányok előkészítése rendelkezésre állási csoport

Ebben a részben el **ContosoSQL1** és **contosoSQL2**a következő lesz:

- A szükséges engedélyekkel, állítsa az alapértelmezett SQL Server-példányt **NT AUTHORITY\System** bejelentkezési hozzáadása

- A rendszergazdák szerep **CORP\Install** az alapértelmezett SQL Server-példányt hozzáadása

- Nyissa meg a tűzfalat távoli eléréséhez, az SQL Server

- Mindig a elérhetősége csoportok szolgáltatás engedélyezése

- A kurzor az **CORP\SQLSvc1** és **CORP\SQLSvc2**, az SQL Server-szolgáltatási fiók módosítása

Ezeket a műveleteket hajthatják sorrendben követik egymást. Mindazonáltal az alábbi lépésekkel azt ismerteti, őket a sorrendben. Kövesse a **ContosoSQL1** és **ContosoSQL2**:

1. Ha nem jelentkezik a távoli asztali munkamenet ki a virtuális, most tegye.

1. Indítsa el a RDP-fájlok **ContosoSQL1** és **ContosoSQL2** , és jelentkezzen be azzal **BUILTIN\AzureAdmin**.

1. Első lépésként hozzáadása **NT AUTHORITY\System** , az SQL Server bejelentkezési és a szükséges engedélyekkel. Indítsa el az **SQL Server Management Studio eszközben**.

1. Kattintson a **Csatlakozás** az alapértelmezett SQL Server-példányt csatlakozni.

1. Az **Objektum Explorer**bontsa ki a **biztonsági**, és bontsa ki a **Bejelentkezés**gombra.

1. Kattintson a jobb gombbal a **NT AUTHORITY\System** bejelentkezés, és válassza a **Tulajdonságok parancsot**.

1. A **Securables** lapon, a helyi kiszolgáló **támogatás** kiválasztása a következő engedélyeket, és kattintson az **OK gombra**.

    - Minden rendelkezésre állási csoport módosítása

    - Csatlakozás SQL

    - Nézet kiszolgáló állapota

1. Ezután adja hozzá **CORP\Install** **rendszergazdák** szerepkör az alapértelmezett SQL Server-példányt. Az **Objektum Explorer**kattintson a jobb gombbal a **bejelentkezések** , és kattintson az **Új bejelentkezési**.

1. Írja be **a bejelentkezési név** **CORP\Install** .

1. A **Kiszolgálói szerepkörök** lapon válassza a **rendszergazdák**. Kattintson az **OK gombra**. Amikor a bejelentkezési létrejött, a **bejelentkezések** az **Objektum Explorer**kibontása láthatja.

1. Következő lépésként meg tűzfal szabály létrehozása az SQL Server. A **kezdőképernyőn** indítsa el **a fokozott biztonságú Windows tűzfal**.

1. A bal oldali ablaktáblában válassza a **Bejövő szabályok**elemre. A jobb oldali ablaktáblán kattintson az **Új szabály**gombra.

1. A **Szabálytípus** lapon jelölje be a **Program**, majd kattintson a **Tovább**gombra.

1. A **Program** lapon jelölje be **a program elérési útját** , és írja be a **%ProgramFiles%\Microsoft SQL Server\MSSQL12. MSSQLSERVER\MSSQL\Binn\sqlservr.exe** a szövegmezőbe (ha követi a irányra, de az SQL Server 2012 használ, az SQL Server-címtár **MSSQL11. MSSQLSERVER**). Kattintson a **Tovább gombra**.

1. A **művelet** lapon **engedélyezze a kapcsolatot** a kijelölt megtartása, és kattintson a **Tovább**gombra.

1. **A profillapon** elfogadhatja az alapértelmezett beállításokat, és kattintson a **Tovább**gombra.

1. A **név** lapon adja meg a szabály nevét, például **SQL Server (Program szabály)** **neve** mezőbe, majd kattintson a **Befejezés gombra**.

1. Ezután lehetősége van engedélyezni a **Mindig a elérhetősége csoportok** funkciót. A **kezdőképernyőn** indítsa el az **SQL Server Configuration Manager**.

1. A böngésző fában kattintson **Az SQL Server Services**, majd kattintson a jobb gombbal az **SQL Server (MSSQLSERVER)** szolgáltatást, és válassza a **Tulajdonságok parancsot**.

1. Kattintson a **Mindig a magas elérhetőség** fülre, majd a **Engedélyezése mindig a elérhetősége csoportok**, válassza az alább látható módon, és kattintson az **Alkalmaz**gombra. Az előugró panelen kattintson az **OK gombra** , és nem zárja be a Tulajdonságok ablak még. Az SQL Server szolgáltatást újraindul a szolgáltatásfiók módosítása után is.

    ![Rendelkezésre állási csoportok mindig engedélyezése](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665520.gif)

1. Következő akkor módosítsa az SQL Server-szolgáltatási fiók. Kattintson a **Bejelentkezés** lapon, majd **CORP\SQLSvc1** (a **ContosoSQL1**) vagy **CORP\SQLSvc2** (a **ContosoSQL2**) írja be a **Fiók nevét**, majd töltse ki a erősítse meg a jelszót, és kattintson **az OK**gombra.

1. Az előugró ablakban kattintson az **Igen gombra** kattintva újra kell indítani az SQL Server szolgáltatást. Az SQL Server szolgáltatást újraindítása után a Tulajdonságok ablak a módosítások érvényesek lesznek.

1. Jelentkezzen ki a VMs.

## <a name="create-the-availability-group"></a>Az elérhetőség csoport létrehozása

Most már készen áll az elérhetőség csoport konfigurálása. Az alábbi képen a csoportba foglalása, hogy mire lesz:

- **ContosoSQL1** létrehozása új adatbázis (**MyDB1**)

- Egy teljes biztonsági mentés és a tranzakció napló biztonsági másolatot az adatbázis készítése

- A teljes visszaállítása, és jelentkezzen be biztonsági másolatok **ContosoSQL2** **NORECOVERY** lehetőséggel

- Az elérhetőség csoport (**AG1**) létrehozása a szinkronizált jóváhagyás, automatikusan átveszi és olvasható másodlagos kópiák

### <a name="create-the-mydb1-database-on-contososql1"></a>Hozzon létre a MyDB1 adatbázis ContosoSQL1:

1. Ha nem már jelentkezik a távoli asztali munkamenetek kívül **ContosoSQL1** és **ContosoSQL2**, most tegye.

1. Indítsa el az RDP-fájlt az **ContosoSQL1** , és jelentkezzen be azzal **CORP\Install**.

1. A **Fájlkezelő szót**a * *C:\**, nevű könyvtár létrehozása * *biztonsági**. A címtár segítségével biztonsági mentése és visszaállítása az adatbázis fogja használni.

1. Kattintson a jobb gombbal az új könyvtár, mutasson a **megosztás**, és válassza a **meghatározott személyeket**, ahogy alább látható módon.

    ![Biztonsági másolat mappa létrehozása](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665521.gif)

1. **CORP\SQLSvc1** hozzáadása az **Olvasási/írási** engedélye, tegyen, majd **CORP\SQLSvc2** hozzáadása és kölcsönözhet az **olvasási** engedélyt alább látható módon, és kattintson a **megosztás**gombra. A fájlmegosztás folyamat befejezése után kattintson a **kész**gombra.

    ![Biztonsági másolat mappára vonatkozó engedélyek](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665522.gif)

1. Ezután hozzon létre az adatbázist. Kattintson a **Start** menü indítsa el az **SQL Server Management Studio**, majd kattintson a **Csatlakozás** az alapértelmezett SQL Server-példányt csatlakozni.

1. Az **Objektum Intéző**kattintson a jobb gombbal az **adatbázisok** , és kattintson az **Új adatbázis**.

1. Az **adatbázis nevét**írja be a **MyDB1**, majd kattintson az **OK gombra**.

### <a name="take-a-full-backup-of-mydb1-and-restore-it-on-contososql2"></a>A teljes MyDB1 biztonsági másolatot készíthet, és visszaállítja a ContosoSQL2:

1. Ezután meg, hogy a teljes az adatbázis biztonsági. Az **Objektum Explorer**bontsa ki a **adatbázisok**, majd kattintson a jobb gombbal a **MyDB1**, majd mutasson a **tevékenységek**, és kattintson a **Feljebb**gombra.

1. A **forrás** csoportban hagyja meg a **teljes** **biztonsági másolat típusa** . A **cél** csoportban kattintson a **távolítsa el** az alapértelmezett elérési utat a biztonsági másolatnak eltávolítása elemre.

1. A **cél** csoportban kattintson a **Hozzáadás**gombra.

1. Írja be a **fájlnév** mezőbe, ** \\ContosoSQL1\backup\MyDB1.bak**. Ezután kattintson az **OK gombra**, és kattintson ismét az **OK gombra** az adatbázis biztonsági mentése. A biztonsági mentés befejeztével kattintson ismét az **OK gombra** a párbeszédpanel bezárásához.

1. Következő lépésként meg, hogy egy tranzakciónaplója az adatbázis biztonsági. Az **Objektum Explorer**bontsa ki a **adatbázisok**, majd kattintson a jobb gombbal a **MyDB1**, majd mutasson a **tevékenységek**, és kattintson a **Feljebb**gombra.

1. **Biztonsági másolat** típusa csoportban válassza a **Tranzakciónaplója**. Hagyja meg a **célként megadott** fájl elérési útja beállítása egy korábban megadott, és kattintson az **OK gombra**. Ha befejeződött a biztonsági mentést, kattintson ismét **az OK gombra** .

1. Ezután a teljes és a tranzakció napló másolatok **ContosoSQL2**visszaállíthatja. Indítsa el az RDP-fájlt az **ContosoSQL2** , és jelentkezzen be azzal **CORP\Install**. Hagyja a távoli asztali munkamenet- **ContosoSQL1** nyitva.

1. Kattintson a **Start** menü indítsa el az **SQL Server Management Studio**, majd kattintson a **Csatlakozás** az alapértelmezett SQL Server-példányt csatlakozni.

1. Az **Objektum Explorer**kattintson a jobb gombbal az **adatbázisok** , és kattintson az **Adatbázis visszaállítása**parancsra.

1. A **forrás** csoportban jelölje ki a **eszközt**, és kattintson a **...** gombra.

1. **Jelölje ki a biztonsági másolat eszközöket**kattintson a **Hozzáadás**gombra.

1. Biztonsági másolat fájl helyét, írja be \\ContosoSQL1\backup, kattintson a frissítés, majd jelölje ki a MyDB1.bak, majd kattintson az OK gombra, és kattintson ismét az OK gombra. Ekkor megjelenik a teljes biztonsági másolatot, és a napló biztonsági másolatot a biztonsági mentés visszaállítása ablak állítja be.

1. Nyissa meg a beállítások lapot, majd válassza ki a VISSZAÁLLÍTANI a NORECOVERY helyreállítási állapotú, és kattintson az OK gombra az adatbázis visszaállítása. Ha a visszaállítási művelet befejeződött, kattintson az OK gombra.

### <a name="create-the-availability-group"></a>Az elérhetőség csoport létrehozása:

1. A **ContosoSQL1**térjen vissza a távoli asztali kapcsolat. Az **Objektum Explorer** a SSMS **Mindig a magas elérhetősége** gombbal, és válassza a **Új elérhetősége csoport varázslóval**, alább látható módon.

    ![Új rendelkezésre állási csoport varázsló elindítása](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665523.gif)

1. **Bevezetés** lapon kattintson a **Tovább**gombra. **Elérhetősége csoport nevét adja meg** a lapon írja be a **AG1** **elérhetősége csoport nevét**, majd kattintson ismét a **Tovább** gombra.

    ![Új AG varázslóban AG nevének megadása](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665524.gif)

1. Az **Adatbázisok kiválasztása** lapon jelölje ki a **MyDB1** , és kattintson a **Tovább**gombra. Az adatbázis-rendelkezésre állási csoport előfeltételei megfelel, mert léptek legalább egy teljes biztonsági másolatot készíteni a tervezett elsődleges replikakészlettagon.

    ![Új AG varázslóban jelölje be az adatbázisok](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665525.gif)

1. A **Kópiák megadása** lapon kattintson a **Replika hozzáadása**.

    ![Új AG varázslóban adja meg a kópiák](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665526.gif)

1. Megjelenik a **Csatlakozás a kiszolgálóhoz** párbeszédpanel. Írja be a **ContosoSQL2** **kiszolgáló nevét**, majd kattintson a **Csatlakozás**gombra.

    ![A csatlakozás a kiszolgálóhoz új AG varázslóban](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665527.gif)

1. Vissza az **Kópiák megadása** lapon ekkor jelennie **ContosoSQL2** **Elérhető kópiák**szerepel. Állítsa be a replikák, alább látható módon. Amikor elkészült, kattintson a **Tovább**gombra.

    ![Új AG varázslóban adja meg a kópiák (kész)](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665528.gif)

1. A **Kezdeti adatszinkronizálás kiválasztása** lapon válassza a **Csatlakozás csak** , és kattintson a **Tovább**gombra. Manuális már elvégzése adatok szinkronizálása a teljes és a tranzakciók biztonsági másolatok elvégez a **ContosoSQL1** és **ContosoSQL2**vissza őket. Helyette megadhatja, hogy a biztonsági másolat és visszaállítása az adatbázis műveleteket, és válassza ki a **teljes** , az új elérhetősége csoport varázsló adatszinkronizálás elvégezheti, hogy nem. Azonban ez nem ajánlott, amelyek az egyes vállalatok nagyon nagy adatbázisokat.

    ![Új AG varázslóban jelölje ki a kiindulási adatok szinkronizálása](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665529.gif)

1. Az **adatérvényesítési** lapon kattintson a **Tovább**gombra. Ez a lap alatt láthatóhoz hasonlóan kell kinéznie. Van egy figyelmeztetés a figyelő konfigurációja, mert nem konfigurálta az elérhetőség csoport figyelő. Ez a figyelmeztetés is figyelmen kívül, mert ez az oktatóanyag nem állítja be a figyelő. Ebben az oktatóanyagban befejezése után a figyelő konfigurálásához a [konfigurálása egy ILB figyelő mindig a elérhetőség csoportok Azure-ban](virtual-machines-windows-classic-ps-sql-int-listener.md)című témakört.

    ![Új AG varázslóban érvényesítése](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665530.gif)

1. Az **Összegzés** lapon kattintson a **Befejezés gombra**, és várja meg, amíg a varázsló konfigurálja az új elérhetősége csoport. A **haladás** lapon választhatja a **További részleteket** a részletes előrehaladását. Miután a varázsló végzett, nézze meg az **eredmények** lapon ellenőrizze, hogy az elérhetőség csoport sikeresen jön létre, alább látható módon, majd kattintson a **Bezárás** gombra kattintva zárja be a varázslót.

    ![Új AG varázslóban eredménye](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665531.gif)

1. Az **Objektum Explorer**bontsa ki a **Mindig a magas elérhetőségét**, majd **Elérhetősége csoportok**kibontása. Ekkor megjelennek a tároló új elérhetőség csoportban. Kattintson a jobb gombbal **AG1 (elsődleges)** , és kattintson az **Irányítópult megjelenítése**.

    ![AG irányítópult megjelenítése](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665532.gif)

1. Az **Irányítópult mindig a** az alább látható módon egy hasonlóan kell kinéznie. Megtekintheti a replikák, mindegyik kópiában és a szinkronizálási állapot feladatátvevő módját.

    ![AG irányítópult](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665533.gif)

1. Térjen vissza **A Kiszolgálókezelő**, válassza az **eszközök pontot**, és indítsa **Feladatátvevőfürt-kezelő**.

1. Bontsa ki a **Cluster1.corp.contoso.com**, és bontsa ki a **szolgáltatások és alkalmazások**. Jelölje ki a **szerepkörök** , és figyelje meg, hogy létrejött a **AG1** elérhetősége csoport szerepkörnek. Ne feledje, hogy AG1 nem bármely IP-cím mely adatbázis ügyfelek csatlakozhatnak az elérhetőség csoport, mert nem állítja be egy figyelő. Csatlakozhat közvetlenül az elsődleges csomópontok írási és olvasási műveletekhez és a másodlagos csomópontra írásvédett lekérdezések.

    ![A Feladatátvevőfürt-kezelő AG](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665534.gif)

>[AZURE.WARNING] Próbálja meg átadni a elérhetősége csoport a a Feladatátvevőfürt-kezelő. Az összes feladatátvételi műveleteket SSMS **Mindig irányítópult** belül a hajtható végre. További tudnivalókért lásd: [a használatával a WSFC Feladatátvevőfürt-kezelő elérhetőség csoportokkal korlátozások](https://msdn.microsoft.com/library/ff929171.aspx).

## <a name="next-steps"></a>Következő lépések
Akkor most sikeresen végrehajtotta az SQL Server mindig a: hozzon létre egy rendelkezésre állási csoport Azure-ban. Az elérhetőség csoport figyelő konfigurálásához a [konfigurálása egy ILB figyelő mindig a elérhetősége csoportok Azure-ban](virtual-machines-windows-classic-ps-sql-int-listener.md)című témakört.

Más használatáról az SQL Server Azure-ban olvassa el a [A Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-server-iaas-overview.md)című témakört.

<properties
    pageTitle="Az Azure Active Directory összevonási szolgáltatások |} Microsoft Azure"
    description="A dokumentumban megtanulhatja, hogyan telepítse az Azure Active Directory összevonási szolgáltatások magas availablity."
    keywords="az azure Active Directory összevonási szolgáltatások üzembe, üzembe azure adfs, az azure adfs, az azure Active Directory összevonási szolgáltatások telepítése az adfs, üzembe az Active Directory összevonási szolgáltatások, az azure-adfs üzembe adfs azure-ban, a azure, az adfs azure, az Active Directory összevonási szolgáltatások, a Azure, az Azure-iaas, ADFS, az Active Directory összevonási szolgáltatások – bevezetés az Active Directory összevonási szolgáltatások üzembe az azure adfs"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/03/2016"
    ms.author="anandy;billmath"/>

# <a name="ad-fs-deployment-in-azure"></a>Azure Active Directory összevonási szolgáltatások bevezetésének 

Active Directory Összevonási szolgáltatások összevonási egyszerűsített, biztonságos identitás és webes egyszeri bejelentkezés (SSO) funkciók. Az Azure Active Directory összevonási vagy az Office 365 lehetővé teszi, hogy a felhasználók hitelesíteni a helyszíni hitelesítő adataival, és minden forrásokat a felhőben. Emiatt válik, fontos, hogy egy könnyen hozzáférhető AD FS-infrastruktúrát erőforrások elérhetősége érdekében mindkét a helyszíni és felhőbeli. Az Azure Active Directory összevonási szolgáltatások telepítésével segíthetnek a minimális erőfeszítéseket meg kell adnia a magas rendelkezésre állás érdekében.
Vannak az Azure Active Directory összevonási szolgáltatások több előnyei, néhány őket az alábbiak:

* **Magas elérhetősége** – az Azure-készletek elérhetőségét, a power biztosíthatja a könnyen hozzáférhető infrastruktúra.
* **Könnyen méretarányra** – további teljesítmény van szüksége? Több hatékony gépek Azure-ban mindössze néhány kattintással egyszerűen áttelepítése
* **Idegen-Geo redundancia** – az Azure Geo redundancia akkor biztos lehet abban, hogy a infrastruktúra érhető el erősen végig a földgömb
* **Könnyen kezelése** – az Azure-portálon erősen egyszerűsített fájlkezelési beállításainak kezelése a infrastruktúra nagyon egyszerűen és barátságos 

## <a name="design-principles"></a>Tervező elvek

![Telepítési Tervező](./media/active-directory-aadconnect-azure-adfs/deployment.png)

A fenti ábrán a javasolt egyszerű topológia üzembe helyezése a Azure Active Directory összevonási szolgáltatások infrastruktúra indításához. A különféle összetevőket topológiájának mögött elvek alatt jelennek meg:

* **Adatközpont / ADFS-kiszolgálók**: 1000-nél kevesebb felhasználó egyszerűen telepíthető az Active Directory összevonási szolgáltatások szerepkör a tartomány vezérlők. Ha nem szeretné, hogy a tartomány vezérlők teljesítmény hatással vagy 1000-nél több felhasználó, majd telepítse az Active Directory összevonási szolgáltatások külön kiszolgálókon.
* **WAP Server** – szükség a webes alkalmazás proxykiszolgálóiban, üzembe, hogy a felhasználók elérhessék az AD FS, amikor nem számítanak a vállalati hálózaton.
* **DMZ**: A webes alkalmazás proxykiszolgálók a DMZ kerülnek, és csak a TCP/443-as access engedélyezett a DMZ és a belső alhálózat között.
* **Betöltése Balancers**: Active Directory összevonási szolgáltatások és a webes alkalmazás proxykiszolgálók magas elérhetősége érdekében azt javasoljuk, egy belső terheléselosztó használata az AD FS-kiszolgáló és Azure betöltése terheléselosztó a webes alkalmazás proxykiszolgálóiban.
* **Elérhetőség beállítása**: az Active Directory összevonási szolgáltatások telepítési redundancia, azt ajánljuk, hogy két vagy több virtuális gépeken futó hasonló munkaterhelésekből az elérhetőség megadása a csoportosítás. Ebben a konfigurációban biztosítja, hogy közben vagy a tervezett vagy nem tervezett karbantartás esemény legalább egy virtuális gép lesz elérhető
* **Tárterület-fiókok**: azt ajánljuk, hogy a két tárterület-fiókot. Egy egyetlen tárolási fiókot kellene egyetlen pont hiba kialakulásához vezethet, és emiatt a lesz egy valószínű esetben, ha megszakad a tárterület-fiók nem érhető el a telepítést. Két tárterület-fiók segítségével rendeljen hibafa soronként egy tárterület-fiókot.
* **Hálózati feladatkörök**: webes alkalmazás proxykiszolgálók kell telepíthetők külön DMZ-hálózatban. Egy virtuális hálózati felosztása két alhálózat, és egy elszigetelt alhálózat webes szolgáltatásalkalmazás-Proxy kiszolgálói telepítését. Egyszerűen adja meg mindegyik alhálózat hálózati biztonsági csoport beállításait, és csak a két alhálózat szükséges kommunikációját engedélyezése. További részletek az alábbi telepítési forgatókönyv per kapnak

##<a name="steps-to-deploy-ad-fs-in-azure"></a>A lépéseket az Azure Active Directory összevonási szolgáltatások terjesztése

Az ebben a szakaszban szereplő lépéseket az útmutató üzembe tagolása az Azure Active Directory összevonási szolgáltatások infrastruktúra kitaláltak alatt.

### <a name="1-deploying-the-network"></a>1. a hálózat telepítése

A fent bemutatottak akkor is vagy hozzon létre két alhálózat virtuális egyetlen hálózat, különben hozzon létre két teljesen más virtuális hálózatok (VNet). Ez a cikk koncentráljon egyetlen virtuális hálózat üzembe helyezése, és a két alhálózat oszthatja meg. Ez a jelenleg könnyebben megközelítés, két külön VNets lenne szükség egy VNet az átjáró VNet kommunikáció.

**1.1 virtuális hálózat létrehozása**

![Virtuális hálózat létrehozása](./media/active-directory-aadconnect-azure-adfs/deploynetwork1.png)
    
Az Azure-portálon válassza a virtuális hálózati és telepíthetik a virtuális hálózat és egy alhálózat azonnal egyetlen kattintással. INT alhálózat is van megadva, és hozzá kell adni a VMs most már készen áll a.
Lépés egy másik alhálózat hozzáadása a hálózaton, azaz a DMZ-alhálózat. A DMZ-alhálózat egyszerűen létrehozása

* Jelölje ki az újonnan létrehozott hálózati
* Válassza a Tulajdonságok alhálózat
* Az alhálózathoz panelen kattintson a Hozzáadás gombra.
* Adja meg a alhálózat neve és címe terület adatokat az alhálózathoz létrehozása

![Alhálózat](./media/active-directory-aadconnect-azure-adfs/deploynetwork2.png)


![Alhálózat DMZ](./media/active-directory-aadconnect-azure-adfs/deploynetwork3.png)

**Az 1.2. a hálózat biztonsági csoportok létrehozása**

Hálózati biztonsági csoport (NSG) Access vezérlőelem-lista (vezérlés) szabályokat, amelyek lehetővé teszik, vagy megtagadja a hálózati forgalmat a virtuális példányaiban virtuális hálózatban listáját tartalmazza. Lehet, hogy NSGs alhálózat vagy a alhálózat egyes virtuális példányokat társított. Ha egy NSG alhálózat társítva, vezérlés szabályokat alkalmazni alhálózat a virtuális példányok.
Ez az útmutató céljából hozunk létre két NSGs: egy minden egy belső hálózati és a DMZ. Ezek neve NSG_INT és NSG_DMZ rendre.

![NSG létrehozása](./media/active-directory-aadconnect-azure-adfs/creatensg1.png)

Miután létrehozta a NSG, 0 0, a bejövő és kimenő szabályok lesz. Miután a szerepkör a megfelelő kiszolgálókon van telepítve, és funkcionális, majd a bejövő és kimenő szabályokat tehetők a kívánt biztonsági szintet megfelelően.

![NSG inicializálni](./media/active-directory-aadconnect-azure-adfs/nsgint1.png)

A NSGs létrehozása után NSG_INT társítása alhálózat INT és az alhálózathoz DMZ NSG_DMZ. Egy példa képernyőképe az alábbi táblázat a:

![NSG konfigurálása](./media/active-directory-aadconnect-azure-adfs/nsgconfigure1.png)

* Az alhálózathoz panel megnyitásához alhálózat gombja
* Jelölje ki a NSG társíthatja az alhálózat 

Konfigurálása után alhálózat munkaablakbeli alatti így néz:

![Alhálózat NSG után](./media/active-directory-aadconnect-azure-adfs/nsgconfigure2.png)

**Az 1.3. Hozzon létre kapcsolatot a helyszíni**

Kapcsolat a helyszíni annak érdekében, hogy a tartományvezérlőnek (Adatközpont) azure telepítése lesz szükség. Azure különböző kapcsolódási lehetőséget kínál a helyszíni infrastruktúra csatlakozhat az Azure infrastruktúra.

* Pont-webhely
* Virtuális hálózati hely webhelyen
* Készült ExpressRoute

Készült ExpressRoute használata ajánlott. Készült ExpressRoute segítségével saját kapcsolatot Azure adatközpontokkal és a helyszíni, vagy egy közös helyre környezetben infrastruktúra között. Készült ExpressRoute kapcsolatok megy nyilvános interneten keresztül. További megbízhatóságot, gyorsabb sebesség, alsó késések és magasabb szintű biztonságos, mint a szokásos kapcsolatok kínálnak az interneten keresztül.
Készült ExpressRoute használata ajánlott, miközben dönthet bármely csatlakozási mód működik a legjobban a szervezete számára. További információ a készült ExpressRoute és a különböző csatlakozási beállítások készült ExpressRoute használatával, olvassa [készült ExpressRoute technikai áttekintés](https://aka.ms/Azure/ExpressRoute).

### <a name="2-create-storage-accounts"></a>2. a tárterület-fiókok létrehozása

Magas rendelkezésre álljon, és egyetlen tárolás fiókkal függés elkerülése érdekében, két tárolás fiókot is létrehozhat. A gép az egyes elérhetősége felosztása két csoportot, és egy külön tároló fiók minden csoport rendelje.

![Tárterület-fiókok létrehozása](./media/active-directory-aadconnect-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3. a elérhetősége készletek létrehozása

Az egyes szerepkörhöz (Adatközpont/Active Directory összevonási szolgáltatások és WAP) hozzon létre, amely tartalmazni fogja a 2 gépek minden legalább elérhetőségének beállítása. Ez segít elérni magasabb rendelkezésre állás minden szerepkörhöz. Létrehozása az elérhetőségét állítja be, azt is fontos döntse el, a következő:
* **Hibafa-tartományok**: virtuális gépeken futó hibafa tartományában az azonos power source és a fizikai hálózati kapcsoló megosztására. 2 hibafa tartományok legalább ajánlott. Az alapértelmezett értéke 3, és azt a telepítéssel célja hagyhatja
* **A tartományok frissítése**: azonos frissítés tartományhoz tartozó gépek közös újraindítja frissítés során. 2 frissítés tartományok minimális szeretné használni. Az alapértelmezett érték 5, és azt a telepítéssel célja hagyhatja

![Elérhetőség beállítása](./media/active-directory-aadconnect-azure-adfs/availabilityset1.png)

A következő elérhetősége halmaz létrehozása

| Elérhetőség beállítása | Szerepkör | Hibafa-tartományok | A tartományok frissítése |
|:----------------:|:----:|:-----------:|:-----------|
| contosodcset | ADATKÖZPONT/ADFS | 3 | 5 |
| contosowapset | WAP | 3 | 5 |

### <a name="4--deploy-virtual-machines"></a>4. a virtuális gépeken futó terjesztése
A következő lépésként telepíteni az infrastruktúra a különféle szerepköröket ugyanis virtuális gépeken futó. Az egyes elérhetősége legalább két gép ajánlott. Az egyszerű telepítéshez hat virtuális gépeken futó létrehozása.

| Gépi | Szerepkör | Alhálózat | Elérhetőség beállítása | Tárterület-fiók | IP-cím |
|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
|contosodc1|ADATKÖZPONT/ADFS|INT|contosodcset|contososac1|Statikus|
|contosodc2|ADATKÖZPONT/ADFS|INT|contosodcset|contososac2|Statikus|
|contosowap1|WAP|DMZ|contosowapset|contososac1|Statikus|
|contosowap2|WAP|DMZ|contosowapset|contososac2|Statikus|

Akkor előfordulhat, hogy láthatta, nincs NSG lett megadva. Ennek oka az azure lehetővé teszi, hogy a alhálózat szintjén NSG használhatja. Ezt követően megadhatja, hogy számítógép hálózati forgalmának engedélyezésére vagy az alhálózathoz, különben a hálózati kártya objektum társított az egyes NSG használatával. Olvassa el a további meg [egy hálózati biztonsági csoport (NSG) tartalma](https://aka.ms/Azure/NSG).
Statikus IP-cím ajánlott, ha tartománya DNS-Rekordokat. Azure DNS használatához, és helyette a tartománya DNS-rekordjait, vonatkozik az Azure teljes tartományneve által az új gépek.
A virtuális gép ablaktábla így néz alatt a telepítés befejezése után:

![Virtuális gépeken futó rendszerbe](./media/active-directory-aadconnect-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-the-domain-controller--ad-fs-servers"></a>5. a tartományvezérlőnek konfigurálása és AD FS-kiszolgáló
 Annak érdekében, hogy minden bejövő kérelmet hitelesíteni, az AD FS fog kapcsolatba kell lépnie a tartományvezérlőnek. Szeretné menteni a költséges utazás az Azure helyszíni Adatközpont hitelesítéshez, ajánlott annak a tartományvezérlőnek Azure-ban kópia telepítéséhez. Magas elérhetősége eléréséhez ajánlott hozzon létre egy elérhetőségét a legkisebb 2 tartomány vezérlők.

|Tartományvezérlőnek|Szerepkör|Tárterület-fiók|
|:-----:|:-----:|:-----:|
|contosodc1|Replika|contososac1|
|contosodc2|Replika|contososac2|

* A két kiszolgáló előléptetése replika tartomány vezérlők a DNS-ben
* Az AD FS-kiszolgáló konfigurálása az Active Directory összevonási szolgáltatások szerepkör Kiszolgálókezelő telepítésével.

###<a name="6---deploying-internal-load-balancer-ilb"></a>6. üzembe helyezése a belső terheléselosztó (ILB)

**6.1. a ILB létrehozása**

Egy ILB üzembe helyezéséhez választó terheléselosztókkal az Azure portálját, és kattintson a Hozzáadás (+).
>[AZURE.NOTE] Ha **Terheléselosztókkal** nem látható a menüben, kattintson a **tallózással keresse meg** a bal alsó sarkában a portálon, és görgessen **Terheléselosztókkal**amíg.  Kattintson a sárga csillag fel szeretne venni a menüben. Most jelölje ki az új betöltés terheléselosztó ikonra a terheléselosztó konfigurációs megkezdéséhez panel megnyitásához.

![Tallózással keresse meg a terheléselosztó](./media/active-directory-aadconnect-azure-adfs/browseloadbalancer.png)

* **Név**: bármilyen alkalmas nevet a terheléselosztó engedély
* **Az Office-téma**: a terheléselosztó kerül az AD FS-kiszolgálók elé, és belső hálózati kapcsolatok csak, jelölje ki a "Belső" célja
* **Virtuális hálózati**: válassza ki a virtuális hálózatot, ahol telepít, az Active Directory összevonási szolgáltatások
* **Alhálózat**: válassza ki a belső alhálózat
* **IP-cím hozzárendelés**: dinamikus

![Belső terheléselosztó](./media/active-directory-aadconnect-azure-adfs/ilbdeployment1.png)
 
Kattintás után létrehozása és a ILB telepíti, pillanthatnak bele a terheléselosztókkal listájában:

![Terheléselosztókkal ILB után](./media/active-directory-aadconnect-azure-adfs/ilbdeployment2.png)
 
Következő lépésként az kódmentes készlet és a kódmentes vizsgálati konfigurálása.

**6.2. ILB kódmentes készletbe konfigurálása**

Jelölje ki az újonnan létrehozott ILB a terheléselosztókkal panelen. Ennek hatására megnyílik a csoportbeállítások panel. 
1.  Jelölje ki a beállítások panel kódmentes készletek
2.  A Hozzáadás kódmentes készlet panelen kattintson a Hozzáadás virtuális gépen
3.  Be kell mutatni a tartalmazó panelt, ahol megadhatja a elérhetőségének beállítása
4.  Kattintson az az Active Directory összevonási szolgáltatások elérhetősége

![ILB kódmentes készlet konfigurálása](./media/active-directory-aadconnect-azure-adfs/ilbdeployment3.png)
 
**6.3. vizsgálati konfigurálása**

A ILB csoportbeállítások panel, válassza a ellenőrzi.
1.  Kattintson a hozzáadása
2.  Részletek a szükséges vizsgálati egy. **Név**: Probe b nevét. **Protocol (protokoll)**: a TCP-c. **Port**: d 443-as (HTTPS). **Intervallum**: 5 (az alapértelmezett érték) – Ez az intervallumra, amelynél a ILB probe lesz a gép e kódmentes készletben. **Sérült küszöbértéke**: a 2 (alapértelmezett val ue) – Ez az a küszöbértékét, amely elteltével ILB deklarálja a gép kódmentes készletben nem válaszol, és a stop forgalom küldene vizsgálati egymást követő hibák.

![ILB vizsgálati konfigurálása](./media/active-directory-aadconnect-azure-adfs/ilbdeployment4.png)
 
**6.4. terheléselosztási szabályok létrehozása**

Annak érdekében, hogy hatékony egyenleg a forgalmat, a ILB úgy kell beállítani, a terheléselosztás szabályokat. Annak érdekében, hogy terheléselosztási szabály létrehozása 
1.  Jelölje ki a terheléselosztás, a ILB beállítások panelről szabály
2.  Kattintson a Hozzáadás gombjára a terheléselosztási szabály panel
3.  A Hozzáadás terheléselosztási szabály panel egy. **Név**: Adja meg a b szabály nevét. **Protocol (protokoll)**: válassza ki a TCP-c. **Port**: 443 d. **Kódmentes port**: 443-as e. **Kódmentes készlet**: Jelölje ki a készletet, az Active Directory összevonási szolgáltatások fürt korábbi f létrehozott. **Vizsgálati**: válassza ki a korábban létrehozott AD FS-kiszolgálók vizsgálati

![Nagyfokú szabályokat ILB konfigurálása](./media/active-directory-aadconnect-azure-adfs/ilbdeployment5.png)

**6.5. a DNS frissítése ILB**

Nyissa meg a DNS-kiszolgálót, és hozzon létre egy CNAME a ILB. A CNAME kell lennie az összevonási szolgáltatás IP-címét, mutasson a ILB IP-címét. A példában ha a ILB DIP cím 10.3.0.8, és az összevonási szolgáltatás telepítve a fs.contoso.com, hozzon létre egy CNAME-10.3.0.8 mutató fs.contoso.com.
Ezzel biztosíthatja, hogy a fs.contoso.com vége kapcsolatos összes kommunikációs, a ILB be, és megfelelően van-e irányítva.

###<a name="7---configuring-the-web-application-proxy-server"></a>7. a webes alkalmazás proxykiszolgáló beállítása

**7.1. beállítása a webes alkalmazás proxykiszolgálók AD FS-kiszolgáló elérésére**

Annak érdekében, hogy győződjön meg arról, hogy a webes alkalmazás proxykiszolgálók is elérhetik az AD FS-kiszolgálók mögött a ILB, hozzon létre egy rekordot a %systemroot%\system32\drivers\etc\hosts a ILB számára. Figyelje meg, hogy a megkülönböztető név (DN) kell-e az összevonási szolgáltatás neve, például fs.contoso.com. És az IP-bejegyzés kell lennie, amely a ILB IP-cím (a példában szereplő 10.3.0.8).

**7.2. a webes alkalmazás Proxy szerepkör telepítése**

Követően, győződjön meg arról, hogy a webes alkalmazás proxykiszolgálóiban is elérhetik az AD FS-kiszolgálók mögött ILB, ezután telepítheti a webes alkalmazás proxykiszolgálóiban. A tartomány nem webes alkalmazás proxykiszolgálók csatlakozik. A webes alkalmazás Proxy szerepkörök telepíthető a webes alkalmazás két proxykiszolgálók a távoli hozzáférés szerepkör kiválasztásával. A Kiszolgálókezelő végigkalauzolja WAP telepítésének befejezéséhez.
WAP telepítéséről további tudnivalókért olvassa el a [telepítése és beállítása a webes alkalmazás proxykiszolgáló](https://technet.microsoft.com/library/dn383662.aspx).

###<a name="8---deploying-the-internet-facing-public-load-balancer"></a>8. az interneten (nyilvános) terheléselosztó néző telepítése

**8.1. (nyilvános) terheléselosztó internetes létrehozása**
 
Az Azure-portálon jelölje ki a terheléselosztókkal, és kattintson a Hozzáadás gombra. Kattintson a létrehozás betöltése terheléselosztó ablaktábla írja be az alábbi információk
1. **Név**: a terheléselosztó nevét
2. **Az Office-téma**: nyilvános – Ez a beállítás azt jelenti az Azure, hogy a terheléselosztó szükséges-e nyilvános címet.
3. **IP-címe**: hozzon létre egy új IP-cím (dinamikus)

![Szemben lévő terheléselosztó Internet](./media/active-directory-aadconnect-azure-adfs/elbdeployment1.png)

A telepítés után megjelenik a terheléselosztó a betöltés balancers listában.

![Betöltési terheléselosztó lista](./media/active-directory-aadconnect-azure-adfs/elbdeployment2.png)
 
**8.2. a DNS-címke hozzárendelése a nyilvános IP**

Kattintson a betöltés balancers panel ahhoz, hogy a munkaablak konfiguráció beállítása az újonnan létrehozott betöltés terheléselosztó bejegyzést. Hajtsa végre az alábbi lépéseket követve állítsa be a DNS-címke a nyilvános IP:
1.  Kattintson a nyilvános IP-címét. Ekkor megnyílik a panelen a nyilvános IP-és annak beállítások
2.  Kattintson a konfiguráció
3.  Adja meg a DNS-címke. Ez lesz a nyilvános DNS-címke, például contosofs.westus.cloudapp.azure.com bárhonnan elérhet. A külső DNS az összevonási szolgáltatás (például fs.contoso.com), amely a külső terheléselosztó (contosofs.westus.cloudapp.azure.com) a DNS-címke is hozzáadhat egy bejegyzést.

![Internetes terheléselosztó konfigurálása](./media/active-directory-aadconnect-azure-adfs/elbdeployment3.png) 

![Állítsa be az internetes terheléselosztó (DNS)](./media/active-directory-aadconnect-azure-adfs/elbdeployment4.png)

**8.3. kódmentes készlet beállítása az internetes (nyilvános) szemben lévő terheléselosztó** 

Hajtsa végre ugyanezeket a lépéseket, ahogy a kódmentes készlet beállítása (nyilvános) betöltése terheléselosztó Internet szemben lévő beállítása a WAP kiszolgálóinak elérhetősége, a belső terheléselosztó létrehozása. Ha például contosowapset.

![Szemben lévő terheléselosztó Internet kódmentes készlete konfigurálása](./media/active-directory-aadconnect-azure-adfs/elbdeployment5.png)
 
**8,4. vizsgálati konfigurálása**

Hajtsa végre ugyanezeket a lépéseket, ahogy a belső terheléselosztó konfigurálása a vizsgálati WAP kiszolgálók kódmentes gyűjtő konfigurálása.

![Az Internet szemben lévő terheléselosztó vizsgálati konfigurálása](./media/active-directory-aadconnect-azure-adfs/elbdeployment6.png)
 
**8.5. terheléselosztási szabály(ok) létrehozása**

Hajtsa végre ugyanezeket a lépéseket, ahogy a TCP 443-as terheléselosztási szabály beállítása ILB.

![Az Internet szemben lévő terheléselosztó terheléselosztó szabályok konfigurálása](./media/active-directory-aadconnect-azure-adfs/elbdeployment7.png)
 
###<a name="9---securing-the-network"></a>9. a hálózat védelme

**9.1. biztonságossá tétele a belső alhálózat**

Általános szükséges hatékony biztonságos belső alhálózathoz (sorrendben az alábbi lépésekkel) a következő szabályok

|Szabály|Leírás|Folyamatábra|
|:----|:----|:------:|
|AllowHTTPSFromDMZ| A HTTPS kommunikáció engedélyezése a DMZ | Bejövő |
|DenyInternetOutbound| Nem lehet hozzáférni az internet | Kimenő |

![INT hozzáférési szabályokat (bejövő)](./media/active-directory-aadconnect-azure-adfs/nsg_int.png)

[Megjegyzés]: <> (![INT hozzáférési szabályokat (bejövő)](./media/active-directory-aadconnect-azure-adfs/nsgintinbound.png)) [Megjegyzés]: <> (![INT hozzáférési szabályokat (kimenő)](./media/active-directory-aadconnect-azure-adfs/nsgintoutbound.png))
 
**9.2. biztonságossá tétele a DMZ-alhálózat**

|Szabály|Leírás|Folyamatábra|
|:----|:----|:------:|
|AllowHTTPSFromInternet| Az internetről HTTPS engedélyezze a DMZ | Bejövő|
|DenyInternetOutbound|  Internetes HTTPS személynevek le van tiltva | Kimenő |

![KÜLSŐ hozzáférési szabályokat (bejövő)](./media/active-directory-aadconnect-azure-adfs/nsg_dmz.png)

[Megjegyzés]: <> (![MELLÉK hozzáférési szabályokat (bejövő)](./media/active-directory-aadconnect-azure-adfs/nsgdmzinbound.png)) [Megjegyzés]: <> (![MELLÉK hozzáférési szabályokat (kimenő)](./media/active-directory-aadconnect-azure-adfs/nsgdmzoutbound.png))

>[AZURE.NOTE] Ha ügyfélalkalmazás felhasználója tanúsítvány-hitelesítés (+ ügyféloldali TLS hitelesítéssel X509 felhasználói tanúsítványok) szükség, majd az Active Directory összevonási szolgáltatások igényel TCP port 49443 befelé kell beállítani.

###<a name="10--test-the-ad-fs-sign-in"></a>10. tesztelje az Active Directory összevonási szolgáltatások bejelentkezés

A legegyszerűbben az AD FS tesztelése van a IdpInitiatedSignon.aspx lapján. Annak érdekében, hogy elvégeztetni, hogy az szükséges ahhoz, hogy az Active Directory összevonási szolgáltatások tulajdonságainak IdpInitiatedSignOn. Hajtsa végre az alábbi lépésekkel ellenőrizze az Active Directory összevonási szolgáltatások beállítása
1.  Futtassa az alábbi parancsmag, a PowerShell használatával állítsa be engedélyezve van az AD FS-kiszolgálón.
    Set-AdfsProperties - EnableIdPInitiatedSignonPage $true 
2.  A minden olyan külső gépi access https://adfs.thecloudadvocate.com/adfs/ls/IdpInitiatedSignon.aspx  
3.  Meg kell jelennie a Active Directory összevonási szolgáltatások lap alatt hasonlóan:

![Teszt bejelentkezési lapja](./media/active-directory-aadconnect-azure-adfs/test1.png)

A sikeres-bejelentkezés fog szolgálni, egy üzenetet az alább látható módon:

![A siker tesztelése](./media/active-directory-aadconnect-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>Az Azure Active Directory összevonási szolgáltatások telepítésével sablon

A sablon egy 6 gépi telepítés 2 minden tartomány vezérlők, az Active Directory összevonási szolgáltatások és WAP üzembe helyezése.

[Azure telepítési sablon az Active Directory összevonási szolgáltatások](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

Egy meglévő virtuális hálózatot használ, vagy hozzon létre egy új VNET ezzel a sablonnal telepítése során. A telepítési testreszabásához elérhető a különböző paraméterek felsorolása olvasható használatát a paramétert a telepítési folyamatot leírását. 

| Paraméter | Leírás |
|:--------|:-----|
|Hely| A terület terjesztése az erőforrások alkalmazásba, például keleti US. |
|StorageAccountType| Az írja be a létrehozott tárterület-fiókhoz|
|VirtualNetworkUsage| Azt jelzi, ha egy új virtuális hálózati jön létre, vagy egy meglévőt használ|
|VirtualNetworkName| A virtuális létrehozása, kötelező, mind a meglévő vagy új virtuális hálózati használatát a hálózat nevét|
|VirtualNetworkResourceGroupName| A meglévő virtuális hálózat helye erőforráscsoport a nevét adja meg. Amikor egy meglévő virtuális hálózat, kötelező paraméter ez lesz az, a telepítési megtalálhatja a meglévő virtuális hálózat azonosítója|
|VirtualNetworkAddressRange| A cím cellatartományt az új VNET kötelező, ha egy új virtuális hálózati létrehozása|
|InternalSubnetName| A belső alhálózat, mindkét virtuális hálózati használatát beállításai (új vagy meglévő) kötelező neve|
|InternalSubnetAddressRange| A belső alhálózat, ha egy új virtuális hálózat létrehozása a tartomány vezérlők és ADFS kiszolgálókon kötelező tartalmazó címtartományokat.|
|DMZSubnetAddressRange| A dmz-alhálózat, ha egy új virtuális hálózat létrehozása a Windows alkalmazás proxykiszolgálók, kötelező tartalmazó címtartományokat.|
|DMZSubnetName| A belső alhálózat, mindkét virtuális hálózati használatát beállításai (új vagy meglévő) kötelező neve. |
|ADDC01NICIPAddress| A belső IP-címe az első tartományvezérlőnek, IP-cím statikus jogokat kap az Adatközpont és egy érvényes IP-cím, a belső alhálózat belül kell lennie.|
|ADDC02NICIPAddress| A második tartományvezérlőnek belső IP-címét, IP-cím statikus jogokat kap az Adatközpont, és egy érvényes IP-cím, a belső alhálózat belül kell lennie.|
|ADFS01NICIPAddress| A belső IP-címe az első ADFS-kiszolgálóra, IP-cím statikus jogokat kap az ADFS-kiszolgálóra és egy érvényes IP-cím, a belső alhálózat belül kell lennie.|
|ADFS02NICIPAddress| A második ADFS-kiszolgálóra belső IP-címét, IP-cím statikus jogokat kap az ADFS-kiszolgálóra és egy érvényes IP-cím, a belső alhálózat belül kell lennie.|
|WAP01NICIPAddress| A belső kiszolgáló IP-címe az első WAP, IP-cím statikus jogokat kap az WAP kiszolgáló és egy érvényes IP-cím, a DMZ-alhálózat belül kell lennie.|
|WAP02NICIPAddress| A belső kiszolgáló IP-címét a második WAP, IP-cím statikus jogokat kap az WAP kiszolgáló és egy érvényes IP-cím, a DMZ-alhálózat belül kell lennie.|
|ADFSLoadBalancerPrivateIPAddress| A belső IP-címét az ADFS-terheléselosztó, IP-cím statikus rendeli hozzá a terheléselosztó és kell lennie, a belső alhálózat egy érvényes IP-cím|
|ADDCVMNamePrefix| Virtuális gép név előtaggal tartomány-vezérlők|
|ADFSVMNamePrefix| Virtuális gép nevének előtagját ADFS-kiszolgálókon|
|WAPVMNamePrefix| Virtuális gép nevének előtagját WAP kiszolgálók|
|ADDCVMSize| A tartomány vezérlők virtuális mérete|
|ADFSVMSize| A virtuális méretét az ADFS-kiszolgálók|
|WAPVMSize| A virtuális méretét a WAP kiszolgálók|
|AdminUserName| A virtuális gépeken futó helyi rendszergazdája neve|
|AdminPassword| A virtuális gépeken futó rendszergazdafiók jelszavát|

## <a name="additional-resources"></a>További források
* [Elérhetőség beállítása](https://aka.ms/Azure/Availability ) 
* [Azure terheléselosztó](https://aka.ms/Azure/ILB)
* [Belső terheléselosztó](https://aka.ms/Azure/ILB/Internal)
* [Internetes elérhető terheléselosztó](https://aka.ms/Azure/ILB/Internet)
* [Tárterület-fiókok](https://aka.ms/Azure/Storage )
* [Azure virtuális hálózatok](https://aka.ms/Azure/VNet)
* [Active Directory összevonási szolgáltatások és a webes alkalmazás Proxy hivatkozások](http://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>Következő lépések

* [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)
* [Beállításáról és kezeléséről az Active Directory összevonási szolgáltatások használatával Azure AD Connect](active-directory-aadconnectfed-whatis.md)
* [Magas elérhetősége határokon földrajzi AD FS telepítési Azure forgalom Manager Azure-ban](active-directory-adfs-in-azure-with-azure-traffic-manager.md)





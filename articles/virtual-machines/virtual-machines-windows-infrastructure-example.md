<properties
    pageTitle="Példa infrastruktúra forgatókönyv |} Microsoft Azure"
    description="Tudjon meg többet a fontos tervezéséhez és kivitelezéséhez alapelve üzembe helyezése egy példa infrastruktúra Azure-ban."
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="example-azure-infrastructure-walkthrough"></a>Példa Azure infrastruktúra útmutató

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

Az alábbiakban ismertetjük épület egy példa alkalmazás infrastruktúra meg. Azt részletesen tervezés-infrastruktúrát egy egyszerű online áruházból, hogy összegyűjti az összes útmutatások és döntések elnevezési szabályokat, elérhetőségének beállítása, virtuális hálózatok és terheléselosztókkal köré, és a ténylegesen üzembe helyezése a virtuális gépeken futó (VMs).


## <a name="example-workload"></a>Példa terhelést

Adventure Works ciklus kíván össze egy online áruházból alkalmazást, amely az Azure-ban:

- Az ügyfél egy webes réteg előtér-kiszolgálókon két IIS
- Két IIS-kiszolgálók adatok és az alkalmazás réteg rendelések feldolgozása
- Két Microsoft SQL Server-példányt csoportokkal AlwaysOn elérhetőség (két SQL-kiszolgálót és a legtöbb csomópont tanú) a termék adatai és a rendelések tárolásához egy adatbázis réteg
- A felhasználói fiókok és a szállítók-hitelesítés réteg két az Active Directory tartományi vezérlő
- Az összes kiszolgálón a két alhálózat találhatók:
    - az előtér-alhálózat a webkiszolgálón 
    - háttér-alhálózat alkalmazáskiszolgálók, SQL fürt, illetve tartomány vezérlők

![Az alkalmazás infrastruktúra a különböző meghatározási diagram](./media/virtual-machines-common-infrastructure-service-guidelines/example-tiers.png)

A bejövő biztonságos internetes forgalmat betöltés-mérlegelendő webes kiszolgálók között, az ügyfelek, keresse meg az online áruházból. A rendelés feldolgozási forgalom HTTP-kérések az internetről kiszolgálók között a alkalmazáskiszolgálók mérlegelendő formájában. Emellett a infrastruktúra magas elérhetőség kell megtervezni.

Az eredményül kapott tervezés beépítése kell:

- Az Azure előfizetéssel, és a fiók
- Egységes erőforrás csoport
- Tárterület-fiókok
- A két alhálózat virtuális hálózaton
- Elérhetőség az hasonló jogosultság a VMs beállítása
- Virtuális gépeken futó

Az összes a fenti hajtsa végre az alábbi elnevezési szabályokat:

- Adventure Works ciklus felhasználási **[informatikai terhelést]-[hely]-[Azure erőforrás]** előtaggal
    - Ebben a példában a "**azos**" (Azure online áruházból) az informatikai terhelést neve pedig "**használata**" (US kelet-2) helye
- Tárterület-fiókok adventureazosusesa**[leírás]** használata
    - "adventure" hozzá lett adva az előtag egyediségét megadására, és a tárhely fióknevét nem támogatja a kötőjeleket.
- Virtuális hálózatok AZOS-használat-VN**[szám]** használata
- Elérhetőség készletek használata azos-használata – mint-**[szerepkör]**
- Virtuális gép neveiben azos-használata-virtuális -**[vmname]**


## <a name="azure-subscriptions-and-accounts"></a>Azure előfizetések és -fiókok

Adventure Works ciklus használatával a nagyvállalati előfizetés esetén, nevű Adventure Works nagyvállalati előfizetés esetén, adja meg a informatikai terhelést számlázását.


## <a name="storage-accounts"></a>Tárterület-fiókok

Adventure Works ciklus határozza meg, hogy azok szükséges két tárterület-fiókot:

- **adventureazosusesawebapp** a webkiszolgálón, a alkalmazáskiszolgálók, és a tartomány vezérlők és a saját adatok lemezt szabványos tárolására szolgáló.
- **adventureazosusesasql** az SQL Server VMs és az adatok lemezt prémium tárolására szolgáló.


## <a name="virtual-network-and-subnets"></a>Virtuális hálózati és alhálózat

A virtuális hálózat nem szükséges, az Adventure munka ciklus helyszíni hálózaton folyamatban lévő kapcsolat, mert azok a felhőben virtuális hálózaton úgy döntött.

Az Azure portálon az alábbi beállításokkal létrehozza őket a felhőben virtuális hálózati:

- Neve: AZOS-használat-VN01
- Hely: Kelet-Amerikai Egyesült Államok 2
- Virtuális hálózati címterület: 10.0.0.0/8
- Első alhálózat:
    - Név: FrontEnd
    - Hely címe: 10.0.1.0/24
- Második alhálózat:
    - Név: Kódmentes
    - Hely címe: 10.0.2.0/24


## <a name="availability-sets"></a>Elérhetőség beállítása

Az online áruházból összes négy meghatározási magas elérhetősége megtartásához Adventure Works ciklus úgy döntött, a négy elérhetőségének beállítása:

- a webhely-kiszolgálók **azos-használat--weblapként**
- az alkalmazás-kiszolgálók **azos--szerint-app használata**
- az SQL-kiszolgálók **azos-használat-mint-sql**
- **azos-használat-mint-adatközpont** a tartomány-vezérlők


## <a name="virtual-machines"></a>Virtuális gépeken futó

A következő nevek, az illető Azure VMs dönteni Adventure Works ciklus:

- **azos-használat-virtuális-web01** az első webkiszolgáló
- **azos-használat-virtuális-web02** a második webkiszolgáló
- **azos-használat-virtuális-app01** az első alkalmazás kiszolgálón
- **azos-használat-virtuális-app02** a második alkalmazáskiszolgáló
- **azos-használat-virtuális-sql01** a fürt az első SQL Server-kiszolgáló
- **azos-használat-virtuális-sql02** a második SQL Server kiszolgálóhoz a fürt
- **azos-használat-virtuális-dc01** az első tartomány vezérlő
- **azos-használat-virtuális-dc02** a második tartományvezérlőnek

Az alábbiakban az eredményül kapott konfigurációt.

![Alkalmazások végleges infrastruktúra rendszerbe Azure-ban](./media/virtual-machines-common-infrastructure-service-guidelines/example-config.png)

Ebben a konfigurációban a következőket tartalmazza:

- A felhőben virtuális-hálózaton: két alhálózat (FrontEnd és Kódmentes)
- Két tárterület-fiókok
- Az online tároló minden réteg egy négy elérhetősége beállítása
- A virtuális gépeken futó a négy réteg számára
- Egy külső terheléselosztás HTTPS-alapú webes forgalom az internetről a webkiszolgálóra beállítása
- Belső terheléselosztás titkosítatlan web-alapú forgalmat a web-kiszolgálókról az alkalmazáskiszolgálók beállítása
- Egységes erőforrás csoport


## <a name="next-steps"></a>Következő lépések

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 
<properties
    pageTitle="Log Analytics adatainak megoldás vezetékes |} Microsoft Azure"
    description="Vezetékes adata MOBILE anyagokkal, például Operations Manager és a Windows-kompatibilis ügynökök számítógépekről hálózattal és teljesítménnyel összesített adatok. Hálózati adatok a napló adatokkal, amelyek segítséget nyújtanak az adatok összehangolására fűzi össze."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="wire-data-solution-in-log-analytics"></a>Log Analytics adatainak megoldás vezetékes

Vezetékes adata MOBILE anyagokkal, például Operations Manager és a Windows-kompatibilis ügynökök számítógépekről hálózattal és teljesítménnyel összesített adatok. Hálózati adatok a napló adatokkal, amelyek segítséget nyújtanak az adatok összehangolására fűzi össze. Az informatikai infrastruktúrát monitor hálózati adatok küldött és az adott számítógépekről számítógépekre telepített MOBILE ügynökök hálózati szintek 2-3 – beleértve a különböző használható portok és protokollok [OSI modell](https://en.wikipedia.org/wiki/OSI_model) .

>[AZURE.NOTE] A átutalásra vonatkozó adatok megoldást jelenleg nem áll rendelkezésre felvenni kívánt munkaterületek. Azon ügyfelek, akik már rendelkezik a átutalásra vonatkozó adatok megoldást engedélyezett továbbra is használhatja a átutalásra vonatkozó adatok megoldás.

Alapértelmezés szerint MOBILE gyűjti össze a naplózott adatok Processzor, a memóriahasználat, a lemez és a hálózati teljesítmény adatok a Windows rendszerbe épített számláló. A hálózati és más adatok gyűjtése történik, az alábbi valós idejű minden ügynök, például alhálózat és a számítógép által használt alkalmazásszintű protokollok. A naplók lapon a beállítások lapon a többi teljesítmény számláló is hozzáadhat.

Ha [sFlow](http://www.sflow.org/) vagy más szoftver [Cisco's NetFlow](http://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html)protokollal felhasználta már, akkor a statisztika és az adatokat, látni fogja a átutalásra vonatkozó adatok lesz ismerős lehet.

A beépített napló keresési lekérdezések típusú a következők:

- Ügynökök is átutalásra vonatkozó adatok megadása
- IP-címe átutalásra vonatkozó adatokat ügynökök beállítása
- IP-címek szerinti kimenő kommunikáció számára
- Alkalmazás protokollok által küldött bájtok számát
- Az alkalmazás szolgáltatás által küldött bájtok számát
- A különböző protokollok megkapta bájt
- Az összes bájt által küldött és fogadott IP
- Az 10.0.0.0/8 alhálózat anyagokkal közölt IP-címek
- Biztos, hogy amelyeket mérése kapcsolatokhoz átlagos időtartama
- Számítógép folyamatok kezdeményezni, illetve a hálózati forgalmának engedélyezésére érkezett
- Egy folyamat hálózati forgalmának összege

A átutalásra vonatkozó adatokat kereséskor szűrheti és a felső ügynökök és a legjobb protokollokat kapcsolatos információk megtekintése csoport az adatokat. Vagy megtekintheti, hogy mikor az egyes számítógépekre (IP-címek és MAC címeket) kommunikált egymással, mennyi ideig és hogy mennyi adatot elküldésének – alapvetően, megtekintheti a hálózati forgalmat, amely keresésen alapuló metaadatait.

Azonban óta metaadatok tekinti, még nem feltétlenül hasznos részletes hibaelhárítása. Hálózati adatok teljes rögzítés MOBILE átutalásra vonatkozó adatok nem áll. Így nem célja mély csomag szintű hibaelhárítási.
Az ügynök és összehasonlítása más webhelycsoport módszerek használatával előnye, hogy nem kell telepíteni készülékek, a hálózati kapcsolók átkonfigurálása vagy preform bonyolult beállítások. Átutalásra vonatkozó adatok, akkor egyszerűen ügynök alapú – a agent meg telepíteni egy olyan számítógépen, és nyomon fogja követni a saját hálózati forgalmának engedélyezésére. Egy másik előnye, amikor a felhőben szolgáltatók vagy a szolgáltatás szolgáltatója vagy a Microsoft Azure, ahol a felhasználó nem rendelkezik a háló réteghez futó feladatok figyelni kívánt.

Mi történik a hálózaton, ha nem telepíti ügynökök a számítógépen a hálózati infrastruktúrára vonatkozó teljes láthatóság viszont nincs.

## <a name="installing-and-configuring-the-solution"></a>Való telepítéséről és konfigurálásáról a megoldás
Az alábbi információk segítségével telepítse és állítsa be a megoldást.

- A átutalásra vonatkozó adatok megoldást megszerzi a Windows Server 2012 R2, a Windows 8.1 és újabb operációs rendszert futtató számítógépeknek adatait.
- Microsoft .NET-keretrendszer 4.0-s vagy újabb operációs rendszerű számítógépeknél, amelyhez átutalásra vonatkozó adatok szerezheti be van szükség.
- A átutalásra vonatkozó adatok megoldás hozzáadása a MOBILE munkaterület [hozzáadása napló Analytics-megoldások a megoldástárból](log-analytics-add-solutions.md)leírt eljárással.  Nem szükséges, nincs további beállításokat.
- Ha szeretne egy adott megoldás átutalásra vonatkozó adatok megtekintése, kell a megoldást a MOBILE munkaterület már hozzáadott van.

## <a name="wire-data-data-collection-details"></a>Adatok gyűjtése Adatrészletek vezetékes

Átutalásra vonatkozó adatok gyűjti össze a metaadat-alapú hálózati forgalmának engedélyezésére az, hogy engedélyezte a ügynökök használatával kapcsolatban.

A következő táblázat mutatja az adatgyűjtési módszerek, és hogyan adatgyűjtés átutalásra vonatkozó adatok más adatait.


| Platform | Közvetlen Agent | SCOM agent | Azure tárhely | Kötelező SCOM? | E-kezelés csoport elküldött SCOM ügynök adatok | a webhelycsoport gyakorisága |
|---|---|---|---|---|---|---|
|A Windows (2012 R2 / 8.1-es vagy újabb verzió)|![igen](./media/log-analytics-wire-data/oms-bullet-green.png)|![igen](./media/log-analytics-wire-data/oms-bullet-green.png)|![nem](./media/log-analytics-wire-data/oms-bullet-red.png)|            ![nem](./media/log-analytics-wire-data/oms-bullet-red.png)|![nem](./media/log-analytics-wire-data/oms-bullet-red.png)| 1 percenként|


## <a name="combining-wire-data-with-other-solution-data"></a>Használjon vezetékes adatokat más megoldást adatokkal

Lehet, hogy a fent látható beépített lekérdezés által visszaadott adatok önmagában érdekes. Az használhatóságát átutalásra vonatkozó adatok létrejött azonban más MOBILE megoldások adatait kombinálva azt. Például a biztonság és naplózási megoldást által gyűjtött biztonsági esemény adatok használata és kombinálni átutalásra vonatkozó adatok szokatlan hálózati bejelentkezési kísérleteket elnevezett folyamatok keres.  Ebben a példában az a és a DISTINCT operátorok használna a keresési lekérdezés az adatpontok csatlakozni.

Követelmények: Használatához az alábbi példában, kell a biztonság és naplózási megoldást telepítve van. Azonban hasonló eredmény elérésére átutalásra vonatkozó adatok egyesítéséhez adatainak egyéb megoldások is használhatja.

### <a name="to-combine-wire-data-with-security-events"></a>Biztonsági eseményekkel átutalásra vonatkozó adatok egyesítéséhez

1. Az Áttekintés oldalon kattintson a **WireData** csempére.
2. Kattintson az **Általános WireData lekérdezések**listáját **Összege a hálózati adatforgalmat (bájt) folyamat által** visszaadott folyamatok listájának megjelenítéséhez.
    ![wiredata lekérdezések](./media/log-analytics-wire-data/oms-wiredata-01.png)
3. Ha túl sok időbe egyszerűen tekintheti folyamatok listáját, módosíthatja a keresési lekérdezést hasonló:

    ```
    Type WireData | measure count() by ProcessName | where AggregatedValue <40
    ```
    Az alábbi példában egy folyamat nevű DancingPigs.exe, mely lehet, hogy gyanús jelennek meg.
    ![wiredata keresési találatok](./media/log-analytics-wire-data/oms-wiredata-02.png)

4. A lista visszaadott adatok használata esetén kattintson egy névvel ellátott folyamat. Ebben a példában DancingPigs.exe kattintott. Az alább látható módon eredmények leírja a hálózati forgalmának engedélyezésére, például a kimenő kommunikáció különböző protokollon keresztül.
    ![névvel ellátott folyamatot bemutató wiredata eredménye](./media/log-analytics-wire-data/oms-wiredata-03.png)

5. A biztonság és naplózási megoldás telepítve van, mert a is probe be a keresési lekérdezés operátorokkal az a és a DISTINCT keresési lekérdezés módosításával Folyamatnév mező azonos értékkel rendelkező biztonsági eseményeket. Azt is megteheti majd amikor átutalásra vonatkozó adatok és a más megoldást naplók értéket tartalmaz ugyanazt a formátumot. Módosítsa a keresési lekérdezés hasonló:

    ```
    Type=SecurityEvent ProcessName IN {Type:WireData "DancingPigs.exe" | distinct ProcessName}
    ```    

    ![wiredata eredménye az egyesített adatok](./media/log-analytics-wire-data/oms-wiredata-04.png)
6. A fenti között láthatja, hogy látható-e a fiók adatait. Most már tovább finomítható a keresési lekérdezés megtudhatja, hogy milyen gyakran használta a fiókot, és a naplózási adatokat tartalmazó egy lekérdezés hasonlítanak a folyamatot:        

    ```
    Type=SecurityEvent ProcessName IN {Type:WireData "DancingPigs.exe" | distinct ProcessName} | measure count() by Account
    ```

    ![fiók adatait megjelenítő wiredata eredménye](./media/log-analytics-wire-data/oms-wiredata-05.png)



## <a name="next-steps"></a>Következő lépések

- [Keresés naplók](log-analytics-log-searches.md) részletes átutalásra vonatkozó adatok keresési rekordjainak megtekintéséhez.
- Látja, Dan's [átutalásra vonatkozó adatok használata műveletek Management csomagja napló keresési blogban közzé](http://blogs.msdn.com/b/dmuscett/archive/2015/09/09/using-wire-data-in-operations-management-suite.aspx) van további információkat, tudni, hogy milyen gyakran adatgyűjtés, és hogyan módosíthatja a webhelycsoport-tulajdonságai az Operations Manager ügynökök.

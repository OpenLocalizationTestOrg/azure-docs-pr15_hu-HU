<properties
    pageTitle="Log Analytics számítógép csoportok jelentkezzen keresések |} Microsoft Azure"
    description="A napló Analytics számítógép csoportok lehetővé teszik hatókör napló keresések számítógépen meghatározott csoportját.  Ez a cikk ismerteti a különböző módszereket használhatja a számítógép-csoportok és használatuk napló keresés létrehozása."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="bwren"/>

# <a name="computer-groups-in-log-analytics-log-searches"></a>A napló Analytics számítógép csoportok jelentkezzen keresések
Log Analytics számítógép csoportok lehetővé teszik hatókör [keresések jelentkezzen](log-analytics-log-searches.md) számítógépen meghatározott csoportját.  Minden csoport vagy lekérdezéssel, amely csak számítógépekkel vagy csoportok importálása különböző forrásokból származó van kitöltve.  Ha a csoport található a napló keresés, eredménye a számítógépek, a csoport egyező rekordok korlátozódik.

## <a name="creating-a-computer-group"></a>Számítógép a csoport létrehozása
A számítógép csoportban az alábbi táblázat módszerek bármelyikével napló Analytics hozhat létre.  Mindegyik módszernek részletes tudnivalókat az alábbi szakaszok találhatók. 

| A módszer | Leírás |
|:---|:---|
| Log keresés       | Hozzon létre egy napló keresést, amely visszaadja a számítógépek listáját, és az eredmények mentése a számítógépre csoport. |
| Jelentkezzen be a keresés API   | A napló keresési API segítségével a napló keresési eredmények alapján számítógép csoport programozás útján létrehozása. |
| Az Active Directory | A csoporttagság bármely, az Active Directory tartományi tagjai és az egyes biztonsági csoportok hozzon létre egy csoportot napló Analytics ügynök számítógépek automatikusan ellenőrizni.
| WSUS              | Automatikusan csoportok kiválasztásával WSUS-kiszolgálók és ügyfelek keresése és napló Analytics csoport létrehozása az egyes. |


### <a name="log-search"></a>Log keresés

Számítógép csoportok létrehozott naplófájl keresési tartalmazni fogja az összes, a számítógépek, amely csak keresési lekérdezés által visszaadott.  A lekérdezés futtatása minden alkalommal a számítógép csoportban használja, így a jelennek meg a csoport létrehozása óta módosításokat.

Az alábbi eljárással számítógép csoport létrehozásához a napló keresési.

1. [A napló keresés létrehozása](log-analytics-log-searches.md) , amely visszaadja a számítógépek listáját.  A keresés vissza kell térnie a számítógépek különböző halmazának valamit, például a **Különböző számítógépen** , vagy a **számítógép mérték count()** a lekérdezés használatával.  
2. Kattintson a **Mentés** gombra a képernyő tetején.
3. Válassza az **Igen gombra** kattintva **a lekérdezés mentése a számítógépre csoport:**.
4. Írja be a **nevét** és a csoport **kategóriát** .  Ha már létezik egy keresést, a név és a kategória, majd kéri felülírja.  A különböző kategóriák azonos nevű több keresések is. 

Az alábbiakban példa kereséseket, mint egy számítógép csoport mentheti.

    Computer="Computer1" OR Computer="Computer2" | distinct Computer 
    Computer=*srv* | measure count() by Computer

### <a name="log-search-api"></a>Jelentkezzen be a keresés API

A napló keresési API-val létrehozott számítógép csoportok megegyeznek a létrehozott naplófájl keresés keresések.

[Log Analytics számítógép csoportok jelentkezzen be a keresési REST API](log-analytics-log-search-api.md#computer-groups)kapcsolatban további részleteket a napló keresési API számítógépet a csoport létrehozása.

### <a name="active-directory"></a>Az Active Directory

Log Analytics importálhatja az Active Directory csoporttagság konfigurálásakor azt fogja elemzése a csoporttagság bármelyik tartományhoz csatlakoztatott számítógép MOBILE ügynök.  Egy számítógép csoport napló Analytics az egyes biztonsági csoportok, az Active Directory jön létre, és minden számítógép megjelenik a biztonsági csoportokat, amelyekhez tartoznak tartozó számítógép-csoportok.  A tagság 4 óránként folyamatosan frissülnek.  

Log Analytics importálhatja az Active Directory-biztonsági csoportokat a **Számítógép csoportok** menüből napló Analytics- **Beállítások**konfigurálása  Jelölje be **az automatizálási** , majd a **importálása az Active Directory csoporttagság számítógépekről**.  Nem szükséges, nincs további beállításokat.

![Az Active Directoryból számítógép-csoportok](media/log-analytics-computer-groups/configure-activedirectory.png)

Csoportok importálása, a menü felsorolásban, az észlelt csoport tagjai számítógépek és csoportok importált számát.  Kattinthat a ezeket a hivatkozásokat, ezt az információt a **ComputerGroup** rekordok közül.

### <a name="windows-server-update-service"></a>A Windows Server Update szolgáltatásba

Log Analytics WSUS csoporttagság importálandó konfigurálásakor azt fog elemzése a MOBILE agent bármilyen számítógépen célcsoport-kezelési csoporttagságát.  Ügyféloldali használata célba juttatása bármely olyan számítógépen, MOBILE csatlakozik, és bármelyik WSUS része csoportok kiválasztásával van napló Analytics importált csoport tagságát. Kiszolgálóoldali használatakor célba juttatása, a MOBILE ügynök kell telepíteni ahhoz, hogy a csoport tagsági információk MOBILE importálandó WSUS-kiszolgálón.  A tagság 4 óránként folyamatosan frissülnek. 

Log Analytics importálhatja az Active Directory-biztonsági csoportokat a **Számítógép csoportok** menüből napló Analytics- **Beállítások**konfigurálása  Válassza **Az Active Directory** , majd az **Importálás Active Directory csoporttagság számítógépekről**.  Nem szükséges, nincs további beállításokat.

![Az Active Directoryból számítógép-csoportok](media/log-analytics-computer-groups/configure-wsus.png)

Csoportok importálása, a menü felsorolásban, az észlelt csoport tagjai számítógépek és csoportok importált számát.  Kattinthat a ezeket a hivatkozásokat, ezt az információt a **ComputerGroup** rekordok közül.

## <a name="managing-computer-groups"></a>Számítógép csoportok kezelése

Számítógép csoportok létrehozott naplófájl keresés vagy a napló keresési API a **Számítógép csoportok** menüből napló Analytics- **Beállítások**megtekintése.  Kattintson az **x** az oszlop **eltávolítása** törli a számítógép csoportot.  Kattintson a **tagok megjelenítése** ikonra kattintva futtassa a csoport napló keresést, amely visszaadja a tagok csoport. 

![Mentett számítógép-csoportok](media/log-analytics-computer-groups/configure-saved.png)

Ha módosítani szeretné a csoportot, az új csoport létrehozása az eredeti csoportban felülírja az azonos **kategória** és **nevével** .

## <a name="using-a-computer-group-in-a-log-search"></a>A napló keresés számítógép csoport használata
A következő szintaxissal segítségével olvassa el a napló keresés számítógépen csoportban.  A **kategória** , nem kötelező, de csak megadása esetén azonos nevű számítógép-csoportok a különböző kategóriák szükséges. 

    $ComputerGroups[Category: Name]

Amikor keresés futtatásához a keresés kiterjed számítógép csoportokat tagjai először szünteti meg.  Ha a csoport napló keresés alapul, majd, hogy a keresés való visszatéréshez a csoport tagjainak a legfelső szintű napló keresés végrehajtása előtt futtassa a függvény.

A napló keresés, ahogy a következő példában az **IN** záradék a számítógép csoportok általában használják.

    Type=UpdateSummary Computer IN $ComputerGroups[My Computer Group]

## <a name="computer-group-records"></a>Számítógép rekordok csoportosítása

A MOBILE tárháza minden számítógép csoporttagság, az Active Directory vagy WSUS létrehozott rekord jön létre.  Ezeket a rekordokat **ComputerGroup** típusú, és tárolja az a tulajdonságokat az alábbi táblázatban.  Rekordokat nem jön létre számítógép csoportok napló keresések alapján.

| A tulajdonság | Leírás |
|:--|:--|
| Típus                | *ComputerGroup* |
| SourceSystem        | *SourceSystem*  |
| Számítógép            | Tag számítógép nevét. |
| Csoport               | A csoport nevét. |
| GroupFullName       | Teljes elérési útját a csoportba, beleértve a forrást, és az adatforrás neve.
| GroupSource         | A forrás csoport begyűjtött volt. <br><br>Active Directory –<br>WSUS<br>WSUSClientTargeting |
| GroupSourceName     | A forrásból származó összegyűjtött a csoportok neve.  Az Active Directory Ez az a tartomány nevét. |
| ManagementGroupName | A kezelés csoport SCOM ügynökök nevét.  Más ügynökök, ez az AOI -\<munkaterület azonosító\> |
| TimeGenerated       | Dátum és idő a számítógép csoport létrehozásakor vagy frissítésekor. |



## <a name="next-steps"></a>Következő lépések

- [Log keresések](log-analytics-log-searches.md) megismerheti az adatforrások és megoldások gyűjtött adatok elemzése céljából.  
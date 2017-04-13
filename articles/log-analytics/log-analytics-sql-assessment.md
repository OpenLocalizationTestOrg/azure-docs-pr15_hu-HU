<properties
    pageTitle="A környezet a SQL értékelési megoldásban napló Analytics optimalizálása |} Microsoft Azure"
    description="Mérje fel, hogy a kockázatok és a környezetek állapotának rendszeres időközönként használhatja az SQL-értékelési megoldás."
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

# <a name="optimize-your-environment-with-the-sql-assessment-solution-in-log-analytics"></a>A környezet a SQL értékelési megoldásban napló Analytics optimalizálása


Mérje fel, hogy a kockázatok és a környezetek állapotának rendszeres időközönként használhatja az SQL-értékelési megoldás. Ez a cikk segít a megoldás telepítése, így a potenciális problémákról korrekciós műveletek hajthatók végre.

Ez a megoldást itt javaslatok a telepített kiszolgáló infrastruktúrájába adott elsőbbségi listáját. A javaslatok kategóriába keresztül hat fókusz területen, amely segít gyorsan áttekintheti a kockázat és helyesbítő művelet végrehajtása.

A tett javaslatok ismeretekkel és a Microsoft szakemberei ügyfél tartománynevére ezer tapasztalatai alapulnak. Minden egyes ajánlási problémát előfordulhat, hogy fontossága Önnek és a javasolt módosításokat megvalósításáról ismerteti.

Megadhatja, hogy a fókusz területeket, amelyek a legfontosabb, hogy szervezete, és a nyilvántartása fut a kockázat ingyenes, és a megfelelő környezet felé.

Miután felvette a megoldást, és annak kiértékelése kitölteni, összefoglaló fókusz funkcióival kapcsolatos információk jelennek meg az **SQL-értékelési** irányítópulton a infrastruktúrájának a környezetben. Az alábbi szakaszok ismertetik, hogyan lehet az **SQL-értékelési** irányítópulton, ahol megtekintheti, és ezután műveletek ajánlott az SQL server infrastruktúra adatokat használja.

![SQL-értékelési csempe képe](./media/log-analytics-sql-assessment/sql-assess-tile.png)

![SQL-értékelési irányítópult képe](./media/log-analytics-sql-assessment/sql-assess-dash.png)

## <a name="installing-and-configuring-the-solution"></a>Való telepítéséről és konfigurálásáról a megoldás
SQL-felmérés működik-e az SQL Server Standard, fejlesztőeszközök és vállalati kiadásai jelenleg támogatott verziója.

Az alábbi információk segítségével telepítse és állítsa be a megoldást.

- Ügynökök telepíteni kell az SQL Server telepítve van kiszolgálókon.
- A felmérés SQL megoldást szükséges .NET-keretrendszer 4 telepített összes olyan számítógépen, amelyen a MOBILE ügynökszoftvert.
- Az SQL-értékelése az Operations Manager ügynök használatakor kell műveletek Manager Run-As fiókot használ. További információt talál [Operations Manager Futtatás fiókok MOBILE](#operations-manager-run-as-accounts-for-oms) alatt.

    >[AZURE.NOTE] A MMA agent nem támogatja a műveletek Manager Run-As fiókok.

- A felmérés SQL megoldás hozzáadása a MOBILE munkaterület [hozzáadása napló Analytics-megoldások a megoldástárból](log-analytics-add-solutions.md)leírt eljárással. Nem szükséges, nincs további beállításokat.

>[AZURE.NOTE] Miután felvette a megoldást, anyagokkal kiszolgálók bekerül a AdvisorAssessment.exe fájlt. Konfigurációs adatainak olvasása és elküldi a feldolgozás a felhőben MOBILE service. Logika a kapott adatokon alkalmazott, és a felhőbeli szolgáltatástól rekordok az adatokat.

## <a name="sql-assessment-data-collection-details"></a>A webhelycsoport Adatrészletek SQL értékelése

SQL-értékelési gyűjti össze a WMI adatok, rendszerleíró adatokat, a Teljesítményadatok és SQL Server dinamikus kezelése eredményeinek megtekintése, hogy engedélyezve van a ügynökök használatával.

A következő táblázat adatgyűjtési módszerek ügynökök, a műveletek Manager (SCOM) szükséges-e, és gyakran adatait gyűjti ügynökszoftvert.

| Platform | Közvetlen Agent | SCOM agent | Azure tárhely | Kötelező SCOM? | E-kezelés csoport elküldött SCOM ügynök adatok | a webhelycsoport gyakorisága |
|---|---|---|---|---|---|---|
|A Windows|![igen](./media/log-analytics-sql-assessment/oms-bullet-green.png)|![igen](./media/log-analytics-sql-assessment/oms-bullet-green.png)|![nem](./media/log-analytics-sql-assessment/oms-bullet-red.png)|    ![nem](./media/log-analytics-sql-assessment/oms-bullet-red.png)|![igen](./media/log-analytics-sql-assessment/oms-bullet-green.png)|   7 nap|

## <a name="operations-manager-run-as-accounts-for-oms"></a>Futtatás Operations Manager számlák MOBILE

Bejelentkezés az Analytics MOBILE használja a Operations Manager ügynök és a kezelés csoport gyűjti össze, valamint adatokat küld a MOBILE szolgáltatás. MOBILE buildjeiben megadására munkaterhelésekből management csomagot, érték-szolgáltatások hozzáadása. Minden egyes terhelést terhelést-specifikus jogosultságok kezelése csomagok futtatásához egy másik biztonsági környezetben, például a tartományi fiók szükséges. Meg kell adnia a hitelesítő adatok műveletek Manager Futtatás mint fiók beállításával.

Az alábbi információk segítségével állítsa be a Futtatás fiókját Operations Manager SQL felméréshez.

### <a name="set-the-run-as-account-for-sql-assessment"></a>Állítsa be a Futtatás mint fiókot SQL felméréshez

 Ha már használja az SQL Server felügyeleti csomag, a Futtatás mint fiók kell használnia.

#### <a name="to-configure-the-sql-run-as-account-in-the-operations-console"></a>A Futtatás SQL más néven fiók konfigurálása a műveletek konzolban

>[AZURE.NOTE] A SCOM agent helyett a MOBILE közvetlen agent használja, ha a felügyeleti csomag mindig fut a helyi rendszerfiók biztonsági környezetében. Lépések 1-5 az alábbi átugrása, és futtassa a Powershell minta NT AUTHORITY\SYSTEM megadása a felhasználó nevét, vagy az SQL-T.

1. Nyissa meg a műveletek konzolt Operations Manager, és válassza a **felügyeleti**.

2. **Futtatása konfigurációs**kattintson a **profilok**gombra, és nyissa meg a **MOBILE SQL értékelési futtatása profilként**.

3. A **Futtatás fiókok** lapon kattintson a **Hozzáadás**gombra.

4. Jelölje ki a Windows Futtatás mint fiókot, amely tartalmazza az SQL Server szükséges hitelesítő adatait, vagy kattintson az **Új** hozhat létre egyet.
    >[AZURE.NOTE] A Futtatás fiók típusa Windows kell lennie. A Futtatás mint fiókot is helyi Rendszergazdák csoport szolgáltatója SQL Server-példány minden Windows kiszolgálókon részének kell lennie.

5. Kattintson a **Mentés**gombra.

6. Módosíthatja, és ezután hajtsa végre a következő példa az SQL-T minden SQL Server-példányt Futtatás fiókot SQL értékelési végrehajtásához szükséges minimum engedélyeket szeretne adni. Azonban nem kell tennie, ha egy futtatása mint fiókkal már a rendszergazdák kiszolgálói szerepkör az SQL Server-példány része.

```
---
    -- Replace <UserName> with the actual user name being used as Run As Account.
    USE master

    -- Create login for the user, comment this line if login is already created.
    CREATE LOGIN [<UserName>] FROM WINDOWS

    -- Grant permissions to user.
    GRANT VIEW SERVER STATE TO [<UserName>]
    GRANT VIEW ANY DEFINITION TO [<UserName>]
    GRANT VIEW ANY DATABASE TO [<UserName>]

    -- Add database user for all the databases on SQL Server Instance, this is required for connecting to individual databases.
    -- NOTE: This command must be run anytime new databases are added to SQL Server instances.
    EXEC sp_msforeachdb N'USE [?]; CREATE USER [<UserName>] FOR LOGIN [<UserName>];'

```
#### <a name="to-configure-the-sql-run-as-account-using-windows-powershell"></a>A Windows PowerShell szolgáltatással SQL Futtatás mint fiók konfigurálása

Nyisson meg egy PowerShell-ablakot, és futtassa az alábbi parancsfájlt, miután frissítette a adatokkal:

```

    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"
     
    $profile = Get-SCOMRunAsProfile -DisplayName "OMS SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```

## <a name="understanding-how-recommendations-are-prioritized"></a>Hogyan javaslatok rangsorolt vannak ismertetése

Minden elvégzett ajánlási súlyozási érték, amely azonosítja az ajánlási relatív fontosságát kap. A tíz legfontosabb ajánlást jelennek meg.

### <a name="how-weights-are-calculated"></a>Hogyan súlyok számítása

Súlyok három főbb tényezők alapján összesített értékek:

- *Annak valószínűsége* , hogy problémákat okozhatnak problémát azonosította lesz. Nagyobb valószínűséggel nagyobb általános pontszám az ajánlása megfelel.

- A *hatás* a problémát, ha ezt a hibát okozhat a. Nagyobb általános pontszám az ajánlása megfelel egy nagyobb hatást.

- A *munkamennyiség* az ajánlási végrehajtásához szükséges. Egy kisebb általános pontot ajánlása megfelel egy újabb munkamennyiség.

Az egyes ajánlási súlyozásának százalékban a teljes pontszám mindegyik fókusz területén érhető el. Például ha a biztonság és megfelelőség fókusz területen ajánlást 5 %-os pontszám, adott ajánlási végrehajtási növelik a teljes biztonság és megfelelőség pontszám alapján 5 %-át.

### <a name="focus-areas"></a>Aktuális terület

**Biztonság és megfelelőség** - e a fókusz területen látható potenciális biztonsági kockázatok és szabályok megsértésével, vállalati házirend és a műszaki, jogi és szabályozói megfelelőségi követelmények javaslatok.

**Elérhetőségéről és az üzleti folytonosságot** - e a fókusz területen látható szolgáltatáselérhetőség, infrastruktúra és üzleti védelem tűrőképessége javaslatok.

**Teljesítmény és méretezhetőség** - e a fókusz területen látható javaslatok segíti szervezete informatikai infrastruktúrát a nagyobb, győződjön meg arról, hogy a környezetben aktuális teljesítmény megfelel, és válaszolhat infrastruktúra igényeinek módosítása.

Javaslatok frissítése, áttelepítéséhez, és az SQL Server telepítése a meglévő infrastruktúrájába **frissítéséhez áttelepítési és telepítési** - e a fókusz területen látható.

**Műveletek és figyelése** - e a fókusz területen látható az IT-műveletek egyszerűbb, a megelőző karbantartás végrehajtása és a teljesítmény maximalizálása javaslatok.

**Módosítása és a modellkonfigurációk kezelésére** - e a fókusz területen látható javaslatok segíti a mindennapos tevékenységek védelme, biztosítására, hogy módosítások nem negatív hatással vannak az infrastruktúra eljárásokat módosítása vezérlőt, és nyomon követésére és naplózási beállításokat a rendszer.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Érdemes célja 100 %-os pontszám minden fókusz területén?

Nem feltétlenül. A javaslatok ismeretekkel és tapasztalatok Microsoft mérnökök több ügyfél tartománynevére ezer alapulnak. Azonban nem két kiszolgáló infrastruktúra megegyeznek, és lehetséges, hogy több vagy kevesebb releváns ajánl. Például biztonsági tanácsokat valószínűleg kevésbé fontos, ha a virtuális gépeken futó nem láthatóak az internethez. Lehet, hogy kevésbé releváns az alacsony prioritású alkalmi adatgyűjtés és jelentéskészítés nyújtó szolgáltatások elérhetősége tanácsokat. Lehet, hogy kevesebb fontos, hogy egy indítási elért üzleti fontos problémákat. Előfordulhat, hogy szeretné azonosítani a fókusz területeket a prioritásának, és keresse meg, hogyan az értékek időbeli változását.

Minden ajánlási arról, hogy miért fontos útmutatást tartalmaz. Az útmutató segítségével kell-e az ajánlási végrehajtási megfelelő, a informatikai szolgáltatások jellegét és a szervezet az üzleti igények felmérése.

## <a name="use-assessment-focus-area-recommendations"></a>Felmérés fókusz terület javaslatok használata

A MOBILE egy értékelést megoldást is használhatja, mielőtt a megoldást, telepítve kell rendelkeznie. További információ a megoldások telepítéséről megoldásainak [napló Analytics hozzáadása a megoldástárból](log-analytics-add-solutions.md). Telepítve van, miután megtekintheti a javaslatok összefoglalása az SQL-értékelési csempe az Áttekintés lapon, a MOBILE használatával.

Az összegzett megfelelőségi felmérések a infrastruktúra, és a-feltárás javaslatok megtekintése.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>A fókusz terület javaslatok megtekintése és helyesbítő művelet végrehajtása

1. Az **Áttekintés** oldalon kattintson a **Felmérés SQL** csempére.
2. Az **SQL-értékelési** lapon tekintse át az összegzett adatokat, az aktuális terület rögzítéséhez egyik, és kattintson az egyik fókusz területre javaslatok megtekintése.
3. Bármelyik terület fókusz lapot megtekintheti az elsőbbségi ajánlások arról, hogy a környezetben. Kattintson a ajánlása az **Érintett objektumok** Miért történik, az ajánlási adatainak megjelenítéséhez.  
    ![SQL-értékelési javaslatok képe](./media/log-analytics-sql-assessment/sql-assess-focus.png)
4. A **Javasolt műveleteket**javasolt korrekciós műveletek hajthatók végre. A cikk a már foglalkozott, amikor újabb felmérések rögzíteni fogja műveletek készítésének és a megfelelőség pontszámhoz növelik ajánlott. Javított elemek **Átadott objektumok**jelennek meg.

## <a name="ignore-recommendations"></a>Figyelmen kívül hagyja a javaslatok

Ha figyelmen kívül hagyása kívánt javaslatok, létrehozhat egy szövegfájlt, amelyekkel a MOBILE megakadályozhatja, hogy megjelenjenek a felmérés eredmény javaslatok.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Javaslatok, amely akkor figyelmen kívül hagyja a azonosítása

1.  Jelentkezzen be a munkaterület, és nyissa meg a napló keresési. Az alábbi lekérdezéssel lista ajánlások, amely nem sikerült a számítógépen a környezetben.

    ```
    Type=SQLAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    Képernyőkép a napló keresési lekérdezés az alábbiakban: ![nem sikerült a javaslatok](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)

2.  Válassza ki a javaslatok, amelyet figyelmen kívül hagyja. Az értékeket az RecommendationId a következő eljárással megadnia.


### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Hozhat létre és használhat egy IgnoreRecommendations.txt szövegfájl

1.  Hozzon létre egy IgnoreRecommendations.txt nevű fájlt.
2.  Illessze be, vagy írja be az egyes RecommendationId az egyes ajánlási szeretné, hogy a MOBILE figyelmen kívül hagyása külön sorban, és mentse és zárja be a fájlt.
3.  Akkor a dokumentumot a következő mappában minden olyan számítógépen, amelyre MOBILE figyelmen kívül hagyja a javaslatok.
    - A számítógépen a Microsoft figyelése ügynök (közvetlenül vagy Operations Manager keresztül csatlakoztatott) - *SystemDrive*: \Program Files\Microsoft figyelés Agent\Agent
    - A kiszolgálón futó Operations Manager management - *SystemDrive*: \Program Files\Microsoft System Center 2012 R2\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Ellenőrizze, hogy a javaslatok figyelmen kívül hagyja

1.  Után a következő ütemezett a felmérés fut, alapértelmezés szerint minden 7 nap, a megadott javaslatok figyelmen kívül vannak megjelölve, és nem jelennek meg értékelést irányítópult.
2.  A következő napló keresési lekérdezések használhatja a figyelmen kívül hagyott javaslatok listáját.

    ```
    Type=SQLAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```
3.  Ha később úgy dönt, hogy megjelenítendő figyelmen kívül hagyott javaslatok, távolít el IgnoreRecommendations.txt fájlokat, vagy RecommendationIDs eltávolíthatja őket.

## <a name="sql-assessment-solution-faq"></a>SQL-értékelési megoldás – gyakori kérdések

*Milyen gyakran fut a értékelést?*
- A felmérés minden napján fut.

*Van-e lehetőség beállítása, hogy milyen gyakran fusson a a felmérés?*
- Nem adott időben.

*Ha másik kiszolgálónak a SQL értékelési megoldást-tárterülethez merül fel, azt értékelendő?*
- Igen, miután kiderül, meg kell értékelni, majd a minden napján.

*Ha egy kiszolgáló leszereltek, amikor azt kikerül a felmérés?*
- Ha egy kiszolgáló nem 3 hétig adatok elküldése, törlődik.

*Mi az a folyamat adatgyűjtési fejlesztő neve?*
- AdvisorAssessment.exe

*Mennyi ideig tart a gyűjtendő adatok?*
- A tényleges adatok gyűjtése a kiszolgálón körülbelül 1 óra értéket veszi. Eltarthat hosszabb rendelkezik adatbázisoknál és az SQL Server-példányok nagyszámú kiszolgálókon.

*Milyen típusú adatot kell felvenni?*
- A következő típusú adatok begyűjtési:
    - WMI
    - Beállításjegyzék
    - Teljesítmény számláló
    - SQL-dinamikus kezelése nézetek (DMV).

*Van-e olyan módon, amikor adatgyűjtés konfigurálása?*
- Nem adott időben.

*Miért kell futtatni, fiók beállítása?*
- Az SQL Server az SQL-lekérdezések kisszámú futnak. Sorrendben szeretné futtatni a Futtatás másként fiókot SQL NÉZET kiszolgáló állapota jogosultsággal rendelkező kell használni.  Ezeken kívül annak érdekében, hogy a lekérdezés WMI, helyi rendszergazdai hitelesítő adatok szükségesek.

*Miért jelennek meg a 10 legjobb javaslatok csak?*
- Helyett, biztosítva a tevékenységek teljes ellenőrizni listáját, azt javasoljuk, hogy először megcímezheti az elsőbbségi ajánlások koncentrálhat. Miután orvosolja azokat, további javaslatok elérhetővé válik. Ha szeretné látni a részletes listát, a MOBILE napló keresése szolgáltatással az összes javaslatok tekinthet meg.

*Van-e lehetőség figyelmen kívül hagyja a ajánlást?*
- Igen, a [figyelmen kívül hagyása](#ignore-recommendations) javaslatok fenti szakasz.



## <a name="next-steps"></a>Következő lépések

- [Keresés naplók](log-analytics-log-searches.md) részletes SQL értékelési adatokat és javaslatok megtekintése.

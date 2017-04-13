<properties
    pageTitle="A környezet, az Active Directory értékelési megoldás napló Analytics optimalizálása |} Microsoft Azure"
    description="Mérje fel, hogy a kockázatok és a környezetek állapotának rendszeres időközönként használhatja az Active Directory értékelési megoldás."
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
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="optimize-your-environment-with-the-active-directory-assessment-solution-in-log-analytics"></a>A környezet, az Active Directory értékelési megoldás napló Analytics optimalizálása

Mérje fel, hogy a kockázatok és a környezetek állapotának rendszeres időközönként használhatja az Active Directory értékelési megoldás. Ez a cikk segít telepítésével és használatával a megoldást, így a potenciális problémákról korrekciós műveletek hajthatók végre.

Ez a megoldást itt javaslatok a telepített kiszolgáló infrastruktúrájába adott elsőbbségi listáját. A javaslatok kategóriába négy különböző fókusz területen, amely segít gyorsan áttekintheti a kockázat, a művelet végrehajtása.

A javaslatok ismeretekkel és a Microsoft szakemberei ügyfél tartománynevére ezer tapasztalatai alapulnak. Minden egyes ajánlási problémát előfordulhat, hogy fontossága Önnek és a javasolt módosításokat megvalósításáról ismerteti.

Megadhatja, hogy a fókusz területeket, amelyek a legfontosabb, hogy szervezete, és a nyilvántartása fut a kockázat ingyenes, és a megfelelő környezet felé.

Miután felvette a megoldást, és annak kiértékelése kitölteni, összefoglaló fókusz funkcióival kapcsolatos információk jelennek meg az **Active Directory értékelési** irányítópulton a infrastruktúrájának a környezetben. Az alábbi szakaszok ismertetik, hogyan lehet adatokat használja az **Active Directory értékelési** irányítópulton, ahol megtekintheti, és ezután műveletek ajánlott az Active Directory-kiszolgáló infrastruktúra.

![SQL-értékelési csempe képe](./media/log-analytics-ad-assessment/ad-tile.png)

![SQL-értékelési irányítópult képe](./media/log-analytics-ad-assessment/ad-assessment.png)


## <a name="installing-and-configuring-the-solution"></a>Való telepítéséről és konfigurálásáról a megoldás
Az alábbi információk segítségével telepítse és állítsa be a megoldásokkal.

- Ügynökök telepíteni kell a tartomány vezérlők, amelyek a tartomány tagjai ki kell értékelni.
- Az Active Directory értékelési megoldás szükséges .NET-keretrendszer 4 telepített összes olyan számítógépen, amelyen a MOBILE ügynökszoftvert.
- Az Active Directory értékelési megoldás hozzáadása a MOBILE munkaterület [hozzáadása napló Analytics-megoldások a megoldástárból](log-analytics-add-solutions.md)leírt eljárással.  Nem szükséges, nincs további beállításokat.

    >[AZURE.NOTE] Miután felvette a megoldást, anyagokkal kiszolgálók bekerül a AdvisorAssessment.exe fájlt. Konfigurációs adatainak olvasása és elküldi a feldolgozás a felhőben MOBILE service. Logika a kapott adatokon alkalmazott, és a felhőbeli szolgáltatástól rekordok az adatokat.

## <a name="active-directory-assessment-data-collection-details"></a>Az Active Directory értékelési webhelycsoport Adatrészletek

Az Active Directory értékelési gyűjti össze a WMI adatok, rendszerleíró adatok és a teljesítményadatok az ügynökök, hogy engedélyezve van.

A következő táblázat adatgyűjtési módszerek ügynökök, a műveletek Manager (SCOM) szükséges-e, és gyakran adatait gyűjti ügynökszoftvert.

| Platform | Közvetlen Agent | SCOM agent | Azure tárhely | Kötelező SCOM? | E-kezelés csoport elküldött SCOM ügynök adatok | a webhelycsoport gyakorisága |
|---|---|---|---|---|---|---|
|A Windows|![igen](./media/log-analytics-ad-assessment/oms-bullet-green.png)|![igen](./media/log-analytics-ad-assessment/oms-bullet-green.png)|![nem](./media/log-analytics-ad-assessment/oms-bullet-red.png)|   ![nem](./media/log-analytics-ad-assessment/oms-bullet-red.png)|![igen](./media/log-analytics-ad-assessment/oms-bullet-green.png)| 7 nap|


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

Javaslatok segítséget frissítése, áttelepítése és az Active Directory telepítése a meglévő infrastruktúrájába **frissítéséhez áttelepítési és telepítési** - e a fókusz területen látható.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Érdemes célja 100 %-os pontszám minden fókusz területén?

Nem feltétlenül. A javaslatok ismeretekkel és tapasztalatok Microsoft mérnökök több ügyfél tartománynevére ezer alapulnak. Azonban nem két kiszolgáló infrastruktúra megegyeznek, és lehetséges, hogy több vagy kevesebb releváns ajánl. Például biztonsági tanácsokat valószínűleg kevésbé fontos, ha a virtuális gépeken futó nem láthatóak az internethez. Lehet, hogy kevésbé releváns az alacsony prioritású alkalmi adatgyűjtés és jelentéskészítés nyújtó szolgáltatások elérhetősége tanácsokat. Lehet, hogy kevesebb fontos, hogy egy indítási elért üzleti fontos problémákat. Előfordulhat, hogy szeretné azonosítani a fókusz területeket a prioritásának, és keresse meg, hogyan az értékek időbeli változását.

Minden ajánlási arról, hogy miért fontos útmutatást tartalmaz. Az útmutató segítségével kell-e az ajánlási végrehajtási megfelelő, a informatikai szolgáltatások jellegét és a szervezet az üzleti igények felmérése.

## <a name="use-assessment-focus-area-recommendations"></a>Felmérés fókusz terület javaslatok használata

A MOBILE egy értékelést megoldást is használhatja, mielőtt a megoldást, telepítve kell rendelkeznie. További információ a megoldások telepítéséről megoldásainak [napló Analytics hozzáadása a megoldástárból](log-analytics-add-solutions.md). Miután telepítve van, megtekintheti a javaslatok összefoglalása a felmérés csempére az Áttekintés lapon, a MOBILE használatával.

Az összegzett megfelelőségi felmérések a infrastruktúra, és a-feltárás javaslatok megtekintése.


### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>A fókusz terület javaslatok megtekintése és helyesbítő művelet végrehajtása

1. Az **Áttekintés** oldalon kattintson a **felmérés** csempét a kiszolgáló infrastruktúra.
2. A **felmérés** lapon tekintse át az összegzett adatokat, az aktuális terület rögzítéséhez egyik, és kattintson az egyik javaslatok, hogy a fókusz terület megtekintéséhez.
3. Bármelyik terület fókusz lapot megtekintheti az elsőbbségi ajánlások arról, hogy a környezetben. Kattintson a ajánlása az **Érintett objektumok** Miért történik, az ajánlási adatainak megjelenítéséhez.  
    ![Felmérés javaslatok képe](./media/log-analytics-ad-assessment/ad-focus.png)
4. A **Javasolt műveleteket**javasolt korrekciós műveletek hajthatók végre. A cikk a már foglalkozott, amikor újabb felmérések rögzíteni fogja műveletek készítésének és a megfelelőség pontszámhoz növelik ajánlott. Javított elemek **Átadott objektumok**jelennek meg.

## <a name="ignore-recommendations"></a>Figyelmen kívül hagyja a javaslatok

Ha figyelmen kívül hagyása kívánt javaslatok, létrehozhat egy szövegfájlt, amelyekkel a MOBILE megakadályozhatja, hogy megjelenjenek a felmérés eredmény javaslatok.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Javaslatok, amely akkor figyelmen kívül hagyja a azonosítása

1.  Jelentkezzen be a munkaterület, és nyissa meg a napló keresési. Az alábbi lekérdezéssel lista ajánlások, amely nem sikerült a számítógépen a környezetben.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    Képernyőkép a napló keresési lekérdezés az alábbiakban: ![nem sikerült a javaslatok](./media/log-analytics-ad-assessment/ad-failed-recommendations.png)

2.  Válassza ki a javaslatok, amelyet figyelmen kívül hagyja. Az értékeket az RecommendationId a következő eljárással megadnia.


### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Hozhat létre és használhat egy IgnoreRecommendations.txt szövegfájl

1.  Hozzon létre egy IgnoreRecommendations.txt nevű fájlt.
2.  Illessze be, vagy írja be az egyes RecommendationId az egyes ajánlási szeretné, hogy a napló Analytics figyelmen kívül hagyása külön sorban, és mentse és zárja be a fájlt.
3.  Akkor a dokumentumot a következő mappában minden olyan számítógépen, amelyre MOBILE figyelmen kívül hagyja a javaslatok.
    - A számítógépen a Microsoft figyelése ügynök (közvetlenül vagy Operations Manager keresztül csatlakoztatott) - *SystemDrive*: \Program Files\Microsoft figyelés Agent\Agent
    - A kiszolgálón futó Operations Manager management - *SystemDrive*: \Program Files\Microsoft System Center 2012 R2\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Ellenőrizze, hogy a javaslatok figyelmen kívül hagyja

Után a következő ütemezett a felmérés fut, alapértelmezés szerint minden 7 nap, a megadott javaslatok *figyelmen kívül* vannak megjelölve, és nem jelennek meg értékelést irányítópult.

1. A következő napló keresési lekérdezések használhatja a figyelmen kívül hagyott javaslatok listáját.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

2.  Ha később úgy dönt, hogy megjelenítendő figyelmen kívül hagyott javaslatok, távolít el IgnoreRecommendations.txt fájlokat, vagy RecommendationIDs eltávolíthatja őket.

## <a name="ad-assessment-solutions-faq"></a>Active Directory értékelési megoldások – gyakori kérdések

*Milyen gyakran fut a értékelést?*
- A felmérés minden napján fut.

*Van-e lehetőség beállítása, hogy milyen gyakran fusson a a felmérés?*
- Nem adott időben.

*Ha egy másik kiszolgáló-tárterülethez egy értékelést megoldás merül fel, azt értékelendő?*
- Igen, miután kiderül, meg kell értékelni, majd a minden napján.

*Ha egy kiszolgáló leszereltek, amikor azt kikerül a felmérés?*
- Ha egy kiszolgáló nem 3 hétig adatok elküldése, törlődik.

*Mi az a folyamat adatgyűjtési fejlesztő neve?*
- AdvisorAssessment.exe

*Mennyi ideig tart a gyűjtendő adatok?*
- A tényleges adatok gyűjtése a kiszolgálón körülbelül 1 óra értéket veszi. Hosszabb időt vehet az Active Directory-kiszolgálók nagy számú kiszolgálókon.

*Milyen típusú adatot kell felvenni?*
- A következő típusú adatok begyűjtési:
    - WMI
    - Beállításjegyzék
    - Teljesítmény számláló

*Van-e olyan módon, amikor adatgyűjtés konfigurálása?*
- Nem adott időben.

*Miért jelennek meg a 10 legjobb javaslatok csak?*
- Helyett, biztosítva a tevékenységek teljes ellenőrizni listáját, azt javasoljuk, hogy először megcímezheti az elsőbbségi ajánlások koncentrálhat. Miután orvosolja azokat, további javaslatok elérhetővé válik. Ha szeretné látni a részletes listát, napló keresése szolgáltatással az összes javaslatok tekinthet meg.

*Van-e lehetőség figyelmen kívül hagyja a ajánlást?*
- Igen, a [figyelmen kívül hagyása](#ignore-recommendations) javaslatok fenti szakasz.


## <a name="next-steps"></a>Következő lépések

- [Log Analytics napló keresések](log-analytics-log-searches.md) segítségével részletes AD értékelési adatokat és javaslatok megtekintése.

<properties
    pageTitle="Betöltési tesztelheti az alkalmazást a Visual Studio Team Services használatával |} Microsoft Azure"
    description="Megtudhatja, hogy miként terhelési tesztelést az Azure Service háló alkalmazások Visual Studio Team Services használatával."
    services="service-fabric"
    documentationCenter="na"
    authors="cawams"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="07/29/2016"
    ms.author="cawa" />

# <a name="load-test-your-application-by-using-visual-studio-team-services"></a>Betöltési tesztelheti az alkalmazást a Visual Studio Team Services használatával

Ebből a cikkből megtudhatja használatáról a Microsoft Visual Studio betöltés funkciók kipróbálása terhelési tesztelést egy alkalmazást. Állapot-nyilvántartó szolgáltatás vissza véget Azure Service háló és egy állapot nélküli szolgáltatás előtér használ. Például alkalmazástól itt egy repülőgép hely simulatort használja. Megad egy repülőgép azonosítója, a indulási idő és a cél. Az alkalmazás vissza vége kérelmeket dolgozza fel, majd az előtér megjelenítése a térképen a repülőgép, amely megfelel a feltételeknek.

Az alábbi ábra szemlélteti fogja tesztelése szolgáltatás háló alkalmazás.

![A példa repülőgép hely alkalmazás ábrája][0]

## <a name="prerequisites"></a>Előfeltételek
Első lépések előtt kell tegye a következőket:

- Visual Studio Team Services-fiók beszerzése. Vásárolhat egyet ingyen [Visual Studio Team Services](https://www.visualstudio.com).
- Első, és telepítse az Visual Studio 2013 vagy a Visual Studio 2015. Ez a cikk a Visual Studio Skype 2015 vállalati edition használ, de a Visual Studio 2013 és a többi kiadás használata hasonlóan.
- A fejlesztői környezet az alkalmazás telepítéséhez. Megtudhatja, [hogy miként helyezhetnek üzembe a távoli fürtre Visual Studio segítségével az alkalmazásokat](service-fabric-publish-app-remote-cluster.md) az alábbi információra kíváncsi.
- Ismerje meg az alkalmazás használatát mintát. Ezt az információt a betöltés minta hasonlóan használják.
- Ismerje meg a cél a betöltés vizsgálathoz. Ez segít értelmezni, és elemzése a betöltés vizsgálati eredmények.

## <a name="create-and-run-the-web-performance-and-load-test-project"></a>Létrehozása és futtatása a project Web teljesítmény és a betöltés tesztelése

### <a name="create-a-web-performance-and-load-test-project"></a>Webes teljesítmény és a betöltés próba projekt létrehozása

1. Nyissa meg a Visual Studio 2015. Válassza a **fájl** > **Új** > **Project** a menüsor megjelenítése az **Új projekt** párbeszédpanel megnyitásához.

2. Bontsa ki a **képi C#** csomópontot, és válassza a **próba** > **webes teljesítmény és a projekt betöltése próba**. Nevezze el a projektet, és válassza az **OK** gombra.

    ![Az új projekt párbeszédpanel képe][1]

    Meg kell jelennie a megoldást Intézőben új webes teljesítmény és a betöltés próba projekt.

    ![Képernyőkép: az új projekt megjelenítő Solution Explorer][2]

### <a name="record-a-web-performance-test"></a>Rekord webhely teljesítményének teszteléséhez

1. Nyissa meg a .webtest projektet.

2. Válassza a Rögzítés indítása a böngészőben a **Felvétel hozzáadása** ikonra.

    ![Képernyőkép: a böngészőben a felvétel hozzáadása ikon][3]

    ![Képernyőkép: a böngészőben a Felvétel gomb][4]

3. Tallózással keresse meg a szolgáltatás háló alkalmazást. A felvétel panel meg kell jelennie a webes kérelmek.

    ![Képernyőkép: a webes kérelmek a felvétel a Vezérlőpulton][5]

4. Hajtsa végre a műveleteket, amelyeket a felhasználók végrehajtásához várt sorozatát. Ezeket a műveleteket a betöltés létrehozásához használt mintaként.

5. Amikor elkészült, válassza a felvétel leállításához **leállítása** gombra.

    ![A Stop gomb képe][6]

    A Visual Studio .webtest projekt kell rögzített kérések sorozata. Dinamikus paraméterek helyett automatikusan jelennek meg. Ezen a ponton, törölheti a minden olyan további és ismételt függőség kérést, amelyek nem szerepelnek a próba-levelezés.

6. A projekt mentése, és válassza a **Futtatás tesztelése** parancs a webes vizsgálat helyben futtatni, és győződjön meg arról, hogy minden helyesen működik.

    ![Képernyőkép: a próba Futtatás parancs][7]

### <a name="parameterize-the-web-performance-test"></a>A webes vizsgálat paraméterezni

A webes vizsgálat pedig kódolt webhely teljesítményének teszteléséhez, és szerkessze a kódot is paraméterezni. Alternatívájaként a webes vizsgálat kell kötni adatok listáját, hogy a vizsgálat kiszámítására az adatok között. Lásd: a [Létrehozás és a Futtatás kódolt webhely teljesítményének teszteléséhez](https://msdn.microsoft.com/library/ms182552.aspx) részletes tudnivalókat a webhely teljesítményének vizsgálat konvertálása kódolt teszten. Lásd: [hozzáadása egy webes teljesítmény adatforrás tesztelése](https://msdn.microsoft.com/library/ms243142.aspx) kötést létrehozni a adatok a webhely teljesítményének teszteléséhez olvashat.

Ebben a példában azt fogja konvertálja a webes vizsgálat kódolt teszten, a repülőgép azonosító cserélje ki a létrehozott globálisan egyedi azonosítója, és adja hozzá további kérések repülőjegyek küldése másik helyre.

### <a name="create-a-load-test-project"></a>Betöltési próba projekt létrehozása

Teszt projekt betöltése a webes teljesítmény vizsgálata és egység tesztje együtt további megadott terhelést beállításainak tesztelése leírt egy vagy több esetek tevődik össze. A következő lépések bemutatják, hogyan betöltés próba projekt létrehozása:

1. A helyi menüben a webhely a teljesítmény és a betöltés próba projekt, válassza az **Add** > **Betöltés próba**. A **Betöltés tesztelése** varázslóban válassza a **Tovább** gombra a próba-beállításainak konfigurálása.

2. A **Minta betöltése** csoportban válasszon, hogy egy állandó felhasználói betöltés vagy egy lépésben betöltése, egy kisebb felhasználócsoporttal kezdődő, és megnöveli a felhasználók adott idő alatt.

    Ha van egy jó becslése felhasználói betöltés mennyiségét, és szeretné, hogy hogyan a jelenlegi rendszer hajt végre, válassza a **Állandó betöltése**. Ha a cél megtudhatja, hogy a rendszer hajt végre egységes különböző terhelés alatt, válassza ki a **Betöltés lépés**.

3. **Mix vizsgálata** csoportban kattintson a **Hozzáadás** gombra, és válassza a a tesztet a betöltés vizsgálat szerepeltetni kívánt. Az **eloszlás** oszlop segítségével adja meg a teljes vizsgálatok futtatása az egyes próba százalékos.

4. A **Futtatás beállításai** csoportban adja meg a betöltés vizsgálat időtartama.

    >[AZURE.NOTE] A **Vizsgálni iterációk száma** beállítás csak a betöltés próba helyileg a Visual Studio segítségével futtatásakor érhető el.

5. Beállítások **Futtatás** **helye** szakaszban adja meg a helyet, ahol betöltés próba kérések jönnek létre. A varázsló kéri előfordulhat, hogy jelentkezzen be a Team Services-fiókkal. Jelentkezzen be, és válassza ki a földrajzi helyét. Amikor elkészült, válassza a **Befejezés** gombra.

6. A betöltés vizsgálat létrehozása után nyissa meg a .loadtest projektet, és válassza az aktuális futtatása beállítást, például a **Futtatás beállítások** > **futtatása Settings1 [aktív]**. A **Tulajdonságok** ablak megnyílik a Futtatás beállításait.

7. A **Futtatás beállítások** tulajdonságablak **eredmények** csoportjában az **Időzítés részletek tároló** beállítás van, hogy **nincs** , az alapértelmezett érték. Ez az érték további információk a betöltéskor teszteredményei **Minden egyes részleteinek** módosítása Visual Studio Team Services kapcsolódni, és egy betöltés teszt további információt a [Tesztelés betöltése](https://www.visualstudio.com/load-testing.aspx) talál.

### <a name="run-the-load-test-by-using-visual-studio-team-services"></a>A betöltés vizsgálat futtatása a Visual Studio Team Services használatával

Indítsa el a tesztet futtatni szeretné **Futtatni betöltés tesztelése** parancs végrehajtása

![Képernyőkép: a betöltés teszt Futtatás parancsot][8]

## <a name="view-and-analyze-the-load-test-results"></a>A betöltés próba eredményeinek megtekintése és elemzése

A betöltés, tesztelje a előrehaladtával, a teljesítményadatok grafikon. Meg kell jelennie a következő ábra hasonló.

![Képernyőkép: a teljesítmény graph betöltés vizsgálati eredmények][9]

1. Kattintson a lap tetején a **jelentés letöltése** hivatkozására. A jelentés letöltése után kattintson a **jelentés megtekintése** gombra.

    A **diagram** lapon megtekintheti a különböző teljesítmény számláló grafikonok. Az **Összegzés** lapon a teljes vizsgálati eredmények jelennek meg. A **táblák** lap átadott és sikertelen betöltés vizsgálatok száma látható.

2. Válassza a szám hivatkozásokat a **próba**- > **nem sikerült** , és a **hibák** > oszlopok**száma** a részletek megjelenítéséhez.

    A **Részletek** fülre információkat jelenít meg virtuális felhasználó és a vizsgálat forgatókönyv sikertelen kérelmekhez. Ezeket az adatokat akkor lehet hasznos, ha a betöltés vizsgálat több esetek tartalmazza.

[Elemzése betöltése vizsgálati eredmények](https://www.visualstudio.com/load-testing.aspx) megtekintéséhez a betöltése tesztelje Analyzer grafikonok nézetén betöltés teszteredményei további információkat.

## <a name="automate-your-load-test"></a>A betöltés próba automatizálása

Visual Studio csapat szolgáltatások betöltése Test API-khoz segít a betöltés vizsgálatok kezelheti és elemezheti a Team Services-fiókjában eredmények biztosít. [Felhő betöltés tesztelése Rest API -khoz](http://blogs.msdn.com/b/visualstudioalm/archive/2014/11/03/cloud-load-testing-rest-apis-are-here.aspx) is találhat további információt.

## <a name="next-steps"></a>Következő lépések
- [Figyelés, és egy helyi számítógép zónában fejlesztési beállítása a services diagnosztizálása](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[0]: ./media/service-fabric-vso-load-test/OverviewDiagram.png
[1]: ./media/service-fabric-vso-load-test/NewProjectDialog.png
[2]: ./media/service-fabric-vso-load-test/Project.png
[3]: ./media/service-fabric-vso-load-test/AddRecording.png
[4]: ./media/service-fabric-vso-load-test/AddRecording2.png
[5]: ./media/service-fabric-vso-load-test/ActionSequence.png
[6]: ./media/service-fabric-vso-load-test/StopRecording.png
[7]: ./media/service-fabric-vso-load-test/RunTest.png
[8]: ./media/service-fabric-vso-load-test/RunTest2.png
[9]: ./media/service-fabric-vso-load-test/Graph.png

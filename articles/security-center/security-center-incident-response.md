<properties
   pageTitle="Azure biztonság otthon használata az esemény választ |} Microsoft Azure"
   description="A dokumentum egy esemény válasz forgatókönyvet a Azure biztonság otthon használatát ismerteti."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="yurid"/>

# <a name="using-azure-security-center-for-an-incident-response"></a>Az esemény választ Azure biztonság otthon használata
Sok szervezetek megtudhatja, hogy miként csak azt követően támadások szenvedő védelmi események válaszolni. A költségek, illetve sérülések csökkentéséhez fontos, hogy egy helyen megtervezése támadások életbe lépése előtt az esemény választ. Azure biztonság otthon is használhatja az esemény választ különböző fázisaiban.

## <a name="incident-response-planning"></a>Válasz az esemény tervezése

Három alapvető funkciók függ, hogy hatékony csomagjával: nem védelme, észlel, és veszélyek válaszolni. Védelem események megakadályozásáról, észlelési szól veszélyek korai azonosítása, pedig válasz a támadó kizárása, és visszaállításáról rendszerek megszegése hátrányos következményeit csökkentésében.

Ez a cikk a biztonsági az esemény válasz szakaszokra át a cikk a [Microsoft Azure biztonsági válasz a felhőben](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678) , használja az alábbi ábrán látható módon:

![Az esemény válasz életciklus](./media/security-center-incident-response/security-center-incident-response-fig1.png)

Biztonság otthon hibakeresés felmérése és diagnosztika szakaszában is használhatja. Az alábbiakban példákat hogyan biztonság otthon akkor lehet hasznos három kezdeti az esemény válasz szakaszában:

- **Hibakeresés**: olvassa el az első feltüntetése esemény nyomozásban.
    - Példa: áttekintés a kezdeti ellenőrzése, hogy egy fontosként biztonsági riasztás a biztonság otthon irányítópulton emelte.
- **Felmérése**: hajtsa végre a gyanús tevékenység további információt szeretne kapni a kezdeti felmérés.
    - Példa: a biztonsági figyelmeztetés további információhoz juthat.
- **Diagnosztizálása**: egy technikai vizsgálat lebonyolítása és biztonsági, kezelési és megoldás stratégiák meghatározása.
    - Példa: kövesse a remediation leírtak szerint biztonság otthon, hogy az adott biztonsági értesítést.

Az alkalmazási példát, a következő bemutatja, hogyan használhatja a biztonság otthon védelmi esemény hibakeresés felmérése és diagnosztika/válasz szakaszában. A biztonság otthon egy [biztonsági esemény](security-center-incident.md) egy erőforrás az összes riasztásainak zárt beállítást a [leállítása egymásba fonódó](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) mintázattal összesítése. Események jelennek meg a [biztonsági figyelmeztetések](security-center-managing-and-responding-alerts.md) csempére és a lap. Az esemény során kiderül, kapcsolódó riasztások, a listáját, amely lehetővé teszi, hogy minden előfordulást bővebb tájékoztatást kaphat. Biztonság otthon is bemutatja önálló biztonsági figyelmeztetések is lefelé a gyanús tevékenység nyomon követésére használható.

## <a name="scenario"></a>Eset

A Contoso nemrégiben át a helyszíni forrásokhoz az Azure, beleértve az egyes a vállalati verzió munkaterhelésekből virtuális gép-alapú és az SQL-adatbázisait. Contoso Core számítógépen biztonsági esemény válasz csoport (CSIRT) által jelzett, a probléma miatt biztonsági intelligenciával kapcsolatos funkciók az aktuális esemény válasz eszközzel nem integrált vizsgálat alatt a biztonsági problémákról. Integráció hiánya probléma vezet be, a Hibakeresés szakaszában (túl sok téves), valamint felmérése és diagnosztika szakaszában. Az áttelepítés részeként azokat úgy döntött, csatlakozás a biztonság otthon őket a probléma megoldása érdekében.

Az első szakasz, ez az áttelepítés befejeződött, ezek után onboarded összes erőforrás és a címzett összes a biztonsági javaslatok biztonsági központból. A Contoso CSIRT található a tájékoztatási számítógépen biztonsági események foglalkozik. A csapat bármely biztonsági esemény kezelésének feladatokkal a felhasználók egy csoportja áll. A csapattagok egyértelműen meghatározta feladatok annak érdekében, hogy nincs terület válasz se maradjon fedetlen.

Ebben az esetben céljából megyünk a fókusz a szerepkörök, a következő personas Contoso CSIRT részét képező:

![Az esemény válasz életciklus](./media/security-center-incident-response/security-center-incident-response-fig2.png)

Kis biztonsági műveletek szerepel. Saját felelősségi köre terjed ki:

- Figyelés és biztonsági veszélyek éjjel válaszolni.
- Escalating felhő terhelést tulajdonosának vagy biztonsági elemző szükség szerint.

SAM egy biztonsági elemző, és a feladatai tartalmazza:

- Vizsgálat alatt támadások.
- Felmérésével értesítések.
- Használata a terhelést a tulajdonosok határozza meg, és alkalmazza a megoldásokkal kapcsolatban.

Amint látható, a kis- és Sam van más felelősségi köre, és azok együtt kell dolgozniuk biztonság otthon információk megosztásához.

## <a name="recommended-solution"></a>Javasolt megoldás

Mivel a kis- és Sam különféle szerepköröket, azok lesz használatban biztonság otthon különböző területei vonatkozó adatokat a mindennapos tevékenységek juthat. Kis fogja használni a **biztonsági figyelmeztetések** a napi ellenőrzés részeként.

![Biztonsági figyelmeztetések](./media/security-center-incident-response/security-center-incident-response-fig3.png)

Kis fogja használni a biztonsági figyelmeztetések Hibakeresés és felmérése szakaszában. Kis kezdeti értékeléséhez befejezése után kikkel előfordulhat, hogy a probléma fiókkezelőhöz hívássá további vizsgálat szükségessége esetén. Ezen a ponton Sam fogja használni a kapott adatok biztonsági központ, néha a diagnosztizálása szakasz áthelyezése más adatforrásokból együtt.


## <a name="how-to-implement-this-solution"></a>Ez a megoldás megvalósításáról.

Szeretné látni, hogyan szeretné használni Azure biztonság otthon egy esemény válasz helyzetben, azt fogja kis 's lépésekkel Hibakeresés és felmérése fázisaiban, dátumtípus és Sam tartalmaz a probléma diagnosztizálása.

### <a name="detect-and-assess-incident-response-stages"></a>Észleli és mérje fel, hogy az esemény válasz lépései

Kis bejelentkezve az Azure-portálra, és a biztonság otthon konzolban működik. A tevékenység ellenőrzése napi részeként kikkel lépések fontosként biztonsági figyelmeztetések áttekintése a következő lépések elvégzésével:

1. Kattintson a **biztonsági figyelmeztetések** csempére, és a **biztonsági figyelmeztetések** lap eléréséhez.
    ![Biztonsági a figyelmeztető lap](./media/security-center-incident-response/security-center-incident-response-fig4.png)

    > [AZURE.NOTE] Ebben az esetben céljából kis fog végre szeretne hajtani annak kiértékelése rosszindulatú SQL tevékenység riasztást, a fenti ábrán látható módon.
2. Kattintson a **kártékony SQL tevékenység** riasztás, és tekintse át a **kártékony SQL tevékenység** lap megtámadott erőforrások:  ![esemény részletei](./media/security-center-incident-response/security-center-incident-response-fig5.png)

    Ez a lap a kis jegyzeteket készíthet az megtámadott erőforrások vonatkozó hogyan számú alkalommal a támadásokat történt, és mikor történt.
3. Kattintson az **erőforrás elleni** a támadásokat további információt szeretne kapni.

A leírás elolvasása, után kis meggyőződése, hogy ez nem egy vakriasztás és, hogy kikkel kell hívássá ebben az esetben fiókkezelőhöz.

### <a name="diagnose-incident-response-stage"></a>Az esemény válasz szakasz diagnosztizálása

SAM megkapja az esetet a kis, és elindítja a javasolt biztonság otthon remediation lépések áttekintése.

![Az esemény válasz életciklus](./media/security-center-incident-response/security-center-incident-response-fig6.png)

### <a name="additional-resources"></a>További források

A válasz az esemény csapatának is is kihasználhatja a különböző típusú jelentések megtekintéséhez [Biztonsági központ Power BI](security-center-powerbi.md) lehetőséget. Ezeket a jelentéseket is segíthetnek a további vizsgálat áttekintéséhez, elemezheti és szűrheti a javaslatok és a biztonsági figyelmeztetések során. Biztonsági információk és a esemény vállalatirányítási (SIEM) megoldás a vizsgálat során használó cégek azok is [integráció a biztonság otthon, a megoldás](security-center-integrating-alerts-with-log-integration.md). Azure naplókat és a virtuális gép (virtuális) biztonsági események integrálhatja a [Azure napló integrációs eszköz](https://blogs.msdn.microsoft.com/azuresecurity/2016/07/21/microsoft-azure-log-integration-preview/)használatával. Támadások vizsgálatához ezeket az adatokat is használhatja az adatokat, amely Biztonság otthon biztosít.


## <a name="conclusion"></a>Elfogadásáról

A Csapat összeállítása a esemény bekövetkezése nagyon fontos, hogy szervezete és pozitív hatással van események kezelésének módját előtt. A pontos lépéseket az ismételt védelmi esemény csapatának is megkönnyítheti azzal a megfelelő eszközök Lync-erőforrások. Biztonság otthon [észlelés](security-center-detection-capabilities.md) segítheti informatikai gyorsan védelmi események megválaszolása és ismételt a biztonsági problémákról.

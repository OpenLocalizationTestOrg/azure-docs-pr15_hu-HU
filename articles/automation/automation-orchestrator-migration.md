<properties
   pageTitle="Áttérés a Orchestrator Azure az automatizálás |} Microsoft Azure"
   description="System Center Orchestrator runbooks és integráció csomagok áttelepítése Azure automatizálást ismerteti."
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="bwren" />


# <a name="migrating-from-orchestrator-to-azure-automation-beta"></a>Áttérés a Orchestrator Azure automatizálás (Beta)

A [System Center Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) Runbooks tevékenységek való integráció csomagok írt kifejezetten Orchestrator közben az Azure automatizálás runbooks alapuló Windows PowerShell alapulnak.  [Grafikus runbooks](automation-runbook-types.md#graphical-runbooks) az Azure automatizálás PowerShell-parancsmagok, a gyermek runbooks és a eszközök tevékenységeik egy hasonló megjelenésének Orchestrator runbooks rendelkezik.

A [System Center Orchestrator áttelepítési eszközkészlet](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) , amely segít a Orchestrator runbooks konvertálása Azure automatizálási eszközök tartalmazza.  Mellett a saját maguk runbooks átalakítás, a Windows PowerShell-parancsmagok integrációs modulok a integrációs csomagokat, amelyek a runbooks tevékenységekkel kell konvertálnia.  

Az alábbiakban az Egyszerű folyamatábra Orchestrator runbooks konvertálásának Azure automatizálási.  A lépések leírása az alábbi szakaszok részletesen.

1.  Töltse le a [System Center Orchestrator áttelepítési eszközkészlet](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) , az eszközök és a jelen cikkben ismertetett modulok tartalmazó.
2.  [Szokásos tevékenységek modul](#standard-activities-module) importálása az Azure automatizálási.  Ide tartoznak a szokásos Orchestrator tevékenységeket konvertált runbooks által esetleg használt konvertált változatának.
3.  [System Center Orchestrator integrációs modulok](#system-center-orchestrator-integration-modules) importálása Azure automatizálási ezeket a System Center hozzáférő runbooks által használt integrációs csomagok.
4.  Egyéni és a harmadik féltől származó integrációs csomagok használatával a [Integrációs csomag konverter](#integration-pack-converter) konvertálhatja, és importálása az Azure automatizálási.
5.  A [Konverter Runbook](#runbook-converter) használatával Orchestrator runbooks konvertálhatja, és telepítse az Azure automatizálás.
6.  Manuális létrehozása szükséges Orchestrator eszközök Azure automatizálást óta a Runbook konverter, ezek az erőforrások nem alakítja át.
7.  Állítsa be a [Hibrid Runbook dolgozó](#hybrid-runbook-worker) konvertált runbooks helyi erőforrások hozzáférő futtatásához a helyi adatok központban.

## <a name="service-management-automation"></a>Szolgáltatás felügyeleti automatizálást

[Szolgáltatások kezelése automatizálása](http://technet.microsoft.com/library/dn469260.aspx) (SMA) tárolja, és a helyi adatközpont, például Orchestrator runbooks fut, és azt használja a azonos integrációs modulok, mint a Azure automatizálást. A [Runbook konverter](#runbook-converter) alakít Orchestrator runbooks grafikus runbooks bár SMA a nem támogatott.  Továbbra is telepítheti a [Szokásos tevékenységek modul](#standard-activities-module) és a [System Center Orchestrator integrációs modulok](#system-center-orchestrator-integration-modules) SMA be, de manuálisan kell [a runbooks átírásának](http://technet.microsoft.com/library/dn469262.aspx).

## <a name="hybrid-runbook-worker"></a>Hibrid Runbook dolgozói

Runbooks Orchestrator az adatbázis-kiszolgálón tárolt és -kiszolgálókon runbook, mind a helyi adatok központban futtatása.  Az Azure automatizálás Runbooks Azure a felhőben vannak tárolva, és a helyi adatok központban [Hibrid Runbook dolgozó](automation-hybrid-runbook-worker.md)futtatását is lehetővé teszi.  Ez a hogyan általában futtatható runbooks átalakította a Orchestrator, mivel a célja, hogy futtassa a helyi kiszolgálókon.

## <a name="integration-pack-converter"></a>Integráció csomag konverter

A integráció csomag konverter integráció csomagok a [Orchestrator integráció eszközkészlet (OIT)](http://technet.microsoft.com/library/hh855853.aspx) való integrációs modulok alapján Azure automatizálást vagy szolgáltatás felügyeleti automatizálást importálható a Windows PowerShell használatával létrehozott alakítja át.  

A integrációs csomag konverter futtatásakor egy varázslót, amely lehetővé teszi, hogy válasszon ki egy integrációs csomag (.oip) fájlt jelenik meg.  A varázsló megjeleníti a integrációs csomag szereplő tevékenységeket, majd kiválasztására, amelyek áttelepítendőként.  Amikor a varázsló-integráció a modul, amely tartalmazza a megfelelő parancsmag az egyes a tevékenységek eredeti integrációs Pack hoz létre.


### <a name="parameters"></a>Paraméterek

A integrációs csomag tevékenység bármely tulajdonságok határozza meg a megfelelő parancsmagot a integrációs modulban alakulnak.  A Windows PowerShell-parancsmagok beállított összes parancsmagokat használható [általános paramétereket](http://technet.microsoft.com/library/hh847884.aspx) .  Ha például a - részletes paramétert a működésével kapcsolatos részletes információkat kimeneti parancsmag okoz.  Előfordulhat, hogy nincs parancsmag közös paraméterként azonos nevű paraméter.  Ha egy tevékenység közös paraméterként azonos nevű tulajdonságot, a varázsló kéri meg kell adnia a paraméter egy másik nevet.

### <a name="monitor-activities"></a>Monitor tevékenységek

Monitor runbooks Orchestrator a [tevékenység figyelése](http://technet.microsoft.com/library/hh403827.aspx) indítása, és futtassa a folyamatosan váró adott esemény érvényesíthetők.  Azure automatizálási nem támogatja a monitor runbooks, így integrációs Pack monitor tevékenységek nem alakulnak.  A helyőrző parancsmag ehelyett a monitor tevékenység integráció modulban jön létre.  Ezzel a parancsmaggal nincs funkciókkal rendelkezik, de lehetővé teszi, hogy minden olyan konvertált runbook telepítésére használó.  A runbook nem tud az Azure automatizálás futtatása, de, hogy módosítani tudja telepíthető.

### <a name="integration-packs-that-cannot-be-converted"></a>Amely nem alakítható integrációs csomagok

A integrációs csomag konverterrel integrációs csomagok nem OIT alkalmazással létrehozott nem alakítható ki. Vannak bizonyos ezzel az eszközzel, amely jelenleg nem alakítható a Microsoft által nyújtott integráció csomagok.  Ezek integrációs csomagok konvertált változatának lett [letöltési megadva](#system-center-orchestrator-integration-modules) , hogy az Azure automatizálási vagy szolgáltatás felügyeleti automatizálási telepíthető.


## <a name="standard-activities-module"></a>Szokásos tevékenységek modul

Orchestrator [Szokásos tevékenységek](http://technet.microsoft.com/library/hh403832.aspx) nem szerepelnek az integration csomagját, de sok runbooks által használt tartalmazza.  A szokásos tevékenységek modul-integráció a modul, amely tartalmazza a egyenértékű parancsmag az minden egyes tevékenységéről.  Telepíteni kell az e integrációs modul az Azure automatizálás bármely konvertált runbooks egy szabványos tevékenység használó importálása előtt.

Nemcsak a konvertált runbooks támogatására, a parancsmagok a szokásos tevékenységek modulban is használhatják valakit, aki ismeri Orchestrator az Azure automatizálás új runbooks létrehozásához.  Miközben a funkcióval az összes, a szokásos tevékenységek parancsmagokat hajtható végre, előfordulhat, hogy működnek másképp.  A parancsmagok az átalakított szokásos tevékenységek modul azonosak a hozzájuk tartozó tevékenységek működik, és a használja az azonos paramétereket.  Ez a meglévő Orchestrator runbook Szerző az Azure automatizálást runbooks az áttűnés segíthet.

## <a name="system-center-orchestrator-integration-modules"></a>System Center Orchestrator integrációs modulok

A Microsoft [integráció csomagok](http://technet.microsoft.com/library/hh295851.aspx) biztosít runbooks System Center összetevők és más termékek automatizálásához fejlesztésére.  Ezek integrációs csomagok részét jelenleg alapuló OIT, de nem jelenleg alakítható integrációs modulok ismert hibák miatt.  [System Center Orchestrator integrációs modulok](https://www.microsoft.com/download/details.aspx?id=49555) ezek nem importálhatók Azure automatizálási és szolgáltatás felügyeleti automatizálási integráció csomagok konvertált változatának tartalmazza.  

Az Ez az eszköz RTM verziója a integrációs csomagok a integrációs csomag konverterrel átalakítható OIT alapján frissített verziója fog jelenni.  Útmutatást segítséget nyújtanak a integrációs csomagok nem alapuló OIT a tevékenységek segítségével runbooks konvertálása is fogja adni.

## <a name="runbook-converter"></a>Runbook konverter

A Runbook konverter Orchestrator runbooks Azure automatizálási importálható [grafikus runbooks](automation-runbook-types.md#graph-runbooks) alakítja át.  

Runbook konverter hajtanak végre, egy PowerShell-modult parancsmag nevű **ConvertFrom-SCORunbook** a konverziót hajt végre.  Az eszköz telepítésekor mutató parancsikont, egy PowerShell-munkamenetet a parancsmag betöltő hoz létre.   

Az alábbiakban az Egyszerű folyamatábra-Orchestrator runbook alakíthatja, és importálja az Azure automatizálási.  A következő szakaszokban további tájékoztatást a eszközzel, és a konvertált runbooks használata.

1. Egy vagy több runbooks exportálása Orchestrator.
2. Szerezze be az összes tevékenységet a runbook integrációs modulok.
3. Az exportált fájl a Orchestrator runbooks konvertálja.
4. Tekintse át az információkat naplókban érvényesítésére az átalakítás és minden szükséges manuális feladatok határozza meg.
4. Azure automatizálási konvertált runbooks importálhat.
5. Minden szükséges eszközök létrehozása az Azure automatizálás.
6. Szerkessze a runbook az Azure automatizálás módosításához szükséges tevékenységek.

### <a name="using-runbook-converter"></a>A konverter Runbook használatával

**ConvertFrom-SCORunbook** szintaxisa a következő:

    ConvertFrom-SCORunbook -RunbookPath <string> -Module <string[]> -OutputFolder <string> 

- RunbookPath - elérési útját az exportált fájlt tartalmazó a runbooks szeretne alakítani.
- Modul - vesszővel tagolt listáját tartalmazó tevékenységek a runbooks integrációs modulok.
- OutputFolder - mappa elérési útját hozhat létre az grafikus runbooks alakulnak. 


A következő példa parancsot a runbooks **MyRunbooks.ois_export**nevű Exportálás fájlba alakítja át.  Ezeket a runbooks használja az Active Directory és az adatok védelme Manager integrációs csomagok.

    ConvertFrom-SCORunbook -RunbookPath "c:\runbooks\MyRunbooks.ois_export" -Module c:\ip\SystemCenter_IntegrationModule_ActiveDirectory.zip,c:\ip\SystemCenter_IntegrationModule_DPM.zip -OutputFolder "c:\runbooks" 


### <a name="log-files"></a>Naplófájlok

A Runbook konverter hoz létre a következő naplófájlokat az átalakított runbook ugyanazon a helyen.  Ha a fájlokat már létezik, majd azok felülíródnak a legutóbbi átalakítás adataival.

| Fájl | Tartalom |
|:---|:---|
| Runbook konverter - Progress.log | Az átalakítás, beleértve az egyes tevékenységek sikeresen konvertálta információkat és a figyelmeztetés az egyes tevékenységek nem alakítja a részletes lépéseket. |
| Runbook konverter - Summary.log  | Az utolsó átalakítás, beleértve az esetleges figyelmeztetéseket összefoglaló és követheti nyomon a feladatokat kell elvégeznie, például az átalakított runbook szükséges változó létrehozása.  |

### <a name="exporting-runbooks-from-orchestrator"></a>Orchestrator runbooks exportálása

A Runbook konverter működik-e az exportált fájlt, amely tartalmazza az egy vagy több runbooks Orchestrator a.  Kattintson az exportált fájlt a megfelelő Azure automatizálási runbook az egyes Orchestrator runbook hoz létre.  

Egy runbook exportálása Orchestrator, kattintson a jobb gombbal a runbook Runbook tervezőben nevét, és válassza ki az **exportálni**.  Exportálja a mappában található összes runbooks, kattintson a jobb gombbal a mappa nevét, és válassza a **exportálja**.


### <a name="runbook-activities"></a>Runbook tevékenységek

A Runbook konverter minden tevékenységet a Orchestrator runbook az Azure automatizálás megfelelő tevékenység értékké alakítja át.  Azoknál a tevékenységeknél, amely nem alakítható létrejön egy helyőrző tevékenység a runbook figyelmeztetés szöveggel.  Után a konvertált runbook importálása Azure automatizálást, ezt kell lecserélnie ezek a tevékenységek valamelyikét érvényes tevékenységek, hajtsa végre a szükséges funkciót.

A [Szokásos tevékenységek modul](#standard-activities-module) Orchestrator tevékenységek alakulnak.  Vannak bizonyos szokásos Orchestrator tevékenységeket, amelyek nem a modul bár és nem lesznek konvertálva.  Például az **Küldése Platform esemény** nincs megfelelője Azure automatizálási rendelkezik, mivel az eseményre az Orchestrator.

[Monitor tevékenységek](https://technet.microsoft.com/library/hh403827.aspx) nem alakulnak, mivel nincs megfelelője, ezeket az Azure automatizálás.  A kivétel monitor tevékenységek a [integrációs csomagok alakítja](#integration-pack-converter) , amely a helyőrző tevékenység alakulnak.

Minden tevékenység [integrációs csomag alakulnak](#integration-pack-converter) át, ha a **modulokat** paraméter biztosítson elérési útját a integrációs modul alakulnak.  System Center integrációs csomagok a [System Center Orchestrator integrációs modulok](#system-center-orchestrator-integration-modules)is használhatja.


### <a name="orchestrator-resources"></a>Orchestrator erőforrások

A Runbook konverter csak runbooks, nem Orchestrator anyagainak például számláló, változók vagy kapcsolatok alakítja át.  Az Azure automatizálási a számláló által nem támogatott.  Változók és a kapcsolatokkal támogatottak, de manuálisan kell létrehoznia őket.  A naplófájlok a program tájékoztatja, ha a runbook van szüksége, például az erőforrások, és adja meg a megfelelő erőforrások létrehozása az Azure automatizálását az átalakított runbook működéséhez szükséges.

Például egy runbook előfordulhat, hogy a változó használható egy adott értéket egy tevékenységet kitöltéséhez.  A konvertált runbook konvertálja a tevékenységet, majd adja meg a változó eszköz az Azure automatizálás neve megegyezik a Orchestrator változó.  Ez az átalakítás után létrehozott **Runbook konverter - Summary.log** fájlban kísérheti.  Meg kell manuálisan a változó eszköz létrehozása az Azure automatizálást a runbook használata előtt. 

 
### <a name="input-parameters"></a>Bemeneti paramétereket

A Orchestrator Runbooks fogadja el a bemeneti paramétereket **Inicializálni adatok** tevékenységgel.  Ha a konvertált runbook tartalmazza a tevékenység, akkor a [bemeneti paraméterre](automation-graphical-authoring-intro.md#runbook-input-and-output) az Azure automatizálási runbook az egyes paraméterek a tevékenység jön létre.  A [munkafolyamat parancsfájl ellenőrző](automation-graphical-authoring-intro.md#activities) tevékenység az átalakított runbook, és az egyes paramétere jön létre.  A kimenet a tevékenységnek a runbook bármely tevékenység, a bemeneti paraméterre használó vonatkoznak.

Ez a beállítás szolgál, hogy oka legjobb tükrözéséhez a Orchestrator runbook funkciói.  Az új grafikus runbooks tevékenységek kell közvetlenül hivatkoznak bemeneti paramétereket Runbook beviteli adatforrás.


### <a name="invoke-runbook-activity"></a>Runbook tevékenység meghívása

A Orchestrator Runbooks más runbooks kezdje a **Meghívása Runbook** tevékenység. Ha a konvertált runbook tartalmazza a tevékenység és a **Várakozás a végrehajtására** beállítás van megadva, majd a runbook tevékenységének jön létre, a konvertált runbook.  Nincs beállítva a **Várakozás a végrehajtására** lehetőséget, ha egy munkafolyamat parancsfájl tevékenység jön létre a runbook indítása **Start-AzureAutomationRunbook** használó.  Után a konvertált runbook importálása Azure automatizálást, módosítania kell a tevékenységnek a tevékenység megadott adatokkal.




## <a name="related-articles"></a>Kapcsolódó cikkek

- [System Center 2012 - Orchestrator](http://technet.microsoft.com/library/hh237242.aspx)
- [Szolgáltatás felügyeleti automatizálást](https://technet.microsoft.com/library/dn469260.aspx)
- [Hibrid Runbook dolgozói](automation-hybrid-runbook-worker.md)
- [Szokásos tevékenységek orchestrator](http://technet.microsoft.com/library/hh403832.aspx)
- [Letöltés System Center Orchestrator áttelepítési eszközkészlet](https://www.microsoft.com/en-us/download/details.aspx?id=47323)
 

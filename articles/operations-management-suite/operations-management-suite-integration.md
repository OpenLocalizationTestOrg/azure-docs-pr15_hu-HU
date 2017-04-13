<properties
   pageTitle="Tevékenységek kezelése csomagja (MOBILE) integrálása |} Microsoft Azure"
   description="Mellett a MOBILE szokásos funkcióival integrálhatja más management alkalmazásokkal, és egy hibrid management környezet kialakítása, adja meg az egyéni kezelési forgatókönyvek egyedi az környezetbe, illetve adja meg egy egyéni management szolgáltatások funkcióját az ügyfelek számára.  Ez a cikk áttekintést nyújt a különféle lehetőségekkel integrálása a MOBILE és a részletes, technikai adatainak a megadásával cikkekre mutató hivatkozásokat."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="bwren" />

# <a name="integrating-with-operations-management-suite-oms"></a>Tevékenységek kezelése csomagja (MOBILE) integrációja

Műveletek Management csomagot a Microsoft felhőalapú management levelezőprogramból, amelyek segítségével kezelése és védelme a helyszíni és felhőbeli infrastruktúra.  Mellett a MOBILE szokásos funkcióival integrálhatja más management alkalmazásokkal, és egy hibrid management környezet kialakítása, adja meg az egyéni kezelési forgatókönyvek egyedi az környezetbe, illetve adja meg egy egyéni management szolgáltatások funkcióját az ügyfelek számára.  Ez a cikk áttekintést nyújt a különféle lehetőségekkel integrálása a MOBILE szolgáltatások és a részletes, technikai adatainak a megadásával cikkekre mutató hivatkozások. 



## <a name="log-analytics"></a>Log Analytics
Log Analytics által gyűjtött adatkezelés olyan adattárban, amely üzemelteti Azure-ban vannak tárolva.  A tárban tárolnak minden adat, napló keresi, amelyek gyorselemzés révén nagyon nagy mennyiségű adatot érhető el.  Lehet, hogy az új adatok elemzése elérhetővé tétele a tárházba feltöltéséhez, vagy a tárat, adja meg egy új megjelenítés létrehozásához, vagy egy másik kezelése eszköz integrálása az adatok kinyerése az integration igényeknek megfelelően alakíthatja.

A tárat az egyes adatcsoportok rekordként vannak tárolva.  Ha elhelyezte a tárat, a felhasználók a megoldás használó rekordtípus és annak tulajdonságait leírását kell megadnia.  Ha az adatok beolvasásához, információra van szüksége a az adatokkal dolgozik.

![A MOBILE tárházba feltöltése](media/operations-management-suite-integration/repository.png)


### <a name="populate-the-log-analytics-repository"></a>A napló Analytics tárházba feltöltése
Több módon az feltöltése a MOBILE tárat.  Az Ön által használt módszer függ, például hol található a forrásadatokat, az adatokat, és amely formátumának tényezőket kell támogatja az ügyfelek számára.  Adatok vannak tárolva a tár, miután azt mindegy, milyen volt a gyűjtött.

Az alábbi szakaszok ismertetik a különböző lehetőségek feltöltése a MOBILE tárat.

#### <a name="connected-sources-and-data-sources"></a>Csatlakoztatott adatok és a források adatforrások 
Csatlakoztatott forrásai a helyek, ahol adatokat tudja visszaszerezni a MOBILE tárhoz.  Adatforrások és megoldások csatlakoztatott adatforrásból futtatni, és a meghatározott adatokat összegyűjtött megadása.  Ha az alkalmazás valamely ezek az adatforrások adatot ír, majd összegyűjtheti azt az adatforrás konfigurálásával.  Például ha az alkalmazás Syslog események hoz létre, majd azok gyűjthetők össze az Syslog adatforrás által egy Linux Agent.

- [A napló Analytics adatforrásokhoz](../log-analytics/log-analytics-data-sources.md)

#### <a name="solutions"></a>Megoldások

Megoldások bővítése MOBILE lehetőségeit.  Megoldást is adatgyűjtés a csatlakoztatott forrásból származó, vagy azt is analízist a már a tárházba összegyűjtött rekordok.  Minden megoldást, amelyet a Microsoft tartalmaz egy egyéni cikk, amely a részletesen gyűjt adatokat.

- [A napló Analytics megoldások](../log-analytics/log-analytics-add-solutions.md)



#### <a name="http-data-collector-api"></a>HTTP adatgyűjtő API

A napló Analytics HTTP adatok adatgyűjtő API a REST API-t, amely lehetővé teszi, hogy JSON adatok hozzáadása a napló Analytics-tárat.  Ez az API is kihasználhatja, ha telepítve van az alkalmazás, amely nem nyújt az adatoknak az egyik más adatforrásokból vagy megoldások.  A tár bármely ügyfélprogramból felhívhatja az API-t, és nem a webhelycsoport ütemterv tetszőleges adatforrás vagy megoldás támaszkodik kitöltéséhez használható.

- [Log Analytics HTTP adatgyűjtő API](../log-analytics/log-analytics-data-collector-api.md)


### <a name="retrieve-data-from-the-log-analytics-repository"></a>Adatok beolvasása a napló Analytics tárházba

Több módon az adatok lekérése a MOBILE tárat.  Előfordulhat, hogy a felhasználók a MOBILE konzolon adatok beolvasásához, és küldje el nekik különböző képi megjelenítések és elemzése.  Adatokat is beolvashatja egy külső folyamat, például egy másik projektmenedzsment megoldás.

#### <a name="log-searches"></a>Log keresések

A MOBILE tárházba tárolható összes adatra napló keresések keresztül érhető el.  Előfordulhat, hogy a felhasználók saját alkalmi elemzést végezhet a MOBILE konzolban, vagy irányítópult létrehozása az egy adott napló keresés megjelenítés.  Megoldások is tartalmazhatnak egyéni nézeteket a megjelenítések előre definiált keresések alapján.  A napló keresési API segítségével a MOBILE adattárban access-adatok a külső alkalmazás vagy kezelése eszközben.  

- [A napló Analytics napló keresések](../log-analytics/log-analytics-log-searches.md)
- [Log Analytics napló keresési REST API-val](../log-analytics/log-analytics-log-search-api.md)
- [Jelentkezzen be a Analytics-parancsmagok](https://msdn.microsoft.com/library/mt188224.aspx)



#### <a name="custom-views"></a>Az egyéni nézetek 
A nézet Tervező lehetővé teszi a MOBILE konzolban, amelyekkel a felhasználók a megjelenítés és a megoldás található adatok elemzése egyéni nézeteket hozhat létre.  Minden egyes nézetről egy csempére értéke, amely csak napló keresések alapuló képi megjelenítés részeinek és a konzol a fő lapon megjelenő tartalmazza.
  
- [Log Analytics Nézettervezőt](../log-analytics/log-analytics-view-designer.md)


#### <a name="power-bi"></a>A Power BI

Log Analytics automatikusan adatok exportálhatók a MOBILE tárat a Power BI úgy is kihasználhatja a képi megjelenítések és elemzőeszközök együtt.  Ez az Exportálás végrehajtott időközönként, naprakészen tartani az adatokat. 

- [Log Analytics adatainak exportálása a Power BI](../log-analytics/log-analytics-powerbi.md)




## <a name="automation"></a>Automatizálási

MOBILE automatizálhatja a folyamatok gyűjtött adatok reagálni vagy más kezelési funkciók.  Előfordulhat, hogy adatokat gyűjtsön az alkalmazás és szúrja be a MOBILE tárat, vagy automatizálni előfordulhat, hogy a módosításokat, tehát a tárházba adatai válaszul ismert probléma. 

![Automatizálási](media/operations-management-suite-integration/automate.png)

### <a name="runbooks"></a>Runbooks

Az Azure automatizálás PowerShell-parancsfájlokat és a munkafolyamatok futtatása a Azure felhőben Runbooks.  Erőforrások Azure-ban vagy a felhőből is elérhető más erőforrások kezelésére használhatja őket.  Runbooks is futtathatja a helyi adatközponthoz hibrid Runbook dolgozó használatával.  Egy runbook elindíthatja az Azure portálról, vagy külső folyamatok több módszerrel, például az automatizálás API vagy a PowerShell használatával is.

- [Egy runbook kezdése az Azure automatizálási](../automation/automation-starting-a-runbook.md)
- [Azure automatizálási parancsmagok](https://msdn.microsoft.com/library/dn690262.aspx)
- [Automatizálási REST API-val](https://msdn.microsoft.com/library/mt662285.aspx)
- [Automatizálási .NET](https://msdn.microsoft.com//library/mt465763.aspx)

### <a name="alerts"></a>Értesítések

A figyelmeztetési szabályok automatikusan futni napló keresések ütemezés szerint.  Ha az eredményeket az adott feltételeknek megfelelő az eredményül kapott értesítést is egy runbook az Azure automatizálás vagy hívás kezdeményezése egy webhook, amelyek egy külső folyamat is.  Ezek a válaszokat a is elhelyezhet a figyelmeztetés, beleértve a napló keresés visszaadott adatok részleteit.

- [Log Analytics riasztásai](../log-analytics/log-analytics-alerts.md)
- [Log Analytics riasztási API](../log-analytics/log-analytics-api-alerts.md)


## <a name="backup-and-site-recovery"></a>Biztonsági mentési és helyreállítási webhely

Azure biztonsági mentése és webhely-helyreállítás a szükséges a vállalati adatok védelme és biztosítása kiszolgálók és az alkalmazások az elérhető szolgáltatásokat.  Az alábbi szolgáltatások végrehajtásához az alkalmazás biztonsági szolgáltatásokat nyújtó vagy egy virtuális gép feladatátvevő kezdeményezése olyan esetek is élvezheti.

- [Azure biztonságimásolat-parancsmagok](https://msdn.microsoft.com/library/mt619253.aspx)
- [Azure webhely helyreállítási REST API-val](https://msdn.microsoft.com/library/azure/mt750497.aspx)
- [Azure webhely helyreállítási parancsmagok](https://msdn.microsoft.com/library/mt637930.aspx)

## <a name="custom-solutions"></a>Egyéni megoldások

Integráció logika a munkaterületen található, vagy egy ügyfél munkaterületen futtatásához egy egyéni megoldásba is beágyazására.  A megoldás is elhelyezhet a integrációs módszerek bármelyikével ebben a cikkben kívül más erőforrások: a teljes eset megadását.  Az erőforrások a megoldás csomagolása, hogy a megoldás távolítja el, ha az összes létrehozott erőforrás törlődjenek a MOBILE munkaterület és Azure előfizetés.

Ha például a a megoldás összegyűjtése és adatfeldolgozás, majd feltölti a napló Analytics-tárat a HTTP-adatok adatgyűjtő API-automatizálási runbook lehetnek.  Egyéni nézet, amely megjeleníti és elemzi a gyűjtött adatok is lehetnek.  

- Egyéni megoldások (hamarosan)    

## <a name="next-steps"></a>Következő lépések
- Hivatkozás a [MOBILE SDK](operations-management-suite-sdk.md) MOBILE szolgáltatások automatizálása műszaki információkat.  

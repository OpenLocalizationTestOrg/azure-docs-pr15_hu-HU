<properties 
    pageTitle="Értékáram-elemzés kibocsátási megjegyzések |} Microsoft Azure" 
    description="Értékáram-elemzés kibocsátási megjegyzések" 
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

#<a name="stream-analytics-release-notes"></a>Értékáram-elemzés kibocsátási megjegyzések

## <a name="notes-for-04152016-release-of-stream-analytics"></a>Jegyzetek 04/15/2016 kiadásának Értékáram-elemzés ##

Ebben a kiadásban tartalmazza a következő frissítést.

Cím | Leírás
---|---
A Power BI kimeneti értékeket általános elérhetősége  | [A Power BI kimenet](stream-analytics-power-bi-dashboard.md) elérhetők általában. A Power BI engedélyezési 90 napos lejártát megszűnt. További információt a esetek, amikor engedélyezési újítható meg kell című [megújítása engedélyezés](stream-analytics-power-bi-dashboard.md#Renew-authorization) a Power BI-irányítópult létrehozása.

## <a name="notes-for-03032016-release-of-stream-analytics"></a>Jegyzetek 03/03/2016 kiadásának Értékáram-elemzés ##

Ebben a kiadásban tartalmazza a következő frissítést.

Cím | Leírás
---|---
Új adatfolyam Analytics-lekérdezési nyelv elemek  | SAQL most már [GetType](https://msdn.microsoft.com/library/azure/mt643890.aspx "GetType MSDN lapon"), a [TRY_CAST](https://msdn.microsoft.com/library/azure/mt643735.aspx "TRY_CAST MSDN lap") és a [REGEXMATCH](https://msdn.microsoft.com/library/azure/mt643891.aspx "REGEXMATCH MSDN lapot").

## <a name="notes-for-12102015-release-of-stream-analytics"></a>Megjegyzések a 12/10/2015 kiadását Értékáram-elemzés ##

Ebben a kiadásban tartalmazza a következő frissítést.

Cím | Leírás
---|---
REST API-verziófrissítés | 2015-10-01 a REST API-verzióra frissült. Részletek [Adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx) és [gépi tanulási integrációs Értékáram-elemzés az](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md)MSDN webhelyen találhatók.
Azure gépi tanulási integrációja | Az ebben a kiadásban megtalálható a támogatás kérése az Azure gépi tanulási a felhasználó által definiált függvények. Az [oktatóanyag](stream-analytics-machine-learning-integration-tutorial.md) további információk, valamint a [küldésről általános blogban](http://blogs.technet.com/b/machinelearning/archive/2015/12/10/apply-azure-ml-as-a-function-in-azure-stream-analytics.aspx)talál.

## <a name="notes-for-11122015-release-of-stream-analytics"></a>Jegyzetek 11/12/2015 kiadásának Értékáram-elemzés ##

Ebben a kiadásban tartalmazza a következő frissítést.

Cím | Leírás
---|---
Jelölje ki az új viselkedése | Jelölje ki a Értékáram-elemzés kibővítése lehetővé teszi * mint egy beágyazott rekord egy tulajdonság hozzáférő. További információért olvassa el a [http://msdn.microsoft.com/library/mt622759.aspx](http://msdn.microsoft.com/library/mt622759.aspx "Összetett adattípusok").

## <a name="notes-for-10222015-release-of-stream-analytics"></a>Jegyzetek 10/22/2015 kiadásának Értékáram-elemzés ##

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

Cím | Leírás
---|---
További lekérdezési nyelvekkel kapcsolatos szolgáltatások | Értékáram-elemzés kibővült a lekérdezési nyelvről, többek között a következő funkciók: [ABS](https://msdn.microsoft.com/library/azure/mt574054.aspx), [plafon](https://msdn.microsoft.com/library/azure/mt605286.aspx), [kitevő](https://msdn.microsoft.com/library/azure/mt605289.aspx), [padló](https://msdn.microsoft.com/library/azure/mt605240.aspx), [kiemelt](https://msdn.microsoft.com/library/azure/mt605287.aspx), [bejelentkezési](https://msdn.microsoft.com/library/azure/mt605290.aspx), [SZÖGLETES](https://msdn.microsoft.com/library/azure/mt605288.aspx)és [gyök](https://msdn.microsoft.com/library/azure/mt605238.aspx).
Eltávolított összesítő korlátai  | Ebben a kiadásban eltávolítja a 15 összegzések korlátozása egy lekérdezésben. Most már nem lekérdezése összegzések száma nincs korlátozva.
A felvett csoport által System.Timestamp szolgáltatás | A [GROUP BY](https://msdn.microsoft.com/library/azure/dn835023.aspx) most funkcióval window_type vagy a [System.Timestamp](https://msdn.microsoft.com/library/azure/mt598501.aspx).
Tumbling és a Hopping windows hozzáadott ELTOLÁSA | Alapértelmezés szerint windows [Tumbling](https://msdn.microsoft.com/library/azure/dn835055.aspx) és [Hopping](https://msdn.microsoft.com/library/azure/dn835041.aspx) igazított ellen nulla időt (1/1/0001 12:00:00 de UTC). Az új (nem kötelező) paraméter "offsetsize" lehetővé teszi, hogy egy egyéni eltolás (vagy igazítás) megadása.


## <a name="notes-for-09292015-release-of-stream-analytics"></a>Jegyzetek 09/29/2015 kiadásának Értékáram-elemzés ##

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

Cím | Leírás
---|---
Azure IoT csomagja nyilvános előzetes verzió | Értékáram-elemzés az Azure IoT csomagot nyilvános mintája szerepel.
Azure portál-integráció | Nemcsak a folyamatos jelenlét az Azure adatkezelési portálon Értékáram-elemzés most integrálva van az [Azure-portálon](https://azure.microsoft.com/overview/preview-portal/). Jegyzet, hogy az előnézeti portálon funkció jelenleg adatfolyam Analytics a funkciót csak egy részhalmazát ajánlja fel az Azure adatkezelési portálon anélkül, hogy a böngészőben a lekérdezés tesztelése esetén támogatása a Power BI kimeneti konfiguráció, és tallózással, vagy új bemeneti és kimeneti erőforrásokat van hozzáférése az előfizetések létrehozása.
DocumentDB kimeneti támogatása | Adatfolyam-elemző feladatok most kimenő [DocumentDB](https://azure.microsoft.com/services/documentdb/).
IoT központi beviteli támogatása | Adatfolyam Analytics feladatok most is ingest IoT hubok adatait.
IDŐBÉLYEG szerint eltérő eseményekre vonatkozóan: | Ha egyetlen adatfolyam időbélyegeket problémákat különböző mezőkben lévő többféle esemény tartalmaz, most segítségével [IDŐBÉLYEG által](http://msdn.microsoft.com/library/mt573293.aspx) kifejezésekkel együtt adja meg a másik időbélyeg mezőket minden esetben.

## <a name="notes-for-09102015-release-of-stream-analytics"></a>Jegyzetek 09/10/2015 kiadásának Értékáram-elemzés ##

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

Cím|Leírás
---|---
Támogatás a PowerBI-csoportok|Ahhoz, hogy a megosztási adatok a Power BI másokkal, Értékáram-elemzés feladatok most írhat a [PowerBI-csoportok](stream-analytics-define-outputs.md#power-bi) a Power BI számla belüli.

## <a name="notes-for-08202015-release-of-stream-analytics"></a>Jegyzetek 08/20/2015 kiadásának Értékáram-elemzés ##

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

Cím|Leírás
---|---
A felvett utolsó függvény |Az [utolsó](http://msdn.microsoft.com/library/mt421186.aspx) függvény már Értékáram-elemzés feladatok, úgy beolvasni a legutóbbi esemény-esemény adatfolyamban egy adott időszakon belül érhető el.
Új tömb függvények|Tömb függvények [GetArrayElement](http://msdn.microsoft.com/library/mt270218.aspx), [GetArrayElements](http://msdn.microsoft.com/library/mt298451.aspx) és [GetArrayLength](http://msdn.microsoft.com/library/mt270226.aspx) most érhetők el.
Új rekord függvények|Rekord függvények [GetRecordProperties](http://msdn.microsoft.com/library/mt270221.aspx) és [GetRecordPropertyValue](http://msdn.microsoft.com/library/mt270220.aspx) most érhetők el.

## <a name="notes-for-07302015-release-of-stream-analytics"></a>Jegyzetek 07/30/2015 kiadásának Értékáram-elemzés ##

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

Cím|Leírás
---|---
A Power BI szervezeti azonosító leválasztott az Azure-azonosító|Ez a szolgáltatás lehetővé teszi, hogy [a Power BI kimeneti](stream-analytics-power-bi-dashboard.md) ASA feladatok a bármely Azure-fiók típusa (Live ID azonosító vagy szervezeti azonosító). Ezenkívül telepítve van egy Szervezetiazonosító-alapú az Azure-fiók és egy másik a Power BI kimeneti engedélyező használható.
Szolgáltatás Bus sorban várakozó kimeneti támogatása|Értékáram-elemzés feladatok a [Szolgáltatás Bus sorban várakozó](stream-analytics-define-outputs.md#service-bus-queues) kimeneti értékeket elérhetők.
Szolgáltatás Bus témakörök kimeneti támogatása|Értékáram-elemzés feladatok a [Szolgáltatás Bus témakörök](stream-analytics-define-outputs.md#service-bus-topics) kimeneti értékeket elérhetők.

## <a name="notes-for-07092015-release-of-stream-analytics"></a>Jegyzetek 07/09/2015 kiadásának Értékáram-elemzés ##

Ebben a kiadásban tartalmazza az alábbi frissítéseket.


Cím|Leírás
---|---
Egyéni Blob-kimeneti szétválasztás|BLOB-tároló kimeneti értékeket most segítségével adja meg az ismétlődési lehetővé teszi, a kimeneti BLOB készül és szerkezetének és a kimeneti elérési út mappaszerkezet formátumát. 

## <a name="notes-for-05032015-release-of-stream-analytics"></a>Jegyzetek 05/03/2015 kiadásának Értékáram-elemzés ##

Ebben a kiadásban tartalmazza az alábbi frissítéseket.


Cím|Leírás
---|---
Maximális érték nagyobb ki a sorrend hibatűrést ablak|A maximális mérete a blokkolás a rendelés hibatűrést ablakban mostantól 59:59 (PP: mm)
JSON kimeneti formátumban: Elválasztott sor vagy tömb|Most már van egy beállítást Blob-tárolóhoz vagy az esemény központi mindkét tömb JSON-objektumok vagy az új sor JSON objektumok elválasztva kimeneti exportálásakor. 

## <a name="notes-for-04162015-release-of-stream-analytics"></a>Jegyzetek 04/16/2015 kiadásának Értékáram-elemzés ##


Cím|Leírás
---|---
Az Azure tároló fiókbeállítás késleltetése|Értékáram-elemzés feladat létrehozása egy tartomány első alkalommal, a rendszer kéri, hozzon létre egy új tárterület-fiókot, vagy adjon meg egy meglévő fiókot az adott régióban Értékáram-elemzés projektek figyelemmel kísérésére. Késés konfigurálása ellenőrzése, hogy egy másik Értékáram-elemzés feladat létrehozása az ugyanabban a régióban 30 percen belüli kérni fogják felhasználóiktól a második tároló fiók helyett a legutóbb beállított egy figyelése tárhelyet fiók legördülő megjelenítő megadására szolgáló. Egy fölösleges tárterület-fiók létrehozása elkerüléséhez várja meg, hogy a feladat létrehozása egy tartomány további feladatokat az adott régióban kiépítése előtt először után 30 percig.
Projekt frissítése|Ekkor a Értékáram-elemzés nem támogatja az élő szerkesztéseket a definíció vagy egy futó feladat beállításait. Beviteli, kimeneti, lekérdezés, skála és konfigurációs aktív feladat módosítása, hogy le kell állítania a feladatot.
Beviteli forrásból származó következtetett adattípusok|A CREATE TABLE utasítás nem használható, ha a beviteli típus beviteli formátum beállításból származik, például CSV összes mezőjét karakterlánc. Mezők kell a megfelelő típusát a CAST függvény használata annak érdekében, hogy a típus eltéréseket hibák elkerülése érdekében kifejezetten alakulnak.
Hiányzó mezőket a null értékek szerint vannak outputted|Egy mezőt, amely nem szerepel a bemeneti forrás hivatkozó a kimeneti esemény null értéket eredményez.
A kimutatások kell megelőző SELECT utasítás|A lekérdezés a SELECT utasítások segédlekérdezések definiált kimutatások be kell követnie.
A memória out probléma|Indítsa újra a kimenő sorrendben események és/vagy összetett lekérdezéseket fenntartása állam nagy mennyiségű okozhat a feladat fogyni a memória, így a feladat egy nagy hibatűrést adatfolyam Analytics feladatokat. Az indítási és leállítási műveletek a feladat művelet naplók látható lesz. Ez a probléma elkerülése érdekében méretezze át a lekérdezést, több partíciót keresztül. Az elkövetkező kiadásokban a korlátozás fog megoldást elrontása teljesítménye a probléma által sújtott feladatok helyett újraindítását őket.
Nagy blob bemenetben tartalom időbélyegző nélkül okozhat Out-memória elfogyását jelző hiba|Nagyméretű fájlok Blob-tárolóból használata más okozhat a Értékáram-elemzés feladatok összeomlik, ha időbélyegző mező nem szerepel a via IDŐBÉLYEG által. A probléma elkerülése érdekében, hogy ne minden blob 10MB alatti méretét.
SQL-adatbázis esemény mennyiségi korlátozása|SQL-adatbázis-kimeneti célként használatakor kimeneti adatai nagyon nagy mennyiségű jelenhet meg a megjelenítő Értékáram-elemzés feladat időtúllépést okoz. A probléma megoldásához a kimeneti hangerő csökkentése összegzések vagy szűrő operátorok használatával, vagy válassza az Azure Blob-tárolóhoz vagy esemény hubok egy kimenet célként helyette.
PowerBI adatkészleteket csak egy táblát tartalmazhat|PowerBI nem támogatja az egy adott adatkészlet egynél több táblát.

## <a name="get-help"></a>Segítség kérése
További segítségért próbálja meg az [Azure-adatfolyam Analytics-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések

- [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md)
- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 

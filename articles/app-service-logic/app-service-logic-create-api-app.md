<properties 
    pageTitle="Hozzon létre egy API összefüggés-alkalmazások" 
    description="Logika alkalmazások használata egy egyéni API létrehozása" 
    authors="jeffhollan" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na" 
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>
    
# <a name="creating-a-custom-api-to-use-with-logic-apps"></a>Logika alkalmazások használata egy egyéni API létrehozása

Ha azt szeretné, ha ki szeretné terjeszteni a logika alkalmazások platform, számos módon felhívása API-khoz vagy rendszerek, amelyek nem érhetők el a sok ki-be az összekötők egyikeként.  Egy adott módon hozhat létre is felhívhatja logika alkalmazás munkafolyamat API-alkalmazást.

## <a name="helpful-tools"></a>Hasznos eszközök

A legjobban működjenek az logika alkalmazások API-k ajánlott a támogatott műveletek és a paraméterek részletező az API [swagger](http://swagger.io) dokumentum létrehozása.  Vannak olyan sok tárak (például [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle)), amely automatikusan létrehozza a swagger meg.  [TRex](https://github.com/nihaue/TRex) segíti a swagger összefüggés-alkalmazások (megjelenítése a neveket, tulajdonság típusok stb.) is dolgozhat a jegyzet is használhatja.  Egyes minták logika alkalmazások épített API-alkalmazások feltétlenül nézze meg a [GitHub tárházba](http://github.com/logicappsio) vagy egy [blogba](http://aka.ms/logicappsblog).

## <a name="actions"></a>Műveletek

Az egyszerű művelet összefüggés-alkalmazásokban egy vezérlő, amely HTTP felkérés elfogadása és választ (általában 200).  Azonban nincsenek különböző mintázatok követheti, ha ki szeretné terjeszteni a szükségletek műveletek.

Alapértelmezés szerint a logika alkalmazás motor lesz időtúllépés kérelmének 1 perc múlva.  A API, hajtsa végre a műveleteket, amelyeket a hosszabb időt vehet igénybe, és a részletes alatti aszinkron vagy webhook mintát követve befejeztével Várakozás motor van azonban is.

A szokásos műveletek egyszerűen írja be egy HTTP kérelmek módja az API-hoz van téve swagger keresztül.  A [GitHub tárházba](https://github.com/logicappsio)összefüggés-alkalmazásokkal is dolgozhat API-alkalmazások mintái tekintheti meg.  Az alábbi módon elvégezheti az egyéni összekötő közös mintázatok.

### <a name="long-running-actions---async-pattern"></a>Hosszú futó műveletek - aszinkron mintával

Hosszú lépés vagy feladat futtatása, a legfontosabb dolog kell tennie esetén ellenőrizze, hogy a motor tudja, hogy még nem időtúllépési. Is kell a motor kommunikál, hogyan, tudni fogja, hogy amikor befejezte a feladatot, és végül vonatkozó adatokat szeretne térni a motor, a munkafolyamat továbbra is szükséges. Hajthatja végre, amely egy API-e a folyamat az alábbi követve. Ezeket a lépéseket a pont-a-nézetben az egyéni API a következők:

1. A kérelem érkezik, azonnal adja vissza választ (előtt munkát). A válasz lesz egy `202 ACCEPTED` válasz, a motor, hogy szerezte be az adatokat, hogy a tartalom elfogadott, és most már vannak feldolgozása. A 202 választ kell tartalmaznia az alábbi fejlécek: 
 * `location`fejléc (kötelező): Ez az abszolút URL-cím összefüggés-alkalmazás elérési út segítségével a feladat állapotának ellenőrzése.
 * `retry-after`(nem kötelező, lesz az alapértelmezett műveletek 20). Az a motor a hely URL-cím állapotának ellenőrzéséhez lekérdezési várakozási másodpercek számát.

2. Ha a feladat állapota be van jelölve, hajtsa végre a következő ellenőrzése: 
 * Ha befejeződött a feladat: vissza egy `200 OK` válasz, a válasz tartalom.
 * Ha a feladat továbbra is feldolgozása: vissza egy másik `202 ACCEPTED` válasz, a kezdeti válaszban azonos élőfejjel

A minta lehetővé teszi az egyéni API szál rendkívül hosszú tevékenységeinek futtatása, de ne az aktív kapcsolat életben a logika alkalmazások motor, akkor nem időtúllépés, vagy továbbra is a munka befejezése előtt. Ez a logika alkalmazásba hozzáadása, fontos vegye figyelembe, nem kell tennie semmit sem a definíciójának a összefüggés-alkalmazás továbbra is lekérdezik és az állapot ellenőrzése. Amint a motor által látott nézetet érvényes helyet fejlécet tartalmazó 202 elfogadott választ, akkor fog figyelembe veszi a aszinkron mintázatot, és továbbra is lekérdezik a hely fejléc, amíg nem 202 ad vissza.

Megjelenik egy mintája szerepel a minta GitHub [Itt](https://github.com/jeffhollan/LogicAppsAsyncResponseSample)

### <a name="webhook-actions"></a>Webhook műveletek

A munkafolyamat-során beállíthatja, hogy a logika alkalmazást, mutasson az egérrel, és várja meg "visszahívási" folytatásához.  A visszahívási megtalálható a HTTP POST formájában.  A minta végrehajtásához meg kell adnia végpontokat a vezérlőn: előfizetés és mondani.

A "előfizetés" a logika alkalmazás létrehozása, és a visszahívási URL-címet, amely a API tárolhatja és a visszahívás regisztrálása kész egy HTTP Küldés pontra.  Minden olyan tartalom fejlécek fog átadandó a logika alkalmazásba, és a munkafolyamat maradéka belül is használható.  Az alkalmazás logika motor az előfizetés pont visszahívja a végrehajtás, amint azt a találatok ezt a lépést.

A Futtatás meg lett szakítva, ha az alkalmazás logika motor fog hívás az "előfizetés lemondása" végpontot.  A API majd a visszahívás URL-címet is unregister, szükség szerint.

A logikai alkalmazás tervező jelenleg nem támogatja a felfedezése a keresztül swagger, egy webhook végpontot, használni a művelet a következő típusú kell adni a "Webhook" műveletet, és adja meg az URL-CÍMÉT, a fejléc és a összehívás törzsében.  Használhatja a `@listCallbackUrl()` munkafolyamat függvény bármely el átadni a visszahívás URL-CÍMÉT a szükséges módosításokat.

Megjelenik egy mintája szerepel az GitHub webhook mintát [Itt](https://github.com/jeffhollan/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs)

## <a name="triggers"></a>Eseményindítók

A műveletek mellett az egyéni API act egy logikai alkalmazásba kiindulópontként is.  Van két mintázatok alatti követve egy összefüggés-alkalmazás elindítása:

### <a name="polling-triggers"></a>Lekérdezési indítók

Lekérdezési indítók működjön a fenti hosszú operációs rendszert futtató aszinkron műveletek hasonlóan.  Az alkalmazás logika motor visszahívja az eseményindító végpontot után egy bizonyos ideje eltelt idő (függő Termékváltozat, 15 másodperc prémium verzióban 1 perc szabványos, és a szabad az 1 óra).

Ha nincs adat elérhető, az eseményindító adja eredményül egy `202 ACCEPTED` válasz, a egy `location` és `retry-after` fejléc.  Azonban eseményindítók ajánlott a `location` fejlécben: lekérdezési paraméter `triggerState`.  Ez a néhány azonosító mikor utoljára a logika alkalmazás akkor következik be, hogy az API-t.  Ha nincs adat elérhető, az eseményindító ad vissza egy `200 OK` válasz a tartalom tartalom.  Ez lesz fire a logika alkalmazást.

Ha például e lett lekérdezési annak ellenőrzéséhez, hogy a fájl elérhető volt a lekérdezési eseményindító, tegye a következőket kellene sikerült generál:

* Ha nincs triggerState egy kérés érkezett az API adja vissza egy `202 ACCEPTED` az egy `location` fejléc, amely tartalmazza az aktuális idő egy triggerState és egy `retry-after` 15.
* Ha egy kérés érkezett egy triggerState:
 * Ellenőrizze, hogy ha a DateTime triggerState után a fájlok lettek hozzáadva. 
  * Ha fájlt 1, a visszatérési érték egy `200 OK` a tartalom tartalom válasz növelni szeretné a fájlt e vissza, és adja meg a DateTime a triggerState a `retry-after` -15.
  * Ha több fájlt, 1 lehet térhet vissza, egyszerre egy `200 OK`, a saját triggerState növelése az `location` fejléc, és a `retry-after` pedig 0.  Ez az, hogy több adat áll rendelkezésre, és azonnal kér őket az motor lehetővé a `location` megadott fejléc.
  * Ha nincsenek fájlok, adja vissza egy `202 ACCEPTED` választ, és kilépés a `location` triggerState azonos.  Beállítása `retry-after` -15.

Megjelenik egy mintája szerepel az GitHub lekérdezési eseményindító [Itt](https://github.com/jeffhollan/LogicAppTriggersExample/tree/master/LogicAppTriggers)

### <a name="webhook-triggers"></a>Eseményindítók Webhook

Webhook indítók Webhook műveletek a fenti hasonlóan működnek.  Az alkalmazás logika motor visszahívja az "előfizetés végpontot, valahányszor webhook az eseményindító hozzáadva, és menti.  A API a webhook URL-cím regisztrálásához és a HTTP-bejegyzés keresztül hívja meg, valahányszor adatokat érhető el.  A tartalom hasznos és a fejlécek futtassa a logika alkalmazásba adható át.

Az eseményindító webhook minden eddiginél törlésekor (vagy a logika alkalmazás teljesen, vagy csak az webhook eseményindító), a motor kezdeményezése az "előfizetés lemondása" URL-címre hol az API unregister is a visszahívás URL-CÍMÉT, és azt leállítása folyamatokat, szükség szerint.

A logikai alkalmazás tervező jelenleg nem támogatja a felfedezése a webhook az eseményindító swagger, keresztül, a művelet a következő típusú használni kell felvétele az "Webhook" eseményindító, és adja meg az URL-CÍMÉT, a fejléc és a összehívás törzsében.  Használhatja a `@listCallbackUrl()` munkafolyamat függvény bármely el átadni a visszahívás URL-CÍMÉT a szükséges módosításokat.

Megjelenik egy mintája szerepel az GitHub webhook eseményindító [Itt](https://github.com/jeffhollan/LogicAppTriggersExample/tree/master/LogicAppTriggers)
<properties
 pageTitle="Prediktív karbantartási előre megoldás |} Microsoft Azure"
 description="Az Azure IoT előre prediktív karbantartási megoldás leírását."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="stevehob"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/17/2016"
 ms.author="araguila"/>

# <a name="predictive-maintenance-preconfigured-solution-overview"></a>Előre beállított prediktív karbantartási megoldás – áttekintés

A *cserélendő karbantartási* előre-megoldás, egy [előre definiált megoldások] [ lnk_preconfigured_solutions] [Microsoft Azure IoT Suite]részeként[lnk_iot_suite]. Ez a megoldás valós idejű eszköz telemetriai webhelycsoport integrálódik készült [Azure gépi tanulási]prediktív modell[lnk_machine_learning].


Az Azure IoT csomagot nagyvállalatoknak is gyorsan és egyszerűen csatlakozás Lync-eszközök és valós idejű adatok elemzése. Az előre beállított prediktív karbantartási megoldás megnyitja az adatokat, és használja a multimédiás irányítópultok és a Megjelenítés vállalkozások számára is meghajtó hatékonyságot, és javíthatja minőségét a bevétel adatfolyamok új intelligenciával.

## <a name="the-scenario"></a>Az alkalmazási példát

A Fabrikam egy területi légitársaság, amely a versenytársak áron nagyszerű felhasználói élmény koncentrál. Nézetbeli egyik oka karbantartási problémák és repülőgép motor karbantartási különösen nehéz. Motor hiba repülőút kerülni minden áron kell, Fabrikam annak motorok rendszeresen megvizsgálja, és nakhttp://go.microsoft.com/fwlink/?LinkId=311864 megfelelően igazodik ütemezett karbantartás programot. Repülőgép motorok mindig nem viselniük azonos. Néhány szükségtelen karbantartási motorok történik. Fontosabb problémák merülnek fel, amelyek repülőgép is szabad, mindaddig, amíg a végrehajtott karbantartási. Ekkor a költséges késleltetése, különösen akkor, ha repülőgép egy helyen, ahol nem érhetők el a megfelelő szakemberek vagy alkatrészek.

A Fabrikam repülőgép motorok a Lync-motor feltételek repülőút érzékelők vannak rendszereken. A Fabrikam Azure IoT csomagja segítségével a repülőút gyűjtött érzékelő adatgyűjtés. Után műveleti motor halmozódó év és a hiba adatait a Fabrikam adatok tudósok modellezni van a hátralévő hasznos élettartama (Szabályainak) repülőgép motor előrejelzésére lehetőséget. Mik azok azonosította egy korrelációs négy a motor érzékelőket és az esetleges hibát eredményez, motor kopás származó adatok között. Biztonság érdekében rendszeres ellenőrzések végrehajtásához Fabrikam továbbra is fennáll, miközben meg most már használhatja a modellek számítja ki az egyes motor Szabályainak, a repülőút motor összegyűjtése minden nézetbeli a telemetriai használata után. Fabrikam most hiba és tervet karbantartási jövőbeli pontjai előrejelzésére, és javítsa ki a korábban.

> [AZURE.NOTE] A megoldás modell motor tényleges kopás adatokat használja.

Karbantartási esedékességekor-beli a pont, által Fabrikam optimalizálhatja az műveleteihez költségek csökkentése érdekében. Karbantartási koordinátorok karbantartási megfelelően leállítása egy adott helyen és a kellő időben az anélkül, hogy az ütemezés zavarok szolgáltatás ki lesz a repülőgép biztosítása repülőgép megtervezésére bejegyzéstípusait dolgozhat. A Fabrikam ütemezhet szakembereknek. biztosítja repülőgép hatékony kiszolgált vannak várakozási idő nélkül. Készlet vezérlő menedzserek karbantartási terveknek, jelenik meg úgy, hogy a rendelési folyamatot optimalizálása, tartalék kijelzők készletértékét. Az összes lehetővé teszi, hogy a Fabrikam repülőgép alapokról idő csökkentése érdekében, és a biztonsági személy-és személyzet biztosítva működési költségek csökkentése.

Megértéséhez hogyan [Azure IoT csomagja] [ lnk_iot_suite] biztosít a funkciók ügyfelek kell prediktív karbantartási lehetőségeinek, olvassa el a [infographic][lnk_infographic].

## <a name="how-the-predictive-maintenance-solution-is-built"></a>Hogyan a cserélendő karbantartási megoldást épül.

A megoldás egy meglévő Azure gépi tanulási modell érhető el, e ezekre a lehetőségekre IoT csomagja szolgáltatások gyűjtött eszköz telemetriai dolgozik megjelenítéséhez sablonként használja. Microsoft létrehozta a [regressziós modell] [ lnk_regression_model] repülőgép motor és a teljes sablont, az adatok közzétett<sup>\[1\]</sup>, és részletes útmutatást a modell használatát.

Az Azure IoT előre prediktív karbantartási megoldás a regressziós modellt, a sablonból; létrehozott használ az Azure-előfizetése rendszerbe, és az automatikusan generált API keresztül elérhetővé tett. A megoldást is (az összes 100) 4, amely a tesztelés adatok alegységének motorok és a 4 (teljes 21) egy pontos eredménye a képzett modellből biztosító érzékelő adatfolyamok.

*\[1\] válaszokhoz Saxena és K. Goebel (2008). "Turbóventillátoros motor csökkenés szimulációk adatkészlet", NASA Ames Prognostics adatok tárházba (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), NASA Ames kutatási központ, Moffett mező, hitelesítésszolgáltató*

## <a name="next-steps"></a>Következő lépések

Szeretne többet megtudni, hogyan Azure IoT prediktív karbantartási forgatókönyveket teszi lehetővé, hogy olvassa el a [Rögzítés mezőbe írja be az internetes dolgot][lnk_capture_value].

Venni [Útmutató] [ lnk-predictive-walkthrough] a cserélendő karbantartás előre a megoldást.

[lnk-predictive-walkthrough]: iot-suite-predictive-walkthrough.md
[lnk_preconfigured_solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk_iot_suite]: iot-suite-overview.md
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk_infographic]: https://www.microsoft.com/server-cloud/predictivemaintenance/Index.html
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3
[lnk_capture_value]: http://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF

Egyéb funkciók közül egyesek és funkciók előre IoT csomagja megoldást is feltárása:

- [Gyakori kérdések IoT csomagja][lnk-faq]
- [IoT biztonsági alapoktól][lnk-security-groundup]

[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md

<properties 
    pageTitle="Mik azok a logika alkalmazások?" 
    description="Többet szeretne tudni az alkalmazás szolgáltatás logikájának alkalmazások" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article" 
    ms.date="10/12/2016"
    ms.author="klam"/>

# <a name="what-are-logic-apps"></a>Mik azok a logika alkalmazások?

Logika alkalmazások egyszerűsítése és méretezhető integrációs és a munkafolyamat végrehajtása a felhőben ad lehetőséget. A minta és a folyamat munkafolyamat neve lépést adatsorként automatizálása vizuális Tervező biztosít.  Vannak olyan [sok összekötők](../connectors/apis-list.md) felhő és a szolgáltatások és protokollok gyorsan integrálása helyszíni.  Egy logikai alkalmazást az eseményindító (like "fiókkal való hozzáadásakor Dynamics CRM rendszerben") kezdődik és az után robbantás előkészületek nélkül használhatja a sok kombinációk műveletek, átalakítások és feltétel összefüggés.

A logikai alkalmazások használatának előnyei közé tartoznak az alábbiak:  

- Időmegtakarítás tervezőeszközök megértéséhez egyszerű használata összetett folyamatok kialakítása
- Minták és munkafolyamatok zökkenőmentes végrehajtási, egyébként lenne nehezen megvalósítását a kódot.
- Gyorsan sablonok használatába
- Saját egyéni API-hoz, kódot és műveletek az logika alkalmazás testreszabása
- Csatlakozás és elemezve rendszerek szinkronizálás a helyszíni és felhőbeli
- Osztályú integrációs támogatásával elhagyja BizTalk server, API-kezelés, Azure funkciók és Azure Service Bus összeállítása

Alkalmazások összefüggés áll egy teljes körű felügyelt iPaaS (integrációs Platform szolgáltatásként) lehetővé teszi a fejlesztők számára, hogy nem kell aggódnia amiatt épület szolgáltatónál, méretezhetőség, elérhetősége és kezelése.  Logika alkalmazások fog méretezni automatikusan igényeknek.

![Adatfolyam-alkalmazás designer](./media/app-service-logic-what-are-logic-apps/LogicAppCapture2.png)

Említett, logika alkalmazással automatizálhatja az üzleti folyamatok. Íme néhány példa:  
 
* Azure tárolóba FTP-kiszolgálón feltöltött fájlokat áthelyezése
* Folyamat és az útvonal rendelések végig a helyszíni és felhőbeli rendszerek
* Egy adott témával kapcsolatban az összes twitterre figyelése, a sentiment elemzése és értesítések és a feladatok nyomon követést erre szolgáló elemek létrehozása.

Esetek az alábbiakhoz is beállítható, az összes vizuális Tervező eszközében, és az egysoros kód írása nélkül. Első lépések [a logikájának app most összeállítását][create].  [Miután írt - egy logikai alkalmazás gyorsan rendszerbe és](app-service-logic-create-deploy-template.md) lehet újrakonfigurálni különböző régiók és több környezetben.

## <a name="why-logic-apps"></a>Miért logika alkalmazások?

Logika alkalmazások sebességétől és méretezhetőség: elérhetővé teszi a nagyvállalati integrációs területre.  Az egyszerű használat a designert, különféle elérhető eseményindítók és műveletek és hatékony eszközeit győződjön az API-khoz eddiginél egyszerűbb összegyűjtő.  Logika alkalmazások vállalkozások mozgatása digitalization felé, korábbi és legmodernebb rendszerek összekapcsolása segítségével.

Emellett a [Nagyvállalati integrációs fiókot] a[ biztalk] integrációs forgatókönyvek és az [XML-üzenetküldés]hatványon érett méretezheti[xml], [partner management kereskedési][tpm], és az egyéb.

- **Könnyen kezelhető tervezőeszközök** - logika alkalmazások lehet megtervezni-végpont a böngészőben és a Visual Studio eszközök. Az eseményindító - egyszerű ütemezést indítása egy GitHub probléma jön létre. Kattintson a műveletek használata az összekötők gazdag gyűjteménye tetszőleges számú téve.

- **Csatlakozás API-khoz egyszerűen** – egyszerűen ahhoz, még akkor is kialakítási feladatokat nehezen megvalósítását a kódot. Logika alkalmazások egyszerűen elemezve rendszerek csatlakozni. A felhőben, a helyszíni megoldás marketing csatlakoztatni kívánt számlázási rendszert? API-k és az egy vállalati szolgáltatás Bus rendszerek üzenetküldés központosítása szeretne? Logika végzi a megoldások ezeknek a problémáknak leginkább megbízható leggyorsabb módja is.

- **Első lépések a sablonok közül gyorsan** – az első lépésekhez biztosítunk Önnek a [sablonok gyűjteménye] [ templates] , amely lehetővé teszi, hogy gyorsan létrehozása az egyes általános megoldásokkal. A speciális B2B megoldások egyszerű szoftver kapcsolatot, és pár, amelyek csak "számára egy kis játék" – a gyűjtemény a power logika alkalmazások – első lépések a leggyorsabb módja.

- **Bővítési sütött az** - nem látható az összekötő van szüksége? Logika alkalmazások tervezve saját API-hoz és a kód; egyszerűen létrehozhat saját API-alkalmazások használja az egyéni összekötő, vagy az [Azure függvény](https://functions.azure.com) végrehajtani a kód igény szerinti kódrészletek felhívása. 

- **Valós integrációs lóerő** - indítsa el a könnyen, és szükség szerint a nagyobb. Alkalmazások logikájának is egyszerűen használja ki az BizTalk, a Microsoft üzleti vezető integration megoldási integration szakemberek szükségük van, a megoldások. Részletesebben a [Vállalati integrációs csomagot](./app-service-logic-enterprise-integration-overview.md).

## <a name="logic-app-concepts"></a>Logika alkalmazás fogalmak

Az alábbiakban néhány, amely tartalmazza az logika alkalmazások, amikor a kulcsfontosságú. 

- **Munkafolyamat** - logika alkalmazások biztosít az üzleti folyamatok modell lépéseket, és a munkafolyamat, grafikus lehetőséget.
- **Felügyelt összekötők** – a logika alkalmazások adatok és szolgáltatások hozzáféréssel kell rendelkeznie. Felügyelt összekötők kifejezetten a támogatási, amikor kapcsolódik, és az adatok használata jönnek létre. Most már elérhető [kezelt összekötők]összekötők listájának megtekintéséhez[managedapis].
- **Eseményindítók** – néhány felügyelt összekötők is működhet az eseményindító. A kiváltó ok mező egy adott esemény, például e-mailt vagy a Azure tárterület-fiókjában módosítás érkezését-alapú új példányát indítja el.
-  **Műveletek** – minden egyes lépés után az munkafolyamatokban eseményindító művelet nevezik. Minden egyes művelet általában a felügyelt összekötő vagy egyéni API-alkalmazások műveletet rendeli hozzá.
- **Nagyvállalati integrációs csomag** – speciális integrációs forgatókönyvek, logika alkalmazások BizTalk a funkciókat tartalmazza. BizTalk szolgáltatás a Microsoft üzleti vezető integrációs platform. A nagyvállalati integrációs csomag összekötők egyszerűen tartalmazza az érvényességi, átalakítási és további a logika alkalmazás munkafolyamatok teszi lehetővé.

## <a name="getting-started"></a>Első lépések  

- Első lépések logikájának alkalmazások, hajtsa végre a [logikájának alkalmazás létrehozása] [ create] oktatóprogram.  
- [Nézet hétköznapi példát és -esetek](app-service-logic-examples-and-scenarios.md)
- [Üzleti folyamatok logika alkalmazások automatizálható](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Megtudhatja, hogy miként rendszerek integrálása logikájának alkalmazások](http://channel9.msdn.com/Events/Build/2016/P462)

[biztalk]: app-service-logic-enterprise-integration-accounts.md
[appservice]: ../app-service/app-service-value-prop-what-is.md
[create]: app-service-logic-create-a-logic-app.md
[managedapis]: ../connectors/apis-list.md
[tpm]: app-service-logic-enterprise-integration-accounts.md
[xml]: app-service-logic-enterprise-integration-b2b.md
[templates]: app-service-logic-use-logic-app-templates.md

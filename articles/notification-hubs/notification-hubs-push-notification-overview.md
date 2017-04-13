<properties
    pageTitle="Azure értesítés hubok"
    description="Megtudhatja, hogy miként használhatja a leküldéses értesítések Azure-ban. A C# a .NET API írt mintakódok."
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"
    documentationCenter=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="multiple"
    ms.devlang="multiple"
    ms.topic="hero-article"
    ms.date="08/25/2016"
    ms.author="yuaxu"/>


#<a name="azure-notification-hubs"></a>Azure értesítés hubok

##<a name="overview"></a>– Áttekintés

Azure értesítés hubok, amely lehetővé teszi, hogy a leküldéses értesítései mobilon küldhessenek bármely kódmentes (a felhőben vagy a helyszíni) bármilyen hordozható platformra könnyen használható, többplatformos, amelyekkel méretarányos telepítés leküldéses infrastruktúrát biztosít.

Az értesítés agyváltók egyszerűen elküldheti-platformok, személyre szabott leküldéses értesítéseket, a részletek, a különböző platform értesítés rendszerek (PNS) abstracting. A egyetlen API-hívást Megcélozhat felhasználónként vagy a teljes közönség szegmensek felhasználója, több millió tartalmazó összes eszközön keresztül.

Vállalati és a fogyasztói felhasználási területei értesítés hubok is használhatja. Példa:

- Legfrissebb hírek értesítések (értesítés hubok hatásköröket előtelepítve minden Windows és a Windows Phone-készülékén a Bing-alkalmazások) alacsony válaszidejű milliós küldje el.
- Helyfüggő kuponokat felhasználói szegmens küldje el.
- Esemény értesítést küld felhasználók vagy csoportok a következőhöz: alkalmazások sports/Pénzügy/játékok.
- Vállalati eseményeket, például az új üzenetek vagy e-maileket, és az üzleti érdeklődőket felhasználók értesítése.
- Küldje el egy-egyszer jelszavak többtényezős hitelesítés szükséges.



##<a name="what-are-push-notifications"></a>Mik azok a leküldéses értesítések

Okostelefonokon és táblaszámítógépeken is "felhasználók értesítése," Ha valamilyen esemény bekövetkezett. Az értesítések többféle lehet.

A Windows áruházból, és a Windows Phone-alkalmazásokat az értesítés lehet egy _bejelentési_formájában: megjelenik egy modális ablak, a hang, a jelet az értesítés új. Más által támogatott értesítés típusú _csempével_ _nyers_és _jelvény_ értesítések tartalmazzák. További információt a Windows-eszközön támogatott értesítések típusú látható [Csempék, a jelvények, és az értesítések](http://msdn.microsoft.com/library/windows/apps/hh779725.aspx)

Apple iOS rendszerű eszközökön a leküldéses hasonlóan egy párbeszédpanel, a felhasználó megtekintéséhez, vagy zárja be az értesítés kérő felhasználó értesítést küld. **Nézet** megnyitása az alkalmazást, az üzenetet fogadó. IOS értesítések kapcsolatos további tudnivalókért olvassa el az [iOS értesítések](http://go.microsoft.com/fwlink/?LinkId=615245)című témakört.

Leküldéses értesítések súgó mobileszközök közben hátralévő energy hatékony friss információk jeleníthetők meg. Értesítések lehet küldeni kódmentes rendszerek mobileszközök akkor is, ha egy eszközön megfelelő alkalmazások nem aktívak. Leküldéses értesítések olyan elengedhetetlen alkatrész fogyasztói alkalmazásokat, felhasználási alkalmazás tetszés szerint elmélyedhet és használatát. Értesítések is hasznos vállalkozások számára, naprakész információk növeli a vállalati események alkalmazott válaszidő beállításokhoz.

Néhány mobil tetszés szerint elmélyedhet esetek adott példák:

1.  Egy mozaikon a Windows 8 vagy Windows Phone frissíteni az aktuális pénzügyi adatait.
2.  Riasztási felhasználó egy bejelentési, amelyet az adott felhasználó egy munkafolyamat-alapú vállalati app rendelt néhány elemet.
3.  Megjelenítés és a szám aktuális értékesítési jelvény végigvezeti a CRM alkalmazásban (például a Microsoft Dynamics CRM).

##<a name="how-push-notifications-work"></a>Hogyan leküldéses értesítések munka

Leküldéses értesítések érkeznek _Platform értesítés rendszerek_ (PNS) nevű platform-specifikus infrastruktúra keresztül. Egy PNS kínál barebones függvények (Ez azt jelenti, hogy nem támogatja a közvetítést, személyre szabási), és a gyakori objektumfelület nem rendelkezik. Például a Windows Áruházbeli alkalmazás értesítést küld, a Fejlesztőeszközök kapcsolatba kell a WNS (a Windows értesítési szolgáltatás). Értesítés küldése az iOS-eszközökön, ugyanazt a Fejlesztőeszközök lépjen kapcsolatba az APN (leküldéses értesítéseket kezelő szolgáltatása Apple), és küldje el az üzenetet egy második alkalommal rendelkezik. Azure értesítés hubok súgó, mert az egy közös felületet, és más funkciók támogatása a leküldéses értesítések minden platformon keresztül.

Magas szintű azonban az összes platform értesítés rendszer kövesse az azonos típusúak:

1.  Az ügyfél alkalmazás kapcsolódik a beolvasni a _kezelheti_az PNS. Fogópont típusát attól függ, hogy a rendszer. WNS, célszerű URI vagy az "értesítési csatornát." APN célszerű jogkivonat.
2.  Az ügyfél alkalmazás tárol a fogópont _háttéradatbázist_ alkalmazás későbbi használatra. WNS a háttéradatbázist általában egy felhőalapú szolgáltatásba. Az Apple a rendszer a _szolgáltató_neve.
3.  Leküldéses értesítést küldhet a alkalmazás háttéradatbázist lép kapcsolatba a PNS a fogópont használata egy adott ügyfél app-példány célba.
4.  A PNS továbbítja az értesítés, hogy az eszközön található fogópontot által megadott.

![][0]

##<a name="the-challenges-of-push-notifications"></a>A leküldéses értesítések problémáit

Habár e rendszerek csak nagyon nagy teljesítményű, továbbra is elhagyják mennyi munka az alkalmazás fejlesztője szeretne még leküldéses értesítést esetei, például közvetítésének lehetőségét, vagy elküldheti a leküldéses értesítések szegmentált felhasználók végrehajtásához.

Leküldéses értesítések mobilalkalmazások cloud services leginkább igényelt szolgáltatások közül. A ennek oka, hogy a szükséges, hogy munka infrastruktúra-e a meglehetősen összetett és az alkalmazás fő üzleti logikai funkcióinak főleg független. Az igény szerinti leküldéses infrastruktúra létrehozásakor a problémáit a következők:

- **Platform függőség.** A más platformokon eszközök küldhet értesítést, több kapcsolat a háttéradatbázist az kell kódolni. Nemcsak a követő részletek eltérőek, de a bemutatót az értesítés (csempére, bejelentési vagy jelvény) is platform-függő. Ezeket a különbségeket vezethet összetett és kemény karbantartása háttéradatbázist kódot.

- **Méretezés.** Ez az infrastruktúra méretezés két szempontból foglalja magában:
    + Használati irányelvek PNS eszköz tokenek frissíteni kell minden alkalommal, amikor az alkalmazást. Ez a forgalom (és csatlakozása adatbázis bejáratok) csak az, hogy az eszköz tokenek naprakészen tartása nagy mennyiségű vezet. Eszközök száma nő (valószínűleg több millió), létrehozása és fenntartása Ez az infrastruktúra költségét esetén nonnegligible.

    + A legtöbb PNSs nem támogatják a közvetítést különféle eszközökön. Következik, hogy a közvetítés, több millió eszközök a PNSs hívásainak milliónyi eredménye. Lehetőséget, ha át kívánja méretezni, ezek a kérelmek oka az Messengerben nem nyilvánvaló, általában alkalmazás fejlesztők szeretné tartani a teljes időtartama. Például az utolsó eszköz az üzenet nem értesítést kell kapniuk a 30 perc után el lett küldve a az értesítéseket, mint sok esetben sértenék célja, hogy a leküldéses értesítéseket.
- **Továbbítás.** PNSs üzenet küldése egy eszközt kínál. Jó helyen jár a legtöbb alkalmazások értesítések célzott felhasználóknak, illetve kamat csoportok (például dolgozókat egy bizonyos ügyfélfiók rendelt). Ilyenként annak érdekében, hogy az értesítések irányítja a megfelelő eszközök, az alkalmazás háttéradatbázist kell karbantartása eszköz tokenek kamat csoportokhoz társít a Registry portálján. Ez a terhelést a teljes időt alkalmazás piaci és karbantartás költségeinek ad.

##<a name="why-use-notification-hubs"></a>Miért érdemes használni az értesítési hubok?

Értesítés hubok kiküszöbölheti összetettsége: nem rendelkezik a leküldéses értesítések problémáit kezeléséhez. Ehelyett egy értesítés hubhoz is használhatja. Értesítés hubok teljes többplatformos, amelyekkel méretarányos telepítés leküldéses értesítést infrastruktúra használatára, és jelentősen csökkenti a leküldéses-specifikus kódot, amely az alkalmazás kódmentes fut. Értesítés hubok leküldéses infrastruktúra a funkciókat hajtja végre. -E csak felelős PNS fogópontok regisztrálása, és a kódmentes felelős a platform beállítástól független üzeneteket küldő felhasználók vagy csoportok, az alábbi ábrán látható módon:

![][1]


Értesítés hubok adja meg egy használatra kész leküldéses értesítést infrastruktúra, a következő előnyökkel jár:

- **Több rendszerek.**
    +  Az összes fő mobil platformokon támogatása. A Windows áruházból, iOS, Android és Windows Phone-alkalmazások értesítés hubok küldhet leküldéses értesítéseket.

    +  Értesítés hubok értesítést küld az összes támogatott platformok közös felületet biztosít. Platform-specifikus protokollok használatuk nem kötelező. Az alkalmazás háttéradatbázist platform-specifikus vagy platform beállítástól független formátumokban értesítést küldhet. Az alkalmazás csak az értesítés hubok kommunikál.

    +  Mobileszköz-kezelés fogópontot. Értesítés hubok tárolja a fogópont beállításjegyzék PNSs visszajelzései.

- **Együttműködik az bármely háttéradatbázist**: felhő, vagy a helyszíni .NET, PHP, Java, csomópontot, stb.

- **Méretezés.** Értesítés hubok tervezővel újra kell vagy shard nélkül eszközök milliónyi méretezhető.


- A **Rich beállítása a kézbesítési mintákra**:

    - *Adás*: lehetővé teszi, hogy a egyetlen API-hívást eszközök milliónyi közelében egyidejű közvetítést.

    - *Egyedi vagy csoportos*: az egyes felhasználók, beleértve az összes azok eszközök; jelölő címkék leküldéses vagy nem elég széles csoport; Ha például külön űrlap tényezők (telefon és táblagép).

    - *Szegmens*: összetett szakaszához címke kifejezés (például a következő a Yankees New York eszközök) által meghatározott leküldéses.

    Minden egyes eszköz, értesítés hubon keresztül csatlakozott, annak fogópont küldésekor is megadhat egy vagy több _címkék_. További információt a [címkék]. Címkék nem rendelkezik előre kiépítéstől vagy értékesített. Felhasználók vagy csoportok kamat értesítéseket küldeni egyszerűvé teszik a címkék. Címkék alkalmazás-specifikus azonosítót (például a felhasználó vagy csoport azonosítók) is tartalmazhatnak, mivel a használatuk szabadítja az alkalmazás háttéradatbázist az, hogy tárolhatják és kezelhetik eszköz fogópontok terhet.

- **Testreszabás**: az egyes beállíthatja, hogy egy vagy több sablonok háttéradatbázis kód megtartásával honosítási eszközök és a személyes beállítások eléréséhez.

- **Biztonsági**: megosztott Access titkos (Társítások) vagy a szövetséges hitelesítési.

- **Rich telemetriai**: elérhető a portál és programozás útján.


##<a name="integration-with-app-service-mobile-apps"></a>Alkalmazás szolgáltatás mobilalkalmazások integrációja

Megkönnyítése érdekében olyan tal zökkenőmentesen és Egységesítő élmény Azure szolgáltatásban, a [Alkalmazás szolgáltatás Mobile-alkalmazások] értesítés hubok használata a leküldéses értesítések támogatása beépített tartalmaz. [Alkalmazás szolgáltatás Mobile-alkalmazások] vállalati fejlesztők és a rendszer kiegészítők mobil fejlesztők funkciók széles körű magasabbra, hogy nagymértékben méretezhető, világszerte elérhető mobilalkalmazás fejlesztési platformot biztosít.

A fejlesztők Mobile-alkalmazások használhat értesítés hubok a következő munkafolyamat:

1. Eszköz PNS fogópont beolvasása
2. Eszköz és a [sablonok] regisztrálása értesítés hubok kényelmes Mobile alkalmazások ügyfél SDK külső.FÜGGV API segítségével
    + Figyelje meg, hogy Mobile-alkalmazások szalagok nem vagyok a gépnél összes címkét a biztonsági okokból regisztrációk. Értesítés hubok dolgozhat a kódmentes közvetlenül az eszközök címkék társítani a.
3. Értesítés küldése az alkalmazás kódmentes az értesítési hubok

Íme néhány szempontból is kényelmes vett fel, az e-integrációval fejlesztői számára:

- **SDK Mobilügyfél-alkalmazások.** A több elem platform SDK regisztrációs egyszerű API-hoz szükséges, és beszélhet az értesítési-központban kapcsolódik a mobilalkalmazás automatikusan. A fejlesztők nem kell alaposabban meg értesítés hubok hitelesítő adatait, és további szolgáltatás használata.
    + A SDK automatikusan nyomon követése a Mobile-alkalmazások hitelesített felhasználói azonosítóval felhasználói forgatókönyv a leküldéses ahhoz, hogy az adott eszközt.
    + A SDK automatikusan használni a Mobile alkalmazások Telepítésazonosítót globálisan egyedi azonosítója regisztrálni a értesítés hubok mentése fejlesztők a problémát, és több szolgáltatás GUID karbantartását jelenti.
    
- **Telepítési modell.** Mobile-alkalmazások működik-e értesítést hubok legújabb leküldéses modell jelenítik meg, hogy a leküldéses értesítést szolgáltatásokkal igazítása és könnyen használható JSON telepítés eszköz társított összes leküldéses tulajdonság.

- **Rugalmas.** A fejlesztők mindig választhat helyen integrációval akár közvetlenül az értesítési hubok végezhető.

- **Jobban integrálhatja a yammert az [Azure-portálon].** Leküldéses egy képesség vizuálisan jeleníti meg a Mobile-alkalmazások és a fejlesztők egyszerű használata a társított értesítés hubon keresztül Mobile-alkalmazások.



##<a name="next-steps"></a>Következő lépések

További tudnivalók a értesítés hubok az itt felsorolt témaköröket találja meg:

+ **[Hogyan felhasználók, akik használják értesítés hubok]**

+ **[Értesítés hubok oktatóanyagok és segédvonalak]**

+ **Értesítés hubok első lépések oktatóanyagok** ([iOS], [Android], [Windows univerzális], [Windows Phone], [Kindle], [Xamarin.iOS], [Xamarin.Android])

A megfelelő .NET felügyelt API-hivatkozások, a leküldéses értesítések találhatók itt:

+ [Microsoft.WindowsAzure.Messaging.NotificationHub]
+ [Microsoft.ServiceBus.Notifications]


  [0]: ./media/notification-hubs-overview/registration-diagram.png
  [1]: ./media/notification-hubs-overview/notification-hub-diagram.png
  [Hogyan felhasználók, akik használják értesítés hubok]: http://azure.microsoft.com/services/notification-hubs
  [Értesítés hubok oktatóanyagok és segédvonalak]: http://azure.microsoft.com/documentation/services/notification-hubs
  [iOS]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started
  [Android-alapú]: http://azure.microsoft.com/documentation/articles/notification-hubs-android-get-started
  [A Windows univerzális]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started
  [Windows Phone]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started
  [Kindle]: http://azure.microsoft.com/documentation/articles/notification-hubs-kindle-get-started
  [Xamarin.iOS]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-ios-get-started
  [Xamarin.Android]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-android-get-started
  [Microsoft.WindowsAzure.Messaging.NotificationHub]: http://msdn.microsoft.com/library/microsoft.windowsazure.messaging.notificationhub.aspx
  [Microsoft.ServiceBus.Notifications]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.aspx
  [Alkalmazás szolgáltatás mobilalkalmazások]: https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-value-prop/
  [sablonok]: notification-hubs-templates-cross-platform-push-messages.md
  [Azure portál]: https://portal.azure.com
  [címkék]: (http://msdn.microsoft.com/library/azure/dn530749.aspx)

<properties
    pageTitle="Azure értesítés hubok – gyakori kérdések"
    description="Gyakori kérdések megvalósításáról tervezése és a értesítés hubok megoldások"
    services="notification-hubs"
    documentationCenter="mobile"
    authors="ysxu"
    manager="erikre"
    keywords="leküldéses értesítést, leküldéses értesítéseket, iOS leküldéses értesítéseket, android leküldéses értesítéseket, ios leküldéses, android leküldéses"
    editor="" />

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu" />

#<a name="push-notifications-with-azure-notification-hubs---frequently-asked-questions"></a>Leküldéses értesítések az Azure értesítés hubok – gyakori kérdések

##<a name="general"></a>Általános
###<a name="1---what-is-the-price-model-for-notification-hubs"></a>1. a Mi az, hogy az ár modell értesítés hubok?
Értesítés hubok a három réteg kínálják:

* **Szabad** - előfizetésenként havonta legfeljebb 1 millió veremmutatót első.
* **Egyszerű** - get 10 millió veremmutatót kvóta NÖV beállításokkal alapterv, mint hó előfizetésenként.
* **Szabványos** - első 10 millió veremmutatót előfizetésenként alapterv, havonta, együtt a kvóta növelje a beállítások, valamint telemetriai gazdag capabilties.

A legfrissebb információkat az [Értesítési hubok árak] lapon találhatók. A árak előfizetés szintre folyamatban van, és a leküldéses értesítést kezdeményezések számát alapul, így mindegy, hogy hány névtér vagy értesítés hubok létrehozott az Azure előfizetés.

**Ingyenes** réteg kínálják nincs SLA garancia fejlesztési célból. A réteg lehet hasznos kiindulási pont azokat, amelyek a leküldéses értesítések keresztül Azure értesítés hubok lehetőségeit megismerheti, amíg nem feltétlenül közepes és nagyméretű alkalmazások a legjobb választás.

**Egyszerű** & **szabványos** rétegek kínálják, az alábbi főbb szolgáltatásokkal gyártási használatát engedélyezve, *csak a Standard esetében az első csoportba tartozó*:

- *Telemetriai Rich* - értesítés hubok ajánlatot az telemetriai adatok exportálása, valamint a leküldéses értesítést regisztrációs adatok kapcsolat nélküli megtekintéshez és elemzési funkciók számot.
- *Több elem bérleti* - ideális, ha értesítést hubok használatával több bérlők támogatja mobilalkalmazásban szeretne létrehozni. Ebben a csoportban adhatja meg a leküldéses értesítést szolgáltatások (PNS) hitelesítő adatait az alkalmazás központi értesítés névtér szinten, és kattintson a bérlők nyújtó őket a közös névtér az egyes hubok is újból. Amíg minden nem határokon-bérlői átfedés biztosítása bérlő & küldése a leküldéses értesítések fogadása az értesítési hubokon Társítások billentyűk megőrzési elkülönített karbantartási Kezeléstechnikai lehetővé teszi.
- *Ütemezett leküldéses* - lehetővé teszi, hogy a leküldéses értesítéseket, később aszinkron és küldött ütemezése.
- A *tömeges importálási* – az alábbi tömegesen regisztrációk importálását teszi lehetővé.

###<a name="2---what-is-the-notification-hubs-sla"></a>2. a Mi az, hogy az értesítési hubok SLA?
Az **egyszerű** és a **szokásos** értesítés hubok rétegek azt garantálja, hogy időt legalább 99,9 %-os megfelelően beállított alkalmazásokat fogja tudni küldje el a leküldéses értesítéseket, vagy regisztráció felügyeleti műveleteket részletez egy értesítés hubhoz belül egy támogatott réteg van telepítve. A SLA kapcsolatos további információért látogasson el az [Értesítési hubok SLA] lapon.

> [AZURE.NOTE] Vannak a láb Platform értesítési szolgáltatás és az eszköz közötti SLA garanciáit óta értesítés hubok attól függenek, külső platform szolgáltatók számára a leküldéses értesítést az eszközön.

###<a name="3---which-customers-are-using-notification-hubs"></a>3. mely felhasználók, akik használják értesítés hubok?
Nagyszámú értesítés hubok használata néhány főbb alábbihoz ügyfelek van:

* 2014-es – 100s Sochi kamat csoportok, a 3 + millió eszközök, a 2 hétből az elküldött 150 + millió értesítés. [CaseStudy - Sochi]
* Skanska - [CaseStudy - Skanska]
* Időpontok Seattle - [CaseStudy - Seattle időpontok]
* Mural.LY - [CaseStudy - Mural.ly]
* 7Digital - [CaseStudy - 7Digital]
* A Bing alkalmazások – az eszközök, több millió 10s értesítések/nap/3 millió küldése.

###<a name="4-how-do-i-upgrade-or-downgrade-my-notification-hubs-to-change-my-service-tier"></a>4. hogyan frissítése, illetve módosíthatja a szolgáltatási réteg az értesítési hubok vissza léptetheti?
Az [Azure klasszikus portálon]kattintson a szolgáltatás Bus, és kattintson a névtér a kattintson az értesítési központi. A méret lapon az értesítési hubok szolgáltatási réteg módosíthatnak fogja.

![](./media/notification-hubs-faq/notification-hubs-classic-portal-scale.png)

##<a name="design--development"></a>Tervezés és a fejlesztés
###<a name="1---which-server-side-platforms-do-you-support"></a>1 melyik kiszolgálóoldali platformokon támogatja?
Elvégezheti a szükséges SDK és a [teljes minták] .NET, Java, PHP, Python, Node.js, hogy egy app kódmentes legyenek értesítés hubok való kommunikáció beállítása következő platformokon bármelyikét használja. Értesítés hubok API-hoz, lehetősége van arra, hogy közvetlenül beszélgetni azok helyett, ha nem szeretne hozzáadni egy további függőség többi felületek alapulnak. További részleteket a [NH - REST API -khoz] lapon találhatók.

###<a name="2---which-client-platforms-do-you-support"></a>2 melyik ügyfél platformokon támogatja?
[Apple iOS](notification-hubs-ios-apple-push-notification-apns-get-started.md)és [Android](notification-hubs-android-push-notification-google-gcm-get-started.md), [Windows univerzális](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md), [Windows Phone](notification-hubs-windows-mobile-push-notifications-mpns.md), [Kindle](notification-hubs-kindle-amazon-adm-push-notification.md), [Android kínai (keresztül Baidu)](notification-hubs-baidu-china-android-notifications-get-started.md), Xamarin küldő leküldéses értesítéseket támogatjuk ([iOS](xamarin-notification-hubs-ios-push-notification-apns-get-started.md) & [Android](xamarin-notification-hubs-push-notifications-android-gcm.md)), [A Chrome-alkalmazások](notification-hubs-chrome-push-notifications-get-started.md) és a [Safari](https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSafari) platformon. Egy teljes listáját az első lépések – oktatóanyagok elleni a következő platformokon küldő leküldéses értesítéseket látogasson el a [NH – első lépések oktatóanyagok] weblapra.

###<a name="3---do-you-support-smsemailweb-notifications"></a>3. támogatják az SMS/E-mail vagy webhely értesítéseket?
Értesítés hubok elsősorban tervezve értesítést küldhet a fent felsorolt platformon használatával mobilalkalmazások. Azt még nem rendelkeznek a képesség küldése e-mailben vagy SMS-értesítések; külső rendszerek, ezeket a lehetőségeket nyújtó azonban integrálhatók natív leküldéses értesítéseket [Azure Mobile-alkalmazások]használatával küldhet értesítést hubok együtt.

Értesítés hubok-a böngésző leküldéses értesítést kézbesítési szolgáltatás ki az-kész nem nyújt. Vevők választhatnak végrehajtásához SignalR használata támogatott platformok kiszolgálóoldali fölött. Ha értesítést küldhet a Chrome védőfalas a böngésző alkalmazások keresi, olvassa el a [Chrome-alkalmazások oktatóprogram].

###<a name="4---what-is-the-relation-between-azure-mobile-apps-and-azure-notification-hubs-and-when-do-i-use-what"></a>4. a Mi az, hogy Azure Mobile-alkalmazások és Azure értesítés hubok közötti kapcsolatot, és mikor használható mi?
Ha egy meglévő mobilalkalmazás kódmentes van, és csak szeretne hozzáadni a leküldéses értesítések küldésére képesség majd használhatja Azure értesítés hubok. Ha szeretne beállítani a mobilalkalmazásban kódmentes nulláról majd vegye figyelembe az Azure Mobile-alkalmazások használata. Az Azure mobilalkalmazás automatikusan kiépítése egy értesítés hubon keresztül csatlakozott, ha engedélyezni szeretné a mobilalkalmazásban kódmentes a egyszerűen küldje el a leküldéses értesítések. Az értesítés hubon Azure Mobile-appokról árak tartalmazza a dátumalapértékhez díjakat, vagy ha csak a mellékelve veremmutatót túl lép. További részleteket a költségeket az [Alkalmazás szolgáltatás árak] lapon érhetők el.

###<a name="5---how-many-devices-can-i-support-if-i-send-push-notifications-via-notification-hubs"></a>5. hány eszközök is lehet támogatják Ha küldöm el valakinek a leküldéses értesítések értesítés hubok keresztül?
Olvassa el a [Értesítés hubok árak] megtalálja a támogatott eszközök számát információkat.

Az egyes alkalmazási eseteit, ha segítségre több, mint 10,000,000 bejegyzett eszközök, kérjük, közvetlenül a [Kapcsolatfelvétel](https://azure.microsoft.com/overview/contact-us/) és azt segítséget nyújtanak méretezni a megoldás.

###<a name="6---how-many-push-notifications-can-i-send-out"></a>6. hány leküldéses értesítéseket is küldése?
Attól függően, hogy a kijelölt réteg Azure automatikusan átméretezi a rendszer áthaladó értesítések száma alapján.

>[AZURE.NOTE] A teljes használati költséget is részletezésből alapján a leküldéses értesítések kiszolgált számára. Győződjön meg arról, hogy a meglévő réteg korlátai [Értesítés hubok árak] lapon tagolt tudatában.

Meglévő ügyfeleink értesítés hubok segítségével küldje el a leküldéses értesítések milliónyi naponta. Nem kell tennie semmi speciális, ha át kívánja méretezni a leküldéses értesítések elérjen mindaddig, amíg az Azure értesítés hubok használja.

###<a name="7---how-long-does-it-take-for-sent-push-notifications-to-reach-my-device"></a>7. mennyi ideig tart az elküldött leküldéses értesítések elérje a készülékemen?
Azure értesítés hubok alkalmas feldolgozása legalább **1 millió leküldéses értesítést küld egy perc** egy normál használata forgatókönyv, ahol a bejövő betöltés meglehetősen konzisztens, és nem spikey jellegű. Ez a kamatláb címkék, a bejövő küld jellegét és az egyéb külső tényezők számától függően változhat.

Becsült szállítási ideje alatt a szolgáltatás nem tudja kiszámítani a célok / platform és az útvonal üzeneteket a megfelelő leküldéses értesítést kézbesítési szolgáltatások a regisztrált címkék/címke kifejezések alapján. Innen a feladata a leküldéses értesítések szolgáltatások (PNS) az értesítés küldése az eszközön.

Egy PNS nem garantálja bármely szolgáltatásiszint-szerződés a kézbesítési értesítések; azonban általában a leküldéses értesítések többsége érkeznek cél eszközök (általában korlátok 10 perc) néhány percen belül a küldi el a platform időpontot. Előfordulhat, hogy néhány kiugró, amely több időt vehet igénybe.

>[AZURE.NOTE] Azure értesítés hubok házirend bármely leküldéses értesítéseket, amelyek nem képesek a PNS 30 percben, kézbesítése lerakási helyen tartozik. A késleltetés akkor fordulhat elő, okokból számú leginkább gyakran, mert a PNS van szabályozásának az alkalmazást.

###<a name="8---is-there-any-latency-guarantee"></a>8. érhető el minden késés garancia?
Leküldéses értesítések (azok érkeznek egy külső, platform-specifikus leküldéses értesítéseket kezelő szolgáltatása által) jellegét, mert nincs késés garancia nem. Leküldéses értesítések többségét kézbesítve általában néhány percen belül.

###<a name="9---what-are-the-considerations-i-need-to-take-into-account-when-designing-a-solution-with-namespaces-and-notification-hubs"></a>9. Mit dolgot kell a névtér és értesítés hubok megoldás tervezésekor figyelembe kell?

####<a name="mobile-appenvironment"></a>Mobil alkalmazás/környezet

* / / Környezetre mobilalkalmazás egy értesítés központi kell lennie.
* Több elem bérlői esetben minden bérlői egy külön hubhoz kell rendelkeznie.
* Soha nem meg kell osztania ugyanahhoz az értesítési elosztóhoz közötti próba vagy üzemi környezetben, mint ez az sorral lejjebb problémákat okozhatnak értesítés küldése közben. Apple pl. védőfalas, és munkakörnyezeti leküldéses végpontok kínál az összes külön hitelesítő adatok problémákat.
* Alapértelmezés szerint az Azure-portálra vagy az Azure integrált összetevő a Visual Studióban keresztül a bejegyzett eszközök elküldheti a próba-értesítések. A küszöbértéke a regisztrációs készletből véletlenszerűen kiválasztott 10 eszközök.

>[AZURE.NOTE] A központi lett beállítva, hogy az eredetileg az Apple védőfalas tanúsítványt, és majd újrakonfigurálni az Apple gyártási tanúsítványt használja, ha a régi eszköz tokenek, akinek az új tanúsítvánnyal érvénytelenné válnak és okozó veremmutatót sikertelen lesz. Célszerű a termelési külön és környezetekben és más hubok használható különböző környezetekben.

####<a name="pns-credentials"></a>PNS hitelesítő adatok

Ha mobilalkalmazásban regisztrált egy platform developer portal (például az Apple vagy Google stb.) majd kattint az alkalmazás-alkalmazás kódmentes van szüksége ahhoz, hogy a Platform leküldéses értesítést szolgáltatások engedélyezni szeretné a leküldéses értesítéseket küldeni az eszközök, amelyek azonosító és a biztonsági tokenek. Ez lehet tanúsítványok (például az Apple iOS operációs rendszerrel vagy a Windows Phone) vagy a biztonsági is (Google Android, Windows stb.) formájában biztonsági tokenek kell konfigurálni kell értesítés hubok. A szokásos értesítés központi szintre befejeződött, de a több elem bérlői helyzetben névtér szinten is elvégezhető.

####<a name="namespaces"></a>Névtér

Névtér használható a csoportosításhoz telepítési.  Is használható, amely minden értesítést hubok a több elem bérlői esetben az azonos alkalmazás az összes bérlők jelképezi.

####<a name="geo-distribution"></a>GEO-eloszlás

GEO-eloszlás kifolyólag sem kritikus leküldéses értesítést helyzetekben. Megjegyzendő, hogy a különböző leküldéses értesítést szolgáltatások (például APN, GCM stb.), végül a leküldéses értesítések kézbesítése eszközök, amelyek nem egyenletesen elosztott vagy.

Ha egy alkalmazást, amely világszerte használatos, kihasználva körül a földgömb különböző Azure régiókban az értesítési hubok szolgáltatás elérhető különböző névtér több hubok is létrehozhat.

>[AZURE.NOTE] A felügyeleti költség - különösen körül regisztrációk, ez megnöveli, ez valójában nem javasolt, és csak kell elvégezni, ha közvetlen szükség van.

###<a name="10--should-i-do-registrations-from-the-app-backend-or-directly-through-client-devices"></a>10. Teendő regisztrációk az alkalmazás kódmentes vagy közvetlenül az ügyfél eszközökön?
Az alkalmazás kódmentes a regisztrációk hasznosak, végezze el a regisztráció létrehozása előtt ügyfél-hitelesítés esetén, vagy telepítette, címkék, amely kell létrehozni vagy módosította az alkalmazás kódmentes néhány alkalmazás logika alapján. További tudnivalók erről további lapok [Kódmentes regisztrációs útmutató – a 2] és a [Kódmentes regisztrációs útmutatást] .

###<a name="11--what-is-the-push-notification-delivery-security-model"></a>11. Mi az, hogy a leküldéses értesítést kézbesítési biztonsági modell?
Azure értesítés hubok használni egy [Megosztott Access-aláírás (Társítások)](../storage/storage-dotnet-shared-access-signature-part-1.md)-alapú biztonsági modell. A Társítások tokenek használhatja a gyökérszinten névtér vagy értesítés hubok alapszinten. Társítások tokenek kell engedélyeket állíthat be különböző engedélyezési szabályokkal pl. küld üzenetet, meghallgatása értesítés engedélyek stb. További részleteket a [NH biztonsági modell] dokumentumban érhetők el.

###<a name="12--how-should-i-handle-sensitive-payload-in-the-push-notifications"></a>12. hogyan kezelje bizalmas tartalom a leküldéses értesítések
Az értesítések érkeznek cél eszközök által a platform leküldéses értesítést szolgáltatások (PNS). Ha a feladó értesítést küld az Azure értesítés hubok azt feldolgozása, majd az értesítés átadni a megfelelő PNS.

Minden kapcsolat a feladó Azure értesítések hubokon, majd a PNS HTTPS használja.

>[AZURE.NOTE] Azure értesítések hubok semmilyen módon nem rögzít a tartalom, az üzenet.

Küldés bizalmas tartalmak számára azonban azt javasoljuk, hogy a biztonságos leküldéses mintát a hol a feladó biztosítja az eszközre, anélkül, hogy a bizalmas tartalom, és amikor az alkalmazás az eszközön megkapja a tartalom üzenet azonosítójú ping"értesítés, célszerű fel tudja hívni egy biztonságos API-t közvetlenül a részletei lehívása. A fenti minta megvalósításáról útmutató [NH - biztonságos leküldéses oktatóprogram] lapon érhető el.

##<a name="operations"></a>Műveletek
###<a name="1---what-is-the-disaster-recovery-dr-story"></a>1. a Mi az, hogy a katasztrófa helyreállítási (DR) szövegegység?
Elvégezheti a szükséges metaadatok vészhelyreállítás követés a végén (értesítési központi nevét, kapcsolati karakterlánc és egyéb fontos információkat). Példa az DR elindításakor regisztrációk adatai az értesítési hubok infrastruktúra elvesznek, amely **csak a szakasz** . Szüksége lesz az új központi utáni helyreállítás be az adatok újbóli feltöltéséhez megoldást végrehajtásához.

- *1. lépés* – hozzon létre egy másodlagos értesítés hubon keresztül csatlakozott egy másik adatközponthoz. Létrehozhat a menet közben a DR esemény idején vagy hozhat létre egyet az első lépjen. Nem létrehozni, akkor a különbség az nagy lehetőséget választja, mivel a értesítés központi kiépítési sorrend néhány másodpercet viszonylag gyors folyamat. Több az elejétől szolgálja, a DR rendezvény az adatkezelési képességeit érintő, így a nagyon ajánlott lehetőséget.

- *2.lépésben* - hidrát a másodlagos értesítés központi együtt a regisztrációk, az elsődleges értesítés-központban. Próbálja meg mindkét hubok regisztrációk kezelése és szinkronizálására őket menet regisztrációk származnak-általában, amely még nem dolgozott jól miatt belső tendencia figyelhető regisztrációk lejár PNS oldalán, próbálja meg nem javasolt. Értesítés hubok eltávolítással érkezése lejárt, vagy érvénytelen regisztrációk PNS visszajelzést.  

Javaslat, melyik vagy egy alkalmazás kódmentes használandó:

- Megtartja az egy adott sor végén regisztrációk, hogy a másodlagos értesítés elosztóhoz abban az esetben, ha DR teheti a tömeges beszúrása

**VAGY**

- Lekérdezi egy szokásos kiírása másolatként elsődleges központból regisztrációk, és nem a tömeges illessze be a másodlagos NH.

>[AZURE.NOTE] Regisztráció exportálási/importálási elérhető szabványos réteg funkcióról a [Regisztrációk exportálási/importálási] dokumentumban.

Ha még nincs telepítve a kódmentes, majd az alkalmazás bármely cél eszközök indításakor, azok elvégez egy új bejegyzés a másodlagos értesítés-központban, és végül a másodlagos értesítés központi fog rendelkezni az aktív eszközök regisztrált.

Mi a hátrányuk, hogy az egy adott időszakra, amikor eszközök, ahol alkalmazások nem nyitotta meg a be nem kapja meg a értesítések lesz.

###<a name="2---is-there-any-audit-log-capability"></a>2. a létezik: bármilyen ellenőrzési napló lehetőségek?
Az összes értesítés hubok adatkezelési műveletek az [Azure klasszikus portál]elérhető művelet naplókat megnyitásához.

##<a name="monitoring--troubleshooting"></a>Figyelés és hibaelhárítás
###<a name="1---what-troubleshooting-capabilities-are-available"></a>1 milyen hibaelhárítási funkciók érhetők el?
Azure értesítés hubok végezze el a gyakori hibáinak elhárítása felhasználó, különösen a kihagyott értesítéseket körül a leggyakoribb helyzet számos szolgáltatást biztosít. Részletesebben a [NH – hibaelhárítás] szeretne.

###<a name="2---what-telemetry-features-are-available"></a>2. milyen telemetriai funkciók érhetők el?
Azure értesítés hubok lehetővé teszi, hogy telemetriai adatainak megtekintése a [Klasszikus Azure-portálon]. A rendelkezésre álló mérési módja miatt részleteit [NH - mértékek] lapon érhetők el.

>[AZURE.NOTE] A sikeres értesítések csak azt jelenti, hogy a leküldéses értesítések lesz küldve a külső leküldéses értesítéseket kezelő szolgáltatás (például az Apple APNS, GCM a Google stb.). A PNS az értesítés végzi a cél eszközök esetén. A PNS általában nem jelenítik kézbesítési mértékek harmadik személyeknek.  

Azt is biztosít a exportálja a telemetriai adatokat programozás útján (a **szabványos** réteg) lehetőséget. Lásd: További információ a [NH - mértékek minta] .

[Azure klasszikus portál]: https://manage.windowsazure.com
[Értesítés hubok árak]: http://azure.microsoft.com/pricing/details/notification-hubs/
[Értesítés hubok SLA]: http://azure.microsoft.com/support/legal/sla/
[CaseStudy - Sochi]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=7942
[CaseStudy - Skanska]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5847
[CaseStudy - Seattle időpontok]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=8354
[CaseStudy - Mural.ly]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=11592
[CaseStudy - 7Digital]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=3684
[NH - REST API-hoz]: https://msdn.microsoft.com/library/azure/dn530746.aspx
[NH – az első lépéseket ismertető oktatóanyagok]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[A Chrome-alkalmazások oktatóprogram]: http://azure.microsoft.com/documentation/articles/notification-hubs-chrome-get-started/
[Mobile Services Pricing]: http://azure.microsoft.com/pricing/details/mobile-services/
[Regisztráció Kódmentes útmutató]: https://msdn.microsoft.com/library/azure/dn743807.aspx
[Regisztráció Kódmentes útmutatást - 2]: https://msdn.microsoft.com/library/azure/dn530747.aspx
[NH biztonsági modell]: https://msdn.microsoft.com/library/azure/dn495373.aspx
[NH - biztonságos leküldéses oktatóprogram]: http://azure.microsoft.com/documentation/articles/notification-hubs-aspnet-backend-ios-secure-push/
[NH – hibaelhárítás]: http://azure.microsoft.com/documentation/articles/notification-hubs-diagnosing/
[NH - mértékek]: https://msdn.microsoft.com/library/dn458822.aspx
[NH - mértékek minta]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel
[Exportálási/importálási regisztrációk]: https://msdn.microsoft.com/library/dn790624.aspx
[Azure Portal]: https://portal.azure.com
[teljes minták]: https://github.com/Azure/azure-notificationhubs-samples
[Azure mobilalkalmazások]: https://azure.microsoft.com/en-us/services/app-service/mobile/
[Árak alkalmazás szolgáltatás]: https://azure.microsoft.com/en-us/pricing/details/app-service/

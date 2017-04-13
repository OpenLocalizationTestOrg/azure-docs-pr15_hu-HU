<properties 
   pageTitle="Azure mobil tetszés szerint elmélyedhet hibaelhárítási útmutatók" 
   description="Útmutató a Microsoft Azure mobil tetszés szerint elmélyedhet – hibaelhárítás" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="azure-mobile-engagement---troubleshooting-guide"></a>Azure mobil tetszés szerint elmélyedhet - hibaelhárítási útmutatójának

## <a name="introduction"></a>– Bevezetés
Az alábbi hibaelhárítási útmutatója segít megérteni a néhány példa a gyakori problémák és fog okát lehetővé teszi a saját kapcsolatos hibák elhárítása. 

## <a name="general"></a>Általános

Az általános mindig bizonyosodjon meg az alábbiakat:

1. Győződjön meg arról, hogy telt-integráció a az [első lépések oktatóanyagok](mobile-engagement-windows-store-dotnet-get-started.md) ismertetett módon szükséges összes lépéseken
2. A Platform SDK a legújabb verzióját használja. 
3. Tesztelje tényleges eszköz és egy irányító, mert csak irányító-specifikus problémák elhárításához. 
4. Vannak nem szerezze a bármely korlátai/szabályozás Mobile tetszés szerint elmélyedhet a ismertetett [Itt](../azure-subscription-service-limits.md)
5. Ha nem tud csatlakozni a Mobile tetszés szerint elmélyedhet szolgáltatás kódmentes vagy megnéz adatok nem betöltésük folyamatosan majd győződni arról, hogy nincsenek-e nincs folyamatban lévő szolgáltatási incidenssel, jelölje be a [következő](https://azure.microsoft.com/status/)

## <a name="monitor-issues"></a>"Képernyő" kapcsolatos problémák

### <a name="i-am-not-seeing-my-device-showing-up-on-the-monitor-tab"></a>Nem látható a videoeszközöm jelenik meg a Monitor lapján
Monitor lapján az a Mobile tetszés szerint elmélyedhet platform valós idejű csatlakoztatott eszközök jeleníti meg. Ha egy irányító és eszköz hibakeresés, majd meg kell jelennie itt legalább egy munkamenetet. Ha az alkalmazás osztottak ki, majd megjelenik az aktív munkamenet űrszelvény az eszközök, amelyek a valós idejű platform csatlakoztatott megfelelően. 

Ha nem láthatók az eszköz Monitor lapján, akkor valószínűleg SDK integrációs problémát. Néhány gyakori kapcsolatos hibák elhárításának lépései a következők:

1. Győződjön meg arról, hogy a megfelelő kapcsolati karakterláncot a mobilalkalmazásban használ, és a SDK kulcsok és nem API billentyűk szakaszát származik. A kapcsolati karakterlánc a mobilalkalmazásban csatlakozik a Mobile tetszés szerint elmélyedhet alkalmazást, amelyben látni fogja az eszköz Monitor lapján példányát. 
2. A Windows platformon – Ha a lap felülírja a `OnNavigatedTo` módszer, győződjön meg arról, hogy hívja fel `base.OnNavigatedTo(e)`.
3. Ha egy meglévő mobilalkalmazás vannak integrálása Mobile tetszés szerint elmélyedhet, majd is biztosítható, hogy nem hiányzik a lépéseket a fejlett integrációs lépéseket megjeleníti [az alábbi](mobile-engagement-windows-store-integrate-engagement.md)
4. Gondoskodjon arról, hogy legalább egy képernyőn/tevékenység szerint felülbírálása az oldal EngagementActivity attól függően, hogy a platform, ahogy az [első lépések oktatóanyagok](mobile-engagement-windows-store-dotnet-get-started.md)dolgozik küldi.

### <a name="i-am-seeing-the-monitor-tab-showing-a-session-even-when-i-have-disconnected-or-closed-my-app-emulator"></a>Jelentek meg a munkamenet megjelenítő Monitor lapján is mikor van kapcsolata, vagy bezárnak saját alkalmazás / irányító. 
Ha jelenleg csak az egyiket, ezen a ponton a platform csatlakozik, és egy irányító esetén az alkalmazás megnyitásához kattintson az irányító régi miatt valószínűleg. Az általános kell győződjön meg arról, hogy, térjen vissza az a sikeres leválasztja az alkalmazás munkamenet irányító a kezdőképernyőn. Emellett a Windows platformon a Visual Studióban, hibakeresése során szükség lehet meggyőződni arról, hogy a Visual Studióban, akkor nyissa meg a **Életciklus események** menüsor és **felfüggesztett** a munkamenet valójában bezárásához kattintson. Lásd: [Windows oktatóanyag](mobile-engagement-windows-store-dotnet-get-started.md) további információt. 

## <a name="analytics-issues"></a>"Elemzés" kapcsolatos problémák

### <a name="i-am-not-seeing-any-data-refreshed-data-on-analytics-tab"></a>E nem látható minden adat / elemzés lap adatok frissítése 
Analytics adatainak rendszeresen frissül, és is eltelhet importálhat e frissítési 24 óra. Az adatok nem valós idejű, és látni fogja azt frissítése a 24 órás időszak belül.
Ellenőrizze, hogy jó helyen jár, hogy küld legalább egy képernyővel vagy a tevékenységhez a platform kódmentes vagy elsőrendű legalább egy oldalra a `EngagementActivity` vagy hívása `SendActivity` explcitly. 

### <a name="i-am-seeing-incorrectly-captured-datetime-for-a-device-on-the-analytics-tab"></a>Az eszköz nem megfelelően rögzített dátum/idő jelentek meg az elemzés lapon
Az időszak elemzéséhez a dátumot a felhasználók eszközbeállítások alapul. Így győződjön meg arról, hogy az eszköz a dátumot, helyesen beállítva. 

## <a name="segment-issues"></a>A szakasz"kapcsolatos problémák

### <a name="i-created-a-segment-and-it-is-showing-up-as-greyed-out-or-not-showing-any-data"></a>Egy szakasz létrehozott és jelenik meg, mint parancs szürkén jelenik meg, vagy nem látható minden adat
A szakasz létrehozási pillanatában valós idejű nem. A analytics adatainak program összesíti, és úgy is eltelhet importálhat 24 óra számítható egy időben. Nézze meg újra később, de eközben gondoskodnia kell arról is, hogy a mobilalkalmazások valóban elküldeni az adatok alapján, amely a vonalszakaszokon végzett módosítások úgy vannak. Pl. Ha nem küldi el "élőlá" bármilyen mobileszközön szerint, majd egy szakasz EventName készült szakasz adatokat értelemszerűen nem látja az esemény = élőlá a feltételként. Azt is ellenőrizze a SDK integrációs ahhoz, hogy a mobilalkalmazásban megfelelően az adatokat küldi. 

## <a name="reach-or-push-notifications-issues"></a>Elérjen"vagy a leküldéses értesítések kapcsolatos problémák

### <a name="my-push-messages-are-not-being-delivered"></a>A leküldéses során nem érkeznek alatt 

1. Próbálja meg értesítést küld a próba először annak érdekében, hogy minden összetevő - mobilalkalmazás, SDK és a szolgáltatás megfelelően csatlakoztatott és előadása a leküldéses értesítések képes eszköz. 
2. Mindig a legegyszerűbb "ki az alkalmazást az értesítés küldése" először egy marketingkampány nem ütemezett keresztül, és minden megadott közönség feltétel van, sem. Az ismét annak szemléltetéséhez, hogy értesítés kapcsolat rendben működik. 
3. Problémák vannak az alkalmazás az előadásához majd is érdemes egy jó első lépés a teendő, először a egy ki az alkalmazást az értesítés küldése. 
4. Győződjön meg arról, hogy a "natív leküldéses" helyesen van beállítva a mobilalkalmazásban. Attól függően, hogy a platform, vagy magában foglalja a hívóbetűk (Android, Windows) vagy a tanúsítványok (iOS). Lásd: a [felhasználói felület - beállítások](mobile-engagement-user-interface-settings.md)
5. Ki letölthető értesítések sikerült is blokkolhatja a mobil operációs rendszer keresztül a felhasználó által, győződjön meg róla, ez nem áll fenn. 
6. Győződjön meg arról, hogy nem állít be a *közönség figyelmen kívül hagyása leküldéses küld az API-t a felhasználók* lehetőséget a gyermekektől marketingkampány **marketingkampány** szakaszában, mert ezzel biztosíthatja, hogy leküldéses értesítéseket sikerült csak elküldeni API-khoz keresztül. 
7. Győződjön meg arról, hogy a két eszközön kattintva kiküszöbölheti a hálózati kapcsolat problémákat lehetséges adatforrásként WiFi-csatlakozás és a telefon operátor hálózaton keresztül csatlakozik a leküldéses marketingkampány tesztelése.
8. Győződjön meg róla, hogy a rendszer dátum/idő eszköz/irányító a helyes mivel minden olyan szinkronban eszköz fog is zavarják a leküldéses értesítéseket kezelő szolgáltatása által értesítések előadásához. 

További platform konkrét hibaelhárítási lépéseket:

1. **iOS** 

    - Győződjön meg arról, hogy a tanúsítványok is érvényes, és az iOS leküldéses értesítések le nem járt. 
    - Győződjön meg arról, hogy helyesen állítja be egy *gyártási* tanúsítványt az részvételét Mobile alkalmazásban. 
    - Győződjön meg arról, hogy tesztelje a egy *valós, fizikai eszközök.* Leküldéses üzenetek nem lehet feldolgozni azokat a iOS simulatort használja.
    - Győződjön meg arról, hogy az első lépésekhez azonosító helyesen van beállítva a mobilalkalmazásban. Olvassa el a útmutatót [Itt](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)
    - Tesztelésekor, használja a "Alkalmi" terjesztési a mobil kiépítési profilhoz. Nem tudja értesítést kap, ha az alkalmazás összeállítása után "Hibakeresési" használatával

2. **Android-alapú**

    - Győződjön meg arról, hogy a projekt megfelelő számot megadott \n karakter követi a mobilalkalmazásban AndroidManifest.xml fájlt. 
    
            <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
        
    - Győződjön meg arról, hogy nem hiányoznak, vagy hibásan beállított engedélyeket az Android cikkét fájlban 
    - Ellenőrizze, hogy a projekt számot vesz fel az ügyfél-alkalmazás ugyanazzal a fiókkal a hol szerezte be a GCM kiszolgáló billentyűt. Bármely a kettő közötti eltéréseket megakadályozza, hogy a veremmutatót nyissa. 
    - Ha kap rendszerüzenetek, de nem az alkalmazás és véleményezés az [Adja meg az értesítések szakaszban ikonjára](mobile-engagement-android-get-started.md) , valószínűleg nem megadása esetén a megfelelő ikon az Android cikkét fájlban. 
    - Ha BigPicture értesítést küld, és biztosítani, hogy ha a külső kép-kiszolgálóval rendelkezik, majd szükségük van engedélyezni szeretné a támogatási HTTP "Beolvasása" és "Címsor".

3. **A Windows**
    
    - Győződjön meg arról, hogy érvényes Windows áruház-alkalmazás és az alkalmazás társított. Visual Studio - fog rendelkezni, kattintson a jobb gombbal a projekt és "Társíthatja az Alkalmazásáruház" beállítást, és jelölje ki az alkalmazást a Windows áruházból létrehozott. A Windows áruházból letölthető kell lennie a hol szerezte be a natív leküldéses hitelesítő adatait, és állítsa be a mobil tetszés szerint elmélyedhet portálon ugyanarra a számítógépre.
    - Ha ki az alkalmazást a leküldéses értesítéseket, de nem az-alkalmazásértesítések az Igen `EngagementOverlay` integration majd győződjön meg róla, a legfelső szintű rács elem van a lapon. Az első "Rács" elem a xaml-fájl hozzáadása a két webes szerepel a lapon a vászonra EngagementOverlay használja. Ha azt szeretné, keresse meg, ahol web nézetek állítja be, a rács "EngagementGrid" nevű, mint ez lesz, de annak érdekében, hogy van elegendő magasságát és szélességét, a két további webes nézetek, amely megjelenik az értesítés és az alkalmazás értesítést, a lenti állíthatók be:
        
            <Grid x:Name="EngagementGrid"></Grid>

### <a name="i-created-a-push-notificationannouncement-campaign-and-even-after-it-sent-me-the-notification-it-is-showing-as-active-what-does-it-mean"></a>Én hoztam létre a leküldéses értesítést/hirdetmények/kampány, és akkor küldi el, az értesítés, után "aktívként megjelenítő. Mit jelent ez? 
A mobil tetszés szerint elmélyedhet a létrehozott **marketingkampány** neve, mert időigényes leküldéses értesítést jelentését foglalja össze, új eszközök kapcsolódni a mobil tetszés szerint elmélyedhet platform, azok automatikusan küld az értesítés, ebben az esetben célszerű konfigurálni mindaddig, amíg a kritérium beállítja a marketingkampány megfelelnek. Ez a nem egy fényképezés egyetlen értesítés beállítása. Be kell manuálisan kattintson a **Befejezés** gombra, hogy nem további értesítés küldése a marketingkampány befejezéséhez. 

### <a name="i-created-a-push-campaign-and-i-am-receiving-notifications-successfully-however-whenever-i-open-up-the-app-i-get-the-same-notification-even-when-i-had-actioned-it-before"></a>Én hoztam létre, hogy a leküldéses marketingkampány és érkeznek értesítések sikeresen azonban be az alkalmazás megnyitásakor szerezhető be az azonos értesítést, ha a korábban actioned azt megelőzően? 
Valószínű, hogy a tesztelés során fordulhat elő, és emulátor vagy egy esetén például TestFlight keretrendszer tesztelése Mi történik, a minden alkalmazás példány futtatása, hogy egy új DeviceID beszerzése és és a kívánt Mobile tetszés szerint elmélyedhet platform tekinti és értesítést küld új eszköz okozó kódmentes küldi el az alábbiakban. 

## <a name="getting-support"></a>Az első támogatás

Ha nem tudja a probléma megoldásához saját maga akkor is:

1. Keresse meg a problémát a meglévő szálak StackOverflow fórum és [fórum az MSDN webhelyen](https://social.msdn.microsoft.com/Forums/windows/en-US/home?forum=azuremobileengagement) található, és nem majd Ha kérdése van. 
2. Ha megtalálta a hiányzó majd hozzáadása/szavazzon a kérelmet, a [UserVoice fórum](https://feedback.azure.com/forums/285737-mobile-engagement/) funkció
3. Ha a Microsoft támogatási nyissa meg a támogatási kérelmeiket, mert az alábbiakat: 
    - Azure előfizetés azonosítója
    - Platform (pl. iOS, Android stb.)
    - Alkalmazás azonosítója
    - Marketingkampány-azonosító (a leküldéses értesítést problémák)
    - Eszköz azonosítója
    - Mobil tetszés szerint elmélyedhet SDK verzióját (például Android SDK v2.1.0)
    - A hibaüzenet pontos és forgatókönyv részletek

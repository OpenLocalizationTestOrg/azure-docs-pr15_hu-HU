<properties 
   pageTitle="Felhasználók tájékoztatását érzékelők vagy más rendszerekből kapott adatok |} Microsoft Azure"
   description="Esemény hubok segítségével a felhasználók tájékoztatását szenzoradatokat ismerteti."
   services="event-hubs"
   documentationCenter="na"
   authors="spyrossak"
   manager="timlt"
   editor="" />
<tags 
   ms.service="event-hubs"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="spyros;sethm" />

# <a name="notify-users-of-data-received-from-sensors-or-other-systems"></a>Felhasználók tájékoztatását érzékelők vagy más rendszerekből kapott adatok

Tegyük fel, hogy telepítve van az alkalmazás figyeli valós idejű adatok, vagy a jelentéseket készít. Ha az adott valós idejű diagramok és a jelentések megjelennek webhelyén keresse meg, amit teendőket igényel jelenhet meg. Mi történik, ha kell lennie ezeknek a lehetőségeknek figyelmeztetés létrejött, ahelyett kattintson a webhely ellenőrzése megjegyzése? Tegyük fel, hogy a megnövesztés fény üvegházhatású a van, és azonnal tudnivalók, ha egyszerűsített mutat-e. Egyik módja a fényérzékelő a üvegházi, az e-mailt küldeni, ha ki kapcsolva a világos rendezése lenne.

![][1]

Egy másik esetben tegyük fel, hogy kedvtelésből kikötéshez létesítmény futtat, és meg kell értesítési szintjének beállításához alacsony készlet ellátási szintek. Például lehet rendezni küldhetők szöveges üzenet a ERP rendszerből, ha a raktári leltár kutyát élelmiszer csökkent kritikus szintre. 

![][2]

A probléma, hogy hogyan kritikus információk bizonyos feltételek teljesülése, nem, ha navigálhat az kivétele statikus jelentés. Eszközök vagy a vállalati alkalmazások, például a [Dynamics AX][]adatokat fogadjon az [Azure esemény központi][] vagy [Azure IoT központi][] szolgáltatást használ, esetén több lehetőség közül feldolgozása őket. Egy olyan webhelyre, megtekintheti, elemezheti őket, is tárolhatja és indíthatja el szeretne elhelyezni parancsokat használhatja őket. Ehhez a hatékony eszközöket, például [Azure webhelyek][], a [SQL Azure][], a [HDInsight][], a [Cortana üzletiintelligencia-csomagot][], a [IoT csomagja][], a [Logika alkalmazások][]vagy a [Azure értesítés hubok][]is használhatja. De néha összes műveletek is elküldheti adatokat terhelést legalább. Megtudhatja, hogy miként csak egy apró kód megtennie, az új minta [AppToNotifyUsers][]biztosítunk Önnek. Beépített lehetőségek (SMTP) e-mail, SMS és telefonszámát.

## <a name="application-structure"></a>Alkalmazás-struktúra

Az alkalmazás írott C#, és a minta fontos fájl tartalmaz módosítása, összeállítása és az alkalmazás közzététele szükséges összes információ. A következő szakaszokban a Mit jelent az alkalmazás magas szintű áttekintése.

Azon a feltételezésen, hogy éppen tolni Azure esemény központi vagy IoT központi fontos események kezdje azt. Minden olyan központi hajt végre, feltéve férhetnek hozzá, és meg, hogy a kapcsolati karakterlánc.

Ha még nem rendelkezik egy esemény központi vagy IoT hubhoz, egyszerűen beállíthatja a próba beágyazása egy Arduino védelem és egy málna Pi, a [Csatlakozás a pontok](https://github.com/Azure/connectthedots) projekt utasításait követve. A fényérzékelő a Arduino védelem a az egyszerűsített szinteket a Pi keresztül küld egy [Azure esemény központi][] (**ehdevices**), és egy [Azure Értékáram-elemzés](https://azure.microsoft.com/services/stream-analytics/) feladat verembe küldi riasztások egy második esemény hubhoz (**ehalerts**), ha egy bizonyos szintre: csökkenhet a kapott egyszerűsített szintek.

**AppToNotify** indításakor konfigurációs fájl (App.config) az URL-CÍMEK és a hitelesítő adatok beszerzése az esemény-központban, az értesítéseket olvassa be. Majd egy folyamat folyamatosan ellenőrzésére, hogy az esemény központi mindaddig, amíg van keresztül – érkező üzenetek férhet hozzá az esemény központi vagy IoT központi és érvényes hitelesítő adatokkal URL-CÍMÉT, esemény hubok olvasó kód folyamatosan olvassa milyen újdonságokat indít. Indításkor az alkalmazás is olvassa be az URL-CÍMEK és a használni kívánt üzenetküldési szolgáltatás (e-mail, SMS-telefon), és a feladó neve és címe és a címzettek listájának hitelesítő adatait.

Az esemény központi monitor azt észleli, hogy egy üzenetet, miután egy folyamat által küldött, ezt az üzenetet a konfigurációs fájl a megadott módszer akkor indítja. Figyelje meg, hogy minden érzékeli üzenetet küld. Ha beállíthatja, mutasson a monitor esemény-hubon keresztül csatlakozott, amely fogadja másodpercenként tíz üzeneteket, és a feladó tíz e-mailek másodpercenként tíz SMS-üzenetek másodpercenként másodpercenként tíz telefonhívások másodpercenként – tíz üzeneteket küld. Éppen ezért győződjön meg arról, hogy figyelheti egy esemény hubhoz, amely csak a küldhető el, nem az összes nyers adatokból fogad a érzékelők vagy alkalmazások esemény központi kell értesítések kapja.

## <a name="applicability"></a>Alkalmazási terület

Ez a példa a kód csak az esemény hubok figyelése és hogyan hívja fel a külső üzenetkezelés szolgáltatások abban az esetben, ha meg szeretné adni ezt a funkciót az alkalmazás jeleníti meg. Ne feledje, hogy a megoldás egy DIY, csak a fejlesztői fókuszáló példa. Vállalati követelmények azt nem foglalkozik, például a redundancia, fail fölé, indítsa újra, hiba stb. További teljes és a gyártási megoldások talál a következő:

- A összekötők vagy a leküldéses értesítéseket, az [Azure logika alkalmazások](../app-service-logic/app-service-logic-connectors-list.md) szolgáltatással.
- A blog [adás leküldéses értesítéseket, több millió mobileszközök használatával Azure értesítés hubok](http://weblogs.asp.net/scottgu/broadcast-push-notifications-to-millions-of-mobile-devices-using-windows-azure-notification-hubs)leírtak [Azure értesítés hubok](https://msdn.microsoft.com/library/azure/jj927170.aspx)használata 

## <a name="next-steps"></a>Következő lépések

Egyszerű hozhat létre egy egyszerű értesítési szolgáltatás, amely e-mailben vagy SMS-EK küld a címzetteknek, illetve meghívja őket, megkapta az esemény központi vagy IoT központi továbbítási adatokhoz. Az alábbi hubok megkapta adatok alapján felhasználók értesítése megoldást üzembe helyez, látogasson el a [AppToNotifyUsers][].

További információt a következő hubok az alábbi cikkekben talál:

- [Azure esemény hubok]
- [Azure IoT központi]
- Első lépések az [esemény hubok oktatóprogram].
- Egy teljes [minta alkalmazást használó esemény hubok].

Az alábbi hubok megkapta adatokon alapuló felhasználók értesítése megoldást üzembe helyez, látogasson el:

- [AppToNotifyUsers][]

[Esemény hubok oktatóprogram]: event-hubs-csharp-ephcs-getstarted.md
[Azure IoT központi]: https://azure.microsoft.com/services/iot-hub/
[Azure esemény hubok]: https://azure.microsoft.com/services/event-hubs/
[Azure esemény központi]: https://azure.microsoft.com/services/event-hubs/
[Esemény hubok használó minta alkalmazás]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[AppToNotifyUsers]: https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications
[Dynamics AX ALKALMAZÁSHOZ]: http://www.microsoft.com/dynamics/erp-ax-overview.aspx
[Azure webhelyek]: https://azure.microsoft.com/services/app-service/web/
[Az SQL Azure]: https://azure.microsoft.com/services/sql-database/
[Hdinsight szolgáltatáshoz]: https://azure.microsoft.com/services/hdinsight/
[Üzletiintelligencia-csomagja Cortana]: http://www.microsoft.com/server-cloud/cortana-analytics-suite/Overview.aspx?WT.srch=1&WT.mc_ID=SEM_lLFwOJm3&bknode=BlueKai
[IoT programcsomagban]: https://azure.microsoft.com/solutions/iot-suite/
[Logika alkalmazások]: https://azure.microsoft.com/services/app-service/logic/
[Azure értesítés hubok]: https://azure.microsoft.com/services/notification-hubs/
[Azure Stream Analytics]: https://azure.microsoft.com/services/stream-analytics/
 
[1]: ./media/event-hubs-sensors-notify-users/event-hubs-sensor-alert.png
[2]: ./media/event-hubs-sensors-notify-users/event-hubs-erp-alert.png
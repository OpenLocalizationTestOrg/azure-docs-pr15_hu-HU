<properties
    pageTitle="Rövid útmutató: gépi tanulási javaslatok API |} Microsoft Azure"
    description="Azure gépi tanulási javaslatok – rövid útmutató"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="luisca"/>

# <a name="quick-start-guide-for-the-cognitive-services-recommendations-api"></a>Rövid útmutató az kognitív szolgáltatások javaslatok API

A dokumentum ismerteti, hogyan alkalmazhat beépített a szolgáltatásra vagy az alkalmazás használatához a [Javaslatok API](http://go.microsoft.com/fwlink/?LinkId=759710).
A javaslatok API- és egyéb kognitív szolgáltatások további tájékoztatást talál [Itt](http://go.microsoft.com/fwlink/?LinkId=759709). Az útmutatóban is találhat a [Javaslatok API hivatkozás](http://go.microsoft.com/fwlink/?LinkId=759348) hasznos.


<a name="Overview"></a>
## <a name="general-overview"></a>Általános áttekintés

A dokumentum részletes útmutatásra. A célja, hogy végigvezetik Önt a láthatja el ismeretekkel a modell, és mutasson az erőforrásokat, amely lehetővé teszi, hogy a modell előnyeit az munkakörnyezetben felhasználni a szükséges lépéseket.

A gyakorlat körülbelül 30 percig tart.

[Javaslatok API](http://go.microsoft.com/fwlink/?LinkId=759710)használatához kell kövesse az alábbi lépéseket:

1. Hozzon létre egy a modellben - a modell a használati adatok, katalógusadatok és az ajánlási modell tároló.
1. Katalógus adatok importálása - katalógust a metaadat-alapú információt tartalmaz.
1. Látogatottsági adatok importálása - használati adatainak fel kell tölteni a 2 módszerek valamelyikével (vagy mindkettő):
  -  Által használatát adatokat tartalmazó fájl feltöltésekor.
  -  Adatok WIA események küldi.
  Használatát fájl általában annak érdekében, hogy az eredeti ajánlási modell (betöltő) létrehozása és használata mindaddig, amíg a rendszer az elég adatait az adatformátum WIA használatával gyűjti össze tudja feltöltése.
1. Javaslat modell összeállítása – Ez az ajánlási rendszer szükséges összes használati adatainak és létrehoz egy ajánlási modell aszinkron műveletet. Ez a művelet vehet igénybe néhány percig, amíg vagy több óra az adatok és a Szerkesztés konfigurációs paraméterek méretétől függően. Az összeállítás el, ha jelenik meg egy összeállítás azonosítót. Használni, amikor az összeállítás folyamat javaslatok használhatnak megkezdése előtt véget ért.
1. Javaslatok - Get javaslatok egy adott elemet vagy elemlistán mobilalkalmazásokban

<a name="GetStarted"></a>
### <a name="lets-get-started"></a>Lássunk hozzá!

Épület adatmodell javaslatok indul el. Ezután azt fogja segítenek az eredmények a modell egy e-kereskedelmi webhelyen power ajánlások által generált használatát.

<a name="Ex1Task1"></a>
#### <a name="task-1---signing-up-for-the-recommendations-api"></a>1 – feladat aláíró tagjának a javaslatok API ####

Ebben a feladatban, fog feliratkozhat a javaslatok API szolgáltatásra, és hozzon létre egy javaslatok modell.

1. Nyissa meg az **Azure-portálon** [http://portal.azure.com/](http://portal.azure.com/) , és jelentkezzen be az Azure-fiókjával.

1.  Kattintson a **+ Új**.

1. Válassza az **üzletiintelligencia** -lehetőséget.

1. Jelölje ki a **Csökkent értelmi szolgáltatások API -khoz** terméket.
Ez a termék indítsa el a csökkent értelmi szolgáltatások API-hoz (ARC, szöveg Analytics, számítógép látás stb.) az előfizetés teszi lehetővé. Az aktuális azt a javaslatok API összpontosít.

1. Adja meg a csökkent értelmi szolgáltatások API céloldal javaslatok előfizetéshez tartozó a **fiók nevére** . (A instace: "MyRecommendations"). Ez a név nem kell minden szóközt.

1. **API-típus**jelölje ki a **javaslatok**.

1. A **réteg árak**jelölje ki azt a csomagot. A 10 000 tranzakciók/hónap **ingyenes** réteg kijelölheti. Az ingyenes tervet, hogy jó módszer indítása, amelyet a rendszer az. Miután gyártási lép, azt javasoljuk fontolja meg a kérelem hangerejét, és a terv típusának ennek megfelelően változik. (Megjegyzés: lehet, hogy egyszerre csak egy ingyenes réteg előfizetés)

1. Jelöljön ki egy **Erőforráscsoport**, vagy hozzon létre egy újat, ha nincs telepítve egyik már.

1. Egyéb elemek a létrehozás párbeszédpanelen módosíthatja. Azt kell mutatnia, hogy az erőforrás-szolgáltató ma csak akkor támogatott, az Amerikai Egyesült Államok adatközpontokban.

1. Ha bármelyik kijelölés végzett, kattintson a **Létrehozás**gombra.

1. Várjon néhány percet, az erőforrás telepíthető.
Ha telepítve van, láthatja el a **billentyűk** szakasz a **Beállítások** lap hol megkapja azt egy elsődleges és másodlagos billentyűt az API-t használja.  Másolja a vágólapra az elsődleges kulcsot, mert meg kell azt, az első modell létrehozásakor.

<a name="Ex1Task2"></a>
#### <a name="task-2---did-you-bring-your-data"></a>2 – tevékenység Did hozása az adatok? ####

A javaslatok API ismerkedhet a katalógus és ügyleteit jó termék javaslatok biztosítása érdekében. Ez azt jelenti, hogy azt a termékek (hívása a **katalógus** fájl) és a tranzakciók elég nagy ahhoz, hogy a megkeresése (hívása a **használatát**) fogyasztási érdekes szokások halmazának jó adatainak-hírcsatorna kell.

1. Általában a az alábbi információkat vehet tranzakciók adatbázisban volna lekérdezés.
Arra az esetre, ha nincs telepítve a praktikus őket, hogy módon biztosítva van a mintaadatokat is tartalmazó Microsoft Store tranzakció adatok alapján.

 Az [alábbi](http://aka.ms/RecoSampleData)letöltheti az adatokat. Másolja a vágólapra, és a MsStoreData.Zip kicsomagolása egy mappába a helyi számítógépre.

 > **Megjegyzés:** A minta kódot, amely, töltse le és a tevékenység 3 futtatása már ágyazva rá – ezt a műveletet nem kötelező, a mintaadatokat tartalmaz.  Említett, a feladat további reálisan adatkészletek letöltése, és lehetővé teszi a bemenetben megérteni a javaslatok API jobb be teszi lehetővé.

1.  Most már most tekintsünk át egy a katalógus fájlt. Nyissa meg azt a helyet, ahol másolt adatokat.
 Nyissa meg a katalógus fájlt a **Jegyzettömbben**.

 Megfigyelheti, hogy a katalógus fájl meglehetősen egyszerű-e. A következő formátumban van`<itemid>,<item name>,<product category>`

 >  AAA-04294, 2013 32 vagy 64 OfficeLangPack E34 Online DwnLd Office <br>
 > AAA-04303, OfficeLangPack 2013 32 vagy 64-ET Online DwnLd Office  <br>
 > C9F-00168, KRUSELL Kiruna tükrözése a Nokia-alapú Lumia 635 - teve, a Kellékek Faxfedőlap

 Hogy lehet, hogy a katalógus fájl jóval magasabb szintű, például felveheti a termékek (hívása ezek a *Funkciók elem*) metaadatait azt kell mutatnia. Ekkor megjelennek a [katalógustétel formátuma](http://go.microsoft.com/fwlink/?LinkID=760716) szakaszban további információt a API-hivatkozás az alkalmazáskatalógus formátumú.

1. Vegyük tegye ugyanezt a használati adatok. Megfigyelheti, hogy van-e a használatát dátum formátum `<User Id>,<Item Id>,<Time Stamp>,<Event>`.

  > 00037FFEA61FCA16, 288186200, 2015/08/04T11:02:52, a vásárlást 0003BFFDD4C2148C, 297833400, 2015/08/04T11:02:50, a vásárlást 0003BFFDD4C2118D, 297833300, 2015/08/04T11:02:40, a vásárlást 00030000D16C4237, 297833300, 2015/08/04T11:02:37, a vásárlást 0003BFFDD4C20B63, 297833400, 2015/08/04T11:02:12, a vásárlást 00037FFEC8567FB8, 297833400, 2015/08/04T11:02:04, megvásárlása

Figyelje meg, hogy kötelező legyen-e az első három elemeket. Az esemény típusa nem kötelező. Ebben a témakörben további információt a [kihasználtság formátum](http://go.microsoft.com/fwlink/?LinkID=760712) érdemes.

 > **Hogy mennyi adatot van szüksége?**
 <p>
Valóban valójában függ a használati adatok magát. A rendszer folyamatosan tanul felhasználók vásárolt sokféle elemhez. Néhány buildjeiben, például FBT fontos, hogy mely elemek vásárolt ugyanazt a tranzakciók. (Hívása a *közös előfordulások*). Egy jó a legjobb megoldás, hogy az elemek többségében 20 tranzakciók vagy több, lehet, ha a katalógus 10 000 elem, volna javasoljuk, hogy rendelkezik-e legalább 20 alkalommal adott számú tranzakciók vagy körülbelül 200,000 tranzakciók.
Még egyszer az egy szabály a legjobb megoldás. Szüksége lesz az adatok kísérletezhet.
</p>

<a name="Ex1Task3"></a>
####Tevékenység 3 - javaslatok adatmodell létrehozása####

Most, hogy fiókja van, és már rendelkezik adatokkal, létrehozása az első modell.

Ebben a feladatban akkor fogja használni a minta alkalmazás összeállítása a első modell.

1. Először is ügyeljen arra, a [Javaslatok API hivatkozást](http://go.microsoft.com/fwlink/?LinkId=759348).

1. Töltse le az [Alkalmazást](http://go.microsoft.com/fwlink/?LinkID=759344) egy helyi mappájában.

1. Nyissa meg a Visual Studio a **RecommendationsSample.sln** megoldást, a **C#** mappában található.

1. Nyissa meg a **SampleApp.cs** fájlt. A Megjegyzés, hogy a fájl az alábbi lépéseket:
 + Adatmodell létrehozása
 + Katalógus fájl
 + A fájl használatát
 + Építés elindítani a modellhez
 + Elemek két alapján ajánlást beszerzése
<p></p>

1. A **AccountKey** mező értékét cserélje le a tevékenység 1 billentyűt.

1. Elvégzése a megoldást, és láthatja, hogy hogyan modell jön létre.

1. Próbálja meg lecseréli az eredeti katalógus és használatát, létrehozhat egy új modellt, a Microsoft Store, illetve a címjegyzék javaslatok letöltött fájlokat. Meg kell módosítása, valamint a modell nevét, és az elemek, amelynek a javaslatok kér.

1. A modell létrehozásakor ügyeljen az alábbiakra a **modell azonosítója** lesz szükség szerint a üzemi környezetben javaslatok kérésekor.

>  További tudnivalók a további információ a Szerkesztés típusát, és hogyan kell kiértékelni buildjeiben minőségét [az alábbi](cognitive-services-recommendations-buildtypes.md).

<a name="Ex1Task4"></a>
### <a name="putting-your-model-in-production"></a>A modell illusztráló gyártási! ###

Most, hogy megismeri hozzon létre egy adatmodellt, és felhasználja a javaslatok, következő lépésként mozgatófogantyúit, hogy a webhelyen mobilalkalmazás gyártási vagy integrálhatja a CRM vagy ERP levelezőrendszerét.
Természetesen rendszer ezeket lenne különböző. Mivel a javaslatok API kérnek webes szolgáltatásként, láthatja, egyszerűen hívni bármilyen ezek különböző környezetekben.

**Fontos:**  Ha szeretne egy nyilvános ügyfélprogramból (például a kereskedelmi webhely) megjelenítése a javaslatok, hozzunk létre adja meg a javaslatok a proxykiszolgálón. Ez a fontos, hogy a API használatával nem érhető el a külső (a potenciálisan nem megbízható) szervezetek.

Az alábbiakban néhány tippet hol használhatók javaslatok helyeken:

### <a name="product-page"></a>Termék lap

**Elem javaslatok**
<p>Ha a modell vásárlás adatok lett képzett, az ügyfél fel *, amelyek valószínűleg a forrás elem, amely megvásárolt személyek érdeklődésére számot tartó termékek*lehetővé.</p>
<p>Ha a modellben lévő lett képzett kattintson az adatok, az ügyfél fel *, amelyek valószínűleg a személyek, amely már meglátogatott a forrás elem meglátogatott termékek*lehetővé teszi. Az ilyen típusú modell esetleg hasonló elemeket.</p>

**A gyakran vásárolt közös javaslatok**
<p>A közös Szerkesztés gyakran vásárolt sikerült kell képzett, így abban, hogy olyan elemhalmazokhoz, valószínűleg vásárolhatók meg ez a cikk együtt.</p>

### <a name="check-out-page"></a>Lap kivétele

**Elem javaslatok**
<p>A javaslatok modell is eltelhet, mint a szövegbeviteli elemek listáját. Így bemenetként átadja javaslatok kérjen minden elem sikerült át a kosár.
Ebben az esetben a modell összes eleme a kosár megadott érdeklődésére számot tartó javaslatok nyújt.
</p>

### <a name="landing-page"></a>Kezdőlapja

**Felhasználói javaslatok**
<p>
A javaslatok modell is igénybe vehet, mint a szövegbeviteli a felhasználói azonosítójával.  Ahhoz, hogy személyre szabott javaslatok a felhasználó által megadott használata a tranzakciók, hogy a felhasználó által az előzményeket.
</p>

Nézze meg az [Első elem javaslatok dokumentáció](http://go.microsoft.com/fwlink/?LinkID=760719).

<a name="Ex1Task6"></a>
### <a name="whats-next"></a>Mi az következő?
Gratulálunk, ha a saját, ez távol! Megtudhatja, hogy több ellátogathat [Javaslatok API hivatkozás](http://go.microsoft.com/fwlink/?LinkId=759348) teljes Ha olyan kérdése van, kérdezze meg a kapcsolatfelvételmlapi@microsoft.com

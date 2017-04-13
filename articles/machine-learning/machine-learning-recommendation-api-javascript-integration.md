<properties 
    pageTitle="Gépi tanulási javaslatok: JavaScript-integráció |} Microsoft Azure" 
    description="Azure gépi tanulási javaslatok - integráció a JavaScript - dokumentáció" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="javascript" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/>

# <a name="azure-machine-learning-recommendations---javascript-integration"></a>Azure gépi tanulási javaslatok - JavaScript-integráció

>[AZURE.NOTE]Ez a verzió nem kell a javaslatok kognitív API-szolgáltatás használatbavétele. A javaslatok kognitív szolgáltatást fogja cseréje ezt a szolgáltatást, és az új szolgáltatások vannak készüljön. Új funkciók támogatása, jobban API Explorer, a tisztább API felületre, egységesebb előfizetési és számlázási élmény stb kötegelés például rendelkezik.
> További tudnivalók [az új kognitív szolgáltatás áttelepítése](http://aka.ms/recomigrate)


A dokumentum történő webhelyére JavaScript integrálásáról jelzik. A JavaScript lehetővé teszi a Adatbeszerzési események küldhet, és felhasználja a javaslatok, Miután elkészítette a ajánlási modell. JS keresztül kész összes művelet a kiszolgálóoldali is elvégezhető.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="1-general-overview"></a>1. a általános áttekintés
A webhely integrálása az Azure Machine Learning javaslatok állnak a 2-fázis:

1.  Azure Machine Learning javaslatok az események küldése Ezzel engedélyezi ajánlási modell össze.
2.  A javaslatok mobilalkalmazásokban Miután elkészült a modell, a javaslatok tudja felhasználni. (A kapcsolatos további információk megjelenítéséhez a rövid útmutató a dokumentum nem bemutatják, hogyan hozhat létre egy adatmodellt, olvassa el).


<ins>E fázis</ins>

Az első szakaszban szúr be a html-lapokat egy kis JavaScript-tárban, amely lehetővé teszi, hogy a lapon, a html-lapon az Azure Machine Learning javaslatok servers (keresztül Datamarket) megjelenésükkor események küldeni:

![Drawing1][1]

<ins>II fázis:</ins>

A második fázisban, amikor szeretné a javaslatok megjelenítéséhez a lapon, válasszon az alábbi lehetőségek közül:

1. a kiszolgálón (a lapleképezés hatékonyságának fázis) javaslatok megszerezni Azure Machine Learning javaslatok kiszolgáló hívások (keresztül Datamarket). Az eredmények elemek azonosítója listáját tartalmazza. A kiszolgáló kell a bemutató az elemekre metaadatai (például képek, a leírás) az eredmények és a létrehozott lapra küldése a böngészőben.

![Drawing2][2]

2. a többi, hogy a fázis egy kis JavaScript-fájl használatával egyszerű lista beszerzése az ajánlott elemek. Itt kapott adatai Karcsúbb, mint az első választógombot.

![Drawing3][3]

##<a name="2-prerequisites"></a>2. a vonatkozó követelmények

1. Hozzon létre egy új modell az API-k használata. Nézze meg a rövid útmutató adunk.
2. Kódolása a &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; base64 együtt. (Ez használandó az alapszintű hitelesítés ahhoz, hogy a JS kódot, hívja fel az API-k).


##<a name="3-send-data-acquisition-events-using-javascript"></a>3. a használatáról JavaScript Adatbeszerzési események küldése
Az alábbi lépéseket a küldő események megkönnyítik:

1.  A kód JQuery tár bele. Letöltheti a nuget az alábbi URL-cím.

        http://www.nuget.org/packages/jQuery/1.8.2
2.  Az alábbi URL-címet a javaslatok Java Script diatárban tartalmazza: http://aka.ms/RecoJSLib1

3.  Azure Machine Learning javaslatok tár a megfelelő paramétereket inicializálni.

        <script>
            AzureMLRecommendationsStart("<base64encoding of username:key>",
            "<model_id>");
        </script>

4.  Küldje el a megfelelő eseményt. Részletes rész alatt látható minden típusú események (Példa: kattintson az esemény)

        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") {      
                        AzureMLRecommendationsEvent = [];
                    }
            AzureMLRecommendationsEvent.push({ event: "click", item: "18321116" });
        </script>


###<a name="31-limitations-and-browser-support"></a>3.1. Korlátozások és böngészőtámogatás
Ez a hivatkozás megvalósítás és van megadva. Minden fő böngésző támogatnia kell azt.

###<a name="32-type-of-events"></a>a 3,2. Esemény típusa
5 típusú eseményt, amely támogatja a tár létezik:, ajánlási gombra, kattintson a Hozzáadás gombra szalon kosárhoz, távolítsa el a bevásárlókocsi szalon kezelése és megvásárlása. A bejelentkezési neve felhasználói környezet beállításához használt további esemény van.

####<a name="321-click-event"></a>3.2.1. Kattintson az esemény
Ez az esemény egy felhasználó az elemen gombra kattintva bármikor kell használni. Általában, amikor a felhasználó kattint egy elemet egy új oldal megnyitásakor az elem adataival; Ezen a lapon az esemény megtörténjen.

Paraméterek:
- esemény (karakterlánc, kötelező) – "kattintson"
- az elem egyedi azonosító (karakterlánc, kötelező) - elem
- itemName (karakterlánc, nem kötelező) – az elem neve
- itemDescription (karakterlánc, nem kötelező) – az elem leírása
- annak az elemnek a kategória (karakterlánc, nem kötelező) - itemCategory
        
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "click", item: "3111718" });
        </script>

Vagy az adatok nem kötelező:

        <script>
            if (typeof AzureMLRecommendationsEvent === "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "click", item: "3111718", itemName: "Plane", itemDescription: "It is a big plane", itemCategory: "Aviation"});
        </script>


####<a name="322-recommendation-click-event"></a>3.2.2. Javaslat esemény gombra.
Ez az esemény egy olyan elemen, amely az Azure Machine Learning javaslatok ajánlott elemként érkezett rákattintott felhasználó bármikor kell használni. Általában, amikor a felhasználó kattint egy elemet egy új oldal megnyitásakor az elem adataival; Ezen a lapon az esemény megtörténjen.

Paraméterek:
- esemény (karakterlánc, kötelező) – "recommendationclick"
- az elem egyedi azonosító (karakterlánc, kötelező) - elem
- itemName (karakterlánc, nem kötelező) – az elem neve
- itemDescription (karakterlánc, nem kötelező) – az elem leírása
- annak az elemnek a kategória (karakterlánc, nem kötelező) - itemCategory
- osztja fel a (karakterlánc tömb; nem kötelező) – a mag által generált a ajánlási lekérdezést.
- recoList (karakterlánc tömb; nem kötelező) - kattintott az elemet létrehozó ajánlási kérelem eredményét.
        
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "recommendationclick", item: "18899918" });
        </script>

Vagy az adatok nem kötelező:

        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: eventName, item: "198", itemName: "Plane2", itemDescription: "It is a big plane2", itemCategory: "Default2", seeds: ["Seed1", "Seed2"], recoList: ["199", "198", "197"]               });
        </script>


####<a name="323-add-shopping-cart-event"></a>3.2.3. Vásárlási bevásárlókocsi esemény hozzáadása
Ez az esemény használandó amikor a felhasználó elem hozzáadása a bevásárlókocsiba.
Paraméterek:
* esemény (karakterlánc, kötelező) – "addshopcart"
* az elem egyedi azonosító (karakterlánc, kötelező) - elem
* itemName (karakterlánc, nem kötelező) – az elem neve
* itemDescription (karakterlánc, nem kötelező) – az elem leírása
* annak az elemnek a kategória (karakterlánc, nem kötelező) - itemCategory
        
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "addshopcart", item: "13221118" });
        </script>

####<a name="324-remove-shopping-cart-event"></a>3.2.4. Vásárlás bevásárlókocsi esemény eltávolítása
Ebben az esetben, amikor a felhasználó eltávolítása egy elemet a bevásárlókocsiba kell használni.

Paraméterek:
* esemény (karakterlánc, kötelező) – "removeshopcart"
* az elem egyedi azonosító (karakterlánc, kötelező) - elem
* itemName (karakterlánc, nem kötelező) – az elem neve
* itemDescription (karakterlánc, nem kötelező) – az elem leírása
* annak az elemnek a kategória (karakterlánc, nem kötelező) - itemCategory
        
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "removeshopcart", item: "111118" });
        </script>

####<a name="325-purchase-event"></a>3.2.5. Vásárlás esemény
Ebben az esetben a bevásárlókocsi megvásárlásakor kell használni.

Paraméterek:
* esemény (karakterlánc) – "beszerzési"
* összehasonlításához ([]) - bejegyzést tartó minden elem vásárolt tömb.<br><br>
Térítéses formázása:
    * elem (karakterlánc) – az elem egyedi azonosítója.
    * darab (int vagy karakterláncot) - elemek száma vásárolt.
    * ár (lebegtetéssel vagy karakterláncot) – nem kötelező mező – az elem árát.

Az alábbi példában látható 3 megvásárlása (33, 34, 35) elemek két töltve mezők (elem, darab és ár), és egy (elem 34) ár nélkül.

        <script>
            if ( typeof AzureMLRecommendationsEvent == "undefined"){ AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "purchase", items: [{ item: "33", count: "1", price: "10" }, { item: "34", count: "2" }, { item: "35", count: "1", price: "210" }] });
        </script>

####<a name="326-user-login-event"></a>3.2.6. Felhasználói bejelentkezési esemény
Azure Machine Learning javaslatok esemény tár hoz létre, és a cookie-k használata ugyanabban a böngészőben, a telepítésük események azonosítása érdekében. Annak érdekében, hogy a eredményeinek javítása a modell Azure Machine Learning javaslatok lehetővé teszi, hogy egy felhasználó egyedi azonosítót, amely felülírja a cookie-k használati beállítása.

Ebben az esetben azt a webhelyet a felhasználói bejelentkezés után kell használni.

Paraméterek:
* esemény (karakterlánc) – "userlogin"
* a felhasználó (karakterlánc) – a felhasználó egyedi azonosítója.
        <script>
  Ha (typeof AzureMLRecommendationsEvent == "nem definiált") {AzureMLRecommendationsEvent = [];} AzureMLRecommendationsEvent.push ({esemény: "userlogin", felhasználói: "ABCD10AA"});      </script>

##<a name="4-consume-recommendations-via-javascript"></a>4. a JavaScript keresztül javaslatok felhasználása
A kód, amely az ajánlási fogyasztása váltja ki néhány JavaScript esemény az ügyfél weblapra. Az ajánlási válasz az ajánlott elemek azonosítók, a nevük és a saját minősítés tartalmazza. Célszerű használni ezt a beállítást csak egy javasolt elemek megjelenítése – kiszolgáló egymás integrálásáról végre kell hajtani összetettebb kezelése (például az elem metaadatok hozzáadása).

###<a name="41-consume-recommendations"></a>4.1-es javaslatok felhasználása
Felhasználása kell javaslatok helyezze el a szükséges JavaScript-tárak a lapot, és hívja fel a AzureMLRecommendationsStart. 2-című részben talál.

Egy vagy több elem javaslatok, kell fordulnia nevű módszer használhatnak: AzureMLRecommendationsGetI2IRecommendation.

Paraméterek:
* elemeket (tömb karakterláncok) – egy vagy több elem javaslatok eléréséhez. Ha egy Fbt build beállíthatja, hogy az alábbi csak egy elemet, majd használja fel.
* numberOfResults (int) - szükséges találatok száma.
* includeMetadata (logikai érték, nem kötelező) – Ha "true" értékre van állítva azt jelzi, hogy kell-e a metaadat-alapú mező töltve az eredményben.
* Függvény - kezelő a javaslatok függvény feldolgozása adja vissza. Az adatokat a rendszer tömbjét adja vissza:
    * Elem - elem egyedi azonosító
    * név - elem neve (ha létezik a katalógus)
    * értékelés – ajánlási értékelése
    * metaadat - karakterlánc, amely a metaadatokat az elem

Példa: A következő kódot kér elem "64f6eb0d-947a-4c18-a16c-888da9e228ba" 8 javaslatok (és nem megadásával includeMetadata - implicit módon megjelölésekor, hogy nincsenek metaadatok nem kötelező), azt be egy pufferelési majd ÖSSZEFŰZ az eredményeket.

        <script>
            var reco = AzureMLRecommendationsGetI2IRecommendation(["64f6eb0d-947a-4c18-a16c-888da9e228ba"], 8, false, function (reco) {
                var buff = "";
                for (var ii = 0; ii < reco.length; ii++) {
                    buff += reco[ii].item + "," + reco[ii].name + "," + reco[ii].rating + "\n";
                }
                alert(buff);
            });
        </script>


[1]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing1.png
[2]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing2.png
[3]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing3.png
 

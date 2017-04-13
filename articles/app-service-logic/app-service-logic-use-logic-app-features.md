<properties 
    pageTitle="Logika App funkcióinak használata |} Microsoft Azure" 
    description="Megtudhatja, hogy miként használhatja a speciális logika alkalmazások funkcióját." 
    authors="stepsic-microsoft-com" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/28/2016"
    ms.author="stepsic"/> 
    
# <a name="use-logic-apps-features"></a>Logika alkalmazások szolgáltatások használata

Az [Előző témakör](app-service-logic-create-a-logic-app.md)az első logikai alkalmazás létre. Most már bemutatjuk, hogyan hozhat létre az alkalmazás szolgáltatások összefüggés-alkalmazások használata a tökéletes folyamat. Ez a témakör bemutatja a következő új logika alkalmazások:

- Feltételes logika, amely végrehajtja a művelet csak egy bizonyos feltétel teljesülése esetén.
- A Kódnézet és módosíthatja a meglévő logika alkalmazást.
- Munkafolyamat elindítása lehetőségek.

Ez a témakör elvégzése előtt töltsön ki [egy új összefüggés-alkalmazás létrehozása](app-service-logic-create-a-logic-app.md)című témakör lépéseit. Az [Azure portál]keresse meg a logika alkalmazás, és **Eseményindítók és a műveletek** gombra a összesítése való logika alkalmazás definíciójának szerkesztése.

## <a name="reference-material"></a>Anyagminta

Előfordulhat az alábbi dokumentumok hasznos:

- [Kezelés és runtime REST API -khoz](https://msdn.microsoft.com/library/azure/mt643787.aspx) – például hogy miként logika alkalmazások közvetlenül meghívása
- [Nyelvi hivatkozási](https://msdn.microsoft.com/library/azure/mt643789.aspx) – az összes támogatott függvények/kifejezések átfogó listája
- [Eseményindító és a művelet](https://msdn.microsoft.com/library/azure/mt643939.aspx) - műveletek különböző típusú és venniük a bemeneti adatok alapján
- [Alkalmazás szolgáltatás áttekintése](../app-service/app-service-value-prop-what-is.md) - megoldás létrehozása, hogy mikor érdemes összetevők leírása

## <a name="adding-conditional-logic"></a>Feltételes logika hozzáadását

Bár az eredeti áramlás működik, vannak bizonyos területeken, amely javítható. 


### <a name="conditional"></a>Feltételes
Logika az alkalmazás, e-mailek sok első vonhat. Az alábbi lépésekkel győződjön meg arról, hogy csak mailt kap, amikor a tweetbe feladójuk, a megadott számú követői logika hozzáadása. 

1. Kattintson a plusz, és keresse meg a műveletet *A felhasználó első* a Twitteren.

2. A **Tweeted szerint** mezőben adja át az eseményindító Twitter felhasználóra vonatkozó információkért.

    ![Felhasználói beszerzése](./media/app-service-logic-use-logic-app-features/getuser.png)

3. Kattintson ismét a plusz, de ez esetben válassza a **Feltétel hozzáadása**

4. Kattintson az első mezőbe a **…** alatt **Felhasználói kérjen** megkeresése a **követők száma** mezőben.

5. A tartalmazó legördülő listára válassza a **nagyobb, mint**

6. A második mezőbe írja be a kívánt felhasználókat, hogy követői számát.

    ![Feltételes](./media/app-service-logic-use-logic-app-features/conditional.png)

7.  Végül a húzással történő áthelyezés az e-mailt mezőjében a **Ha igen** mezőbe. Ez azt jelenti, akkor csak kap e-mailek követő metódus számának teljesülése esetén.

## <a name="repeating-over-a-list-with-foreach"></a>Ismétlődő fölé egy listát forEach

ForEach le adja meg a tömb művelet megismétléséhez fölé. Ha még nem tömb a folyamat sikertelen lesz. Példaként, amely az üzenetek tömb kimenet action1 van, és el szeretné küldeni az összes üzenetet, ha is felvehet az forEach nyilatkozat a művelet tulajdonságainak: forEach:"@action('action1').outputs.messages"
 

## <a name="using-the-code-view-to-edit-a-logic-app"></a>A Kódnézet használatával egy összefüggés-alkalmazás szerkesztése

A Tervező mellett közvetlenül szerkesztheti a kódot, amely definiálja az egy logikai alkalmazást, az alábbi képlettel történik. 

1. Kattintson a parancssávon **kód megjelenítése** gombra. 

    Ekkor megnyílik egy, a szerkesztett definíciója látható teljes szerkesztő.

    ![Kódnézet](./media/app-service-logic-use-logic-app-features/codeview.png)

    A text szerkesztő másolja és illessze be a tetszőleges számú műveletek az ugyanarra alkalmazásból logika vagy logikai alkalmazás között. Is egyszerűen felvehet és egész szakasz eltávolítása a definíció, és definíciók tartalmakat oszthat meg velük.

2. A Kódnézet végezze el a módosításokat, egyszerűen kattintson a **Mentés**gombra. 

### <a name="parameters"></a>Paraméterek
Vannak bizonyos funkciók csak a kód nézetben használható alkalmazások összefüggés. Ezek példája paramétereket. Paraméterek megkönnyítik a logika alkalmazás egész újra felhasználhat értékek. Ha például ha szeretné, hogy a több műveletet használt e-mail cím, be kell állítania, paraméterként.

A következő paraméterek használata a lekérdezési kifejezés a meglévő logika alkalmazás frissíti.

1. A Kódnézet keresse meg a `parameters : {}` objektum és a következő témakör objektum beszúrása:

        "topic" : {
            "type" : "string",
            "defaultValue" : "MicrosoftAzure"
        }
    
2. Görgessen le a `twitterconnector` művelet, keresse meg a lekérdezés értéket, és cserélni `#@{parameters('topic')}`.
    Közös két vagy több karakterláncok fűzhetők, például a **összefűzés** függvényt is használhatja: `@concat('#',parameters('topic'))` megegyezik a fent. 
 
Paramétereket kijjebb értéket tartalmaz megváltozhat sokkal jó módszer is. Azok különösen hasznosak, ha paramétereket különböző környezetekben felülbírálása kell. Paraméterek alapján környezetben felülbírálása további tájékoztatást a [REST API-dokumentáció](https://msdn.microsoft.com/library/mt643787.aspx)témakörben talál.

Most, kattintson a **Mentés**gombra, ha óránként kap minden új twitterre, amelyek a Dropbox **twitterre** nevű mappa kézbesítve legfeljebb 5 retweets.

Logika alkalmazás definíciók kapcsolatos további információért olvassa el a [logika alkalmazás definíciók Szerző](app-service-logic-author-definitions.md)című témakört.

## <a name="starting-a-logic-app-workflow"></a>Logika alkalmazás munkafolyamat indítása
Nincsenek többféle lehetőség elindítja a munkafolyamatot logika alkalmazás definiálva. Munkafolyamat mindig indítható el igény szerinti az [Azure-portálon].

### <a name="recurrence-triggers"></a>Ismétlődés indítók
Ismétlődés eseményindító megadott időközönként fut. Amikor az eseményindító feltételes logika van, az eseményindító határozza meg, vagy sem a munkafolyamat futtatásához szükséges rendszerkövetelmények. A kiváltó ok mező azt jelzi, hogy azt működnie kell visszaadó egy `200` állapotkód. Ha nem futtatásához szükséges, akkor adja vissza egy `202` állapotkód.

### <a name="callback-using-rest-apis"></a>A visszahívási REST API-k használata
Szolgáltatások felhívhatja a logika alkalmazás végpont munkafolyamat indítása. Című [logika hívható végpontok,](app-service-logic-connector-http.md) további információt. Adott típusú logika alkalmazás igény szerinti indításához kattintson a **Futtatás** gombra a parancssávon. 

<!-- Shared links -->
[Azure portál]: https://portal.azure.com 
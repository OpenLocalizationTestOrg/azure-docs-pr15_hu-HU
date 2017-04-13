<properties
    pageTitle="Első lépések az Azure-keresés a NodeJS |} Microsoft Azure |} A felhőben tárolt keresési szolgáltatás"
    description="Haladjon végig, és a felhőben tárolt keresési szolgáltatásának keresőalkalmazás támaszkodva NodeJS a használják a programnyelv Azure."
    services="search"
    documentationCenter=""
    authors="EvanBoyle"
    manager="pablocas"
    editor="v-lincan"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.date="07/14/2016"
    ms.author="evboyle"/>

# <a name="get-started-with-azure-search-in-nodejs"></a>Azure-keresés NodeJS az első lépések
> [AZURE.SELECTOR]
- [Portál](search-get-started-portal.md)
- [.NET](search-howto-dotnet-sdk.md)

Megtudhatja, hogyan hozhat létre egy egyéni NodeJS keresőalkalmazás, amely a keresési élményt Azure keresése használja. Ebben az oktatóanyagban objektumot és a következő gyakorlatban használt műveletek létrehozása az [Azure keresési szolgáltatás REST API -t](https://msdn.microsoft.com/library/dn798935.aspx) használja.

Azt használt [NodeJS](https://nodejs.org) és NPM, [Sublime szöveg 3](http://www.sublimetext.com/3)és a Windows PowerShell a Windows 8.1 fejlesztésére, és tesztelje a kódot.

Ez a példa futtatásához kell rendelkeznie az Azure keresési szolgáltatása, amelynek a jelentkezzen be az be az [Azure-portálon](https://portal.azure.com). Lásd: a [létrehozása az Azure keresési szolgáltatás a portálon](search-create-service-portal.md) részletes útmutatást.

## <a name="about-the-data"></a>Az adatokkal

Minta alkalmazás az [Amerikai Egyesült Államok geológiai szolgáltatások (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), kattintson a állapota a Rhode sziget adatkészlet méretének csökkentése érdekében szűri adatait használja. Az adatok használjuk, amely visszaadja a környezet épületeket, például a kórházat és iskolai, valamint geológiai szolgáltatásokat, például a adatfolyamok tavakat és summits keresőalkalmazás össze.

Ennek az alkalmazásnak a **DataIndexer** program hoz létre, és betölti az index az [Indexelő](https://msdn.microsoft.com/library/azure/dn798918.aspx) szerkezetet, a szűrt USGS adatkészlet lekérése nyilvános Azure SQL-adatbázis használata. Hitelesítő és kapcsolati az online adatforrás információkat a program kódot. További beállítás szükség.

> [AZURE.NOTE] A adatkészlet csoportban a szabad árak réteg 10 000 dokumentum határértékén kapcsolatának azt alkalmazott szűrő. Ha használja a szabványos réteg, ezt a korlátot nem vonatkoznak. Az egyes árak réteg kapacitás kapcsolatos részletekért című cikkben találhat [keresési szolgáltatás](search-limits-quotas-capacity.md).


<a id="sub-2"></a>
## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>Keresse meg a szolgáltatás neve és az api-kulcs az Azure keresési szolgáltatás

Miután létrehozta a szolgáltatást, visszatérhet az portált az URL-cím vagy `api-key`. A keresési szolgáltatás kapcsolat megkövetelése, hogy rendelkezik-e mindkét URL-CÍMÉT, és egy `api-key` hitelesítést végezni a hívást.

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com).
2. Ugrás sávon kattintson a lista minden vásárlásáról az előfizetéshez tartozó kiépítéstől Azure keresési szolgáltatás **keresési szolgáltatás** parancsra.
3. Jelölje ki a használni kívánt szolgáltatás.
4. A szolgáltatás irányítópulton látni fogja a lényeges adatokkal csempéit és elérni a felügyeleti billentyűk kulcs ikonra.

    ![][3]

5. Másolja a szolgáltatás URL-CÍMÉT, egy rendszergazda billentyű és egy lekérdezés billentyűt. Meg kell mindhárom később, amikor ad hozzá őket a config.js fájlt.

## <a name="download-the-sample-files"></a>A minta fájlok letöltése

A minta letöltése használata vagy az alábbi eljárások egyikét.

1. Nyissa meg a [AzureSearchNodeJSIndexerDemo](https://github.com/AzureSearch/AzureSearchNodeJSIndexerDemo).
2. Kattintson a **Letöltés ZIP-**, a .zip fájl mentése és kibonthatja a benne található fájlokat.

További fájl módosítását és a Futtatás kimutatások szemben a mappában található fájlok történik.


## <a name="update-the-configjs-with-your-search-service-url-and-api-key"></a>Frissítse a config.js. a keresési szolgáltatás URL-CÍMÉT és az api-kulcs

Az URL használatát és a korábban kimásolt API-ja kulcs adja meg az URL-címet, felügyeleti-billentyűt, és a lekérdezés billentyűs konfigurációs fájl.

Rendszergazdai billentyűk teljes hozzáférés szolgáltatási műveletek, például létrehozása vagy törlése az index és dokumentumok betöltése fölé. Lekérdezés kulcsok viszont a jellemző felhasználási módjuk látható Azure keresési csatlakozó ügyfélalkalmazások csak olvasható műveletekhez vannak.

Ez a példa azt írja be a lekérdezés kulcs érdekében célszerű a lekérdezés kulccsal ügyfélalkalmazásokban megerősítése.

Az alábbi képernyőképen látható **config.js** nyissa meg a szöveg-szerkesztőben és a megfelelő bejegyzések jelölni, hogy hol szeretné frissíteni a fájlt, amely a keresési szolgáltatás érvényes értékeket tartalmazó megjelenik.

![][5]


## <a name="host-a-runtime-environment-for-the-sample"></a>A Host egy futtatókörnyezet a minta

A minta szükséges HTTP-kiszolgáló, amely globálisan használatával npm telepíthető.

Az alábbi parancsok használata egy PowerShell-ablakot.

1. Nyissa meg azt a mappát, amely **package.json** fájlt tartalmazza.
2. Típus `npm install`.
2. Típus `npm install -g http-server`.

## <a name="build-the-index-and-run-the-application"></a>Létrehozhatja a tárgymutatót, és futtassa az alkalmazást

1. Típus `npm run indexDocuments`.
2. Típus `npm run build`.
3. Típus `npm run start_server`.
4. Közvetlenül a böngészőben elemre`http://localhost:8080/index.html`

## <a name="search-on-usgs-data"></a>USGS adatok keresése

A USGS adatkészlet tartalmazó szempontjából fontos Rhode sziget állam rekordokat tartalmazza. Ha egy üres keresőmezőbe **Keresés** gombra kattint, kapja a felső 50 bejegyzéseket, az alapértelmezett.

Keresési kifejezés beírásával képet ad a keresőprogram parancsával egy üzenetet. Adjon meg egy területi nevet. "Roger Williams" Rhode sziget első elsősorban volt. Számos parkok, épületeket és iskolai azonos nevű vele.

![][9]

Is megpróbálhatnak e kifejezések egyikét:

- Pawtucket
- Pembroke
- lúd + modern


## <a name="next-steps"></a>Következő lépések

Ez a az első Azure keresési oktatóprogram NodeJS és az USGS adatkészlet alapján. Az idő Ez az oktatóanyag további keresési funkciókat, érdemes lehet használni a egyéni megoldások az igazolni fogja kibővíthetjük.

Már tájékoztatatást Azure keresés, is használhatja az alábbi példa egy springboard suggesters (javaslatokat vagy az automatikus kiegészítés lekérdezések), szűrők és síklapos navigációs próbálja a. Is javíthatja a keresési eredmények lapja alapján számolja hozzáadásával, és kötegelés dokumentumok, így a felhasználók is lapozhat a az eredmények.

Új Azure keresés? Azt javasoljuk, hogy próbálja más oktatóanyagok fejlesztését megértéséhez mi hozhat létre. Keresse fel a [dokumentáció lap](https://azure.microsoft.com/documentation/services/search/) további információforrásokért. A hivatkozások megjelenítése a [Video- és oktatóanyag lista](search-video-demo-tutorial-list.md) eléréséhez további információt.

<!--Image references-->
[1]: ./media/search-get-started-nodejs/create-search-portal-1.PNG
[2]: ./media/search-get-started-nodejs/create-search-portal-2.PNG
[3]: ./media/search-get-started-nodejs/create-search-portal-3.PNG
[5]: ./media/search-get-started-nodejs/AzSearch-NodeJS-configjs.png
[9]: ./media/search-get-started-nodejs/rogerwilliamsschool.png

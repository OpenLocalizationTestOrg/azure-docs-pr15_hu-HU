<properties 
    pageTitle="Media Services REST API-áttekintés |} Microsoft Azure" 
    description="Media Services REST API-nak – áttekintés" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="juliako"/>


# <a name="media-services-rest-api-overview"></a>Media Services REST API-nak – áttekintés 

[AZURE.INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

Microsoft Azure Media Services szolgáltatása, amely az OData-alapú HTTP kéréseket fogad, és visszatér az részletes JSON vagy atom + pub válaszolhat. Azure tervezési útmutató Media Services megfelel, mert nincs egy sor olyan szükséges HTTP-fejlécek, amely az egyes ügyfelek kell használnia, Media Services való csatlakozáskor, valamint egy sor olyan választható fejlécek használható. Az alábbi szakaszok ismertetik a fejlécek és használhatja, ha a HTTP-műveletek igénylések létrehozása, és választ kap a Media Services.

##<a name="considerations"></a>Megfontolandó szempontok 

A következő szempontokat mérlegelve többi használatakor alkalmazhat.

- Lekérdezés szervezetek, ha van legfeljebb 1000 szervezetek egyszerre vissza, mert a nyilvános többi v2 korlátozza a lekérdezés eredményében 1000 eredményekre. Akkor használja a **Kihagyás gombra** , és **készítése** (.NET) [Ebben a példában a .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) és a [REST API-példában](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities)ismertetett: **felső** (REST)-, /. 

- Ha JSON használ, és megadja, hogy használja a **__metadata** kulcsszót a kérelem (például a hivatkozások a csatolt objektumot) kell állítani az **Elfogadás** élőfej [JSON részletes formátum](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/) (lásd az alábbi példa). OData nem érti a **__metadata** tulajdonság a kérelmet, kivéve, ha állítsa be a részletes.  

        POST https://media.windows.net/API/Jobs HTTP/1.1
        Content-Type: application/json;odata=verbose
        Accept: application/json;odata=verbose
        DataServiceVersion: 3.0
        MaxDataServiceVersion: 3.0
        x-ms-version: 2.11
        Authorization: Bearer <token> 
        Host: media.windows.net
        
        {
            "Name" : "NewTestJob", 
            "InputMediaAssets" : 
                [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aba5356eb-30ff-4dc6-9e5a-41e4223540e7')"}}]
        . . . 
        

## <a name="standard-http-request-headers-supported-by-media-services"></a>Szabványos HTTP-kérés fejlécek Media Services által támogatott

Minden hívás, akkor készítsen Media Services szerepelnie kell a kérelem szükséges fejlécek halmazának van, és is választható fejlécek halmazának érdemes tartalmazza. A következő táblázat felsorolja a szükséges fejlécek:


Élőfej|Típus|Érték
---|---|---
Engedély|Bearer|A rendszer csak elfogadott engedélyezési mechanizmusa bearer. Az érték is tartalmaznia kell a hozzáférési jogkivonat ACS által biztosított.
x – az ms-verzió|Decimális|2.11
DataServiceVersion|Decimális|3.0
MaxDataServiceVersion|Decimális|3.0



>[AZURE.NOTE] Mivel Media Services OData kattintva jelenítse meg a mögöttes eszköz metaadatok tárházba keresztül REST API-khoz is használja, az DataServiceVersion és MaxDataServiceVersion fejlécek szerepelnie kell kérésének; jó helyen jár Ha nem, majd jelenleg Media Services feltételezi, hogy a használati DataServiceVersion érték 3.0.

Az alábbi olyan fejlécek nem kötelező:

Élőfej|Típus|Érték
---|---|---
Dátum|RFC 1123 dátum|A kérés időbélyeg
Fogadja el|Webhelytartalom-típus|A kért tartalomtípus a válasz, például a következőket:<p> -alkalmazás/json; odata = részletes<p> -alkalmazás/atom + xml<p> Válaszok lehetnek egy másik webhelytartalom-típus, például egy blob fájllehívási hol sikeres választ a blob-adatfolyam, mint a tartalom fog tartalmazni.
Fogadja el kódolást|Gzip, homorú|GZIP és DEFLATE kódolást, ha alkalmazható. Megjegyzés: Nagy erőforrások esetén Media Services előfordulhat, hogy figyelmen kívül hagyja a fejléc és noncompressed adatok.
Fogadja el nyelvi|"en", "es", és így tovább.|Adja meg a használni kívánt nyelvet, a válasz.
Fogadja el karakterkészlet|Karakterkészlet típus, például "UTF-8"|Alapértelmezés szerint UTF-8.
X – HTTP-módszer|HTTP-metódus|Lehetővé teszi az ügyfelek vagy a tűzfalak úgy, hogy például az alábbi módszerekkel, bújtatott keresztül GET-hívás kezdeményezése a helyezése vagy a törlés HTTP módszerek nem támogató.
Tartalomtípus|Webhelytartalom-típus|A kérelem szervezet helyezése vagy a bejegyzés tartalomtípus kéri.
ügyfél-kérés-azonosító|Karakterlánc|Egy hívó által megadott érték, amely azonosítja az adott kérelem. Adja meg, ha ezt az értéket szerepelni fog a válaszüzenetben kérések módja. <p><p>**Fontos**<p>Értékek kell felső határa 2096b (2-k).

## <a name="standard-http-response-headers-supported-by-media-services"></a>Szabványos HTTP-válasz fejlécek Media Services által támogatott

Az alábbiakban állítható vissza, attól függően, hogy az erőforrás, amelyeket akitől és a végrehajtani kívánt művelet rovatfejek csoportja.


Élőfej|Típus|Érték
---|---|---
kérés-azonosító|Karakterlánc|Az aktuális művelet szolgáltatás generált egyedi azonosító.
ügyfél-kérés-azonosító|Karakterlánc|Ha jelen van a hívó az eredeti összehívásban megadott azonosítót.
Dátum|RFC 1123 dátum|A kérés feldolgozásának dátum.
Tartalomtípus|Változó|A válasz szervezet tartalomtípus.
Tartalom-kódolás|Változó|Gzip homorú, vagy szükség szerint.


## <a name="standard-http-verbs-supported-by-media-services"></a>Media Services támogatja a normál HTTP-műveletek

Az alábbiakban teljes listáját a HTTP-műveletek, amikor HTTP kezdeményezhet kér használható:


Művelet|Leírás
---|---
GET|Objektum jelenlegi értékét adja eredményül.
BEJEGYZÉS|Létrehoz egy objektumot megadott adatok alapján, illetve elküld egy parancsot.
HELYEZÉSE|Lecseréli egy objektumot, vagy objektumot hoz létre elnevezett (ha alkalmazható).
TÖRLÉSE|Objektum törlése
KÖRLEVÉL|Frissíti a meglévő objektum elnevezett tulajdonság módosításokkal.
CÍMSOR|Visszajelzés kérése az objektum eredménye metaadat-alapú.

##<a name="limitation"></a>Korlátozás

Lekérdezés szervezetek, ha van legfeljebb 1000 szervezetek egyszerre vissza, mert a nyilvános többi v2 korlátozza a lekérdezés eredményében 1000 eredményekre. Akkor használja a **Kihagyás gombra** , és **készítése** (.NET) [Ebben a példában a .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) és a [REST API-példában](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities)ismertetett: **felső** (REST)-, /. 


## <a name="discovering-media-services-model"></a>Media Services modell felfedezése

A tételére Media Services szervezetek további, a $metadata műveletet is használható. Lehetővé teszi a lekérdezni az összes érvényes személy típusú, entitás tulajdonságai, társítások, függvények, műveletek és így tovább. A következő példa bemutatja, hogyan kell a URI Egyenletszerkesztővel: https://media.windows.net/API/$ metaadatok.

Érdemes a Hozzáfűzés "? api-version=2.x" a metaadatok megtekintése a böngészőben, és ne kerüljön bele az x-ms-verzió élőfej kérését a URI végére.



##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





 

<properties 
    pageTitle="Hogyan hozhat létre egy Media processzor |} Microsoft Azure" 
    description="Megtudhatja, hogyan hozhat létre kódolását, formátum konvertálni, titkosítása vagy médiatartalom visszafejteni az Azure Media Services media feldolgozó összetevő." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="how-to-get-a-media-processor-instance"></a>Útmutató: egy Media processzor példány beszerzése


> [AZURE.SELECTOR]
- [.NET](media-services-get-media-processor.md)
- [TÖBBI](media-services-rest-get-media-processor.md)

##<a name="overview"></a>– Áttekintés

A Media Services media feldolgozó összetevő, amely egy adott feldolgozási tevékenység kódolást, például kezeli konvertálás, titkosítása, vagy a médiatartalom visszafejtése formázhatja. Általában hoz létre egy media processzor kódolását, titkosítása és multimédiás tartalom formátumának konvertálása feladat létrehozásakor.

A következő táblázat a név és leírás minden elérhető médiafájlokat processzor tartalmaz.

Multimédia-feldolgozó neve|Leírás|További információ
---|---|---
Media Encoder Standard|Szabványos szolgáltatásokat nyújt az igény szerinti kódolását. |[Áttekintés és a igény szerinti Media kódolók Azure összehasonlítása](media-services-encode-asset.md)
Media Encoder prémium munkafolyamat|Lehetővé teszi Media Encoder prémium munkafolyamat kódolási feladatok futtatását.|[Áttekintés és a igény szerinti Media kódolók Azure összehasonlítása](media-services-encode-asset.md)
Azure Media indexelő| Lehetővé teszi, hogy a médiafájlok és a tartalom kereshető, valamint a kódolt feliratok számok és a kulcsszavak készítése.|[Azure Media indexelő](media-services-index-content.md)
Azure Media Hyperlapse (előzetes verzió)|A videóban a videó stabilizációjának "ívek" simítja teszi lehetővé. Is lehetővé teszi a tartalom gyorsítása felhasználható klip be.|[Azure Media Hyperlapse](media-services-hyperlapse-content.md)
Azure Media Encoder|Értékcsökkenés
Tárterület visszafejtés| Értékcsökkenés|
Azure Media Packager|Értékcsökkenés|
Azure Media titkosító|Értékcsökkenés|

##<a name="get-mediaprocessor"></a>MediaProcessor beszerzése

>[AZURE.NOTE] Amikor a Media Services REST API-val, a következő érvényesek:
>
>A Media Services szervezetek elérésekor a be kell állítani a HTTP-összehívásokban adott fejlécmezők és értékek. További tudnivalókért olvassa el a [Media Services REST API -val fejlesztési beállítása](media-services-rest-how-to-use.md)című témakört.

>Https://media.windows.net sikeres kapcsolódás után jelenik meg a 301 átirányítást másik Media Services URI megadása. A [Csatlakozás a Media Services REST API segítségével](media-services-rest-connect-programmatically.md)leírtak szerint új URI későbbi hívásainak kell végeznie. 


A következő többi hívás szemlélteti, hogyan kérhet egy media processzor-példány nevét (Ez esetben **Media Encoder szabványos**). 



    
A kérelem:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.11
    Host: media.windows.net
    
Válasz:
        
    . . .
    
    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-steps"></a>Következő lépések

Most, hogy egy media processzor példány beszerzése, lépjen a [hogyan kódolását tárgyi eszköz](media-services-rest-get-started.md) témakört, amely bemutatja, hogyan használja a Media Encoder szabványos kódolását tárgyi eszköz.

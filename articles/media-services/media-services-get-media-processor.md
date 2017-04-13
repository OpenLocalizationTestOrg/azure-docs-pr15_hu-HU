<properties 
    pageTitle="Hogyan hozhat létre egy Media processzor |} Microsoft Azure" 
    description="Megtudhatja, hogyan hozhat létre kódolását, formátum konvertálni, titkosítása vagy médiatartalom visszafejteni az Azure Media Services media feldolgozó összetevő. Mintakódok írt C#, és használja a Media Services SDK .NET." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
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

##<a name="get-media-processor"></a>Első Media processzor

A következő módszerrel egy media processzor példány beszerzése jeleníti meg. A példa feltételezi, hogy **_context** az kiszolgálói környezetben hivatkozni, a szakaszban leírt módon nevű modul szintű változó felhasználása [hogyan: csatlakozás Media Services programozás útján](media-services-dotnet-connect-programmatically.md).

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
        return processor;
    }


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-steps"></a>Következő lépések

Most, hogy egy media processzor példány beszerzése, lépjen a [hogyan kódolását tárgyi eszköz](media-services-dotnet-encode-with-media-encoder-standard.md) témakört, amely bemutatja, hogyan használja a Media Encoder szabványos kódolását tárgyi eszköz.



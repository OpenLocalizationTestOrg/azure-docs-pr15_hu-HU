<properties
    pageTitle="Azure Media Services Analytics áttekintése |} Microsoft Azure"
    description="Azure Media Services kínál az Azure Media Analytics, beszéd- és a vállalati skála, megfelelőség, biztonság és globális vannak látás szolgáltatások gyűjteménye nyilvános előnézetét. Azure Media Analytics-szolgáltatások használatával az Azure Media Services fő platform összetevőinek kialakításának, és így készen áll a nap egy skála feldolgozása media kezelheti. "
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/24/2016"   
    ms.author="milanga;juliako;johndeu"/>

# <a name="azure-media-services-analytics-overview"></a>Azure Media Services Analytics – áttekintés

##<a name="overview"></a>– Áttekintés

További szervezetek és vállalkozások is magában foglal videó az előnyben részesített közepes a munkatársak, folytatására a vevők és a dokumentum üzleti funkciók másként. Felhőalapú számítások teszi hatékony tárolhat, adatfolyam, és elérheti ezeket a nagy médiafájlokat, de a saját videokép tartalomtár növekedésével a vállalatok kell egy vonalba hatékony eszközöket kibontása új összefüggésekre videóból létrehozásához az alkalmasabb, személyre szabott a célközönségek interakciók és azok üzleti készítése felsőfokon.

Kell, hogy növekvő a a piactér, az Azure Media Services kínál Media Analytics, beszéd- és látás alkatrészek (a nagyvállalati skála, megfelelőség, biztonság és globális vannak), amelyek megkönnyítik a szervezetek és a videofájlok az értekezletekre háttérismeretek forrásául vállalkozások gyűjteménye. Azure Media Analytics-szolgáltatások használatával az Azure Media Services fő platform összetevőinek kialakításának, és így készen áll a nap egy skála feldolgozása media kezelheti.

Azure Media Analytics engedélyezze a fejlesztők gyorsan első lépések a videó fölött skála korlátozott látás funkciókhoz, és jelenítse meg a speciális funkciókat alkalmazásba. Azure Media Analytics épül fel a teljes skála, megfelelőség, biztonsági és globális vannak a nagyobb szervezetek szükséges a vállalati környezetben való használatra.

Az alábbi ábra mutatja **Media Analytics** - és egyéb a Media Services platform fő részei. 

![VoD munkafolyamat](./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png)

Médiafájlok Analytics media processzorok konzerv MP4 vagy JSON fájlokat. Ha egy media processzor előállítása MP4-fájlként, fokozatosan letöltheti a fájlt. Ha egy media processzor JSON fájl előállítása, melyet letölthet a fájlt az Azure blob-tárolóhoz. 

## <a name="azure-media-analytics-services"></a>Azure Media Analytics-szolgáltatások

- **Indexelő** – Azure Media indexelő lehetővé teszi, hogy a tartalom kereshető, jól kódolt feliratok számok készítése. Azure Media Services gyorsabban indexelési és szélesebb nyelvek támogatása **Azure Media indexelő 2 Preview** jelent meg. A támogatott nyelvek közé tartozik, spanyol, német, angol, francia, olasz, kínai, portugál és arab. Részletes információk és Példák című témakörben talál [az Azure Media indexelő 2 folyamat videók](media-services-process-content-with-indexer2.md)
 
- **Hyperlapse** – Microsoft Hyperlapse 20 év számítógép látás kutatás a Microsoft Research (MSR), videó exportstabilizációt és az idő rendelkezések megkerülését, rövid, felhasználható, tetszetős videók létrehozása a hosszú űrlapról származó tartalom kombinálása eredménye. Idő elévül létrehozása, mellett is használhatja Hyperlapse stabil videók készítése mobiltelefonok és camcorderek rögzített shaky videók. Részletes tudnivalókat és példákat talál [Az Azure Media Hyperlapse Hyperlapse médiafájlok](media-services-hyperlapse-content.md)
 
- **Mozgásvonalas észlelése** – mozgásvonalas észleli a levélpapír hátterek a videó szolgáltatást is használhatja. Az ügyfelek, akik ellenőrizni szeretné a mozgásvonalak események észleli a felügyeleti videó adatcsatornák felügyeleti fényképezőgépek téves az ideális. Részletes információk és Példák című témakörben talál [Az Azure Media analitika mozgásvonal észlelési](media-services-motion-detection.md).
 
- A **arc észlelési és arc érzelmek** – ezzel a szolgáltatással, elvégezheti a meghívottak arc és azok érzelmek, beleértve a Boldogsága, sadness, jelzés nélküli, utasítás, engedetlenség címén indíthat pert, félelem, Undort és indifference/semleges. Számos hasznos üzleti alkalmazások leírása alább, összesítése és vehet részt esemény személyek reakció elemzése többek között azt. Részletes információk és Példák című témakörben talál [arc és Emotion észlelése az Azure Media Analytics](media-services-face-and-emotion-detection.md).
 
- **Videó összefoglaló** – videó összefoglaló segítséget nyújtanak a forrás videóban érdekes kódrészletek automatikusan választva hosszú videók összegzéseinek hozhat létre. Ez akkor hasznos, ha meg szeretné, hogy mire számíthat, hosszú a videó gyors áttekintést nyújt. Részletes tudnivalókat és példákat talál [Használata Azure Media videó miniatűrjét hozhat létre egy videó összefoglaló](media-services-video-summarization.md)

- **Optikai karakterfelismeréssel** - Azure Media Analytics OCR (optikai karakterfelismeréssel) lehetővé teszi a videofájlok szövegtartalom szerkeszthető, kereshető digitális szöveggé alakíthatja. Ez lehetővé teszi, hogy értelmes metaadatok kivonása, a videó jel médiafájlt a automatizálása.
 
- **Méretezhető arc kivonási** - **Azure Media Redactor** az Azure Media Analytics MP, amely felajánlja a méretezhető arc kivonási a felhőben. Arc kivonási lehetővé teszi, hogy a videoklip módosíthatja a kijelölt személyek arc életlenítés érdekében. Előfordulhat, hogy használni kívánt a arc kivonási szolgáltatás nyilvános biztonsági és hírek media helyzetekben. Kivonás manuálisan az órát is igénybe vehet a Képanyag több ARC tartalmazó néhány percet, de ez a szolgáltatás a arc kivonási folyamat esetén kell néhány egyszerű lépéssel. További tudnivalókért lásd: [Ez](media-services-face-redaction.md) a cikk.

 
## <a name="common-scenarios"></a>Gyakori alkalmazási területek

Az alábbiakban néhány esetek, ahol Azure Media Analytics segítségével a szervezetek és vállalkozások keresztül ágazatok glean hozhat létre több személyre szabott közönség és alkalmazott Előjegyzések videóból új összefüggésekre, valamint videotartalmak nagy mennyiségű hatékonyabban kezelése:

- A **hívás középre igazítása** – az ügyfél hívás erőforrások továbbra is megkönnyítik az ügyfél szolgáltatás tranzakciókat nagy része a közösségi média megjelenésével páros. A hang adatok kódolt van azoknak a felhasználóknak, javítja a termék ütemtervben és is munkatársak hívás központ eléréséhez magasabb szintű felhasználói elégedettséget elemezheti adatait különböző. Azure Media indexelő használatával a felhasználók is tudják kinyerni a szöveget, és a keresési index és a intelligenciával kapcsolatos leggyakoribb körül kibontásához irányítópultok panaszt tesz, forrásának panaszt tesz build és egyéb vonatkozó adatokat.

- A **felhasználók létrehozott tartalom moderálása** – hírek media csatlakozó aljzatokkal rendőrségi részlegek, hogy a sok szervezet rendelkezik nyilvános szemben lévő portálokról, ahol elfogadása UGC adathordozóra, például a képek és videók. A tartalom mennyisége miatt nem várt események is lefoglalását. Forgatókönyvekben akkor van közelében lehetetlenné megfelelőségét tartalmának hatékony kézi felülvizsgálata elvégzéséhez. Ügyfelek számíthat a tartalom moderálása szolgáltatás a megfelelő tartalmat összpontosíthat.

- **Felügyeleti** - az IP-fényképezők értéknövekedésével van egy alábontási felügyeleti videoképet. Időre intenzív és emberi hiba támadásokkal manuálisan a felügyeleti videó megtekintésével szükség. Azure Media Analytics például mozgásvonalas észlelése, arc észlelési és Hyperlapse, hogy a folyamat áttekintése, kezelése és származtatott könnyebben létrehozása több összetevők biztosít.

## <a name="media-services-analytics-media-processors"></a>Media Services Analytics Media processzorok 

Ez a szakasz összes a Media Services Analytics Media processzorok (MP), és látható használatával hogyan .NET vagy a többi egy MP objektum beolvasása.

### <a name="mp-names"></a>MP nevek


- Azure Media indexelő 2 előzetes verzió
- Azure Media indexelő
- Azure Media Hyperlapse
- Azure Media arc jelentő
- Azure Media Mozgásvonalas jelentő
- Azure Media Video miniatűrjei
- Azure Media optikai Karakterfelismerés

### <a name="net"></a>.NET

A következő függvénynek megnyitja az egyik megadott MP nevére, és vissza MP objektumot.

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors
            .Where(p => p.Name == mediaProcessorName)
            .ToList()
            .OrderBy(p => new Version(p.Version))
            .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }


## <a name="rest"></a>TÖBBI

A kérelem:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Azure%20Media%20OCR' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.12
    Host: media.windows.net
    
Válasz:
        
    . . .
    
    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:074c3899-d9fb-448f-9ae1-4ebcbe633056",
             "Description":"Azure Media OCR",
             "Name":"Azure Media OCR",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

##<a name="demos"></a>Bemutatók

[Azure Media Analytics-bemutatók](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

##<a name="next-steps"></a>Következő lépések

Tekintse át a Media Services tanulási javaslatok.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-articles"></a>Kapcsolódó cikkek

[Media Services Analytics bejelentése](https://azure.microsoft.com/blog/introducing-azure-media-analytics/)
  

<!-- Images -->

[overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png

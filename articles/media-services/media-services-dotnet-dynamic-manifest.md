<properties 
    pageTitle="Szűrők létrehozása az Azure Media Services .NET SDK" 
    description="Ez a témakör ismerteti, hogyan hozhat létre a szűrők, így az ügyfélnek lehet a segítségükkel adatfolyam meghatározott szakaszait megjelenítő Értékáram. Media Services eléréséhez a szelektív streaming dinamikus jegyzékfájlok hoz létre." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="07/18/2016"
    ms.author="juliako;cenkdin"/>


#<a name="creating-filters-with-azure-media-services-net-sdk"></a>Szűrők létrehozása az Azure Media Services .NET SDK

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-dynamic-manifest.md)
- [TÖBBI](media-services-rest-dynamic-manifest.md)

2.11 megjelenés kezdve, a Media Services lehetővé teszi a eszközök szűrők megadása. Ezek a szűrők kiszolgáló egymás szabályok, amelyek lehetővé teszik az ügyfelek dönt, hogy a többek között: csak egy részét (helyett a teljes videó lejátszása) a videó lejátszás, vagy adjon meg, hogy az ügyfél eszköz (helyett az összes a megjelenítések az eszköz társított) képes kezelni, hang- és megjelenítések csak egy részét. **Dinamikus cikkét**s, az ügyfél kérésre adatfolyam-videó megadott szűrő alapján létrehozott keresztül érhető el ez a szűrés a eszközök.

További információt, szűrők és dinamikus cikkét kapcsolatos [dinamikus jegyzékfájlok áttekintése](media-services-dynamic-manifest-overview.md)című témakörben találhat.

Ez a témakör bemutatja, hogy hogyan Media Services .NET SDK létrehozása, módosítása és törlése a szűrők. 


Megjegyzés: Ha frissíti a szűrőt, a folyamatos átvitelű végpont frissítheti a szabályok az legfeljebb 2 percig is eltarthat. Ha a tartalom lett felszolgált szűrővel (és tárolja a proxykiszolgálók és CDN gyorsítótárát), a szűrő frissítése okozhat player hibák. A gyorsítótárat a szűrő frissítése után ajánlott. Ha ez a beállítás nem lehetséges, akkor fontolja meg egy másik szűrőt. 

##<a name="types-used-to-create-filters"></a>Szűrők létrehozásához használt típusai

A következő típusú használatosak szűrők létrehozása: 

- **IStreamingFilter**.  Ez a típus alapul a következő REST API- [Szűrés](http://msdn.microsoft.com/library/azure/mt149056.aspx)
- **IStreamingAssetFilter**. Ez a típus alapján a következő REST API- [AssetFilter](http://msdn.microsoft.com/library/azure/mt149053.aspx)
- **PresentationTimeRange**. Ez a típus alapján a következő REST API- [PresentationTimeRange](http://msdn.microsoft.com/library/azure/mt149052.aspx)
- **FilterTrackSelectStatement** és **IFilterTrackPropertyCondition**. Ezek a típusok alapján a következő REST API-khoz [FilterTrackSelect és FilterTrackPropertyCondition](http://msdn.microsoft.com/library/azure/mt149055.aspx)


##<a name="createupdatereaddelete-global-filters"></a>Globális szűrők létrehozása és frissítése vagy olvasási/törlése

A következő kódot .NET létrehozása, módosítása, olvassa el, és törölje a digitális eszköz kiválasztása szűrők mutatja.
    
    string filterName = "GlobalFilter_" + Guid.NewGuid().ToString();
                
    List<FilterTrackSelectStatement> filterTrackSelectStatements = new List<FilterTrackSelectStatement>();
    
    FilterTrackSelectStatement filterTrackSelectStatement = new FilterTrackSelectStatement();
    filterTrackSelectStatement.PropertyConditions = new List<IFilterTrackPropertyCondition>();
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackNameCondition("Track Name", FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackBitrateRangeCondition(new FilterTrackBitrateRange(0, 1), FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackTypeCondition(FilterTrackType.Audio, FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatements.Add(filterTrackSelectStatement);
    
    // Create
    IStreamingFilter filter = _context.Filters.Create(filterName, new PresentationTimeRange(), filterTrackSelectStatements);
    
    // Update
    filter.PresentationTimeRange = new PresentationTimeRange(timescale: 500);
    filter.Update();
    
    // Read
    var filterUpdated = _context.Filters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filter.Delete();


##<a name="createupdatereaddelete-asset-filters"></a>Hozzon létre és frissítés/olvasási/törlése digitáliseszköz-szűrők

A következő kódot .NET létrehozása, módosítása, olvassa el, és törölje a digitális eszköz kiválasztása szűrők mutatja.

    
    string assetName = "AssetFilter_" + Guid.NewGuid().ToString();
    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);
    
    string filterName = "AssetFilter_" + Guid.NewGuid().ToString();
    
        
    // Create
    IStreamingAssetFilter filter = asset.AssetFilters.Create(filterName,
                                        new PresentationTimeRange(), 
                                        new List<FilterTrackSelectStatement>());
    
    // Update
    filter.PresentationTimeRange = 
            new PresentationTimeRange(start: 6000000000, end: 72000000000);
    
    filter.Update();
    
    // Read
    asset = _context.Assets.Where(c => c.Id == asset.Id).FirstOrDefault();
    var filterUpdated = asset.AssetFilters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);
    
    // Delete
    filterUpdated.Delete();
    



##<a name="build-streaming-urls-that-use-filters"></a>A folyamatos átvitelű URL-címeit, szűrők használó összeállítása

Tegye közzé és használhat az előadáshoz eszközeit, akkor olvassa el a [Tartalom előadásához vevők – áttekintés](media-services-deliver-content-overview.md)olvashat.


Az alábbi példák bemutatják, hogyan szűrők hozzáadása a továbbított URL-ek.

**MPEG-KÖTŐJEL** 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP Live adatfolyam (HLS) V4**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP Live adatfolyam (HLS) V3**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Zökkenőmentes Streaming**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


**HDS**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=f4m-f4f, filter=MyFilter)


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Lásd még: 

[Dinamikus jegyzékfájlok – áttekintés](media-services-dynamic-manifest-overview.md)
 


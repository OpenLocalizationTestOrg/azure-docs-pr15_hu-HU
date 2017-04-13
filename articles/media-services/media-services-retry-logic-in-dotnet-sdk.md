<properties
    pageTitle="Ismételje meg a Media Services SDK logika a .NET rendszerhez |} Microsoft Azure"
    description="A témakör áttekintést újrapróbálkozási logika a Media Services SDK a .NET rendszerhez."
    authors="Juliako"
    manager="erikre"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016" 
    ms.author="juliako"/>


# <a name="retry-logic-in-the-media-services-sdk-for-net"></a>Ismételje meg a Media Services SDK logika a .NET rendszerhez

Ideiglenes (tranziens) hiba akkor fordulhat elő, a Microsoft Azure-szolgáltatások használatakor. Ha egy ideiglenes (tranziens) hiba lép fel, a legtöbb esetben néhány próbálkozás után sikeres művelet. A .NET rendszerhez, a Media Services SDK végrehajtja a kivételek és webes kérések okozó hibák kapcsolódó tranziens hibák kezelheti a újrapróbálkozási logika lekérdezések, a menti a módosításokat, és tárolási műveletek végrehajtása.  Alapértelmezés szerint a .NET rendszerhez, a Media Services SDK négy újrapróbálkozások előtt a kivételt, az alkalmazás ismételt értesítő hajt végre. Az alkalmazás a kódot kell majd kezelheti a Ez a kivétel megfelelően.  
  
 Az alábbiakban látható webes kérése, a tárhely, a lekérdezés és a létrehozva házirendek rövid útmutató:  
  
-   A tárolási házirend blob tárolási műveletek (feltöltések vagy eszköz letöltés) szolgál.  
  
-   A webes kérelem házirend általános webes kérelmek (például az első egy hitelesítési jogkivonat, és a felhasználók fürt végpont feloldása) szolgál.  
  
-   A lekérdezési szabályok használható lekérdezése személyek a többi (például mediaContext.Assets.Where(...)).  
  
-   A létrehozva házirend használható módon mindent, ami módosítja a szolgáltatás (például létrehozása egy egyed, hívja fel a szolgáltatás függvény egy művelet frissítése entitás) adatait.  
  
 Ez a témakör felsorolja a kivétel, és a .NET rendszerhez a Media Services SDK által kezelt hibakódok újra összefüggés.  
  
## <a name="exception-types"></a>Kivétel típusai  

Az alábbi táblázat ismerteti a kivételeket, amelyek a .NET rendszerhez, a Media Services SDK kezeli, illetve bizonyos műveletek, amelyek tranziens hibák miatt az nem kezeli.  
  
Kivétel|Webes kérelem|Tárhely|Lekérdezés|Létrehozva
----|------|----|---|---
WebException<br/>További információ a [WebException állapot kódok](media-services-retry-logic-in-dotnet-sdk.md#WebExceptionStatus) című.|igen|igen|igen|igen  
DataServiceClientException<br/> További tudnivalókért lásd: a [HTTP hibakódok állapotát](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|nem|igen|igen|igen
DataServiceQueryException<br/> További tudnivalókért lásd: a [HTTP hibakódok állapotát](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|nem|igen|igen|igen  
DataServiceRequestException<br/> További tudnivalókért lásd: a [HTTP hibakódok állapotát](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|nem|igen|igen|igen  
DataServiceTransportException|nem|nem|igen|igen
TimeoutException|igen|igen|igen|nem
SocketException|igen|igen|igen|igen  
StorageException|nem|igen|nem|nem 
IOException|nem|igen|nem|nem
  
###  <a name="WebExceptionStatus"></a>WebException állapot kódok  

A következő táblázat mutatja, hogy melyik WebException hibakódok az újraküldés logikája hajtanak végre. A [WebExceptionStatus](http://msdn.microsoft.com/library/system.net.webexceptionstatus.aspx) felsorolás állapot kód határozza meg.  
  
Állapot|Webes kérelem|Tárhely|Lekérdezés|Létrehozva  
-----|-----------------|-------------|-----------|----------  
ConnectFailure|igen|igen|igen|igen
NameResolutionFailure|igen|igen|igen|igen  
ProxyNameResolutionFailure|igen|igen|igen|igen  
SendFailure|igen|igen|igen|igen
PipelineFailure|igen|igen|igen|nem  
ConnectionClosed|igen|igen|igen|nem  
KeepAliveFailure|igen|igen|igen|nem  
UnknownError|igen|igen|igen|nem 
ReceiveFailure|igen|igen|igen|nem  
RequestCanceled|igen|igen|igen|nem  
Határidő|igen|igen|igen|nem
Protokollhiba lépett <br/>A kísérletek száma Protokollhiba lépett a HTTP állapot kód kezelési szabályozza. További tudnivalókért lásd: a [HTTP hibakódok állapotát](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|igen|igen|igen|igen|  
  
###  <a name="HTTPStatusCode"></a>HTTP hibakódok állapota  

Lekérdezés és létrehozva műveletek throw DataServiceClientException, DataServiceQueryException vagy DataServiceQueryException, ha a HTTP állapot hibakód az StatusCode tulajdonságban.  A következő táblázat mutatja, hogy melyik hibakódok az újraküldés logikája hajtanak végre.  
  
 
Állapot|Webes kérelem|Tárhely|Lekérdezés|Létrehozva 
---|----|----|----|----
401|nem|igen|nem|nem
403|nem|igen<br/>Hosszabb vár ismétlések kezelése.|nem|nem  
408|igen|igen|igen|igen
429|igen|igen|igen|igen  
500|igen|igen|igen|nem  
502|igen|igen|igen|nem  
503|igen|igen|igen|igen  
504|igen|igen|igen|nem  
  
Ha ki szeretne venni egy pillantást a Media Services SDK tényleges végrehajtása a .NET rendszerhez logika újra című [azure-sdk-a-multimédia-szolgáltatások](https://github.com/Azure/azure-sdk-for-media-services/tree/dev/src/net/Client/TransientFaultHandling).

## <a name="next-steps"></a>Következő lépések

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

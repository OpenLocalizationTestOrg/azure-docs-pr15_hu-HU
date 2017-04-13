<properties
 pageTitle="Lejárat Azure CDN Azure Web Apps alkalmazások/Cloud Services, az ASP.NET és az IIS tartalmának kezelése |} Microsoft Azure"
 description="Megtudhatja, hogy miként kezelheti a felhőalapú szolgáltatás tartalmának Azure CDN lejártát"
 services="cdn"
 documentationCenter=".NET"
 authors="camsoper"
 manager="erikre"
 editor=""/>
<tags
 ms.service="cdn"
 ms.workload="media"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="09/19/2016"
 ms.author="casoper"/>

# <a name="how-to-manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a>Lejárati Azure CDN Azure Web Apps alkalmazások/Cloud Services, az ASP.NET vagy az IIS tartalmának kezelése

> [AZURE.SELECTOR]
- [Azure Web Apps alkalmazások/Cloud Services, az ASP.NET vagy az IIS](cdn-manage-expiration-of-cloud-service-content.md)
- [Azure blob tárhelyszolgáltatáshoz](cdn-manage-expiration-of-blob-content.md)

Nyilvánosan hozzáférhető origin webkiszolgálón származó fájlok gyorsítótárba helyezhető az Azure CDN mindaddig, amíg a program a time-to-live (TTL).  A TTL (élettartam) a HTTP-válasz a forráskiszolgálóval a [ *Gyorsítótár-vezérlők* élőfej](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) határozza meg.  Ez a cikk ismerteti, hogy miként állíthatja `Cache-Control` az Azure Web Apps alkalmazások, Azure Cloud Services, az ASP.NET-alkalmazások és az Internet Information Services-webhelyek, ami hasonló módon konfigurált fejlécek.

>[AZURE.TIP] Előfordulhat, hogy válassza a fájl nem TTL (élettartam) beállítása.  Ebben az esetben a Azure CDN automatikusan elhelyezi a TTL (élettartam), a hét napig alapértelmezett.
>
>A fájlokat és más erőforrások: az access gyorsítására Azure CDN működésével kapcsolatos további tudnivalókért olvassa el a az [Azure CDN – áttekintés](./cdn-overview.md)című témakört.

## <a name="setting-cache-control-headers-in-configuration"></a>Gyorsítótár-vezérlő fejlécek konfiguráció beállítása

Statikus tartalom, például a képek és a stíluslapok megadhatja, hogy a frissítési gyakoriság a **következő** vagy **web.config** fájlokat a webalkalmazás módosításával.  A beállítási fájl **system.webServer\staticContent\clientCache** elemének állítja a `Cache-Control` élőfej a tartalom számára. Az **web.config**a beállítások hatással mindent az a mappa belső mappáival, kivéve, ha az almappa szintjén felülbírálva.  Például beállítása alapértelmezett time-to-live a legfelső szintű, hogy az összes statikus tartalommá gyorsítótárban való 3 nap, de azt az almappát, amelynek 6 órával gyorsítótár beállítása több változó tartalom van.  A **következő**a webhelyen az összes alkalmazás hatással lesz, de az alkalmazások **web.config** fájlok felülírható.

Az alábbi XML-mutatja és példa beállítás **clientCache** 3 nap maximális kort megadásához:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

**UseMaxAge** megadása hozzáadása egy `Cache-Control: max-age=<nnn>` fejléc, a válasz a **CacheControlMaxAge** attribútumban megadott érték alapján. Az időszak formátuma az **cacheControlMaxAge** attribútum nem <days>. <hours>:<min>:<sec>. A **clientCache** csomópontra további tudnivalókért lásd: [ügyfél gyorsítótárának <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-in-code"></a>Gyorsítótár-vezérlő fejlécek beállítása a kódot.

ASP.NET-alkalmazásokhoz beállíthatja a CDN-gyorsítótárazás viselkedése programozás útján **HttpResponse.Cache** tulajdonság beállításával. A **HttpResponse.Cache** tulajdonság kapcsolatos további tudnivalókért olvassa el a [HttpResponse.Cache tulajdonság](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) és [HttpCachePolicy osztály](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx)című témakört.  

Ha szeretne programozás útján gyorsítótár-alkalmazás tartalmának ASP.NET, győződjön meg arról, hogy a tartalom van megjelölve cacheable *nyilvános*HttpCacheability megadásával. Is győződjön meg arról, hogy a gyorsítótár érvényesítő van-e beállítva. A gyorsítótár érvényesítő lehet az utolsó módosítás hívja fel a SetETag beállítása időbélyegző, hívja fel a SetLastModified vagy etag értéket ad meg. Tetszés szerint SetExpires hívja fel a gyorsítótár elévülési időt is megadhatja, vagy az alapértelmezett gyorsítótár heurisztikus a dokumentum korábbi leírt számíthat.  

Ha például tartalom egy órával a gyorsítótár, adja meg a következőket:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a>Következő lépések

- [Ismerje meg részletesebben a **clientCache** elem](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
- [Olvassa el a dokumentáció **HttpResponse.Cache** tulajdonság](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
- [Az **Osztály HttpCachePolicy**dokumentációjában olvasható](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

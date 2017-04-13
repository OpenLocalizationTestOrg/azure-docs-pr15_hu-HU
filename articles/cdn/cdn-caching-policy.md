<properties
    pageTitle="CDN-gyorsítótár-házirend Media Services bővítmény"
    description="Ez a témakör áttekintést nyújt az a CDN gyorsítótárazás Media Services bővítmény házirend."
    services="media-services,cdn"
    documentationCenter=".NET"
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="juliako"/>
 
#<a name="cdn-caching-policy-in-media-services-extension"></a>CDN-gyorsítótár-házirend Media Services bővítmény

Azure Media Services itt HTTP alapú adaptív Streaming és a progresszív letöltési. Adatfolyam-alapú HTTP a proxy és CDN-rétegek, valamint ügyféloldali gyorsítótárazás gyorsítótárazás előnyei erősen méretezhető. A folyamatos átvitelű végpontok Itt általános adatfolyam funkciók, és a HTTP-gyorsítótár fejlécek konfigurációs. A folyamatos átvitelű végpontok állítja be a HTTP-gyorsítótár-vezérlők: max-kor és elévülési élőfejek. További információt a HTTP-gyorsítótár fejlécek amely letölthető [W3.org](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html).

##<a name="default-caching-headers"></a>Alapértelmezett gyorsítótárazás fejlécek

Alapértelmezés szerint a folyamatos átvitelű végpontok 3 nap gyorsítótár fejlécek az igény szerinti adatfolyam (tényleges media töredékek/szövegadattömb) és manifest(playlist) vonatkoznak. Élő streaming adatfolyam végpontok alkalmazása 3 nap gyorsítótár fejlécek az adatokat (tényleges media töredékek/szövegadattömb) és 2 másodpercig gyorsítótár manifest(playlist) kérések fejlécére. Ha az élő program kikapcsolja az igény szerinti (élő archiválási) majd igény szerinti adatfolyam gyorsítótár fejlécek vonatkoznak.

##<a name="azure-cdn-integration"></a>Azure CDN-integráció

Azure Media Services [integrált CDN](https://azure.microsoft.com/updates/azure-media-services-now-fully-integrated-with-azure-cdn/) nyújt a folyamatos átvitelű végpontok. Gyorsítótár-vezérlő fejlécek alkalmazza az adott dokumentumkészletből ugyanúgy CDN adatfolyam végpontok adatfolyam végpontok engedélyezve van. Azure CDN-t használja a folyamatos átvitelű végpont gyorsítótár értékeket adja meg a leírási_idő időt, a belső gyorsítótárazott objektumok beállítva, és ezt az értéket is használja a kézbesítés gyorsítótár fejlécek beállítása. Ha adatfolyam végpontok CDN használata engedélyezve nem ajánlott kis gyorsítótár értékek beállítására. Kis értékeinek beállítása csökkentheti a teljesítmény és csökkentése CDN előnyeit. Gyorsítótár fejlécek kisebb, mint 600 másodperc CDN-adatfolyam végpontok engedélyezett beállítása nem engedélyezett.

>[AZURE.IMPORTANT] **Azure CDN Verizon az**Azure Media Services-integráció a Azure CDN valósítja.  **Azure CDN Akamai az** Azure Media Services használni kíván, be kell [a végpont manuális beállítását](cdn-create-new-endpoint.md).  Azure CDN-szolgáltatásokkal kapcsolatos további tudnivalókért olvassa el a [CDN – áttekintés](cdn-overview.md)című témakört.

##<a name="configuring-cache-headers-with-azure-media-services"></a>Az Azure Media Services konfigurálása a gyorsítótár fejlécek

Azure felügyeleti portál vagy az Azure Media Services API-khoz segítségével állíthatja be a gyorsítótár élőfej értékét.

1. Beépülő modul használatával gyorsítótár fejlécek konfigurálása portál olvassa el [a folyamatos átvitelű végpontok kezelése](../media-services/media-services-portal-manage-streaming-endpoints.md) című szakaszt a folyamatos átvitelű végpont beállítása.
2. Azure Media Services REST API-t, [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx#StreamingEndpointCacheControl).
3. Azure Media Services .NET SDK, [StreamingEndpointCacheControl tulajdonságait](http://go.microsoft.com/fwlink/?LinkId=615302).

##<a name="cache-configuration-precedence-order"></a>Gyorsítótár konfigurációs elsőbbségi sorrend

1. Azure Media Services beállított gyorsítótár érték felülbírálja az alapértelmezett értéket.
2. Ha nincs manuális konfiguráció, alapértékeinek vonatkozik.
3. És alapértelmezett 2 másodpercig gyorsítótár fejlécek vonatkozik függetlenül az Azure Media vagy Azure tároló konfigurációs adatfolyam manifest(playlist) live felülbírálása az érték nem érhető el.

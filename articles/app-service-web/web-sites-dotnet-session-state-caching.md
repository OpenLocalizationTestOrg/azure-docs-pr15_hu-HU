<properties 
    pageTitle="Azure vgx.dll gyorsítótár Azure App szolgáltatásban a munkamenet-állapot" 
    description="Megtudhatja, hogy miként ASP.NET munkamenet állam gyorsítótárazás támogatja az Azure gyorsítótár-szolgáltatás használatával." 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="Rick-Anderson" 
    manager="wpickett" 
    editor="none"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="get-started-article" 
    ms.date="06/27/2016" 
    ms.author="riande"/>


# <a name="session-state-with-azure-redis-cache-in-azure-app-service"></a>Azure vgx.dll gyorsítótár Azure App szolgáltatásban a munkamenet-állapot


Ez a témakör ismerteti a munkamenet-állapota az Azure vgx.dll gyorsítótár-szolgáltatás használata.

Ha az ASP.NET-webalkalmazást a munkamenet-állapot használ, szüksége lesz egy külső munkamenet állam szolgáltató (vgx.dll gyorsítótár-szolgáltatás vagy SQL Server munkamenet állapot-szolgáltató) beállítása. Használja a munkamenet-állapotot, és nem használja egy külső-szolgáltatóval, fogja korlátozott egyik példányán a web App alkalmazásban. A vgx.dll gyorsítótár-szolgáltatás a leggyorsabban és legegyszerűbben ahhoz, hogy.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

##<a id="createcache"></a>A gyorsítótár létrehozása
Kövesse [ezeket az utasításokat](../cache-dotnet-how-to-use-azure-redis-cache.md#create-cache) a gyorsítótár létrehozásához.

##<a id="configureproject"></a>A RedisSessionStateProvider NuGet csomag felvétele a web App alkalmazásba
Telepítse a NuGet `RedisSessionStateProvider` csomagot.  A következő parancsot használja a csomag manager konzolról telepítéséhez (**eszközök** > **NuGet csomag Manager** > **Csomag kezelője konzol**):

  `PM> Install-Package Microsoft.Web.RedisSessionStateProvider`
  
Az **eszközök**telepítése > **NuGet csomag Manager** > **NugGet csomagok kezelése megoldás**, keressen a `RedisSessionStateProvider`.

További információt talál az [NuGet RedisSessionStateProvider lapra](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/ ) , és [a gyorsítótár-ügyfél konfigurálása](../cache-dotnet-how-to-use-azure-redis-cache.md#NuGet).

##<a id="configurewebconfig"></a>Módosítsa a fájlt
Nemcsak a gyorsítótár összeállítás hivatkozások elvégzése a NuGet csomag helyettes tételek hozzáadása *a fájlt* . 

1. Nyissa meg a *web.config* , és keresse meg a az **sessionState** elemet.

1. Írja be az értékeket az `host`, `accessKey`, `port` (az SSL port 6380 legyen), és adja meg `SSL` való `true`. Ezeket az értékeket az [Azure-portálon](http://go.microsoft.com/fwlink/?LinkId=529715) a lap a gyorsítótár-példány szerezheti be. További tudnivalókért olvassa el a [Csatlakozás a gyorsítótárhoz](../cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache). Figyelje meg, hogy az-SSL port le van tiltva, alapértelmezés szerint az új gyorsítótárát. Az-SSL port engedélyezésével kapcsolatos további tudnivalókért című [Az Access-portok](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) a [konfigurálása az Azure vgx.dll gyorsítótárban gyorsítótárat](https://msdn.microsoft.com/library/azure/dn793612.aspx) témakörben. A következő Korrektúra nézetben *a fájlt, konkrétan a *port*, *host*, accessKey megváltozik* a módosítások*, és *ssl *.

          <system.web>;
            <customErrors mode="Off" />;
            <authentication mode="None" />;
            <compilation debug="true" targetFramework="4.5" />;
            <httpRuntime targetFramework="4.5" />;
            <sessionState mode="Custom" customProvider="RedisSessionProvider">;
              <providers>;  
                  <!--<add name="RedisSessionProvider" 
                    host = "127.0.0.1" [String]
                    port = "" [number]
                    accessKey = "" [String]
                    ssl = "false" [true|false]
                    throwOnError = "true" [true|false]
                    retryTimeoutInMilliseconds = "0" [number]
                    databaseId = "0" [number]
                    applicationName = "" [String]
                  />;-->;
                 <add name="RedisSessionProvider" 
                      type="Microsoft.Web.Redis.RedisSessionStateProvider" 
                      port="6380"
                      host="movie2.redis.cache.windows.net" 
                      accessKey="m7PNV60CrvKpLqMUxosC3dSe6kx9nQ6jP5del8TmADk=" 
                      ssl="true" />;
              <!--<add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false" />;-->;
              </providers>;
            </sessionState>;
          </system.web>;


##<a id="usesessionobject"></a>A munkafolyamat-objektum kód használata
Az utolsó lépésként elkezdheti használni a ASP.NET-kódot a munkamenet-objektumot. Objektumok hozzáadása a munkamenet-állapot **Session.Add** módszerrel. Ez a módszer elemek tárolására munkamenet állam gyorsítótár kulcs – érték párokká használja.

    string strValue = "yourvalue";
    Session.Add("yourkey", strValue);

A következő kódot beolvassa a munkamenet-állapot ezt az értéket.

    object objValue = Session["yourkey"];
    if (objValue != null)
       strValue = (string)objValue; 

A gyorsítótár-objektumok vgx.dll gyorsítótár a webalkalmazásban is használhatja. További című témakörben [MVC film alkalmazás az Azure vgx.dll gyorsítótár 15 perces](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/).
ASP.NET-munkamenet állapot használatáról további információt a [ASP.NET munkamenet állam áttekintése][]című témakörben találhat.

>[AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók kezdéshez Azure alkalmazás szolgáltatással, nyissa meg a [Próbálja alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=523751), ahol azonnal létrehozhat egy rövid életű starter web app alkalmazás szolgáltatásban. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

## <a name="whats-changed"></a>Mi változott
* Útmutató a módosítása a webhelyekre alkalmazás szolgáltatás című: [Azure alkalmazás szolgáltatás, és a hatás a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

  *[Rich Anderson](https://twitter.com/RickAndMSFT) szerint*
  
  [installed the latest]: http://www.windowsazure.com/downloads/?sdk=net  
  [ASP.NET-munkamenet állapot – áttekintés]: http://msdn.microsoft.com/library/ms178581.aspx

  [NewIcon]: ./media/web-sites-dotnet-session-state-caching/CacheScreenshot_NewButton.png
  [NewCacheDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CreateOptions.png
  [CacheIcon]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheIcon.png
  [NuGetDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_NuGet.png
  [OutputConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_OC_WebConfig.png
  [CacheConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheConfig.png
  [EndpointURL]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_EndpointURL.png
  [ManageKeys]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_ManageAccessKeys.png
 

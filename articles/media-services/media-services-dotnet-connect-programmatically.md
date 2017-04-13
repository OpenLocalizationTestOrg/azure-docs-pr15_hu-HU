<properties 
    pageTitle="Kapcsolódás a Media Services-fiók használatával a .NET rendszerhez" 
    description="Ez a témakör bemutatja, hogyan csatlakozhat a Media Services uisng .NET." 
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
    ms.date="09/26/2016"
    ms.author="juliako"/>


# <a name="connecting-to-media-services-account-using-media-services-sdk-for-net"></a>Kapcsolódás a Media Services-fiók használatával Media Services SDK a .NET rendszerhez

> [AZURE.SELECTOR]
- [TÖBBI](media-services-rest-connect-programmatically.md)
- [.NET](media-services-dotnet-connect-programmatically.md)


Ez a témakör a Microsoft Azure Media Services programozott kapcsolat juthat, amikor a .NET rendszerhez programozási a Media Services SDK a ismerteti.


## <a name="connecting-to-media-services"></a>Kapcsolódás a Media Services

Programozás útján Media Services szeretne csatlakozni, meg kell van korábban Azure fiók beállítása, konfigurált Media Services, a fiók, és majd beállítása a Media Services SDK fejlesztési Visual Studio projekt a .NET rendszerhez. További tudnivalókért lásd: beállítása fejlesztési együtt a Media Services SDK a .NET rendszerhez.

A Media Services fiók telepítési folyamatot végén szerezte be az alábbi kapcsolathoz szükséges értékeket. Használhatja, hogy a Media Services programozott kapcsolatok.

- A Media Services fiók nevére.

- A Media Services fiókkulcs.

Ha ezeket az értékeket, az Azure felügyeleti portált, jelölje ki a Media szolgáltatás fiókját, és kattintson a portál ablakának alján lévő "**Kulcsok kezelése**" ikonra. A minden szövegmező melletti ikonra kattintva másolja át az értéket, a rendszer a vágólapra.


## <a name="creating-a-cloudmediacontext-instance"></a>CloudMediaContext példány létrehozása

Indítsa el a Media Services ellen programozási szüksége **CloudMediaContext** , amely a kiszolgáló helyi példány létrehozása. A **CloudMediaContext** fontos gyűjtemények feladatok, eszközök, fájlokat, az access-házirendek és Locator mutató hivatkozásokat tartalmaz.

>[AZURE.NOTE] A **CloudMediaContext** osztály nem biztonságos szál. Hozzon létre egy új CloudMediaContext szálon vagy egy műveletek.


CloudMediaContext öt konstruktor túlterhelések tartalmaz. Konstruktorok használatát, amelyek **MediaServicesCredentials** paraméterként használata ajánlott. További tudnivalókért lásd: a **Újbóli felhasználása diagramsablonok Access vezérlő szolgáltatás tokenek** követi. 

Az alábbi példában a nyilvános CloudMediaContext(MediaServicesCredentials credentials) konstruktor:

    // _cachedCredentials and _context are class member variables. 
    _cachedCredentials = new MediaServicesCredentials(
                    _mediaServicesAccountName,
                    _mediaServicesAccountKey);
    
    _context = new CloudMediaContext(_cachedCredentials);


## <a name="reusing-access-control-service-tokens"></a>Az Access vezérlő szolgáltatás tokenek újrafelhasználása

Ez a szakasz újbóli használata az Access-vezérlő szolgáltatást tokenek CloudMediaContext konstruktorok használatát, amelyek paraméterként MediaServicesCredentials használatával mutatja be.


[Azure Active Directory hozzáférés-vezérlés](https://msdn.microsoft.com/library/hh147631.aspx) (más néven vezérlő szolgáltatást vagy ACS) egy felhőalapú szolgáltatás, amely egyszerűvé hitelesítése és engedélyezése a felhasználóknak, hogy elérjék a webalkalmazásokban. Microsoft Azure Media Services elérését szabályozza szolgáltatásai azonban egy ACS jogkivonat igénylő OAuth protokoll. Media Services engedélyezési Server kapja meg a ACS tokenek.

A Media Services SDK fejlesztésekor választva nem foglalkozik azzal a tokenek, mert a SDK kód menedzserek azokat meg. Azonban a teljes mértékben kezelése a ACS tokenek SDK engedélyezem vezet szükségtelen jogkivonat kérések. Tokenek kérésének időt vesz igénybe, és az ügyfél- és kiszolgálóoldali erőforrásokat fogyaszt. A ACS kiszolgáló lehetővé túl nagy a ráta esetén a kérelmek. A korlát kérésre 30 másodpercenként, [ACS Szolgáltatáselérhetőség](https://msdn.microsoft.com/library/gg185909.aspx) talál további információt.

A Media Services SDK 3.0.0.0-s kezdve, így újból felhasználhatja a ACS tokenek. A **CloudMediaContext** konstruktorok használatát, amelyek **MediaServicesCredentials** paraméterként engedélyezze a megosztást, a ACS tokenek több környezet közötti. A MediaServicesCredentials osztály beágyazza a Media Services hitelesítő adatokat. Ha egy ACS jogkivonat érhető el, és a lejárati ideje ismert, hozzon létre egy új MediaServicesCredentials példány a jogkivonat, és adja át CloudMediaContext konstruktorának. Figyelje meg, hogy a Media Services SDK automatikusan frissíti tokenek, valahányszor járnak. Kétféleképpen vezérlősablonok ACS tokenek, az alábbi példában látható módon.

- A memória (például a statikus osztály változóban) **MediaServicesCredentials** objektumra is gyorsítótár. A gyorsítótárban tárolt objektum majd átadni a CloudMediaContext konstruktor. A MediaServicesCredentials objektum egy ACS jogkivonat felhasználhassa, ha a továbbra is érvényes, amely tartalmazza. Ha a token nem érvényes, azt frissíti a által a Media Services SDK MediaServicesCredentials konstruktorának megadott hitelesítő adataival.

    Figyelje meg, hogy a **MediaServicesCredentials** objektum kap érvényes token után a RefreshToken nevezik. A **CloudMediaContext** **RefreshToken** módszer a konstruktor felvételét. Ha azt tervezi, jogkivonat értékeket mentése egy külső tárolóval, ellenőrizze, hogy e TokenExpiration érték érvényes jogkivonat adatok mentése előtt. Ha még nem érvényes, hívja fel a RefreshToken gyorsítótárazás előtt.

        // Create and cache the Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);

        
        // Use the cached credentials to create a new CloudMediaContext object.
        if(_cachedCredentials == null)
        {
            _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);
        }
        
        CloudMediaContext context = new CloudMediaContext(_cachedCredentials);

- A AccessToken karakterlánc és a TokenExpiration értékek is gyorsítótár. Az értékek később kell használni, hozhat létre egy új MediaServicesCredentials objektum jogkivonat gyorsítótárazott adatokat tartalmazó.  Az esetek, amikor a token biztonságosan megosztható folyamatok vagy a számítógépek között különösen hasznos.

    A következő kódtöredék hívja fel az ebben a példában nem definiált SaveTokenDataToExternalStorage GetTokenDataFromExternalStorage és UpdateTokenDataInExternalStorageIfNeeded módszerek. Tárolhat, beolvasásához, és egy külső tárolóval jogkivonat adatainak módszerekben adható meg. 

        CloudMediaContext context1 = new CloudMediaContext(_mediaServicesAccountName, _mediaServicesAccountKey);
        
        // Get token values from the context.
        var accessToken = context1.Credentials.AccessToken;
        var tokenExpiration = context1.Credentials.TokenExpiration;
        
        // Save token values for later use. 
        // The SaveTokenDataToExternalStorage method should check 
        // whether the TokenExpiration value is valid before saving the token data. 
        // If it is not valid, call MediaServicesCredentials’s RefreshToken before caching.
        SaveTokenDataToExternalStorage(accessToken, tokenExpiration);
        
    A mentett jogkivonat értékek segítségével MediaServicesCredentials.


        var accessToken = "";
        var tokenExpiration = DateTime.UtcNow;
        
        // Retrieve saved token values.
        GetTokenDataFromExternalStorage(out accessToken, out tokenExpiration);
        
        // Create a new MediaServicesCredentials object using saved token values.
        MediaServicesCredentials credentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey)
        {
            AccessToken = accessToken,
            TokenExpiration = tokenExpiration
        };
        
        CloudMediaContext context2 = new CloudMediaContext(credentials);

    Abban az esetben, ha a token frissítette a Media Services SDK, frissítse a token példányát. 
    
        if(tokenExpiration != context2.Credentials.TokenExpiration)
        {
            UpdateTokenDataInExternalStorageIfNeeded(accessToken, context2.Credentials.TokenExpiration);
        }
        

- Ha több Media Services-fiók (például a megosztás célra és Geo-eloszlás betöltése), a MediaServicesCredentials objektumok használata a System.Collections.Concurrent.ConcurrentDictionary webhelycsoport (a ConcurrentDictionary gyűjtemény jelöli kulcs/érték párokká is elérhető szálak egyidejűleg szál vannak csoportjára) is gyorsítótárba. A GetOrAdd módszerrel majd kinyeri a gyorsítótárban tárolt hitelesítő adatokat. 

        // Declare a static class variable of the ConcurrentDictionary type in which the Media Services credentials will be cached.  
        private static readonly ConcurrentDictionary<string, MediaServicesCredentials> mediaServicesCredentialsCache = 
            new ConcurrentDictionary<string, MediaServicesCredentials>();
        

        // Cache (or get already cached) Media Services credentials. Use these credentials to create a new CloudMediaContext object.
        static public CloudMediaContext CreateMediaServicesContext(string accountName, string accountKey)
        {
            CloudMediaContext cloudMediaContext;
            MediaServicesCredentials mediaServicesCredentials;
        
            mediaServicesCredentials = mediaServicesCredentialsCache.GetOrAdd(
                accountName,
                valueFactory => new MediaServicesCredentials(accountName, accountKey));
        
            cloudMediaContext = new CloudMediaContext(mediaServicesCredentials);
        
            return cloudMediaContext;
        }
        
## <a name="connecting-to-a-media-services-account-located-in-the-north-china-region"></a>Csatlakozás egy Észak-kínai régióban található Media Services-fiókhoz

A fiók Észak-kínai régióban helyezkedik el, ha használja az alábbi konstruktort:

    public CloudMediaContext(Uri apiServer, string accountName, string accountKey, string scope, string acsBaseAddress)

Példa:


    _context = new CloudMediaContext(
        new Uri("https://wamsbjbclus001rest-hs.chinacloudapp.cn/API/"),
        _mediaServicesAccountName,
        _mediaServicesAccountKey,
        "urn:WindowsAzureMediaServices",
        "https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn");


## <a name="storing-connection-values-in-configuration"></a>Kapcsolat érték tárolása konfigurálása

Kapcsolat értékek, például a fiók nevét és jelszavát, különösen bizalmas értékeket tároló konfigurációban erősen javasolt. Azt is bizalmas konfigurációs adatok titkosítása javasolt. A teljes konfigurációs fájl titkosíthatja a Windows titkosítja fájl rendszer (titkosított fájlrendszer) használatával. Ahhoz, hogy a fájl a titkosított fájlrendszer, kattintson a jobb gombbal a fájlra, válassza a **Tulajdonságok parancsot**, és engedélyezni a titkosítást, a **Speciális** beállítások lapon. Vagy a kijelölt részeire konfigurációs fájl titkosításához védett konfigurációs használatával egyéni megoldást hozhat létre. Lásd: a [titkosított konfigurációs információk segítségével védett konfigurációs](https://msdn.microsoft.com/library/53tyfkaw.aspx).

A következő App.config fájl kapcsolathoz szükséges értékeket tartalmazza. Az értékeket a <appSettings> elem a szükséges értékeket a Media Services fiók telepítés során kapott.

    <configuration>
      <appSettings>
        <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
        <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
      </appSettings>
    </configuration>


Kapcsolat értékek lekérése konfigurációs, használhatja a **ConfigurationManager** osztály és hozzárendelheti a értékek mezők kódban:
    
    private static readonly string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
    private static readonly string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];



##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

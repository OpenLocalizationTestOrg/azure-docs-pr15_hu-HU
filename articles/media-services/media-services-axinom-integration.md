<properties 
    pageTitle="Axinom használatával végzi a Widevine licencek Azure Media Services |} Microsoft Azure" 
    description="Ez a cikk azt ismerteti, hogyan használhatja Azure Media Services (AMS), amely a PlayReady és a Widevine DRMs AMS dinamikusan titkosítja adatfolyam előadásához. A PlayReady licenc Media Services PlayReady licenc kiszolgáló származik, és Widevine licenc hozta Axinom licenc kiszolgáló." 
    services="media-services" 
    documentationCenter="" 
    authors="willzhan" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"   
    ms.author="willzhan;Mingfeiy;rajputam;Juliako"/>

#<a name="using-axinom-to-deliver-widevine-licenses-to-azure-media-services"></a>Axinom használatával végzi a Widevine licencek Azure Media Services  

> [AZURE.SELECTOR]
- [castLabs](media-services-castlabs-integration.md)
- [Axinom](media-services-axinom-integration.md)

##<a name="overview"></a>– Áttekintés

Azure Media Services (AMS) hozzáadta a Google Widevine dinamikus védelmet (lásd: További információ [Mingfei a blog](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/) ). Ezeken kívül Azure Media Player (AMP) is felvette Widevine támogatási (lásd: További információ [AMP dokumentum](http://amp.azure.net/libs/amp/latest/docs/) ). A fő megtalálja köztük a folyamatos átvitelű kötőjel tartalom CENC többfázisú-native-DRM (PlayReady és Widevine) a védett az hangkártyát MSE és EME modern böngészőkben.

A Media Services .NET SDK verzió 3.5.2 kezdve, a Media Services lehetővé teszi Widevine licenc sablon konfigurálása és Widevine licenceket. A következő AMS partnerek segít Widevine licencek előadása is használhatja: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).

Ez a cikk ismerteti, hogyan integráció, és tesztelése a Widevine licenc kiszolgáló Axinom kezeli. Kifejezetten hogy az elfedje:  

- Dinamikus közös titkosítási beállítás a többszörös-DRM (PlayReady és Widevine) megfelelő licenccel WIA URL;
- A JWT jogkivonat létrehozása; licenc kiszolgáló követelmények teljesítése érdekében
- Azure Media Player appot, amely JWT jogkivonat hitelesítéssel; licenc WIA kezeli kialakítása

A részletes rendszerkövetelmények és a folyamat tartalom főbb, kulcs azonosító kulcs rendező, JTW jogkivonat, és a jogcímalapú legjobb leírt az alábbi ábra szerint.

![VONAL és CENC](./media/media-services-axinom-integration/media-services-axinom1.png)

##<a name="content-protection"></a>Tartalomvédelem

Dinamikus védelme és a fő kézbesítési házirend beállítása, olvassa el Mingfei a blog: [Widevine csomagolást az Azure Media Services konfigurálása](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services).

Dinamikus CENC védelmet a többszörös-DRM adhatja meg a problémát a következő két streaming szaggatott:

1. MS Edge és IE11, amelyek egy jogkivonat engedélyezési korlátozások PlayReady védelmét. A token korlátozott házirend mellékelni kell egy által egy biztonságos jogkivonat szolgáltatás (STS), például az Azure Active Directory; kiadott jogkivonat
1. Chrome Widevine védelmét, azt is jogkivonat hitelesítés szükséges a másik STS által kibocsátott jogkivonat. 

[JWT jogkivonat előállítása](media-services-axinom-integration.md#jwt-token-generation) talál miért Azure Active Directory nem lehet használni egy STS Axinom's Widevine licenc Server szakaszát.

###<a name="considerations"></a>Megfontolandó szempontok

1. A megadott Axinom kell használnia kulcs rendező (8888000000000000000000000000000000000000) és a létrehozott vagy a kijelölt főbb azonosító kattintva állítsa elő a fő kézbesítési szolgáltatás konfigurálása tartalom billentyűjét. Az azonos, tesztelése és munkakörnyezeti érvényes kulcs rendező alapján tartalom billentyűk tartalmazó összes licenc Axinom licenc kiszolgáló ki.
1. Widevine licenc WIA URL-címe tesztelése: [https://drm-widevine-licensing.axtest.net/AcquireLicense](https://drm-widevine-licensing.axtest.net/AcquireLicense). HTTP és HTTS is használhatók.

##<a name="azure-media-player-preparation"></a>Azure Media Player előkészítése

AMP v1.4.0 dinamikusan mellékelve PlayReady és a Widevine DRM AMS tartalmának lejátszás támogatja.
Ha Widevine licenc kiszolgáló nem igényel jogkivonat hitelesítés, nincs további kell tennie tesztelése Widevine védett kötőjel tartalom. Példa a AMP csapatának egyszerű [minta](http://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevine_notoken.html), ahol megtekintheti azt a széle és a PlayReady IE11 és Widevine a Chrome használata biztosít.
A Widevine licenc kiszolgáló által Axinom JWT jogkivonat hitelesítést igényel. A JWT jogkivonat kell küldhető licenc kérésével HTTP-fejlécben "X-AxDRM-üzenet" között. Erre a célra, hogy fel kell vennie a következő javascript az weblapon AMP szolgáltatója a forrás beállítása előtt:

    <script>AzureHtml5JS.KeySystem.WidevineCustomAuthorizationHeader = "X-AxDRM-Message"</script>

A többi AMP kód szabványos AMP API AMP dokumentum ahogy [Itt](http://amp.azure.net/libs/amp/latest/docs/).

Ne feledje, hogy a fenti javascript beállítás egyéni engedélyezési fejléc továbbra is a rövid megközelítés AMP hosszú távú megközelítést megjelent hivatalos előtt.

##<a name="jwt-token-generation"></a>JWT jogkivonat előállítása

Axinom Widevine licenc kiszolgáló teszteléshez JWT jogkivonat hitelesítést igényel. Ezenkívül a JWT jogkivonat követelések egyik egy összetett objektumtípus egyszerű adattípus helyett.

Sajnos Azure Active Directory csak abban az esetben JWT tokenek egyszerű kiválasztásával. Hasonlóképpen .NET-keretrendszer API (System.IdentityModel.Tokens.SecurityTokenHandler és JwtPayload) csak lehetővé teszi összetett objektumtípus bemeneti igények szerint. Követelések azonban továbbra is vannak szerializálásának karakterláncként. Ezért a JWT jogkivonat Widevine licenc kérelme létrehozásához a kettő közül nem használjuk.


János Sheehan [JWT Nuget csomag](https://www.nuget.org/packages/JWT) megfelel az igényeinek megfelelően, használja a Nuget csomag fogjuk.

Az alábbi van a teszteléshez Axinom Widevine licenc kiszolgáló által megkövetelt szükséges követelések létrehozása JWT jogkivonat-kódot:

    
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.IdentityModel.Tokens;
    using System.IdentityModel.Protocols.WSTrust;
    using System.Security.Claims;
    
    namespace OpenIdConnectWeb.Utils
    {
        public class JwtUtils
        {
            //using John Sheehan's NuGet JWT library: https://www.nuget.org/packages/JWT/
            public static string CreateJwtSheehan(string symmetricKeyHex, string key_id)
            {
                byte[] symmetricKey = ConvertHexStringToByteArray(symmetricKeyHex);  //hex string to byte[] Note: Note that the key is a hex string, however it must be treated as a series of bytes not a string when encoding.
    
                var payload = new Dictionary<string, object>()
                             {
                                 { "version", 1 },
                                 { "com_key_id", System.Configuration.ConfigurationManager.AppSettings["ax:com_key_id"] },
                                 { "message", new { type = "entitlement_message", key_ids = new string[] { key_id } }  }
                             };
    
                string token = JWT.JsonWebToken.Encode(payload, symmetricKey, JWT.JwtHashAlgorithm.HS256);
    
                return token;
            }
    
            //convert hex string to byte[]
            public static byte[] ConvertHexStringToByteArray(string hexString)
            {
                if (hexString.Length % 2 != 0)
                {
                    throw new ArgumentException(String.Format(System.Globalization.CultureInfo.InvariantCulture, "The binary key cannot have an odd number of digits: {0}", hexString));
                }
    
                byte[] HexAsBytes = new byte[hexString.Length / 2];
                for (int index = 0; index < HexAsBytes.Length; index++)
                {
                    string byteValue = hexString.Substring(index * 2, 2);
                    HexAsBytes[index] = byte.Parse(byteValue, System.Globalization.NumberStyles.HexNumber, System.Globalization.CultureInfo.InvariantCulture);
                }
    
                return HexAsBytes;
            }
    
        }  
    
    }  

Axinom Widevine licenc kiszolgáló

    <add key="ax:laurl" value="http://drm-widevine-licensing.axtest.net/AcquireLicense" />
    <add key="ax:com_key_id" value="69e54088-e9e0-4530-8c1a-1eb6dcd0d14e" />
    <add key="ax:com_key" value="4861292d027e269791093327e62ceefdbea489a4c7e5a4974cc904b840fd7c0f" />
    <add key="ax:keyseed" value="8888000000000000000000000000000000000000" />

###<a name="considerations"></a>Megfontolandó szempontok

1.  Habár AMS PlayReady licenc kézbesítési szolgáltatásnak van szüksége "Bearer =" megelőző egy hitelesítési jogkivonat, Axinom Widevine licenc kiszolgáló nem használja.
2.  A Axinom kommunikációs billentyűt az aláírási kulcs szolgál. Ne feledje, hogy a kulcs hexadecimális karakterlánc, ám akkor kell kezelni bájt karakterlánc sorozata kódolás esetén. Ez a módszer ConvertHexStringToByteArray érhető el.

##<a name="retrieving-key-id"></a>Fő azonosító beolvasása

Láthatta, hogy a kód egy JWT létrehozásához jogkivonat, kulcs azonosító szükség. Mivel a JWT jogkivonat kell lennie előtt betöltése AMP lejátszó, készen áll a főbb azonosító kell JWT jogkivonat készítése érdekében visszaszerezni.

Kulcs első tartása több módon tanfolyam-azonosítóval. Ha például egy tárolhatnak kulcs azonosítója tartalom-metaadatok adatbázisban együtt. Vagy meghallgathatja azonosítója kötőjel MPD (Media bemutató leírás) fájlból. Az alábbi kód ez utóbbi szolgál.

    //get key_id from DASH MPD
    public static string GetKeyID(string dashUrl)
    {
        if (!dashUrl.EndsWith("(format=mpd-time-csf)"))
        {
            dashUrl += "(format=mpd-time-csf)";
        }
    
        XPathDocument objXPathDocument = new XPathDocument(dashUrl);
        XPathNavigator objXPathNavigator = objXPathDocument.CreateNavigator();
        XmlNamespaceManager objXmlNamespaceManager = new XmlNamespaceManager(objXPathNavigator.NameTable);
        objXmlNamespaceManager.AddNamespace("",     "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("ns1",  "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("cenc", "urn:mpeg:cenc:2013");
        objXmlNamespaceManager.AddNamespace("ms",   "urn:microsoft");
        objXmlNamespaceManager.AddNamespace("mspr", "urn:microsoft:playready");
        objXmlNamespaceManager.AddNamespace("xsi",  "http://www.w3.org/2001/XMLSchema-instance");
        objXmlNamespaceManager.PushScope();
    
        XPathNodeIterator objXPathNodeIterator;
        objXPathNodeIterator = objXPathNavigator.Select("//ns1:MPD/ns1:Period/ns1:AdaptationSet/ns1:ContentProtection[@value='cenc']", objXmlNamespaceManager);
    
        string key_id = string.Empty;
        if (objXPathNodeIterator.MoveNext())
        {
            key_id = objXPathNodeIterator.Current.GetAttribute("default_KID", "urn:mpeg:cenc:2013");
        }
    
        return key_id;
    }

##<a name="summary"></a>Összefoglalás

Widevine támogatás az Azure Media Services tartalomvédelmi és az Azure Media Player legújabb hozzáadásával azt a folyamatos átvitelű kötőjel + többfázisú-native-DRM (PlayReady + Widevine) mindkét AMS és Widevine licenc kiszolgálóról Axinom a következő modern böngészőkkel PlayReady licenc szolgáltatással képesek legyenek:

- A Chrome
- A Windows 10 rendszerben Microsoft Edge
- IE Windows 8.1 és a Windows 10 11
- Firefox (asztali) és a (nem iOS) a Mac rendszeren futó Safari szintén támogatottak a Silverlight és az Azure Media Player azonos URL-címe

A következő paraméterek a minimális megoldás emelés Axinom Widevine licenc kiszolgálón van szükség. Kulcs kivételével azonosítója, a többi paraméterek által nyújtott Axinom azok Widevine server telepítése alapján.


Paraméter|Hogyan használják
---|---
Kapcsolati kulcs azonosítója|Meg kell adni a felelős "com_key_id" JWT jogkivonat értéket (lásd az [Ebben](media-services-axinom-integration.md#jwt-token-generation) a szakaszban).
Kapcsolati kulcs|Az aláírási kulcs JWT jogkivonat kell használni (lásd az [Ebben](media-services-axinom-integration.md#jwt-token-generation) a szakaszban).
Fő seed (mag)|Használatával az adott tartalmakat tartalom kulcs generálása azonosítója (lásd az [Ebben](media-services-axinom-integration.md#content-protection) a szakaszban).
Widevine licenc WIA URL-címe|Az eszköz kézbesítési házirend beállítása a vonal folyamatos átvitelű (lásd: [Ez](media-services-axinom-integration.md#content-protection) a szakasz) kell használni.
Tartalom kulcs azonosítója|Jogosultság üzenet állítást JWT jogkivonat értékének szerepelnie kell (lásd az [Ebben](media-services-axinom-integration.md#jwt-token-generation) a szakaszban). 


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

###<a name="acknowledgments"></a>Visszaigazolások 

Azt szeretné, nyugtázza járult felé létrehozni a dokumentumot a következő személyek: a Axinom Kristjan Jõgi Mingfei Pozsony és Amit Rajput.

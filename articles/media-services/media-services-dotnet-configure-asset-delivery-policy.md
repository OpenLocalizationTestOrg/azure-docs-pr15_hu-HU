<properties 
    pageTitle="A digitális eszköz kiválasztása kézbesítési házirendek beállítása a .NET SDK |} Microsoft Azure" 
    description="Ez a témakör bemutatja, hogyan különböző eszköz kézbesítési házirendek beállítása az Azure Media Services .NET SDK." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako;mingfeiy"/>

#<a name="configure-asset-delivery-policies-with-net-sdk"></a>A digitális eszköz kiválasztása kézbesítési házirendek beállítása a .NET SDK
[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

##<a name="overview"></a>– Áttekintés

Ha titkosított kézbesítési eszközökre, az egyik lépés a Media Services tartalomkézbesítési munkafolyamat van konfigurálása kézbesítési házirendek eszközök. Az eszköz kézbesítési házirend alapján Media Services hogyan szeretné az eszközre, kézbesítése: be kell a digitális eszköz kiválasztása lehet dinamikusan csomagolt (az például MPEG kötőjel, HLS, zökkenőmentes Streaming vagy az összes), attól függetlenül, hogy az eszköz dinamikusan titkosítani kívánt, és hogyan mely streaming jegyzőkönyv (a borítékon vagy a közös titkosítási).

Ez a témakör azt ismerteti, hogy miért és hogyan hozhat létre és eszköz kézbesítési házirendek beállítása.

>[AZURE.NOTE]Dinamikus csomagolást és dinamikus titkosítást tudja gondoskodnia arról, hogy legalább egy időosztás egységet (más néven adatfolyam egység). További információért tájékozódhat [skála Media szolgáltatás](media-services-portal-manage-streaming-endpoints.md).
>
>Az eszköz is adaptív átviteli sebesség MP4s vagy adaptív átviteli sebesség zökkenőmentes Streaming fájlok kell tartalmaznia.

Az azonos eszköz sikerült különböző szabályok vonatkoznak. Például PlayReady titkosítási sikerült alkalmazása zökkenőmentes Streaming és AES boríték titkosítási MPEG szaggatott és HLS. Bármely kézbesítési házirend nem meghatározott protokollok (például felvett egy egyetlen házirendet, amely csak a protokolljaként adja meg HLS) letiltja a folyamatos átvitelű. Ez a kivétel ez alól nincs eszköz kézbesítési házirend egyáltalán definiált esetén. Ezt követően törölje minden protokoll engedélyezett.

Ha szeretne egy tároló titkosított eszköz olvasása, meg kell adnia az eszköz kézbesítési házirend. Az eszköz folyamatosan lejátszott is kell, mielőtt a továbbított kiszolgáló eltávolítja a tárterület-titkosítás, és adatfolyamként továbbítja a tartalom, a megadott kézbesítési házirend használata. Például az eszköz speciális Encryption Standard (AES) boríték titkosítási kulcs titkosított előadásához, állítsa a házirend típusa **DynamicEnvelopeEncryption**. Tárterület titkosításának eltávolítása és törlése a eszköz adatfolyam, állítsa a házirend típusa **NoDynamicEncryption**. Az alábbi példák, amelyek bemutatják, hogyan állítsa be ezeket a házirend-típusokat.

Az eszköz kézbesítési házirend beállításaitól függően az esetben is dinamikusan csomagolása, dinamikusan titkosítása és a következő adatfolyam protokollok adatfolyam: folyamatos zökkenőmentes, HLS, MPEG szaggatott és HDS adatfolyam megjelenítését.

A következő lista mutatja a formátumok sima, HLS, szaggatott és HDS adatfolyamként való használható.

Zökkenőmentes Streaming:

{a folyamatos átvitelű végpontjának neve-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS:

{a folyamatos átvitelű végpontjának neve-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG-KÖTŐJEL

{a folyamatos átvitelű végpontjának neve-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

HDS

{a folyamatos átvitelű végpontjának neve-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

Tárgyi eszköz közzététele és adatfolyam URL-cím létrehozása című cikkben olvashat [összeállítása adatfolyam URL-címet](media-services-deliver-streaming-content.md).

##<a name="considerations"></a>Megfontolandó szempontok

- Egy tárgyi eszköz társított, miközben egy (adatfolyam) OnDemand megnevezés létezik az adott eszköz AssetDeliveryPolicy nem törölhetők. Az ajánlási a házirendet eltávolítása az eszköz a házirend törlése előtt.
- Egy adatfolyam megnevezés nem hozható létre a tárhely titkosított eszköz nincs eszköz kézbesítési házirend beállítása esetén.  Ha az eszköz nem titkosított tárhely, a rendszer közli, hozzon létre egy megnevezés, valamint a eszköz törlése nélkül az eszköz kézbesítési házirend-adatfolyam.
- Lehet, hogy több eszköz kézbesítési házirendek egyetlen eszköz társított, de csak adhatja meg lehet egy adott AssetDeliveryProtocol kezelni.  Ami azt jelenti, ha megpróbál adja meg a hibát eredményez, mivel a rendszer nem tudja, amely egy szeretne alkalmazni, amikor egy ügyfél zökkenőmentes Streaming kérelmet AssetDeliveryProtocol.SmoothStreaming protokollnak két kézbesítési házirendek hivatkozás.
- Tárgyi eszköz egy meglévő adatfolyam megnevezés, ha nem tudja csatol egy új házirendet az eszköz (válassza a eszközből meglévő házirend leválasztása, vagy egy kézbesítési házirendet, az eszköz társított frissítése).  Először távolítsa el a továbbított megnevezés, módosítsa a házirendek és hozza létre a továbbított megnevezés van.  Az azonos locatorId is használhatja, amikor a továbbított megnevezés hozza létre ismét, de győződjön meg arról, hogy nem problémákat okoznak ügyfélalkalmazások óta tartalom gyorsítótárba helyezhető az origin vagy a későbbi CDN.


##<a name="clear-asset-delivery-policy"></a>Eszköz törlése kézbesítési házirend

A következő **ConfigureClearAssetDeliveryPolicy** módszer Itt adhatja meg, nem vonatkoznak a dinamikus titkosítást és használhat az előadáshoz az adatfolyam bármely, az alábbi protokollokat: MPEG kötőjel HLS és zavartalan Streaming protokollok. Ez a szabály alkalmazása a titkosított tároló eszközök célszerű.

Milyen értékeket szeretne megadhat egy AssetDeliveryPolicy létrehozásakor című [típusok AssetDeliveryPolicy megadásakor használható](#types) .

statikus nyilvános érvénytelenítése ConfigureClearAssetDeliveryPolicy (IAsset eszköz) {IAssetDeliveryPolicy házirend = _context. AssetDeliveryPolicies.Create ("Törlése házirend" AssetDeliveryPolicyType.NoDynamicEncryption, AssetDeliveryProtocol.HLS |} AssetDeliveryProtocol.SmoothStreaming |} AssetDeliveryProtocol.Dash, null);

a digitális eszköz kiválasztása. DeliveryPolicies.Add(policy); }

##<a name="dynamiccommonencryption-asset-delivery-policy"></a>DynamicCommonEncryption eszköz kézbesítési házirend


A következő **CreateAssetDeliveryPolicy** módszer a **AssetDeliveryPolicy** van konfigurálva, hogy dinamikus közös titkosítást (**DynamicCommonEncryption**) alkalmazása a zökkenőmentes adatfolyam jegyzőkönyv (más protokollok, azzal blokkolhatja a folyamatos átvitelű) hoz létre. A módszer két paraméterrel veszi: **Tárgyi eszköz** (a digitális eszköz kiválasztása, amelyhez hozzá szeretne alkalmazni a kézbesítési házirend) és **IContentKey** (a tartalom billentyűt az **CommonEncryption** típusú, további információért lásd: [tartalom kulcs létrehozása](media-services-dotnet-create-contentkey.md#common_contentkey)).

Milyen értékeket szeretne megadhat egy AssetDeliveryPolicy létrehozásakor című [típusok AssetDeliveryPolicy megadásakor használható](#types) .


statikus nyilvános érvénytelenítése CreateAssetDeliveryPolicy (IAsset eszköz, IContentKey kulcs) {Uri acquisitionUrl = billentyűt. GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

A szótár < AssetDeliveryPolicyConfigurationKey, karakterlánc > assetDeliveryPolicyConfiguration új szótár < AssetDeliveryPolicyConfigurationKey karakterlánc > = {{AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()}};

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.SmoothStreaming,
            assetDeliveryPolicyConfiguration);

        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " +
            assetDeliveryPolicy.AssetDeliveryPolicyType);
    }

Azure Media Services lehetővé teszi a Widevine titkosítás hozzáadása is. A következő példa bemutatja, hogyan PlayReady és hozzáadódjanak az eszköz kézbesítési házirend Widevine is.


    static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        // Get the PlayReady license service URL.
        Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);
        

        // GetKeyDeliveryUrl for Widevine attaches the KID to the URL.
        // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
        // The WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption 
        // to append /? KID =< keyId > to the end of the url when creating the manifest.
        // As a result Widevine license acquisition URL will have KID appended twice, 
        // so we need to remove the KID that in the URL when we call GetKeyDeliveryUrl.

        Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
        UriBuilder uriBuilder = new UriBuilder(widevineUrl);
        uriBuilder.Query = String.Empty;
        widevineUrl = uriBuilder.Uri;

        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
        {
            {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
            {AssetDeliveryPolicyConfigurationKey.WidevineLicenseAcquisitionUrl, widevineUrl.ToString()}

        };

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
            assetDeliveryPolicyConfiguration);


        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

    }

>[AZURE.NOTE]Widevine titkosításához volna csak is használhat az előadáshoz kötőjel használatával. Ellenőrizze, hogy adja meg a vonal eszköz kézbesítési protokoll.


##<a name="dynamicenvelopeencryption-asset-delivery-policy"></a>DynamicEnvelopeEncryption eszköz kézbesítési házirend 

A következő **CreateAssetDeliveryPolicy** módszerrel hoz létre a **AssetDeliveryPolicy** van konfigurálva, hogy dinamikus boríték titkosítási (**DynamicEnvelopeEncryption**) alkalmazása zökkenőmentes Streaming HLS és kötőjel protokollok (Ha úgy dönt, hogy nem adja meg az egyes protokollok, hogy, azzal blokkolhatja a folyamatos átvitelű). A módszer két paraméterrel veszi: **Tárgyi eszköz** (a digitális eszköz kiválasztása, amelyhez hozzá szeretne alkalmazni a kézbesítési házirend) és **IContentKey** (a tartalom billentyűt az **EnvelopeEncryption** típusú, további információért lásd: [tartalom kulcs létrehozása](media-services-dotnet-create-contentkey.md#envelope_contentkey)).


Milyen értékeket szeretne megadhat egy AssetDeliveryPolicy létrehozásakor című [típusok AssetDeliveryPolicy megadásakor használható](#types) .   

    private static void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        
        //  Get the Key Delivery Base Url by removing the Query parameter.  The Dynamic Encryption service will
        //  automatically add the correct key identifier to the url when it generates the Envelope encrypted content
        //  manifest.  Omitting the IV will also cause the Dynamice Encryption service to generate a deterministic
        //  IV for the content automatically.  By using the EnvelopeBaseKeyAcquisitionUrl and omitting the IV, this
        //  allows the AssetDelivery policy to be reused by more than one asset.
        //
        Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        UriBuilder uriBuilder = new UriBuilder(keyAcquisitionUri);
        uriBuilder.Query = String.Empty;
        keyAcquisitionUri = uriBuilder.Uri;

        // The following policy configuration specifies: 
        //   key url that will have KID=<Guid> appended to the envelope and
        //   the Initialization Vector (IV) to use for the envelope encryption.
        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string> 
        {
            {AssetDeliveryPolicyConfigurationKey.EnvelopeBaseKeyAcquisitionUrl, keyAcquisitionUri.ToString()},
        };

        IAssetDeliveryPolicy assetDeliveryPolicy =
            _context.AssetDeliveryPolicies.Create(
                        "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                        AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                        assetDeliveryPolicyConfiguration);

        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " + assetDeliveryPolicy.AssetDeliveryPolicyType);
    }


##<a id="types"></a>Adattípusainak AssetDeliveryPolicy megadásakor

###<a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol 

    /// <summary>
    /// Delivery protocol for an asset delivery policy.
    /// </summary>
    [Flags]
    public enum AssetDeliveryProtocol
    {
        /// <summary>
        /// No protocols.
        /// </summary>
        None = 0x0,

        /// <summary>
        /// Smooth streaming protocol.
        /// </summary>
        SmoothStreaming = 0x1,

        /// <summary>
        /// MPEG Dynamic Adaptive Streaming over HTTP (DASH)
        /// </summary>
        Dash = 0x2,

        /// <summary>
        /// Apple HTTP Live Streaming protocol.
        /// </summary>
        HLS = 0x4,

        /// <summary>
        /// Adobe HTTP Dynamic Streaming (HDS)
        /// </summary>
        Hds = 0x8,

        /// <summary>
        /// Include all protocols.
        /// </summary>
        All = 0xFFFF
    }

###<a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType

    /// <summary>
    /// Policy type for dynamic encryption of assets.
    /// </summary>
    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// The Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption to the asset.
        /// </summary>
        /// 
        NoDynamicEncryption,  

        /// <summary>
        /// Apply Dynamic Envelope encryption.
        /// </summary>
        DynamicEnvelopeEncryption,

        /// <summary>
        /// Apply Dynamic Common encryption.
        /// </summary>
        DynamicCommonEncryption
    }

###<a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

    /// <summary>
    /// Delivery method of the content key to the client.
    /// </summary>
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        /// </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        /// </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        /// </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }

###<a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey

    /// <summary>
    /// Keys used to get specific configuration for an asset delivery policy.
    /// </summary>
    public enum AssetDeliveryPolicyConfigurationKey
    {
        /// <summary>
        /// No policies.
        /// </summary>
        None,

        /// <summary>
        /// Exact Envelope key URL.
        /// </summary>
        EnvelopeKeyAcquisitionUrl,

        /// <summary>
        /// Base key url that will have KID=<Guid> appended for Envelope.
        /// </summary>
        EnvelopeBaseKeyAcquisitionUrl,
        
        /// <summary>
        /// The initialization vector to use for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// The PlayReady License Acquisition Url to use for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// The PlayReady Custom Attributes to add to the PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// The initialization vector to use for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }

##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



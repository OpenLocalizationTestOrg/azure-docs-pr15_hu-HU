<properties 
    pageTitle=".NET ContentKeys létrehozása" 
    description="Megtudhatja, hogy miként hozhat létre, amely biztonságos hozzáférés nyújtása eszközökhöz tartalom billentyűk." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
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


#<a name="create-contentkeys-with-net"></a>.NET ContentKeys létrehozása

> [AZURE.SELECTOR]
- [TÖBBI](media-services-rest-create-contentkey.md)
- [.NET](media-services-dotnet-create-contentkey.md)

Media Services lehetővé teszi, hogy hozhat létre és használhat az előadáshoz titkosított eszközök. Egy **ContentKey** a **digitáliseszköz**-s biztonságos hozzáférést biztosít. 

Amikor létrehoz egy új eszköz (például előtt [tölthet fel fájlokat](media-services-dotnet-upload-files.md)), adhatja meg az alábbi titkosítási beállítások: **StorageEncrypted**, **CommonEncryptionProtected**vagy **EnvelopeEncryptionProtected**. 

Eszközök előadása az ügyfelek számára, akkor [dinamikusan titkosítható eszközök beállítása](media-services-dotnet-configure-asset-delivery-policy.md) a következő két titkosításhoz állított be egy: **DynamicEnvelopeEncryption** vagy **DynamicCommonEncryption**.

Titkosított eszközök kell társítani **ContentKey**s. Ez a cikk ismerteti, hogyan hozhat létre olyan tartalom billentyűt.

>[AZURE.NOTE] Új **StorageEncrypted** eszköz használatával a Media Services .NET SDK létrehozásakor a **ContentKey** automatikusan létrejön, és az eszköz kapcsolódik.

##<a name="contentkeytype"></a>ContentKeyType

Az értékek közül, hogy kell-e beállítva mikor létrehozása a tartalom billentyűt a webhelytartalom-típus. Az alábbi értékek közül lehet választani. 

    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is the default value.</remarks>
        CommonEncryption = 0,

        /// <summary>
        /// Specifies a content key for storage encryption.
        /// </summary>
        StorageEncryption = 1,

        /// <summary>
        /// Specifies a content key for configuration encryption.
        /// </summary>
        ConfigurationEncryption = 2,

        /// <summary>
        /// Specifies a content key for Envelope encryption.  Only used internally.
        /// </summary>
        EnvelopeEncryption = 4
    }

##<a id="envelope_contentkey"></a>A boríték típus ContentKey létrehozása

A következő kódrészletet egy boríték titkosítási típusú tartalom kulcsot hoz létre. Majd a megadott eszköz a kulcs társít.

    static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
    {
        // Create envelope encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.EnvelopeEncryption);

        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int size)
    {
        byte[] randomBytes = new byte[size];
        using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
        {
            rng.GetBytes(randomBytes);
        }

        return randomBytes;
    }

hívás

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);



##<a id="common_contentkey"></a>Gyakori típus ContentKey létrehozása    

A következő kódrészletet egy közös titkosítási típusú tartalom kulcsot hoz létre. Majd a megadott eszköz a kulcs társít.

    static public IContentKey CreateCommonTypeContentKey(IAsset asset)
    {
        // Create common encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.CommonEncryption);

        // Associate the key with the asset.
        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int length)
    {
        var returnValue = new byte[length];

        using (var rng =
            new System.Security.Cryptography.RNGCryptoServiceProvider())
        {
            rng.GetBytes(returnValue);
        }

        return returnValue;
    }
hívás

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<properties 
    pageTitle="Tartalom főbb engedélyezési házirend konfigurálása használatával Media Services .NET SDK |} Microsoft Azure" 
    description="Megtudhatja, hogy miként állítsa be az engedélyezés házirend Media Services .NET SDK használatával tartalom kulcs." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016"
    ms.author="juliako;mingfeiy"/>



# <a name="dynamic-encryption-configure-content-key-authorization-policy"></a>Dinamikus titkosítást: tartalom főbb engedélyezési házirend beállítása

[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

##<a name="overview"></a>– Áttekintés

Microsoft Azure Media Services lehetővé teszi speciális Encryption Standard (AES) (128 bites titkosítási kulcs használatával) vagy [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/)védett MPEG-vonal, a zökkenőmentes Streaming és a HTTP-Live-adatfolyam (HLS) adatfolyamok előadásához. AMS is lehetővé teszi a SZAGGATÁS adatfolyamok Widevine DRM titkosított előadásához. A közös titkosítási (ISO/IEC 23001-7 CENC) specifikációja egy titkosított PlayReady és Widevine is.

Media Services is tartalmaz, amelyhez az ügyfelek kaphatnak AES billentyűk vagy a titkosított tartalom lejátszásához PlayReady/Widevine licencek **Kulcs/licenc kézbesítési szolgáltatásnak** .

Ha azt szeretné, a Media Services tárgyi eszköz titkosítására, kell társítani a titkosítási kulcs (**CommonEncryption** vagy **EnvelopeEncryption**) az eszköz (leírt [ide](media-services-dotnet-create-contentkey.md)), és is beállíthatja az engedélyezési házirendek az billentyű (a jelen cikkben leírt).

Adatfolyam a Windows Media Player kérésekor Media Services használja a megadott kulcs dinamikusan titkosítása a tartalom DRM vagy a AES titkosítást használ. Visszafejteni az adatfolyam, a Windows Media player felkéri a kulcsot a fő kézbesítési szolgáltatásból. Döntse el, vagy sem a felhasználó jogosult a kulcs első, a szolgáltatás kiértékeli a kulcs megadott engedélyezési házirendek.

Media Services támogatja a több módon is, hogy a felhasználók, akik fő kéréseket hitelesítése. A tartalom főbb engedélyezési házirend lehetnek egy vagy több engedélyezési korlátozások: **Nyissa meg** vagy **jogkivonat** korlátozás. A token korlátozott házirend által egy biztonságos jogkivonat szolgáltatás (STS) kiadott token mellékelni kell. Media Services tokenek támogatja az **Egyszerű webes tokenek** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) és **JSON webes jogkivonat** ([JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3)) formátum.

Media Services nem biztonságos jogkivonat szolgáltatást biztosít. Hozzon létre egy egyéni STS, vagy a Microsoft Azure ACS kihasználhatja a probléma tokenek. A STS kell beállítania, hogy hozzon létre egy jogkivonat, a megadott billentyű és a probléma követelések, amely a token korlátozás konfiguráció (ebben a cikkben leírt) megadott aláírva. Ha a token érvényes, és a token követelések megegyeznek a tartalom kulcsot be van állítva a Media Services fő kézbesítési szolgáltatás a titkosítási kulcs ad vissza az ügyfél számára.

További tudnivalókért lásd:

[JWT jogkivonat authenitcation](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Azure Media Services OWIN MVC integráció alapú alkalmazást és az Azure Active Directory és JWT jogcímeken alapuló kulcs tartalomkézbesítési korlátozása](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[A probléma tokenek használata az Azure ACS](http://mingfeiy.com/acs-with-key-services).

###<a name="some-considerations-apply"></a>Néhány érvényesek:

- Dinamikus csomagolást és dinamikus titkosítást tudja gondoskodnia arról, hogy legalább egy, a folyamatos átvitelű fenntartott egység. További információért tájékozódhat [skála Media szolgáltatás](media-services-portal-manage-streaming-endpoints.md).
- Az eszköz adaptív átviteli sebesség MP4s vagy adaptív átviteli sebesség zökkenőmentes Streaming fájlok kell tartalmaznia. További tudnivalókért lásd: [Tárgyi eszköz kódolása](media-services-encode-asset.md).
- Töltse fel és kódolását **AssetCreationOptions.StorageEncrypted** beállítással eszközeit.
- Ha szeretné, hogy ugyanazt a házirend konfigurációt igénylő több tartalom kulcsok, erősen ajánlott hozzon létre egy egyetlen engedély házirendet, és felhasználhatja több tartalom billentyűkkel.
- A kulcs kézbesítési szolgáltatás a 15 perces ContentKeyAuthorizationPolicy és a kapcsolódó objektumainak (házirend-beállításokat, és korlátozásokat) gyorsítótárban.  Hozzon létre egy ContentKeyAuthorizationPolicy, és adja meg a "Jogkivonat" korlátozás használata, ha tesztelje azt, és kattintson a házirend módosítása a "Megnyitás" korlátozás, hosszabb nagyjából 15 percet, mielőtt a házirend – Váltás a szabály a "Megnyitás" verzióra.
- Ha szeretne hozzáadni, vagy a digitáliseszköz-kézbesítési házirend módosítására, törölje a egy meglévő Megnevezés (ha van ilyen), és hozzon létre egy új megnevezés.
- Jelenleg nem titkosíthatja streaming format HDS, vagy az progresszív.


##<a name="aes-128-dynamic-encryption"></a>Dinamikus titkosítást AES-128

###<a name="open-restriction"></a>Nyissa meg a korlátozás

Megnyitott korlátozás azt jelenti, hogy a rendszer fog kézbesítése a kulcs bárki, aki kulcs kérést. Ez a korlátozástípus akkor lehet hasznos, tesztelés céljából.

Az alábbi példa létrehoz egy megnyitott engedélyezési házirendet, és hozzáadja őket a tartalom billentyűt.

statikus nyilvános érvénytelenítése AddOpenAuthorizationPolicy (IContentKey contentKey) {/ / ContentKeyAuthorizationPolicy létrehozása, megnyitása korlátozásokkal / / és engedélyezési házirend IContentKeyAuthorizationPolicy házirend létrehozása = _context.
ContentKeyAuthorizationPolicies.
CreateAsync ("Megnyitás engedélyezési házirend"). Eredmény;

Lista<ContentKeyAuthorizationPolicyRestriction> korlátozások új lista =<ContentKeyAuthorizationPolicyRestriction>();
    
        ContentKeyAuthorizationPolicyRestriction restriction =
            new ContentKeyAuthorizationPolicyRestriction
            {
                Name = "HLS Open Authorization Policy",
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                Requirements = null // no requirements needed for HLS
            };
    
        restrictions.Add(restriction);
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
            "policy", 
            ContentKeyDeliveryType.BaselineHttp, 
            restrictions, 
            "");
    
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
    }


###<a name="token-restriction"></a>Jogkivonat korlátozás

Ez a szakasz tartalom főbb engedélyezési házirend létrehozása, és a tartalom kulcs társítása ismerteti. A hitelesítési házirend ismerteti, hogy milyen hitelesítési követelményeket határozza meg, ha a felhasználó jogosult a kulcs (például hatással van a "ellenőrzés billentyűje" listát tartalmaz a kulcs, amely a token van aláírva) teljesülnie kell.

Állítsa be a token korlátozása beállítást, szüksége XML használatával ismertetik a token hitelesítési követelményeket. Az alábbi XML-séma meg kell felelnie a token korlátozás konfiguráció XML.

####<a id="schema"></a>Jogkivonat korlátozás séma
    
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:complexType name="TokenClaim">
        <xs:sequence>
          <xs:element name="ClaimType" nillable="true" type="xs:string" />
          <xs:element minOccurs="0" name="ClaimValue" nillable="true" type="xs:string" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenClaim" nillable="true" type="tns:TokenClaim" />
      <xs:complexType name="TokenRestrictionTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AlternateVerificationKeys" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
          <xs:element name="Audience" nillable="true" type="xs:anyURI" />
          <xs:element name="Issuer" nillable="true" type="xs:anyURI" />
          <xs:element name="PrimaryVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
          <xs:element minOccurs="0" name="RequiredClaims" nillable="true" type="tns:ArrayOfTokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenRestrictionTemplate" nillable="true" type="tns:TokenRestrictionTemplate" />
      <xs:complexType name="ArrayOfTokenVerificationKey">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenVerificationKey" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
      <xs:complexType name="TokenVerificationKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
      <xs:complexType name="ArrayOfTokenClaim">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenClaim" nillable="true" type="tns:TokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenClaim" nillable="true" type="tns:ArrayOfTokenClaim" />
      <xs:complexType name="SymmetricVerificationKey">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:TokenVerificationKey">
            <xs:sequence>
              <xs:element name="KeyValue" nillable="true" type="xs:base64Binary" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="SymmetricVerificationKey" nillable="true" type="tns:SymmetricVerificationKey" />
    </xs:schema>

Ha házirend beállítása a **token** korlátozott, meg kell adnia a elsődleges** kulcs ellenőrzése**, a **kibocsátó** és a **közönség** paraméterek. Az **ellenőrzés elsődleges kulcsot **, tartalmazza, a kulcs, amely a token van aláírva **kibocsátó** a biztonságos jogkivonat-szolgáltatás, amely a token problémák. A **célközönség** (más néven **hatókör**) a token szándékának mutatja be, vagy az erőforrás a token engedélyezi a hozzáférést. A Media Services fő kézbesítési szolgáltatás ellenőrzi, hogy ezeket az értékeket a token értékekre a sablonban. 

Amikor **Media Services SDK a .NET rendszerhez**, a korlátozás jogkivonat létrehozásához **TokenRestrictionTemplate** osztály is használhatja.
A következő példa házirendet hoz létre engedélyezési jogkivonat korlátozását. Ebben a példában az ügyfél szeretné, hogy bemutatására jogkivonat, amely tartalmaz: az aláíró kulcs (VerificationKey), a jogkivonat-kibocsátó és a szükséges követelések.
    
    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();
    
        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                new List<ContentKeyAuthorizationPolicyRestriction>();
    
        ContentKeyAuthorizationPolicyRestriction restriction =
                new ContentKeyAuthorizationPolicyRestriction
                {
                    Name = "Token Authorization Policy",
                    KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                    Requirements = tokenTemplateString
                };
    
        restrictions.Add(restriction);
    
        //You could have multiple options 
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
                "Token option for HLS",
                ContentKeyDeliveryType.BaselineHttp,
                restrictions,
                null  // no key delivery data is needed for HLS
                );
    
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
    
        return tokenTemplateString;
    }
    
    static private string GenerateTokenRequirements()
    {
        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);
    
        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();
    
        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);
    
        return TokenRestrictionTemplateSerializer.Serialize(template);
    }

####<a id="test"></a>Teszt jogkivonat

Ha a teszt jogkivonat, a token korlátozást, a fő engedélyezési házirend használt alapján, tegye a következőket:
    
    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
    
    // Generate a test token based on the the data in the given TokenRestrictionTemplate.
    // Note, you need to pass the key id Guid because we specified 
    // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
    Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
    
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
    Console.WriteLine();


##<a name="playready-dynamic-encryption"></a>Dinamikus PlayReady titkosítás: 

Media Services lehetővé teszi, hogy állítsa be a tartalomvédelmi és korlátozásokat kívánt a PlayReady DRM futtatókörnyezet meg lehessen őrizni a felhasználó lejátszásához védett tartalmat közben. 

Amikor a tartalom PlayReady védheti, a következőkre lesz szüksége a hitelesítési házirend megadása egyik XML-karakterlánc, amely definiálja az [PlayReady licenc sablont](media-services-playready-license-template-overview.md). A Media Services SDK a .NET rendszerhez a **PlayReadyLicenseResponseTemplate** és **PlayReadyLicenseTemplate** osztályok segít a PlayReady licenc sablon.

[Ez a témakör](media-services-protect-with-drm.md) bemutatja, hogyan titkosítása **PlayReady** és **Widevine**a tartalmakat.

###<a name="open-restriction"></a>Nyissa meg a korlátozás
    
Megnyitott korlátozás azt jelenti, hogy a rendszer fog kézbesítése a kulcs bárki, aki kulcs kérést. Ez a korlátozástípus akkor lehet hasznos, tesztelés céljából.

Az alábbi példa létrehoz egy megnyitott engedélyezési házirendet, és hozzáadja őket a tartalom billentyűt.

    static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
    {
    
        // Create ContentKeyAuthorizationPolicy with Open restrictions 
        // and create authorization policy          
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Open", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open, 
                Requirements = null
            }
        };
    
        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);
    
        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;
    
    
        contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
        // Associate the content key authorization policy with the content key.
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    }

###<a name="token-restriction"></a>Jogkivonat korlátozás

Állítsa be a token korlátozása beállítást, szüksége XML használatával ismertetik a token hitelesítési követelményeket. Meg kell felelnie a token korlátozás konfiguráció XML az XML-sémát, [Ebben](#schema) a szakaszban látható.
    
    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();
    
        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Token Authorization Policy", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                Requirements = tokenTemplateString, 
            }
        };
    
        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);
    
        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;
                
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
        // Associate the content key authorization policy with the content key
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    
        return tokenTemplateString;
    }
    
    static private string GenerateTokenRequirements()
    {
    
        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);
    
        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();
    
    
        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);
    
        return TokenRestrictionTemplateSerializer.Serialize(template);
    } 
    
    static private string ConfigurePlayReadyLicenseTemplate()
    {
        // The following code configures PlayReady License Template using .NET classes
        // and returns the XML string.

        //The PlayReadyLicenseResponseTemplate class represents the template for the response sent back to the end user. 
        //It contains a field for a custom data string between the license server and the application 
        //(may be useful for custom app logic) as well as a list of one or more license templates.
        PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();

        // The PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
        // to be returned to the end users. 
        //It contains the data on the content key in the license and any rights or restrictions to be 
        //enforced by the PlayReady DRM runtime when using the content key.
        PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
        //Configure whether the license is persistent (saved in persistent storage on the client) 
        //or non-persistent (only held in memory while the player is using the license).  
        licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;
       
        // AllowTestDevices controls whether test devices can use the license or not.  
        // If true, the MinimumSecurityLevel property of the license
        // is set to 150.  If false (the default), the MinimumSecurityLevel property of the license is set to 2000.
        licenseTemplate.AllowTestDevices = true;


        // You can also configure the Play Right in the PlayReady license by using the PlayReadyPlayRight class. 
        // It grants the user the ability to playback the content subject to the zero or more restrictions 
        // configured in the license and on the PlayRight itself (for playback specific policy). 
        // Much of the policy on the PlayRight has to do with output restrictions 
        // which control the types of outputs that the content can be played over and 
        // any restrictions that must be put in place when using a given output.
        // For example, if the DigitalVideoOnlyContentRestriction is enabled, 
        //then the DRM runtime will only allow the video to be displayed over digital outputs 
        //(analog video outputs won’t be allowed to pass the content).

        //IMPORTANT: These types of restrictions can be very powerful but can also affect the consumer experience. 
        // If the output protections are configured too restrictive, 
        // the content might be unplayable on some clients. For more information, see the PlayReady Compliance Rules document.

        // For example:
        //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);

        responseTemplate.LicenseTemplates.Add(licenseTemplate);

        return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
    }


A próba jogkivonat, a token korlátozást, a fő hitelesítéshez használt alapján házirend [Ebben](#test) a szakaszban megtekinteni. 

##<a id="types"></a>Adattípusainak ContentKeyAuthorizationPolicy megadásakor

###<a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType

    public enum ContentKeyRestrictionType
    {
        Open = 0,
        TokenRestricted = 1,
        IPRestricted = 2,
    }

###<a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

    public enum ContentKeyDeliveryType
    {
      None = 0,
      PlayReadyLicense = 1,
      BaselineHttp = 2,
      Widevine = 3
    }

###<a id="TokenType"></a>TokenType

    public enum TokenType
    {
        Undefined = 0,
        SWT = 1,
        JWT = 2,
    }



##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-step"></a>Következő lépés
Most, hogy a tartalom kulcs engedélyezési házirend állította be, nyissa meg az [eszköz kézbesítési házirend beállítása](media-services-dotnet-configure-asset-delivery-policy.md) témakörre.
 

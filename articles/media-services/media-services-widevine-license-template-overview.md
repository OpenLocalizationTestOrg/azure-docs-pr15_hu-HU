<properties 
    pageTitle="Widevine licenc sablon áttekintése |} Microsoft Azure" 
    description="Ez a témakör áttekintést Widevine licencek konfigurálása használt Widevine licenc sablon." 
    authors="juliako" 
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
    ms.date="09/26/2016"  
    ms.author="juliako"/>

#<a name="widevine-license-template-overview"></a>Widevine licenc sablon – áttekintés

##<a name="overview"></a>– Áttekintés

Azure Media Services most funkcióval konfigurálása és kérése Widevine licenceket. Amikor megpróbálja a végfelhasználói Windows Media player játssza le a védett Widevine tartalmakat, kérelem érkezik a licenc kézbesítési szolgáltatás licencet. A szolgáltatás a kérelem jóváhagyása, általa a licenc, amely küldi el az ügyfélnek és visszafejteni, és játssza le a megadott tartalom is használható.

Widevine licenc kérését JSON üzenetként van formázva.  

Ne feledje, hogy választva hozzon létre egy üres üzenetet értékkel nem rendelkező csak "{}" licenc sablon létrejön az összes alapértelmezett beállításokkal.  

    {  
       “payload”:“<license challenge>”,
       “content_id”: “<content id>” 
       “provider”: ”<provider>”
       “allowed_track_types”:“<types>”,
       “content_key_specs”:[  
          {  
             “track_type”:“<track type 1>”
          },
          {  
             “track_type”:“<track type 2>”
          },
          …
       ],
       “policy_overrides”:{  
          “can_play”:<can play>,
          “can persist”:<can persist>,
          “can_renew”:<can renew>,
          “rental_duration_seconds”:<rental duration>,
          “playback_duration_seconds”:<playback duration>,
          “license_duration_seconds”:<license duration>,
          “renewal_recovery_duration_seconds”:<renewal recovery duration>,
          “renewal_server_url”:”<renewal server url>”,
          “renewal_delay_seconds”:<renewal delay>,
          “renewal_retry_interval_seconds”:<renewal retry interval>,
          “renew_with_usage”:<renew with usage>
       }
    }

##<a name="json-message"></a>JSON-üzenet

név | Érték | Leírás
---|---|---
tartalom |Base64-címként kódolt karakterláncot |A licenc kérelem egy ügyfél által küldött. 
content_id | Base64-címként kódolt karakterláncot|Használható KeyId(s) és tartalom billentyű(k) az egyes content_key_specs.track_type azonosítója.
szolgáltató |karakterlánc |Alkalmazott lépések elvégzésével keresheti meg a tartalom kulcsok és házirendeket. Szükséges.
házirendnév | karakterlánc |Egy előzőleg regisztrált szabály nevét. Nem kötelező.
allowed_track_types | felsorolás  | SD_ONLY vagy SD_HD. Vezérlők, amelynek tartalomtípus-kulcsok egy licenc részét képező
content_key_specs | JSON tömbje struktúrákat, az alábbi **Tartalom kulcs Specs** lásd: | Táblázatrácsot nyomtatott vezérlők tartalom kulcsok való visszatéréshez. Lásd: a tartalom kulcs specifikáció alatti további információt.  Csak az egyik allowed_track_types és content_key_specs adható meg. 
use_policy_overrides_exclusively | logikai érték. IGAZ vagy hamis | Házirend-attribútumok policy_overrides által megadott használja, és hagyja ki az összes korábban tárolt házirend.
policy_overrides | JSON struktúra, lásd: a **Házirend felülbírálja** az alábbi | Házirend-beállítások annak a licencnek a jelölőnégyzetéből.  Abban az esetben, ha ez az eszköz tartozik egy előre definiált házirend, a következő megadott értékeket lesz. 
session_init | JSON struktúra, lásd: az alábbi **Munkamenet inicializálni** | Választható adatok átadott licencre.
parse_only | logikai érték. IGAZ vagy hamis | A licenc kérelem elemzi, de nincsenek licencek kibocsátott. A licenc kérelem szerepeljenek eredményként a válasz űrlap azonban értékeket.  

##<a name="content-key-specs"></a>Tartalom kulcs Specs 

Ha létezik egy már meglévő házirendet, nincs szükség van az értékek közül a tartalom kulcs specifikáció adhat meg.  A már meglévő házirendet a tartalom társított lesz a kimeneti védelmet, például HDCP és CGMS határozza meg.  Ha egy már meglévő házirend nem regisztrált, a Widevine licenc-kiszolgálóval, a tartalomszolgáltató is helyezhet el az értékeket a licenc-összehívásba.   


Minden szám, a beállítás use_policy_overrides_exclusively függetlenül minden content_key_specs meg kell adni. 


név | Érték | Leírás
---|---|---
content_key_specs. track_type | karakterlánc | Változáskövetési típus nevét. Ha content_key_specs van megadva, az a licenc kérelmet, ellenőrizze, hogy az összes típusú nyomon követése explicit módon. Ezt a hibát eredményez hiba lejátszás múltbeli 10 másodperc. 
content_key_specs  <br/> security_level | UInt32 | Ügyfél robosztussági követelményeket lejátszásra határozza meg. <br/> 1 – szoftveres whitebox crypto szükség. <br/> 2 – szoftver crypto és egy kódolt médiadekódoló szükség. <br/> 3 – kulcs anyagokat és crypto műveleteket a hardver biztonsági megbízható végrehajtás környezetén belül kell elvégezni. <br/> 4 – a crypto és a tartalom dekódolását hardver biztonsági megbízható végrehajtási környezetben kell elvégezni.  <br/> 5 – a crypto, dekódolási és a média (tömörített és tömörítés nélkül) az összes kezelési hardver biztonsági megbízható végrehajtási környezetben kell kezelni.  
content_key_specs <br/> required_output_protection.hDC | karakterlánc - közül: HDCP_NONE, HDCP_V1, HDCP_V2 | Azt jelzi, hogy szükség van-e a HDCP
content_key_specs <br/>kulcs | Base64 <br/>kódolt karakterláncot|Ehhez a számhoz használni tartalmi billentyűt. Adja meg, ha a track_type vagy key_id szükség.  Ezzel a beállítással a tartalom billentyűt az a szám helyett Widevine licenc kiszolgáló készítése, vagy egy kulcsot keresési engedélyezem a beillesztendő tartalomszolgáltató.
content_key_specs.key_id| Base64 címként kódolt karakterláncot bináris, 16 bájt | A kulcs egyedi azonosítója. 


##<a name="policy-overrides"></a>Házirend felülbírálása 

név | Érték | Leírás
---|---|---
policy_overrides. can_play | logikai érték. IGAZ vagy hamis | Azt jelzi, hogy a tartalom lejátszás engedélyezve van. Alapértelmezés szerint hamis.
policy_overrides. can_persist | logikai érték. IGAZ vagy hamis |Azt jelzi, hogy a licenc előfordulhat, hogy állandó, nem ideiglenes tárolóhoz kapcsolat nélküli használatra. Alapértelmezés szerint hamis.
policy_overrides. can_renew | logikai IGAZ vagy hamis |Jelzi, hogy engedélyezve van-e a licenc megújítása. IGAZ, ha a licenc hosszának bővíthető szívveréséhez. Alapértelmezés szerint hamis. 
policy_overrides. license_duration_seconds | Int64 | Azt jelzi, hogy az időkeret ezt a licencet. A 0 értéket, az azt jelzi, hogy az időtartam nincs korlátozva van. Alapértelmezett érték 0. 
policy_overrides. rental_duration_seconds | Int64 | Az időkeret azt jelzi, miközben lejátszás engedélyezett (permitted). A 0 értéket, az azt jelzi, hogy az időtartam nincs korlátozva van. Alapértelmezett érték 0. 
policy_overrides. playback_duration_seconds | Int64 | A Megtekintés ablak idő után a lejátszás indítása a licenc időtartam belül. A 0 értéket, az azt jelzi, hogy az időtartam nincs korlátozva van. Alapértelmezett érték 0. 
policy_overrides. renewal_server_url |karakterlánc | A licenc szívveréséhez (megújítási) az összes kérelmet a rendszer a megadott URL-címre érkező forgalmat. Ez a mező csak használatos can_renew teljesülése esetén.
policy_overrides. renewal_delay_seconds |Int64 |Hány másodperc után license_start_time, mielőtt a megújítás először megpróbálkozik vele a rendszer. Ez a mező csak használatos can_renew teljesülése esetén. Az alapértelmezett érték 0 
policy_overrides. renewal_retry_interval_seconds | Int64 | Másodpercben késleltetés későbbi megújítás licenc, abban az esetben, ha hiba között. Ez a mező csak használatos can_renew teljesülése esetén. 
policy_overrides. renewal_recovery_duration_seconds | Int64 | Az ablak az időt, amelyben a lejátszás továbbra is a megújítás ugyan még az oka az, hogy sikertelen kísérlete kódmentes problémák a licenc-kiszolgálóval. A 0 értéket, az azt jelzi, hogy az időtartam nincs korlátozva van. Ez a mező csak használatos can_renew teljesülése esetén.
policy_overrides. renew_with_usage | logikai IGAZ vagy hamis |Azt jelzi, hogy a licenc küldik megújításra használatát indításakor. Ez a mező csak használatos can_renew teljesülése esetén. 

##<a name="session-initialization"></a>Munkamenet inicializálni

név | Érték | Leírás
---|---|---
provider_session_token | Base64-címként kódolt karakterláncot |A munkamenet token vissza a licencet átadott, és a későbbi megújítását megtalálható lesz.  A munkamenet jogkivonat túl munkamenetek nem áll fenn. 
provider_client_token | Base64-címként kódolt karakterláncot | Vissza a licencet válasz küldése token ügyfél.  Ha a licenc kérés tartalmaz egy ügyfél jogkivonat, ez az érték figyelmen kívül hagyja. Az ügyfél jogkivonathoz túl licenc munkamenetek áll fenn.
override_provider_client_token | logikai érték. IGAZ vagy hamis |Hamis, valamint a licenc kérés tartalmaz egy ügyfél jogkivonat, ha még akkor is, ha egy ügyfél token megadott ennél a módszernél használata a jogkivonat a kérést.  IGAZ, ha mindig használja a megadott ennél a módszernél jogkivonat.

##<a name="configure-your-widevine-licenses-using-net-types"></a>A .NET-típus használatával Widevine licencek beállítása

Media Services Itt adhatja meg a Widevine licencek .NET API-khoz. 

###<a name="classes-as-defined-in-the-media-services-net-sdk"></a>A Media Services .NET SDK megadottak osztályok

Az alábbiakban az ilyen típusú definíciók.

    public class WidevineMessage
    {
        public WidevineMessage();
    
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public AllowedTrackTypes? allowed_track_types { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public ContentKeySpecs[] content_key_specs { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public object policy_overrides { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum AllowedTrackTypes
    {
        SD_ONLY = 0,
        SD_HD = 1
    }
    public class ContentKeySpecs
    {
        public ContentKeySpecs();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string key_id { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public RequiredOutputProtection required_output_protection { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public int? security_level { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string track_type { get; set; }
    }

    public class RequiredOutputProtection
    {
        public RequiredOutputProtection();

        public Hdcp hdcp { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum Hdcp
    {
        HDCP_NONE = 0,
        HDCP_V1 = 1,
        HDCP_V2 = 2
    }

###<a name="example"></a>Példa

A következő példa bemutatja a .NET API-k használata egyszerű Widevine licenc konfigurálása.

    private static string ConfigureWidevineLicenseTemplate()
    {
        var template = new WidevineMessage
        {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                new ContentKeySpecs
                {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                }
            },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
        };

        string configuration = JsonConvert.SerializeObject(template);
        return configuration;
    }


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Lásd még:

[PlayReady és/vagy Widevine dinamikus közös titkosítással](media-services-protect-with-drm.md)

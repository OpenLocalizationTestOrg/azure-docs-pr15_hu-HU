<properties 
    pageTitle="Media Services kibocsátási megjegyzések |} Microsoft Azure" 
    description="Media Services kibocsátási megjegyzések" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="media" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako"/>

# <a name="azure-media-services-release-notes"></a>Azure Media Services kibocsátási megjegyzések

Ezek kibocsátási megjegyzések összegzés korábbiaktól és az ismert problémák a változtatásokat.

>[AZURE.NOTE] Ügyfeleink és problémák megoldása, befolyásoló koncentráljon szeretnénk. Jelentse a problémát, vagy tegyen fel kérdéseket, kérjük, bejegyzése az [Azure Media Services MSDN-fórum].


##<a id="issues"></a>Ismert problémák

### <a id="general_issues"></a>Media Services általános problémák

A probléma|Leírás
---|---
Néhány gyakori HTTP-fejlécek nem szerepelnek a REST API-t.|A REST API Media Services-alkalmazásokat fejleszt, ha úgy gondolja, hogy néhány gyakori HTTP fejlécmezők (ügyfél-KÉRÉS-ID, beleértve a kérelem –, és VISSZATÉRÉSI-ügyfél-KÉRÉS--azonosító) nem támogatottak. A fejlécek megjelenik a későbbi frissítése.
URL-kódolás nem engedélyezett.|Media Services használja a IAssetFile.Name tulajdonság értékét adatfolyam (például http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) tartalma URL-címek készítésekor Emiatt az URL-kódolás nem engedélyezett. A **Name** tulajdonság értéke nem lehet a következő [karakterek készültségi kódolást-fenntartott](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Még csak lehet egy "." az a fájlnévhez tartozó kiterjesztést.
A ListBlobs módszer a Azure tároló SDK verzió 3.x sikertelen részét.|Media Services Társítások URL-címeit, a [2012-02 – 12](http://msdn.microsoft.com/library/azure/dn592123.aspx) verzió alapján készít. Azure tároló SDK használatával lista BLOB blob-tárolóban tetszés módszernek az alkalmazását a [CloudBlobContainer.ListBlobs](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobs.aspx) Azure tároló SDK verzió részét képező 2.x. A ListBlobs módszer, amely a az Azure tároló SDK verzió 3.x sikertelen lesz.
Mechanizmusa szabályozásának Media Services korlátozza az Erőforrás kihasználtsága alkalmazások, amelyek a fölösleges kérelem a szolgáltatás. A szolgáltatás esetleg a szolgáltatás nem érhető el (503) HTTP-állapotkód.|További tudnivalókért lásd: a HTTP-503 állapotkód az [Azure Media Services hibakódok](http://msdn.microsoft.com/library/azure/dn168949.aspx) témakör leírását.
Lekérdezés szervezetek, ha van legfeljebb 1000 szervezetek egyszerre vissza, mert a nyilvános többi v2 korlátozza a lekérdezés eredményében 1000 eredményekre. | Akkor használja a **Kihagyás gombra** , és **készítése** (.NET) [Ebben a példában a .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) és a [REST API-példában](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities)ismertetett: **felső** (REST)-, /. 
Egyes ügyfelek végig a folyamatos zökkenőmentes jegyzék egy ismétlési címke problémát származnak.|További tudnivalókért lásd az [Ebben](media-services-deliver-content-overview.md#known-issues) a szakaszban.
Azure Media Services .NET SDK objektumok nem használhatók, amelyekről, és emiatt nem működnek Azure gyorsítótárazás.|Ha fel szeretne venni Azure gyorsítótárazás az SDK AssetCollection objektum szerializálható próbál, akkor kivétel.
Üzenet-karakterlánc kódolási feladatok, melyről "szakasz: DownloadFile. Kód: System.NullReferenceException ".|A szokásos kódolási munkafolyamat beviteli video-fájl feltöltése a bemeneti tárgyi eszköz és egy vagy több kódolási feladatok elküldése a beviteli eszköz, további módosítása nélkül, amely a digitális eszköz kiválasztása a szövegbeviteli. Jó helyen jár Ha módosítja a bemeneti az eszköznek (például hozzáadása/törlése és átnevezése fájlok belül az eszköz), majd egymást követő feladatok során, melyről DownloadFile hiba. A megoldás, hogy a bemeneti eszköz törlése, újra töltse fel a bemeneti fájl egy új eszközre. 

##<a id="rest_version_history"></a>REST API-korábbi verziók

Információt a Media Services REST API-korábbi verziók olvassa el az [Azure Media Services REST API-hivatkozás]című témakört.

##<a id="july_changes16"></a>A Megjelenés július 2016-ban

###<a name="updates-to-manifest-file-ism-generated-by-encoding-tasks"></a>Fájl cikkét frissítéseit (*. ISM) generálja a tevékenységek kódolást

Kódolási feladatról Media Encoder normál vagy az Azure Media Encoder leadásakor a kódolási tevékenység generál [adatfolyam nyilvánvalóan fájl](media-services-deliver-content-overview.md) (* .ism) a kimeneti fájl a digitális eszköz kiválasztása. A szolgáltatás a legújabb kiadásra adatfolyam nyilvánvalóan fájl az alábbi változásáról.

>[AZURE.NOTE]Az alábbi adatfolyam jegyzék (.ism) fájl fenntartva belső használatra, és a későbbi kiadásokban változhatnak. Ne módosítsa, vagy a fájl tartalmát módosítására.

###<a name="a-new-client-manifest-ismc-file-is-generated-in-the-output-asset-when-an-encoding-task-outputs-one-or-more-mp4-files"></a>Egy új ügyfél jegyzék (*. ISMC) fájl jön létre, az eredményt ad, amikor egy kódolási tevékenység kimenet egy vagy több MP4-fájlok eszköz

A kezdési a legújabb kiadásra szolgáltatás létrehoz egy kódolási feladatról befejezése után további MP4 fájlokat, a kimeneti eszköz is tartalmaz adatfolyam ügyfél jegyzék (*.ismc) fájl. A .ismc fájl segítségével, dinamikus streaming teljesítményének javítása. 

>[AZURE.NOTE]Az ügyfél jegyzék (.ismc) fájl szintaxisa a következő fenntartva belső használatra, és a későbbi kiadásokban változhatnak. Ne módosítsa, vagy a fájl tartalmát módosítására.

További tudnivalókért lásd: a [blogban](https://blogs.msdn.microsoft.com/randomnumber/2016/07/08/encoder-changes-within-azure-media-services-now-create-ismc-file/) .

### <a name="known-issues"></a>Ismert problémák

Egyes ügyfelek aross egy ismétlési címke problémát a folyamatos zökkenőmentes jegyzék származnak. További tudnivalókért lásd az [Ebben](media-services-deliver-content-overview.md#known-issues) a szakaszban.

##<a id="apr_changes16"></a>Április 2016 megjelenés

### <a name="azure-media-analytics"></a>Azure Media Analytics

Azure Media Servces Azure Media Analytics hatékony videó intelligenciához jelent meg. Részletes információt olvassa el az [Azure Media Services Analytics – áttekintés](media-services-analytics-overview.md)című témakört.

### <a name="apple-fairplay-preview"></a>Apple FairPlay (előzetes verzió)

Azure Media Services lehetővé teszi a HTTP Live Streaming (HLS) tartalom és a Apple FairPlay dinamikusan titkosítása most. AMS licenc kézbesítési szolgáltatás FairPlay licencek végzi a ügyfelek is használhatja. További információt a [használata Azure Media Services kattintva letöltheti a HLS tartalom az Apple FairPlay védett ](media-services-protect-hls-with-fairplay.md)témakörben talál.
  
##<a id="feb_changes16"></a>A Megjelenés február 2016-ban

Az Azure Media Services SDK .NET (3.5.3.) a legújabb verzióját Widevine kapcsolódó hibajavítás tartalmazza. A problémát: AssetDeliveryPolicy nem tudtuk felhasználható Widevine titkosított több eszközök. A hibajavítás részeként a következő tulajdonság hozzá lett adva a SDK csomagjában talál: **WidevineBaseLicenseAcquisitionUrl**.
    
    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
    {
        {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl,"http://testurl"},
        
    };

##<a id="jan_changes_16"></a>Január 2016 megjelenés

Fenntartott egységek kódolást átnevezni kódoló neve való csökkentése érdekében.

A Basic, normál és Premium kódolási fenntartott egységek S1, S2, hogy átnevezi és S3 fenntartott egységek, illetve.  Egyszerű kódolást RUs ma alkalmazó felhasználók láthatják S1 Azure Portal (és a számlázási) címkéjeként közben Standard és a prémium a kurzor megjelenik a címkéket, S2 és S3. 

##<a id="dec_changes_15"></a>A Skype 2015 december megjelenés

Az Azure SDK csoport közzé egy új kiadását az [Azure SDK PHP-](http://github.com/Azure/azure-sdk-for-php) csomag, amely tartalmazza azokat a frissítéseket és új funkciók a Microsoft Azure Media Services. Az Azure Media Services SDK PHP különösen támogatja az új [tartalomvédelmi](media-services-content-protection-overview.md) szolgáltatások: dinamikus titkosítást AES és DRM (PlayReady és Widevine) és jogkivonat korlátozás nélkül. Is támogat, [Kódolást egységek](media-services-dotnet-encoding-units.md)méretezését.

További tudnivalókért lásd:

- A [Microsoft Azure Media Services SDK PHP-](http://southworks.com/blog/2015/12/09/new-microsoft-azure-media-services-sdk-for-php-release-available-with-new-features-and-samples/) blog.
- A következő [mintakódok](http://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices) megkezdéséhez szoftver használatának gyors megkezdése:
    - **vodworkflow_aes.php**: egy PHP-fájl, amely a AES-128 dinamikus titkosítást és kulcs kézbesítési szolgáltatás használatát ismerteti. [Ez](media-services-protect-with-aes128.md) a cikk ismerteti a .NET mintán alapul.
    - **vodworkflow_aes.php**: egy PHP-fájl, amely a PlayReady dinamikus titkosítást és licenc kézbesítési szolgáltatás használatát ismerteti. [Ez](media-services-protect-with-drm.md) a cikk ismerteti a .NET mintán alapul.
    - **scale_encoding_units.php**: Ez az PHP-fájl, amely bemutatja, hogyan méretezheti kódolási fenntartott egység.


##<a id="nov_changes_15"></a>A Skype 2015 november megjelenés

Azure Media Services kínál a Google Widevine licenc kézbesítési szolgáltatás a felhőben. További részletekért olvassa el az [értesítő blogban](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/). További információ [Ebben az oktatóanyagban](media-services-protect-with-drm.md) , valamint [GitHub tárházba](http://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm). 

Ne feledje, hogy Widevine licenc kézbesítési szolgáltatásait Azure Media Sevices előzetes verzióban. További információt talál a [blogban](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/).

##<a id="oct_changes_15"></a>A Skype 2015 október megjelenés

Azure Media Services (AMS) már élő a a következő adatközpontokban: Brazília Dél, India nyugati, India déli és indiai központi. Most már használhatja az Azure [Media szolgáltatás-fiókok](media-services-portal-create-account.md) létrehozása klasszikus portál és leírt változatos műveleteket hajthat végre [az alábbi](https://azure.microsoft.com/documentation/services/media-services/). Jó helyen jár Live kódolást nincs engedélyezve a következő adatközpontokban. További nem az összes típusú fenntartott egységek kódolást érhetők el az alábbi adatközpontokban.

- Brazília Dél: Csak a érhetők el a szokásos és kódolást fenntartott alapegység
- India nyugati, India déli és indiai központi: csak egyszerű kódolást fenntartott egységek érhetők el


##<a id="september_changes_15"></a>A Skype 2015 szeptember megjelenés 

- AMS kínál az azt jelenti, hogy igény szerinti videó (VOD) és a Widevine elemes DRM technológiával élő adatfolyamok védelme. Használhatja a következő kézbesítési szolgáltatások partnerek segít előadása Widevine licencek: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/). További tudnivalókért lásd: a [blogban](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).

    [AMS.NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (a 3.5.1-es verzióját kezdve) vagy a REST API segítségével állítsa be a AssetDeliveryConfiguration Widevine használni.  

- AMS hozzáadott Apple ProRes videók támogatása. Most már feltölthet a forrás QuickTime-videók Apple ProRes vagy más kodekekről használó fájlok. További tudnivalókért lásd: a [blogban](https://azure.microsoft.com/blog/announcing-support-for-apple-prores-videos-in-azure-media-services/).

- Most már használhatja Media Encoder szabványos rész vágás és élő archív kinyerésének tegye. További tudnivalókért lásd: a [blogban](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).

- Az alábbi szűrési frissítések végzett: 

    - Csak hang szűrővel most már használhatja az Apple HTTP Live Streaming (HLS) formátumot. A frissítés lehetővé teszi, hogy a eltávolítása csak hangot tartalmazó nyomon követése a megadása (csak hangot tartalmazó = false) URL-címét.
    - Az eszközök szűrők megadásakor most már azt jelenti, hogy több kombinálása (legfeljebb 3) szűrők egyetlen URL-címet.

    További információt talál a [blogban](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) .

- AMS most támogatja az e-keretek HLS v4. E-keret támogatási előretekerés és visszatekerés műveletek optimalizálja. Alapértelmezés szerint minden HLS v4 kimeneti értékeket tartalmazza e-keret lista (EXT-X-I-FRAME-STREAM-INF).
 
    További információt talál a [blogban](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) .

##<a id="august_changes_15"></a>A Skype 2015 augusztus megjelenés

- Java V0.8.0 kiadás és az új minta az Azure Media Services SDK elérhetők. További tudnivalókért lásd:

    - [Webnapló-hozzászólás](http://southworks.com/blog/2015/08/25/microsoft-azure-media-services-sdk-for-java-v0-8-0-released-and-new-samples-available/)
    - [Java minták tárházba](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- Több audio-adatfolyam-támogatás az Azure Media Player frissítés. További tudnivalókért lásd:
    - [Webnapló-hozzászólás](https://azure.microsoft.com/blog/2015/08/13/azure-media-player-update-with-multi-audio-stream-support/)

##<a id="july_changes_15"></a>A Skype 2015 július megjelenés

- Az általános elérhetősége Media Encoder szabványos kihirdetése. További tudnivalókért olvassa el a [blogbejegyzésben](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)című témakört.

    Media Encoder szabványos használja a [jelen](http://go.microsoft.com/fwlink/?LinkId=618336) szakaszban ismertetett készleteket. Figyelje meg, hogy amikor készlet változatával 4k kódolja szerezheti be az **prémium** fenntartott egység típusát. További információért tájékozódhat [skála kódolását](media-services-scale-media-processing-overview.md).
- Valós idejű feliratok az Azure Media Services és Player élő. További tudnivalókért olvassa el a [blogbejegyzésben](https://azure.microsoft.com/blog/2015/07/08/live-real-time-captions-with-azure-media-services-and-player/) című témakört.

###<a name="media-services-net-sdk-updates"></a>Media Services .NET SDK frissítések

Azure Media Services .NET SDK ettől 3.4.0.0 verziója. Az alábbi funkciókat ebben a kiadásban hozzá lett adva:  

- Élő archív megvalósított támogatása. Figyelje meg, hogy nem lehet letölteni tárgyi eszköz egy élő archívum tartalmazó.
- Dinamikus szűrőknek megvalósított támogatása.
- Megvalósított funkció, amely lehetővé teszi a felhasználóknak ahhoz, hogy a tárhely tároló közben eszköz törlése.
- Ismételje meg a csatornák házirendek kapcsolódó hibajavítás.
- Engedélyezett **Media Encoder prémium munkafolyamat**.

##<a id="june_changes_15"></a>Június 2015 megjelenés

###<a name="media-services-net-sdk-updates"></a>Media Services .NET SDK frissítések

Azure Media Services .NET SDK ettől 3.3.0.0 verziója. Az alábbi funkciókat ebben a kiadásban hozzá lett adva:  

- OpenId csatlakozás feltárás specifikáció, támogatása
- identitás-szolgáltató oldalán billentyűk átváltási kezelésének támogatását. 

Egy közzététele OpenID csatlakozás felderítési dokumentum, amely identitásszolgáltató használata (, ahogy a következő szolgáltatók: Azure Active Directory, a Google, Salesforce), az aláírási kulcsot juthat, JWT jogkivonat OpenID az érvényesítési csatlakozás feltárás specifikáció vonatkozó Azure Media Services találhatók. 

További tudnivalókért lásd: [Json webes billentyűk segítségével feltárás specifikáció OpenID csatlakozás JWT jogkivonat-hitelesítéssel az Azure Media Services használata a](http://gtrifonov.com/2015/06/07/using-json-web-keys-from-openid-connect-discovery-spec-to-work-with-jwt-token-authentication-in-azure-media-services/).


##<a id="may_changes_15"></a>Május 2015 megjelenés

Az alábbi új funkciók kihirdetése:

- [Media Services kódolást élő mintája](media-services-manage-live-encoder-enabled-channels.md)
- [Dinamikus jegyzék](media-services-dynamic-manifest-overview.md)
- [Azure Media Hyperlapse media processzor előnézete](https://azure.microsoft.com/blog/?p=286281&preview=1&_ppp=61e1a0b3db)

##<a id="april_changes_15"></a>Április 2015 megjelenés

 ###<a name="general-media-services-updates"></a>Általános Media Services-frissítések

- Az [Azure Media Player kihirdetése](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/).
- Media Services többi 2.10-es verziójával csatornát, melyet vannak beállítva, hogy egy RTMP protokoll ingest kezdődő jönnek létre, az elsődleges és másodlagos ingest URL-ek. További tudnivalókért lásd: a [csatorna ingest konfigurációk](media-services-live-streaming-with-onprem-encoders.md#channel_input)
- Azure Media indexelő frissítések
- Támogatás spanyol nyelvhez
- Új konfigurációs xml formátumban

További információt talál a [blogban](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).
###<a name="media-services-net-sdk-updates"></a>Media Services .NET SDK frissítések

Azure Media Services .NET SDK ettől 3.2.0.0 verziója.

Az alábbiakban néhány az ügyfél szemben lévő frissítések:

- **Módosítás megszakítása**: **TokenRestrictionTemplate.Issuer** és a **TokenRestrictionTemplate.Audience** karakterlánc típusú megváltozott.
- Egyéni létrehozásával kapcsolatos frissítések házirendek próbálkozzon újra.
- Fájlok feltöltése és letöltése kapcsolódó hibajavítás.
- A **MediaServicesCredentials** osztály most elfogad elsődleges és másodlagos access vezérlő végpont hitelesítő.



##<a id="march_changes_15"></a>Március 2015 megjelenés

### <a name="general-media-services-updates"></a>Általános Media Services-frissítések

- Media Services most itt Azure CDN-integráció. A támogatási integrációja, a **CdnEnabled** tulajdonság **StreamingEndpoint**lett hozzáadva.  **CdnEnabled** használható REST API-khoz 2.9 verzióval indítása (további tudnivalókért lásd: [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx)).  **CdnEnabled** kínál 3.1.0.2 verzióval kezdési .NET SDK (további tudnivalókért lásd: [StreamingEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.istreamingendpoint(v=azure.10).aspx)).
- **Media Encoder prémium munkafolyamat**bejelentése. További tudnivalókért olvassa el a [Prémium kódolást bemutatása Azure Media Services](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)című témakört.
 


##<a id="february_changes_15"></a>A Skype 2015 február megjelenés

### <a name="general-media-services-updates"></a>Általános Media Services-frissítések

Mostantól verzió 2.9 Media Services REST API-t. Ez a verzió kezdve, engedélyezheti a Azure CDN-integráció, a végpontok streaming. További tudnivalókért olvassa el a [StreamingEndpoint](https://msdn.microsoft.com/library/dn783468.aspx)című témakört.

##<a id="january_changes_15"></a>Január 2015 megjelenés

### <a name="general-media-services-updates"></a>Általános Media Services-frissítések

Az általános elérhetőségű kiadás dinamikus titkosítással tartalom védelem bejelentése. További tudnivalókért olvassa el a [DRM általános elérhetősége technológiával adatfolyam biztonsági hatékonyabbá tehető az Azure Media Services](https://azure.microsoft.com/blog/2015/01/29/azure-media-services-enhances-streaming-security-with-general-availability-of-drm-technology/)című témakört.

###<a name="media-services-net-sdk-updates"></a>Media Services .NET SDK frissítések

Azure Media Services .NET SDK ettől 3.1.0.1 verziója.

Ebben a kiadásban a feleslegessé vált, az alapértelmezett Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization.TokenRestrictionTemplate konstruktorral megjelölve. Az új konstruktort TokenType argumentumaként vesz igénybe.

    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);


##<a id="december_changes_14"></a>December 2014-es kiadás

###<a name="general-media-services-updates"></a>Általános Media Services-frissítések

- Egyes frissítések és az új funkciók hozzáadták az Azure Media-indexelő processzor. További tudnivalókért olvassa el az [Azure Media indexelő formátumú 1.1.6.7 kibocsátási megjegyzések](https://azure.microsoft.com/blog/2014/12/03/azure-media-indexer-version-1-1-6-7-release-notes/)című témakört.
- Megjelenik egy új REST API-val, amely lehetővé teszi, hogy a frissítés kódolást fenntartott mennyiség: [a többi EncodingReservedUnitType](http://msdn.microsoft.com/library/azure/dn859236.aspx).
- A felvett CORS fő kézbesítési szolgáltatás támogatása.
- Teljesítménybeli fejlesztések engedélyezési házirend-beállítások lekérdezésére befejeződött volna.
- A kínai adatközpont [Kulcs kézbesítési URL-cím](http://msdn.microsoft.com/library/azure/ef4dfeeb-48ae-4596-ab28-44d6b36d8769#get_delivery_service_url) ettől kezdve egy ügyfél (hasonlóan az egyéb adatközpontokban).
- A felvett HLS automatikus cél időtartam. Amikor módon élő streaming, HLS mindig dinamikusan csomagolt. Alapértelmezés szerint Media Services automatikusan alapján a képkockára helyezheti intervallum (KeyFrameInterval), a képek – GOP, az élő encoder érkezik, más néven HLS szakasz összecsomagolása arány (FragmentsPerSegment) számítja ki. További tudnivalókért olvassa el a [használata az Azure Media Services Live Streaming]című témakört.
 
###<a name="media-services-net-sdk-updates"></a>Media Services .NET SDK frissítések

- [Azure Media Services.NET SDK](http://www.nuget.org/packages/windowsazure.mediaservices/) ettől 3.1.0.0-s verziója.
- A .net SDK függőség frissíti a .NET-keretrendszer 4.5.
- Megjelenik egy új API-val, amely lehetővé teszi, hogy kódolási fenntartott mennyiség módosítása. További tudnivalókért olvassa el a [fenntartott egység típusának módosítása és a kódolás RUs növekvő használt .NET](http://msdn.microsoft.com/library/azure/jj129582.aspx)című témakört.
- A felvett JWT (JSON webes jogkivonat) jogkivonat hitelesítés támogatása. További tudnivalókért olvassa el a [JWT jogkivonat hitelesítési az Azure Media Services és dinamikus titkosítást](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)című témakört.
- A felvett relatív eltolás BeginDate és ExpirationDate a PlayReady licenc sablonban.


##<a id="november_changes_14"></a>November 2014-es kiadás

- Media Services most lehetővé teszi az élő zökkenőmentes Streaming (FMP4) tartalma ingest SSL-kapcsolaton keresztül. SSL ingest, ellenőrizze, hogy frissíthető a HTTPS ingest URL-CÍMÉT.  További információt az élő adatfolyam, lásd: az [Azure Media Services Live Streaming használata].
- Figyelje meg, hogy jelenleg meg nem ingest egy RTMP élő adatfolyam SSL-kapcsolaton keresztül.
- A tartalom is lehet adatfolyamként, SSL-kapcsolaton keresztül. Ehhez ellenőrizze, hogy a továbbított URL-címek Kiindulás HTTPS-e.
- Figyelje meg, hogy akkor is csak adatfolyam SSL Ha az adatfolyam végpontot, amelyből a tartalmak olvasása után szeptember 10, 2014-es jött létre. A továbbított URL-ek a létrehozása után szeptember 10 adatfolyam végpontok alapulnak, ha az URL a "streaming.mediaservices.windows.net" (az új formátumban) tartalmaz. Adatfolyam URL-címeit tartalmazó "origin.mediaservices.windows.net" (a régi formátum) nem támogatják az SSL. Ha az URL-címe szerepel a régi formátum, és engedélyezni szeretné, hogy [Hozzon létre egy új adatfolyam végpontot](media-services-portal-manage-streaming-endpoints.md)SSL-adatfolyam kívánja. Az új adatfolyam végpontot alapján létrehozott URL használatával a adatfolyamként SSL.

##<a id="october_changes_14"></a>Október 2014-es kiadás

### <a id="new_encoder_release"></a>Media Services Encoder megjelenés

Az új kiadását Media Services Azure Media Encoder kihirdetése. A legújabb Azure Media Encoder csak az előfizetést terhelő kimeneti GB, de más módon az új encoder kompatibilitása a korábbi kódoló a szolgáltatás. Ha további információra [Media Services árak részletei]).

### <a id="oct_sdk"></a>.NET SDK Media Services 

.NET-bővítmények Media Services SDK ettől 2.0.0.3 verziója.

A .NET rendszerhez Media Services SDK ettől 3.0.0.8 verziója.

A következő módosításokat végzett:

- Újrabontása újrapróbálkozási házirend osztályok.
- HTTP-kérés fejlécek hozzáadása a felhasználói ügynökök karakterlánc.
- Nuget visszaállítása build lépés hozzáadása.
- Rögzítési forgatókönyv vizsgálatok x509 használni a tárházba tanúsítvány.
- Beállítások ellenőrzése a csatorna frissítésekor, és a folyamatos átvitelű célból.
 

### <a name="new-github-repository-to-host-media-services-samples"></a>A host Media Services minták új GitHub tárházba

A minták az [Azure Media Services minták GitHub](https://github.com/Azure/Azure-Media-Services-Samples)adattárban találhatók.


##<a id="september_changes_14"></a>Szeptember 2014-es kiadás

Metaadat-alapú Media Services többi verziója most 2.7. Többet szeretne tudni a többi legújabb frissítéseket olvassa el az [Azure Media Services REST API-hivatkozás]című témakört.

A .NET rendszerhez Media Services SDK ettől kezdve 3.0.0.7 verziója
 
### <a id="sept_14_breaking_changes"></a>Módosítások megszakítása

* [StreamingEndpoint] **érvényességé** lett átnevezi.
* Az alapértelmezett működés, amikor az **Azure klasszikus Portal** segítségével kódolását, és tegye közzé a MP4 fájlok változását.

Korábban, amikor az Azure klasszikus portál használatával közzé a egy fájlból MP4 videó eszköz Társítások URL-cím létrejön (Társítások URL-címek lehetővé teszi, hogy töltse le a videó blob-tárolóhoz). Jelenleg az Azure klasszikus portál kódolását, és tegye közzé a egy fájlból MP4 videó eszköz használatakor létrehozott URL-CÍMÉT mutat az Azure Media Services a folyamatos átvitelű végpontot.  Ez a módosítás nem befolyásolja a MP4 videókat, amelyeket közvetlenül feltöltött Media Services és nélkül az Azure Media Services kódolás közzé.

Jelenleg a probléma megoldására az alábbi két lehetőség van.

* Adatfolyam egységek engedélyezése, és dinamikus összecsomagolása adatfolyam-zökkenőmentes adatfolyam bemutatóként az .mp4 eszköz segítségével.

* Hozzon létre egy Társítások URL-címét az .mp4 letöltése (vagy fokozatosan lejátszása). A Társítások megnevezés létrehozásával kapcsolatos további tudnivalókért lásd: [Tartalom előadásához].


### <a id="sept_14_GA_changes"></a>Engedje fel az új szolgáltatások és forgatókönyveket Georgia részét képező

* **Indexelő Media processzor**. További információ [Az Azure Media indexelő médiafájlok indexelés].

* A [StreamingEndpoint] entitás most hozzáadását teszi egyéni tartománynevek (állomás).

    Egyéni tartománynevet a Media Services adatfolyam végpontjának neve helyőrzőként frissítenie kell a továbbított végpont hozzáadása egyéni állomásnevek. Egyéni állomásnevek kövesse a Media Services REST API-hoz vagy a .NET SDK csomagjában talál.
    
    A következő érvényesek:
    
    * Az egyéni tartománynév tulajdonjogának kell rendelkeznie.
    
    * Azure Media Services ellenőrizni kell a tartomány tulajdonjogát. A tartomány ellenőrzése, hozzon létre egy CName leképezi <MediaServicesAccountId>.<parent domain> verifydns. < mediaservices-dns-zóna >. 
    
    * Létre kell hoznia egy másik CName, hogy az egyéni a host name (például sports.contoso.com) megfelelteti a Media Services StreamingEndpont a host name (például amstest.streaming.mediaservices.windows.net).


    További tudnivalókért lásd: a **CustomHostNames** tulajdonság a [StreamingEndpoint] témakörben.

### <a id="sept_14_preview_changes"></a>Új funkciók/esetek, amelyek a nyilvános előzetes kiadásában

* Élő adatfolyam minta. További tudnivalókért olvassa el a [használata az Azure Media Services Live Streaming]című témakört.

* Fő kézbesítési szolgáltatásnak. További tudnivalókért lásd: [AES-128 dinamikus titkosítást használ, és a kulcs kézbesítési szolgáltatás].

* Dinamikus-AES titkosítást. További tudnivalókért lásd: [AES-128 dinamikus titkosítást használ, és a kulcs kézbesítési szolgáltatás].

* PlayReady licenc kézbesítési szolgáltatásnak. További tudnivalókért lásd: [PlayReady dinamikus titkosítást használ, és a licenc kézbesítési szolgáltatásnak].

* Titkosítás PlayReady dinamikus. További tudnivalókért lásd: [PlayReady dinamikus titkosítást használ, és a licenc kézbesítési szolgáltatásnak].

* Media Services PlayReady licenc sablont. További információt a [Media Services PlayReady licenc sablon áttekintése]című témakörben találhat.

* A folyamatos átvitelű tároló titkosított eszközök. További tudnivalókért lásd: [A folyamatos átvitelű tároló titkosított tartalom].

##<a id="august_changes_14"></a>Augusztus 2014-es kiadás

Tárgyi eszköz kódolását meg, amikor a kódolási feladat befejezésekor kimeneti tárgyi eszköz elő. Ebben a kiadásban, amíg az Azure Media Services Encoder előállított kimeneti eszközök metaadatait. Ebben a kiadásban a kódoló kezdődő is beviteli eszközök metaadatait hoz létre. További információ az témakörökben [Beviteli metaadatok] és a [Kimeneti metaadatokat] .


##<a id="july_changes_14"></a>Július 2014-es kiadás

A következő hibajavítás végzett az Azure Media Services Packager és titkosító:

* Csak hangot csak azok a vissza, ha egy élő archív eszköz HTTP Live Streaming – transmuxing a rögzített, és most hallható hang- és videobeállítások.

* HTTP Live Streaming és AES 128 bites boríték titkosítást tárgyi eszköz csomagolása, amikor a csomagolt adatfolyamok nem felolvasás az Android-eszközökön – ezt a hibát rögzített, és a csomagolt adatfolyam lejátssza az Android-eszközökön, amelyek támogatják a HTTP Live Streaming.

##<a id="may_changes_14"></a>Május 2014-es kiadás

### <a id="may_14_changes"></a>Általános Media Services-frissítések

[Dinamikus összecsomagolása] most segítségével adatfolyam HTTP-Live Streaming (HLS) v3. Adatfolyam-HLS v3, a következő formátumban hozzáadása az origin megnevezés elérési út: * .ism/manifest(format=m3u8-aapl-v3). További információért tekintse meg [Nick Drouin].

Dinamikus összecsomagolása most is támogatja a titkosított PlayReady HLS (v3 és 4-es) előadásához zökkenőmentes Streaming PlayReady statikus titkosított alapján. Zökkenőmentes Streaming PlayReady titkosításának további tudnivalókért lásd [védelme zökkenőmentes adatfolyam a PlayReady].

### <a name="may_14_donnet_changes"></a>Media Services .NET SDK frissítések

Az alábbi fejlesztéseket számításba veszi a Media Services .NET SDK 3.0.0.5 verzióban:

* Jobb sebességétől és a multimédiás elemeket feltöltésének vagy letöltésének címtárfrissítések.

* Továbbfejlesztett újrapróbálkozási logika és tranziens kezelésének módját: 

    * Ideiglenes (tranziens) hiba észlelése, és próbálkozzon újra logika az lekérdezése, a módosítások mentése, a tölt fel vagy a fájlok letöltése okozta kivételek voltak elérését. 
    
    * Webes kivételek első (például során ACS jogkivonat felkérés), megfigyelheti, hogy a kritikus hibák nem működnek gyorsabb most.

További tudnivalókért lásd: [A .NET rendszerhez, a Media Services SDK logikája újra].

##<a id="april_changes_14"></a>Április 2014-es Encoder megjelenés

### <a name="april_14_enocer_changes"></a>Media Services Encoder frissítések

* Hol található a videó enyhe fű völgyi EDIUS nemlineáris szerkesztő segítségével létrehozott AVI fájlok ingesting hozzáadott támogatása tömöríteni, fű völgyi HQ/HQX kodek. További tudnivalókért lásd: [Fű völgyi bejelenti EDIUS 7 Streaming keresztül a felhőben].

* Verziónak támogatnia kell az adja meg a Media Encoder által létrehozott fájlok az elnevezésük is egységes. További tudnivalókért lásd: [Media szolgáltatás Encoder kimeneti fájlnév szabályozása].

* Videó vagy hang átfedi hozzáadott támogatása. További tudnivalókért olvassa el a [Átfedi létrehozása]című témakört.

* Több videó szegmensek közös varrással hozzáadott támogatása. További tudnivalókért lásd: [Videó szegmensek varrással].

* Rögzített MP4s, ahol a hang kódolású MPEG-1 Audio layer 3 (más néven MP3), a transcoding kapcsolatos hiba.


##<a id="jan_feb_changes_14"></a>Január/február 2014-es verziókban

### <a name="jan_fab_14_donnet_changes"></a>Azure Media Services .NET SDK 3.0.0.1, 3.0.0.2 és 3.0.0.3

A módosítások 3.0.0.1 és 3.0.0.2 a következők:

* Rögzített problémák megoldásához OrderBy kimutatások LINQ lekérdezés használatát.

* A felosztott próba megoldások [GitHub] egység-alapú vizsgálatok és vizsgálatok forgatókönyv-alapú.

Többet szeretne tudni a módosításokat, lásd: [Azure Media Services .NET SDK 3.0.0.1 és 3.0.0.2 elengedi].

A következő módosításokat végzett a 3.0.0.3:

* A frissített Azure tároló függőségek 3.0.3.0 verzióját használja. 

* Kompatibilitás a korábbi verziókkal probléma 3.0 rögzíteni. *.* kiadásokban. 


##<a id="december_changes_13"></a>December 2013 Release

### <a name="dec_13_donnet_changes"></a>Azure Media Services .NET SDK 3.0.0.0-s

>[AZURE.NOTE] 3.0.x.x verziókban nem kompatibilis 2.4.x.x verziókban állnak.

A legújabb a Media Services SDK ettől 3.0.0.0-s. Töltse le a legújabb csomag Nuget, vagy a bittel beolvasása [GitHub].

A Media Services SDK 3.0.0.0-s kezdve, így újból felhasználhatja az [Azure Active Directory Access vezérlő szolgáltatást (ACS)] tokenek. További információt a "Újbóli felhasználása diagramsablonok Access vezérlő szolgáltatás tokenek" a [Csatlakozás a Media Services a .NET rendszerhez, a Media Services SDK] témakör című.

### <a name="dec_13_donnet_ext_changes"></a>Azure Media Services .NET SDK bővítmények 2.0.0.0

Az Azure Media Services .NET-SDK bővítmények bővítmény módszerek és a segítő funkciók, amelyek a kód egyszerűbbé és egyszerűvé válik az Azure Media Services kidolgozása. A legújabb bittel az [Azure Media Services .NET SDK-bővítmények]elérheti.

##<a id="november_changes_13"></a>November 2013 Release

### <a name="nov_13_donnet_changes"></a>Azure Media Services .NET SDK módosítása

Ez a verzió kezdve, a Media Services SDK a .NET rendszerhez fordulhat elő, amikor a Media Services REST API-réteghez híváskezdeményezési tranziens hibafa hibákról kezeli.

##<a id="august_changes_13"></a>Augusztus 2013 Release

### <a name="aug_13_powershell_changes"></a>Media Services PowerShell-parancsmagok szereplő Azure Sdk eszközök

A következő Media Services PowerShell-parancsmagok az [azure-sdk-eszközök]most már szereplő.

* Get-AzureMediaServices 

    Ha például `Get-AzureMediaServicesAccount`.

* Új AzureMediaServicesAccount 

    Ha például `New-AzureMediaServicesAccount -Name “MediaAccountName” -Location “Region” -StorageAccountName “StorageAccountName”`.

* Új AzureMediaServicesKey 

    Ha például `New-AzureMediaServicesKey -Name “MediaAccountName” -KeyType Secondary -Force`.

* Eltávolítás-AzureMediaServicesAccount 

    Ha például `Remove-AzureMediaServicesAccount -Name “MediaAccountName” -Force`.

##<a id="june_changes_13"></a>Június 2013 Release

### <a name="june_13_general_changes"></a>Azure Media Services módosítása

A módosítások ebben a részben felsorolt frissítéseiről az 2013 júniusában Media Services verziókban.

* Több tárhely fiók hivatkozni szeretne a multimédia-szolgáltatási fiók lehetőséget. 

    StorageAccount
    
    Asset.StorageAccountName és Asset.StorageAccount

* Job.Priority frissítése lehetőséget. 

* Értesítés kapcsolódó személyek és a tulajdonságok: 

    JobNotificationSubscription
    
    NotificationEndPoint
    
    Feladat

* Asset.Uri 

* Locator.Name 

### <a name="june_13_dotnet_changes"></a>Azure Media Services .NET SDK módosítása

A következő módosításokat számításba veszi 2013 júniusában a Media Services SDK elengedi. A legújabb Media Services SDK GitHub érhető el.

* Kezdési 2.3.0.0 verzióval, a Media Services SDK támogatja több tárterületet csatolása számlák Media Services-fiókjába. A következő API-k támogatja ezt a funkciót:
    
    A IStorageAccount típusát.
    
    A Microsoft.WindowsAzure.MediaServices.Client.CloudMediaContext.StorageAccounts tulajdonsága.
    
    A StorageAccount tulajdonsága.
    
    A StorageAccountName tulajdonsága.
    
    További tudnivalókért lásd: [Kezelése szolgáltatások médiafájlok több tárterületet fiók között].

* Értesítés kapcsolódó API-khoz. Kezdő verziójával 2.2.0.0 Azure várólista tároló értesítések meghallgatása lehetősége van. A további információt a [Media Services feladat értesítések kezelése].
    
    A Microsoft.WindowsAzure.MediaServices.Client.IJob.JobNotificationSubscriptions tulajdonsága.
    
    A Microsoft.WindowsAzure.MediaServices.Client.INotificationEndPoint típusát.
    
    A Microsoft.WindowsAzure.MediaServices.Client.IJobNotificationSubscription típusát.
    
    A Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointCollection típusát.
    
    A Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointType típusát.
    
    A Microsoft.WindowsAzure.MediaServices.Client.NotificationJobState típusát.

* Függőség az Azure tároló ügyfélen SDK 2.0-s (Microsoft.WindowsAzure.StorageClient.dll).

* Függés OData 5.5 (Microsoft.Data.OData.dll).


##<a id="december_changes_12"></a>December 2012 megjelenés

### <a name="dec_12_dotnet_changes"></a>Azure Media Services .NET SDK módosítása

* Az IntelliSense: Adja hozzá a hiányzó Intellisense dokumentáció számos típusú.

* Microsoft.Practices.TransientFaultHandling.Core: Rögzített problémát hol a SDK továbbra is kellett egy függőség a összeállítás régi verzióját. A SDK most hivatkozik, a összeállítás 5.1.1209.1 verzióját.

Javítások problémákra November 2012 SDK található:

* IAsset.Locators.Count: Ez a szám most helyesen jelenteni új IAsset kapcsolaton összes Locator törlése után.

* IAssetFile.ContentFileSize: Ez az érték most megfelelően meg egy feltöltés után IAssetFile.Upload(filepath) szerint.

* IAssetFile.ContentFileSize: Ez a tulajdonság most beállítható, hogy egy digitáliseszköz-fájl létrehozásakor. Volt korábban csak olvasható.

* IAssetFile.Upload(filepath): Rögzített problémát, ahol a szinkronizált feltöltési módszer volt értesítő a következő hiba esetén több fájl feltöltése a digitális eszköz kiválasztása. A következő hiba történt "kiszolgáló nem sikerült hitelesíteni a kérést. Győződjön meg arról, hogy engedélyezési fejléc értékének megfelelően többek között az aláírás lett létrehozva."

* IAssetFile.UploadAsync: Rögzített problémát, ahol legfeljebb 5 fájlok tölthető fel egyszerre.

* IAssetFile.UploadProgressChanged: Ebben az esetben most által biztosított a SDK csomagjában talál.

* IAssetFile.DownloadAsync (karakterlánc, BlobTransferClient, ILocator, CancellationToken): Ez a módszer túlterhelés most megadva.

* IAssetFile.DownloadAsync: Rögzített problémát, ahol legfeljebb 5 fájlok tölthető le egyszerre.

* IAssetFile.Delete(): Rögzített hol hívó törlés előfordulhat, hogy kivételhibát Ha nincs fájl feltöltése a IAssetFile a problémát.

* Feladatok: Rögzített problémát hol láncolás egy "MP4 zökkenőmentes adatfolyamok tevékenységhez" egy "PlayReady védelem feladathoz" feladat sablon használatával nem eredményezne egyetlen tevékenységet sem egyáltalán.

* EncryptionUtils.GetCertificateFromStore(): Ezzel a módszerrel a tanúsítvány alapján tanúsítvány konfigurációs problémák keresése a hibák miatt null hivatkozás kivételt már nem okoz.

##<a id="november_changes_12"></a>November 2012 megjelenés

Az ebben a részben felsorolt volt a November 2012 (verzió 2.0.0.0) frissítéseiről SDK csomagjában talál. Ezek a változások megkövetelheti bármely, módosítását és újraírásra kiadás SDK június 2012 előzetes verziójának megírt kód.

* Eszközök
    
    IAsset.Create(assetName), akkor a csak a digitális eszköz kiválasztása létrehozási függvény. IAsset.Create fájlok már nem feltölti a módszer hívás részeként. Használjon IAssetFile feltöltése.
    
    A IAsset.Publish módszer, és a AssetState.Publish felsorolási érték el lett távolítva a szolgáltatások SDK csomagjában talál. Bármilyen kódot, amely ezt az értéket támaszkodik újra írt kell lennie.

* FileInfo

    Az osztály eltávolítja, és a IAssetFile helyébe.

    IAssetFiles

    IAssetFile FileInfo lecseréli, és egy másik viselkedik. Ahhoz, hogy használhassa, hozható létre az IAssetFiles objektumra, és utána egy fájl feltöltése a Media Services SDK vagy az Azure tároló SDK segítségével. A következő IAssetFile.Upload túlterhelések használható:

    * IAssetFile.Upload(filePath): Egy szinkronizált módszer, amely letiltja a szálhoz tartozó üzenet, és csak akkor, ha egy fájl feltöltése ajánlott.
    
    * IAssetFile.UploadAsync (fájl elérési útja, blobTransferClient, Megnevezés, cancellationToken): egy aszinkron módszert. Ez egy, az előnyben részesített feltöltési módot. 

    Ismert hibaadatbázis: segítségével a cancellationToken valóban megszüntetése a feltöltés; azonban a tevékenységek állapotát a lemondás lehet valamelyik államokból számos. Meg kell megfelelően tájékozódást segíti, és kezelheti a kivételek.

* Locator
    
    Az Origin-specifikus verziók el lett távolítva. Az Társítások-specifikus környezetben. Elavult vagy GA eltávolította Locators.CreateSasLocator (eszköz, accessPolicy) lesznek megjelölve. Című Locator új funkciók a frissített elemeire vonatkozó.


##<a id="june_changes_12"></a>Június 2012 előzetes verzióval

Az alábbi funkciókat volt a SDK November kiadását az új.

* Személyek törlése

    IAsset, IAssetFile, ILocator, IAccessPolicy, IContentKey objektumok most törlődnek a objektum szintjén, azaz IObject.Delete() megkövetelése a gyűjteményben, amely cloudMediaContext.ObjCollection.Delete(objInstance) a törlés helyett.

* Locator

    Locator most létre kell hozni az CreateLocator módszerrel, és a LocatorType.SAS vagy LocatorType.OnDemandOrigin felsorolás értékek használata argumentumaként megnevezés adott típusú szeretne létrehozni.

    Új tulajdonságokat Locator, így azok könnyebben beszerzése a tartalom számára használható URL-címe hozzáadott. A újratervezése Locator nagyobb rugalmasság érdekében a jövőbeli külső bővítési szükséges és a könnyű kezelés--használati media ügyfélalkalmazásokban növelése lett szólnak.

* Aszinkron módszer támogatás

    Aszinkron támogatási minden módszer lett hozzáadva.


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


<!-- Anchors. -->

<!-- Images. -->

<!--- URLs. --->
[Azure Media Services-fórum az MSDN webhelyen]: http://social.msdn.microsoft.com/forums/azure/home?forum=MediaServices
[Azure Media Services REST API-hivatkozás]: http://msdn.microsoft.com/library/azure/hh973617.aspx 
[Media Services árakról]: http://azure.microsoft.com/pricing/details/media-services/
[Beviteli metaadatok]: http://msdn.microsoft.com/library/azure/dn783120.aspx
[Kimeneti metaadatok]: http://msdn.microsoft.com/library/azure/dn783217.aspx
[Tartalom továbbítása]: http://msdn.microsoft.com/library/azure/hh973618.aspx
[Az indexelési médiafájlok az Azure Media indexelő]: http://msdn.microsoft.com/library/azure/dn783455.aspx
[StreamingEndpoint]: http://msdn.microsoft.com/library/azure/dn783468.aspx
[Élő adatfolyam az Azure Media Services használata]: http://msdn.microsoft.com/library/azure/dn783466.aspx
[Dinamikus titkosítást AES-128 és a fő kézbesítési szolgáltatás használatával]: http://msdn.microsoft.com/library/azure/dn783457.aspx
[Dinamikus PlayReady titkosítás és a licenc kézbesítési szolgáltatás használatával]: http://msdn.microsoft.com/library/azure/dn783467.aspx
[Preview features]: http://azure.microsoft.com/services/preview/
[Media Services PlayReady licenc sablon – áttekintés]: http://msdn.microsoft.com/library/azure/dn783459.aspx
[A folyamatos átvitelű tároló titkosított tartalom]: http://msdn.microsoft.com/library/azure/dn783451.aspx
[Azure Classic Portal]: https://manage.windowsazure.com
[Dinamikus csomagolása]: http://msdn.microsoft.com/library/azure/jj889436.aspx
[Nick Drouin Blog]: http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/
[A PlayReady zökkenőmentes adatfolyam védelme]: http://msdn.microsoft.com/library/azure/dn189154.aspx
[Ismételje meg a Media Services SDK logika a .NET rendszerhez]: http://msdn.microsoft.com/library/azure/dn745650.aspx
[Fű völgyi EDIUS 7 bejelenti a folyamatos átvitelű keresztül a felhőben]: http://www.streamingmedia.com/Producer/Articles/ReadArticle.aspx?ArticleID=96351&utm_source=dlvr.it&utm_medium=twitter
[Ellenőrző Media Encoder kimeneti fájlnév szolgáltatás]: http://msdn.microsoft.com/library/azure/dn303341.aspx
[Átfedi létrehozása]: http://msdn.microsoft.com/library/azure/dn640496.aspx
[Videó szegmensek való]: http://msdn.microsoft.com/library/azure/dn640504.aspx
[Azure Media Services .NET SDK 3.0.0.1 és 3.0.0.2 verziókban]: http://www.gtrifonov.com/2014/02/07/windows-azure-media-services-.net-sdk-3.0.0.2-release/
[Azure Active Directory vezérlő szolgáltatást (ACS)]: http://msdn.microsoft.com/library/hh147631.aspx
[Csatlakozás a Media Services SDK Media Services a .NET rendszerhez]: http://msdn.microsoft.com/library/azure/jj129571.aspx
[Azure Media Services .NET SDK bővítmények]: https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev
[Azure-sdk-eszközök]: https://github.com/Azure/azure-sdk-tools
[GitHub]: https://github.com/Azure/azure-sdk-for-media-services
[Eszközök kezelése Media szolgáltatások több tárterületet fiók keresztül]: http://msdn.microsoft.com/library/azure/dn271889.aspx
[Feladat értesítések kezelése Media Services]: http://msdn.microsoft.com/library/azure/dn261241.aspx
 

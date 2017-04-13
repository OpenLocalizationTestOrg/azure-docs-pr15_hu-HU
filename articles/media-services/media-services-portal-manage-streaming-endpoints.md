<properties 
    pageTitle="Az Azure portálján adatfolyam végpontok kezelése |} Microsoft Azure" 
    description="Ez a témakör bemutatja az Azure portálján adatfolyam végpontok kezelése." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    writer="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016"
    ms.author="juliako"/>


#<a name="manage-streaming-endpoints-with-the-azure-portal"></a>Az Azure portálján adatfolyam végpontok kezelése

## <a name="overview"></a>– Áttekintés

> [AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez Azure-fiók szükséges. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/). 

Microsoft Azure Media Services a **Folyamatos átvitelű végpont** adatfolyam szolgáltatás, amely a tartalmak tartana közvetlenül egy ügyfélalkalmazás player, vagy hogy a tartalom kézbesítési hálózati (CDN) további eloszlás jelöl. Media Services is zökkenőmentesen elvégezhető Azure CDN-integráció tartalmaz. A kimenő adatfolyam StreamingEndpoint szolgáltatásból lehet élő adatfolyam vagy egy videót arról, hogy igény szerint a digitális eszköz kiválasztása a Media Services-fiókjában.

Ezeken kívül szabályozhatja, hogy a folyamatos átvitelű végpont szolgáltatás kezelése növekvő sávszélesség igényeinek eredményhez tartozó adatfolyam egységek kapacitása. Ajánlott lefoglalhat egy vagy több skála egységek alkalmazások üzemi környezetben. Adatfolyam egységek nyújt az időosztáson 200 MB vásárolhatja dedikált kilépési kapacitás és a további funkciókat, amelyek tartalmazzák még: [dinamikus csomagolása](media-services-dynamic-packaging-overview.md), CDN-integráció és speciális konfigurációs.

>[AZURE.NOTE]Vannak csak számlát kapni, amikor a folyamatos átvitelű végpont állam futtatása.

Ez a témakör áttekintést nyújt a folyamatos átvitelű végpontok által biztosított főbb funkciók. A témakör is mutatja az Azure portal segítségével adatfolyam végpontok kezelése. Az adatfolyam végpontot méretezni, akkor olvassa el [Ebben](media-services-portal-scale-streaming-endpoints.md) a témakörben olvashat.

## <a name="start-managing-streaming-endpoints"></a>Indítsa el a továbbított végpontok kezelése

Adatfolyam végpontjait a fiók kezelése indításához kövesse az alábbi.

1. Az [Azure portál](https://portal.azure.com/)jelölje ki az Azure Media Services-fiókját.
2. A **Beállítások** ablakban jelölje ki a **adatfolyam végpontok**.

    ![A folyamatos átvitelű végpont](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints1.png)

##<a name="adddelete-a-streaming-endpoint"></a>A továbbított végpont hozzáadása és törlése

Az Azure portálon adatfolyam végpont hozzáadása és törlése, tegye a következőket:

1. A továbbított végpont hozzáadásához kattintson a **+ végpont** elemre a lap tetején. 
2. Ha törölni szeretne egy adatfolyam végpontot, nyomja le a **törlése** gombra. 

    Az alapértelmezett végpont streaming nem lehet törölni.
2. A **Start** gombra kattintva indítsa el az adatfolyam végpontot.

    ![A folyamatos átvitelű végpont](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints2.png)

Alapértelmezés szerint egy vagy két adatfolyam végpontokat is. Ha szeretne további kérése, olvassa el a [kvótáinak és korlátai](media-services-quotas-and-limitations.md).
    
##<a id="configure_streaming_endpoints"></a>A továbbított végpont beállítása

A folyamatos átvitelű végpont lehetővé teszi, hogy legalább 1 időosztás egységet esetén, állítsa be a következő tulajdonságokat: 

- Hozzáférés-vezérlés
- Gyorsítótár vezérlő
- Idegen webhely access szabályai

Tulajdonságokból kapcsolatos részletes tudnivalókért olvassa el a [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx)című témakört.

Adatfolyam-végpontot adhatja meg az alábbiak szerint:

1. Válassza ki a beállítani kívánt adatfolyam végpontot.
1. Kattintson a **Beállítások**gombra.
  
A mezők egy rövid leírást követi.

![A folyamatos átvitelű végpont](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints4.png)
  
1. Gyorsítótár maximális házirend: gyorsítótár-élettartam eszközök és a továbbított végpont felszolgált konfigurálásához. Ha nincs érték van beállítva, az alapértelmezett használják. Az alapértelmezett értékeket is definiálható közvetlenül az Azure-tárolóban lévő. Azure CDN engedélyezve van az adatfolyam végpontot, ha nem beállíthatja a gyorsítótár házirend értéke kisebb, mint a 600 másodpercre.  

2. Engedélyezett IP-címek: Itt adhatja meg a közzétett adatfolyam végpont csatlakoztatása engedélyezett IP-címek. Ha nincs megadva IP-címet a bármely IP-cím kapcsolódhat lenne. IP-címek lehet megadni egy egyetlen IP-címet (például 10.0.0.1), az IP-tartomány IP-címet és egy CIDR alhálózat maszk (például 10.0.0.1/22) vagy az IP-címet és egy pontozott decimális alhálózat maszk IP-tartomány (például "10.0.0.1(255.255.255.0)').

3. Konfigurációs Akamai aláírás élőfej hitelesítéshez: Itt adhatja meg, hogyan aláírás élőfej hitelesítési kérelem Akamai kiszolgálókról van konfigurálva. Lejárat UTC szerepel.



##<a id="enable_cdn"></a>Azure CDN alkalmazással való integráció engedélyezése

Megadhatja, hogy a folyamatos átvitelű végpont (Ez alapértelmezés szerint nincs engedélyezve.) az Azure CDN alkalmazással való integráció engedélyezése

Az Azure CDN-integráció beállítása igaz:

- Az adatfolyam végpontot legalább egy adatfolyam egység kell rendelkeznie. Ha később szeretné skála egységek értéke 0, először ki kell kapcsolnia a CDN-integrációt. Alapértelmezés szerint egy új streaming létrehozásakor végpont adatfolyam egységnyi értéke automatikusan.

- Az adatfolyam végpontot leállítva állapotban kell lennie. A CDN kap engedélyezése után elindíthatja az adatfolyam végpontot. 

Első engedélyezve van az Azure CDN-integráció legfeljebb 90 perc is eltelhet.  A módosítások aktívnak kell lennie keresztül minden a CDN POP két órát vesz igénybe.

CDN-integráció engedélyezve van az összes Azure adatközpontokban: US nyugati, US kelet, Észak-Európa, nyugati Europe, japán nyugati, japán keleti, Dél-kelet-ázsiai és kelet-ázsiai.

Ha engedélyezve van, a **Hozzáférés-vezérlés** konfiguráció kap letiltja.

![A folyamatos átvitelű végpont](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints5.png)

>[AZURE.IMPORTANT] **Azure CDN Verizon az**Azure Media Services-integráció a Azure CDN valósítja.  **Azure CDN Akamai az** Azure Media Services használni kíván, be kell [a végpont manuális beállítását](../cdn/cdn-create-new-endpoint.md).  Azure CDN-szolgáltatásokkal kapcsolatos további tudnivalókért olvassa el a [CDN – áttekintés](../cdn/cdn-overview.md)című témakört.

###<a name="additional-considerations"></a>További megfontolandó szempontok

- CDN engedélyezve van egy adatfolyam végpontot, amikor az ügyfelek tartalom nem közvetlenül az origin kérhet. Tesztelje a tartalmakat, vagy azok nélkül CDN lehetősége van szüksége, ha egy másik adatfolyam végpontot, amely nem engedélyezett CDN is létrehozhat.
- A továbbított végpont hostname (állomásnév) változatlan marad a CDN engedélyezése után. Nem kell bármilyen módosítást végez a media services munkafolyamat CDN engedélyezése után. Például ha az adatfolyam végpont hostname (állomásnév) strasbourg.streaming.mediaservices.windows.net, CDN engedélyezése után, a pontos azonos hostname (állomásnév) használja.
- Az új adatfolyam végpontok engedélyezheti CDN egyszerűen létrehoz egy új végpontot; a meglévő adatfolyam végpontokhoz kell először állítsa le a végpontot, és engedélyezheti a CDN-t.
 

További információ, [kihirdetése Azure Media Services-integráció a Azure CDN (tartalom kézbesítési hálózat)](http://azure.microsoft.com/blog/2015/03/17/announcing-azure-media-services-integration-with-azure-cdn-content-delivery-network/).


##<a name="next-steps"></a>Következő lépések

Tekintse át a Media Services tanulási javaslatok.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
 

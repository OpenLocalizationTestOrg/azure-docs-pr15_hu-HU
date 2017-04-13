<properties
    pageTitle="Méretezés Media feldolgozása áttekintése |} Microsoft Azure"
    description="Ez a témakör áttekintést méretezési Media feldolgozása az Azure Media Services."
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="juliako"/>


# <a name="scaling-media-processing-overview"></a>Méretezési Media feldolgozása – áttekintés

Ezen az oldalon a megtudhatja, hogy hogyan és miért, ha át kívánja méretezni media feldolgozás adja vissza. 

## <a name="overview"></a>– Áttekintés

A Media Services-fiók hozzárendelve egy fenntartott egység típusát, amely azt határozza meg, a sebesség, amellyel a feladatok feldolgozása media feldolgozása. A következő fenntartott egység típusok között is választhat: **S1**, **S2**vagy **S3**. Például az ugyanazon kódolási feladatot fut gyorsabb **S2** fenntartott egység típusú viszonyítási alap **S1** írja be ide használatakor. További tudnivalókért lásd: a [Fenntartott egység típusok](https://azure.microsoft.com/blog/high-speed-encoding-with-azure-media-services/).

Mellett a fenntartott egység típusának megadása, megadhatja, hogy a fiók kiépítése fenntartott egységekkel. Kiépített fenntartott mennyiségek határozza meg, hogy egyidejűleg dolgozható egy adott fiókban media tevékenységek száma. Például ha a fiókban öt fenntartott egységek, majd öt media feladatok fog futni egyidejűleg hosszú, amennyi a tevékenységek dolgozható fel. A hátralévő tevékenységek várakozik, a várakozási sorban található, és amikor futtatott feladat befejezése egymás után feldolgozásra fog első felvételre. Ha a fiók nem bármely kiépítéstől fenntartott mennyiség, majd feladatok fog felvételre egymás után. Ebben az esetben a várakozási idő között egy tevékenység befejezési és a következő egy kezdő függ az erőforrások elérhetőségének a rendszer.

## <a name="choosing-between-different-reserved-unit-types"></a>Választás a különböző fenntartott egység típusai

Az alábbi táblázat segítségével beállíthatja a döntési közötti különböző kódolási sebesség kiválasztásakor. Azt is tartalmaz néhány javasolt esetek bejegyzései, és Társítások URL-címeit, videókat, amelyen végezheti el a saját vizsgálatok letöltéséhez használható:

Felhasználási területei|**S1**|**S2**|**S3**|
----------|------------|----------|------------
Olyan eset| Egy átviteli sebesség kódolás. <br/>Fájlok Ft vagy az alatt a felbontás, nem a bizalmas, alacsony költség időt.|Egy átviteli sebesség és több átviteli sebesség kódolását.<br/>Normál használatát kódolási Ft és HD-minőségű is. |Egy átviteli sebesség és több átviteli sebesség kódolását.<br/>Teljes HD és 4K felbontás videókat. Idő bizalmas, a gyorsabb esetenként kódolását. 
Javasolt|[Bemeneti fájl: 5 percig tart sokáig 640x360p 29,97 a keretek/második](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_360p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D).<br/><br/>Egy egyetlen átviteli sebesség MP4 fájl megegyező felbontásban kódolást perc körülbelül 11.|[A bemeneti fájl: 5 percig tart sokáig 1280x720p 29,97 a keretek/második](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_720p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D)<br/><br/>"H264 egyetlen átviteli sebesség 720p" kódolást előre beállított veszi körülbelül 5 percig tart.<br/><br/>A kódolás "H264 több átviteli sebesség 720p" beállított körülbelül 11,5 percet vesz igénybe.|[Bemeneti fájl: 5 percig tart sokáig 1920x1080p 29,97 a keretek/második](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_1080p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D). <br/><br/>"H264 egyetlen átviteli sebesség 1080p" kódolást előre beállított veszi körülbelül 2.7 percet.<br/><br/>A kódolás "H264 több átviteli sebesség 1080p" beállítás a körülbelül 5.7 percig tart.

##<a name="considerations"></a>Megfontolandó szempontok

>[AZURE.IMPORTANT] Tekintse át a jelen szakaszban ismertetett szempontokat.  

- Fenntartott egység összes media feldolgozása, többek között az indexelő feladatokat, használja az Azure Media indexelő parallelizing működik.  Azonban eltérően kódolás indexelő feladatokat első feldolgozása nem gyorsabb gyorsabb fenntartott egységekkel.

- A megosztott készlet változatával Ez azt jelenti, hogy nélkül bármely fenntartott egységek, majd a kódolása feladatok esetén ugyanazt a teljesítmény S1 RUs a. Nincs azonban nem felső határa az időt, a feladatok tölthetnek várólistás állapotú, és az adott időpontban, legfeljebb egyszer valóban futnia.

- A következő adatközpontokban nem ajánlja fel a **S2** fenntartott mértékegysége: Brazília Dél, India nyugati, India központi és indiai Dél.

- A következő adatközpontokban nem ajánlja fel a **S3** fenntartott mértékegysége: Brazília Dél, India nyugati, India központi.

- A költség kiszámítása a legmagasabb a 24 órás időszakra megadott számú használják.


##<a name="quotas-and-limitations"></a>Kvóták és korlátai

Kvóták és korlátozások, és hogy miként nyithatja meg a támogatási jegyek információkért témakörben [kvóták](media-services-quotas-and-limitations.md).

##<a name="next-step"></a>Következő lépés

A méretezési media feldolgozási tevékenység egy ezeket a technológiákat elérése: 

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encoding-units.md)
- [Portál](media-services-portal-scale-media-processing.md)
- [TÖBBI](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

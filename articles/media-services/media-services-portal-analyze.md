<properties
    pageTitle="Az Azure portálon médiatartalmak elemzése |} Microsoft Azure"
    description="Ez a témakör ismerteti a multimédiás Media Analytics media processzorok (MPs) az Azure portálon feldolgozása."
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
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="analyze-your-media-using-the-azure-portal"></a>Az Azure portálon médiatartalmak elemzése

> [AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez Azure-fiók szükséges. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="overview"></a>– Áttekintés

Azure Media Services Analytics beszéd- és látás alkatrészek (a nagyvállalati skála, megfelelőség, biztonság és globális vannak), amelyek megkönnyítik a szervezetek és a videofájlok az értekezletekre háttérismeretek forrásául vállalkozások gyűjteménye. [A](media-services-analytics-overview.md) témakör az Azure Media Services Analytics részletesebb áttekintése. 

Ez a témakör ismerteti a multimédiás Media Analytics media processzorok (MPs) az Azure portálon feldolgozása. Médiafájlok Analytics MPs konzerv MP4 vagy JSON fájlokat. Ha egy media processzor előállítása MP4-fájlként, fokozatosan letöltheti a fájlt. Ha egy media processzor JSON fájl előállítása, melyet letölthet a fájlt az Azure blob-tárolóhoz. 

## <a name="choose-an-asset-that-you-want-to-analyze"></a>Válassza ki az elemezni kívánt tárgyi eszköz 
 
1. Az [Azure portál](https://portal.azure.com/)jelölje ki az Azure Media Services-fiókját.
2. A **Beállítások** ablakban válassza ki az **eszközöket**.  
.
    ![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze001.png)

2. Jelölje ki az eszközt, amelyeket szeretne elemezni, és kattintson az **elemzés** gombra.
        
    ![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze002.png)

3. A **folyamat media eszköz Media Analytics** ablakában jelölje ki a processzor. 

    A cikk a többi megtudhatja, hogy miért minden processzor használatához. 
   
4. Nyomja le a **Létrehozás** indítása egy feladatot.

## <a name="azure-media-indexer"></a>Azure Media indexelő

Az **Azure Media indexelő** media processzor lehetővé teszi, hogy a médiafájlok és a tartalom kereshető tétele, valamint a kódolt feliratok számok készítése. Ez a szakasz tájékoztatást nyújt a bizonyos beállításokat is megadhat a MP.

![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze003.png)

### <a name="language"></a>Nyelvi

A természetes nyelvű ismeri fel a multimédiás fájl. Ha például angol és spanyol. 

### <a name="captions"></a>Feliratok hozzáadása

Megadhatja, hogy a tartalom a létrehozott felirat alapértelmezettől. Egy indexelési feladat kódolt felirat fájlokat hozhat létre, a következő formátumokban:  

- **SZÁMI**
- **TTML**
- **WebVTT**

A következő formátumú fájlok is használható, hogy a hang-és videofájlok hallja a fogyatékkal élők CC felirat lezárva.

### <a name="aib-file"></a>AIB fájl

Akkor válassza ezt a lehetőséget, ha azt szeretné, hozza létre a hang Index Blob-fájlt az egyéni SQL Server IFilter való használatra. További tudnivalókért lásd: a [blogban](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) .

### <a name="keywords"></a>Kulcsszavak

Akkor válassza ezt a lehetőséget, ha szeretné, hogy a kulcsszavak XML-fájl készítése. Ezt a fájlt tartalmazza e kulcsszavakat a beszéd tartalom kiolvasott gyakoriság és eltolás adatokkal.

### <a name="job-name"></a>Feladat neve

Egy rövid nevet, amellyel azonosítani a feladatot. [Ez](media-services-portal-check-job-progress.md) a cikk azt ismerteti, hogy miként figyelheti a projekt előrehaladását. 

### <a name="output-file"></a>Kimenő fájl

Egy rövid nevet, amellyel azonosítani a kimeneti tartalmat. 

## <a name="azure-media-hyperlapse"></a>Azure Media Hyperlapse

Azure Media Hyperlapse egy MP által létrehozott zökkenőmentes idő elévült videók első-személy vagy művelet fényképezőgép-tartalom.  További tudnivalókért lásd: [Ez](media-services-hyperlapse-content.md) a témakör. Ez a szakasz tájékoztatást nyújt a bizonyos beállításokat is megadhat a MP.

![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze004.png)

### <a name="speed"></a>Sebessége 

Adja meg, amellyel a bemeneti videó gyorsítása sebességét. A kimenet stabil, és időt elévült képmegjelenítés, a videó bevitt.

### <a name="job-name"></a>Feladat neve

Egy rövid nevet, amellyel azonosítani a feladatot. [Ez](media-services-portal-check-job-progress.md) a cikk azt ismerteti, hogy miként figyelheti a projekt előrehaladását. 

### <a name="output-file"></a>Kimenő fájl

Egy rövid nevet, amellyel azonosítani a kimeneti tartalmat. 

## <a name="azure-media-face-detector"></a>Azure Media arc jelentő

Az **Azure Media arc jelentő** media processzor (MP) lehetővé teszi, hogy a száma, a mozgásának nyomon, és a még figyelheti a hallgatóság részvételét és reakció arc kifejezések keresztül. Ez a szolgáltatás két funkciókat tartalmazza: 

- **Arc kimutatására**

    Arc észlelési keres, és nyomon követi a videó emberi lapjaira. Több arcot is az általuk észlelt, és ezt követően kell nyomon követi azokat Navigálás, az időt és a hely metaadatokkal JSON fájlban adja vissza. A nyomon követés során akkor próbálja konzisztens Azonosítóra adni az azonos arc közben a személy található beszúrási pont áthelyezése a képernyőn, ha vannak a probléma egyik oka, vagy röviden elhagyja a keret.

    >[AZURE.NOTE]Ez a szolgáltatás nem hajt végre arc felismerés. Olyan személy, aki elhagyja a keret vagy válik a probléma egyik oka az túl sokáig kap egy új Azonosítót amikor vissza őket.

- **Emotion kimutatására**
    
    Emotion észlelési része egy nem kötelező a arc észlelési Media processzor, amely analysis több érzelmi attribútumain köztük Boldogsága, sadness, félelem, utasítás és más észlel, arc vissza. 

![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze005.png)

### <a name="detection-mode"></a>Észlelési mód

A következő üzemmódok valamelyikében a processzor használható:

- arc kimutatására
- egy oldallal emotion kimutatására
- összesítő emotion kimutatására

### <a name="job-name"></a>Feladat neve

Egy rövid nevet, amellyel azonosítani a feladatot. [Ez](media-services-portal-check-job-progress.md) a cikk azt ismerteti, hogy miként figyelheti a projekt előrehaladását. 

### <a name="output-file"></a>Kimenő fájl

Egy rövid nevet, amellyel azonosítani a kimeneti tartalmat. 

## <a name="azure-media-motion-detector"></a>Azure Media Mozgásvonalas jelentő

Az **Azure Media Mozgásvonalas jelentő** media processzor (MP) lehetővé teszi, hogy hatékony az egyes szakaszokhoz kamat-egyébként hosszú és Eseménytelen videó belül. Mozgásvonalas észlelése statikus kamera képanyag hol fordul elő a mozgásvonalak videó szakaszok azonosítása használható. A metaadat-alapú időbélyegeket és a határoló régió, ahol az esemény bekövetkezett tartalmazó JSON fájl hoz létre.

Célzott biztonsági videó hírcsatornák felé, a technológia el tudja mozgásvonalas sorolni fontos események és téves például árnyékok, és a megvilágítás módosításokat. Ez lehetővé teszi a biztonsági figyelmeztetések készítése a kamera hírcsatornák anélkül, hogy éppen címünkre végtelen lényegtelen eseményekkel, miközben érdeklődésre számot tartó percet kinyerése rendkívül hosszú felügyeleti videókat is.

![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze006.png)

## <a name="azure-media-video-thumbnails"></a>Azure Media Video miniatűrjei

A processzor segíthetnek a forrás videóban érdekes kódrészletek automatikusan választva hosszú videók összegzéseinek hozhat létre. Ez akkor hasznos, ha meg szeretné, hogy mire számíthat, hosszú a videó gyors áttekintést nyújt. Részletes tudnivalókat és példákat talál [Használata Azure Media videó miniatűrjét hozhat létre egy videó összefoglaló](media-services-video-summarization.md)

![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze008.png)

### <a name="job-name"></a>Feladat neve

Egy rövid nevet, amellyel azonosítani a feladatot. [Ez](media-services-portal-check-job-progress.md) a cikk azt ismerteti, hogy miként figyelheti a projekt előrehaladását. 

### <a name="output-file"></a>Kimenő fájl

Egy rövid nevet, amellyel azonosítani a kimeneti tartalmat. 


##<a name="next-steps"></a>Következő lépések

Nézet Media Services tanulási javaslatok.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



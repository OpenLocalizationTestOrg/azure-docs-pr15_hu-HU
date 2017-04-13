<properties 
    pageTitle="Speciális kódolási munkafolyamatok létrehozását a munkafolyamat-Tervező |} Microsoft Azure" 
    description="További tudnivalók a munkafolyamat-Tervező speciális kódolási munkafolyamatok létrehozását." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016"
    ms.author="juliako;johndeu;anilmur"/>


#<a name="create-advanced-encoding-workflows-with-workflow-designer"></a>Speciális kódolási munkafolyamatok létrehozását a munkafolyamat-Tervező

##<a name="overview"></a>– Áttekintés

A **Munkafolyamat-Tervező** a Windows asztali használt eszköz tervezhet és készíthet, egyéni munkafolyamatok kódolási **Media Encoder prémium**munkafolyamathoz.
A power a munkafolyamat-Tervező eszköz használatával is tervezhet és **Media Encoder prémium**futtatandó összetett munkafolyamatok létrehozása.  

Munkafolyamatok is ügyfél döntés logika, és a bemeneti forrásfájl Tulajdonságok elágaztatási alapján. Munkafolyamatok létrehozását bírálhat tulajdonságok és dinamikus értékek, hogy még a legtöbb összetett kódolási feladatok egyszerűen ismételje meg, és testre szabhatja a felhőben.

Példa a munkafolyamatok hozható létre a következők:

- Döntés alapú munkafolyamatok, nézze meg a forrás tartalom vonatkozó felbontásban, és csak a kívánt kimeneti számok kódolását.  Ez a helfpul a feleslegesen számok által a forrás tartalom akaratlanul upscaling volna létrehozott kiküszöbölésével.
- Több bemeneti fájlok feliratok, átfedi és való együtt tartalom használható. 

Ez az eszköz is használható a [közzétett munkafolyamatok](media-services-workflow-designer.md#existing_workflows)módosíthatók. 

>[AZURE.NOTE]Ha a munkafolyamat-Tervező eszköz-példányát, kérjük, forduljon az mepd@microsoft.com.


Munkafolyamat-fájl létrehozása után eszközként tölthet fel, majd médiafájlok kódolási használhatók. Kódolását **Media Encoder prémium munkafolyamat** **.NET**használatával kapcsolatos további tudnivalókért lásd [Speciális Media Encoder prémium munkafolyamat kódolást](media-services-encode-with-premium-workflow.md).

##<a id="existing_workflows"></a>Módosítsa a meglévő munkafolyamatok

A [munkafolyamatok közzétett](media-services-workflow-designer.md#existing_workflows) alapértelmezett módosítható a Webhelytervező eszköz használatával. Elérheti az alapértelmezett munkafolyamat fájlok [Itt](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). A mappa is tartalmazza ezeket a fájlokat a leírását.

A következő videók a használható szemléltetik.

###<a name="day-1--getting-started"></a>Napi 1 – első lépések

1 nap videó foglalja magában:

- Tervező – áttekintés
- Alapvető munkafolyamatok – a "Helló, világ"
- Több létrehozása a kimeneti MP4 fájlokat a folyamatos átvitelű Azure Media Services való használatra

> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-1]

###<a name="day-2"></a>2 nap

2 nap videó foglalja magában:

- Adatforrás-fájl esetek változó – a hang kezelése
- Speciális logika a munkafolyamat
- Graph szakaszban

> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-2]

###<a name="day-3"></a>3 nap

3 nap videó foglalja magában:

- A parancsfájlok munkafolyamatok és tervrajzokat belül
- Korlátozás az aktuális Encoder
- A kérdések és válaszok
 
> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-3]


## <a name="next-step"></a>Következő lépés

Tekintse át a Media Services tanulási javaslatok.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


Ha szükséges támogatja vagy egyéni munkafolyamatok létrehozása a munkafolyamat designer eszközben kérdése van, küldjön e-mailt mepd@microsoft.com.

##<a name="see-also"></a>Lásd még:

[Azure prémium Encoder munkafolyamat-Tervező tanfolyamok](http://johndeutscher.com/2015/07/06/azure-premium-encoder-workflow-designer-training-videos/)

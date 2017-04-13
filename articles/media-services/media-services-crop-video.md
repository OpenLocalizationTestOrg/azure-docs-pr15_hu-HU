<properties
    pageTitle="Videokép körülvágása |} Microsoft Azure"
    description="Ez a cikk bemutatja, hogyan Media Encoder standard videók körülvágása."
    services="media-services"
    documentationCenter=""
    authors="anilmur"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016"  
    ms.author="anilmur;juliako;"/>

# <a name="crop-videos-with-media-encoder-standard"></a>Kép körülvágása videók Media Encoder standard

A beviteli videó kivághatja Media Encoder szabványos (MES) is használhatja. Körülvágás belül a videoklip keretére téglalap alakú ablak kijelölése, és csak az adott ablakon belül képpont kódolás során a rendszer. Az alábbi ábra segítségével, a folyamatot szemlélteti.

![Videoklip vágása](./media/media-services-crop-video/media-services-crop-video01.png)

Tegyük fel, bemeneteként 1920 x 1080 képpont (16:9 oldalarány) felbontású, de fekete sávok (Oszlop-mezők) van, a bal és jobb oldali videók, hogy csak egy 4:3 vagy 1440 x 1080 képpont aktív videofájlt tartalmaz. MES segítségével a fekete sávra, szerkesztése, és a 1440 x 1080 régió kódolását.

Körülvágás MES a helyzet feldolgozása előre szakaszban, a körülvágási paraméterek a kódolási készletet az eredeti beviteli videó alkalmazása. Kódolása a későbbi szakaszában, és a szélesség és magasság beállítások alkalmazása a *feldolgozása előre* videó, nem pedig az eredeti videó. A beállított tervezésekor kell tennie a következő: (a) válassza ki a Körülvágás paramétereket az eredeti beviteli videó alapján, és a (b) válassza a beállításokat a levágott videó alapján kódolását. Ha nem egyezik meg a beállításokat, hogy a levágott videó kódolását, a kibocsátás nem megfelelően működnek.

A [következő](media-services-advanced-encoding-with-mes.md#encoding_with_dotnet) témakör a MES egy kódolási feladat létrehozása és megadása a kódolási tevékenység előre definiált egyéni jeleníti meg. 

## <a name="creating-a-custom-preset"></a>Egyéni készlet létrehozása

A példában az ábrán látható:

1. Eredeti bemeneti érték 1920 x 1080
1. Meg kell levágja a közepére kerül, a bemeneti keretbe 1440 x 1080 kimenete
1. Ez azt jelenti, hogy X eltolása (1920 – 1440) / 2 = 240, és egy Y eltolás nulla
1. Szélességét és magasságát, az azt jelző téglalap, hogy 1440 és 1080, illetve
1. Az kódolása terület az ask konzerv három rétegek, a megoldások 1440 x 1080, 960 x 720 és 480 x 360, illetve

###<a name="json-preset"></a>JSON beállított


    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "Crop": {
                "X": 240,
                "Y": 0,
                "Width": 1440,
                "Height": 1080
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1440,
              "Height": 1080,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1250,
              "MaxBitrate": 1250,
              "BufferWindow": "00:00:05",
              "Width": 480,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


##<a name="restrictions-on-cropping"></a>Körülvágás korlátozásai

A körülvágási funkció kialakítva kézi. A beviteli videó betöltése, amelyek segítségével válassza ki az érdeklődésre számot tartó keretek, vigye a kurzort a körülvágási téglalap határozza meg a kódolási készletet van beállítva, hogy adott videó-és egyéb elemek számára, hogy az eltolás meghatározása megfelelő szerkesztési eszköz kimutatásadatokat. Ez a funkció nem jelenti azt, hogy összetevőjét, például: automatikus észlelése és a videó beviteli fekete postaláda/pillarbox a szegélyek eltávolítása.

A körülvágási funkció következő megkötések vonatkoznak. Ha ezek nem teljesül, a tevékenység kódolása is sikertelen, vagy konzerv egy nem várt eredményt ad.

1. A fordítani és az azt jelző téglalap méretének kell elférjenek a bemeneti videó
1. Amint már említettük, a szélesség és magasság kódolása beállításainak kell megegyeznie a körülvágott kép
1. Fekvő üzemmódban rögzített videók Körülvágás vonatkozik (tehát nem vonatkozik a rögzített okostelefonon videók tartott függőlegesen vagy a fekvő üzemmódban)
1. Videoklipnek leginkább fokozatos négyzetes képpontokat rögzítése

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-step"></a>Következő lépés
 
Lásd: az Azure Media Services tanulási javaslatok, segít AMS által kínált remek funkciókról.  

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

<properties 
    pageTitle="Speciális Media Encoder szabványos kódolást" 
    description="Ez a témakör bemutatja, hogyan végezhetők el a speciális kódolást Media Encoder szabványos tevékenység készletek testre szabásával. A témakör bemutatja, hogyan Media Services .NET SDK használata egy kódolási feladatot, és a feladat létrehozásához. Azt is megtudhatja, hogy miként adja meg az kódolási projekthez egyedi készleteket." 
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
    ms.date="09/25/2016"   
    ms.author="juliako"/>


#<a name="advanced-encoding-with-media-encoder-standard"></a>Speciális Media Encoder szabványos kódolást

##<a name="overview"></a>– Áttekintés

Ez a témakör bemutatja, hogyan Media Encoder standard speciális kódolási feladatokat. A témakör bemutatja, [hogyan .NET hozhat létre egy kódolási tevékenység és a feladat, amely végrehajtja a feladat végrehajtásához használja](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet). Azt is megtudhatja, hogy miként adja meg a kódolási tevékenységhez egyéni készleteket. Elemeket, amelyek a beépített által használt listájáért lásd: [Ehhez a dokumentumhoz](https://msdn.microsoft.com/library/mt269962.aspx). 

Az egyéni készletek, amely a következő kódolási feladatokat igazolni vannak:

- [A miniatűrök készítése](media-services-custom-mes-presets-with-dotnet.md#thumbnails)
- [(Kivágási) Videoklip vágása](media-services-custom-mes-presets-with-dotnet.md#trim_video)
- [Hozzon létre egy további](media-services-custom-mes-presets-with-dotnet.md#overlay)
- [Szúrjon be egy csendes hangsáv, ha a bemeneti még nincs hang](media-services-custom-mes-presets-with-dotnet.md#silent_audio)
- [Automatikus vonja rövidebbnek letiltása](media-services-custom-mes-presets-with-dotnet.md#deinterlacing)
- [Csak hang-készletek](media-services-custom-mes-presets-with-dotnet.md#audio_only)
- [ÖSSZEFŰZ két vagy több videofájlok](media-services-custom-mes-presets-with-dotnet.md#concatenate)
- [Kép körülvágása videók Media Encoder standard](media-services-custom-mes-presets-with-dotnet.md#crop)
- [Szúrjon be egy video követése, ha a bemeneti még nincs videó](media-services-custom-mes-presets-with-dotnet.md#no_video)
- [Videó elforgatása](media-services-custom-mes-presets-with-dotnet.md#rotate_video)

##<a id="encoding_with_dotnet"></a>A Media Services .NET SDK kódolást

Az alábbi példa a Media Services .NET SDK használatával végezze el az alábbi műveleteket:

- Hozzon létre egy kódolási feladatot.
- Ismerkedés a Media Encoder szabványos kódoló mutató hivatkozás.
- Az egyéni XML- vagy JSON készlet betöltéséhez. Az XML mentheti vagy JSON [(például XML-](media-services-custom-mes-presets-with-dotnet.md#xml) vagy [JSON](media-services-custom-mes-presets-with-dotnet.md#json) egy fájlt, és használja az alábbi kódot a betöltése a fájlt.

        // Load the XML (or JSON) from the local file.
        string configuration = File.ReadAllText(fileName);  
- Egy kódolási tevékenység hozzáadása a projekthez. 
- Adja meg a kívánt beviteli eszköz.
- Hozzon létre egy kimeneti eszköz, amely tartalmazza a kódolt eszköz.
- Adja hozzá a feladat állapotának ellenőrzése a eseménykezelő.
- A feladat küldése
    
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Net;
        using System.Security.Cryptography;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Newtonsoft.Json.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using System.Web;
        using System.Globalization;
        
        namespace CustomizeMESPresests
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                // Field for service context.
                private static CloudMediaContext _context = null;
                private static MediaServicesCredentials _cachedCredentials = null;
        
                private static readonly string _mediaFiles =
                    Path.GetFullPath(@"../..\Media");
        
                private static readonly string _singleMP4File =
                    Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");
        
                static void Main(string[] args)
                {
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the chached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    // Get an uploaded asset.
                    var asset = _context.Assets.FirstOrDefault();
        
                    // Encode and generate the output using custom presets.
                    EncodeToAdaptiveBitrateMP4Set(asset);
        
                    Console.ReadLine();
                }
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
                
        
                    // Load the XML (or JSON) from the local file.
                    string configuration = File.ReadAllText("CustomPreset_JSON.json");
                
                    // Create a task
                    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                        processor,
                        configuration,
                        TaskOptions.None);
                
                    // Specify the input asset to be encoded.
                    task.InputAssets.Add(asset);
                    // Add an output asset to contain the results of the job. 
                    // This output is specified as AssetCreationOptions.None, which 
                    // means the output asset is not encrypted. 
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.None);
                
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
                
                    return job.OutputMediaAssets[0];
                }
        
                static public IAsset UploadMediaFilesFromFolder(string folderPath)
                {
                    IAsset asset = _context.Assets.CreateFromFolder(folderPath, AssetCreationOptions.None);
        
                    foreach (var af in asset.AssetFiles)
                    {
                        // The following code assumes 
                        // you have an input folder with one MP4 and one overlay image file.
                        if (af.Name.Contains(".mp4"))
                            af.IsPrimary = true;
                        else
                            af.IsPrimary = false;
        
                        af.Update();
                    }
        
                    return asset;
                }
        
        
                static public IAsset EncodeWithOverlay(IAsset assetSource, string customPresetFileName)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        
                    // Load the XML (or JSON) from the local file.
                    string configuration = File.ReadAllText(customPresetFileName);
        
                    // Create a task
                    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input assets to be encoded.
                    // This asset contains a source file and an overlay file.
                    task.InputAssets.Add(assetSource);
        
                    // Add an output asset to contain the results of the job. 
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.None);
        
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
                    return job.OutputMediaAssets[0];
                }
        

                private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine("Job state changed event:");
                    Console.WriteLine("  Previous state: " + e.PreviousState);
                    Console.WriteLine("  Current state: " + e.CurrentState);
                    switch (e.CurrentState)
                    {
                        case JobState.Finished:
                            Console.WriteLine();
                            Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
                            break;
                        case JobState.Canceling:
                        case JobState.Queued:
                        case JobState.Scheduled:
                        case JobState.Processing:
                            Console.WriteLine("Please wait...\n");
                            break;
                        case JobState.Canceled:
                        case JobState.Error:
        
                            // Cast sender as a job.
                            IJob job = (IJob)sender;
        
                            // Display or log error details as needed.
                            break;
                        default:
                            break;
                    }
                }
        
        
                private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
                    return processor;
                }
        
            }
        }


##<a name="support-for-relative-sizes"></a>Relatív méretek támogatása

Azt miniatűrjei létrehozásakor, nem kell mindig adja meg az kimeneti szélességet és magasságot képpontban. A százalékos, tartomány [1 %-os,..., 100 %-os] megadhatja őket.

###<a name="json-preset"></a>JSON beállított 
    
    "Width": "100%",
    "Height": "100%"

###<a name="xml-preset"></a>XML-beállítás
    
    <Width>100%</Width>
    <Height>100%</Height>
    
##<a id="thumbnails"></a>A miniatűrök készítése

Ebben a részben egy készletet által generált miniatűrök testreszabása. Az alábbiakban meghatározott beállítás a hogyan szeretné a fájlt, valamint a miniatűrök létrehozásához szükséges információk kódolását vonatkozó információkat tartalmazza. Bármelyikének a MES készletek dokumentált [Itt](https://msdn.microsoft.com/library/mt269960.aspx) , és a forrás szerkesztésével által generált miniatűrök.  

>[AZURE.NOTE]IGAZ, ha egy videó egy átviteli sebesség szeretne kódolni kívánt a **SceneChangeDetection** beállítást, csak a következő előre definiált lehet beállítani. Ha több-átviteli sebesség videoklipre mutató kódolni és **SceneChangeDetection** értéke igaz, a kódoló hibát jelez.  


Séma kapcsolatos tudnivalókért lásd: [Ez](https://msdn.microsoft.com/library/mt269962.aspx) a témakör.

Ellenőrizze, hogy tekintse át az [előtt megfontolandó kérdések](media-services-custom-mes-presets-with-dotnet.md#considerations) szakaszt.

###<a id="json"></a>JSON beállított


    {
      "Version": 1.0,
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": "true",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 4500,
              "MaxBitrate": 4500,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "ReferenceFrames": 3,
              "EntropyMode": "Cabac",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
       
            }
          ],
          "Type": "H264Video"
        },
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "{Best}",
          "Type": "JpgImage"
        },
        {
          "PngLayers": [
            {
              "Type": "PngLayer",
              "Width": 640,
              "Height": 360,
            }
          ],
          "Start": "00:00:01",
          "Step": "00:00:10",
          "Range": "00:00:58",
          "Type": "PngImage"
        },
        {
          "BmpLayers": [
            {
              "Type": "BmpLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "10%",
          "Step": "10%",
          "Range": "90%",
          "Type": "BmpImage"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "PngFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "BmpFormat"
          }
        },
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


###<a id="xml"></a>XML-beállítás


    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <SceneChangeDetection>true</SceneChangeDetection>
          <H264Layers>
            <H264Layer>
              <Bitrate>4500</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>4500</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
        <JpgImage Start="{Best}">
          <JpgLayers>
            <JpgLayer>
              <Width>640</Width>
              <Height>360</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
        <BmpImage Start="10%" Step="10%" Range="90%">
          <BmpLayers>
            <BmpLayer>
              <Width>640</Width>
              <Height>360</Height>
            </BmpLayer>
          </BmpLayers>
        </BmpImage>
        <PngImage Start="00:00:01" Step="00:00:10" Range="00:00:58">
          <PngLayers>
            <PngLayer>
              <Width>640</Width>
              <Height>360</Height>
            </PngLayer>
          </PngLayers>
        </PngImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <BmpFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <PngFormat />
        </Output>
      </Outputs>
    </Preset>

###<a name="considerations"></a>Megfontolandó szempontok

A következő érvényesek:

- Kezdés/lépés vagy tartomány explicit időbélyegeket is használja azt feltételezi, hogy a bemeneti forrás 1 perc hosszú.
- JPG/Png/BmpImage elemek indítási, lépésben van, és tartomány karakterlánc attribútumok – ezek bírálatként értelmezhetők:

    - Keret számot, ha még nem negatív egész számok, például "indítása": "120",
    - Ha kifejezett % suffixed, időtartam és a forrásoszlop, például "indítása" relatív: "15 %", vagy
    - Ha óó kifejezett időbélyeg... formátum, például "indítása": "00: 01:00"

    Keverjen ki, és közben jelölések kérjük megfelelően.
    
    Ezenkívül kezdő speciális makró is támogatja: {ajánlott}, amely megkísérli a tartalom Megjegyzés az első "érdekes" képkocka meghatározása: (lépés és tartomány függvény figyelmen kívül hagyja kezdő {legjobb} beállítása esetén)
    
    - Alapértelmezett: Indítása: {legjobb}
- Kimeneti formátum kell kell explicit módon megadva minden kép formázása: Jpg/Png/BmpFormat. Ha igen, MES felel meg a JpgFormat JpgVideo és így tovább. OutputFormat vezet be az új kép-kodek adott makró: {Index}, mely kell lennie bemutatása (egyszer és csak egyszer) kép kimeneti formátumban.

##<a id="trim_video"></a>(Kivágási) Videoklip vágása

Ebben a szakaszban a kódoló beépített ClipArt, és hol található a bemeneti, egy úgynevezett Emeletes vagy igény szerinti fájl beviteli videó módosításával kapcsolatos folytatott beszélgetést. A kódoló is használható ClipArt, vagy a körülvágási eszköz, amely élő adatfolyam archivált vagy rögzített – ehhez a részletek érhetők el a [blogban](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).

Levághatja a videók, bármelyikének a MES készletek dokumentált [ide](https://msdn.microsoft.com/library/mt269960.aspx) , és módosítsa a **források** elem (mint alább). A kezdő időpont értékét kell felel meg a bemeneti videó abszolút időbélyegeket. Például ha a bemeneti videó az első képkocka 12:00:10.000, az időbélyegző, majd kezdő időpont kell legalább 12:00:10.000 és nagyobb. Az alábbi példában feltételezzük, hogy rendelkezik-e a bemeneti videó időbélyeg kezdő nulla. **Adatforrások** szeretné elhelyezni a készletet az elején. 
 
###<a id="json"></a>JSON beállított
    
    {
      "Version": 1.0,
      "Sources": [
        {
          "StartTime": "00:00:04",
          "Duration": "00:00:16"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1280,
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
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1500,
              "MaxBitrate": 1500,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1000,
              "MaxBitrate": 1000,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 650,
              "MaxBitrate": 650,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 400,
              "MaxBitrate": 400,
              "BufferWindow": "00:00:05",
              "Width": 320,
              "Height": 180,
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

###<a name="xml-preset"></a>XML-beállítás
    
Levághatja a videók, bármelyikének a MES készletek dokumentált [ide](https://msdn.microsoft.com/library/mt269960.aspx) , és módosítsa a **források** elem (mint alább).

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source StartTime="PT4S" Duration="PT14S"/>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>3400</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>3400</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>2250</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>2250</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1500</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1500</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1000</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1000</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>650</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>650</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>400</Bitrate>
              <Width>320</Width>
              <Height>180</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>400</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>

##<a id="overlay"></a>Hozzon létre egy további

Media Encoder szokásos lehetővé teszi a meglévő videó alakzatot kép átfedő. Jelenleg az alábbi formátumok támogatottak: png, jpg, a gif vagy bmp. Az alábbiakban meghatározott beállítás látható egy egyszerű egy videó átfedő.

Mellett egy előre definiált fájl definiálásával, akkor is, hogy a Media Services, hogy az eszköz a fájlt az átfedő kép, mely fájl már a videó forrás alakzatot, amelynek meg szeretné átfedő a képet. A videofájl nem az **elsődleges** fájl lehet. 

A fenti példa a .NET határozza meg a két: **UploadMediaFilesFromFolder** és **EncodeWithOverlay**. A UploadMediaFilesFromFolder függvény fel a fájlokat (például BigBuckBunny.mp4 és Image001.png) mappára, és a mp4-fájlt, az elsődleges fájl a digitális eszköz kiválasztása a állítja be. A **EncodeWithOverlay** függvény használja az egyéni előre definiált fájlt meg (például a készletet, amely követi) kapott a kódolási feladat létrehozásához. 

>[AZURE.NOTE]Aktuális korlátozások:
>
>Az átfedő Áttetszőség beállítása nem támogatott.
>
>A forrásfájl videó és a átfedő képfájl kell lenniük a ugyanazon eszközre, és a videofájlt kell legyen az elsődleges fájl a digitális eszköz kiválasztása.

###<a name="json-preset"></a>JSON beállított
    
    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "VideoOverlay": {
              "Position": {
                "X": 100,
                "Y": 100,
                "Width": 100,
                "Height": 50
              },
              "AudioGainLevel": 0.0,
              "MediaParams": [
                {
                  "OverlayLoopCount": 1
                },
                {
                  "IsOverlay": true,
                  "OverlayLoopCount": 1,
                  "InputLoop": true
                }
              ],
              "Source": "Image001.png",
              "Clip": {
                "Duration": "00:00:05"
              },
              "FadeInDuration": {
                "Duration": "00:00:01"
              },
              "FadeOutDuration": {
                "StartTime": "00:00:03",
                "Duration": "00:00:04"
              }
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
              "Bitrate": 1045,
              "MaxBitrate": 1045,
              "BufferWindow": "00:00:05",
              "ReferenceFrames": 3,
              "EntropyMode": "Cavlc",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Type": "CopyAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}{Extension}",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


###<a name="xml-preset"></a>XML-beállítás
    
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source>
          <Streams />
          <Filters>
            <VideoOverlay>
              <Source>Image001.png</Source>
              <Clip Duration="PT5S" />
              <FadeInDuration Duration="PT1S" />
              <FadeOutDuration StartTime="PT3S" Duration="PT4S" />
              <Position X="100" Y="100" Width="100" Height="50" />
              <Opacity>0</Opacity>
              <AudioGainLevel>0</AudioGainLevel>
              <MediaParams>
                <MediaParam>
                  <IsOverlay>false</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>false</InputLoop>
                </MediaParam>
                <MediaParam>
                  <IsOverlay>true</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>true</InputLoop>
                </MediaParam>
              </MediaParams>
            </VideoOverlay>
          </Filters>
          <Pad>true</Pad>
        </Source>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>1045</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>0</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cavlc</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1045</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <CopyAudio />
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}{Extension}">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>


##<a id="silent_audio"></a>Szúrjon be egy csendes hangsáv, ha a bemeneti még nincs hang

Alapértelmezés szerint ha küldi el a csak a videó és hang nélkül tartalmazó kódoló egy bemeneti majd a kimenet eszköz tartalmaz csak videó adatokat tartalmazó fájlt. Néhány lejátszó nem lehet ilyen kimenő adatfolyamok kezelheti. E beállítás használatával a kódoló csendes hangsáv felvenni az adott helyzetben kimeneti kényszerítése.

Hogy a kódoló kapcsol tartalmazó csendes hangsáv, amikor a bemeneti van nélkül hang tárgyi eszköz, adja meg a "InsertSilenceIfNoAudio" értéket.

Bármelyikének a MES készletek dokumentált [ide](https://msdn.microsoft.com/library/mt269960.aspx), és ellenőrizze a következő módosítás:

###<a name="json-preset"></a>JSON beállított

    {
      "Channels": 2,
      "SamplingRate": 44100,
      "Bitrate": 96,
      "Type": "AACAudio",
      "Condition": "InsertSilenceIfNoAudio"
    }

###<a name="xml-preset"></a>XML-beállítás

    <AACAudio Condition="InsertSilenceIfNoAudio">
      <Channels>2</Channels>
      <SamplingRate>44100</SamplingRate>
      <Bitrate>96</Bitrate>
    </AACAudio>

##<a id="deinterlacing"></a>Automatikus vonja rövidebbnek letiltása

Ügyfelek nem kell tennie semmit sem kell automatikusan vonja váltott soros váltott soros csoportban tartalmát azok tetszés. (Alapértelmezett) automatikus vonja félképekre bekapcsolt állapotában a MES az automatikus észlelési váltott soros keretek tartalmaz, és csak vonja interlaces kereteket tartalmazó váltott megjelölve.

Kikapcsolhatja az automatikus vonja félképekre. Ez a beállítás nem ajánlott.

###<a name="json-preset"></a>JSON beállított
    
    "Sources": [
    {
     "Filters": {
        "Deinterlace": {
          "Mode": "Off"
        }
      },
    }
    ]

###<a name="xml-preset"></a>XML-beállítás
    
    <Sources>
    <Source>
      <Filters>
        <Deinterlace>
          <Mode>Off</Mode>
        </Deinterlace>
      </Filters>
    </Source>
    </Sources>


##<a id="audio_only"></a>Csak hang-készletek

Ez a szakasz szemlélteti két csak hangot tartalmazó MES készletek: AAC hang- és AAC jó minőségű hang.

###<a name="aac-audio"></a>AAC hang 

    {
      "Version": 1.0,
      "Codecs": [
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
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

###<a name="aac-good-quality-audio"></a>A hatékony hangminőség AAC

    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 192,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

##<a id="concatenate"></a>ÖSSZEFŰZ két vagy több videofájlok

Az alábbi példa bemutatja, hogyan hozhat létre egy készletet való összefűzésére két vagy több videofájlokat. Az a leggyakoribb helyzet esetén a fejléc vagy egy hozzáadása a fő videót szeretne. A tervezett használata a közös szerkesztés alatt álló videofájlok megosztásakor a Tulajdonságok (képfelbontás, keret ráta, hangsáv darab stb.). Meg kell gondoskodik a nem videók különböző keret kamatlábak, vagy különböző számú zeneszámok keverjen ki.

###<a name="requirements-and-considerations"></a>Követelmények és kapcsolatos szempontok

- Beviteli videók csak egy hangsáv vannak.
- Beviteli videók vannak az összes azonos keret mértékű.
- A videók feltöltése külön eszközök be kell, és a videók beállítása az egyes eszközök elsődleges fájlként.
- Kell tudnia a videók időtartamát.
- Az előre beállított NMÉ feltételezi, hogy a bemeneti videók Kiindulás időbélyeg nulla. Módosítania kell a kezdő értékeket a videók esetén más kezdő időbélyeg, akárcsak általában az élő archívumok.
- A JSON készletet az eszközkód értékekre, a bemeneti eszközök közvetlen hivatkozás teszi.
- A minta kódot feltételezi, hogy a JSON készletet mentését helyi fájlként, például "C:\supportFiles\preset.json". Azt is feltételezi, hogy két eszközök hozta két videó fájlfeltöltéskor, és hogy eszközkód eredő értékeket.
- A kódtöredék és JSON beállított látható, két videofájlok szereplő. A kettőnél több videókhoz szerint bővítheti azt:

    1. Hívás a feladatot. InputAssets.Add() többször sorrendben további videók, hozzáadni.
    2. Több tételt, ugyanabban a sorrendben hozzáadásával megfelelő tétele a JSON-ban, a "Források" elem szerkesztése. 


###<a name="net-code"></a>.NET-kód

    
    IAsset asset1 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:606db602-efd7-4436-97b4-c0b867ba195b").FirstOrDefault();
    IAsset asset2 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e").FirstOrDefault();
    
    // Declare a new job.
    IJob job = _context.Jobs.Create("Media Encoder Standard Job for Concatenating Videos");
    // Get a media processor reference, and pass to it the name of the 
    // processor to use for the specific task.
    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
    
    // Load the XML (or JSON) from the local file.
    string configuration = File.ReadAllText(@"c:\supportFiles\preset.json");
    
    // Create a task
    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
        processor,
        configuration,
        TaskOptions.None);
    
    // Specify the input videos to be concatenated (in order).
    task.InputAssets.Add(asset1);
    task.InputAssets.Add(asset2);
    // Add an output asset to contain the results of the job. 
    // This output is specified as AssetCreationOptions.None, which 
    // means the output asset is not encrypted. 
    task.OutputAssets.AddNew("Output asset",
        AssetCreationOptions.None);
    
    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
    job.Submit();
    job.GetExecutionProgressTask(CancellationToken.None).Wait();

###<a name="json-preset"></a>JSON beállított

Frissítse a készletet az ÖSSZEFŰZ kívánt eszközök azonosítók, és a megfelelő időben szakasz minden videoklipet egyéni.

    {
      "Version": 1.0,
      "Sources": [
        {
          "AssetID": "606db602-efd7-4436-97b4-c0b867ba195b",
          "StartTime": "00:00:01",
          "Duration": "00:00:15"
        },
        {
          "AssetID": "a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e",
          "StartTime": "00:00:02",
          "Duration": "00:00:05"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": true,
          "H264Layers": [
            {
              "Level": "auto",
              "Bitrate": 1800,
              "MaxBitrate": 1800,
              "BufferWindow": "00:00:05",
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
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

##<a id="crop"></a>Kép körülvágása videók Media Encoder standard

A [Körülvágás videók Media Encoder standard](media-services-crop-video.md) témakörben talál.

##<a id="no_video"></a>Szúrjon be egy video követése, ha a bemeneti még nincs videó

Alapértelmezés szerint ha küldi el a kódoló csak hang, és a nincs kép tartalmazó egy bemeneti majd a kimeneti eszköz tartalmaz csak hangos adatokat tartalmazó fájlt. Néhány lejátszó, beleértve Azure Media Player (lásd: [a](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) nem lehet ilyen adatfolyamok kezelheti. E beállítás használatával a fekete-fehér videó követése hozzáadása a kimenet adott esetben a kódoló kényszerítése. 

>[AZURE.NOTE]A kimenő videokép nyomon követése beszúrása kódoló kényszerítése növeli a kimenet méretét eszköz, és a költség egyúttal kapcsolatban felmerülő a kódolási tevékenység. Ellenőrizze, hogy a eredő növekedés csak egy mérsékelt hatással van a havi költségek vizsgálatok kell futtatnia.

### <a name="inserting-video-at-only-the-lowest-bitrate"></a>A csak az legalacsonyabb átviteli Videó beszúrása

Tegyük fel, hogy használni egy több átviteli sebesség kódolást például előre definiált ["H264 több átviteli sebesség 720p"](https://msdn.microsoft.com/library/mt269960.aspx) kódolása a teljes bemeneti katalógus folyamatos, kombinálja a videó és indíthatnak csak hangot tartalmazó fájlokat tartalmazó. Ebben az esetben ha a bemeneti nincs videó, érdemes a kódoló szeretne beszúrni a fekete-fehér videó követése csak az legalacsonyabb átviteli, nem pedig a Videó beszúrása az összes kimenő átviteli sebesség kényszerítése. Cél, meg kell adni a "InsertBlackIfNoVideoBottomLayerOnly" jelölő.

Bármelyikének a MES készletek dokumentált [ide](https://msdn.microsoft.com/library/mt269960.aspx), és ellenőrizze a következő módosítás:

#### <a name="json-preset"></a>JSON beállított

    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideoBottomLayerOnly",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>XML-beállítás

    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <StretchMode>AutoSize</StretchMode>
    <Condition>InsertBlackIfNoVideoBottomLayerOnly</Condition>

### <a name="inserting-video-at-all-output-bitrates"></a>Videó beszúrása az összes kimenő bitrates

Tegyük fel, hogy használni egy több átviteli sebesség kódolást például előre definiált ["H264 több átviteli sebesség 720p](https://msdn.microsoft.com/library/mt269960.aspx) kódolása a teljes bemeneti katalógus folyamatos, kombinálja a videó és indíthatnak csak hangot tartalmazó fájlokat tartalmazó. Ebben az esetben ha a bemeneti nincs videó, érdemes a kódoló egyáltalán beszúrása a fekete-fehér videó nyomon követése a kimeneti bitrates kényszerítése. Ezzel a kimeneti biztosítható, hogy az összes homogén részletez videó sávok száma és zeneszámok olyan eszközök. Cél, meg kell adni a "InsertBlackIfNoVideo" jelölő.

Bármelyikének a MES készletek dokumentált [ide](https://msdn.microsoft.com/library/mt269960.aspx), és ellenőrizze a következő módosítás:

#### <a name="json-preset"></a>JSON beállított

    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideo",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>XML-beállítás
    
    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <StretchMode>AutoSize</StretchMode>
    <Condition>InsertBlackIfNoVideo</Condition>

##<a id="rotate_video"></a>Videó elforgatása

[Media Encoder szabványos](media-services-dotnet-encode-with-media-encoder-standard.md) 0/90/180 270 által többszöröseire Elforgatás támogatja. Az alapértelmezett működés "Automatikus", ahol próbál meg a Forgatás metaadatok észleli a bejövő videofájlban, és azt kompenzálja. Többek között a következő **forrásokból** elemet az egyik az Elhelyezés definiált [Itt](http://msdn.microsoft.com/library/azure/mt269960.aspx):

### <a name="json-preset"></a>JSON beállított

    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [
    
    ...
### <a name="xml-preset"></a>XML-beállítás

    <Sources>
        <Source>
          <Streams />
          <Filters>
            <Rotation>90</Rotation>
          </Filters>
        </Source>
    </Sources>

További információ [Ebben](https://msdn.microsoft.com/library/azure/mt269962.aspx#PreserveResolutionAfterRotation) a témakörben további információt a kódoló értelmezése a szélesség és magasság beállítások a készletet, a Forgatás támogatás elindításakor a.

A Forgatás metaadat-alapú figyelmen kívül hagyja, ha ez a videó a bevitt megtalálható kódoló jelezheti a "0" értéket is használhatja. 


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Lásd még: 

[Media Services kódolási – áttekintés](media-services-encode-asset.md)

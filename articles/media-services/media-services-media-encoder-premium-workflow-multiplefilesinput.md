<properties
    pageTitle="Több beviteli fájlok és az összetevő-tulajdonságok használata prémium Encoder |} Microsoft Azure"
    description="Ebből a témakörből megtudhatja, hogyan setRuntimeProperties segítségével több beviteli fájlokat használ, és egyéni adatot átadni a Media Encoder prémium munkafolyamat media processzor."
    services="media-services"
    documentationCenter=""
    authors="xpouyat"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"  
    ms.author="xpouyat;anilmur;juliako"/>

# <a name="using-multiple-input-files-and-component-properties-with-premium-encoder"></a>A több bemeneti fájlok és összetevő-tulajdonságok használata prémium kódoló

## <a name="overview"></a>– Áttekintés

Vannak olyan esetek, amelyben előfordulhat, hogy testre összetevő tulajdonságai, adja meg a ClipArt lista XML-tartalmat, vagy szükséges több beviteli fájlok küldése, amikor a **Media Encoder prémium munkafolyamat** media processzor tevékenység elküldése. Néhány példa a következők:

- A video- és beállítása a szöveget (például az aktuális dátum) értéket minden beviteli videó futásidőben felirataként a szöveget.
- Testreszabás a ClipArt lista XML-(adja meg egy vagy több adatforrás-fájlt, vagy azok nélkül elrejtést stb.).
- Miközben a videó felirataként a videó a bemeneti egy emblémaképhez.

Ha tudja, hogy a módosítani kívánt egyes tulajdonságai az munkafolyamatokban a feladat létrehozásához vagy több beviteli fájlokat küldeni, kell használnia, konfigurációs karakterlánc **Media Encoder prémium munkafolyamat** engedélyezni szeretné tartalmazó **setRuntimeProperties** és/vagy **transcodeSource**. Ebből a témakörből megtudhatja, hogyan használhatók.


## <a name="configuration-string-syntax"></a>Konfigurációs karakterlánc szintaxisa

A konfiguráció karakterlánc módosítani szeretné a kódolási tevékenység használja az XML-dokumentum, amely a következőképpen néz ki:

    <?xml version="1.0" encoding="utf-8"?>
    <transcodeRequest>
      <transcodeSource>
      </transcodeSource>
      <setRuntimeProperties>
        <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
      </setRuntimeProperties>
    </transcodeRequest>


Az alábbiakban a C# kód, amely fájlból olvassa be az XML-konfigurációja, és átadja a feladat egy feladatot:

    XDocument configurationXml = XDocument.Load(xmlFileName);
    IJob job = _context.Jobs.CreateWithSingleTask(
                                                  "Media Encoder Premium Workflow",
                                                  configurationXml.ToString(),
                                                  myAsset,
                                                  "Output asset",
                                                  AssetCreationOptions.None);


## <a name="customizing-component-properties"></a>Összetevő-tulajdonságok testreszabása  

### <a name="property-with-a-simple-value"></a>Egyszerű értéket tartalmazó tulajdonság
Bizonyos esetekben akkor lehet hasznos, ha testre szeretné szabni a munkafolyamat-fájlt, a Media Encoder prémium munkafolyamat hajtja végre fog együtt összetevő tulajdonság.

Tegyük fel, megtervezni a munkafolyamat adott átfedi a szöveget a videók, és a szöveget (például az aktuális dátum) értendő futásidőben kell állítani. Ez a szöveg az új érték megadása a szöveg tulajdonsága az átfedő összetevő állítható be a kódolási tevékenységből küldésével teheti meg. Ez az eljárás használatával más (például a pozíció vagy az átfedő színét, az átviteli AVC encoder, stb.) az munkafolyamatokban összetevő tulajdonságainak módosítása.

a munkafolyamat összetevői tulajdonság felülbírálása **setRuntimeProperties** használják.

Példa:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
          <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
          <property propertyPath="Optional Overlay/Overlay/filename" value="MyLogo.png"/>
          <property propertyPath="Optional Text Overlay/Text To Image Converter/text" value="Today is Friday the 13th of May, 2016"/>
      </setRuntimeProperties>
    </transcodeRequest>


### <a name="property-with-an-xml-value"></a>XML értékkel tulajdonság

A tulajdonság üres, amely egy XML-érték vár, beágyazására használatával `<![CDATA[ and ]]>`.

Példa:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

>[AZURE.NOTE]Ügyeljen arra, hogy nem helyezi el kocsi vissza csak utána `<![CDATA[`.


### <a name="propertypath-value"></a>propertyPath érték

Az előző példákban a propertyPath lett "/ médiafájl beviteli/fájlnév" vagy "/ inactiveTimeout" vagy "clipListXml".
Ez a általában az összetevő nevére, majd a tulajdonság neve. Az elérési út beállíthatja, hogy több vagy kevesebb szintek, például "/ primarySourceFile" (mivel a tulajdonságot a legfelső szintű a munkafolyamatot) vagy "/ videó feldolgozás/grafikus átfedő/áttetszőség" (mivel az átfedő csoport).    

Jelölje be az elérési utat és a tulajdonság neve, használja a művelet gombot, amely közvetlenül az egyes tulajdonság. Kattintson a művelet gombra, és válassza a **Szerkesztés**. Meg kell követnie a tulajdonságot, valamint a közvetlenül fölötte, a névtér tényleges nevét.

![Művelet/szerkesztése](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture6_actionedit.png)

![A tulajdonság](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture7_viewproperty.png)

## <a name="multiple-input-files"></a>Több bemeneti fájlok

Minden feladat, amely a **Media Encoder prémium munkafolyamat** elküldése szükséges két eszközök:

- A legelső *Munkafolyamat tárgyi eszköz* egy munkafolyamat-fájlt tartalmazó. Munkafolyamat-fájlok tervezhet a [Munkafolyamat-Tervező](media-services-workflow-designer.md)használatával.
- A második egyik *Media eszköz* , amely tartalmazza a használni kívánt kódolását médiafájl lesz.

Több médiafájlokat a **Media Encoder prémium munkafolyamat** kódoló küld üzenetet, ha az alábbi korlátozások vonatkoznak:

- A médiafájlok ugyanazon *Media eszköz*kell lennie. Több multimédiás elemeket használata nem támogatott.
- Meg kell adnia az elsődleges fájl a digitális eszköz (ideális esetben ez a a fő videofájl, hogy meg kell adnia a kódoló feldolgozása).
- A processzor szeretné az **setRuntimeProperties** és/vagy **transcodeSource** elemet tartalmazó konfigurációs adatok átadni szükség.
  - a fájlnév és egy másik tulajdonság a munkafolyamat részeihez felülbírálása **setRuntimeProperties** használják.
  - Adja meg a ClipArt lista XML-tartalom **transcodeSource** szolgál.

A munkafolyamat kapcsolatok:

 - Ha egy vagy több multimédiás fájl beviteli összetevők és terv használatával **setRuntimeProperties** használatával adja meg a fájl nevét, majd nem kapcsolódik az elsődleges fájl összetevő PIN-kód őket. Győződjön meg arról, hogy az elsődleges fájl objektumra, és a multimédiás fájl Input(s) között nincs kapcsolat.
 - Ha szeretne használni a ClipArt lista az XML és az egyik Media forrásösszetevőt, majd csatlakoztathatja mindkét együtt.

![Az elsődleges forrásfájl multimédiás fájl beviteli nincs kapcsolat](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture0_nopin.png)

*Ha a setRuntimeProperties a fájlnév tulajdonság, nincs kapcsolat az elsődleges fájlból a multimédiás fájl beviteli komponens.*

![XML-forráslista ClipArt ClipArt listából kapcsolat](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture1_pincliplist.png)

*Csatlakozás ClipArt lista XML adathordozóról és transcodeSource használható.*


### <a name="clip-list-xml-customization"></a>ClipArt-lista XML testreszabása
Megadhatja a ClipArt-lista XML-munkafolyamaton belül a futásidőben a konfigurációs karakterláncban XML **transcodeSource** használatával. Ehhez a ClipArt-lista XML PIN-kódot kell csatlakoznia az Media forrásösszetevőt az munkafolyamatokban.

    <?xml version="1.0" encoding="utf-16"?>
      <transcodeRequest>
        <transcodeSource>
          <clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>video-part1.mp4</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>video-part1.mp4</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
          </clipList>
        </transcodeSource>
        <setRuntimeProperties>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

Ha szeretné használni ezt a tulajdonságot a kimeneti fájl neve "a kifejezések használatával /primarySourceFile adja meg, majd azt javasoljuk a ClipArt-lista XML áthaladó megegyezik egy tulajdonság, *Miután* a /primarySourceFile tulajdonság zsugorítása a ClipArt-lista /primarySourceFile beállítása írhatók felül.

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="c:\temp\start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>c:\temp\start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>c:\temp\start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

A további keret pontos szűrése:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <trim>
                  <inPoint fps="25">00:00:05:24</inPoint>
                  <outPoint fps="25">00:00:10:24</outPoint>
                </trim>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
               <trim>
                  <inPoint fps="25">00:00:05:24</inPoint>
                  <outPoint fps="25">00:00:10:24</outPoint>
                </trim>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>


## <a name="example"></a>Példa

Fontolja meg egy példát, amelyben meg szeretné egy emblémaképhez a videó a bemeneti átfedő, miközben a videó. Ebben a példában a bemeneti videó neve "MyInputVideo.mp4" és az embléma neve "MyLogo.png". Az alábbi lépéseket kell végrehajtani:

- A munkafolyamat-eszköz létrehozása a munkafolyamat-fájl segítségével (lásd az alábbi példa).
- Hozzon létre egy Media eszközt, két fájlokat tartalmazó: MyInputVideo.mp4, valamint az elsődleges fájl MyLogo.png.
- Küldje el a fenti beviteli eszközök Media Encoder prémium munkafolyamat media processzor egy tevékenység, és adja meg a következő konfigurációs karakterláncot.

Konfigurációs:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
          <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
          <property propertyPath="Media File Input Logo/filename" value="MyLogo.png" />
        </setRuntimeProperties>
      </transcodeRequest>


A fenti példában a videó a fájl nevét a rendszer elküldi a multimédiás fájl beviteli összetevő, és a primarySourceFile tulajdonságot. Az embléma fájl nevét a rendszer elküldi a grafikus átfedő összetevő csatlakozik egy másik Media fájl beviteli.

>[AZURE.NOTE]A videó fájl nevét a rendszer elküldi a primarySourceFile tulajdonság. A hiba okát környezetbe ezt a tulajdonságot az munkafolyamatokban a megfelelő kimeneti fájlnév kifejezésekkel, például készítéséhez.


### <a name="step-by-step-workflow-creation-that-overlays-a-logo-on-top-of-the-video"></a>Lépésenkénti munkafolyamat létrehozása, amely lefedi a fölött a videó embléma     

Íme egy munkafolyamatot, amely megnyitja a két bemeneti adataiként létrehozásának lépései: videó és a képet. Azt fogja átfedő fölött a videó a képet.

Nyissa meg **a munkafolyamat-Tervező** , és kattintson a **fájl** > **Új munkaterület** > **Alakítható át tervrajz**.

Az új munkafolyamat három elem látható:

- Elsődleges forrásfájl
- ClipArt-lista XML
- Kimeneti fájl/eszköz  

![Új kódolási munkafolyamat](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture9_empty.png)

*Új kódolási munkafolyamat*


Annak érdekében, hogy fogadja el a beviteli médiafájl, indítsa el a multimédiás fájl beviteli összetevő hozzáadásával. Összetevő a munkafolyamat hozzáadásához keresse meg azt a tárházba keresőmezőbe, és a kívánt bejegyzést húzza az alakzatot a Lekérdezéstervező ablakban.

Ezután adja hozzá a munkafolyamat tervezése használandó a videofájlt. Ehhez kattintson a háttér ablaktáblán, a munkafolyamat-Tervező, és keresse meg a tulajdonság jobb oldali ablaktáblán kattintson az elsődleges forrásfájl tulajdonság. Kattintson a mappaikonra, és jelölje ki a megfelelő videofájlt.

![Elsődleges fájl forrás](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture10_primaryfile.png)

*Elsődleges fájl forrás*


Ezután adja meg a multimédiás fájl beviteli összetevő a videofájlt.   

![Médiafájl bemeneti forrása](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture11_mediafileinput.png)

*Médiafájl bemeneti forrása*


Amint ez történik, a multimédiás fájl beviteli összetevő vizsgálja meg a fájlt, és feltölteni a fájlt, akkor ellenőrzött megfelelően a kimeneti PIN.

A következő lépésként hozzáadása egy "videó adatok típusát Updater" a szín terület Rec.709 megadásához. Adja hozzá a "videó konverter" adatok elrendezés/elrendezéstípust beállított = konfigurálható sík. Ezt a formátumot, amely az átfedő összetevő adatforrásként tehetők a videokép konvertálja.

![Video-adatok típusát Updater és konverter](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter.png)

*Videó adatainak típus Updater és konverter*

![Elrendezés típusa = konfigurálható sík](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter2.png)

*Elrendezés típusa konfigurálható sík.*

Ezután a videó átfedő összetevő hozzáadása, és csatlakozás (nem tömörített) a videó PIN-kódot a (nem tömörített) videó PIN-kódot a multimédiás fájl beviteli.

Adja hozzá a másik Media fájl bemeneti (az emblémát fájl betöltése), kattintson a összetevő, és nevezze át a "Multimédiás fájl beviteli embléma", és a kép (például .png fájlok) jelölje ki a fájl tulajdonság. A kép nem tömörített PIN-kód csatlakozni az átfedés nem tömörített kép PIN-kódot.

![Átfedő összetevő és a kép fájl forrás](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture13_overlay.png)

*Átfedő összetevő és a kép fájl forrás*


Ha helyét a videó emblémáról módosítani szeretné (például érdemes lehet helyezze el a videó a bal felső sarokban elhagyja a 10 %-os), jelölje be a "Kézi bemenet" jelölőnégyzetet. Művelet, mert a multimédiás fájl beviteli használatával adja meg az embléma fájlt az átfedő összetevőt.

![Átfedő pozíció](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture14_overlay_position.png)

*Átfedő pozíció*


A video-adatfolyam H.264 kódolni, adja hozzá a videó Encoder AVC és AAC encoder összetevőket a tervezési felületre. Csatlakozás a PIN-kódok.
Állítsa be a AAC kódoló, és jelölje ki a hang formátum átalakítás/készletet: 2.0-s (L, R).

![Hang- és kódolók](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture15_encoders.png)

*Hang- és kódolók*


Most az **ISO Mpeg-4-es Multiplexer** és a **Kimeneti fájl** összetevők hozzáadása és csatlakozás a PIN-kódok látható módon.

![MP4 multiplexer és a kimeneti fájl](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture16_mp4output.png)

*MP4 multiplexer és a kimeneti fájl*


Kell beállítania a kimeneti fájl nevét. Kattintson a **Kimeneti fájl** összetevő, és a kifejezést, a fájl szerkesztése:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_withoverlay.mp4

![Kimeneti fájlnév](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture17_filenameoutput.png)

*Kimeneti fájlnév*

Helyi meghajtóra, hogy ellenőrizze, hogy megfelelően fut-e a munkafolyamat fut.

Miután befejeződött, futtathatja az Azure Media Services.

Első lépésként előkészítése az Azure Media Services-két fájlok eszköz: a videofájlt és az embléma. Ehhez a .NET vagy a REST API-t használja. Akkor is ehhez az Azure portál és az [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE) segítségével.

Ebből az oktatóanyagból megtudhatja, hogy hogyan AMSE a eszközeinek kezelésében. Kétféleképpen adhat további fájlokat tárgyi eszköz:

- Hozzon létre egy helyi mappát, másolja a két fájlokat, és húzással helyezze a mappát, az **eszköz** fülre.
- Töltse fel a videofájlt eszközként, az eszköz adatainak megjelenítése, nyissa meg a fájlok fülre, és töltse fel egy további fájlt (embléma).

>[AZURE.NOTE]Ellenőrizze, hogy módosítani szeretné egy elsődleges fájl az eszköz (a fő videofájl).

![AMSE fájlok eszköz](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture18_assetinamse.png)

*AMSE fájlok eszköz*


Jelölje ki az eszközt, és válassza a prémium Encoder kódolni azt. Töltse fel a munkafolyamatot, és jelölje ki.

Kattintson a gombra a processzor adatot átadni, és adja hozzá a következő XML a futtatókörnyezet beállításához:

![A AMSE prémium kódoló](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture19_amsepremium.png)

*A AMSE prémium kódoló*


Ezután másolja az alábbi XML-adatok. Adja meg a videó a fájl nevét a multimédiás fájl beviteli és a primarySourceFile szüksége. Adja meg a fájl nevére, az embléma a túl.

    <?xml version="1.0" encoding="utf-16"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
          <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

![setRuntimeProperties](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture20_amsexmldata.png)

*setRuntimeProperties*


A .NET SDK használatával létrehozása és futtatása a tevékenységet, ha az XML-adatok konfigurációs karakterláncként átadandó rendelkezik.

    public ITask AddNew(string taskName, IMediaProcessor mediaProcessor, string configuration, TaskOptions options);

A feladat befejezése után, a kimeneti eszköz MP4 fájlban jeleníti meg az átfedés!

![A videó a átfedő](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture21_resultoverlay.png)

*A videó a átfedő*


Letöltheti a minta munkafolyamat [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).


## <a name="see-also"></a>Lásd még:

- [Az Azure Media Services kódolást prémium bemutatása](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

- [Az Azure Media Services használatáról a prémium kódolást](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

- [Az Azure Media Services kódolási igény szerinti tartalom](media-services-encode-asset.md#media_encoder_premium_workflow)

- [Media Encoder prémium munkafolyamat formátumok és a kodekekről](media-services-premium-workflow-encoder-formats.md)

- [Mintafájlok munkafolyamat](https://github.com/AzureMediaServicesSamples/Encoding-Presets/tree/master/VoD/MediaEncoderPremiumWorkfows)

- [Azure Media Services Explorer eszköz](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

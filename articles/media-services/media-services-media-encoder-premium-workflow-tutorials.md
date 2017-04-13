<properties 
    pageTitle="Avanced Media Encoder prémium munkafolyamat oktatóanyagok" 
    description="A dokumentum, amely Media Encoder prémium munkafolyamattal speciális feladatok elvégzéséhez, illetve hogy miként összetett munkafolyamatok létrehozását a munkafolyamat-Tervező forgatókönyvek tartalmaz." 
    services="media-services" 
    documentationCenter="" 
    authors="xstof" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016"  
    ms.author="xstof;xpouyat;juliako"/>

#<a name="advanced-media-encoder-premium-workflow-tutorials"></a>Speciális Media Encoder prémium munkafolyamat oktatóanyagok

##<a name="overview"></a>– Áttekintés 

A dokumentum tartalmaz, amelyek bemutatják, hogyan szabhatja testre a **Munkafolyamat-Tervező**munkafolyamatok forgatókönyvek. A tényleges munkafolyamat fájlokat megtalálhatja [az alábbi](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/PremiumEncoderWorkflowSamples).  

##<a name="toc"></a>TARTALOMJEGYZÉK

Az alábbi témaköröket:

- [Egy egyetlen átviteli sebesség MP4 be kódolási MXF](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)
    - [Új munkafolyamat indítása](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_start_new) 
    - [A multimédiás fájl bemeneti használatával](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_file_input)
    - [Adatfolyamok vizsgálata](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_streams)
    - [A videó kódoló hozzáadása. MP4-fájl létrehozása](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_file_generation)
    - [Audio-adatfolyam kódolást](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio)
    - [Hang és videó adatfolyamok multiplex egy MP4 tárolóba](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio_and_fideo)
    - [A MP4 fájl írásakor](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_writing_mp4)
    - [A kimeneti fájl a Media Services eszköz létrehozása](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_asset_from_output)
    - [Helyi meghajtóra kész munkafolyamat tesztelése](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_test)
- [MXF kódolást multibitrate MP4s – az engedélyezett dinamikus csomagolása](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)
    - [Egy vagy több további MP4 kimeneti értékeket hozzáadása](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_more_outputs)
    - [A kimenet fájlnevek konfigurálása](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_conf_output_names)
    - [Hozzáadás egy külön lejátszása](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_audio_tracks)
    - [Hozzáadás a. ISM SMIL fájl](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_ism_file)
- [MP4 - továbbfejlesztett tervrajz multibitrate be kódolási MXF](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4)
    - [Tartalmasabbá teszi a munkafolyamat – áttekintés](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_overview)
    - [Fájlelnevezési szabályok](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_file_naming)
    - [Az alakzatot a munkafolyamat legfelső szintű közzétételi összetevő tulajdonságai](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_publishing)
    - [A közzétett értékű neveket használja arra, a kimeneti fájl hozott létre.](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_output_files)
- [A miniatűrök multibitrate MP4 kimeneti hozzáadása](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)
    - [Miniatűrök hozzáadni a munkafolyamat – áttekintés](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to_multibitrate_MP4_overview)
    - [JPG kódolást hozzáadása](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4__with_jpg)
    - [Szín terület átalakítás kezelése](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_color_space)
    - [A miniatűrök írása](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_writing_thumbnails)
    - [Hibák észlelése az munkafolyamatokban](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_errors)
    - [Befejezett munkafolyamatok](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_finish)
- [Multibitrate MP4 kimeneti időalapú levágása](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim)
    - [Arra, hogy el a szűrés munkafolyamatok áttekintése](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_start)
    - [Az adatfolyamban elrejtő használatával](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_use_stream_trimmer)
    - [Befejezett munkafolyamatok](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_finish)
- [A parancsfájlok összetevő bemutatása](media-services-media-encoder-premium-workflow-tutorials.md#scripting)
    - [A munkafolyamaton belül a parancsfájlok: Helló, világ](media-services-media-encoder-premium-workflow-tutorials.md#scripting_hello_world)
- [Multibitrate MP4 kimeneti keret-alapú levágása](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)
    - [Tervrajz arra, hogy el a szűrés – áttekintés](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_start)
    - [A ClipArt-lista XML használata](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clip_list)
    - [A ClipArt lista parancsfájl alapú összetevő módosítása](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_modify_clip_list)
    - [Egy ClippingEnabled kényelmesebbé tulajdonság hozzáadása](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clippingenabled_prop)

##<a id="MXF_to_MP4"></a>Egy egyetlen átviteli sebesség MP4 be kódolási MXF
 
Az útmutató azt létre fogja hozni egy egyetlen átviteli sebesség. MP4-fájl az AAC-HE-címként kódolt hangjához egy. MXF bemeneti fájlt. 

###<a id="MXF_to_MP4_start_new"></a>Új munkafolyamat indítása 

Nyissa meg a munkafolyamat-Tervező, és jelölje be a "Fájl"-"új munkaterület"-"alakítható át tervrajz" 

Az új munkafolyamat 3 elemek jelennek meg: 

- Elsődleges forrásfájl
- ClipArt-lista XML
- Kimeneti fájl/eszköz  

![Új kódolási munkafolyamat](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-transcode-blueprint.png)

*Új kódolási munkafolyamat*

###<a id="MXF_to_MP4_with_file_input"></a>A multimédiás fájl bemeneti használatával

Fogadja el a beviteli médiafájl, hogy egy kezdődik hozzáadása a multimédiás fájl beviteli összetevőt. Összetevő a munkafolyamat hozzáadásához keresse meg azt a tárházba keresőmezőbe, és a kívánt bejegyzést húzza az alakzatot a Lekérdezéstervező ablakban. Hajthatja végre a multimédiás fájl beviteli és beolvasása a forrásfájl elsődleges összetevő a fájlnév beviteli PIN-kódot a multimédiás fájl beviteli.

![Csatlakoztatott médiafájl beviteli](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-input.png)

*Csatlakoztatott médiafájl beviteli*

Mielőtt azt nem lehet sok másra, azt először kell jelezheti a munkafolyamat-tervezőben milyen mintafájl azt szeretné, a munkafolyamat segítségével. Ehhez a Lekérdezéstervező ablakban háttér elemre, és keresse meg a tulajdonság jobb oldali ablaktáblán kattintson az elsődleges forrásfájl tulajdonság. Kattintson a mappaikonra, és jelölje ki a kívánt fájlt, és a munkafolyamat tesztelése céljából. Amint ez történik, a multimédiás fájl beviteli összetevő vizsgálja meg a fájlt, és feltölteni a fájlt, akkor ellenőrzött megfelelően a kimeneti PIN.

![Feltöltött médiafájl beviteli](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-populated-media-file-input.png)

*Feltöltött médiafájl beviteli*

Amíg ez adja meg, hogy milyen bemeneti azt szeretne dolgozni, akkor nem megállapítani, hogy még ha a kódolt kimenet kell megnyitásához. Hasonló hogyan az elsődleges forrásfájl van beállítva, és most konfigurálása a kimeneti mappa változó tulajdonság, közvetlenül a alatt.

![Konfigurált bemeneti és kimeneti tulajdonságai](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configured-io-properties.png)

*Konfigurált bemeneti és kimeneti tulajdonságai*

###<a id="MXF_to_MP4_streams"></a>Adatfolyamok vizsgálata

Gyakran azt még a kívánt tudnivalók az adatfolyam megjelenésével hasonlóan flow a munkafolyamaton keresztül. Nézze meg a munkafolyamat bármely pontján adatfolyam, csak kattintson egy kimenet vagy a beviteli PIN-kód, minden összetevő. Ebben az esetben próbálkozzon a videó nem tömörített kimeneti PIN-kódot a multimédiás fájl beviteli parancsra. Egy párbeszédpanel nyílik meg, amely lehetővé teszi, hogy vizsgálja meg a kimenő videót.

![A videó nem tömörített kimeneti PIN-kód vizsgálata](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-inspecting-uncompressed-video-output.png)

*A videó nem tömörített kimeneti PIN-kód vizsgálata*

Ebben az esetben közli velünk például, hogy azt is foglalkozik egy 1920 x 1080 beviteli a 24 keretek másodpercenként a 4:2:2 mintavételnél szinte 2 percig tart a videót.

###<a id="MXF_to_MP4_file_generation"></a>A videó kódoló hozzáadása. MP4-fájl létrehozása

Megjegyzendő, hogy most egy nem tömörített Video- és több nem tömörített hang kimeneti PIN-kódok esetében, használja a multimédiás fájl beviteli. Annak érdekében, hogy a bejövő videó kódolása, szükséges egy kódolási összetevő - ebben az esetben az generálni. Mp4-fájlok.

A video-adatfolyam H.264 kódolni, adja hozzá a videó Encoder AVC tervezési felületére. Az összetevő egy Kibontás video-adatfolyam bemeneteként elfoglalja, és egy AVC tömörített video-adatfolyam a kimeneti PIN-kódot a biztosítja.

![Nem AVC kódoló](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-avc-encoder.png)

*Nem AVC kódoló*

Megjeleníti a tulajdonságait határozza meg, hogyan pontosan a kódolást történik. Következzenek a fontosabb beállításai figyelmébe:

- Kimeneti magasság- és kimenet: ezek határozzák meg, hogy a kódolt videó felbontását. Abban az esetben most lépjen a 640 x 360
- Keret ráta: Ha értéke átadó fog csak fogadja el a forrás keret ráta, lehetőség ennek ellenére felülbírálására. Figyelje meg, hogy ilyen képkockasebességhez átalakítás van nem mozgásvonalas-visszatérített.
- A profil- és szint: ezek a AVC profil és a szint határozzák meg. További információt a különböző szinteket és profilok kényelmesen beszerzéséhez kattintson a kérdőjel ikonra a AVC videó Encoder összetevő, és a súgó lapon megjelenik a szintek talál részletesebb. A minta esetében megkönnyítésére vegyük sorra nyissa Main profil szintre 3,2 (alapértelmezett).
- Ráta mód szabályozása és átviteli sebesség (KB): az esetben azt kiválaszthatják egy állandó átviteli sebesség (CBR) 1200 KB a kimeneti számára
- Videoformátum: Ez az a VUI (videó használhatósági információ) kap bejegyzi a H.264 adatfolyam (a, amely lehet egy médiadekódoló tartalmasabbá teszi a kijelző által használt, de nem feltétlenül megfelelően dekódolását egymás információ) kapcsolatban:
- NTSC (Amerikai Egyesült Államok vagy japán, használja a 30 képkocka tipikus)
- PAL (tipikus Európa, 25 képkocka használata)
- GOP méret mód: azt fogja rögzített GOP méret beállítása mostani célok szempontjából a lezárt GOPs 2 másodpercig kulcs időközt. Ezzel biztosíthatja, hogy kompatibilis-e a dinamikus összecsomagolása Azure Media Services biztosít.

A AVC encoder hírcsatornára, csatlakoztassa a nem tömörített videó kimeneti PIN-kódot a multimédiás fájl beviteli összetevő és a nem tömörített videó beviteli PIN-kódot a AVC kódoló.

![Csatlakoztatott AVC kódoló](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-avc-encoder.png)

*Csatlakoztatott AVC fő kódoló*

###<a id="MXF_to_MP4_audio"></a>Audio-adatfolyam kódolást

Ezen a ponton azt a videó van kódolva, de az eredeti nem tömörített audio-adatfolyam még elvégzendő tömörítve. Ez az AAC-kódoló (Dolby) összetevő kódolással AAC később találhat. Adja hozzá a munkafolyamatot.

![Nem AVC kódoló](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-aac-encoder.png)

*Nem AAC kódoló*

Mostantól kompatibilisek: nem csak a egyetlen nem tömörített hang beviteli PIN-kódot az AAC-kódoló során valószínűleg több, mint a multimédiás fájl beviteli lesz két különböző nem tömörített audio-adatfolyam érhető el: egy, a bal oldali hang csatorna és jobbra. (Ha körül hang kezelése, amely a 6 csatornák.) Így még nem lehet csatlakozni közvetlenül a hangot a multimédiás fájl beviteli forrásból származó be a hang AAC-kódoló. Az AAC-összetevő vár egy úgynevezett "időosztásos" audio-adatfolyam: egyetlen adatfolyam, amelynek a bal és jobb csatornák egymással interleaved. Miután a forrásfájl media tudjuk melyik zeneszámok, a forrás milyen pozíciója a azt hozhat létre a bal és jobb megfelelően hozzárendelt előadói pozíciójának ilyen időosztásos audio-adatfolyam.

Először egy is szükség van egy időosztásos adatfolyam hozza létre a szükséges forrás hang csatornák. A hang adatfolyam Interleaver összetevő fogja kezelni a nekünk. Vegye fel a munkafolyamatot, és csatlakoztassa a hang kimeneti értékeket a multimédiás fájl bemeneti bele.

![Csatlakozik Audio-adatfolyam Interleaver](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-audio-stream-interleaver.png)

*Csatlakozik Audio-adatfolyam Interleaver*

Most, hogy elkészült a időosztásos audio-adatfolyam, azt még mindig nem helyének megadása a balra vagy jobbra a hangszóró beosztások szeretne rendelni. Annak érdekében, hogy adja meg, ezzel, azt is kihasználhatja a hangszóró pozíció Assigner.

![A hangszóró pozíció Assigner hozzáadása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-speaker-position-assigner.png)

*A hangszóró pozíció Assigner hozzáadása*

Állítsa be a hangszóró pozíció Assigner használatra sztereó beviteli adatfolyam "Custom" és "2.0-s (L, R)" nevű előre csatorna-kódoló előre definiált szűrőn keresztül. (Ez lesz a bal oldali előadói csökkentésével 1 csatorna és hozzárendelése a megfelelő előadói pozíció 2 csatorna.)

A hangszóró pozíció Assigner kimenetét csatlakozzon a bemeneti a AAC kódoló. Ezt követően, hogy egy "2.0-s (L, R)" dolgozhat a AAC kódoló csatorna készletet, így tudja a sztereó hang bemeneteként foglalkozik.

###<a id="MXF_to_MP4_audio_and_fideo"></a>Hang és videó adatfolyamok multiplex egy MP4 tárolóba

A AVC adott kódolt video-adatfolyam és az AAC kódolt audio-adatfolyam, azt is rögzíthet mindkettő be egy. MP4 tároló. A különböző adatfolyamok keverése be egyetlen egy folyamata neve "multiplex" (vagy "muxing"). Ebben az esetben azt is kihagyásos a hang- és a video-összefüggő egyetlen adatfolyam megjelenítését. MP4-csomag. Ezt a koordináták a összetevő. MP4 tároló az ISO MPEG-4-es Multiplexer neve. A tervezési felületre lehetőséget, és a AVC videó kódoló, mind a AAC kódoló csatlakozzon a bemeneti adatok alapján.

![Csatlakoztatott MPEG4 Multiplexer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-mpeg4-multiplexer.png)

*Csatlakoztatott MPEG4 Multiplexer*

###<a id="MXF_to_MP4_writing_mp4"></a>A MP4 fájl írásakor

Kimeneti fájl írásakor, a kimeneti fájl összetevőt használja. Azt csatlakozhat Ez az ISO MPEG-4-es Multiplexer kimenetét, hogy az eredményt kapja írt lemezre. Ehhez a tároló (MPEG-4-es) kimeneti PIN-kód csatlakozni a kimeneti fájl beviteli írási PIN kódot.

![Csatlakozik a kimeneti fájl](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-file-output.png)

*Csatlakozik a kimeneti fájl*

A fájlnév használt a fájl tulajdonsága határozza meg. Míg a tulajdonság szoftveresen kötött egy adott értéket is lehetnek, nagy valószínűséggel fog szeretné helyette beállítása kifejezésből.

Szeretné, hogy a munkafolyamat automatikusan meghatározza a kimeneti fájl neve tulajdonság a kifejezést, kattintson a (mellett a mappaikonra) a fájlnév mellett a része. A legördülő menüből válassza a "Kifejezés". Ez a Kifejezésszerkesztő be állapotba kerül. Először törölje a szerkesztő tartalmát.

![Üres Kifejezésszerkesztő](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-empty-expression-editor.png)

*Üres Kifejezésszerkesztő*

A Kifejezésszerkesztő lehetővé teszi, hogy minden olyan konstans érték, egy vagy több változót vegyesen megadását. Változók egy dollárjelet kezdődik. Gombra kattint a $ billentyűt, miközben a szerkesztő megjelenik a rendelkezésre álló változót választási lehetőséget a legördülő lista jelölőnégyzetet. Abban az esetben a kimeneti címtár változó és az alap beviteli Fájlnév változó használjuk:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}.MP4

![A Kifejezésszerkesztő ki adatokkal](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-expression-editor.png)

*A Kifejezésszerkesztő ki adatokkal*

>[AZURE.NOTE]Megjelenítéséhez lásd: a kimeneti fájl a kódolási feladat Azure-ban, meg kell adnia egy értéket a kifejezés szerkesztőben. 

A kifejezés szerezze meg ok erősítse meg, ha a Tulajdonságok ablak a Mi az az érték a fájl-tulajdonságok lefagyását ezen a ponton a egyszerre fog előnézetét.

![Fájl kifejezés eredménye a kimeneti dir](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-expression-resolves-output-dir.png)

*Fájl kifejezés eredménye a kimeneti dir*

###<a id="MXF_to_MP4_asset_from_output"></a>A kimeneti fájl a Media Services eszköz létrehozása

Azt írt MP4 kimeneti fájl, miközben továbbra is jelzi, hogy a fájl tartozik a kimeneti eszköz, amelynek a media services hoz létre, a munkafolyamat végrehajtása eredményeként szükség. Ennek a kimeneti fájl/eszköz csomópontot, a munkafolyamat vászonra használnak. A csomópont összes bejövő fájlokat láthatóvá válik az eredményül kapott Azure Media Services eszköz részét.

Csatlakozás a kimeneti fájl/digitáliseszköz-komponens a munkafolyamat befejezési a kimeneti fájl összetevőt.

![Befejezett munkafolyamatok](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow.png)

*Befejezett munkafolyamatok*

###<a id="MXF_to_MP4_test"></a>Helyi meghajtóra kész munkafolyamat tesztelése

A munkafolyamat helyileg teszteléséhez találati a lejátszás gombra az eszköztáron a képernyő tetején. Befejeződése a munkafolyamat végrehajtása után vizsgálja meg a kimenet hozza létre a beállított kimeneti mappában. A kész MP4 kimeneti fájl, amely a MXF beviteli forrásfájl kódolva lett megjelenik.

##<a id="MXF_to_MP4_with_dyn_packaging"></a>Dinamikus összecsomagolása engedélyezett MXF kódolást MP4 - multibitrate be

Az útmutató azt létrehoznia több átviteli sebesség MP4-fájlokat az AAC-címként kódolt egyetlen hang. MXF bemeneti fájlt.

Ha a többszörös-átviteli sebesség eszköz kimeneti van szükség együttes használata Azure Media Services, az egyes egy másik átviteli sebesség és a felbontás kell hozható létre több GOP igazított MP4 fájl által biztosított dinamikus összecsomagolása funkcióival. Ehhez a [Kódolás MXF be egy egyetlen átviteli sebesség MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4) forgatókönyv ad egy jó kiindulási pont.

![Munkafolyamat indítása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow.png)

*Munkafolyamat indítása*

###<a id="MXF_to_MP4_with_dyn_packaging_more_outputs"></a>Egy vagy több további MP4 kimeneti értékeket hozzáadása

Az eredményül kapott Azure Media Services eszköz minden MP4 fájlt egy másik átviteli sebesség és a felbontás támogatja. Egy vagy több MP4 kimeneti fájlok hozzáadása a munkafolyamathoz.

Ellenőrizze, hogy van-e a videó kódolók készült ugyanazokat a beállításokat, a már meglévő AVC videó kódoló megkettőzése, és állítsa be a másik kombinációját átviteli sebesség (hozzáadása 960 x 540 közül a 25 keretek másodpercenként a 2,5 MB) és felbontása legkényelmesebb. A meglévő kódoló duplikálni, Másolás illessze be a Lekérdezéstervező területen.

Csatlakozás a multimédiás fájl bemeneti nem tömörített videó kimeneti PIN-kódot be az új AVC összetevőt.

![Második AVC encoder csatlakoztatott](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-avc-encoder-connected.png)

*Második AVC encoder csatlakoztatott*

Most megfelelően alakíthat át az adatokat az új AVC encoder 960 x 540 2,5 MB a kimeneti. (Használja annak tulajdonságait "kimeneti szélesség", "Kimeneti magasság" és "Átviteli sebesség (KB)" ahhoz, hogy ez.)

Adott szeretnénk az eredményül kapott eszköz az Azure Media Services dinamikus összecsomagolása együtt használja, a továbbított végpontot képes létrehozása az egymással oly módon, hogy a különböző bitrates közötti eddig ügyfelek első egy egyetlen, folytonos zökkenőmentes videó és hang élmény pontosan igazított HLS/Fragmented MP4/kötőjel töredékek MP4 fájlokat. Ahhoz, hogy, amelyek akkor aktiválódnak, szükséges ahhoz, hogy, mindkét AVC kódolók a GOP ("csoport képek") tulajdonságok mindkét MP4-fájlok mérete értéke 2 másodpercig, teheti meg:

- Rögzített GOP méretét a GOP méret mód beállítást, és
- a másodperc két kulcs keret időközt.
- is beállíthat a GOP IDR vezérlőelem lezárt GOP ahhoz, hogy az összes GOP vannak állandó nélkül saját függőségeket

Ahhoz, hogy a munkafolyamat hasznos, ha meg szeretné érteni, nevezze át az első AVC encoder való "AVC videó Encoder 640 x 360 1200 KB" és a második AVC kódoló "AVC videó Encoder 960 x 540 2 500 KB".

Most adjon hozzá egy második ISO MPEG-4-es Multiplexer és egy második kimeneti fájl. Az új AVC encoder a multiplexer kapcsolódni, és ellenőrizze, hogy az eredményt irányítja a kimeneti fájl be. Csatlakoztassa is a AAC hang encoder eredménye pedig az új multiplexer a bemeneti. A kimeneti fájl viszont majd kapcsolhat össze fel szeretne venni a Media Services eszköz, amely létrejön a kimeneti fájl/eszköz csomópontot.

![Második Muxer és a kimeneti fájl kapcsolatban](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-muxer-file-output-connected.png)

*Második Muxer és a kimeneti fájl kapcsolatban*

Azure Media Services dinamikus összecsomagolása való kompatibilitás érdekében a multiplexer adattömb mód GOP darab és időtartam beállítása, és jelölje be a GOPs adattömb egy 1. (Ez legyen az alapértelmezett.)

![Muxer adattömb módok](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-muxer-chunk-modes.png)

*Muxer adattömb módok*

Megjegyzés: érdemes ismételje meg ezt a folyamatot minden más átviteli sebesség és a felbontás kombináció az eszköz eredményt ad hozzá szeretne.

###<a id="MXF_to_MP4_with_dyn_packaging_conf_output_names"></a>A kimenet fájlnevek konfigurálása

Egynél több egyetlen fájl hozzáadása a kimeneti eszköz van. Ezzel a megoldással ellenőrizze, hogy a fájlnevekben a kimeneti fájl minden egyes különböznek egymástól és esetleg válik a fájl neve törlése a esetén foglalkozó, még akkor is alkalmazása egy fájl-elnevezési konvenció szükséges.

Fájl kimeneti elnevezési szabályozható a tervezőben kifejezések keresztül. Nyissa meg a tulajdonság ablaktáblát, a kimeneti fájl összetevők egyik, és nyissa meg a fájl tulajdonság a Kifejezésszerkesztő. Az első kimeneti fájl a következő kifejezés keresztül konfigurálásáról (lásd: az [MXF szeretne egy egyetlen átviteli sebesség MP4 kimeneti](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)változik az oktatóprogram):

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}.MP4

Ez azt jelenti, hogy a fájlnév határozzák meg két változó: írja be a kimeneti könyvtár és a forrásfájl alap neve. Az előbbi van téve a legfelső szintű munkafolyamat tulajdonság, és az importált fájl által meghatározott. Ne feledje, hogy a kimeneti könyvtár teszteléshez helyi; használata Ezt a tulajdonságot a rendszer felülírja a munkafolyamat-motor által a felhőalapú media feldolgozó az Azure Media Services a munkafolyamat végrehajtása esetén.
Ad mindkét a kimeneti fájl a kimeneti következetes elnevezési, módosítsa az első fájlelnevezési kifejezést:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

a másodikat:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_960x540_2.MP4

Hajtsa végre az köztes próba futtatásával ellenőrizze, hogy mindkét MP4 kimeneti fájl megfelelően jönnek létre.

###<a id="MXF_to_MP4_with_dyn_packaging_audio_tracks"></a>Hozzáadás egy külön lejátszása

Ahogy azt láthatja, később amikor azt készítése egy .ism fájlt, válassza a MP4 kimeneti fájlok, azt is szüksége lesz MP4 csak hangot tartalmazó fájl, a hangsáv a adaptív folyamatos. Ez a fájl létrehozásához vegyen fel egy további muxer a munkafolyamatot (ISO-MPEG-4-es Multiplexer), és kapcsolja a az AAC-kódoló kimeneti PIN-kód és annak a bemeneti PIN-kód követése 1.

![Hang Muxer hozzáadása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-added.png)

*Hang Muxer hozzáadása*

Hozzon létre egy harmadik kimeneti fájl összetevő a kimenő adatfolyam a muxer a kimeneti, és állítsa be a fájlelnevezési másként kifejezést:
    
    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_128kbps_audio.MP4

![Hang Muxer a kimeneti fájl létrehozása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-creating-file-output.png)

*Hang Muxer a kimeneti fájl létrehozása*

###<a id="MXF_to_MP4_with_dyn_packaging_ism_file"></a>Hozzáadás a. ISM SMIL fájl

Dinamikus csomagolásához MP4 fájlok (mind a csak hangot tartalmazó MP4) a Media Services eszköz dolgozhat együtt, azt is szükséges nyilvánvalóan (más néven "SMIL" fájl: szinkronizált multimédia integrációs nyelvi). Ezt a fájlt jelzi az Azure Media Services milyen MP4-fájlok dinamikus csomagolása, és közülük a hang folyamatos szempontok melyiket érhetők el. Egy tipikus nyilvánvalóan fájl MP4 meg az egyetlen audio-adatfolyam halmazára néz ki:
    
    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    <smil xmlns="http://www.w3.org/2001/SMIL20/Language">
      <head>
        <meta name="formats" content="mp4" />
      </head>
      <body>
        <switch>
          <video src="H264_1900kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_1300kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_900kbps_AAC_und_ch2_96kbps.mp4" />
          <audio src="AAC_ch2_96kbps.mp4" title="AAC_und_ch2_96kbps" />
        </switch>
      </body>
    </smil>

A .ism belül állandót, minden egyes MP4 videofájlok és mellett azokat is (legalább) hangfájlt hivatkozik egy MP4 a hangot tartalmazó hivatkozást tartalmaz.

MP4-féle a csoportját a nyilvánvalóan fájl létrehozása történik át az "AMS cikkét író" összetevőt. Azt, húzza az alakzatot a felület és a kimeneti fájl három összetevő az "Írja be teljes" kimeneti PIN csatlakoztatása a AMS cikkét író bemeneti. Végezze el a arról, hogy AMS cikkét minden kimenő a kimeneti fájl/eszköz szeretne csatlakozni.

Az egyéb fájl kimeneti összetevők, a konfigurálása a .ism kimeneti fájlnév kifejezést:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_manifest.ism

A kész munkafolyamat néz ki az alábbi:

![Befejezett MXF multibitrate MP4 munkafolyamatra](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-mxf-to-multibitrate-mp4-workflow.png)

*Befejezett MXF multibitrate MP4 munkafolyamatra*

##<a id="MXF_to__multibitrate_MP4"></a>MP4 - továbbfejlesztett tervrajz multibitrate be kódolási MXF

Az [előző munkafolyamat útmutató](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging) azt láthatta, hogyan egyetlen MXF beviteli eszköz alakíthatók át kimeneti tárgyi eszköz többszörös-átviteli sebesség MP4 fájlokat, csak hangot tartalmazó MP4 fájlként és használata az Azure Media Services dinamikus összecsomagolása együtt nyilvánvalóan fájl.

Ez a forgatókönyv hogyan néhány szempontjait is fokozott és elvégzett kényelmesebb jelennek meg.

###<a id="MXF_to_multibitrate_MP4_overview"></a>Tartalmasabbá teszi a munkafolyamat – áttekintés

![A munkafolyamat Multibitrate MP4 növelése érdekében](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-enhance.png)

*A munkafolyamat Multibitrate MP4 növelése érdekében*

###<a id="MXF_to__multibitrate_MP4_file_naming"></a>Fájlelnevezési szabályok

Az előző munkafolyamat azt egy egyszerű kifejezés létrehozása a kimeneti fájl nevek alapjaként megadva. Bár van néhány Ismétlődés: összes a az egyéni kimeneti fájl összetevők megadott ilyen kifejezés.

Például a fájl kimeneti tartalom blogokhoz – az első videofájl van beállítva az alábbi kifejezést:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

A videó második kimeneti van még egy kifejezés, például:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_960x540_2.MP4

Lenne tisztább, mínusz jobban és kényelmesebb, ha sikerült néhány Ez az Ismétlődések eltávolítása és tétele több konfigurálható helyett a hiba? Azt is Szerencsére: a Tervező kifejezés funkciók együtt az azt jelenti, hogy hozzon létre egyéni tulajdonságok a munkafolyamat legfelső szintű képet ad us kényelmesebbé egy hozzáadott réteget.

Tegyük fel, azt fogja a bitrates az egyes MP4-fájlokat a fájlnév konfigurációs meghajtó. Ezek a bitrates azt fogja célja, hogy egy központi helyen (a legfelső szintű a grafikonon), ahol fogja hozzáférhetők konfigurálása és a meghajtó fájlnevek konfigurálása. Ehhez azt indítsa el az átviteli sebesség tulajdonság mindkét AVC kódolók a munkafolyamat-legfelső szintű közzétételi, hogy a AVC kódolók fájltípusba mindkét legfelső szintjén is elérhető lesz. (Még akkor is, ha két különböző máshol látható, nincs csak egy mögöttes érték.)

###<a id="MXF_to__multibitrate_MP4_publishing"></a>Az alakzatot a munkafolyamat legfelső szintű közzétételi összetevő tulajdonságai

Nyissa meg az első AVC encoder, nyissa meg a átviteli sebesség (KB) tulajdonságot, és a legördülő listából válassza a közzététel lehetőséget.

![Közzététel a átviteli sebesség tulajdonság](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-bitrate-property.png)

*Közzététel a átviteli sebesség tulajdonság*

A közzététel a munkafolyamat-diagram gyökerébe közzététel párbeszédpanel állítson be egy közzétett neve "video1bitrate" és "Videó 1 átviteli sebesség" olvasható megjelenítendő neve. Állítsa be az egyéni csoport neve "Streaming Bitrates" nevű, majd kattintson a közzététel.

![Közzététel a átviteli sebesség tulajdonság](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-bitrate-property.png)

*Átviteli sebesség tulajdonság közzététele párbeszédpanel*

Ismételje meg ugyanezt a második AVC kódoló átviteli sebesség tulajdonsága, és az azonos egyéni csoportba "Streaming Bitrates" a megjelenítendő neve "Videó 2 átviteli sebesség", "video2bitrate" nevet.

Ha azt most vizsgálja meg a munkafolyamat legfelső szintű tulajdonságait, azt az egyéni csoport a jelenik meg a két közzétett tulajdonságok jelennek meg. Mindkét vannak tükröző a megfelelő AVC encoder átviteli sebesség értékét.

![A munkafolyamat legfelső szintű közzétett átviteli sebesség kellékeinek](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-bitrate-props-on-workflow-root.png)

Amikor azt szeretné, tulajdonságokból eléréséhez, kódot vagy kifejezést, akkor jelennek meg végezheti el:

- a beágyazott kód egy összetevőjéből a legfelső közvetlenül alatt: node.getPropertyAsString('.. / video1bitrate ", null)
- kifejezés belül: ${ROOT_video1bitrate}
 
Vegyük fejezze be a "Streaming Bitrates" csoport a hangsáv átviteli sebesség rajta, valamint közzétéve. Belül az AAC-kódoló tulajdonságainak keresse meg az átviteli sebesség beállítása, és mellette a legördülő listából válassza a közzététel. Közzététel a legfelső szintű neve "audio1bitrate" a diagramot, és megjelenítendő neve "Hang 1 átviteli sebesség" az egyéni csoport "Streaming Bitrates" belül.

![Hang átviteli sebesség közzétételi párbeszédpanelje](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-audio-bitrate.png)

*Hang átviteli sebesség közzétételi párbeszédpanelje*

![A legfelső szintű eredményül kapott hang- és videofájlok kellékeinek](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-resulting-video-and-audio-props-on-root.png)

*A legfelső szintű eredményül kapott hang- és videofájlok kellékeinek*

Ne feledje, hogy bármelyik hármat módosítása értékek is újra konfigurálása módosítja az értékek vannak összekapcsolva a megfelelő összetevők (és ha közzétett).

###<a id="MXF_to__multibitrate_MP4_output_files"></a>A közzétett értékű neveket használja arra, a kimeneti fájl hozott létre.

Hardcoding helyett a létrehozott fájl nevek, az azt most módosíthatja a fájlnév kifejezés minden a kimeneti fájl összetevőket azt csak a diagram legfelső szintű közzétett átviteli sebesség tulajdonságainak támaszkodhat. Az első kimeneti fájl kezdve, a fájl tulajdonságot, és jelennek meg a kifejezés beírására:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video1bitrate}kbps.MP4

Ez a kifejezés a különböző paraméterek mindenki számára hozzáférhetők és a billentyűzeten a kifejezés ablakban a dollárjel szerezze által megadott. A rendelkezésre álló paraméterek egyike a video1bitrate tulajdonság, amely a korábban közzétett azt.

![Paraméterek kifejezésben elérése](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-accessing-parameters-within-an-expression.png)

*Paraméterek kifejezésben elérése*

Tegye ugyanezt a kimeneti fájl a második videót:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video2bitrate}kbps.MP4

és a kimeneti csak hangot tartalmazó fájl:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_audio1bitrate}bps_audio.MP4

Azt most módosítsa az átviteli-, video- vagy fájlokat, ha a megfelelő kódoló újrakonfigurálni, és az átviteli sebesség-alapú fájl neve használni fog kell fogadja összes automatikus.

##<a id="thumbnails_to__multibitrate_MP4"></a>A miniatűrök multibitrate MP4 kimeneti hozzáadása

Létrehoz [egy multibitrate egy MXF MP4 kimenetét bemeneti](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)munkafolyamat kezdve, azt fogja most kell megjeleníti a miniatűrök hozzáadása a kimenő.

###<a id="thumbnails_to__multibitrate_MP4_overview"></a>Miniatűrök hozzáadni a munkafolyamat – áttekintés

![Multibitrate MP4 munkafolyamat indítása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-start-from.png)

*Multibitrate MP4 munkafolyamat indítása*

###<a id="thumbnails_to__multibitrate_MP4__with_jpg"></a>JPG kódolást hozzáadása

A szívecskés a miniatűrök generációs lesz képes JPG-fájlok kimeneti JPG Encoder összetevőt.

![JPG kódoló](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-jpg-encoder.png)

*JPG kódoló*

Azt nem közvetlenül azonban csatlakoztassa a nem tömörített Video-adatfolyam a multimédiás fájl bemeneti a JPG kódoló be. Ehelyett várhatóan egyes keretek adni. Ez azt teheti a videoklip keretére kapu összetevő keresztül.

![Csatlakozás a JPG kódoló keret kaput](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-frame-gate-to-jpg-encoder.png)

*Csatlakozás a JPG kódoló keret kaput*

Miután minden olyan sok másodpercre, illetve keretek lehetővé teszi, hogy a videoklip keretére átadni a keret kapu. Az intervallum és az idő, amelyhez ez történik OFSZET konfigurálható az a tulajdonságokat.

![Videó keret kapu tulajdonságai](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-video-frame-gate-properties.png)

*Videó keret kapu tulajdonságai*

Hozzunk létre egy miniatűrre percenként idő (másodperc) és az intervallum 60 a mód beállításával.

###<a id="thumbnails_to__multibitrate_MP4_color_space"></a>Szín terület átalakítás kezelése

Logikusnak tűnik mindkét nem tömörített videó PIN kódokat biztosítanak a keret kapu és a multimédiás fájl bemeneti most kapcsolhat össze, miközben azt volna figyelmeztetés jelenik meg, ha azt szeretné, hajtsa végre.

![Beviteli szín terület hiba](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-input-color-space-error.png)

*Beviteli szín terület hiba*

Ennek oka az, mely színű információk jelennek meg az eredeti nyers nem tömörített videofolyamot, a MXF, érkező módja eltér a Mi a JPG kódoló meglepő. Pontosabban egy úgynevezett "színmodell", "RGB" vagy "Szürkeárnyalatos" várhatóan módot. Ez azt jelenti, hogy a videoklip keretére kapu bejövő video-adatfolyam lesz szüksége a szín terület kapcsolatban először alkalmazott konverzió.

Húzza az alakzatot a munkafolyamat a szín terület konverter – Intel, és csatlakoztassa a keret kapu.

![Csatlakozás egy szín terület konverter](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-color-space-convertor.png)

*Csatlakozás egy szín terület konverter*

A Tulajdonságok párbeszédpanelen válassza ki a BGR 24 bejegyzést a készlet listában.

###<a id="thumbnails_to__multibitrate_MP4_writing_thumbnails"></a>A miniatűrök írása

A MP4 videó eltér a JPG Encoder összetevő fog kimeneti egynél több fájl. Annak érdekében, hogy ez foglalkozik, a jelenet keresési JPG fájl minden összetevő használható: a bejövő JPG miniatűrök készíthet, és írja be őket, minden egyes egy másik számot által éppen suffixed fájlnév. (A általában a az adatfolyamban, amely a miniatűrjére a rajzolt másodperc/egységek számát megadó szám.)


![A jelenet keresési JPG-fájl író bemutatása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer.png)

*A jelenet keresési JPG-fájl író bemutatása*

A kimenet mappa elérési útja tulajdonság beállítása a kifejezéssel: ${ROOT_outputWriteDirectory} 

és a fájlnév előtag tulajdonságot: 

    ${ROOT_sourceFileBaseName}_thumb_

Az előtag meghatározza, hogy hogyan a miniatűrök fájlok neve alatt. Azok az adatfolyam a hordozható helyét jelző szám suffixed lesz.


![Jelenet keresési JPG fájl író tulajdonságai](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer-properties.png)

*Jelenet keresési JPG fájl író tulajdonságai*

Csatlakozás a jelenet keresési JPG-fájl író a kimeneti fájl/eszköz csomópontot.

###<a id="thumbnails_to__multibitrate_MP4_errors"></a>Hibák észlelése az munkafolyamatokban

A szín terület konverter a bemeneti csatlakozzon a nyers nem tömörített videó eredménye. Most már elvégezheti a munkafolyamathoz tartozó helyi útszakasz. A munkafolyamat váratlanul leállítja a végrehajtása és a hibába ütközött összetevőt egy piros körvonalú jelezheti valószínűséggel nem:

![Szín terület konverter hiba](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error.png)

*Szín terület konverter hiba*

Kattintson a jobb felső sarokban a szín terület konverter összetevő kattintva megtekintheti, hogy mi az az oka a kódolási kísérlet sarkában nem sikerült kis piros "E" ikonra.

![Szín terület konverter hiba párbeszédpanel](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error-dialog.png)

*Szín terület konverter hiba párbeszédpanel*

Azt, azzal kikapcsolja, amint látható, hogy rendelkezik-e a szabványos, a szín terület konvertáló bejövő szín szóköz rec601 az RGB-YUV kért átalakításhoz használandó. Az adatfolyam látszólag nem jelzi annak rec601. (A rekord 601 mező a digitális videó formában váltott soros analóg videó jeleket kódolási szabvány. Az aktív terület 720 fényessége minták és 360 chrominance minták soronként kiterjedő adja meg. A rendszer kódolást szín nevezik YCbCr 4:2:2.)

Kijavításához azt fogja jelezheti, amely azt is rec601 tartalom kezelése az adatfolyam metaadatait. Ezt az egy videó adatok típusát Updater-összetevőt, amely azt fogja helyezi el a nyers forrás- és a szín terület átalakítás összetevő elhelyezésével használjuk. A adatok típusát updater lehetővé teszi, hogy az egyes videó adatok kézi frissítése tulajdonságokat. Állítsa be azt, hogy egy szín terület Standard a "Rekord mező 601". Ennek hatására a videó adatok típusát Updater a "Rekord mező 601" szín szóközzel az adatfolyam címkézéséhez, ha nem volt szín hely meghatározva. (Nem felülírja a meglévő metaadatokat, kivéve, ha a felülírása jelölőnégyzetet, de.)

![Az adatok típusát Updater szín terület szabvány frissítése](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-update-color-space-standard-on-data-type.png)

*Az adatok típusát Updater szín terület szabvány frissítése*

###<a id="thumbnails_to__multibitrate_MP4_finish"></a>Befejezett munkafolyamatok

Most, hogy a a munkafolyamat befejezése, és végezze el a másik próba Futtatás fázis látni.

![A miniatűrök többszörös-mp4 kimeneti kész munkafolyamat](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-for-multi-mp4-thumbnails.png)

*A miniatűrök többszörös-mp4 kimeneti kész munkafolyamat*

##<a id="time_based_trim"></a>Multibitrate MP4 kimeneti időalapú levágása

Létrehoz [egy multibitrate egy MXF MP4 kimenetét bemeneti](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)munkafolyamat kezdve, azt fog most kell azt be a forrás videó időbélyeggel alapján csonkolása.

###<a id="time_based_trim_start"></a>Arra, hogy el a szűrés munkafolyamatok áttekintése

![Elrejtést szeretne hozzáadni a munkafolyamat indítása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow-to-add-trimming.png)

*Elrejtést szeretne hozzáadni a munkafolyamat indítása*

###<a id="time_based_trim_use_stream_trimmer"></a>Az adatfolyamban elrejtő használatával

Az adatfolyam elrejtő összetevő lehetővé teszi, hogy az elején és az alap időzítését információk (másodpercig, percekig,...) a beviteli adatfolyam befejezési vágása. A elrejtő keret alapuló szűrés nem támogatja.

![Adatfolyam-elrejtő](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-stream-trimmer.png)

*Adatfolyam-elrejtő*

Helyett a AVC kódolók és csatolása előadói pozíció assigner a multimédiás fájl bemeneti közvetlenül, azt fogja helyezze azokat, hanem a adatfolyam elrejtő. (Egy a videó jel és az a időosztásos hang jel.)

![Adatfolyam-elrejtő helyezi el a kettő között](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-put-stream-trimmer-in-between.png)

*Adatfolyam-elrejtő helyezi el a kettő között*

A elrejtő vegyük beállítása, hogy azt csak dolgozza video- és audiofájl 15 másodperc és a videó a 60 másodperc között.

Nyissa meg a Video-adatfolyam elrejtő tulajdonságait, és (15 mp) kezdési idő és a befejezési idő (60s) tulajdonságainak konfigurálása. Győződjön meg arról, mind a hang- és elrejtő vannak mindig beállítva, hogy az azonos kezdő és záró érték, akkor teszi közzé, azokat a munkafolyamat gyökerébe.

![A kezdési idő tulajdonságot adatfolyam elrejtő közzététele](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-start-time-from-stream-trimmer.png)

*A kezdési idő tulajdonságot adatfolyam elrejtő közzététele*

![Tegye közzé a Tulajdonságok párbeszédpanel időpontra](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-start-time.png)

*Tegye közzé a Tulajdonságok párbeszédpanel időpontra*

![A tulajdonság párbeszédpanelt közzététele a befejezési idő](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-end-time.png)

*A tulajdonság párbeszédpanelt közzététele a befejezési idő*


Azt a legfelső szintű a munkafolyamat most vizsgálja meg, ha mindkét tulajdonságok lesz szépen megjelenített és állítható be onnan.

![A legfelső szintű közzétett tulajdonságai](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-properties-available-on-root.png)

*A legfelső szintű közzétett tulajdonságai*

Most elrejtést tulajdonságainak megnyitása a hang elrejtő, és mind kezdő és záró időpont állítson be egy kifejezés, amely a közzétett legfelső szintű a munkafolyamat-tulajdonságok hivatkozik.

A hang elrejtést időpontra:

    ${ROOT_TrimmingStartTime}

és a befejező időpont:

    ${ROOT_TrimmingEndTime}

###<a id="time_based_trim_finish"></a>Befejezett munkafolyamatok

![Befejezett munkafolyamatok](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-time-base-trimming.png)

*Befejezett munkafolyamatok*


##<a id="scripting"></a>A parancsfájlok összetevő bemutatása

Parancsfájl összetevők tetszőleges parancsfájlokat futtathat fázisaiban, a munkafolyamat végrehajtása. Végrehajtható, négy különböző parancsprogramok adott tulajdonságokat és a saját hely, a munkafolyamat-életciklusának:

- **commandScript**
- **realizeScript**
- **processInputScript**
- **lifeCycleScript**

A dokumentáció a parancsfájl alapú összetevő Ugrás részletesebben az egyes a fenti. [A következő szakaszban](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)a parancsfájlok futtatásának **realizeScript** -összetevő segítségével egy cliplist xml menet Egyenletszerkesztővel, a munkafolyamat indításakor. A parancsfájl neve a összetevő csak egyszer történik, annak életciklus a telepítés során.


###<a id="scripting_hello_world"></a>A munkafolyamaton belül a parancsfájlok: Helló, világ

Parancsfájl alapú összetevő húzza az alakzatot a tervezési felületre, és nevezze át (például "SetClipListXML").

![Parancsfájl összetevő hozzáadása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Parancsfájl összetevő hozzáadása*

Nézze meg a parancsfájl alapú összetevő tulajdonságait, a különböző parancsfájl négyféle lesznek mutatják, egy másik parancsfájl minden konfigurálható.

![Parancsfájl összetevő tulajdonságai](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Parancsfájl összetevő tulajdonságai*

Törölje a jelet a processInputScript, és nyissa meg a realizeScript a szerkesztő. Most már azt is állíthatja be, és készen áll a kezdésre parancsfájlok.

Parancsfájlok Groovy, a Java platform, amely megőrzi a Java kompatibilitásának dinamikusan lefordított parancsnyelvének írt. Valójában azonban a legtöbb Java-kód érvényes Groovy kódot.

Vegyük írhat egyszerű helló világ groovy parancsfájl a realizeScript környezetében. Írja be a következőt a szerkesztőben:

    node.log("hello world");

Most már végrehajtása egy helyi vizsgálat. Ez a Futtatás után vizsgálja meg (keresztül a lapon kattintson a parancsfájl alapú összetevő) a naplók tulajdonság.

![Helló világ kimenet](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output.png)

*Helló világ kimenet*

A csomópont objektum hívása a napló módszer, az aktuális "csomópont" vagy az azt is parancsfájlok belül összetevő hivatkozik. Minden olyan összetevőre az azt jelenti, hogy a kimeneti naplózás adatokat, a lapon keresztül érhető el. Ebben az esetben a szövegkonstans "Helló, világ" kimeneti azt. Ha meg szeretné érteni, hogy ez igazolni tudja különösen hibakeresési eszközt az alábbiakban fontos, amelyekből, mely a parancsfájl betekintést ténylegesen tesz a.

A parancsfájlok futtatásának környezetben. azt is hozzáférést tulajdonságok a más összetevőket. TEENDŐ:


    //inspect current node: 
    def nodepath = node.getNodePath(); 
    node.log("this node path: " + nodepath);
    
    //walking up to other nodes: 
    def parentnode = node.getParentNode(); 
    def parentnodepath = parentnode.getNodePath(); 
    node.log("parent node path: " + parentnodepath);
    
    //read properties from a node: 
    def sourceFileExt = parentnode.getPropertyAsString( "sourceFileExtension", null ); 
    def sourceFileName = parentnode.getPropertyAsString("sourceFileBaseName", null); 
    node.log("source file name with extension " + sourceFileExt + " is: " + sourceFileName);

A napló ablak jeleníti meg velünk a következő:

![A kimenet elérési útjának eléréséhez](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output2.png)

*A kimenet elérési útjának eléréséhez*


##<a id="frame_based_trim"></a>Multibitrate MP4 kimeneti keret-alapú levágása

Létrehoz [egy multibitrate egy MXF MP4 kimenetét bemeneti](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)munkafolyamat kezdve, azt fogja most kell keresi az Elrejtés a forrás videó keret száma alapján.

###<a id="frame_based_trim_start"></a>Tervrajz arra, hogy el a szűrés – áttekintés

![Munkafolyamat arra, hogy el a szűrés](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-workflow-start-adding-trimming-to.png)

*Munkafolyamat arra, hogy el a szűrés*

###<a id="frame_based_trim_clip_list"></a>A ClipArt-lista XML használata

Az összes előző munkafolyamat oktatóanyagok azt a multimédiás fájl beviteli összetevő a videó beviteli forrásaként használni. Ebben az esetben a azonban fogjuk használni a ClipArt forráslista összetevő helyett. Figyelje meg, hogy ez nem kell a használni kívánt módon működik; csak akkor használja a ClipArt forráslista, ha ezt valós okának (például a az esetben, ha azt készít alatt az ClipArt lista elrejtést funkciók használata).

A multimédiás fájl bemeneti a ClipArt forráslista a vált, a ClipArt forráslista összetevő húzza az alakzatot a tervezési felületre, és csatlakozás a ClipArt-lista XML PIN-kódot a munkafolyamat-tervezőben ClipArt lista XML-csomópont. Ez kell feltöltése a ClipArt forráslista a kimeneti PIN, a videó beviteli megfelelően. Mostantól csatlakozni, a nem tömörített Video- és nem tömörített hang PIN-kódok a a a ClipArt forráslista a megfelelő AVC kódolók és hang adatfolyam Interleaver. Ekkor eltávolítja a multimédiás fájl bemeneti.

![A multimédiás fájl bemeneti cseréli a ClipArt forráslista](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-replaced-media-file-with-clip-source.png)

*A multimédiás fájl bemeneti cseréli a ClipArt forráslista*

A ClipArt forráslista összetevő annak a bemeneti adataiként megnyitja a "ClipArt lista XML". Jelölje ki a forrásfájlt, tesztelje a helyi meghajtóra, a ClipArt-lista xml esetén automatikus töltve meg.

![Automatikus töltve ClipArt lista XML-tulajdonság](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-auto-populated-clip-list-xml-property.png)

*Automatikus töltve ClipArt lista XML-tulajdonság*

Megjeleníti az XML-fájl egy kicsit közelebb,: az hogyan hasonlóan néz ki:

![Szerkesztése clip listája párbeszédpanel](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-edit-clip-list-dialog.png)

*Szerkesztése clip listája párbeszédpanel*

Ez azonban nem tükrözni a ClipArt-lista xml lehetőségeit. Egy van még azt javasoljuk, hogy adjon hozzá egy "Vágás" elemet, mind a video- és a forrás felirat alatt jelennek meg:

![A Körülvágás elem felvétele a ClipArt-listához](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-trim-element-to-clip-list.png)

*A Körülvágás elem felvétele a ClipArt-listához*

Ha a ClipArt-lista xml jelennek meg a fenti módosítása, és végezze el a helyi útszakasz, látni fogja a videó megfelelően történt díszítve a videó a 10 és 20 másodperc között.

Mi történik, amikor egy helyi Futtatás bár tegye, ellentétes Ez nagyon azonos cliplist xml nem kellene ugyanazt az eredményt az Azure Media Services futó munkafolyamat alkalmazásakor. Azure prémium Encoder indításakor cliplist xml jön létre, minden alkalommal, amikor a kódolási feladatot kapott, a bemeneti fájl ismét alapján. Ez azt jelenti, hogy mi az XML módosításokat szeretne sajnos írhatók felül.

Számláló a cliplist xml-kódolási feladat indításakor teljes mértékben éppen törlődnek, hogy azt újra miatt azt menet közvetlenül a munkafolyamat indítása után. Olyan egyéni műveleteket keresztül úgynevezett "Parancsfájl alapú összetevő" lehessen hozni. További tudnivalókért olvassa el a [a parancsfájl alapú összetevő bemutatása](media-services-media-encoder-premium-workflow-tutorials.md#scripting)című témakört.


Parancsfájl alapú összetevő húzza az alakzatot a tervezési felületre, és nevezze át a "SetClipListXML".

![Parancsfájl összetevő hozzáadása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Parancsfájl összetevő hozzáadása*

Nézze meg a parancsfájl alapú összetevő tulajdonságait, a különböző parancsfájl négyféle lesznek mutatják, egy másik parancsfájl minden konfigurálható.

![Parancsfájl összetevő tulajdonságai](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Parancsfájl összetevő tulajdonságai*


###<a id="frame_based_trim_modify_clip_list"></a>A ClipArt lista parancsfájl alapú összetevő módosítása

Mielőtt azt újból írni, hogy a munkafolyamat indításakor létrehozott cliplist xml, azt kell fér hozzá a cliplist XML-tulajdonság és annak tartalmát. Ez például azt végezheti el:

    // get cliplist xml: 
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: " + clipListXML);

![A naplózott bejövő ClipArt lista](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-incoming-clip-list-logged.png)

*A naplózott bejövő ClipArt lista*

Először szükséges határozza meg, hogy melyik helyétől eddig mely pont azt a Videoklip vágása lehetőséget. Ez kényelmes szeretne végezni a kisebb-technikai felhasználó: a munkafolyamat, tegye közzé a diagram legfelső szintű két tulajdonságait. Ehhez kattintson a jobb gombbal a tervezési felületre, és válassza a "Tulajdonság hozzáadása":

- Első tulajdonság: "ClippingTimeStart" típusú: "IDŐKÓDOK"
- A második tulajdonság: "ClippingTimeEnd" típusú: "IDŐKÓDOK"

![Adja meg a tulajdonság párbeszédpanelt kivágási kezdetének](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-start-time.png)

*Adja meg a tulajdonság párbeszédpanelt kivágási kezdetének*

![Vágás idő kellékeinek munkafolyamat legfelső szintű közzé](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-time-props.png)

*Vágás idő kellékeinek munkafolyamat legfelső szintű közzé*

A megfelelő értéket mindkét tulajdonságainak beállítása:

![A képernyőrész kezdő konfigurálása és a Tulajdonságok befejezése](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configure-clip-start-end-prop.png)

*A képernyőrész kezdő konfigurálása és a Tulajdonságok befejezése*

Most a belül a parancsfájlt, azt érheti el mindkét tulajdonságok jelennek meg:

    
    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();
    
    node.log("clipping start: " + clipstart);
    node.log("clipping end: " + clipend);

![Log ablak, amelyen a kezdete és vége kivágási](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-show-start-end-clip.png)

*Log ablak, amelyen a kezdete és vége kivágási*

Vegyük értelmezhető a időkódok karakterláncok egy kényelmesebb, egy egyszerű kifejezéssel használ, az űrlapon:
    
    //parse the start timing: 
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart); 
    startregresult.matches(); 
    def starttimecode = startregresult.group(1); 
    node.log("timecode start is: " + starttimecode); 
    def startframerate = startregresult.group(2); 
    node.log("framerate start is: " + startframerate);
    
    //parse the end timing: 
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend); 
    endregresult.matches(); 
    def endtimecode = endregresult.group(1); 
    node.log("timecode end is: " + endtimecode); 
    def endframerate = endregresult.group(2); 
    node.log("framerate end is: " + endframerate);

![Elemzett időkódok a kimeneti ablakban napló](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-output-parsed-timecode.png)

*Elemzett időkódok a kimeneti ablakban napló*

Ezen információk kéznél azt most módosíthatja a cliplist xml, a kezdő és záró időpontjának a film kívánt keret pontos kivágási megfelelően.

![Parancsfájl-kódot az kimetsz elemek felvétele](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-trim-elements.png)

*Parancsfájl-kódot az kimetsz elemek felvétele*

Erre lett normál karakterlánc-kezelő műveletek révén. Az eredményül kapott módosított ClipArt lista xml írása vissza az clipListXML tulajdonság munkafolyamat legfelső szintű a "tulajdonságbeállítása" módszerrel keresztül. A napló ablak másik próba futtatása után szeretné legyen a következő:

![Naplózás az eredményül kapott ClipArt-lista](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-result-clip-list.png)

*Naplózás az eredményül kapott ClipArt-lista*

Végezze el a megjelenítéséhez, milyen volt a video- és adatfolyamok levágott vizsgálat. Egynél több vizsgálat a szűrés pontok eltérő értékű módon fog észre fogja, hogy a program nem figyelembe kell venni azonban! Ennek oka az, hogy a tervező az Azure futtatókörnyezet eltérően nem írja felül az cliplist XML-fájl minden futtatás. Ez azt jelenti, hogy csak az első alkalommal állított az és, pontok, elindítja az XML-átalakítás, minden egyéb esetben a guard záradék (Ha (clipListXML.indexOf ("<trim>") == -1)) megakadályozza, hogy a munkafolyamat egy másik kimetsz elem felvétele, ha már van egy bemutató.

Ahhoz, hogy a munkafolyamat tesztelése helyileg kényelmes, legjobb hozzáadunk Házikó-megőrzése kód, amely megvizsgálja, ha már egy kimetsz elemet. Ha igen, hogy távolíthatja el az új értékű xml módosításával a továbblépés előtt. Egyszerű karakterlánc műveletek helyett érdemes valószínűleg biztonságosabb ehhez elemzése valós xml-objektummodell segítségével.

Mielőtt azonban lehet hozzáadni az ilyen kódot, azt kell importálása kimutatások szám hozzáadása a parancsprogram elején először:
    
    import javax.xml.parsers.*; 
    import org.xml.sax.*; 
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*; 
    import javax.xml.transform.*; 
    import javax.xml.transform.stream.*; 
    import javax.xml.transform.dom.*;

Ezt követően azt adhatja hozzá a szükséges gyorsítótárának kódot:

    //for local testing: delete any pre-existing trim elements from the clip list xml by parsing the xml into a DOM:
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML)); 
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already: 
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim"; 
    NodeList trimelems = xpath.evaluate(findAllTrimElements,dom,XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection 
    Set<Element> elementsToDelete = new HashSet<Element>(); 
    for (int i = 0; i < trimelems.getLength(); i++) { 
        Element e = (Element)trimelems.item(i); 
        elementsToDelete.add(e); 
    }

    node.log("about to delete any existing trim nodes");
     //delete the trim nodes: 
    elementsToDelete.each{ 
        e -> e.getParentNode().removeChild(e);
    }; 
    node.log("deleted any existing trim nodes");
    
    //serialize the modified clip list xml dom into a string: 
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result); 
    clipListXML = result.getWriter().toString();
    
Ez a kód feletti a pont, ahol azt a trim elemek felvétele az cliplist XML-kerül.

Ezen a ponton hogy futtatása és a munkafolyamat, mint amennyit közben a módosítások alkalmazása minden eddiginél problémákat szeretnénk idő módosítása.    

###<a id="frame_based_trim_clippingenabled_prop"></a>Egy ClippingEnabled kényelmesebbé tulajdonság hozzáadása

Előfordulhat, hogy nem mindig szeretné fordulhat elő, hogy csonkolása, mint nézzük befejezési kikapcsolása a munkafolyamat kényelmes logikai jelző, amely azt jelzi, vagy sem szeretnénk csonkolása / kivágási engedélyezése hozzáadásával.

Ugyanúgy, mint előtt, tegye közzé a legfelső szintű a munkafolyamat "ClippingEnabled" nevű új tulajdonság vonatkozó típusának "logikai".

![A közzétett tulajdonságait kivágási engedélyezése](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-enable-clip.png)

*A közzétett tulajdonságait kivágási engedélyezése*

Az az alábbi egyszerű guard záradékba, hogy elrejtést szükség esetén jelölje be, és döntse el, ha a ClipArt listáját ilyenként kell módosítani kell-e.

    //check if clipping is required: 
    def clippingrequired = node.getProperty("../ClippingEnabled"); 
    node.log("clipping required: " + clippingrequired.toString()); 
    if(clippingrequired == null || clippingrequired == false) 
    {
        node.setProperty("../clipListXml",clipListXML); 
        node.log("no clipping required"); 
        return; 
    }


###<a id="code"></a>Teljes kódot.

    import javax.xml.parsers.*; 
    import org.xml.sax.*; 
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*; 
    import javax.xml.transform.*; 
    import javax.xml.transform.stream.*; 
    import javax.xml.transform.dom.*;
    
    // get cliplist xml: 
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: \n" + clipListXML);
    // get start and end of clipping: 
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();
    
    //parse the start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart); 
    startregresult.matches(); 
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);
    
    //parse the end timing: 
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches(); 
    def endtimecode = endregresult.group(1); 
    node.log("timecode end is: " + endtimecode); 
    def endframerate = endregresult.group(2);

    node.log("framerate end is: " + endframerate);
    
    //for local testing: delete any pre-existing trim elements 
    //from the clip list xml by parsing the xml into a DOM:
    
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder(); 
    InputSource is=new InputSource(new StringReader(clipListXML)); 
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath(); 
    String findAllTrimElements = "//trim"; 
    NodeList trimelems = xpath.evaluate(findAllTrimElements, dom, XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection 
    Set<Element> elementsToDelete = new HashSet<Element>(); 
    for (int i = 0; i < trimelems.getLength(); i++) { 
        Element e = (Element)trimelems.item(i); 
        elementsToDelete.add(e); 
    }
    
    node.log("about to delete any existing trim nodes");
    //delete the trim nodes:
    elementsToDelete.each{ e -> 
        e.getParentNode().removeChild(e); 
    };
    node.log("deleted any existing trim nodes");

    //serialize the modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString()); 
    if(clippingrequired == null || clippingrequired == false) 
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return; 
    }

    //add trim elements to cliplist xml 
    if ( clipListXML.indexOf("<trim>") == -1 ) 
    {
        //trim video 
        clipListXML = clipListXML.replace("<videoSource>","<videoSource>\n <trim>\n <inPoint fps=\""+ 
            startframerate +"\">" + starttimecode + 
            "</inPoint>\n" + "<outPoint fps=\"" + endframerate +"\"> " + endtimecode + 
            " </outPoint>\n </trim> \n"); 
        //trim audio 
        clipListXML = clipListXML.replace("<audioSource>","<audioSource>\n <trim>\n <inPoint fps=\""+ 
            startframerate +"\">" + starttimecode + 
            "</inPoint>\n" + "<outPoint fps=\""+ endframerate +"\">" + 
            endtimecode + "</outPoint>\n </trim>\n");
        node.log( "clip list going out: \n" +clipListXML ); 
        node.setProperty("../clipListXml",clipListXML); 
    }


##<a name="also-see"></a>Lásd még: 

[Az Azure Media Services kódolást prémium bemutatása](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

[Azure Media Services kódolást prémium használata](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

[Az Azure Media szolgáltatás igény szerinti tartalom kódolást](media-services-encode-asset.md#media_encoder_premium_workflow)

[Media Encoder prémium munkafolyamat formátumok és a kodekekről](media-services-premium-workflow-encoder-formats.md)

[Mintafájlok munkafolyamat](http://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows)

[Azure Media Services Explorer eszköz](http://aka.ms/amse)

##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

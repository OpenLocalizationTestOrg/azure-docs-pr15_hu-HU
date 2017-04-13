<properties 
    pageTitle="Élő streaming hibaelhárítási útmutatója |} Microsoft Azure" 
    description="Ez a témakör javaslatok élő adatfolyam hibáinak elhárítása a adja vissza." 
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
    ms.date="10/12/2016"  
    ms.author="juliako"/>

#<a name="troubleshooting-guide-for-live-streaming"></a>Élő streaming hibaelhárítási útmutatója

Ez a témakör bizonyos élő adatfolyam hibáinak elhárítása javaslatok kínál.

## <a name="issues-related-to-on-premises-encoders"></a>A helyszíni kódolók kapcsolatos problémák 

Ez a szakasz javaslatok a küldhet egy átviteli sebesség adatfolyam AMS csatornák élő kódolást engedélyezett konfigurált helyszíni kódolók kapcsolatos problémák elhárítása adja vissza.

###<a name="problem-would-like-to-see-logs"></a>Probléma: Szeretné naplók 

- **A lehetséges probléma**: nem keresés encoder naplózza, amely segíthet a hibakereséshez problémák.
    
    - **Telestream Wirecast**: megtalálja a C:\Users naplók\{username} \AppData\Roaming\Wirecast\ 
    - **Elemi Live**: naplók mutató hivatkozásokat tartalmaz az adatkezelési portálon található. Kattintson a **stat**, majd a **Naplók**. A **Naplófájlok** lapon megjelenik az összes LiveEvent elem; naplók listája Jelölje ki a megfelelő aktuális munkamenete. 
    - **A villámkitöltés élő Media Encoder**: a **Naplózási könyvtár...** megtalálhatja a **Napló kódolás** lap navigálással.
    
###<a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a>Probléma: Nincs lehetőség a progresszív adatfolyam írása

- **A problémát**: A használt kódoló automatikusan félképek nem összefésülése. 

    **Hibaelhárítási lépéseket**: keresse meg a kódoló felületen egy vonja kötésre lehetőséget. Miután engedélyezte a vonja rövidebbnek, ellenőrizze ismét a progresszív kimeneti beállítások. 
 
###<a name="problem-tried-several-encoder-output-settings-and-still-unable-to-connect"></a>Probléma: A több kódoló kimeneti beállítások próbált és még mindig nem tudja elérni. 

- **A lehetséges probléma**: nem megfelelően alaphelyzetbe Azure kódolási csatornát. 

    **Hibaelhárítási lépéseket**: Győződjön meg arról, hogy a kódoló már nem terjesztése AMS való, állítsa le és állítsa alaphelyzetbe a csatornát. Ha ismét fut, próbáljon meg a kódoló az új beállításokkal. Ha ez még nem oldja meg a problémát, próbálkozzon a teljesen új csatorna létrehozása, akkor időnként csatornák előfordulhat, hogy sérült több sikertelen próbálkozás után.  

- **A problémát**: A GOP méret és fő keret beállítások nem optimálisan. 

    **Hibaelhárítási lépéseket**: ajánlott GOP méretének vagy képkockára helyezheti intervallum 2 másodpercig. Néhány kódolók számítja ki ezt a beállítást, keretek száma, míg mások használnak másodperc. Példa: 30 képkocka exportálásakor GOP méretének lenne 60 keretek, amely megegyezik 2 másodpercig.  
     
- **A problémát**: zárt portok amelyek gátolják az adatfolyam. 

    **Hibaelhárítás lépései**: Ha a folyamatos átvitelű RTMP keresztül, ellenőrizze, hogy tűzfal és/vagy a proxykiszolgáló beállításainak győződjön meg arról, hogy kimenő 1935 és 1936 nyitva legyen. Amikor a folyamatos átvitelű RTP, erősítse meg, hogy kimenő port 2010 megnyitása. 


###<a name="problem-when-configuring-the-encoder-to-stream-with-the-rtp-protocol-there-is-no-place-to-enter-a-host-name"></a>Probléma: A kódoló az adatfolyam RTP protokollal történő beállításakor nem nincs hely írja be az állomásnév. 

- **A problémát**: sok RTP kódolók nem teszik lehetővé a állomásnevek és IP-címet kell lennie szerezte be.  

    **Hibaelhárítási lépéseket**: keresse meg az IP-címet, nyissa meg egy parancssort bármely olyan számítógépen. A Windows ehhez nyissa meg a Futtatás megnyitó ikonra (WIN + R), és írja be a "cmd" megnyitásához.  

    Miután meg nyitva a parancssor parancsot, írja be a "Ping [AMS Host Name]". 

    Az állomásnév is lehet nyert kimarad az Azure Ingest URL-címmel, mint az alábbi példa a kiemelt portszámot: 

    RTP://test2-amstest009.RTP.Channel.mediaservices.Windows.NET:2010 / 

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle10.png)

###<a name="problem-unable-to-playback-the-published-stream"></a>Probléma: Nem lehet a közzétett adatfolyam lejátszás.
 
- **A lehetséges probléma**: nem nem fut a folyamatos átvitelű végpontot, vagy nincs kiosztva adatfolyam mennyiség (skála egységek) nem. 

    **Hibaelhárítási lépéseket**: Nyissa meg azt a AMSE eszközben a "Streaming végpont" fülre, és erősítse meg, a folyamatos átvitelű végpont operációs rendszert futtató egyik adatfolyam egységgel van. 
    


>[AZURE.NOTE] Ha a lépések végrehajtása után hibaelhárítási, még mindig nem tud adatfolyam-, az Azure portálon támogatási jegy nyújt.

##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

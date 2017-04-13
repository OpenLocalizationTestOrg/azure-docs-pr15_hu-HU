<properties
    pageTitle="Adja hozzá a adatbevitelhez, a megjelenítő Értékáram-elemzés feladatok |} Microsoft Azure"
    description="További információ a Értékáram-elemzés feladatok, mint a folyamatos átvitelű adatbevitel Blog tárhelyről esemény hubok vagy hivatkozás adatokból egy adatforrás jelölje be a csatlakoztatni."
    keywords="adatfolyam, bemeneti adatokat"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>


# <a name="add-a-streaming-data-input-or-reference-data-to-a-stream-analytics-job"></a>Értékáram-elemzés feladat adatfolyam adatok beviteli vagy hivatkozás adatok hozzáadása

További információ a folyamatos átvitelű Blob-tárolóhoz esemény hubok vagy hivatkozás adatait a bevitt adatok, a megjelenítő Értékáram-elemzés feladatok adatforrás jelölje be a csatlakoztatni.

Azure Értékáram-elemzés feladatok kapcsolhat össze az adatbevitel egy vagy több meglévő adatforrásból listában, amelyek kapcsolatot határozza meg. Adatokat küldi el, hogy adatforráshoz, ahogy azt a Értékáram-elemzés feladat elfogyasztott és valós idejű adatok streaming, feldolgozott. Értékáram-elemzés első osztályú integrálása az [Azure esemény hubok](https://azure.microsoft.com/services/event-hubs/) és [Azure Blob-tárolóhoz](../storage/storage-dotnet-how-to-use-blobs.md) belüli és kívüli a feladat előfizetés is tartalmaz.

Ez a cikk a [Értékáram-elemzés tanulási javaslat](/documentation/learning-paths/stream-analytics/)lépése.

## <a name="data-input-streaming-data-and-reference-data"></a>Az adatbevitel: a folyamatos átvitelű és hivatkozási adatok

Értékáram-elemzés a bemenetben két különböző típusú: adatfolyamok és hivatkozási adatok.

- **Adatfolyamok**: Értékáram-elemzés feladatok tartalmaznia kell legalább egy adatfolyam adatbevitel elfogyasztott és a transzformált által a feladatot. Azure Blob-tárolóhoz és Azure esemény hubok adatforrások adatfolyam beviteli támogatottak. Azure esemény hubok gyűjthetők össze az esemény adatfolyamok csatlakoztatott eszközök, szolgáltatások és alkalmazások használnak. Tömeges adatok adatfolyamként ingesting Azure Blob-tárolóhoz használható az beviteli forrásaként.  
- **Hivatkozási adatok**: Értékáram-elemzés második típusú kiegészítő beviteli nevű hivatkozást adatokat támogatja.  Mozgás adatokat, nem pedig a adata statikus vagy lassítása módosítása.  A szokásos szolgál look-ups és adatfolyamok az összefüggések elvégzésére vonatkozó adatok sokoldalúbb készlet létrehozása.  Azure Blob-tárolóhoz jelenleg hivatkozási adatok egyetlen támogatott beviteli forrását.  

Értékáram-elemzés kinyomtat egy bemeneti hozzáadása:

1. Az Azure-portálon kattintson a **bemeneti adatok alapján** , és válassza **egy beviteli hozzáadása** a Értékáram-elemzés feladatok.

    ![Azure klasszikus portál - hozzáadása egy beviteli.](./media/stream-analytics-add-inputs/1-stream-analytics-add-inputs.png)  

    Az Azure-portálon kattintson a **bemeneti adatok alapján** csempére, a megjelenítő Értékáram-elemzés feladat.  

    ![Azure portál - hozzáadása a bevitt adatoknak.](./media/stream-analytics-add-inputs/7-stream-analytics-add-inputs.png)  

2. Adja meg a bemeneti: **adatfolyam** vagy a **hivatkozási adatok**.

    ![A megfelelő adatokat a szövegbeviteli, folyamatosan lejátszott vagy hivatkozás hozzáadása](./media/stream-analytics-add-inputs/2-stream-analytics-add-inputs.png)  

    ![A megfelelő adatokat a szövegbeviteli, folyamatosan lejátszott vagy hivatkozás hozzáadása](./media/stream-analytics-add-inputs/8-stream-analytics-add-inputs.png)  

3. Ha egy adatfolyam beviteli létrehozni, adja meg az adatforrás az adatbevitelt.  Ezt a lépést is kihagyja, csak a tárhely jelenleg támogatott Blob hivatkozási adatok létrehozása során.

    ![Adatfolyam-adatok bevitt adatok hozzáadása](./media/stream-analytics-add-inputs/3-stream-analytics-add-inputs.png)  

    ![Adatfolyam-adatok bevitt adatok hozzáadása](./media/stream-analytics-add-inputs/9-stream-analytics-add-inputs.png)  

4. Adja meg egy rövid nevet, a bemeneti Alias mezőbe a adatbevitelt.  Ez a név használandó lekérdezésben a feladatok később a bemeneti hivatkozik.

    Töltse ki a többi szükséges kapcsolat tulajdonságainak szeretne csatlakozni az adatforráshoz. Ezeket a mezőket beviteli és az adatforrás típusa típusától függően változnak, és részletesen definiált [Itt](stream-analytics-create-a-job.md).  

    ![Esemény központi adatbevitel hozzáadása](./media/stream-analytics-add-inputs/4-stream-analytics-add-inputs.png)  

5. Adja meg a bemeneti adatok szerializálási beállításait:
    - Adja meg az **Esemény szerializálási formátum** bejövő, győződjön meg arról, hogy a lekérdezések meg a várt módon működik.  Támogatott szerializálási formátumok JSON CSV és Avro.
    - Ellenőrizze a **kódolás** adatokhoz.  UTF-8 jelenleg csak az támogatott kódolási formátumot.

    ![A bemeneti adatokat szerializálási adatbeállítások](./media/stream-analytics-add-inputs/5-stream-analytics-add-inputs.png)  

    ![A bemeneti adatokat szerializálási adatbeállítások](./media/stream-analytics-add-inputs/10-stream-analytics-add-inputs.png)  

6. Miután elkészült a bemeneti létrehozását, a megjelenítő Értékáram-elemzés ellenőrzi, hogy tud csatlakozni a bemeneti forrás.  A kapcsolat tesztelése művelet állapotát megtekintheti az értesítési-központban.

    ![A bemeneti adatfolyam-kapcsolat tesztelése](./media/stream-analytics-add-inputs/6-stream-analytics-add-inputs.png)  

    ![A bemeneti adatfolyam-kapcsolat tesztelése](./media/stream-analytics-add-inputs/11-stream-analytics-add-inputs.png)  

## <a name="get-help-with-streaming-data-inputs"></a>Segítség kérése az adatok bemenetben streaming
További segítségért próbálja meg az [Azure-adatfolyam Analytics-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések

- [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md)
- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)

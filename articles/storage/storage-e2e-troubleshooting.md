<properties
    pageTitle="Végpont Azure tárolási mértékek és a naplózás, a AzCopy és az üzenet Analyzer elhárítása |} Microsoft Azure"
    description="Végpont – hibaelhárítás Azure tároló Analytics, AzCopy és Microsoft üzenet Analyzer igazoló oktatóanyagok"
    services="storage"
    documentationCenter="dotnet"
    authors="robinsh"
    manager="carmonm"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>


# <a name="end-to-end-troubleshooting-using-azure-storage-metrics-and-logging-azcopy-and-message-analyzer"></a>Azure tárolási mértékek és a naplózás, a AzCopy és az üzenet Analyzer végpont – hibaelhárítás

[AZURE.INCLUDE [storage-selector-portal-e2e-troubleshooting](../../includes/storage-selector-portal-e2e-troubleshooting.md)]

## <a name="overview"></a>– Áttekintés

Diagnosztizálása és hibaelhárítási összeállítását, és a Microsoft Azure adathordozós ügyfélalkalmazásokban segédfájlok kulcs szakértelem. Az Azure alkalmazások elosztott jellegét, miatt diagnosztizálása és hibák és teljesítménnyel kapcsolatos problémák elhárítása lehet komplexebb, mint a hagyományos környezetben.

Ebben az oktatóanyagban az ügyfél egyes hibák teljesítményét, és a végpont e hibák elhárítása azonosítása mutatja azt be eszközeivel Microsoft és Azure tárhelyek – által biztosított annak érdekében, hogy az ügyfélalkalmazás optimalizálása.

Ebben az oktatóprogramban egy végpontok közötti hibaelhárítási forgatókönyv a rutinszerzést feltárása biztosít. Egy meg szeretné vizsgálni elvi útmutató hibaelhárítási Azure tároló alkalmazások lásd: [Monitor, diagnosztizálása, és a Microsoft Azure tároló – problémamegoldás](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="tools-for-troubleshooting-azure-storage-applications"></a>Azure tároló alkalmazások hibaelhárítási eszközök

A Microsoft Azure tároló ügyfélalkalmazásokban elhárításához a eszközök együttes használatával azt állapíthatja meg, mikor történt meg a problémát, és a probléma oka lehet. Ezek az eszközök a következők:

- **Azure tároló Analytics**. [Azure tároló Analytics](http://msdn.microsoft.com/library/azure/hh343270.aspx) mértékek és a naplózást Azure tároló biztosít.
    - **Tárolási mértékek** nyomon követi a tranzakciók mértékek és kapacitás mértékek a tárterület-fiókjához. Mértékek használata esetén megadhatja, hogyan számos különböző mértékek szerint az alkalmazás hajt végre. További információt a mértékek követi a tárhely Analytics típusú [Tároló Analytics mértékek táblázat séma](http://msdn.microsoft.com/library/azure/hh343264.aspx) témakörben talál.

    - Minden egyes kérelem **tároló naplózás** jegyez az Azure tároló kiszolgálóoldali naplózási szolgáltatások. A napló részletes adatainak minden kérés, beleértve a végrehajtott művelet, a művelet, és a késés információk állapotának nyomon követi. További információt a kérelem és válasz adatokat tároló Analytics írja be a naplókat [Tároló Analytics napló formázása](http://msdn.microsoft.com/library/azure/hh343259.aspx) című témakörből.

> [AZURE.NOTE] A Zone felesleges tárterület (ZRS) replikációs típusú tárolási számla nem rendelkezik a mértékek vagy jelenleg engedélyezve van a naplózási szolgáltatás. 

- **Azure portálon**. Beállíthatja mértékek és a naplózás a tárterület-fiók az [Azure-portálon](https://portal.azure.com). Is megtekintése, diagramokkal és grafikonokkal, amelyek a megjelenítése, hogyan az alkalmazás időbeli hajt végre, és hogy értesítést küldjön, ha az alkalmazás az adott mérőszám vártnál másképp hajt végre értesítések konfigurálása.

    Lásd: a [Monitor egy tárterület-fiókkal az Azure-portálon](storage-monitor-storage-account.md) beállításáról az Azure-portálon figyelése információt.

- **AzCopy**. BLOB, mint az Azure tároló kiszolgáló naplók tárolja, másolja a napló BLOB egy helyi elemzés használata a Microsoft üzenet Analyzer AzCopy is használhatja. Lásd: az [adatok, mellettük a AzCopy parancssori segédprogram átadása](storage-use-azcopy.md) AzCopy kapcsolatban további tudnivalókat.

- **A Microsoft üzenet Analyzer**. Üzenet Analyzer eszköz naplófájlok fogyasztása és, amely megkönnyíti a szűrő, a Keresés és a csoport naplóadatok hasznos készletben hibák és teljesítménnyel kapcsolatos problémák elemzéséhez használható vizuális formában jeleníti meg a naplóadatokat. Útmutatójában [Microsoft üzenet Analyzer működő](http://technet.microsoft.com/library/jj649776.aspx) üzenet Analyzer kapcsolatban további tudnivalókat.

## <a name="about-the-sample-scenario"></a>A példa kapcsolatban

Az ebben az oktatóanyagban azt fogja megvizsgálja azt jelzi, ha Azure tárolási mértékek egy olyan alkalmazás, amely felhívja Azure tároló alacsony százalékos sikeres kamatlábát példa. Az alacsony százalékos sikeres ráta metrikus (mint **PercentSuccess** az [Azure-portálra](https://portal.azure.com) , és a mértékek táblázatokban látható), amely a sikeresek, de, amely vissza egy HTTP állapotkód, amely nagyobb mint 299 műveletek nyomon követi. Ezeket a műveleteket a kiszolgálóoldali tárolási naplófájlok **ClientOtherErrors**tranzakció állapotban rögzítése. Ha többet szeretne tudni az alacsony százalékos sikeres metrikus lásd: [Mértékek alacsony PercentSuccess megjelenítése vagy analytics naplóbejegyzések ClientOtherErrors állapotú tranzakció műveletekre van](storage-monitoring-diagnosing-troubleshooting.md#metrics-show-low-percent-success).

Azure tárolási műveletek esetleg HTTP állapot kódok nagyobb, mint 299 normál funkcióik részeként. De bizonyos esetekben a hibák azt jelzi, hogy akkor optimalizálhatja az ügyfélalkalmazás-nagyobb teljesítmény.

Ebben az esetben azt fogja fontolja meg egy alacsony százalékos sikeres ráta kell semmit, 100 %-os alatt. Választhat másik metrikus szintre, azonban az egyéni igényeknek megfelelően. Azt javasoljuk, hogy az alkalmazás tesztelése során hozunk létre egy eredeti hibatűrést a fő teljesítménymutatók a. Például előfordulhat, hogy megállapítható, tesztjei alapján, hogy az alkalmazás kell %-os 90, vagy 85 %-os egységes százalékos sikeres díjának. Ha a mértékek adatok azt mutatja, hogy az alkalmazás a telefonszámát el van eltérő, majd is vizsgálja meg, mi növekedését okozza.

A minta forgatókönyvet amint azt, hogy van-e a százalékos sikeres ráta mérőszám alatti 100 %-os korábban kialakított azt megvizsgálja a naplókat, amely a metrikus egyeztetéséhez, és mi okozza a alsó százalékos sikeres ráta megállapítása használhatja őket a hibák keresése. Áttekintjük konkrétan a 400 tartományban található hibákat. Ezután azt fogja jobban vizsgálja meg a (nincs) 404-es hibák.

### <a name="some-causes-of-400-range-errors"></a>Néhány a 400-tartomány hibák okai

Az alábbi példák néhány 400-tartomány hibák iránti kérelmek azokat az Azure Blob-tárolóhoz és a lehetséges okok példákat talál arra jeleníti meg. E hibák, valamint a 300 tartományt, és a 500 tartomány hibák bármelyikét hozzájárulhat a alacsony százalékos sikeres díjának.

Ne feledje, hogy az alábbi listákat teljes távol. Lásd: [Állapot és hibakódok](http://msdn.microsoft.com/library/azure/dd179382.aspx) MSDN általános Azure tároló hibákról, és minden további tárterület szolgáltatások adott hibákkal kapcsolatos részleteket.

**Példák a állapot 404-es kódot (nem található)**

Fordul elő, ha olvasási egy tároló vagy blob elleni művelet sikertelen lesz, mert a blob vagy tároló nem található.

- Előfordul, ha egy tároló vagy blob törölte egy másik ügyfél, mielőtt a kérést.
- Ha használja, amely hoz létre a tároló vagy blob ellenőrzése, hogy létezik után API-hívásban fordul elő. A CreateIfNotExists API-k hívás vezetője először a tároló vagy blob; meglétének ellenőrzése Ha még nem létezik, egy 404-es hibaüzenet jelenik meg, és kattintson a második helyezése hívását írja be a tárolót vagy blob.

**Állapotkód 409 (Ütközés) példák**

- Ha egy létrehozása API segítségével hozzon létre egy új tárolót vagy blob, először létezésének ellenőrzése nélkül, és már létezik egy tároló vagy egy ilyen nevű blob fordul elő.
- Akkor fordul elő, ha a tároló törlődik, és próbál létrehozni egy új tároló azonos nevű a törlési művelet befejezése előtt.
- Ha egy bérleti tároló vagy blob adja meg, és már létezik egy bemutató bérleti fordul elő.

**Állapotkód 412 (előfeltétel) példák**

- Fordul elő, ha a feltételes fejlécet által megadott feltétel nem teljesül.
- Fordul elő, ha a megadott bérleti azonosító értéke nem egyezik meg a tároló vagy blob bérleti azonosítója.

## <a name="generate-log-files-for-analysis"></a>Az elemzéshez naplófájlok készítése

Ebben az oktatóanyagban használjuk üzenet Analyzer készült három különböző típusú naplófájlok, bár választja is dolgozhat az alábbiak valamelyikét:

- A **kiszolgáló naplója**, amely Azure tárhely a naplózás engedélyezése jön létre. A kiszolgáló naplója minden egyes egyeztetése az egyik az Azure tároló szolgáltatás – blob, várólista, táblázat vagy fájl neve művelet adatait tartalmazza. A kiszolgáló naplója azt jelzi, mely művelet hívták meg, és milyen állapotkód a kérelem és a válasz visszaadott, valamint más részleteket.
- A **.NET-ügyfél napló**, amely hoz létre, ha engedélyezi, hogy az ügyféloldali naplózás a .NET alkalmazáson belül. Az ügyfél napló hogyan az ügyfél előkészíti a kérelem és fogadása és a válasz feldolgozásával részletes információkat tartalmaz.
- A **HTTP hálózati nyomkövetési naplót**, amely által gyűjtött adatokat a HTTP-/ HTTPS kérések és válaszok adatainak, többek között a műveletek Azure tárolás ellen. Ebben az oktatóanyagban azt fogja készítése a hálózati nyomkövetési üzenet Analyzer keresztül.

### <a name="configure-server-side-logging-and-metrics"></a>Kiszolgálóoldali naplózás és -mértékek beállítása

Először is azt kell mértékek, és Azure tároló naplózásának beállítása, hogy az adatok elemzése az ügyfélalkalmazás van. Beállíthatja a naplózás és mértékek számos módon – az [Azure-portálon](https://portal.azure.com)keresztül PowerShell, használatával vagy programozás útján. Kapcsolatos további tudnivalók: [segítségével, így a tárolási mértékek és a mértékek adatok megtekintése](http://msdn.microsoft.com/library/azure/dn782843.aspx) és [tároló naplózás engedélyezése és a naplóadatokat elérése](http://msdn.microsoft.com/library/azure/dn782840.aspx) MSDN és mérőszámok naplózásának konfigurálása.

**Az Azure portálon keresztül**

Naplózás és az [Azure-portálon](https://portal.azure.com)fiókjának tárolási mértékek beállításához kövesse a [Monitor egy tárterület-fiókkal az Azure-portálon](storage-monitor-storage-account.md).

> [AZURE.NOTE] Még nem lehet beállítani az Azure-portálon perc mértékek. Azt javasoljuk azonban PIN őket az ebben az oktatóanyagban alkalmazásában, és a vizsgálat alatt teljesítménnyel kapcsolatos problémák az alkalmazással. Beállíthatja, hogy perc mértékek PowerShell alább látható módon, vagy a tárterület-ügyfél tár programozás útján használatával.
>
> Figyelje meg, hogy az Azure-portálon nem tudja megjeleníteni a percek mértékek, csak óránként mértékek.

**A PowerShell használatával**

Az első lépések az Azure PowerShell, megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md).

1. A [Hozzáadás-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) parancsmag használatával Azure felhasználói fiók felvétele a PowerShell ablakban:

    ```
    Add-AzureAccount
    ```

2. **Jelentkezzen be a Microsoft Azure** ablakban írja be az e-mail címét és a fiókjához tartozó jelszót. Azure hitelesíti menti a hitelesítő adatait, és kattintson az ablak bezárása.
3. Alapértelmezett tárolási fiókot beállítani, a következő parancsok végrehajtása a PowerShell ablakában használja az oktatóprogram-tárterület-fiókjába:

    ```
    $SubscriptionName = 'Your subscription name'
    $StorageAccountName = 'yourstorageaccount'
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
    ```

4. A Blob-szolgáltatás tároló naplózás engedélyezése:

    ```
    Set-AzureStorageServiceLoggingProperty -ServiceType Blob -LoggingOperations Read,Write,Delete -PassThru -RetentionDays 7 -Version 1.0
    ```
5. Engedélyezze a Blob-szolgáltatás, ügyelve arra, hogy **-MetricsType** meg, hogy a tárolási mértékek `Minute`:

    ```
    Set-AzureStorageServiceMetricsProperty -ServiceType Blob -MetricsType Minute -MetricsLevel ServiceAndApi -PassThru -RetentionDays 7 -Version 1.0
    ```

### <a name="configure-net-client-side-logging"></a>.NET ügyféloldali naplózás

Ügyféloldali naplózás .NET alkalmazáshoz, engedélyezze a .NET diagnosztika az alkalmazás konfigurációs fájl (web.config vagy app.config). [Ügyféloldali naplózás a .NET-ügyfél tárral tárhely](http://msdn.microsoft.com/library/azure/dn782839.aspx) és a látható [ügyféloldali naplózás a Microsoft Azure tároló SDK Java az](http://msdn.microsoft.com/library/azure/dn782844.aspx) MSDN további információt.

A ügyféloldali naplókban hogyan az ügyfél előkészíti a kérelem és fogadása és a válasz feldolgozásával részletes információkat tartalmaz.

A tároló ügyfél tár ügyféloldali naplóadatok az alkalmazás konfigurációs fájl (web.config vagy app.config) megadott helyen tárolja.

### <a name="collect-a-network-trace"></a>A hálózati nyomkövetési összegyűjtése

Üzenet Analyzer gyűjt a HTTP-/ HTTPS-hálózati nyomkövetési, az ügyfélalkalmazás futása közben is használhatja. A háttér [Fiddler](http://www.telerik.com/fiddler) üzenet Analyzer használja. Mielőtt a hálózati nyomkövetési gyűjtött, azt javasoljuk, úgy beállítani, hogy Fiddler titkosítatlan HTTPS-forgalom rögzítése:

1. Telepítse a [Fiddler](http://www.telerik.com/download/fiddler).
2. Indítsa el a Fiddler.
2. Válassza az eszközök **|} Fiddler beállítások**.
3. A beállítások párbeszédpanel győződjön meg róla, hogy **HTTPS kapcsolódik rögzítése** és **HTTPS-forgalom visszafejtése** mind van jelölve, alább látható módon.

![Fiddler beállításainak konfigurálása](./media/storage-e2e-troubleshooting/fiddler-options-1.png)

Az oktatóprogram összegyűjtése és a hálózati nyomkövetési először mentse az üzenet Analyzer, és hozzon létre egy analysis foglalkozáson, ahol a követés és a naplókat elemzése. Az üzenet Analyzer hálózati nyomkövetéshez kapcsolódó összegyűjtéséhez:

1. Az üzenet Analyzer, jelölje be a fájl **|} Rövid nyomkövetési |} HTTPS titkosítatlan**.
2. A követés azonnal elindul. Jelölje ki a **leállítása** a követés leállítása az, hogy azt adhatja nyomkövetési tárterület forgalom.
3. Válassza a **Szerkesztés** a nyomkövetési munkamenet szerkesztéséhez.
4. Az **konfigurálása** hivatkozásra a **Microsoft-Pef-WebProxy** esemény-nyomkövetés szolgáltató jobbra.
5. A **Speciális beállítások** párbeszédpanelen kattintson a **szolgáltató** fülre.
6. A **Hostname (állomásnév) szűrő** mezőben adja meg a tárhely végpontok, szóközzel elválasztva. Ha például megadhatja a végpontok következőképpen; módosítása `storagesample` tároló fiókja nevére:

    ```
    storagesample.blob.core.windows.net storagesample.queue.core.windows.net storagesample.table.core.windows.net
    ```

7. Lépjen ki a párbeszédpanelt, majd kattintson a **Indítsa újra** a követés gyűjtése a hostname (állomásnév) szűrővel helyen, a kezdéshez, hogy csak Azure tároló hálózati forgalmának engedélyezésére szerepel a követés gombra.

>[AZURE.NOTE] Miután végzett a hálózati nyomkövetési naplóban gyűjteni, javasoljuk, hogy a beállításokat, előfordulhat, hogy módosította az Fiddler visszafejtés HTTPS-forgalom visszaállítja. Fiddler beállításai párbeszédpanelen törölje a **HTTPS kapcsolódik rögzítése** és **HTTPS-forgalom visszafejtése** jelölőnégyzetének jelölését.

[A hálózati nyomkövetés szolgáltatásainak használata](http://technet.microsoft.com/library/jj674819.aspx) a TechNet webhelyen találhat további információt.

## <a name="review-metrics-data-in-the-azure-portal"></a>Tekintse át a mértékek adatok az Azure-portálon

Miután ideje az alkalmazás nem fut, áttekintheti a mértékek diagramok, amelyek az [Azure-portálon](https://portal.azure.com) megfigyelheti, hogy hogyan a szolgáltatás még lett elvégzéséhez jelennek meg. Először nyissa meg azt az Azure-portálon tárterület-fiókjába, és a **Sikeres százalékos** mérőszám diagram hozzáadása.

Az Azure-portálon **Sikeres százalékos** együtt bármely más mértékek, előfordulhat, hogy hozzáadta a megfigyeléssel diagramban most már láthatja. Az a az alkalmazási példát azt fogja vizsgálja meg a következő a naplókat, az üzenet Analyzer elemzésével, a százalék sikeres ráta némileg 100 %-os alatt.

A mértékek figyelés oldalához ad, lásd: [hogyan: mértékek elhelyezése a mértékek táblázatban](storage-monitor-storage-account.md#how-to-add-metrics-to-the-metrics-table).

> [AZURE.NOTE] Eltarthat egy kis időt, a mérőszámok adatok jelennek meg az Azure-portálon, miután engedélyezte a tárolási mértékek. Ennek az oka az előző óra óránkénti mértékek, nem jelennek meg az Azure-portálra az aktuális óra elteltével. Ezenkívül perc mértékek nem jelenleg jelennek meg az Azure-portálon. Így attól függően, ha engedélyezi a mértékek, azt órát is igénybe vehet fel két mértékek adatok megjelenítéséhez.

## <a name="use-azcopy-to-copy-server-logs-to-a-local-directory"></a>Kiszolgáló naplók másolása egy helyi könyvtár AzCopy használatával

Azure tároló kiszolgáló naplóadatok mértékek táblák vannak írása közben BLOB, ír. Log BLOB érhetők el a jól ismert `$logs` tároló a tárterület-fiókjához. Log BLOB neve hierarchikusan év, hónap, nap és órát, így egyszerűen keresse meg a vizsgálja meg a kívánt időtartományt. Ha például az a `storagesample` fiókja, a 2015 01/02 /, 8 – 9-es am, az a napló BLOB-tárolóhoz `https://storagesample.blob.core.windows.net/$logs/blob/2015/01/08/0800`. Az egyes BLOB ebben a tárolóban elnevezése egymás után, kezdődő `000000.log`.

A AzCopy parancssori eszközzel szeretne tölteni a kiszolgálóoldali naplófájlok a kívánt helyre a helyi számítógépre. Például segítségével a következő parancsot a naplófájlok történt 2015 január 2 azt a mappát, blob-műveletekhez letöltése `C:\Temp\Logs\Server`; Csere `<storageaccountname>` tároló fiókját, a nevet és `<storageaccountkey>` az access a fiókkulcs:

    AzCopy.exe /Source:http://<storageaccountname>.blob.core.windows.net/$logs /Dest:C:\Temp\Logs\Server /Pattern:"blob/2015/01/02" /SourceKey:<storageaccountkey> /S /V

AzCopy letöltési [Azure letöltések](https://azure.microsoft.com/downloads/) lapon érhető el. AzCopy használatával kapcsolatos részletekért olvassa [át a AzCopy parancssori segédprogram adataival](storage-use-azcopy.md).

Letöltési kiszolgálóoldali naplók kapcsolatos további tudnivalókért olvassa el a [naplóadatokat tároló naplózás letöltése](http://msdn.microsoft.com/library/azure/dn782840.aspx#DownloadingStorageLogginglogdata)című témakört.

## <a name="use-microsoft-message-analyzer-to-analyze-log-data"></a>Használja a Microsoft üzenet Analyzer napló adatok elemzése céljából

A Microsoft üzenet Analyzer rögzítéséhez, megjelenítése és elemzése a forgalmat, események és egyéb hibaelhárítási és diagnosztikai forgatókönyvek rendszer vagy alkalmazás üzenetek üzenetküldés protokoll eszköz. Üzenet Analyzer is lehetővé teszi, hogy betöltése, összesítése és naplófájlból adatok elemzése és fájlba menti. További információt a üzenet Analyzer útmutatójában [Microsoft üzenet Analyzer működő](http://technet.microsoft.com/library/jj649776.aspx).

Üzenet Analyzer eszközök, amelyek segítségével elemezheti server, az ügyfél és a hálózati naplók Azure tárolására is tartalmaz. Ebben a részben mutatjuk be azokat az eszközök használatáról a tárhely naplókban alacsony százalékos sikeres a probléma megoldására.

### <a name="download-and-install-message-analyzer-and-the-azure-storage-assets"></a>Töltse le és telepítse az üzenet Analyzer és az Azure tároló eszközök

1. Töltse le a Microsoft Download Center [Üzenet Analyzer](http://www.microsoft.com/download/details.aspx?id=44226) , és futtassa a telepítőjét.
2. Indítsa el az üzenetet Analyzer.
3. Az **eszközök** menüből válassza ki **a digitális eszköz kiválasztása felettesét**. A **Digitáliseszköz-kezelő** párbeszédpanelen jelölje be a **Letöltések**, majd **Azure tároló**szűrni. Ekkor megjelenik az Azure tárolási eszközök, az alábbi képen látható módon.
4. Kattintson a **Szinkronizálás az összes megjelenített elemek** telepítse az Azure tároló eszközök gombra. A rendelkezésre álló eszközök a következők:
    - **Azure tárolási szín szabályok:** Azure tárolási szín szabályok lehetővé teszi, hogy a színét, szöveget és betűstílusok használatával jelölje ki a nyomkövetési meghatározott adatokat tartalmazó üzenetek speciális szűrők megadása.
    - **Azure tároló diagramok:** Azure tárolására, amely a kiszolgáló naplóadatok graph előre definiált diagramok. Figyelje meg, hogy most Azure tárterület-diagramok használatához, előfordulhat, hogy csak betöltése a kiszolgáló naplója a Analysis rács.
    - **Azure tároló elemzők:** Az Azure tárolási elemzők elemezni az Azure tárolási ügyfél, a kiszolgáló és a HTTP naplók annak érdekében, hogy a Analysis rács megjelenítheti őket.
    - **Azure tárolási szűrők:** Azure tároló szűrők olyan előre megadott feltételek a Analysis rácson az adatok lekérdezésére használható.
    - **Azure tároló nézet elrendezések:** Azure tároló nézet elrendezések, hogy előre meghatározott oszlop elrendezések és az elemzés rács csoportosítását.
4. Az eszközök telepítése után indítsa újra az üzenet Analyzer.

![Üzenet Analyzer eszközt Manager](./media/storage-e2e-troubleshooting/mma-start-page-1.png)

> [AZURE.NOTE] Telepítse az összes az Azure tároló eszközök a célból, ebben az oktatóanyagban látható.

### <a name="import-your-log-files-into-message-analyzer"></a>A naplófájlok importálása üzenet Analyzer

Kell importálnia az összes (kiszolgálóoldali ügyféloldali és hálózati) a mentett naplófájlokat egyetlen munkamenethez Microsoft üzenet Analyzer az elemzéshez.

1. Kattintson a **fájl** menü, a Microsoft üzenet Analyzer kattintson az **Új munkamenetet**, és válassza az **Üres munkamenetet**. Az **Új munkamenet** párbeszédpanelen adja meg a elemzési munkamenet nevét. A **Munkamenet részletek** panelen kattintson a **fájlok** gombra.
1. Üzenet Analyzer által létrehozott hálózati nyomkövetési adatok betöltése, kattintson a **Fájlok hozzáadása**, keresse meg azt a helyet, a .matp fájl mentési a webes nyomkövetési munkamenetből, jelölje ki a .matp fájlt, és kattintson a **Megnyitás**gombra.
1. A kiszolgálóoldali naplóadatokat betöltéséhez, kattintson a **Fájlok hozzáadása gombra**, tallózással keresse meg azt a helyet, ahol a kiszolgálóoldali naplók letöltött, jelölje ki az elemezni kívánt időtartomány a naplófájlok és kattintson a **Megnyitás**gombra. Ezt követően a **Munkamenet részletek** panel, állítsa be a **Szöveg napló beállítása** az egyes kiszolgálóoldali naplófájlok legördülő **AzureStorageLog** annak érdekében, hogy Microsoft üzenet Analyzer is megfelelően elemzi a naplófájl.
1. Az ügyféloldali naplóadatok betöltéséhez kattintson a **Fájlok hozzáadása**, keresse meg azt a helyet, az ügyféloldali naplókban mentési helyének, jelölje ki az elemezni kívánt naplófájljának és kattintson a **Megnyitás**gombra. Ezt követően a **Munkamenet részletek** panel, állítsa be a **Szöveg napló beállítása** az egyes ügyféloldali naplófájlok legördülő **AzureStorageClientDotNetV4** annak érdekében, hogy Microsoft üzenet Analyzer is megfelelően elemzi a naplófájl.
1. Betöltése és elemezni a naplóadatokat **Új munkamenet** párbeszédpanelen kattintson a **Start** gombra. A naplóadatokat a üzenet Analyzer Analysis rács jeleníti meg.

Az alábbi képen egy példa munkamenet server, az ügyfél és a hálózati nyomkövetési naplófájlok konfigurált.

![Üzenet Analyzer munkamenet konfigurálása](./media/storage-e2e-troubleshooting/configure-mma-session-1.png)

Figyelje meg, hogy üzenet Analyzer betölti-e a memória naplófájlok. Ha nagyszámú naplóadatait, érdemes a legjobb teljesítmény elérése érdekében beolvasása üzenet Analyzer szűrésének.

Első lépésként határozza meg, amelyek érdeklik megtekintésével időkereten, hogy megőrizze a időkereten lehető legkisebb. Sok esetben azt szeretné, tekintse át az időszak perc vagy a legtöbb óra. A naplók meg az igényeinek megfelelő legkisebb készlete importálni.

Ha továbbra is nagy mennyiségű naplóadatait, majd érdemes adja meg a naplóadatokat előtt töltse be a munkamenet szűrőt. **Munkamenet szűrő** párbeszédpanelen válassza a **tár** gombra kattintva válasszon egy előre definiált szűrőt; például válasszon **globális idő szűrő i.** az Azure tárolási szűrők szűrő a időtartamot. Módosítsa a kezdési és befejezési időbélyeg a megtekinteni kívánt intervallum megadása szűrőfeltételeket. Akkor is szűrhet egy adott állapotkód; Ha például megadhatja csak állapotkód esetén 404 naplóbejegyzések betöltéséhez.

További információt a Microsoft üzenet Analyzer naplóadatok importáláshoz [Üzenet adatok beolvasása](http://technet.microsoft.com/library/dn772437.aspx) a TechNet webhelyen találhat.

### <a name="use-the-client-request-id-to-correlate-log-file-data"></a>Az ügyfél-azonosító használja összehangolására naplófájl adatainak

Az Azure tároló ügyfél tár automatikusan létrehoz egy egyedi ügyfél-kérés azonosító összes kérés. Ezt az értéket írja be az ügyfél napló, a kiszolgáló naplója és a hálózati nyomkövetés így adatok összehangolására át az összes három naplók üzenet Analyzer belül használhatja. Lásd: a [kérelem ügyfél-azonosító](storage-monitoring-diagnosing-troubleshooting.md#client-request-id) információkat szeretne kapni az ügyfél kérése azonosítóval.

Az alábbi szakaszok ismertetik, hogyan lehet előre beállított és egyéni elrendezés nézeteket használja összehangolására és a csoport adatok alapján az ügyfél kérelem azonosítóját.

### <a name="select-a-view-layout-to-display-in-the-analysis-grid"></a>Az elemzés rács megjelenítése nézet elrendezés kiválasztása

A tárterület-eszközök, az üzenet Analyzer olyan előre beállított nézetek, a hasznos, csoportok és a különböző felhasználási területei oszlopok az adatok megjelenítéséhez használható Azure tároló nézet elrendezések tartalmazzák. Is elrendezés egyéni nézet létrehozása és mentése újrafelhasználás céljából.

Az alábbi képen az **Elrendezési nézet** menü érhető el az Menüszalaglap **Nézet elrendezés** kiválasztásával. Azure tárhelyként használható nézet elrendezés csoportban a menüben a **Azure tárolási** csomópont vannak csoportosítva. Kereshet `Azure Storage` Azure tárolón szűrése a keresőmezőbe kizárólag elrendezéseket megtekintése. Emellett bejelölheti a csillag melletti teszik a kedvenc, és jelenítse meg a menü tetején elrendezés nézetben.

![Az Elrendezés menü megjelenítése](./media/storage-e2e-troubleshooting/view-layout-menu.png)

Először jelölje ki a **csoportosításra ClientRequestID és a modul**. A elrendezés csoportok megtekintése összes három naplók adatainak kérelem ügyfél-azonosító szerint, majd a forrás naplófájl (vagy az üzenet Analyzer **modul** ) először jelentkezzen be. Ebben a nézetben Lehatolás egy adott ügyfél azonosítója, és adatokat az összes három naplófájlok ügyfél kérelem azonosítóval.

A kép alatt látható a elrendezés nézetben érvényes a minta naplóadatokat a megjelenített oszlopok egy részét. Láthatja, hogy egy adott ügyfél kérelem azonosító Analysis rácsának származó adatokat jeleníti meg az ügyfél napló, a kiszolgáló naplója és a hálózati nyomkövetési naplót.

![Azure tároló nézet elrendezés](./media/storage-e2e-troubleshooting/view-layout-client-request-id-module.png)

>[AZURE.NOTE] Különböző naplófájlok van a különböző oszlopokba, így több naplófájlból adatok elemzése rácsának jelenik meg, amikor oszloppal, nem tartalmaz az adott sor adatokat. Például a fenti képen az ügyfél napló sorok ne jelenjen meg semmilyen adatot a **időbélyeg**, a **TimeElapsed**, a **forrás**és a **cél** oszlopban, mert ezekben az oszlopokban az ügyfél napló nem léteznek, de a hálózati nyomkövetési megtalálható. Hasonlóképpen az **időbélyegző** oszlop megjeleníti a kiszolgáló naplója időbélyeg adatainak, de adatok nélkül jelenik meg a **TimeElapsed**, a **forrás**és a **cél** oszlopokat, amelyeket nem része a kiszolgáló naplója.

Mellett az Azure tárolási nézet elrendezések használata esetén is megadása és a saját nézet elrendezések menteni. Jelölje ki a többi kívánt mezők csoportosításához adatokat, és mentse a csoportosítást az egyéni elrendezés részeként.

### <a name="apply-color-rules-to-the-analysis-grid"></a>Az elemzés rács szín szabályok alkalmazása

A tároló eszközök is szín szabályok, amelyek a visual azt jelenti, hogy azonosítsa a különböző típusú hibákat a Analysis rács felajánl. Előre definiált szín szabályok vonatkoznak a HTTP-hibák, hogy csak a kiszolgálón naplókban és a hálózati nyomkövetés a jelenjenek meg.

Szín szabályokat alkalmazni, válassza ki az Menüszalaglap **Szín szabályokat** . Az Azure tárolási szín szabályok menüjében talál. Az oktatóprogram jelölje ki az **Ügyfél hibák (StatusCode 400 és 499 között)**, az alábbi képen látható módon.

![Azure tároló nézet elrendezés](./media/storage-e2e-troubleshooting/color-rules-menu.png)

Az Azure tároló szín szabályok használata mellett is meghatározása és mentése szín szabályokat.

### <a name="group-and-filter-log-data-to-find-400-range-errors"></a>Naplóadatok csoport és a szűrő 400-tartomány hibák keresése

Ezután azt fogja csoport adatok szűrésére és a napló 400 tartomány összes hibát kereséséhez.

1. Keresse meg a **StatusCode** oszlopban a Analysis rács, kattintson a jobb gombbal az oszlopfejlécre, és válassza ki a **csoportot**.
2. A csoport következő, a **ClientRequestId** oszlopra. Láthatja, hogy az adatok elemzése rácsának most rendeztünk állapotkód és ügyfél kérelem azonosítóját.
1. Ha nem látható a nézetben szűrő eszköz ablak megjelenítése Válassza az Menüszalaglap **Eszköz Windows**, majd a **Nézetben szűrő**.
2. Megjelenítendő csak 400-tartomány hibák naplója adatok szűrése, az alábbi szűrési feltételek hozzáadása a **Nézetben szűrő** ablakban, és kattintson az **Alkalmaz**gombra:

        (AzureStorageLog.StatusCode >= 400 && AzureStorageLog.StatusCode <=499) || (HTTP.StatusCode >= 400 && HTTP.StatusCode <= 499)

Az alábbi képen a csoportosítás és a szűrés eredményét. A **ClientRequestID** mező alatt a csoportosítást tartozó állapotkód 409 kibontása, például egy adott állapotkód eredményező műveletet jeleníti meg.

![Azure tároló nézet elrendezés](./media/storage-e2e-troubleshooting/400-range-errors1.png)

Ez a szűrő alkalmazása, után láthatja, hogy az ügyfél napló sorának tartoznak, mint az ügyfél napló nem része a **StatusCode** oszlop. Rajta fog tanulmányozzák a kiszolgáló és a hálózati nyomkövetési naplók 404-es hibák megkereséséhez, és azt fogja térjen vissza az ügyfél napló megvizsgálja az ügyféloldali műveletek vezető őket.

>[AZURE.NOTE] A **StatusCode** oszlopra szűrheti és három naplók, beleértve az ügyfél napló, ha a kifejezés hozzáadása a szűrő, amely tartalmazza a naplóbejegyzések, ahol az állapotkód értéke null adatait is megjelenítheti. A filter kifejezésére Egyenletszerkesztővel, használhatja:
>
> <code>&#42;StatusCode >= 400 or !&#42;StatusCode</code>
>
> Ez a szűrő ad vissza minden sor az ügyféltől naplókban és csak azokat a sorokat, a kiszolgáló naplója, és a HTTP napló ahol állapotkód nagyobb, mint 400. A nézet elrendezés csoportosítva kérelem ügyfél-azonosító és a modul telepítését, ha, illetve kereshet Görgessen végig a naplóbejegyzések keresése szegélyei hol jelennek meg az összes három naplók.   

### <a name="filter-log-data-to-find-404-errors"></a>Szűrő naplóadatok 404-es hibák keresése

A tároló eszközök a naplóadatokat a hibák és a trendek keres kereséséhez szűkítése használható előre definiált szűrők tartalmazzák. Ezután azt fogja két előre definiált szűrők alkalmazásával:, amely a szűrők a kiszolgáló és a hálózati nyomkövetési naplók 404-es hibákat, és egy szűrőt az adatokat a megadott tartomány.

1. Ha nem látható a nézetben szűrő eszköz ablak megjelenítése Válassza az Menüszalaglap **Eszköz Windows**, majd a **Nézetben szűrő**.
2. Nézetszűrő ablakában jelölje ki a **tárban**, és keressen rá a `Azure Storage` keresse meg az Azure tárolási szűrők. Jelölje ki a szűrő **összes naplók 404-es (nem található)**üzenetek.
3. A **tár** menü újbóli megjelenítéséhez, és keresse meg és jelölje ki a **Globális idő szűrő értékét**.
4. A tartományához meg szeretné tekinteni a szűrő látható időbélyegeket szerkesztése. Ez segít az elemezni kívánt adatokat köre szűkítése.
5. A szűrő megjelennek az alábbihoz hasonló. Kattintson az **Alkalmaz** a szűrő alkalmazásához az elemzés rácsra.

        ((AzureStorageLog.StatusCode == 404 || HTTP.StatusCode == 404)) And
        (#Timestamp >= 2014-10-20T16:36:38 and #Timestamp <= 2014-10-20T16:36:39)

![Azure tároló nézet elrendezés](./media/storage-e2e-troubleshooting/404-filtered-errors1.png)

### <a name="analyze-your-log-data"></a>Log adatok elemzése

Most, hogy vannak csoportosítva, és az adatok szűrt, ellenőrizheti egyéni kérések által generált hibák 404-es részleteit. Az aktuális nézet elrendezésben ügyfél kérelem azonosító szerint, majd a napló forrás szerint csoportosítja az adatokat. Azt szűr kérelmek a hol a StatusCode mező 404, mert azt láthatja, csak a kiszolgáló és a hálózati nyomkövetési adatok, az ügyfél napló adatokat nem.

Az alábbi kép mutatja egy adott kérelmet, ahol egy első Blob művelet származó-e egy 404, mert nem létezik a blob. Figyelje meg, hogy egyes oszlopok el lett távolítva a szokásos nézet a fontos adatok megjelenítése érdekében.

![Szűrt Server és a hálózati nyomkövetési naplók](./media/storage-e2e-troubleshooting/server-filtered-404-error.png)

Ezután azt fogja összehangolására a a kérelem ügyfél-azonosító az ügyfél napló adatokkal az ügyfél lett véve, ha a hiba történt műveletek megtekintéséhez. Ebben a munkamenetben megnyílik egy második lapon ügyfél naplóadatok megtekintéséhez új elemzési rács nézet jelenítheti meg:

1. Első lépésként másolja a vágólapra a **ClientRequestId** mező értékét. Ehhez vagy sor kijelölése, megkeresése a **ClientRequestId** mezőt, kattintson a jobb gombbal az adatok értéke, és válassza a **Másolás "ClientRequestId"**.
1. Az Menüszalaglap jelölje be az **Új megjelenítő**, majd jelölje be az **Analysis rács** nyisson meg egy új lapot. Az új lap az összes adat a naplófájlokat, csoportosítási, szűrési és a szín szabályok nélkül jeleníti meg.
2. Az Menüszalaglap válassza az **Elrendezési nézet**, majd az **Azure tárolás** területen jelölje be a **.NET-ügyfél összes oszlopot** . Ez az elrendezés nézet jeleníti meg az ügyfél adatait, napló, valamint a kiszolgáló és a hálózati nyomkövetési naplók. Alapértelmezés szerint vannak rendezve az **LastMessage elemet** oszlopra.
3. Ezután az ügyfél napló kereshet az ügyfél kérelem azonosítóját. Jelölje be a **Üzenetek keresése**az Menüszalaglap, majd adja meg a **Keresés** mezőben az ügyfél-kérés azonosító egyéni szűrőt. A szűrő, adja meg a saját kérelem ügyfél-azonosító használja az alábbi szintaxist:

        *ClientRequestId == "398bac41-7725-484b-8a69-2a9e48fc669a"

Üzenet Analyzer keresi meg, és kiválasztja az első naplóbejegyzés, ahol a keresési feltételek megfelel-e az ügyfél kérelem azonosítóját. Az ügyfél naplóban vannak több tételek minden ügyfél-kérés azonosító, érdemes csoportba foglalni őket, így azok könnyebben megtekintheti őket az összes közös **ClientRequestId** alapján. Az alábbi képen minden üzenetet be az ügyfél számára a megadott ügyfél kérelem azonosítóját.

![Ügyfél napló mutató 404-es hibák](./media/storage-e2e-troubleshooting/client-log-analysis-grid1.png)

A nézet elrendezéseket a következő két lapokon látható adatok használata esetén a kérelem adatainak határozza meg, a hiba oka mi elemezheti. Is megnézheti tekintheti meg, ha egy előző eseményre előfordulhat, hogy a 404-es hiba vezettek alábbihoz megelőzi összehívások. Például az ügyfél naplóbejegyzések, ez határozza meg, hogy a blob törölt, vagy ha a hiba miatt az ügyfélalkalmazás olyan CreateIfNotExists API hívása tároló vagy blob ügyfél-kérés azonosító megelőző áttekintheti. Az ügyfél naplóban talál a blob-címet a **Leírás** mezőben; a kiszolgáló és a hálózati nyomkövetési naplók ezt az információt az **Összefoglalás** mező jelenik meg.

Ha már elsajátította az blob, amely a 404-es hiba származó címét, kiderítheti további. Ha a további műveletek az azonos blob társított üzenetek naplóbejegyzések keres, ellenőrizheti, hogy az ügyfél korábban már törölte a szervezet.

## <a name="analyze-other-types-of-storage-errors"></a>Más típusú tároló hibák elemzése

Most, hogy ismeri a napló adatok elemzése az üzenet Analyzer használatával, elemezheti hibák megtekintése más típusú elrendezések, a szín szabályok és a Keresés és szűrés. Az alábbi táblázat felsorolja a előfordulhatnak problémák elhárításához és a szűrési feltételek használatával keresse meg őket. Szűrők és a szűrést nyelvi üzenet Analyzer megépítése kapcsolatos további tudnivalókért olvassa el a [Üzenet adatok szűrése](http://technet.microsoft.com/library/jj819365.aspx)című témakört.

|    Megvizsgálandó...                                                                                               |    Használjon szűrőkifejezés …                                                                                                                                                                                                                                        |    Kifejezés vonatkozik a napló (ügyfél, a kiszolgáló, a hálózaton, az összes)    |
|------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------|
|    Az üzenet kézbesítését várólista a váratlan késések                                                              |    AzureStorageClientDotNetV4.Description tartalmazza az "Újrapróbálkozás művelet sikertelen."                                                                                                                                                                                |    Ügyfél                                                        |
|    HTTP PercentThrottlingError növelése                                                                       |    HTTP. Response.StatusCode == 500 és #124; és #124; HTTP. Response.StatusCode == 503                                                                                                                                                                                          |    Hálózati                                                       |
|    Növelje a PercentTimeoutError                                                                               |    HTTP. Response.StatusCode == 500                                                                                                                                                                                                                             |    Hálózati                                                       |
|    Növelje a PercentTimeoutError (mind)                                                                         |    * StatusCode == 500                                                                                                                                                                                                                                          |    Az összes                                                           |
|    Növelje a PercentNetworkError                                                                               |    AzureStorageClientDotNetV4.EventLogEntry.Level < 2                                                                                                                                                                                                          |    Ügyfél                                                        |
|    HTTP 403 (tiltott) üzenetek                                                                                 |    HTTP. Response.StatusCode == 403                                                                                                                                                                                                                             |    Hálózati                                                       |
|    HTTP (nem található) 404 üzenetek                                                                                 |    HTTP. Response.StatusCode == 404                                                                                                                                                                                                                             |    Hálózati                                                       |
|    404 (mind)                                                                                                     |    * StatusCode == 404                                                                                                                                                                                                                                          |    Az összes                                                           |
|    A megosztott Access aláírás (Társítások) engedélyezési probléma                                                             |    AzureStorageLog.RequestStatus == "SASAuthorizationError"                                                                                                                                                                                                     |    Hálózati                                                       |
|    HTTP (Ütközés) 409 üzenetek                                                                                  |    HTTP. Response.StatusCode == 409                                                                                                                                                                                                                             |    Hálózati                                                       |
|    409 (mind)                                                                                                     |    * StatusCode == 409                                                                                                                                                                                                                                          |    Az összes                                                           |
|    Alacsony PercentSuccess vagy analytics naplóbejegyzések van ClientOtherErrors állapotú tranzakció műveletei    |    AzureStorageLog.RequestStatus == "ClientOtherError"                                                                                                                                                                                                         |    Kiszolgáló                                                        |
|    Nagle figyelmeztetés                                                                                               |    ((AzureStorageLog.EndToEndLatencyMS-AzureStorageLog.ServerLatencyMS) > (AzureStorageLog.ServerLatencyMS * 1,5)) és (AzureStorageLog.RequestPacketSize < 1460) és (AzureStorageLog.EndToEndLatencyMS - AzureStorageLog.ServerLatencyMS > = 200)        |    Kiszolgáló                                                        |
|    A kiszolgáló és a hálózat naplók időtartományt                                                                    |    #Időbélyeg > = 2014-10-20T16:36:38 és #Timestamp < 2014-es = – 10-20T16:36:39                                                                                                                                                                                     |    Hálózati kiszolgálón                                               |
|    A kiszolgáló naplók időtartományt                                                                                |    AzureStorageLog.Timestamp > = 2014-10-20T16:36:38 és AzureStorageLog.Timestamp < = 2014-10-20T16:36:39                                                                                                                                                     |    Kiszolgáló                                                        |


## <a name="next-steps"></a>Következő lépések

Azure-tárolóban lévő hibaelhárítási végpont esetek olvashat az alábbi forrásokban talál:

- [Lync-diagnosztizálása és kapcsolatos hibák elhárítása a Microsoft Azure-tárolóhoz](storage-monitoring-diagnosing-troubleshooting.md)
- [Tárterület-elemzés](http://msdn.microsoft.com/library/azure/hh343270.aspx)
- [A tároló fiókot az Azure-portálon figyelése](storage-monitor-storage-account.md)
- [A AzCopy parancssori segédprogram az adatok átvitele](storage-use-azcopy.md)
- [A Microsoft üzenet Analyzer működő útmutató](http://technet.microsoft.com/library/jj649776.aspx)

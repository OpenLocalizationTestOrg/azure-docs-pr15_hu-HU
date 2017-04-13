<properties 
    pageTitle="Az Azure tárolási irányító használható fejlesztés és vizsgálata |} Microsoft Azure" 
    description="Az Azure tároló irányító fejlesztés és Azure tárolás ellen tesztelés egy ingyenes helyi fejlesztői környezet biztosít. Tudjon meg többet a tárhely irányító, többek között a kérések hitelesítésének módját, kattintson az alkalmazás a irányító keresztüli és a parancssori eszközével." 
    services="storage" 
    documentationCenter="" 
    authors="tamram" 
    manager="carmonm" 
    editor="tysonn"/>
<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="tamram"/>

# <a name="use-the-azure-storage-emulator-for-development-and-testing"></a>Használja az Azure tároló irányító fejlesztés és tesztelése

## <a name="overview"></a>– Áttekintés

A Microsoft Azure tárolási irányító, amelynek azonos fejlesztési célokra helyezése Azure Blob, a várakozási sora és a táblázat helyi környezetet nyújt. A tároló irányító használ, tesztelheti az alkalmazás szemben a helyi meghajtóra, a tároló szolgáltatást létrehozása az Azure előfizetéssel, vagy bármely költség felmerülése nélkül. Amikor elégedett hogyan az alkalmazás a irányító dolgozik, válthat a felhőben Azure tároló fiókot használ.

> [AZURE.NOTE] A tároló irányító érhető el a [Microsoft Azure SDK](https://azure.microsoft.com/downloads/)részeként. A tároló irányító a [különálló installer](https://go.microsoft.com/fwlink/?linkid=717179&clcid=0x409)használatával is telepítheti. A tároló irányító konfigurálásához rendszergazdai jogosultságokkal kell rendelkeznie a számítógépen.
> 
> A tároló irányító jelenleg csak a Windows fut.
>  
> Figyelje meg, hogy a tárhely irányító egy verziójában létrehozott adatok előfordulhat, hogy lesznek elérhetők, ha egy másik verzióját használja. Ha az adatok megmaradnak a hosszú távú kell tárolja ezeket az adatokat az Azure tárterület-fiókokban található, hanem a tárhely irányító ajánlott.

## <a name="how-the-storage-emulator-works"></a>A tároló irányító működése
 
A tároló irányító használja egy helyi Microsoft SQL Server-példányt, és a helyi fájlrendszerben emulálására a Azure tároló szolgáltatást. Alapértelmezés szerint a tárhely irányító adatbázis használja a Microsoft SQL Server 2012 Express LocalDB.  Megadhatja, hogy a tárhely irányító helyi példányt az SQL Server helyett az LocalDB példány eléréséhez konfigurálása. Lásd: a [kezdő és a tárhely irányító initialize](#start-and-initialize-the-storage-emulator) alatti további információt.

Az SQL Server Management Studio Express LocalDB telepítésének kezelése telepítheti. A tároló irányító SQL Server- vagy Windows-hitelesítéssel LocalDB csatlakozik. 

A használható funkciók körét eltérések létezik a tárhely irányító és Azure tároló szolgáltatások között. Ezeket a különbségeket kapcsolatos további tudnivalókért lásd: [a tárterület irányító és Azure tároló közötti különbségek](#differences-between-the-storage-emulator-and-azure-storage).

## <a name="authenticating-requests-against-the-storage-emulator"></a>Szemben a tárhely irányító hitelesítési kérések

Csak a Azure tárhely a felhőben, minden-összehívásba, amelyet a tárhely irányító elleni teszi hitelesíteni kell, kivéve ha névtelen kérést. A megosztott kulcs hitelesítéssel tároló irányító ellen vagy egy megosztott access-aláírással (Társítások) kérések hitelesítheti.

### <a name="authentication-with-shared-key-credentials"></a>Hitelesítés, kulcs megosztott hitelesítő adatokkal

[AZURE.INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

A kapcsolati karakterláncot, lásd: [Konfigurálása Azure tároló kapcsolati karakterláncot](storage-configure-connection-string.md). 

### <a name="authentication-with-a-shared-access-signature"></a>Megosztott access-aláírással rendelkező hitelesítés 

Néhány Azure tároló ügyfél tárai, például a Xamarin könyvtár csak a megosztott aláírás (Társítások) jogkivonat a hitelesítés támogatására. Szüksége lesz a Társítások token eszközzel vagy a megosztott kulcs hitelesítési támogató alkalmazást létrehozni. Ugyanígy a biztonsági jogkivonat készítése Azure PowerShell található:

1. Ha még nem tette meg, telepítse az Azure PowerShell. A legújabb Azure PowerShell-parancsmagok használata ajánlott. Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md#Install) telepítési utasításokat.

2. Nyissa meg az Azure PowerShell, és futtassa az alábbi parancsokat. Ne feledje, hogy le szeretné cserélni a *rendszer* és *ACCOUNT_KEY ==* a saját hitelesítő adataival. *CONTAINER_NAME* cserélje le a nevét.

        $context = New-AzureStorageContext -StorageAccountName "ACCOUNT_NAME" -StorageAccountKey "ACCOUNT_KEY=="
        
        New-AzureStorageContainer CONTAINER_NAME -Permission Off -Context $context
        
        $now = Get-Date 
        
        New-AzureStorageContainerSASToken -Name CONTAINER_NAME -Permission rwdl -ExpiryTime $now.AddDays(1.0) -Context $context -FullUri

Az eredményül kapott átengedése aláírás URI új tároló kell lennie az alábbihoz hasonló:

    https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2015-07-08T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3Dsss

Az ebben a példában készült átengedése aláírás egynapos nem érvényes. Az aláírás hozzárendeli a tárolóban BLOB teljes hozzáférés (azaz olvasható, írási, törlés, lista).

Megosztott access aláírások kapcsolatos további tudnivalókért olvassa el a [Használatával megosztott Access aláírások (Társítások)](storage-dotnet-shared-access-signature-part-1.md)című témakört.


## <a name="start-and-initialize-the-storage-emulator"></a>Indítsa el, és a tárhely irányító inicializálni

Az Azure tároló irányító elindításához kattintson a Start gombra, vagy nyomja le a Windows billentyűt. Írja be az **Azure tárolási irányító**, és jelölje ki a irányító-alkalmazások listájából. 

A irányító fut, megjelenik egy ikon, a Windows tálca értesítési területén.

A tároló irányító indításakor megjelenik egy parancssori ablak. Parancssori ablak segítségével indítása és leállítása a tárhely irányító, valamint adatok törlése, első aktuális állapotát, és a irányító inicializálni. További tudnivalókért lásd [tároló irányító parancssori eszközt](#storage-emulator-command-line-tool-reference).

A parancssor ablak nincs megnyitva, amikor a tároló irányító futtatásához továbbra is. Ahhoz, hogy újra meg a parancssort, kövesse a fenti lépéseket, mintha a tárhely irányító kezdve.

A tároló irányító az első futtatásakor a helyi tároló környezet van inicializálni meg. A inicializálni folyamat LocalDB hoz létre egy adatbázist, és az egyes helyi tároló szolgáltatásokhoz HTTP-portok fenntartja. 

A tároló irányító telepítve van a C:\Program Files (x86) \Microsoft SDKs\Azure\Storage irányító címtárhoz alapértelmezés szerint. 

### <a name="initialize-the-storage-emulator-to-use-a-different-sql-database"></a>A tároló irányító különböző SQL-adatbázis használata inicializálni

A tároló irányító parancssori eszköz használatával a tárhely irányító, mutasson az SQL-adatbázis példány nem az alapértelmezett LocalDB példány inicializálni. Tartsa szem előtt, hogy kell futtatnia a parancssori eszközt rendszergazdai jogosultságokkal a háttéradatbázist inicializálni tároló irányító:

1. Kattintson a **Start** gombra, vagy nyomja le a **Windows** billentyűt. Írja be `Azure Storage Emulator` , és jelölje ki azt, amikor úgy tűnik, jelenítse meg a tárhely irányító parancssori eszköz.
2. A parancssorablakban írja be a következő parancsot, ahol `<SQLServerInstance>` az SQL Server-példányt neve. LocalDb használni, adja meg a `(localdb)\v11.0` mint az SQL Server-példányt.

        AzureStorageEmulator init /server <SQLServerInstance> 
    
    A következő parancs, amely arra utasítja az alapértelmezett SQL Server-példányt használni a irányító is használhatja:

        AzureStorageEmulator init /server .\\ 

    Vagy használhatja az alapértelmezett LocalDB példányt az adatbázisból újra előkészíti a következő parancsot:

        AzureStorageEmulator init /forceCreate 

Ezek a parancsok kapcsolatos további tudnivalókért lásd [tároló irányító parancssori eszköz](#storage-emulator-command-line-tool-reference).

## <a name="addressing-resources-in-the-storage-emulator"></a>A tároló irányító címzésének források

A szolgáltatás végpontok tároló irányító eltérnek Azure tárterület-fiók. A különbség az, annak oka, hogy a helyi számítógép nem hajt végre tartomány névfeloldás, így a tárhely irányító végpontok megkövetelése a tartománynév helyett egy helyi cím.

Ha egy erőforrás az Azure tárterület-fiókokban található, címének, a következő terv, ahol a fiók nevét a URI állomásnév része, és az erőforrás megközelítése URI elérési része használhatja:

    <http|https>://<account-name>.<service-name>.core.windows.net/<resource-path>

Például az alábbi URI is érvényes, az Azure tárterület-fiókokban található blob-címet:

    https://myaccount.blob.core.windows.net/mycontainer/myblob.txt

A tároló irányító a helyi számítógép ne hajtsa végre a tartomány névfeloldás, mert a fióknév része helyett az állomásnév URI elérési. A tároló irányító futó erőforrás használhatja a következő sorrendben:

    http://<local-machine-address>:<port>/<account-name>/<resource-path>

A következő címre például eléréséhez a tárhely irányító a blob kell használni:

    http://127.0.0.1:10000/myaccount/mycontainer/myblob.txt

A szolgáltatás végpontok tároló irányító a következők:

    Blob Service: http://127.0.0.1:10000/<account-name>/<resource-path>
    Queue Service: http://127.0.0.1:10001/<account-name>/<resource-path>
    Table Service: http://127.0.0.1:10002/<account-name>/<resource-path>

### <a name="addressing-the-account-secondary-with-ra-grs"></a>A fiók TS-GRS a másodlagos címek

Kezdődő 3.1-es, a tárhely irányító fiókot támogat olvasásra geo felesleges replikációs (TS-GRS). Tárhely a felhőben, és a helyi irányító az erőforrások esetén is elérheti a másodlagos helyre szerint hozzáfűzése – másodlagos fiók nevére. A következő címre például eléréséhez a csak olvasható másodlagos használata a tárhely irányító blob kell használni:

    http://127.0.0.1:10000/myaccount-secondary/mycontainer/myblob.txt 

> [AZURE.NOTE] A tároló irányító, a másodlagos programozott érni használata a tárterület-ügyfél tár .NET 3,2 vagy újabb verziója. Lásd: további információt a [Microsoft Azure tárolási ügyfél tár a .NET rendszerhez](https://msdn.microsoft.com/library/azure/dn261237.aspx) .

## <a name="storage-emulator-command-line-tool-reference"></a>Tárterület irányító parancssori eszköz hivatkozás

A tároló irányító indításakor kezdve, 3.0-s verziója, megjelenik egy parancssori ablakban emlékeztető. A parancssori ablak segítségével elindítása és leállítása, valamint a irányító állapot lekérdezése és egyéb műveletek hajthatók végre.

> [AZURE.NOTE] Ha a Microsoft Azure irányító telepítve számítása, egy tálcaikonjára jelenik meg a tárhely irányító indításakor. Kattintson a jobb gombbal az ikonra kattintva jelenítse meg a menüt, amely elindítása és leállítása a tárhely irányító grafikus lehetőséget nyújt.

### <a name="command-line-syntax"></a>Parancssori szintaxisa

    AzureStorageEmulator [start] [stop] [status] [clear] [init] [help]

### <a name="options"></a>Beállítások

Írja be a beállításlista megtekintéséhez `/help` parancsot.

| A beállítás | Leírás                                                    | Parancs                                                                                                 | Argumentumok                                                                                                         |
|--------|----------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| **Indítása**  | A tároló irányító elindul.                                | `AzureStorageEmulator start [-inprocess]`                                                                    | *-inprocess*: a irányító indítása az aktuális folyamatban, egy új folyamat létrehozása helyett.                          |
| **állj**   | A tároló irányító leáll.                                    | `AzureStorageEmulator stop`                                                                                  |                                                                                                                   |
| **Állapot** | A tároló irányító állapotának nyomtatja.                     | `AzureStorageEmulator status`                                                                                |                                                                                                                   |
| **Törlése**  | Törli a jelet a parancssorban megadott szolgáltatások adatait. | `AzureStorageEmulator clear [blob] [table] [queue] [all]                                                    `| *BLOB*: törlése blob-adatokat. <br/>*a várakozási*: várólista adatok törlése. <br/>*táblázat*: törli a táblázatok adatait. <br/>*az összes*: az összes szolgáltatás az összes adat törlése. |
| **Init**   | Egyszeri inicializálni állíthatja be a irányító végrehajtása.       | `AzureStorageEmulator.exe init [-server serverName] [-sqlinstance instanceName] [-forcecreate] [-inprocess]` | *-kiszolgáló Kiszolgálónév\példánynév*: Itt adhatja meg az SQL-példány tároló kiszolgáló. <br/>*-sqlinstance példánynév*: az alapértelmezett kiszolgálói példány használt SQL-példány nevét adja meg. <br/>*-forcecreate*: az SQL-adatbázis létrehozása kezd, még akkor is, ha már létezik. <br/>*-inprocess*: az aktuális folyamat helyett egy új folyamat terjesztése inicializálni hajt végre. Az aktuális folyamat inicializálni végrehajtásához, emelt engedélyekkel rendelkező indítása          |
                                                                                                                  
## <a name="differences-between-the-storage-emulator-and-azure-storage"></a>A tároló irányító és Azure tároló közötti különbségek

Mivel a tárhely irányító egy helyi SQL-példány futó emulált környezeti, létezik néhány funkció közötti különbségek a irányító és Azure tárterület-fiókja a felhőben:

- A tároló irányító csak egyetlen rögzített fiók és egy jól ismert hitelesítési kulcs támogatja.

- A tároló irányító nem egy méretezhető tárhelyszolgáltatáshoz, és nem támogatja a sok egyidejű ügyfelek.

- [A tároló irányító címzés erőforrásainak](#addressing-resources-in-the-storage-emulator)leírtak erőforrások foglalkozik eltérően a tárhely irányító és Azure tárterület-fiókkal. Ez gyakran nem csak arra, hogy tartomány névfeloldás akkor érhető el a felhőben, de nem a helyi számítógépen.

- Kezdődő 3.1-es, a tárhely irányító fiókot támogat olvasásra geo felesleges replikációs (TS-GRS). A irányító, a minden fiók RA GRS engedélyezve van, és soha nincs bármely eltérés az elsődleges és másodlagos kópiák között. Blob-szolgáltatás stat első várólista szolgáltatást stat beszerzése és első tábla szolgáltatás stat műveletek az ügyfél, a másodlagos támogatottak, és mindig ad vissza az érték, a `LastSyncTime` válasz eleme, amely szerint az alapul szolgáló SQL-adatbázis a pontos idő.

    A tároló irányító, a másodlagos programozott érni használata a tárterület-ügyfél tár .NET 3,2 vagy újabb verziója. Lásd: további információt a [Microsoft Azure tárolási ügyfél tár a .NET rendszerhez](https://msdn.microsoft.com/library/azure/dn261237.aspx) .

- A fájl szolgáltatás és a kis-és Középvállalatok protokoll szolgáltatás végpontok jelenleg nem támogatottak a tárhely irányító a.

- A tároló irányító eredménye egy VersionNotSupportedByEmulator hiba (HTTP állapotkód 400 – Hibás kérés), ha a tárhely szolgáltatások verziójával rendelkezik, amely még nem támogatott a irányító, használja az verziója.

### <a name="differences-for-blob-storage"></a>Különbségek az Blob-tárolóhoz 

Az alábbi különbségek a irányító a Blob-tárolóhoz vonatkoznak:

- A tároló irányító csak támogatja blob legfeljebb 2 GB méretű.

- Egy elhelyezni Blob művelet sikerülhet szemben, amely szerepel-e a tárhely irányító, és az aktív bérleti blob, még akkor is, ha a bérleti azonosító nincs megadva a kérelem részeként. 

- Hozzáfűzi a irányító által nem támogatott műveletek Blob. Egy FeatureNotSupportedByEmulator hiba (HTTP állapotkód 400 – Hibás kérés) kísérel meg egy hozzáfűző blob műveletet adja eredményül.

### <a name="differences-for-table-storage"></a>Különbségek az táblatároló 

Az alábbi különbségek a irányító a táblatároló vonatkoznak:

- A táblázat szolgáltatás a tárhely irányító Dátumtulajdonságok támogatja a csak a tartomány, SQL Server 2005 által támogatott (*azaz*ezek szükség lehet későbbi, mint 1753 január 1). Ez az érték előtt 1753 január 1 kezdődő összes dátumot változnak. Dátumok pontosság korlátozódik a pontosság az SQL Server 2005, azaz a dátumok pontos 1/300th másodperc.

- A tároló irányító partíciót billentyűt, és a sor kulcs tulajdonságértékeket kisebb, mint 512 bájt támogatja. Emellett a fióknév, a táblázat neve és a főbb tulajdonságok neve együtt teljes mérete nem haladhatja meg a 900 bájt.

- A teljes egy-egy sor a tárhely irányító a táblázat mérete kisebb, mint 1 MB korlátozódik.

- Az adatok tulajdonságok írja be a tárhely irányító `Edm.Guid` vagy `Edm.Binary` támogatja a csak a `Equal (eq)` és `NotEqual (ne)` összehasonlító operátorok lekérdezésben szűrése karakterláncok.

### <a name="differences-for-queue-storage"></a>Különbségek várólista tárolására

Nincsenek várólista tárolóhoz a irányító az adott különbségek.

## <a name="storage-emulator-release-notes"></a>Tárterület irányító kibocsátási megjegyzések

### <a name="version-45"></a>4.5-ös verzió

- Rögzített inicializálni és telepítése sikertelen lesz a mögöttes adatbázis átnevezéskor a tárhely irányító okozó hiba.

### <a name="version-44"></a>Verzió 4.4.

- A tároló irányító támogatja a tároló szolgáltatást 2015-12-11-es verzió Blob várólista és táblázat szolgáltatás végponton.

- A tároló irányító szemétgyűjtő blob-adatok ettől hatékonyabb nagyszámú BLOB kapcsolatban felmerülő.

- Rögzített tároló vezérlés XML hogyan a tárhelyszolgáltatáshoz jelent az, a kissé másképp érvényesítendő okozó hiba.

- A hibát okozó néha max, min és dátum-és időértékek kell jelenteni, nem a megfelelő időzónát a rögzített.

### <a name="version-43"></a>A 4,3 verziója

- A tároló irányító támogatja a tároló szolgáltatást 2015-07-08 verziójának Blob várólista és táblázat szolgáltatás végponton.

### <a name="version-42"></a>Verzió 4.2.

- A tároló irányító támogatja a tároló szolgáltatást 2015-04-05 verziójának Blob várólista és táblázat szolgáltatás végponton.

### <a name="version-41"></a>4.1-es verziójában

- A tároló irányító verziójának 2015-02-21 Blob várólista és táblázat szolgáltatási végpontok tároló szolgáltatást kivételével az új hozzáfűző Blob-szolgáltatásokat támogatja. 

- A tároló irányító most értelmes hibaüzenetet ad vissza, ha a tárhely szolgáltatások verziójával rendelkezik, amely még nem támogatott a irányító adott verziója által. Azt javasoljuk, hogy a irányító legújabb verzióját használja. Ha egy VersionNotSupportedByEmulator hiba (HTTP állapotkód 400 – Hibás kérés), töltse le a tárhely irányító legújabb verzióját.

- A fajta okoz entitás Táblázatadatok egyidejű egyesítési művelet során nem helyes Itt megadhatja rögzített hiba.

### <a name="version-40"></a>4.0-s verzió

- A tároló irányító végrehajtható átnevezése a *AzureStorageEmulator.exe*.

### <a name="version-32"></a>A 3,2 verziója

- A tároló irányító támogatja a tároló szolgáltatást 2014-02-14 verziójának Blob várólista és táblázat szolgáltatás végponton. Figyelje meg, hogy fájl szolgáltatási végpontok jelenleg nem támogatottak a tárhely irányító a. Lásd: [a Azure tároló szolgáltatást a verziószámozás](https://msdn.microsoft.com/library/azure/dd894041.aspx) 2014-02 – 14-es verzióval kapcsolatban.

### <a name="version-31"></a>3.1-es verzióját

- A tároló irányító most támogatott olvasásra geo felesleges tároló (TS-GRS). A Blob-szolgáltatás stat első, az első várólista szolgáltatást stat és a első tábla szolgáltatás stat API-khoz fog mindig értékét adja eredményül a LastSyncTime válasz elem, az aktuális idő szerint az alapul szolgáló SQL-adatbázis használata támogatott, a másodlagos fiók. A tároló irányító, a másodlagos programozott érni használata a tárterület-ügyfél tár .NET 3,2 vagy újabb verziója. .NET hivatkozás a Microsoft Azure tároló ügyfél tár talál további információt.

### <a name="version-30"></a>3.0-s verzió

- Az Azure tároló irányító már nem teljesült, a számítási irányító azonos csomagban található.

- A tároló irányító grafikus felhasználói felület elavult parancsfájlok segítségével parancssor helyett. A parancssor a részletek a tárhely irányító parancssori eszköz – segédlet című cikkben. A grafikus felületen továbbra is megtalálható 3.0-s verziója, de csak elérhető legyen a tálcaikonjára a jobb gombbal, és válassza a tárterület irányító UI megjelenítése a kiszámítania irányító telepítésekor.

- Verzió 2013-08-15 az Azure tároló szolgáltatások most teljesen támogatott. (Korábban tároló irányító 2.2.1 verziója által támogatott az Ez a verzió csak előzetes.)

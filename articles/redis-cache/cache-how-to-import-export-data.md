<properties 
    pageTitle="Azure vgx.dll gyorsítótárban lévő adatok importálása és exportálása |} Microsoft Azure" 
    description="Útmutató: adatok importálása és exportálása és onnan való a prémium Azure vgx.dll gyorsítótár-példányok blob-tárolóhoz" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="import-and-export-data-in-azure-redis-cache"></a>Azure vgx.dll gyorsítótárban lévő adatok importálása és exportálása

Az importálás/exportálás az Azure vgx.dll gyorsítótár adatok kezelése művelet, amely lehetővé teszi az adatok importálása az Azure vgx.dll gyorsítótár és adatexportálás Azure vgx.dll gyorsítótárból importálása és exportálása vgx.dll gyorsítótár-adatbázis (Rekordadatbázis) pillanatkép prémium gyorsítótárból az Azure tárterület-fiókokban található lap blob. Az importálás/exportálás lehetővé teszi, hogy a különböző Azure vgx.dll gyorsítótár-példányok közötti áttelepítéséhez, vagy a gyorsítótár használat előtt adatokkal.

Ez a cikk útmutató nyújt a adatimportálás és adatexportálás az Azure vgx.dll gyorsítótár, és gyakran feltett kérdésekre adott válaszok biztosít.

>[AZURE.IMPORTANT] Az importálás/exportálás előzetes verzióban és lehetőség csak a [prémium réteg](cache-premium-tier-intro.md) gyorsítótárát.

## <a name="import"></a>Importálás

Importálás a bármely vgx.dll bármelyik felhő vagy környezetre, beleértve a vgx.dll Linux rendszerhez, a Windows vagy bármely felhő például Amazon webszolgáltatásokhoz és mások futó operációs rendszert futtató kiszolgáló kompatibilis Rekordadatbázis fájlokat vgx.dll tárolt használható. Adatok importálása az ugyanígy gyorsítótár létre előre megadott adatokkal. Az importálási folyamat során Azure vgx.dll gyorsítótár Rekordadatbázis fájlok betölti Azure tárhelyről a memóriában, és a billentyűk majd beillesztése a gyorsítótár.

>[AZURE.NOTE] Mielőtt elkezdené az importálás során, győződjön meg róla, a vgx.dll adatbázis (Rekordadatbázis) fájlt vagy fájlokat feltölteni az Azure-tárolóban lévő, az régió és az előfizetés az Azure vgx.dll gyorsítótár-példányt lap BLOB. További tudnivalókért olvassa el a [Azure Blob-tárolóhoz – első lépések](../storage/storage-dotnet-how-to-use-blobs.md)című témakört. Az exportált Rekordadatbázis fájljait a [Azure vgx.dll gyorsítótár exportálása](#export) funkció használata, ha a Rekordadatbázis fájl már van tárolva lap blob és készen áll a importálása.

1. Egy vagy több exportált gyorsítótár BLOB, az Azure-portálon a [Tallózás gombra, és a gyorsítótár](cache-configure.md#configure-redis-cache-settings) és **adatok importálása** gombra a **Beállítások** lap a gyorsítótár-példány a.

    ![Adatok importálása][cache-import-data]

2. **Válassza a Blob(s)** kattintson, és jelölje ki a tárterület-fiókot az importálandó adatokat tartalmazó.

    ![Válassza a tárterület-fiók][cache-import-choose-storage-account]

3. Az importálandó adatokat tartalmazó tároló gombra.

    ![Válassza ki a tárolóhoz][cache-import-choose-container]

4. Egy vagy több BLOB importálhatja a terület bal oldalán a blob nevére kattintva jelölje ki, és kattintson a **Jelölje ki**.

    ![Válassza a BLOB][cache-import-choose-blobs]

5. Kattintson a **importálása** az importálás megkezdéséhez.

    >[AZURE.IMPORTANT] A gyorsítótár nem érhető el gyorsítótár-ügyfelek az importálási folyamat során, és a meglévő, a gyorsítótárban lévő adatok törlődik.

    ![Importálás][cache-import-blobs]

    Az importálási művelet végrehajtásának figyelheti az értesítés az Azure portál vagy az események megtekintése a [naplózási jelentkezzen](cache-configure.md#support-amp-troubleshooting-settings)be.

    ![Importálás folyamatban][cache-import-data-import-complete] 


## <a name="export"></a>Exportálás

Exportálás az Azure-vgx.dll vgx.dll kompatibilis Rekordadatbázis fájlokat a gyorsítótárban tárolt adatokat exportálni teszi lehetővé. Ez a funkció használatával adatok áthelyezése egyik Azure vgx.dll gyorsítótár példányából, vagy egy másik vgx.dll kiszolgálóra. Az exportálási folyamat során ideiglenes fájl a virtuális, amelyen az Azure vgx.dll gyorsítótár-kiszolgálói példány jön létre, és a fájlt töltenek fel a kijelölt tárterület-fiókjába. Az exportálási művelet befejeztével vagy állapotban sikeres vagy sikertelen az ideiglenes fájlok törlődik.

1. A tárterület-gyorsítótár, [tallózással keresse meg a gyorsítótár](cache-configure.md#configure-redis-cache-settings) az Azure-portálon aktuális tartalmának exportálása, és kattintson a **Beállítások** lap a gyorsítótár-példány **exportálása** .

    ![Válassza a tárterület-tároló][cache-export-data-choose-storage-container]

2. **Válassza a tárterület-tároló** gombra, és válassza ki a kívánt tárterület-fiókot. A tárterület-fiókot kell lennie előfizetést és a régió, a gyorsítótár.

    >[AZURE.IMPORTANT] Az importálás/exportálás működik, oldal okkal, amely klasszikus és a ARM tároló partnerek által támogatott, de [Blob-tároló partnerek](../storage/storage-blob-storage-tiers.md#blob-storage-accounts) által jelenleg nem támogatott.

    ![Tárterület-fiók][cache-export-data-choose-account]

3. Válassza ki a kívánt blob-tárolóhoz, és kattintson a **Kijelölés**gombra. Új tároló, kattintson **A tároló hozzáadása** először felvennie azt, és válasszon a listából.

    ![Válassza a tárterület-tároló][cache-export-data-container]

4. Írja be a **Blob név előtaggal** , és kattintson az **Exportálás** az exportálási folyamat indításához. A blob-név előtaggal előtag az exportálási művelet által létrehozott fájlok azoknak a szolgál.

    ![Exportálás][cache-export-data]

    Az értesítés az Azure portál vagy az események megtekintése a [naplózási jelentkezzen](cache-configure.md#support-amp-troubleshooting-settings)be az exportálási művelet végrehajtásának figyelheti.

    ![][cache-export-data-export-complete]

    Az exportálási folyamat során gyorsítótárát is használható.


## <a name="importexport-faq"></a>Gyakori kérdések az importálás/exportálás

Ez a szakasz az importálás/exportálás funkcióival kapcsolatos gyakori kérdéseket tartalmaz.

-   [Milyen árak tiers lehetőség az importálás/exportálás?](#what-pricing-tiers-can-use-importexport)
-   [Importálhatok adatok bármely vgx.dll kiszolgálóról?](#can-i-import-data-from-any-redis-server)
-   [A gyorsítótár érhetők el az importálási vagy exportálási művelet során?](#will-my-cache-be-available-during-an-importexport-operation)
-   [Van lehetőség az importálás/exportálás vgx.dll fürthöz?](#can-i-use-importexport-with-redis-cluster)
-   [Az importálás/exportálás működése a egy egyéni adatbázisban beállítást?](#how-does-importexport-work-with-a-custom-databases-setting)
-   [Miben különbözik az importálás/exportálás az adatmegőrzési vgx.dll?](#how-is-importexport-different-from-redis-persistence)
-   [Automatizálható importálás/exportálás PowerShell, CLI vagy más management ügyfelek?](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
-   [Az importálási vagy exportálási művelet során kaptam időtúllépésről szóló hibaüzenetet. Mit jelent ez?](#i-received-a-timeout-error-during-my-importexport-operation.-what-does-it-mean)
-   [Kaptam hiba az adatok exportálásakor Azure Blob-tárolóhoz mi történt?](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage.-what-happened)


### <a name="what-pricing-tiers-can-use-importexport"></a>Milyen árak tiers lehetőség az importálás/exportálás?

Az importálás/exportálás csak a réteg árak támogatás érhető el.

### <a name="can-i-import-data-from-any-redis-server"></a>Importálhatok adatok bármely vgx.dll kiszolgálóról?

Igen, mellett az Azure vgx.dll gyorsítótár-példányok exportálja az adatok importálása importálhat Rekordadatbázis fájlok a Windows rendszert futtató bármelyik felhő vagy Linux rendszerhez, például a környezet vgx.dll kiszolgálóról és a felhő szolgáltatók, például az Amazon webszolgáltatásokhoz. Ehhez a kívánt vgx.dll kiszolgáló Rekordadatbázis fájlból feltölteni az Azure tárterület-fiókokban található lap blob, és importálja a prémium Azure vgx.dll gyorsítótár-példány. Például érdemes exportálja az adatokat a termelési gyorsítótárból, és importálja a fejlesztői környezet részeként használható tesztelése és az áttelepítés gyorsítótárat. 

### <a name="will-my-cache-be-available-during-an-importexport-operation"></a>A gyorsítótár érhetők el az importálási vagy exportálási művelet során?

-   A **export** - gyorsítótárát elérhetők maradnak, és továbbra is a gyorsítótár használata közben az exportálási művelet.
-   **Importálás** - gyorsítótárát válnak elérhetetlenné, amikor elindítja az importálás során, és az importálási művelet befejezése után válik elérhetővé.


### <a name="can-i-use-importexport-with-redis-cluster"></a>Van lehetőség az importálás/exportálás vgx.dll fürthöz?

Igen, és meg is importálás/exportálás egy csoportosított gyorsítótár és egy nem csoportosított gyorsítótár között. Óta vgx.dll fürt [csak támogatja az adatbázis 0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), 0 eltérő adatbázisokban adatokat nem importálhatók. Csoportosított gyorsítótár adatok importálásakor a billentyűk van osztja el a shards a fürt. 

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a>Az importálás/exportálás működése a egy egyéni adatbázisban beállítást?

Néhány árak rétegek van különböző [adatbázisok korlátozások](cache-configure.md#databases), így dolgot kell néhány importálásakor, ha úgy állította be, hogy az egyéni érték a `databases` beállítás alatt gyorsítótárának létrehozása.

-   Az egy alsó egy árak réteg célobjektuma `databases` korlát, mint a réteg, ahonnan exportálni:
    -   Ha használja az alapértelmezett száma `databases` melyiket érdemes az összes árak rétegek 16, nincs adat elvész.
    -   Ha használja az egyéni szám `databases` korlátok adott esik, a réteg, amelyhez importál, az adatok nélkül elvész.
    -   Ha az exportált adatokat egy adatbázist, amely az új réteg határain meghaladja az adatokat, a program nem importálja az adott magasabb adatbázisból származó adatok.

### <a name="how-is-importexport-different-from-redis-persistence"></a>Miben különbözik az importálás/exportálás az adatmegőrzési vgx.dll?

Azure vgx.dll gyorsítótár adatmegőrzési Azure tárolóhoz vgx.dll tárolt adatok megmaradnak teszi lehetővé. Adatmegőrzési konfigurálásakor Azure vgx.dll gyorsítótár továbbra is fennáll a vgx.dll gyorsítótár vgx.dll bináris formátumban egy konfigurálható biztonsági gyakoriság alapján lemezre pillanatkép megtekintéséhez. Esetben a katasztrofális eseményeket, amely letiltja az elsődleges és a replika gyorsítótárat a gyorsítótár adatok visszahelyezi használatával automatikusan a legutóbbi pillanatkép. További információért megtudhatja, [hogy miként adatok adatmegőrzési prémium Azure vgx.dll gyorsítótár beállítása](cache-how-to-premium-persistence.md).

Importálás / exportálás lehetővé teszi az adatokat vihet vagy exportálása Azure vgx.dll gyorsítótárból. Állítsa be a biztonsági másolat, és nem visszaállítás vgx.dll adatmegőrzési segítségével.


### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a>Automatizálható importálás/exportálás PowerShell, CLI vagy más management ügyfelek?

Igen, PowerShell témaköröket olvassa el [a vgx.dll gyorsítótár importálása](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) és [exportálása egy vgx.dll gyorsítótár](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).



### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a>Az importálási vagy exportálási művelet során kaptam időtúllépésről szóló hibaüzenetet. Mit jelent ez?

Ha továbbra is a az **adatok importálása** vagy **exportálása** lap kezdeményezése a művelet előtt 15 percnél hosszabb, az alábbihoz hasonló hibaüzenetet kap.

    The request to import data into cache 'contoso55' failed with status 'error' and error 'One of the SAS URIs provided could not be used for the following reason: The SAS token end time (se) must be at least 1 hour from now and the start time (st), if given, must be at least 15 minutes in the past.

Megoldásához, az importálás megkezdéséhez vagy exportálási művelet előtt 15 percet telt el.

### <a name="i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened"></a>Kaptam hiba az adatok exportálásakor Azure Blob-tárolóhoz mi történt?

Az importálás/exportálás együttműködve csak Rekordadatbázis fájlok tárolása, oldal BLOB. Más blob nem támogatja a jelenleg, beleértve blob tárhely, hideg- és melegvíz-meghatározási.


## <a name="next-steps"></a>Következő lépések
Megtudhatja, hogy miként prémium-gyorsítótár további funkcióinak használata.

-   [Az Azure-gyorsítótár prémium vgx.dll réteg – bevezetés](cache-premium-tier-intro.md)    

  
<!-- IMAGES -->
[cache-settings-import-export-menu]: ./media/cache-how-to-import-export-data/cache-settings-import-export-menu.png
[cache-export-data-choose-account]: ./media/cache-how-to-import-export-data/cache-export-data-choose-account.png
[cache-export-data-choose-storage-container]: ./media/cache-how-to-import-export-data/cache-export-data-choose-storage-container.png
[cache-export-data-container]: ./media/cache-how-to-import-export-data/cache-export-data-container.png
[cache-export-data-export-complete]: ./media/cache-how-to-import-export-data/cache-export-data-export-complete.png
[cache-export-data]: ./media/cache-how-to-import-export-data/cache-export-data.png
[cache-import-data]: ./media/cache-how-to-import-export-data/cache-import-data.png
[cache-import-choose-storage-account]: ./media/cache-how-to-import-export-data/cache-import-choose-storage-account.png
[cache-import-choose-container]: ./media/cache-how-to-import-export-data/cache-import-choose-container.png
[cache-import-choose-blobs]: ./media/cache-how-to-import-export-data/cache-import-choose-blobs.png
[cache-import-blobs]: ./media/cache-how-to-import-export-data/cache-import-blobs.png
[cache-import-data-import-complete]: ./media/cache-how-to-import-export-data/cache-import-data-import-complete.png









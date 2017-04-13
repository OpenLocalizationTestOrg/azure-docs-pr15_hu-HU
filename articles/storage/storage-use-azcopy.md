<properties
    pageTitle="Másolja vagy helyezze át az adatokat tároló AzCopy a |} Microsoft Azure"
    description="A AzCopy segédprogrammal áthelyezése vagy másolása az adatokat, illetve blob, a táblázat és a fájl tartalmát. Azure tárolóhoz adatok helyi fájlok másolása vagy adatokat belül, illetve a tárhely fiókok között. Az adatok egyszerűen áttelepítése Azure-tárterületet."
    services="storage"
    documentationCenter=""
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="micurd"/>

# <a name="transfer-data-with-the-azcopy-command-line-utility"></a>A AzCopy parancssori segédprogram az adatok átvitele

## <a name="overview"></a>– Áttekintés

AzCopy az adatok másolása, és az optimális teljesítmény egyszerű parancsok használata a Microsoft Azure Blob, a fájl és a táblázat tárhelyről készült Windows parancssori segédprogramot. Másolhatja adatok egyik objektumból a másikba tárterület-fiókja vagy tároló fiókok között.

> [AZURE.NOTE] Ez az útmutató feltételezi, hogy ismeri már a [Azure tároló](https://azure.microsoft.com/services/storage/). Ha nem, hasznosak lehetnek az [Azure tároló – bevezetés](storage-introduction.md) dokumentáció olvasási lesz. Legfontosabb szüksége lesz [a tárterület-fiók](storage-create-storage-account.md#create-a-storage-account) létrehozása AzCopy használat érdekében.

## <a name="download-and-install-azcopy"></a>Töltse le és telepítse a AzCopy

### <a name="windows"></a>A Windows

Töltse le a [AzCopy legújabb verzióját](http://aka.ms/downloadazcopy).

### <a name="maclinux"></a>A Mac vagy Linux rendszerhez

A Mac vagy Linux OSs AzCopy nem érhető el. Azure CLI azonban egy megfelelő helyettesítő az adatok másolása, és Azure-tárhelyről. Olvassa el [az Azure CLI Azure adathordozós használatával](storage-azure-cli.md) kapcsolatos további.

## <a name="writing-your-first-azcopy-command"></a>Az első AzCopy parancs írása

Egyszerű AzCopy parancsok szintaxisa:

    AzCopy /Source:<source> /Dest:<destination> [Options]

A parancs ablak megnyitásához, és nyissa meg azt a számítógép - AzCopy telepítési mappájába hol a `AzCopy.exe` végrehajtható fájl található. Ha azt szeretné, a rendszer elérési út felveheti AzCopy telepítési helyet. Alapértelmezés szerint a AzCopy telepítve van a `%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` vagy `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.

Az alábbi példák bemutatják az adatok másolása és a Microsoft Azure BLOB, fájlok és táblázatok forgatókönyvek számos. Keresse meg az egyes mintákban használt paraméterek részletes leírását a [AzCopy paraméterek](#azcopy-parameters) szakaszát.

## <a name="blob-download"></a>BLOB: letöltése

### <a name="download-single-blob"></a>Egyetlen blob letöltése

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"

Megjegyzés: Ha a mappa `C:\myfolder` nem létezik, AzCopy fog létrehozni a dokumentumot, és töltse le a `abc.txt ` az új mappába.

### <a name="download-single-blob-from-secondary-region"></a>Töltse le a egyetlen blob másodlagos terület

    AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Figyelje meg, hogy olvasásra geo felesleges tároló engedélyezve kell rendelkeznie.

### <a name="download-all-blobs"></a>Az összes BLOB letöltése

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S

Tegyük fel, az alábbi BLOB a megadott tárolóban találhatók:  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

A letöltési műveletet, a címtárban után `C:\myfolder` tartalmazni fogja a következő fájlokat:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

Ha nem adja meg a beállítás `/S`, nincs BLOB-mait letölt.

### <a name="download-blobs-with-specified-prefix"></a>Név előtaggal megadott BLOB letöltése

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S

Tegyük fel, az alábbi BLOB a megadott tárolóban találhatók. A előtaggal kezdődő összes BLOB `a` tölti le:

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

A letöltési műveletet, a mappa után `C:\myfolder` tartalmazni fogja a következő fájlokat:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

Az előtag a virtuális könyvtár, blob nevének első részét képező vonatkozik. A fenti példában a virtuális könyvtár értéke nem egyezik meg a megadott előtagot, így esetén nem töltődik le. Ezenkívül ha a beállítás `\S` nincs megadva, AzCopy nem tölt le bármely BLOB.

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a>A legutóbbi módosítás időpont exportált fájlokat lehet ugyanaz, mint a forrás BLOB beállítása

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT

BLOB is zárni a letöltési műveletet, a legutóbbi módosítás időpontja alapján. Például ha el szeretne rejteni BLOB, amelyek legutóbbi módosítás időpontja is ugyanazon vagy újabb, mint a célfájl hozzáadása a `/XN` beállítást:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN

Vagy ha el szeretne rejteni, amelyek legutóbbi módosítás időpontja BLOB ugyanazon vagy régebbi, mint a célfájl hozzáadása a `/XO` beállítást:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO

## <a name="blob-upload"></a>BLOB: feltöltése

### <a name="upload-single-file"></a>Egy fájlból feltöltése

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"

Ha a megadott cél tároló nem létezik, AzCopy létrehozni a dokumentumot, és töltse fel a fájlt a mikrofonba.

### <a name="upload-single-file-to-virtual-directory"></a>Egy fájlból virtuális könyvtár feltöltése

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt

Ha a megadott virtuális könyvtár nem létezik, AzCopy feltöltődnek a fájlt, ha meg szeretné jeleníteni a virtuális könyvtár a nevük (*pl.* `vd/abc.txt` a fenti példában).

### <a name="upload-all-files"></a>Az összes fájl feltöltése

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S

A beállítás megadása `/S` feltölti Blob-tároló rekurzív, ami azt jelenti, hogy minden almappák és a fájlok feltöltődnek, valamint a megadott könyvtár tartalmát. Tegyük fel például, mappában találhatók a következő fájlok `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

A feltöltés művelet után a tároló a következő fájlokat is tartalmazza:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Ha nem adja meg a beállítás `/S`, AzCopy nem feltöltődnek rekurzív. A feltöltés művelet után a tároló a következő fájlokat is tartalmazza:

    abc.txt
    abc1.txt
    abc2.txt

### <a name="upload-files-matching-specified-pattern"></a>Megadott mintával egyező fájlok feltöltése

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S

Tegyük fel, az alábbi fájlok mappában találhatók `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

A feltöltés művelet után a tároló a következő fájlokat is tartalmazza:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Ha nem adja meg a beállítás `/S`, AzCopy csak tölthet fel, amelyek nem találhatók a virtuális könyvtárában található BLOB:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a>Adja meg a célként megadott blob MIME tartalomtípust

Alapértelmezés szerint AzCopy állítja be a cél blob tartalomtípusa `application/octet-stream`. Verzió 3.1.0 kezdve explicit módon adhatja meg a tartalomtípust a beállítás keresztül `/SetContentType:[content-type]`. E szintaktikai valamennyi BLOB-tartalomtípus a feltöltés művelet állítja be.

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4

Ha megad `/SetContentType` anélkül, hogy egy értéket, majd AzCopy állítja minden blob vagy fájl webhelytartalom-típus szerint a fájl kiterjesztését.

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType

## <a name="blob-copy"></a>BLOB: másolása

### <a name="copy-single-blob-within-storage-account"></a>Egyetlen blob-tároló számla belüli másolása

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt

A tárhely fiók belül blob másolásakor a [kiszolgálóoldali](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) fájlmásolás történik.

### <a name="copy-single-blob-across-storage-accounts"></a>Másolja a vágólapra egyetlen blob tároló fiókok között

    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt

Másolásakor blob tároló fiókok különböző [kiszolgálóoldali](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) fájlmásolás történik.

### <a name="copy-single-blob-from-secondary-region-to-primary-region"></a>Egyetlen blob másolása a másodlagos terület az elsődleges terület

    AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt

Figyelje meg, hogy olvasásra geo felesleges tároló engedélyezve kell rendelkeznie.

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a>Másolja a vágólapra egyetlen blob- és a pillanatképek tároló fiókok között

    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot

A másolás után a cél tároló a blob- és a pillanatképek fogja tartalmazni. Feltételezve, hogy a fenti példában blob két pillanatképek rendelkezik, a tároló tartalmazza a következő blob és pillanatképek:

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="synchronously-copy-blobs-across-storage-accounts"></a>Szinkronizált másolása BLOB tároló fiókok között

Alapértelmezés szerint AzCopy aszinkron másolja az adatokat tároló végpontokat között. Emiatt a Másolás fog futni a háttérben használatával hogyan pedig nincs SLA gyors tartalmazó tartalék sávszélesség kapacitás blob kerülnek, és AzCopy rendszeresen ellenőrizni az állapot másolása mindaddig, amíg a másolás befejeződött, vagy nem sikerült.

A `/SyncCopy` lehetőség biztosítja, hogy a másolási művelet kapja egységes sebességét. AzCopy a szinkronizált másolat letöltése a BLOB a megadott forrás helyi memóriában szeretné másolni, és feltöltheti a Blob-tároló cél hajt végre.

    AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy

`/SyncCopy`Előfordulhat, hogy készítése további kilépési költség aszinkron másolása és összehasonlítása, a javasolt megközelítés, az Azure virtuális, amely a forrás tároló fiókként kilépési költségek elkerülésére ugyanabban a régióban használni ezt a beállítást.

## <a name="file-download"></a>Fájl: letöltése

### <a name="download-single-file"></a>Egy fájlból letöltése

    AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Ha a megadott forrás egy Azure fájlmegosztás, akkor vagy adja meg pontosan a fájl nevét (*például* `abc.txt`) töltse le egy fájlt, vagy adja meg a beállítás `/S` a megosztás rekurzív lévő összes fájl letöltése. Adja meg a fájl minta és a beállítás próbál `/S` közös hibát eredményez.

### <a name="download-all-files"></a>Az összes fájl letöltése

    AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceKey:key /S

Figyelje meg, hogy minden olyan üres mappát, nem lehet letölteni.

## <a name="file-upload"></a>Fájl: feltöltése

### <a name="upload-single-file"></a>Egy fájlból feltöltése

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:abc.txt

### <a name="upload-all-files"></a>Az összes fájl feltöltése

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S

Figyelje meg, hogy minden olyan üres mappát, nem lehet feltölteni.

### <a name="upload-files-matching-specified-pattern"></a>Megadott mintával egyező fájlok feltöltése

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S

## <a name="file-copy"></a>Fájl: másolása

### <a name="copy-across-file-shares"></a>Másolja át fájlmegosztások

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S

### <a name="copy-from-file-share-to-blob"></a>Megosztás blob-fájl másolása

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S

Figyelje meg, hogy a aszinkron bemásolja a fájltároló lapon Blob nem támogatott.

### <a name="copy-from-blob-to-file-share"></a>Fájl megosztása blob másolása

    AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S

### <a name="synchronously-copy-files"></a>Szinkronizált fájlok másolása

Megadhatja a `/SyncCopy` adatok másolásához a fájlt tároló tárhely, fájltároló Blob-tárolóhoz és Blob-tárolóhoz való Fájltárolással egyidejű beállítás, AzCopy végzi a forrásadatok helyi memória letöltésével, és töltse fel újra célhelyre.

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy

Másolásakor a fájltároló Blob-tárolóhoz, az alapértelmezett blob-típus blokk blob, felhasználó megadhatja a beállítás `/BlobType:page` a cél blob módosítása.

Megjegyzendő, hogy `/SyncCopy` költség aszinkron másolandó összehasonlítása további kilépési hozhat létre, a javasolt megközelítés használni ezt a lehetőséget a Azure virtuális, amely ugyanabban a régióban kilépési költség elkerülése érdekében a forrás tároló fiókként.

## <a name="table-export"></a>Táblázat: exportálás

### <a name="export-table"></a>Táblázat exportálása

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key

AzCopy nyilvánvalóan fájl a megadott célmappába ír. Keresse meg a szükséges adatfájlokat, és végezze el az adatok érvényesítése a nyilvánvalóan fájlt az importálási folyamat használják. A nyilvánvalóan fájl alapértelmezés szerint az alábbi elnevezési konvenció használ:

    <account name>_<table name>_<timestamp>.manifest

Felhasználó is beállíthatja a beállítás `/Manifest:<manifest file name>` beállítása a nyilvánvalóan fájlnevet.

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest

### <a name="split-export-into-multiple-files"></a>Több fájl az Exportálás felosztása

    AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100

AzCopy *mennyiségi index* létrehozása az osztott adatok fájlnevekben több fájl megkülönböztetni használja. A mennyiségi index két részből áll, *partíciót fő tartomány index* létrehozása és egy *fájl index felosztása*. Mindkét indexek nulla alapú.

A partíciók kulcs tartomány index 0 lesz, ha a felhasználó nem adja meg a beállítás `/PKRS`.

Tegyük fel például, AzCopy hoz létre két adatfájlt, a felhasználó adja meg a beállítás után `/SplitSize`. Az eredményül kapott adatok fájlnevek lehet:

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

Megjegyzendő, hogy a beállítás értéke a minimális lehetséges `/SplitSize` 32 MB. Ha a megadott cél Blob-tárolóhoz, AzCopy felosztása az adatfájl, miután a méretű eléri a blob-méretkorlátja (200GB), függetlenül attól, hogy beállítás `/SplitSize` a felhasználó által lett megadva.

### <a name="export-table-to-json-or-csv-data-file-format"></a>A táblázat exportálása JSON- vagy CSV adatok fájlformátum

Alapértelmezés szerint AzCopy táblák exportálása JSON-adatfájlokat. Megadhatja, hogy a beállítás `/PayloadFormat:JSON|CSV` a táblák exportálása JSON-vagy CSV.

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV

A tartalom a CSV formátum megadása esetén AzCopy is készítése a séma kiterjesztésű fájl `.schema.csv` minden adatfájl.

### <a name="export-table-entities-concurrently"></a>Táblázat szervezetek egyidejűleg exportálása

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"

AzCopy egyidejű műveletek exportálása szervezetek, amikor a felhasználó beállítást adja meg a program ekkor elkezdi `/PKRS`. Az egyes műveletek partíciót egy fő tartomány exportálja.

Figyelje meg, hogy egyidejű műveletek számát is vezérelhető beállítás `/NC`. AzCopy core processzorok számának használja az alapértelmezett értéket `/NC` másolásakor a táblázat személyek, még ha `/NC` nincs megadva. Amikor a felhasználó megadja a beállítás `/PKRS`, AzCopy használja a két érték közül a kisebb – partíciót kulcs tartományok és implicit és explicit megadott egyidejű műveletek - való egyidejű műveletek indításához számának meghatározása. További részletekért írja be a `AzCopy /?:NC` a parancssorból.

### <a name="export-table-to-blob"></a>A blob-táblázat exportálása

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2

AzCopy hoz létre egy JSON adatfájl az elnevezésük is egységes követő blob-tárolóban be:

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

A létrehozott JSON-adatfájl követi a minimális metaadat-alapú tartalom formátumát. A tartalom formátum a részletekért olvassa el [Tartalom formázása táblázat szolgáltatási műveletek](http://msdn.microsoft.com/library/azure/dn535600.aspx).

Ne feledje, hogy ha BLOB táblák exportál, AzCopy fog töltse le a táblázat személyek helyi ideiglenes adatfájlokat töltse a blob e szervezetek. Ideiglenes adatfájlok az alapértelmezés szerint a napló fájl mappába kerülnek "<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>", megadhatja, hogy a beállítás/Z: [lapjának-fájl-mappa] módosíthatja a napló fájl a mappa helyét, és így az ideiglenes fájlok helyének módosítása. Az ideiglenes fájlok méretét, amelyekről a táblázat személyek és a beállítás /SplitSize a megadott mérete, bár a merevlemezre az ideiglenes adatfájl azonnali törlődik, miután feltöltötte a blob szeretne, ellenőrizze, hogy van elegendő helyi lemezterület ezek ideiglenes adatok fájlok tárolására, törlés előtt.

## <a name="table-import"></a>Táblázat: importálása

### <a name="import-table"></a>Tábla importálása

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace

A beállítás `/EntityOperation` azt jelzi, hogyan szúrhat be a személyek a táblázatba. Lehetséges értékek a következők:

- `InsertOrSkip`: Egy meglévő személy átugrása, vagy szúrja be egy új entitás, ha nem létezik a táblázatban.
- `InsertOrMerge`: Meglévő entitás egyesíti, vagy szúrja be egy új entitás, ha nem létezik a táblázatban.
- `InsertOrReplace`: Lecseréli a meglévő entitás, vagy szúrja be egy új entitás, ha nem létezik a táblázatban.

Megjegyzés:, hogy a beállítás nem adhat meg `/PKRS` az importálás használatával. Az Exportálás az esetben, amelyben meg kell adnia a beállítás eltérően `/PKRS` egyidejű műveletek indításához AzCopy alapértelmezés szerint elkezdenek egyidejű műveletek tábla importálásakor. Az alapértelmezett szám lépések egyidejű műveletek megegyezik a core processzorok; száma jó helyen jár, megadhatja a különféle egyidejű beállítás `/NC`. További részletekért írja be a `AzCopy /?:NC` a parancssorból.

Figyelje meg, hogy AzCopy csak támogatja a JSON, nem CSV-az importálást. AzCopy nem támogatja a felhasználó által létrehozott JSON táblából származó és cikkét fájlokat. Ezeket a fájlokat a táblázat exportálása AzCopy kell származnia. Hibák elkerülése érdekében, hogy ne módosítsa az exportált JSON vagy fájl cikkét.

### <a name="import-entities-to-table-using-blobs"></a>Személyek BLOB használatával táblázat importálása

Tegyük fel, Blob-tároló tartalmazza a következő: Azure-táblából és a hozzá mellékelt nyilvánvalóan fájl, amely A JSON-fájlt.

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

A következő parancsot a szervezetek importálhatja egy táblába, használja a nyilvánvalóan fájlt, hogy blob-tárolóban futtatását is lehetővé teszi:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"

## <a name="other-azcopy-features"></a>Egyéb AzCopy szolgáltatások

### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a>Csak adatok másolása, amely nem szerepel a cél

A `/XO` és `/XN` paraméterek lehetővé teszi, hogy a másolt, illetve a régebbi vagy újabb verziójával forrás erőforrások kizárása. Ha csak másolni kívánt forrás források, amelyek nem szerepelnek a cél, a mindkét paraméter a AzCopy parancs a is megadhatja:

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

Megjegyzés: Ez nem támogatott esetén a forráslista vagy a cél táblához.

### <a name="use-a-response-file-to-specify-command-line-parameters"></a>Parancssori paramétert válasz fájl használatával

    AzCopy /@:"C:\responsefiles\copyoperation.txt"

Válasz fájl AzCopy parancssori paramétereket is. AzCopy dolgozza fel a fájlt a paraméterek, mintha a parancssorban megadott lett a fájl tartalmát a közvetlen helyettesítés hajt végre.

Tegyük fel, a válasz nevű fájlt `copyoperation.txt`, a következő sorokat tartalmazza. AzCopy paramétereknél lehet megadni egy sorban

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

vagy külön sorokban:

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

AzCopy meghiúsul, ha a paraméter felosztása két sorra végig az itt ismertetett módon a `/sourcekey` paraméter:

    http://myaccount.blob.core.windows.net/mycontainer
    C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-to-specify-command-line-parameters"></a>Parancssori paramétert több válasz-fájl használatával

Tegyük fel, a válasz nevű fájlt `source.txt` forrás tároló Itt adhatja meg:

    /Source:http://myaccount.blob.core.windows.net/mycontainer

Egy válaszcsoport- fájl nevű `dest.txt` , amely egy célmappára adja meg a fájlrendszerben található:

    /Dest:C:\myfolder

Egy válaszcsoport- fájl nevű `options.txt` , amely megadja AzCopy lehetőségeit:

    /S /Y

Ezekkel a válasz-fájlokkal AzCopy felhívására, ami lakik könyvtárában található `C:\responsefiles`, ezzel a paranccsal:

    AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   

AzCopy folyamatok Ez a parancs, ahogyan azt szeretné, ha felvette a parancssorban az egyes paraméterek mindegyikében:

    AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

### <a name="specify-a-shared-access-signature-sas"></a>Adja meg a megosztott access aláírás (Társítások)

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt

A Társítások is a tároló URI adhat meg:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S

### <a name="journal-file-folder"></a>Napló fájl mappa

Minden alkalommal, amikor egy parancs elküldésekor AzCopy, hogy az e lapjának fájl megtalálható-e az alapértelmezett mappát, vagy hogy létezik, amely a megadott beállítás keresztül mappában ellenőrzi. Ha a Jegyzetfüzet-fájlban nem létezik vagy helyen, AzCopy új tekinti a művelet, és létrehoz egy új lapjának fájlt.

A naplófájl létezik, ha AzCopy ellenőrzi, hogy bevitt parancssori megegyezik-e a parancssorból a napló fájlban. Ha felel meg a két parancssorokat, AzCopy folytatja a hiányos működését. Ha nincs egyeznek, a rendszer kéri, a naplófájl új művelet indítása, illetve az aktuális művelet visszavonása vagy felülírásához.

Ha azt szeretné, az alapértelmezett helyet a Jegyzetfüzet-fájlban használni:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z

Ha nincs megadva beállítás `/Z`, vagy adja meg a beállítás `/Z` anélkül, hogy a mappa elérési útja, ahogy a fenti AzCopy hoz létre a naplófájl alapértelmezett hely, amely `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`. Ha a Jegyzetfüzet-fájlban már létezik, AzCopy folytatja a műveletet, a naplófájl alapján.

Ha szeretne megadni egy egyéni a napló fájl helyét:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\

Ebben a példában a napló fájlt hoz létre, ha még nem létezik. Ha létezik, AzCopy folytatja a műveletet, a naplófájl alapján.

Ha szeretne egy AzCopy folytatása:

    AzCopy /Z:C:\journalfolder\

Ebben a példában a legutóbbi művelet elvégzéséhez nem sikerült folytatódik.

### <a name="generate-a-log-file"></a>Hozzon létre naplófájlt

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V

Ha a beállítás `/V` anélkül, hogy egy fájl elérési útját a részletes naplózás, majd AzCopy naplófájlt hoz létre az alapértelmezett hely, amely `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.

Egyéb esetben egy naplófájl hozhat létre egyéni helyen:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log

Ne feledje, ha a következő beállítás relatív elérési utat megadhat `/V`, például: `/V:test/azcopy1.log`, akkor jön létre a részletes naplózás az aktuális munkakönyvtár nevű almappába belül `test`.

### <a name="specify-the-number-of-concurrent-operations-to-start"></a>Egyidejű műveletek indításához számának megadása

A beállítás `/NC` egyidejű másolás műveletek számát adja meg. Alapértelmezés szerint a AzCopy elindítja a megadott számú egyidejű műveletek az átadás fájlmegosztásra növeléséhez. Táblázat műveletekhez egyidejű műveletek száma megegyezik van processzorok számának. Blob és a fájl műveletek párhuzamos műveletek száma nem egyenlő van processzorok számának 8 időpontokat. Ha AzCopy futtatja a kis sávszélességű hálózaton keresztül, megadhatja az erőforrás versenyhelyzetből okozta hiba elkerülése érdekében /NC egy kisebb értéket.

### <a name="run-azcopy-against-azure-storage-emulator"></a>Azure tároló irányító AzCopy futtatása

AzCopy futtatását is lehetővé teszi az [Azure tároló irányító](storage-use-emulator.md) BLOB ellen:

    AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S

és táblázatokhoz:

    AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table

## <a name="azcopy-parameters"></a>AzCopy paraméterei

Paraméterek AzCopy az alábbiakban ismertetjük. Is beírhatja a parancssorból segítséget az alábbi parancsok egyikére AzCopy használata:

- AzCopy részletes parancssori segítséget:`AzCopy /?`
- Bármely AzCopy paraméter részletes segítséget:`AzCopy /?:SourceKey`
- Parancssori példákat:`AzCopy /?:Samples`

### <a name="sourcesource"></a>/ Forrás: "forrás"

Adja meg a forrásadatok, amelyből másolni szeretne. A forrás lehet egy fájl rendszerkönyvtárán, blob-tároló, blob virtuális könyvtár, tároló fájlmegosztás, tároló fájl könyvtár, vagy az Azure-táblából.

**Alkalmazandó:** Táblázatok BLOB, a fájlok

### <a name="destdestination"></a>/ Cél: "cél"

Adja meg a cél szeretné másolni. A cél lehet egy fájl rendszerkönyvtárán, blob-tároló, blob virtuális könyvtár, tároló fájlmegosztásról, tároló fájl könyvtár, vagy az Azure-táblából.

**Alkalmazandó:** Táblázatok BLOB, a fájlok

### <a name="patternfile-pattern"></a>/ Mintázat: "fájlt, mintát"

Adja meg a fájl mintát, amely jelzi, amely a másolni kívánt fájlokat. A vezérlő viselkedése /Pattern paraméter forrásadatainak helyét, és a rekurzív üzemmód jelenlétét határozza meg. Rekurzív mód keresztül /s beállítás van megadva.

Ha a megadott forrás könyvtár a fájlrendszerben található, majd szabványos helyettesítő karakterek vannak érvényben, és a fájl mintában szereplő megegyezik a címtáron belül fájlok szemben. Ha a beállítás /S van megadva, majd AzCopy is megfelelő szemben a címtárban bármely almappákat lévő összes fájl a megadott minta.

Ha a megadott forrás blob-tárolóhoz vagy a virtuális könyvtár, majd a helyettesítő karakterek nem veszi. Ha a beállítás /S van megadva, majd AzCopy a megadott fájl minta értelmezi blob előtagot. Ha beállítás /S nincs megadva, majd AzCopy felel meg a fájl minta pontos blob-neveket.

Ha a megadott forrás egy Azure fájlmegosztás, akkor kell vagy adja meg pontosan a fájl nevét, (pl. abc.txt) másolása egy fájlt, vagy adja meg a beállítás /S a megosztás rekurzív lévő összes fájl másolni szeretné. Adja meg a fájl minta és a beállítás próbál /S közös eredményezi hibát jelez.

AzCopy használja a kis-és nagybetűket egyező, amikor a forrás blob-tárolóhoz vagy blob virtuális könyvtár, és használja a nagybetűk egyező minden egyéb esetben.

Az alapértelmezett fájlt használja, ha nincs fájl minta meg van adva a mintája *.* a fájlrendszerben vagy egy üres előtag az Azure tárolási helye a. Több fájl mintázatot tartalmazó nem támogatott.

**Alkalmazandó:** BLOB, fájlok

### <a name="destkeystorage-key"></a>/ DestKey: "tárterület-kulcs"

Itt adhatja meg a célként megadott erőforrás a tárhely fiókkulcs.

**Alkalmazandó:** Táblázatok BLOB, a fájlok

### <a name="destsassas-token"></a>/ DestSAS: "társítások-jogkivonat"

Adja meg a megosztott Access aláírás (Társítások) a cél OLVASÁSI és írási engedélyekkel (ha van). A kettős idézőjelek közé, a Társítások foglalni, szerint parancssori különleges karaktereket is tartalmaz.

Ha a cél erőforrás egy blob-tárolóhoz, a fájlmegosztás vagy a táblázatot, vagy adja meg ezt a lehetőséget a biztonsági jogkivonat követ, vagy a Társítások adhatja meg a cél blob-tárolóhoz, fájlmegosztáson vagy anélkül, hogy ez a beállítás a táblázat URI részeként.

Ha a forrás- és mindkét BLOB, majd a cél blob ugyanazt a tárterület-fiókot, a forrás blob belül kell lennie.

**Alkalmazandó:** Táblázatok BLOB, a fájlok

### <a name="sourcekeystorage-key"></a>/ SourceKey: "tárterület-kulcs"

Adja meg a tárhely fiókkulcs az adatforrás erőforrás.

**Alkalmazandó:** Táblázatok BLOB, a fájlok

### <a name="sourcesassas-token"></a>/ SourceSAS: "társítások-jogkivonat"

Adja meg a megosztott Access aláírás az Olvasás és a lista engedélyeit a forrás (ha van). A kettős idézőjelek közé, a Társítások foglalni, szerint parancssori különleges karaktereket is tartalmaz.

Ha az adatforrás erőforrás blob-tároló, és egy kulcsot, sem a Társítások megadva, majd a blob-tárolóhoz olvasható lesz a névtelen hozzáférés keresztül.

Ha a forrás a fájlmegosztás vagy a táblát, meg kell adni egy kulcsot vagy egy Társítások.

**Alkalmazandó:** Táblázatok BLOB, a fájlok

### <a name="s"></a>/S

Másolás műveletekre rekurzív mód megadása. Rekurzív módban AzCopy BLOB vagy a fájlokat, amelyek megfelelnek a megadott fájl mintát, beleértve azokat a mappákat is másolja.

**Alkalmazandó:** BLOB, fájlok

### <a name="blobtypeblock--page--append"></a>/ BlobType: "blokk" |} "lap" |} "hozzáfűzés"

Megadja, hogy a cél blob blokk blob, oldal blob vagy egy hozzáfűző blob-e. Ezt a beállítást csak akkor blob feltöltése esetén alkalmazható. Egyéb esetben a program hibát jelez. Ha a cél blob a használatát pedig ez a beállítás nincs megadva, alapértelmezés szerint a AzCopy blokk blob hoz létre.

**Alkalmazandó:** BLOB

### <a name="checkmd5"></a>/ CheckMD5

Egy letöltött adatok MD5 kivonat kiszámítja, és ellenőrzi, hogy a MD5 kivonat az blob tárolt vagy fájl tartalom-MD5 tulajdonság megegyezik a számított kivonat. A MD5 jelölőnégyzet ki van kapcsolva alapértelmezés szerint, ezt a lehetőséget, ha adatainak letöltése MD5 ellenőriznie kell megadnia.

Figyelje meg, hogy Azure tároló nem garantálja az MD5 kivonat blob-és fájlokban tárolt naprakész. Feladata ügyfél MD5 frissítése, amikor a blob vagy fájl módosítása.

AzCopy mindig után feltölteni a szolgáltatás állítja be a tartalom-MD5 tulajdonság Azure blob vagy fájlt.  

**Alkalmazandó:** BLOB, fájlok

### <a name="snapshot"></a>/ Pillanatfelvétel

Azt jelzi, hogy pillanatképek át. Ezt a beállítást csak akkor érvényes, ha a forrás blob.

Az átvitt blob pillanatképet átnevezi a következő formátumban: .extension blob-name (pillanatkép-idő)

Alapértelmezés szerint pillanatképek nem kerülnek.

**Alkalmazandó:** BLOB

### <a name="vverbose-log-file"></a>/ V: [részletes-naplófájl]

Kimeneti értékeket részletes állapotüzenetek fájlba.

Alapértelmezés szerint a részletes naplófájl neve a AzCopyVerbose.log `%LocalAppData%\Microsoft\Azure\AzCopy`. Ha ezt a lehetőséget egy meglévő fájl helyét adja meg, a részletes naplózás hozzáfűzi a fájlhoz.  

**Alkalmazandó:** Táblázatok BLOB, a fájlok

### <a name="zjournal-file-folder"></a>/ Z: [lapjának-fájl-mappa]

Adja meg a művelet folytatásához lapjának mappába.

AzCopy mindig támogatja a Folytatás, ha egy művelet meg lett szakítva.

Ha ez a beállítás nincs megadva, vagy egy mappa elérési útja nélkül van megadva, majd AzCopy hozza létre az alapértelmezett helyre, amely % LocalAppData%\Microsoft\Azure\AzCopy a napló fájlt.

Minden alkalommal, amikor egy parancs elküldésekor AzCopy, hogy az e lapjának fájl megtalálható-e az alapértelmezett mappát, vagy hogy létezik, amely a megadott beállítás keresztül mappában ellenőrzi. Ha a Jegyzetfüzet-fájlban nem létezik vagy helyen, AzCopy új tekinti a művelet, és létrehoz egy új lapjának fájlt.

A naplófájl létezik, ha AzCopy ellenőrzi, hogy bevitt parancssori megegyezik-e a parancssorból a napló fájlban. Ha felel meg a két parancssorokat, AzCopy folytatja a hiányos működését. Ha nincs egyeznek, a rendszer kéri, a naplófájl új művelet indítása, illetve az aktuális művelet visszavonása vagy felülírásához.

A Jegyzetfüzet-fájlban a művelet sikeres befejezését követően a program törli.

Figyelje meg, hogy a folytatása AzCopy korábbi verziójának által létrehozott lapjának fájlból művelet nem támogatott.

**Alkalmazandó:** Táblázatok BLOB, a fájlok

### <a name="parameter-file"></a>/@:"parameter-file"

Adja meg a paraméterek tartalmazó fájlt. AzCopy dolgozza fel a paraméterek a fájlt, mintha a parancssorban megadott lett.

Egy válasz fájlban is több paramétert egy sorban, vagy adja meg az egyes paraméterek saját külön sorába. Figyelje meg, hogy az egyes paraméterek nem lehet többsoros.

Válasz fájlokat is elhelyezhet a # jel kezdődő megjegyzések sorokat.

Megadhatja, hogy több válasz fájlokat. Megjegyzendő, hogy AzCopy nem támogatja a beágyazott válasz fájlokat.

**Alkalmazandó:** Táblázatok BLOB, a fájlok

### <a name="y"></a>/ Y

Letiltja az összes AzCopy megerősítési képernyőn megjelenő utasításokat.

**Alkalmazandó:** Táblázatok BLOB, a fájlok

### <a name="l"></a>L

Adja meg a nevére a partnerlistában a művelet csak; a program nem az adatok.

AzCopy értelmezi a használja ezt a beállítást a szimulációk futtatásához a parancssorból anélkül, hogy ez a beállítás az l és megszámolni, hogy hány objektumok kerülnek, megadhatja, hogy mely objektumai ellenőrizni egy időben /V kerülnek a a részletes naplózás beállítást.

Ezt a lehetőséget a működését is határozza meg a forrásadatok helyét, és a rekurzív beállítás /S és a fájl mintát üzemmód /Pattern jelenlétét.

AzCopy forrás hely LISTÁBAN és OLVASÁSI engedéllyel kell rendelkeznie, ez a beállítás használata esetén.

**Alkalmazandó:** BLOB, fájlok

### <a name="mt"></a>/MT

A letöltött fájl legutóbbi módosítás időpontja azonosnak kell lennie a forrás blob vagy fájl akkor állítja be.

**Alkalmazandó:** BLOB, fájlok

### <a name="xn"></a>/XN

Kizárja az adatforrás újabb erőforrás. Az erőforrás nem lesznek másolva, ha a legutóbbi módosítás idejét a forrás azonos vagy újabb, mint a cél.

**Alkalmazandó:** BLOB, fájlok

### <a name="xo"></a>/XO

Nem tartalmazza az egy régebbi forrás erőforrás. Az erőforrás nem lesznek másolva, ha a legutóbbi módosítás idejét a forrás azonos vagy régebbi, mint a cél.

**Alkalmazandó:** BLOB, fájlok

### <a name="a"></a>/A

Csak az Archiválandó fájlok feltölti.

**Alkalmazandó:** BLOB, fájlok

### <a name="iarashcnetoi"></a>/ IA: [RASHCNETOI]

Csak a megadott attribútumok beállítás közül bármelyiket fájlok feltölti.

Rendelkezésre álló attribútumok közé tartozik:

- R = csak olvasható fájlok
- A = archiválásra kész
- S = Rendszerfájl
- H = a rejtett fájlok
- C = tömörített fájlok
- N = normál fájlok
- E = a titkosított fájlok
- Kétmintás T = az ideiglenes fájlok
- O = Offline fájlok
- E = a fájlok nem indexelt

**Alkalmazandó:** BLOB, fájlok

### <a name="xarashcnetoi"></a>/ XA: [RASHCNETOI]

Kizárja a fájlokat, amelyek a megadott attribútumok beállítás közül.

Rendelkezésre álló attribútumok közé tartozik:

- R = csak olvasható fájlok
- A = archiválásra kész
- S = Rendszerfájl
- H = a rejtett fájlok
- C = tömörített fájlok
- N = normál fájlok
- E = a titkosított fájlok
- Kétmintás T = az ideiglenes fájlok
- O = Offline fájlok
- E = a fájlok nem indexelt

**Alkalmazandó:** BLOB, fájlok

### <a name="delimiterdelimiter"></a>/ Elválasztó: "Elválasztó"

Azt jelzi, hogy a virtuális könyvtárak blob nevét a határoló használt elválasztó karakter.

Alapértelmezés szerint AzCopy használ, és az elválasztó karaktert. Viszont AzCopy támogatja a bármely közös használata (például: @, # vagy %) tagolni. Ha szeretne a parancssorból a következő speciális karakterek valamelyikét tartalmazza, foglalja a fájl neve dupla idézőjelek.

Ez a beállítás csak akkor alkalmazható, töltse le a BLOB.

**Alkalmazandó:** BLOB

### <a name="ncnumber-of-concurrent-operations"></a>/ NC: "szám – a-egyidejű-műveletek"

Műveletek párhuzamos számát adja meg.

Alapértelmezés szerint AzCopy megadott számú egyidejű műveletek az átadás fájlmegosztásra növeléséhez kezdődik. Ne feledje, hogy sok kis sávszélességű környezetben egyidejű műveletek elborít a hálózati kapcsolat, és megakadályozhatja, hogy a műveletek teljesen befejezését. A tényleges rendelkezésre álló hálózati sávszélesség alapján egyidejű műveletek szabályozása.

Műveletek párhuzamos felső határa 512.

**Alkalmazandó:** Táblázatok BLOB, a fájlok

### <a name="sourcetypeblob--table"></a>/ Forrástípus: "Blob" |} "Táblázat"

Meghatározza, hogy a `source` erőforrás elérhető a helyi fejlesztői környezet, a tárhely irányító futó blob.

**Alkalmazandó:** BLOB, táblák

### <a name="desttypeblob--table"></a>/ DestType: "Blob" |} "Táblázat"

Meghatározza, hogy a `destination` erőforrás elérhető a helyi fejlesztői környezet, a tárhely irányító futó blob.

**Alkalmazandó:** BLOB, táblák

### <a name="pkrskey1key2key3"></a>/ PKRS: "azonosító1 # azonosító2 key3 #..."

A partíciók fő tartományt ahhoz, hogy az exportálási művelet sebességének növeli párhuzamosan táblázat-adatok exportálása kettéválaszthatja-e.

Ha ez a beállítás nincs megadva, majd AzCopy használ egy szál az exportálandó táblázat szervezetek. Ha például a felhasználó /PKRS ad meg: "aa #bb", majd a AzCopy elindul három egyidejű műveletek.

Az egyes műveletek exportálása egy három partíciót kulcs tartományok, alább látható módon:

  [első partíciót-kulcs, ε)

  [aa, bb)

  [bb utolsó billentyűs partíciót]

**Alkalmazandó:** Táblák

### <a name="splitsizefile-size"></a>/ SplitSize: "fájlméret"

Itt adhatja meg az exportált fájl méretét megabájtban a megengedett minimális érték felosztása 32.

Ha ez a beállítás nincs megadva, az AzCopy exportálja Táblázatadatok egy fájlból.

Ha blob exportálja a táblázat adatait, és az exportált fájl méretét eléri a 200 GB korlátját blob méretét, majd a AzCopy az az exportált fájlt felosztás, még akkor is, ha ez a beállítás nincs megadva.

**Alkalmazandó:** Táblák

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a>/ EntityOperation: "InsertOrSkip" |} "InsertOrMerge" |} "InsertOrReplace"

Itt adhatja meg az adatok importálása táblázatviselkedési.

- InsertOrSkip – egy meglévő személy átugrása, vagy szúrja be egy új entitás, ha nem létezik a táblázatban.

- InsertOrMerge - egyesíti a meglévő entitás, vagy szúrja be egy új entitás, ha nem létezik a táblázatban.

- InsertOrReplace - lecseréli a meglévő entitás, vagy szúrja be egy új entitás, ha nem létezik a táblázatban.

**Alkalmazandó:** Táblák

### <a name="manifestmanifest-file"></a>/ Nyilvánvalóan: "jegyzék-fájl"

Adja meg a nyilvánvalóan fájlt, a táblázat exportálása és importálása művelet.

Ez a beállítás nem kötelező, az exportálás során, AzCopy nyilvánvalóan fájl az előre definiált név hoz létre, ha ez a beállítás nincs megadva.

Ezt a lehetőséget történjenek az importálás során, az adatfájlok megkeresése az szükség.

**Alkalmazandó:** Táblák

### <a name="synccopy"></a>/ SyncCopy

Azt jelzi, hogy szinkron másolása BLOB vagy a fájlok közötti végpontokat Azure tároló.

Alapértelmezés szerint AzCopy kiszolgálóoldali aszinkron másolás használja. Adja meg a szinkronizált másolat, mely BLOB vagy a fájlok helyi memória letölti és majd feltölti őket Azure-tárolóhoz végezze el ezt a lehetőséget.

Használhatja ezt a beállítást, fájlok belül Blob-tárolóhoz fájltároló, akár a fájltárolás és fordítva Blob-tároló másolásakor.

**Alkalmazandó:** BLOB, fájlok

### <a name="setcontenttypecontent-type"></a>/ SetContentType: "tartalomtípus-"

Adja meg a MIME tartalom rendeltetési BLOB vagy fájlok.

AzCopy alkalmazás/nyolcas-adatfolyam alapértelmezés szerint a webhelytartalom-típus blob vagy fájl állítja. Beállíthatja, hogy a tartalomtípust az összes BLOB vagy fájlok kifejezetten megadásával beállítás értékét.

Ha megad egy érték nélküli ezt a lehetőséget, majd AzCopy állítja minden blob vagy fájl webhelytartalom-típus szerint a fájl kiterjesztését.

**Alkalmazandó:** BLOB, fájlok

### <a name="payloadformatjson--csv"></a>/ PayloadFormat: "JSON" |} "CSV"

Itt adhatja meg a táblázat exportált adatokat fájl formátuma.

Ha ez a beállítás nincs megadva, alapértelmezés szerint AzCopy exportálja táblázat adatfájl JSON formátumban.

**Alkalmazandó:** Táblák

## <a name="known-issues-and-best-practices"></a>Ismert problémák és a gyakorlati tanácsok

### <a name="limit-concurrent-writes-while-copying-data"></a>Korlát egyidejű írás közben adatok másolása

BLOB- vagy AzCopy fájlok másolásakor ne feledje, hogy egy másik alkalmazásban módosítja az adatok másolása, miközben. Ha lehetséges győződjön meg arról, hogy a másolt adatok nem megváltoztatta a másolási művelet során. Például az Azure virtuális gép társított egy virtuális másolásakor győződjön meg arról, hogy nincs más alkalmazások jelenleg az éppen írt a virtuális. A művelet legegyszerűbben úgy, hogy az erőforrás másolandó bérleti. Ehelyett a virtuális pillanatképét először létrehozhat és másoljon a pillanatkép.

Ha nem tudja megakadályozhatja, hogy más alkalmazások BLOB vagy fájlok írás közben is másolt, majd tartsa szem előtt, hogy a feladat befejezése időpontonként a másolt erőforrások előfordulhat, hogy nem kell többé teljes eltérés áll fenn, az adatforrás erőforrásokkal.

### <a name="run-one-azcopy-instance-on-one-machine"></a>Futtassa a egy AzCopy példány egy gépen.
AzCopy az célja, hogy a gép erőforrás, hogy felgyorsítsa az adatátvitel hasznosítása maximalizálása, azt javasoljuk, hogy csak egy AzCopy példány futtatja egy számítógépen, és adja meg a beállítás `/NC` Ha több egyidejű műveletekre van szüksége. További részletekért írja be a `AzCopy /?:NC` a parancssorból.

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a>FIPS szabványnak megfelelő MD5 algoritmusok engedélyezése AzCopy amikor, "használata FIPS megfelelő algoritmusok titkosításhoz, a kivonatolás és aláíráshoz".
Alapértelmezés szerint AzCopy .NET MD5 végrehajtása használja a számítja ki a MD5, amikor az objektumok másolása, de néhány biztonsági követelményeknek FIPS kompatibilis MD5 beállítás engedélyezéséhez AzCopy van szükség.

Létrehozhat egy app.config fájl `AzCopy.exe.config` tulajdonság `AzureStorageUseV1MD5` és a AzCopy.exe függetlenül.

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

Az IGAZ - tulajdonság "AzureStorageUseV1MD5" • az alapértelmezett érték, AzCopy .NET MD5 végrehajtása fogja használni.
• Hamis – AzCopy FIPS kompatibilis MD5 algoritmus fogja használni.

Figyelje meg, hogy FIPS szabványnak megfelelő algoritmusok le van tiltva a Windows számítógépen alapértelmezés szerint, a Futtatás ablakba parancsra, és jelölje be a biztonsági beállítás a kapcsolót -> helyi csoportházirend-Biztonság > rendszer titkosítási-beállítások >: használata FIPS megfelelő algoritmusok titkosításhoz, a kivonatolás és aláíráshoz.

## <a name="next-steps"></a>Következő lépések

Azure-tárhely és a AzCopy kapcsolatos további tudnivalókért tekintse át az alábbi forrásokban.

### <a name="azure-storage-documentation"></a>Azure tároló dokumentáció:

- [Azure tároló – bevezetés](storage-introduction.md)
- [A .NET Blob-tárolóhoz használata](storage-dotnet-how-to-use-blobs.md)
- [A .NET fájltároló használata](storage-dotnet-how-to-use-files.md)
- [A .NET táblatároló használata](storage-dotnet-how-to-use-tables.md)
- [Hogyan hozhat létre, kezelése és tároló fiók törlése](storage-create-storage-account.md)

### <a name="azure-storage-blog-posts"></a>Azure tároló blogbejegyzések:
- [Az Azure tároló adatok mozgását tár Preview bemutatása](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
- [AzCopy: Szinkron másolás és a testre szabott tartalomtípus bemutatása](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
- [AzCopy: Általános elérhetőségét a AzCopy 3.0 plusz előnézeti release AzCopy 4.0-s a tábla- és támogatási kihirdetése](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
- [AzCopy: Nagyméretű másolás esetek optimalizálva](http://go.microsoft.com/fwlink/?LinkId=507682)
- [AzCopy: Olvasási-hozzáférés geo felesleges tároló támogatása](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
- [AzCopy: Újra másik mód és a biztonsági jogkivonat adatátviteli](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
- [AzCopy: Keresztté-fiók másolás Blob használatával](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
- [AzCopy: Feltöltése vagy fájlok letöltése Azure BLOB-](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)

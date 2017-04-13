<properties
   pageTitle="Első lépések az Azure köteg CLI |} Microsoft Azure"
   description="Rövid ismertetés a Köteg paranccsal az Azure CLI az Azure köteg szolgáltatás erőforrások kezelése"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="09/30/2016"
   ms.author="marsma"/>

# <a name="get-started-with-azure-batch-cli"></a>Azure köteg CLI – első lépések

A platformok Azure parancssori kezelőfelületről Azure lehetővé teszi, hogy a köteg fiókok és erőforrások, például készletek, feladatok és Linux, Mac és Windows parancs ismertetése a tevékenységek kezelése. Az Azure CLI végrehajtani, és parancsfájl-sok feladatot azonos a köteg API-hoz, Azure portál és köteg PowerShell-parancsmagok hajt végre.

Ez a cikk Azure CLI verzió 0.10.5 alapul.

## <a name="prerequisites"></a>Előfeltételek

* [Telepítse az Azure CLI](../xplat-cli-install.md)

* [Az Azure CLI csatlakoztatása az Azure előfizetés](../xplat-cli-connect.md)

* **Erőforrás-kezelő módban**váltás:`azure config mode arm`

>[AZURE.TIP] Azt javasoljuk, hogy Ön-telepítés frissítése az Azure CLI gyakran kell szolgáltatásfrissítések és bővítmények előnyeit.

## <a name="command-help"></a>Súgó gomb

Megjelenítéséhez súgószöveg a minden parancs az Azure CLI a Hozzáfűzés `-h` után a parancs csak webhelybeállításként. Példa:

* A segítség a `azure` parancsot, írja be:`azure -h`
* Az összes köteg parancsok listája az a CLI, használja:`azure batch -h`
* Segítség a köteg fiók létrehozása, írja be:`azure batch account create -h`

Ha kétségei vannak, használhatja a `-h` parancssori kapcsoló használatával, kérjen segítséget a bármely Azure CLI parancsot.

## <a name="create-a-batch-account"></a>Hozzon létre egy köteg fiókot

Szintaxis:

    azure batch account create [options] <name>

Példa:

    azure batch account create --location "West US"  --resource-group "resgroup001" "batchaccount001"

Létrehoz egy új köteget fiókot a megadott paramétereknek. Meg kell adnia a legalább egy helyen, a erőforráscsoport és a fiók nevére. Ha még nincs erőforráscsoport, hozzon létre egyet, futtatásával `azure group create`, és adja meg az Azure régiók (például "US nyugati") közül a `--location` lehetőséget. Példa:

    azure group create --name "resgroup001" --location "West US"

> [AZURE.NOTE] A köteg fióknév az Azure, létrejön a fiók terület belül egyedinek kell lennie. Csak a kisbetűsre alfanumerikus karaktereket tartalmazhat, és a 3-24 karakter hosszúságú lehet. Különleges karakterek, például nem használhat `-` vagy `_` a köteg fióknevét.

### <a name="linked-storage-account-autostorage"></a>Csatolt tároló fiók (autostorage)

(Nem kötelező) mutató hivatkozás létrehozásához egy **Általános célú** tárolás fiókot a köteg fiók létrehozásakor. A köteg [alkalmazáscsomagok](batch-application-packages.md) szolgáltatását használja blob-tárolóhoz egy csatolt általános célú tárterület-fiókkal, a a [Köteg fájl szabályok .NET](batch-task-output.md) tárat. Nem kötelező funkciókról segítséget nyújtanak az alkalmazások futtatásához a köteg feladatokat üzembe helyezése, és az adatokat azok konzerv pályától tartós.

Meglévő Azure tárolás ügyfél csatolása az új köteget fiók létrehozásakor, adja meg a `--autostorage-account-id` lehetőséget. Ez a beállítás a teljesen minősített erőforrás-azonosító tárolás fiók szükséges.

Első lépésként részletek tárolás fiókjához:

    azure storage account show --resource-group "resgroup001" "storageaccount001"

Ezután írja be az **URL-címe** értéket az a `--autostorage-account-id` lehetőséget. Az URL-címe értéket "/ előfizetések /" kezdődik, és az előfizetés Azonosítóját és az erőforrás elérési útja látható a tárterület-fiók:

    azure batch account create --location "West US"  --resource-group "resgroup001" --autostorage-account-id "/subscriptions/8ffffff8-4444-4444-bfbf-8ffffff84444/resourceGroups/resgroup001/providers/Microsoft.Storage/storageAccounts/storageaccount001" "batchaccount001"

## <a name="delete-a-batch-account"></a>A köteg fiók törlése

Szintaxis:

    azure batch account delete [options] <name>

Példa:

    azure batch account delete --resource-group "resgroup001" "batchaccount001"

A megadott köteg fiók törlése Amikor a rendszer kéri, erősítse meg a fiók eltávolítása (fiók eltávolítása eltarthat egy kis időt befejezéséhez).

## <a name="manage-account-access-keys"></a>Hívóbetűk fiók kezelése

Hívóbetű hozhat [létre és módosíthat erőforrások](#create-and-modify-batch-resources) van szüksége a köteg-fiókjában.

### <a name="list-access-keys"></a>Hívóbetűk lista

Szintaxis:

    azure batch account keys list [options] <name>

Példa:

    azure batch account keys list --resource-group "resgroup001" "batchaccount001"

A fiókot a megadott köteg fiók használható billentyűparancsok listája.

### <a name="generate-a-new-access-key"></a>Az access új kulcs generálása

Szintaxis:

    azure batch account keys renew [options] --<primary|secondary> <name>

Példa:

    azure batch account keys renew --resource-group "resgroup001" --primary "batchaccount001"

A megadott fiók billentyűt az adott köteg fiók újragenerálása.

## <a name="create-and-modify-batch-resources"></a>Létrehozása és módosítása a köteg erőforrások

Az Azure CLI segítségével létrehozás, Olvasás, frissítés, és törlése (CRUD) köteg erőforrások, mint a készletek, csomópontok, feladatok és feladatok számítja ki. CRUD ezeket a műveleteket csak a köteg fióknév hívóbetű és végpontot. Ezen a megadhatja a `-a`, `-k`, és `-u` beállítások vagy a CLI használó automatikusan (ha van töltve) beállítása [a környezeti változók](#credential-environment-variables) .

### <a name="credential-environment-variables"></a>Hitelesítő adatok környezeti változók

Beállíthatja, hogy `AZURE_BATCH_ACCOUNT`, `AZURE_BATCH_ACCESS_KEY`, és `AZURE_BATCH_ENDPOINT` környezeti változók helyett megadása `-a`, `-k`, és `-u` beállítások parancsára a parancssorból a minden parancs végrehajtása. A köteg CLI használja-e változók (Ha a beállítása), hogy kihagyhatja a `-a`, `-k`, és `-u` beállítások. Ez a cikk hátralévő feltételezi, hogy ezek a környezeti változók használata.

>[AZURE.TIP] A billentyűparancsok a lista `azure batch account keys list`, és megjeleníti a fiók végpont `azure batch account show`.

### <a name="json-files"></a>JSON-fájlok

Köteg erőforrások, mint készletek és a feladatok létrehozásakor megadhatja az új erőforrás konfigurációs helyett áthaladó paramétereken szerint parancssori kapcsolói tartalmazó JSON fájlt. Példa:

`azure batch pool create my_batch_pool.json`

Csak parancssori kapcsolók használata sok erőforrást létrehozási műveleteket végezheti el, miközben bizonyos szolgáltatásokhoz erőforrás részleteit tartalmazó JSON-formátumú fájl. Például JSON fájl kell használnia, ha meg szeretné adni a tevékenység kezdési erőforrásfájl.

Keresse meg a szükséges hozzon létre egy erőforrást JSON, olvassa el a [köteget REST API-hivatkozás] [ rest_api] MSDN dokumentációt. Minden egyes *"Erőforrástípus*hozzáadása" témakör példa JSON hozhat létre az erőforrás, amelynek JSON fájlok, a webhelysablonok használatával. Például JSON készlet létrehozása a program [a fiók-készlet hozzáadása]a[rest_add_pool].

>[AZURE.NOTE] Ha JSON fájl ad meg, amikor létrehoz egy erőforrás, minden más paraméter adja meg, ha a parancssorban az adott erőforrás figyelmen kívül hagyja.

## <a name="create-a-pool"></a>Készlet létrehozása

Szintaxis:

    azure batch pool create [options] [json-file]

Példa (virtuális gép konfigurációja):

    azure batch pool create --id "pool001" --target-dedicated 1 --vm-size "STANDARD_A1" --image-publisher "Canonical" --image-offer "UbuntuServer" --image-sku "14.04.2-LTS" --node-agent-id "batch.node.ubuntu 14.04"

Példa (Cloud Services konfigurálása):

    azure batch pool create --id "pool002" --target-dedicated 1 --vm-size "small" --os-family "4"

A köteg szolgáltatásban hoz létre a számítási csomópontok erőforráskészlethez tartozik.

A [Köteg szolgáltatás áttekintése](batch-api-basics.md#pool)említett, két lehetőség közül választhat a készletet az operációs rendszer a csomópontok kijelölésekor: **Virtuális gép beállításait** , illetve **Cloud Services beállításait**. Használja a `--image-*` beállítások hozhat létre virtuális gép konfigurációs készletek, és `--os-family` Cloud Services konfigurációs készletek létrehozása. Nem adhat meg mindkét `--os-family` és `--image-*` beállítások.

Megadhatja a készlet [alkalmazáscsomagok](batch-application-packages.md) és a parancssorból a [feladat indítása](batch-api-basics.md#start-task). Szeretne megadni a kezdő tevékenység erőforrásfájl, azonban inkább kell használnia a [JSON-fájlt](#json-files).

Törölje a erőforráskészlethez tartozik:

    azure batch pool delete [pool-id]

>[AZURE.TIP] A megfelelő értékek, jelölje be a [virtuális gép képek listája](batch-linux-nodes.md#list-of-virtual-machine-images) a `--image-*` beállítások.

## <a name="create-a-job"></a>Feladat létrehozása

Szintaxis:

    azure batch job create [options] [json-file]

Példa:

    azure batch job create --id "job001" --pool-id "pool001"

A köteg fiók egy feladatot ad, és adja meg a készlet feladatainak hajtsa végre.

A feladat törlése:

    azure batch job delete [job-id]

## <a name="list-pools-jobs-tasks-and-other-resources"></a>Lista-készletek, feladatok, feladatok és más erőforrások:

Minden köteg erőforrástípus támogatja a `list` lekérdezése a köteg fiók, és megjeleníti az adott típusú erőforrások parancsot. Például a készletek jeleníthet meg a fiókja és a projekt a tevékenységek:

    azure batch pool list
    azure batch task list --job-id "job001"

### <a name="listing-resources-efficiently"></a>Erőforrások hatékony listázása

Gyorsabb lekérdezése megadhatja **Jelölje ki**, **szűrheti**és **bontsa ki a** záradék beállításainak `list` műveletek. Ezek a beállítások segítségével a köteg szolgáltatás által visszaadott adatok mennyiségét korlátozni. Kiszolgálóoldali szűrés összes fordul elő, mert az csak azokat az adatokat, érdeklik a vezetékes metszéspontja. Használja a következő záradékok sávszélesség mentése (és ezért időpontot, amikor) Ha a lista műveleteket hajt végre.

Például ez csak amelynek azonosítók Kiindulás "renderTask" készletek ad eredményül:

    azure batch task list --job-id "job001" --filter-clause "startswith(id, 'renderTask')"

A köteg CLI a köteg szolgáltatás által támogatott összes három záradéka támogatja:

* `--select-clause [select-clause]`Vissza a minden személyhez a tulajdonságok egy részét
* `--filter-clause [filter-clause]`Csak azok a személyek, amelyek megfelelnek a megadott OData kifejezés visszatérési
* `--expand-clause [expand-clause]`Szerezze be a telefonál egyetlen mögöttes többi személy adatai. A kibontás záradék csak támogatja a `stats` tulajdonság adott időben.

A három záradéka és a velük teljesítményű lista lekérdezések részletekért olvassa el [a köteg Azure szolgáltatás hatékony lekérdezés](batch-efficient-list-queries.md).

## <a name="application-package-management"></a>Alkalmazáskezelés csomag

A számítási csomópontok a készletek alkalmazások terjesztése egyszerűsített alkalmazáscsomagok kínál. Az Azure CLI az alkalmazáscsomagok feltöltése, csomag verziók kezelése és törlése a csomagok.

Hozzon létre egy új alkalmazást, és egy csomag verziójának hozzáadásához:

**Létrehozás** kérelmet:

    azure batch application create "resgroup001" "batchaccount001" "MyTaskApplication"

**Hozzáadás** az alkalmazás-csomagok:

    azure batch application package create "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" package001.zip

**Aktiválja** a csomagot:

    azure batch application package activate "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" zip

Az **alapértelmezett verzióra** az alkalmazás beállítása:

    azure batch application set "resgroup001" "batchaccount001" "MyTaskApplication" --default-version "1.10-beta3"

### <a name="deploy-an-application-package"></a>Az alkalmazás-csomagok telepítése

Megadhatja a telepítéshez alkalmazáscsomagok, amikor új készlet létrehozása. Készlet létrehozása időben megadása egy csomag esetén telepítené minden csomópontot, a csomópont illesztések készletre. Csomagok is van telepítve, ha egy csomópont indítani, vagy reimaged.

Adja meg a `--app-package-ref` erőforráskészlethez tartozik egy alkalmazáscsomag a készlet csomópontok szeretne telepíteni, ahogy azok a csatlakozás a készlet létrehozása lehetőséget. A `--app-package-ref` beállítás fogadja el a telepítéshez használni a számítási csomópontok alkalmazásazonosítók pontosvesszővel tagolt listáját.

    azure batch pool create --pool-id "pool001" --target-dedicated 1 --vm-size "small" --os-family "4" --app-package-ref "MyTaskApplication"

Parancssori kapcsolók a készletet hoz létre, jelenleg nem adhat *telepítéshez használni a számítási csomópontok, például "1.10-beta3" csomag alkalmazás verziószámának* . Emiatt azt először meg kell adnia az alkalmazás az alapértelmezett verzióra `azure batch application set [options] --default-version <version-id>` a készlet létrehozása előtt (lásd az előző szakasztól). Azonban adhatja meg a készlet egy csomag verzió használatakor [JSON fájl](#json-files) parancssori kapcsolói helyett meg a készlet létrehozásakor.

Alkalmazáscsomagok további információt az [alkalmazás környezet, amelyben az Azure köteg alkalmazáscsomagok](batch-application-packages.md)talál.

>[AZURE.IMPORTANT] A köteg fiókjába alkalmazáscsomagok használatához be kell [Azure tárterület-fiókkal hivatkozásra](#linked-storage-account-autostorage) .

### <a name="update-a-pools-application-packages"></a>Egy készlet alkalmazáscsomagok frissítése

Ha frissíteni szeretné az alkalmazások, kijelölt egy meglévő csoportba, hiba a `azure batch pool set` parancsot a a `--app-package-ref` beállítást:

    azure batch pool set --pool-id "pool001" --app-package-ref "MyTaskApplication2"

Az új alkalmazáscsomag számítja ki a már meglévő készletben csomópontokat üzembe helyezéséhez indítsa újra, illetve azokat a csomópontokat reimage:

    azure batch node reboot --pool-id "pool001" --node-id "tvm-3105992504_1-20160930t164509z"

>[AZURE.TIP] A csomópontok egy készletben együtt csomópont azonosítójuk listája szerezhet a `azure batch node list`.

Tartsa szem előtt kell már beállított az alkalmazás egy alapértelmezett verziójával a telepítés előtt (`azure batch application set [options] --default-version <version-id>`).

## <a name="troubleshooting-tips"></a>Hibaelhárítási tippek

Ez a szakasz célja, hogy biztosítson Azure CLI problémáinak használandó erőforrások. Azt nem feltétlenül problémamegoldó összes, de azt segíthet a szűkítéséhez a probléma okát, és mutasson az erőforrások súgó.

* Használat `-h` **Súgószöveg** beszerzése minden CLI parancs

* Használat `-v` és `-vv` **részletes** parancs kimenet; megjelenítéséhez `-vv` "extra" részletes, és a tényleges többi kérések és válaszok jeleníti meg. A kapcsolók olyan hasznos a teljes hiba kimeneti megjelenítéséhez.

* **Parancs generálni JSON** tekinthet meg a `--json` lehetőséget. Ha például `azure batch pool show "pool001" --json` pool001's tulajdonságok JSON formátumban jeleníti meg. Másolhatja és módosíthatja a kimenet használni egy `--json-file` (lásd: Ez a cikk korábbi részében [JSON-fájlok](#json-files) ).

* A [Köteg-fórum az MSDN] [ batch_forum] remek súgó erőforrás van, és a köteg csapattagok szorosan nyomon. Ügyeljen arra, hogy kérdéseiket az ott Ha hiba lép fel, illetve szeretné segítséget egy adott művelet.

* Nem minden köteg erőforrás művelet jelenleg az Azure CLI által támogatott. Például jelenleg nem lehet megadni egy alkalmazás csomag *verzió* készlet, csak a csomag azonosítóját. Ezekben az esetekben előfordulhat, hogy ellátási egy `--json-file` a parancs parancssori kapcsolói használata helyett. Ügyeljen arra, hogy naprakész legyen a CLI legújabb jövőbeli kapcsolatos fejlesztések átviteléhez.

## <a name="next-steps"></a>Következő lépések

*  Lásd: [alkalmazás környezet, amelyben az Azure köteg alkalmazáscsomagok](batch-application-packages.md) megtudhatja, hogy miként kezelheti és helyezhetnek üzembe a köteg számítási csomóponton végrehajtása alkalmazásokat a funkció használatához.

* [A köteg szolgáltatás hatékony lekérdezés](batch-efficient-list-queries.md) további kapcsolatos tudnivalók című részben lévő elemek számának és az információtípust, a függvény által visszaadott lekérdezések köteghez csökkentése.

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
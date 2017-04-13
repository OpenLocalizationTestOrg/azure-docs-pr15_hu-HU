<properties
    pageTitle="Azure kulcs tárolóra naplózás |} Microsoft Azure"
    description="Ebben az oktatóanyagban segítségével Azure kulcs tárolóból elemre az első lépésekhez naplózás."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/31/2016"
    ms.author="cabailey"/>

# <a name="azure-key-vault-logging"></a>Azure kulcs tárolóra naplózás #
Azure kulcs tárolóból elemre a legtöbb régióban érhető el. További tudnivalókért olvassa el a [kulcs tárolóra árak, oldal](https://azure.microsoft.com/pricing/details/key-vault/)című témakört.

## <a name="introduction"></a>– Bevezetés  
Miután létrehozott egy vagy több fő tárolókban, valószínűleg érdemes figyelni hogyan és mikor a fő tárolókban is elérhető, és ki által. Ehhez kulcs tárolóból elemre, amely az Ön által Azure tároló fiók adatokat tárolja a naplózás engedélyezése. **Háttérismeretek – naplók-auditevent** nevű új tároló automatikusan létrejön a megadott tárterület-fiókjához, és több fő tárolókban naplók összegyűjtési használható ez ugyanazzal a tárterület-fiókkal.

Elérheti a naplóadatokat legfeljebb 10 perc után a kulcs tárolóból elemre a művelet. A legtöbb esetben legyen gyorsabban ennél.  Érdemes, hogy a naplók a tárterület-fiókjában kezelése:

- Szabványos Azure hozzáférési vezérlője segítségével a naplók biztonságos korlátozásával ki érheti el őket.
- Törölje a naplókat, amelyet már nem szeretne tartani a tárterület-fiókjában.

Ebben az oktatóanyagban segítségével első lépések a naplózás, hozza létre a tárterület-fiókot, a naplózás engedélyezése és a naplóadatokat gyűjtött értelmezése Azure kulcs tárolóból elemre.  


>[AZURE.NOTE]  Ebben az oktatóanyagban nem része a hozott létre fő tárolókban, billentyűk vagy titkos kulcsok útmutatót. Ezt az információt olvassa el az [első lépések az Azure kulcs tárolóból elemre](key-vault-get-started.md)című témakört. Másik lehetőségként platformok parancssori felület útmutatásért lásd: [Ebben az oktatóanyagban egyenértékű](key-vault-manage-with-cli.md).
>
>Azure kulcs tárolóból elemre jelenleg nem konfigurálja az Azure-portálon. Használja az alábbi Azure Powershellhez útmutatást.

A naplókban összegyűjtött, a műveletek Management programcsomagot a napló analytics használatával megjeleníthetők. További tudnivalókért lásd: a [napló Analytics Azure kulcs tárolóból elemre (előzetes verzió) megoldás](../log-analytics/log-analytics-azure-key-vault.md).

Azure kulcs tárolóra kapcsolatos információ áttekintése [Mi az Azure kulcs tárolóra?](key-vault-whatis.md)

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban befejezéséhez az alábbiakat kell rendelkeznie:

- Egy meglévő kulcs tárolóból elemre, amelyet használ.  
- Azure PowerShell **1.0.1-es verziójú a legkisebb verziószáma**. Azure PowerShell telepítése, és azt társítása Azure-előfizetése, megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md). Ha már telepítette az Azure PowerShell, és nem tudja, hogy a verziót, a Azure PowerShell konzolról, írja be a `(Get-Module azure -ListAvailable).Version`.  
- A kulcs tárolóra naplók Azure a megfelelő tárhelyet.


## <a id="connect"></a>Az előfizetések csatlakoztatása ##

Indítsa el az Azure PowerShell-munkamenetet, és jelentkezzen be az Azure-fiók a következő parancsot:  

    Login-AzureRmAccount

Az előugró böngészőablakához írja be az Azure-fiók felhasználónevét és jelszavát. Azure PowerShell jelenik meg az előfizetések alapértelmezés szerint, és az ehhez a fiókhoz tartozó, használja a legelső.

Ha több előfizetéssel rendelkezik, lehet, ha egy adott megjegyzés létrehozása az Azure kulcs tárolóra használt adja meg. Írja be az alábbi módon lásd: a fiók az előfizetések:

    Get-AzureRmSubscription

Ezután adja meg az előfizetést, a naplózás, fő tárolóra van társítva, írja be:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Azure PowerShell beállításával kapcsolatos további tudnivalókért megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md).


## <a id="storage"></a>A naplók új tárterület-fiók létrehozása ##

Bár a naplók egy meglévő tárterület-fiókot is használhat, azt létre fogja hozni az új tárterület-fiók inkább Naplók kulcs tárolóból elemre. Kényelmesebbé, amikor azt meg kell adnia a később az a részletek fogja tároljuk be **rendszergazdai**nevű változó.

További könnyű kezelés, az is használjuk az azonos erőforráscsoport mint, amely tartalmazza a fő tárolóból elemre. Az [első lépések az oktatóprogram](key-vault-get-started.md)az erőforrás csoport neve **ContosoResourceGroup** és továbbítottuk fogja használni a kelet-ázsiai helyre. Helyettesítő ezeket az értékeket a saját, szükség szerint:

    $sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup -Name ContosoKeyVaultLogs -Type Standard_LRS -Location 'East Asia'


>[AZURE.NOTE]  Ha úgy dönt, hogy egy meglévő tárterület-fiókot használ, meg kell használni az azonos előfizetés a fő tárolóból elemre, és azt a klasszikus telepítési modell helyett az erőforrás-kezelő telepítési modell kell használnia.

## <a id="identify"></a>A fő tárolóból elemre a naplók azonosítása ##

A keresztüli lépések oktatóanyagban a fő tárolóból elemre volt neve **ContosoKeyVault**, így fog továbbítottuk a név és a részletek tárolása **kv**nevű változó be:

    $kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'


## <a id="enable"></a>A naplózás engedélyezése ##

Kulcs tárolóból elemre a naplózás engedélyezéséhez használjuk a Set-AzureRmDiagnosticSetting parancsmag együtt a változók létrehozott saját új tárterület-fiók és a fő tárolóból elemre. Fogja azt is beállíthatja a **-engedélyezett** jelző, amely **$true** és AuditEvent (a billentyű tárolóból elemre a naplózás csak kategóriájára), a kategória beállítása:


    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

A kimenet ahhoz, hogy ez tartalmazza:

    StorageAccountId   : /subscriptions/<subscription-GUID>/resourceGroups/ContosoResourceGroup/providers/Microsoft.Storage/storageAccounts/ContosoKeyVaultLogs
    ServiceBusRuleId   :
    StorageAccountName :
        Logs
        Enabled           : True
        Category          : AuditEvent
        RetentionPolicy
        Enabled : False
        Days    : 0


Ez megerősíti, hogy a naplózás most engedélyezve van a fő tárolóból elemre, az adatok mentése a tárterület-fiókjába.

Másik lehetőségként is beállíthatja, hogy az adatmegőrzési a naplók úgy, hogy a régebbi naplók automatikusan törlődnek. Adatmegőrzési szabály használatával **$true** **- RetentionEnabled** Üzenetjelölő beállítása és a **-RetentionInDays** paraméter értéke **90** , hogy a naplók 90 napnál régebbi rendszer automatikusan törli.

    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent -RetentionEnabled $true -RetentionInDays 90

Mi a rendszer rögzíti:

- Az összes hitelesített REST API-kérés van bejelentkezve, amely magában foglalja a hozzáférési engedélyek, rendszerhibákat vagy hibás kérések eredményeként sikertelen kérelmek.
- A kulcs műveletek vault létrehozása, törlése, beállítást fő tárolóból elemre az access házirendek tartalmazó magát, és a címkék fő tárolóra attribútumok például frissítése.
- Műveletek kulcsok és a fő tárolóból elemre, amely magában foglalja létrehozása, módosítása vagy törlése a következő kulcsok vagy titkos kulcsok; a titkos kulcsok Műveletek, például a bejelentkezési, ellenőrizze, titkosítása, visszafejtése, tördelése és a billentyűk tördelésének, titkos kulcsok, lista kulcsok és titkos kulcsok és a verziókkal.
- Nem hitelesített kérelmeket, amelyeknél a egy 401 választ. Ha például összehívások nem rendelkezik egy bearer jogkivonathoz vagy hibás vagy lejárt, vagy érvénytelen token.  


## <a id="access"></a>A naplók elérése ##

A **háttérismeretek – naplók-auditevent** tároló a tárterület-fiókot a megadott kulcs tárolóra naplók tárolja. Ez a tárolóban lévő valamennyi BLOB, írja be a következőt:

    Get-AzureStorageBlob -Container 'insights-logs-auditevent' -Context $sa.Context

A kimenet jelenik meg az alábbihoz hasonló:

**A tároló Uri: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent**


**név**

**----**

**resourceId = / ELŐFIZETÉSEK/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/szolgáltatók/a MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json**

**resourceId = / ELŐFIZETÉSEK/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/szolgáltatók/a MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json**

** resourceId = / ELŐFIZETÉSEK/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/szolgáltatók/a MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json***


A kimenet az látható, mint a BLOB hajtsa végre az elnevezni: **resourceId =<ARM resource ID>/y =<year>/m =<month>/d =<day of month>/h =<hour>/m =<minute>/filename.json**

A dátum és idő értékek UTC használja.

Naplógyűjtés erőforrások több használható ugyanazzal a tárterület-fiókkal, a teljes erőforrás-azonosító blob nevét a rendkívül hasznos eléréséhez, vagy csak a szükséges BLOB letöltése található. De fogja először foglalkozunk előtte, valamennyi BLOB letöltéséről.

Először hozzon létre egy mappát töltse le a BLOB. Példa:

    New-Item -Path 'C:\Users\username\ContosoKeyVaultLogs' -ItemType Directory -Force

Kattintson a lista beszerzése az összes BLOB:  

    $blobs = Get-AzureStorageBlob -Container $container -Context $sa.Context

Ebben a listában "Get-AzureStorageBlobContent" a BLOB letöltése a cél mappába keresztül pipe:

    $blobs | Get-AzureStorageBlobContent -Destination 'C:\Users\username\ContosoKeyVaultLogs'

Ha a második parancsot futtatja a **/** a blob nevében elválasztó szerkezetbe teljes mappa a célmappát csoportjában, és töltse le és tárolni a BLOB-fájlként szolgálnak a struktúra.

BLOB szelektív letöltéséhez használhat helyettesítő karaktereket. Példa:

- Ha több fő tárolókban van, és szeretné tölteni az egyetlen fő tárolóból elemre a naplókat, nevű CONTOSOKEYVAULT3:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3

- Ha több erőforrás csoportokat, és töltse le a naplók csak egy erőforrás csoport szeretne van `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'

- Ha azt szeretné, a naplókat a hónapot január 2016 töltheti le, `-Blob '*/year=2016/m=01/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'

Most már készen indítsa el, mit tartalmaz a naplókat megjeleníti. De az alakzatot, hogy a szükség lehet, hogy Get AzureRmDiagnosticSetting két további paramétereinek áthelyezése előtt:

- Lekérdezés állapotának a fő tárolóra erőforrás diagnosztikai beállítások:`Get-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId`

- A fő tárolóra erőforrás naplózás letiltása:`Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories AuditEvent`


## <a id="interpret"></a>A kulcs tárolóra naplók értelmezése ##

Szövegként formázott JSON blob-ként egyes BLOB tárolja. Ez a egy példa naplóbejegyzés futtatásának `Get-AzureRmKeyVault -VaultName 'contosokeyvault'`:

    {
        "records":
        [
            {
                "time": "2016-01-05T01:32:01.2691226Z",
                "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
                "operationName": "VaultGet",
                "operationVersion": "2015-06-01",
                "category": "AuditEvent",
                "resultType": "Success",
                "resultSignature": "OK",
                "resultDescription": "",
                "durationMs": "78",
                "callerIpAddress": "104.40.82.76",
                "correlationId": "",
                "identity": {"claim":{"http://schemas.microsoft.com/identity/claims/objectidentifier":"d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn":"live.com#username@outlook.com","appid":"1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},
                "properties": {"clientInfo":"azure-resource-manager/2.0","requestUri":"https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id":"https://contosokeyvault.vault.azure.net/","httpStatusCode":200}
            }
        ]
    }


Az alábbi táblázat a mező felsorolását és leírását.


| A mező neve        | Leírás |
| ------------- |-------------|
| idő      | Dátum és idő (UTC).|
| resourceId      | Azure erőforrás-kezelő erőforrás-azonosító. Kulcs tárolóból elemre a naplók ez nem mindig kulcs tárolóra erőforrás-azonosító.|
| operationName      | A művelet a következő táblázat ismertetett módon neve.|
| operationVersion      | Az ügyfél által kért a REST API-verzió: az.|
| kategória      | A kulcs tárolóból elemre a naplók AuditEvent értéke egyetlen, érhető el.|
| resultType      | Eredmény: REST API-összehívás.|
| resultSignature      | HTTP állapota.|
| resultDescription     | További leírást az eredményt, ha lehetséges.|
| durationMs      | A mennyi ideig tartott a REST API-összehívási ablak ezredmásodperc szolgáltatás. A hálózati késés ez nem vonatkozik, így előfordulhat, hogy az ügyféloldali mérése idő nem egyezik meg most.|
| callerIpAddress      | Az ügyfél, aki a kérelmet IP-címe.|
| correlationId      | Az ügyfél továbbíthatja (kulcs tárolóra) szolgáltatás mellett naplók az ügyféloldali naplókban összehangolására választható GUID.|
| azonosító      | A token lett mutatják be, amikor a REST API-kérés, az Identitáskezelés. Ez általában a "felhasználó", "szolgáltatás egyszerű" vagy "felhasználó + appId" ahogy kérelmének eredő Azure PowerShell-parancsmag álló együttes.|
| Tulajdonságok      | Ez a mező alapján a művelet (operationName) különböző információkat fogja tartalmazni. A legtöbb esetben tartalmaz az ügyfelek adatainak (a useragent karakterlánc az ügyfél által átadott), a pontos URI REST API-összehívást, és a HTTP-állapotkód. Ezeken kívül, ha egy objektum kérése (például KeyCreate vagy VaultGet) ad vissza azt is tartalmazza a kulcs URI (mint "azonosító"), a tárolóból elemre URI vagy a titkos URI.|




A **operationName** mező értékei ObjectVerb formátumban. Példa:

- Az összes kulcs tárolóra művelet a "tárolóból elemre`<action>`" formázni, például: `VaultGet` és `VaultCreate`.

- Az összes főbb művelet a "kulcs`<action>`" formázni, például: `KeySign` és `KeyList`.

- Az összes titkos művelet a "titkos`<action>`" formázni, például: `SecretGet` és `SecretListVersions`.

Az alábbi táblázat a operationName és a megfelelő REST API-parancsot.

| operationName        | REST API-parancs |
| ------------- |-------------|
| Hitelesítés      | Azure Active Directory-végpontot keresztül|
| VaultGet      | [A fő tárolóra adatainak megtekintése](https://msdn.microsoft.com/en-us/library/azure/mt620026.aspx)|
| VaultPut      | [Létrehozása vagy módosítása a fő tárolóból elemre](https://msdn.microsoft.com/en-us/library/azure/mt620025.aspx)|
| VaultDelete      | [A fő tárolóra törlése](https://msdn.microsoft.com/en-us/library/azure/mt620022.aspx)|
| VaultPatch      | [Frissítés a fő tárolóból elemre](https://msdn.microsoft.com/library/azure/mt620025.aspx)|
| VaultList      | [A lista összes kulcs tárolókban egy erőforrás csoportban](https://msdn.microsoft.com/en-us/library/azure/mt620027.aspx)|
| KeyCreate      | [Hozzon létre egy kulcsot](https://msdn.microsoft.com/en-us/library/azure/dn903634.aspx)|
| KeyGet      | [Egy kulcsot adatainak megtekintése](https://msdn.microsoft.com/en-us/library/azure/dn878080.aspx)|
| KeyImport      | [Egy kulcsot importálása a tárolóból elemre](https://msdn.microsoft.com/en-us/library/azure/dn903626.aspx)|
| KeyBackup      | [Biztonsági másolat egy kulcsot](https://msdn.microsoft.com/en-us/library/azure/dn878058.aspx).|
| KeyDelete      | [Egy kulcsot törlése](https://msdn.microsoft.com/en-us/library/azure/dn903611.aspx)|
| KeyRestore      | [Kulcs helyreállítása](https://msdn.microsoft.com/en-us/library/azure/dn878106.aspx)|
| KeySign      | [Jelentkezzen be a használatával](https://msdn.microsoft.com/en-us/library/azure/dn878096.aspx)|
| KeyVerify      | [Ellenőrizze a használatával](https://msdn.microsoft.com/en-us/library/azure/dn878082.aspx)|
| KeyWrap      | [Egy kulcsot tördelése](https://msdn.microsoft.com/en-us/library/azure/dn878066.aspx)|
| KeyUnwrap      | [Egy kulcsot tördelésének](https://msdn.microsoft.com/en-us/library/azure/dn878079.aspx)|
| KeyEncrypt      | [Titkosítás használatával](https://msdn.microsoft.com/en-us/library/azure/dn878060.aspx)|
| KeyDecrypt      | [Visszafejtése használatával](https://msdn.microsoft.com/en-us/library/azure/dn878097.aspx)|
| KeyUpdate      | [A kulcs módosítása](https://msdn.microsoft.com/en-us/library/azure/dn903616.aspx)|
| KeyList      | [A billentyűparancsok a tárolóból elemre a listában](https://msdn.microsoft.com/en-us/library/azure/dn903629.aspx)|
| KeyListVersions      | [A lista egy kulcsot verziója](https://msdn.microsoft.com/en-us/library/azure/dn986822.aspx)|
| SecretSet      | [Hozzon létre egy titkos kulcs](https://msdn.microsoft.com/en-us/library/azure/dn903618.aspx)|
| SecretGet      | [Titkos beszerzése](https://msdn.microsoft.com/en-us/library/azure/dn903633.aspx)|
| SecretUpdate      | [A titkos frissítése](https://msdn.microsoft.com/en-us/library/azure/dn986818.aspx)|
| SecretDelete      | [A titkos törlése](https://msdn.microsoft.com/en-us/library/azure/dn903613.aspx)|
| SecretList      | [A tárolóból elemre az lista titkos kulcsok](https://msdn.microsoft.com/en-us/library/azure/dn903614.aspx)|
| SecretListVersions      | [A titkos lista verziója](https://msdn.microsoft.com/en-us/library/azure/dn986824.aspx)|




## <a id="next"></a>Következő lépések ##

Azure kulcs tárolóból elemre a webes alkalmazásokban használó oktatóanyagot lásd: [Használata Azure kulcs webalkalmazást a tárolóból elemre](key-vault-use-from-web-application.md).

Programozási hivatkozások, [fejlesztői](key-vault-developers-guide.md)útmutatójában található az Azure kulcs tárolóból elemre.

Azure PowerShell 1.0-parancsmagok az Azure kulcs tárolóra listáját lásd: [Azure kulcs tárolóra parancsmagok](https://msdn.microsoft.com/library/azure/dn868052.aspx).

A fő elforgatás és a napló naplózás az Azure kulcs tárolóból elemre a tanulásra megtudhatja, [hogy miként kulcs tárolóra végpont kulcs elforgatás és a naplózás beállítási](key-vault-key-rotation-log-monitoring.md).

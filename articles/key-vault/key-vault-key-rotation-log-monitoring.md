<properties
    pageTitle="Kulcs tárolóra beállítása végpont kulcs elforgatás és naplózás |} Microsoft Azure"
    description="Ez az útmutató használata segítséget beállítási kulcs elforgatás és a fő tárolóra naplók figyelése"
    services="key-vault"
    documentationCenter=""
    authors="swgriffith"
    manager="mbaldwin"
    tags=""/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="jodehavi;stgriffi"/>
#<a name="how-to-setup-key-vault-with-end-to-end-key-rotation-and-auditing"></a>Kulcs tárolóra beállítása végpont kulcs elforgatás és naplózás

##<a name="introduction"></a>– Bevezetés

Miután létrehozta a Azure kulcs tárolóból elemre, fogja tudni feljebb helyezése a tárolóból elemre a billentyűk és titkos kulcsok indításához. Az alkalmazások már nem szükséges továbbra is fennáll a billentyűk vagy a titkos kulcsok, hanem szeretne kérni fogja őket a a fő tárolóból elemre szükség szerint. Így nélkül szeretné frissíteni kulcsok és titkos kulcsok érintő a titkos management viselkedéséről és az alkalmazás lehetőségeit körül a termékkulcsot egy szélessége nyit viselkedését.

Az alábbiakban ismertetjük példa feljebb helyezése tárolásához egy titkos, ebben az esetben az Azure tároló fiókkulcs érhető el egy alkalmazást, amely Azure kulcs tárolóból elemre. Azt fogja is bemutatják, egy adott tároló fiókkulcs ütemezett Elforgatás végrehajtását. Végül, azt ismerteti, hogy miként figyelheti a naplókat meg a fő tárolóból elemre, és értesítések emelni, ha a váratlan kérelmeket egy bemutató.

> \[AZURE. Megjegyzés:\] ebben az oktatóanyagban nem szánt elmagyarázni részletesen a kezdetként felfelé, az Azure kulcs tárolóból elemre. Ezt az információt olvassa el az [első lépések az Azure kulcs tárolóból elemre](key-vault-get-started.md)című témakört. Másik lehetőségként platformok parancssori felület útmutatásért lásd: [Ebben az oktatóanyagban egyenértékű](key-vault-manage-with-cli.md).

##<a name="setting-up-keyvault"></a>KeyVault beállítása

Ahhoz, hogy egy titkos lekérése az Azure kulcs tárolóból elemre az alkalmazás, először kell létrehozni a titkos és töltse fel a tárolóból elemre. Ez lehet elvégezni egyszerűen PowerShell alább látható módon.

Indítsa el az Azure PowerShell-munkamenetet, és jelentkezzen be az Azure-fiók a következő parancsot:

```powershell
Login-AzureRmAccount
```

Az előugró böngészőablakához írja be az Azure-fiók felhasználónevét és jelszavát. Azure PowerShell jelenik meg az előfizetések alapértelmezés szerint, és az ehhez a fiókhoz tartozó, használja a legelső.

Ha több előfizetéssel rendelkezik, lehet, ha egy adott megjegyzés létrehozása az Azure kulcs tárolóra használt adja meg. Írja be az alábbi módon lásd: a fiók az előfizetések:

```powershell
Get-AzureRmSubscription
```

Ezután adja meg az előfizetést, a naplózás, fő tárolóra van társítva, írja be:

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID> 
```

Ez a cikk bemutatja, hogy egy tároló fiókkulcs, mint egy titkos tárolását, mint szüksége lesz a tárhely fiókkulcs első.

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

Követően a titkos beolvasása, ebben az esetben a tárhely fiókkulcs szüksége lesz egy biztonságos karakterlánc, amely átalakítása és majd hozzon létre egy titkos az adott értéket abban a fő tárolóból elemre.

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```
A Tovább gombra a URI beszerzése az imént létrehozott titkos kívánt. Ez lesz egy újabb lépésben a beolvasni a titkos kulcs-tárolóra hívásakor. Futtassa az alábbi PowerShell-parancsot, és jegyezze fel a "Azonosító" érték, amely a titkos URI.

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

##<a name="setting-up-application"></a>Alkalmazás beállítása

Most, hogy van egy titkos tárolt kívánt beolvasni a titkos és használni a kódot. Létezik néhány olyan lépés, ez az első és a legfontosabb, amely az alkalmazás regisztrálása az Azure Active Directory és majd jelzi az alkalmazás adatai Azure kulcs tárolóból elemre, így engedélyezheti, hogy az alkalmazás érkező kérések eléréséhez szükséges.

> \[AZURE. Megjegyzés:\] az alkalmazás a kulcs tárolóra bérlői Azure Active Directory webhelyen kell létrehozni. 

Először nyissa meg az alkalmazások fülre az Azure Active Directory

![Az Azure Active Directory alkalmazások](./media/keyvault-keyrotation/AzureAD_Header.png)

Válassza az Azure Active Directory új alkalmazás hozzáadása "Hozzáadás"

![Válassza a alkalmazás hozzáadása](./media/keyvault-keyrotation/Azure_AD_AddApp.png)

A "Webes API Application Programming Interface és/vagy webhely" alkalmazás típusú hagyja, és nevezze el az alkalmazást.

![Az alkalmazás neve](./media/keyvault-keyrotation/AzureAD_NewApp1.png)

Az alkalmazás adjon meg egy "bejelentkezési URL-címe" és a egy "alkalmazás azonosítója URI". Ezek bármilyen Ez a bemutató módját, és megváltoztathatja később szükség esetén.

![Adja meg a szükséges URL-címe](./media/keyvault-keyrotation/AzureAD_NewApp2.png)

Ha az alkalmazás Azure AD hozzá van rendelve, tudomására az alkalmazás lappá. Ettől kezdve lapon kattintson a "Konfigurálása", és keresse meg és másolja az ügyfél-azonosító értéket. Jegyezze fel az ügyfél-azonosító újabb lépéseket.

A Tovább gombot az alkalmazás láthatja az Azure Active Directory vezérléséhez kulcs generálása kell. Ez a "Billentyűparancsok" szakasz a "Konfiguráció" lapon hozhat létre. Jegyezze fel a leendő használatra az Azure Active Directory-alkalmazásokból az újonnan létrehozott billentyűt.

![Azure Active Directory-alkalmazás kulcsok](./media/keyvault-keyrotation/Azure_AD_AppKeys.png)

Kulcs tárolóból elemre az alkalmazás bármely hívásait létrehozó előtt meg kell megállapítani, hogy a kulcs tárolóból elemre az alkalmazásról és az "engedélyek. A következő parancs megnyitja a tárolóból elemre nevét és az Azure Active Directory-alkalmazás az ügyfél-azonosító, és a "Get" hozzáférést biztosít a fő tárolóból elemre az alkalmazás.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

Ezen a ponton készen áll a alkalmazás hívások felépítésének megkezdéséhez. Az alkalmazás meg fog először telepítenie kell a NuGet csomagok vezérléséhez Azure kulcs tárolóból elemre, és Azure Active Directory szükséges. A Visual Studio csomag Manager konzolról adja meg a következő parancsokat. Ne feledje, hogy ez a cikk az írás a az Active Directory – csomag aktuális verziójának 3.10.305231913, ezért előfordulhat, hogy erősítse meg a legújabb verzióját, és ennek megfelelően frissítése szeretné.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

Az alkalmazás kódot hozzon létre egy osztály tartsa lenyomva az Active Directory hitelesítési módszer a. Ez a példa, hogy a osztály "Utils" nevezik. Majd kell hozzá a következő.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Ezután adja hozzá a JWT jogkivonat lekérése az Azure Active Directory, a következő módszerrel. Karbantartási követelmények érdemes áthelyezni a merevlemez-kódolt a webes vagy alkalmazás konfigurációban megadott értékeket.

```csharp
public async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);

    ClientCredential clientCred = new ClientCredential("<AzureADApplicationClientID>","<AzureADApplicationClientKey>");

    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)

    throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

Végül a szükséges kódot, és hívja fel a kulcs tárolóból elemre, és a titkos érték beolvasása is hozzáadhat. Először fel kell vennie a következő SQL-utasítás megadásával.

```csharp
using Microsoft.Azure.KeyVault;
```

Ezután ad hozzá, valamint a titkos kulcs tárolóból elemre meghívása a módszer hívások számára. Ezt a módszert nyújt a titkos URI, amely egy előző lépésben a mentett. Megjegyzés: a GetToken módszer az előbb létrehozott Utils osztály.
    
```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

Az alkalmazás futtatásakor most már az Azure Active Directory hitelesítő és a titkos érték majd lekérése az Azure kulcs tárolóból elemre.

##<a name="key-rotation-using-azure-automation"></a>Kulcs Forgatás Azure automatizálási használatával

A Forgatás stratégia tárolhat, mint Azure kulcs tárolóra titkos kulcsok értékek végrehajtása különféle lehetőségek is rendelkezésre állnak. Titkos kulcsok forgatható kézi folyamat részeként, azok is forgatható programozottan által API-hívások feljebb helyezése vagy is forgatható az automatizálási parancsfájl útján. A célból, ez a cikk azt fogja kell feljebb helyezése Azure PowerShell kombinálni Azure automatizálási Azure tároló fiók hívóbetű módosítása, és majd azt frissíti a fő tárolóra titkos új billentyűre. 

Ahhoz, hogy a fő tárolóból elemre a titkos értékek beállítására Azure automatizálási szüksége lesz az ügyfél-azonosító beszerzése a kapcsolat "AzureRunAsConnection", ha Ön az Azure automatizálási példány folyamatban létrehozott nevű. Az azonosító "Eszközök" parancsával az Azure automatizálási példány elérheti. Innen "Kapcsolatok" gombra, és válassza a "AzureRunAsConnection" szolgáltatás elve. Ajánljuk figyelmébe az alkalmazás azonosítója kívánt. 

![Azure automatizálási ügyfél-azonosító](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

Miközben továbbra is az eszközök ablak is érdemes "Modulok" választhatja ki. Modulból szeretne jelölje be a "Tár", majd keresse meg és a "Import" frissíti a következő modulokat minden egyes verzióiban.

    Azure
    Azure.Storage   
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage
    
> \[AZURE. Megjegyzés:\] , a jelen cikk az írás csak a fenti jegyezni szükséges frissíteni a megosztott alatti parancsfájl modulokat. Ha úgy találja, hogy a automatizálási feladatok meghiúsul, ellenőrizze, hogy van-e az összes szükséges modulok és azok importált függőségek.

Az Azure automatizálási kapcsolat beolvasása a azonosítója, után szüksége lesz állapítható meg, hogy az Azure kulcs tárolóból elemre, hogy az alkalmazás hozzáfér a tárolóból elemre a titkos kulcsok frissítéséhez. Ez az alábbi PowerShell paranccsal végezhető el.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

Ezután az Azure automatizálási példány csoportban az "Runbooks" erőforrás kiválasztott, és válassza a "egy Runbook hozzáadása". Jelölje ki a "Rövid létrehozása". Nevezze el a runbook, és jelölje be a PowerShell"runbook típust. Tetszés szerint adja meg a leírását. Végül kattintson a "Létrehozása".

![Runbook létrehozása](./media/keyvault-keyrotation/Create_Runbook.png)

Az új runbook editor ablakban megjelenő beillesztésének az alábbi PowerShell parancsprogramot.

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get the connection "AzureRunAsConnection "
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in to Azure..."
    Add-AzureRmAccount `
        -ServicePrincipal `
        -TenantId $servicePrincipalConnection.TenantId `
        -ApplicationId $servicePrincipalConnection.ApplicationId `
        -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
    "Login complete."
}
catch {
    if (!$servicePrincipalConnection)
    {
        $ErrorMessage = "Connection $connectionName not found."
        throw $ErrorMessage
    } else{
        Write-Error -Message $_.Exception
        throw $_.Exception
    }
}

#Optionally you may set the following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for the storage account
New-AzureRmStorageAccountKey -ResourceGroupName $RGName -StorageAccountName $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

A szerkesztő ablakból "Próba ablaktábla" választhat a parancsprogram tesztelése. A parancsfájlok futtatása után hiba nélkül is az "Közzététel" lehetőséget választja, és vissza, a runbook konfigurációs ablakban a runbook ütemezésének alkalmazhatja.

##<a name="key-vault-auditing-pipeline"></a>A folyamat kulcs tárolóból elemre a naplózás

Az Azure kulcs tárolóra beállítása során, kikapcsolhatja a naplózást naplógyűjtés a hozzáférési kérelmek a kulcs tárolóra végzett. Ezeket a naplókat egy kijelölt Azure tárterület-fiókban vannak tárolva, és tudja majd kell húzni, figyeli és elemezheti. Alább az alábbiakban az Azure függvények használja példa Azure összefüggés-alkalmazások és a kulcs tárolóból elemre naplók létrehozása egy folyamat küldése e-mailben, titkos kulcsok a a tárolóból elemre az alkalmazás, amelyek megegyeznek a web app alkalmazás azonosítója szerint vannak beolvasását.

Először is szüksége lesz a kulcs tárolóból elemre a naplózás engedélyezése. Ez az alábbi PowerShell-parancsait keresztül végezhető (minden részlet is láthatja [az alábbi](key-vault-logging.md)):

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>' 
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

Miután ez engedélyezve van, majd naplókat elkezdenek gyűjtése a kijelölt tárterület-fiókba. Ezek a naplók események olvashat, és amikor a kulcs tárolókban érhetők el, és ki által is tartalmaz. 

> \[AZURE. Megjegyzés:\] is elérheti a naplóadatokat legfeljebb, a művelet után a kulcs 10 perc tárolóra. A legtöbb esetben legyen gyorsabban ennél.

A következő lépésként [Hozzon létre egy Azure Service Bus várólista](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md). Ez lesz, hol vannak tolni a naplókat kulcs tárolóból elemre. Egyszer a várakozási sorban található, a logika alkalmazás fog felveszi őket, és őket működésbe lépnek. Hozzon létre egy szolgáltatás Bus van a továbbítás viszonylag egyenes és a magas szintű lépései a következők:

1. Szolgáltatás Bus névtér létrehozása (Ha már van egy szeretné használni a, majd ugorjon a 2).
2. Keresse meg a szolgáltatás Bus a portálon, és válassza ki a névteret szeretne létrehozni, kattintson a sor.
3. Válassza az új, és válassza a szolgáltatás Bus várólista ->, és adja meg a szükséges adatait.
4. Ragadja meg a szolgáltatás Bus kapcsolódási adatok kiválasztása a névtér, és kattintson a _Kapcsolat adatait_. A következő kijelző szüksége lesz az információk.

Ezután automatikusan [létrehozása az Azure függvény](../azure-functions/functions-create-first-azure-function.md) a kulcs tárolóra naplók a tárhely számla belüli lekérdezik, és válassza az új eseményre vonatkozóan. Ez lesz az ütemezés kiváltó függvényt.

Hozzon létre egy Azure függvény (az új gombot a portálon-függvény alkalmazás >). Létrehozása során egy meglévő Office csomag használja, vagy hozzon létre egy újat. Tudta is kiválaszthatják dinamikus elhelyezésére. Rendszeren függvény szolgáltatója beállítások találhatók [Itt](../azure-functions/functions-scale.md).

Ha a Azure függvényt jön létre, keresse meg a fájlt, és válassza a időzítőt, függvény és a C\# kattintson a **Létrehozás** gombra a kezdőképernyőről.

![Azure függvények, indítsa el a lap](./media/keyvault-keyrotation/Azure_Functions_Start.png)

Az _elektronikus oktatóanyagok elkészítése_ lap cserélje ki a run.csx kódot a következő:

```csharp
#r "Newtonsoft.Json"

using System;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.ServiceBus.Messaging; 
using System.Text;

public static void Run(TimerInfo myTimer, TextReader inputBlob, TextWriter outputBlob, TraceWriter log) 
{ 
    log.Info("Starting");

    CloudStorageAccount sourceStorageAccount = new CloudStorageAccount(new StorageCredentials("<STORAGE_ACCOUNT_NAME>", "<STORAGE_ACCOUNT_KEY>"), true);

    CloudBlobClient sourceCloudBlobClient = sourceStorageAccount.CreateCloudBlobClient();

    var connectionString = "<SERVICE_BUS_CONNECTION_STRING>";
    var queueName = "<SERVICE_BUS_QUEUE_NAME>";

    var sbClient = QueueClient.CreateFromConnectionString(connectionString, queueName);

    DateTime dtPrev = DateTime.UtcNow;
    if(inputBlob != null)
    {
        var txt = inputBlob.ReadToEnd();

        if(!string.IsNullOrEmpty(txt))
        {
            dtPrev = DateTime.Parse(txt);
            log.Verbose($"SyncPoint: {dtPrev.ToString("O")}");
        }
        else
        {
            dtPrev = DateTime.UtcNow;
            log.Verbose($"Sync point file didnt have a date. Setting to now.");
        }
    }

    var now = DateTime.UtcNow;

    string blobPrefix = "insights-logs-auditevent/resourceId=/SUBSCRIPTIONS/<SUBSCRIPTION_ID>/RESOURCEGROUPS/<RESOURCE_GROUP_NAME>/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/<KEY_VAULT_NAME>/y=" + now.Year +"/m="+now.Month.ToString("D2")+"/d="+ (now.Day).ToString("D2")+"/h="+(now.Hour).ToString("D2")+"/m=00/";

    log.Info($"Scanning:  {blobPrefix}");

    IEnumerable<IListBlobItem> blobs = sourceCloudBlobClient.ListBlobs(blobPrefix, true);

    log.Info($"found {blobs.Count()} blobs");

    foreach(var item in blobs)
    {
        if (item is CloudBlockBlob)
        {
            CloudBlockBlob blockBlob = (CloudBlockBlob)item;

            log.Info($"Syncing: {item.Uri}");

            string sharedAccessUri = GetContainerSasUri(blockBlob);

            CloudBlockBlob sourceBlob = new CloudBlockBlob(new Uri(sharedAccessUri));

            string text;
            using (var memoryStream = new MemoryStream())
            {
                sourceBlob.DownloadToStream(memoryStream);
                text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
            }

            dynamic dynJson = JsonConvert.DeserializeObject(text);

            //required to order by time as they may not be in the file
            var results = ((IEnumerable<dynamic>) dynJson.records).OrderBy(p => p.time);

            foreach (var jsonItem in results)
            {
                DateTime dt = Convert.ToDateTime(jsonItem.time);

                if(dt>dtPrev){
                    log.Info($"{jsonItem.ToString()}");

                    var payloadStream = new MemoryStream(Encoding.UTF8.GetBytes(jsonItem.ToString()));
                    //When sending to ServiceBus, use the payloadStream and set keeporiginal to true
                    var message = new BrokeredMessage(payloadStream, true);
                    sbClient.Send(message);
                    dtPrev = dt;
                }
            }
        }
    }
    outputBlob.Write(dtPrev.ToString("o"));
}

static string GetContainerSasUri(CloudBlockBlob blob) 
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

    sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

    //Generate the shared access signature on the container, setting the constraints directly on the signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```
> \[AZURE. Megjegyzés:\] ellenőrizze, hogy le szeretné cserélni, mutasson a tárterület-fiókjába, hol a kulcs tárolóra naplók kerülnek, a korábban létrehozott szolgáltatás Bus és a fő tárolóra tároló naplók az adott elérési utat a fenti kódban változó figyelembevételével.

A függvény felveszi a legújabb naplófájl a tárterület-fiókból hol a kulcs tárolóra naplók kerülnek, grabs a legújabb eseményeket a fájl, és szeretné egy szolgáltatás Bus várólista verembe küldi őket. Egy fájl több események lehetnek, mivel pl. fölé egy teljes órát, majd hozzunk létre, amely a funkció is megvizsgálja az határozza meg az utolsó esemény, amelyek lett felvett időbélyegzőjét _sync.txt_ fájl. Ezzel biztosíthatja, hogy azt nem tolás ugyanazt a esemény többször. A _sync.txt_ fájl egyszerűen tartalmazza az utolsó észlelt esemény időbélyeget. A naplók betöltésekor kell rendeznie az időbélyegző, annak érdekében, hogy megfelelő sorrendben alapján.

Ez a funkció azt, néhány további tárakat, amelyek nem érhetők el az Azure függvények beépített hivatkozást. Ezek felvételéhez szükséges Azure függvények csoportosítani őket nuget használatával. Válassza a _Fájlok megtekintése_ lehetőséget. 

![Fájlok nézetbeállítása](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

és vegyen fel egy új fájlt nevű _project.json_ a következő tartalmakkal:

```json
    {
      "frameworks": {
        "net46":{
          "dependencies": {
                "WindowsAzure.Storage": "7.0.0",
                "WindowsAzure.ServiceBus":"3.2.2"
          }
        }
       }
    }
```
_Mentés_ esetén ez elindítja a letöltéséhez szükséges bináris elemet az Azure függvények. 

Kattintson a **Integrate** fülre, és adjon az időzítő paraméter egy jól érthető nevet a függvényen belül. A fenti kód vár a _myTimer_kell meghívni időzítő. Adja meg a [CRON kifejezés](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) , az alábbi képlettel történik: 0 \* \* \* \* \* esetében az óra, amelyek hatására percenként egyszer futtatásához a függvény. 

Az azonos **Integrate** lapon adja meg egy beviteli, amely _Azure Blob-tárolóhoz_típusú lesz. Ez a _sync.txt_ fájlt, amely tartalmazza az időbélyegző, a függvény által nézett az utolsó esemény fog mutatni. Ez lesz elérhető a függvényen belül a paraméter neve. A fenti kód a Azure Blob-tárolóhoz bemeneti vár _inputBlob_kell a paraméter neve. A tárterület-fiókot a _sync.txt_ fájlt tároló (lehet azonos vagy egy másik tárterület-fiókot), és az elérési út mezőben adja meg az elérési utat, ahol a fájl él, formátuma {container-name}/path/to/sync.txt.

Vegye fel olyan eredménye, amely _Azure Blob-tárolóhoz_ kimeneti típusú lesz. A bemeneti a definiált _sync.txt_ fájlt is meg fog mutatni. Ez lesz a függvény az időbélyegző az utolsó esemény nézett írni. A fenti kód vár _outputBlob_hívható a paraméter.

Ezen a ponton a függvény készen áll a. Ellenőrizze, hogy lépjen vissza a lap **elektronikus oktatóanyagok elkészítése** és _mentése_ a kódot. Jelölje be a fordítási hibákat a kimeneti ablakban, és ennek megfelelően javítása azokat. Lefordítja az, ha a kód most futnia kell majd percenként naplókban a kulcs tárolóból elemre, és azt minden új események alakzatot a definiált szolgáltatás Bus várólista leküldéses. Meg kell jelennie a napló ablak űrlap jelenik megnyitásakor a függvény induljanak írni naplózási adatait.

###<a name="azure-logic-app"></a>Azure logika alkalmazás

Következő azt kell létrehozása az Azure logika alkalmazás, amely az eseményeket, a függvény a szolgáltatás Bus sorba van terjesztése elhozatala, a tartalom elemezni, és küldjön e-mailt az egyező feltétel alapján.

[Egy logikai-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md) új ismételt-logika alkalmazás >. 

Amikor létrejött a logika alkalmazást, keresse meg a fájlt, és válassza a _Szerkesztés_. A logikai alkalmazás szerkesztője válassza a _Szolgáltatás Bus várólista_ felügyelt api, és adja meg a szolgáltatás Bus hitelesítő adatait, és csatlakoztassa a sor.

![Azure logika alkalmazás szolgáltatás Bus](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

Ezután dönt, hogy _egy feltétel hozzáadása_. A feltétel, lépjen a _Speciális szerkesztő_ , és adja meg az alábbiakat, a APP_ID cseréli a tényleges APP_ID a web App:

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

Ez a kifejezés lényegében ad vissza **hamis,** a bejövő rendezvény (Ez a szolgáltatás Bus üzenet törzsét) az **appid** tulajdonság nem esetén az alkalmazás a **appid** . 

Ezen a ponton hozzon művelet a _Ha nem, nem tesz semmit..._ lehetőség alatt.

![Azure logika alkalmazás művelet kiválasztása](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

A művelet az _Office 365 - e-mail küldése_lehetőséget. Töltse ki a mezők, hozzon létre egy e-mailt küldeni, ha a megadott feltétel hamis értéket ad vissza. Ha nem rendelkezik az Office 365 sikerült tekintse alternatívák azonos eléréséhez.

Ezen a ponton van egy végpont folyamat, percben egyszer úgy gondolja, hogy az új kulcs tárolóra naplókat. Minden új naplók keres, akkor fog leküldéses őket egy szolgáltatás Bus sorba. A logikai alkalmazás aktiválódik, amint egy új üzenetet a várakozási sorban található hajtanak végre, és ha belül az esemény appid felel meg az alkalmazás azonosítója, a hívó alkalmazás, akkor nem küldhet e-mailben. 

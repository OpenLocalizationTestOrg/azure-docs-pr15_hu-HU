
<properties
   pageTitle="Azure erőforrás-kezelővel biztonságos szolgáltatás háló fürt létrehozása |} Microsoft Azure"
   description="Ez a cikk ismerteti, hogy miként állíthatja be a biztonságos szolgáltatás háló fürtre Azure erőforrás-kezelő, Azure kulcs tárolóból elemre, és Azure Active Directory (AAD) használatával ügyfél-hitelesítés Azure-ban."
   services="service-fabric"
   documentationCenter=".net"
   authors="chackdan"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="create-a-service-fabric-cluster-in-azure-using-azure-resource-manager"></a>Szolgáltatás háló fürt létrehozása az Azure-kezelővel Azure-ban

> [AZURE.SELECTOR]
- [Azure erőforrás-kezelő](service-fabric-cluster-creation-via-arm.md)
- [Azure portál](service-fabric-cluster-creation-via-portal.md)

Ez a cikk részletesen ismerteti, amelyek végigvezetik Önt a Azure erőforrás-kezelővel Azure-ban egy biztonságos Azure Service háló fürt beállításának lépéseit. Ez az útmutató végigvezeti az alábbi lépéseket:

 - Állítsa be a kulcs tárolóból elemre a fürt és az alkalmazás biztonsági billentyűparancsok kezelése.
 - A védett fürtre létrehozása az Azure erőforrás-kezelő Azure-ban.
 - A program hitelesíti a felhasználókat az Azure Active Directory (AAD) fürt kezelésére.

A biztonságos fürtre fürthöz megakadályozza, hogy ezzel az illetéktelen hozzáférést az adatkezelési műveletek, telepítése, frissítése és törlése a alkalmazások, a szolgáltatások és a bennük adatokat tartalmazó. Egy nem biztonságos fürthöz, hogy bárki bármikor csatlakozhat, és a adatkezelési műveletek hajthatók végre fürt. Bár is létre lehet hozni egy nem biztonságos fürthöz, érdemes **biztonságos fürt létrehozása nagyon ajánlott**. Egy nem biztonságos fürt **később nem biztonságos** – új fürt létre kell hozni.

A fogalmak megegyeznek hozhat létre biztonságos fürt, hogy a fürt Linux fürt vagy a Windows fürt. További információ és a segítő parancsfájlok biztonságos Linux fürt hozhat létre olvassa el a [Linux biztonságos fürt létrehozása](#secure-linux-clusters) című témakört.

## <a name="log-in-to-azure"></a>Bejelentkezés az Azure
Ez az útmutató használja az [Azure PowerShell][azure-powershell]. Ha egy új PowerShell-munkamenet indítása, jelentkezzen be az Azure-fiók, és jelölje ki azt az előfizetést Azure parancsok végrehajtása előtt.

Jelentkezzen be az azure-fiók:

```powershell
Login-AzureRmAccount
```

Jelölje ki azt az előfizetést:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-key-vault"></a>Kulcs tárolóra beállítása

Ez a szakasz az alábbiakban létrehozása kulcs tárolóból elemre, és a szolgáltatás háló alkalmazások számára a szolgáltatás háló fürtre Azure-ban. Kulcs tárolóból elemre a teljes útmutató, tekintse át az [kulcs tárolóra használatának megkezdését segítő útmutatót][key-vault-get-started].

Szolgáltatás háló X.509 tanúsítványok fürt biztonságos, és adja meg az alkalmazás biztonsági funkciókat használja. Azure kulcs tárolóból elemre az Azure Service háló fürt tanúsítványainak kezelésére szolgál. Fürt telepítik az Azure-ban, ha felelős szolgáltatás háló fürt létrehozása az Azure erőforrás-szolgáltató gyűjti össze a tanúsítványok a kulcs tárolóból elemre, és a fürt VMs telepíti őket.

Az alábbi ábra viszonyokat kulcs tárolóból elemre, a szolgáltatás háló fürtre és az Azure erőforrás-szolgáltató által használt tanúsítványokat, amikor létrehoz egy fürt tárolt kulcs tárolóból elemre között:

![Tanúsítvány telepítése][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a>Erőforráscsoport létrehozása

Első lépésként erőforrás csoport létrehozásához a kifejezetten a kulcs tárolóból elemre. Ajánlott üzembe helyezése a saját erőforráscsoport kulcs tárolóból elemre. Ez lehetővé teszi, hogy távolítsa el a számítási és a tárhely erőforrás csoport, beleértve a az erőforráscsoport, amelynek a szolgáltatás háló fürt a billentyűk és titkos kulcsok megtartásával. Az erőforráscsoport, amelynek a kulcs tárolóból elemre a fürt által használt ugyanabban a régióban kell lennie.

```powershell

    New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    
    ResourceGroupName : mycluster-keyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

### <a name="create-key-vault"></a>Hozzon létre fő tárolóból elemre 

Az új erőforráscsoport hozzon létre egy kulcsot tárolóból elemre. A kulcs tárolóból elemre **telepítéshez engedélyeznie kell** a szolgáltatás háló erőforrás használatához úgy juthat az tanúsítványok és fürt csomóponton telepítése:

```powershell

    New-AzureRmKeyVault -VaultName 'myvault' -ResourceGroupName 'mycluster-keyvault' -Location 'West US' -EnabledForDeployment
    
    
    Vault Name                       : myvault
    Resource Group Name              : mycluster-keyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault
    Vault URI                        : https://myvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions to Keys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions to Secrets   :    all
    
    
    Tags                             :
```

Ha egy meglévő kulcs tárolóból elemre, használja az Azure CLI telepítéshez engedélyezheti újra:

```cli
> azure login
> azure account set "your account"
> azure config mode arm 
> azure keyvault list
> azure keyvault set-policy --vault-name "your vault name" --enabled-for-deployment true
```

<a id="add-certificate-to-key-vault"></a>
## <a name="add-certificates-to-key-vault"></a>Adja hozzá a tanúsítványok kulcs tárolóból elemre.

Tanúsítványok szolgáltatás háló használt hitelesítési és titkosítási biztonságossá tétele fürt és a saját alkalmazások számos tulajdonságát megadását. A tanúsítványok használata szolgáltatás háló a további tudnivalókért lásd: a [szolgáltatás háló fürt biztonsági esetek][service-fabric-cluster-security].

### <a name="cluster-and-server-certificate-required"></a>Fürt és kiszolgálói tanúsítvány (kötelező) 

A tanúsítvány fürt biztonságos, és ezzel az illetéktelen hozzáférést megakadályozására szükséges. Néhány módokon fürt biztonsági biztosít:
 
 - **Fürt hitelesítés:** Csomópontok kapcsolati fürt összevonáshoz hitelesíti. Csak a igazolni tudja a személyazonosságot a tanúsítvánnyal csomópontok fürthöz is.
 - **Kiszolgálói hitelesítési:** Adatkezelési ügyfél, a fürt management végpontok hitelesíti, hogy az adatkezelési ügyfél tudja, hogy a valódi fürthöz folytat beszélgetést. A tanúsítvány és is biztosít az SSL API HTTPS-kezelés szolgáltatás háló Explorer HTTPS.

A tanúsítvány ezt a célt szolgálnak, az alábbi feltételeknek kell megfelelnie:

 - A tanúsítvány egy titkos kulcs kell tartalmaznia.
 - A tanúsítvány kulcs exchange, a személyes információkat Exchange (.pfx) fájlba exportálható létre kell hozni.
 - A tanúsítvány tulajdonosneve egyeznie kell a Service háló fürt eléréséhez használt tartomány. A matchng szükség a fürt HTTPS-kezelés végpontok és szolgáltatás háló Explorer SSL nyújtsanak. Az SSL-tanúsítvány (CA) tanúsítványszolgáltatótól nem szerezze be a `.cloudapp.azure.com` tartományt. Egyéni tartománynevet a fürt kell beszereznie. Tanúsítvány kérése Hitelesítésszolgáltatótól, amikor a tanúsítvány tulajdonosneve egyeznie kell az egyéni tartománynevet a fürthöz.

### <a name="application-certificates-optional"></a>Alkalmazás tanúsítványok (nem kötelező)

További tanúsítványok tetszőleges számú biztonsági okokból fürt telepíthető. Mielőtt hoz létre a fürt, vegye figyelembe az alkalmazás biztonsági jelenik meg, hogy az telepítve van a csomópontok, például egy tanúsítványt:

 - Titkosítás és az alkalmazás-konfiguráció értékek visszafejtés
 - Titkosítási adatok keresztül csomópontok replikáció során 

### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Formázás a tanúsítványok az Azure erőforrás-szolgáltató használata

Titkos kulcs fájlok (.pfx) hozzáadhatók és használt közvetlenül kulcs tárolóból elemre. Azonban a az Azure erőforrás-szolgáltató szükséges speciális JSON formátumban, amely tartalmazza a .pfx, mint egy alap 64 tárolni billentyűk címként kódolt karakterláncot, és a titkos kulcs jelszava. Ezeknek a követelményeknek igazodik billentyűk kell egy JSON karakterláncban elhelyezni és *titkos kulcsok* kulcs tárolóból elemre, majd tárolja.

Egyszerűbbé teheti, hogy egy PowerShell-modult érhető el [a GitHub][service-fabric-rp-helpers]. Kövesse az alábbi lépéseket a modul használatára:

  1. Töltse le a repó teljes tartalmának a helyi könyvtár be. 
  2. A PowerShell ablakában a modul importálása:

  ```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
  ```
     
A `Invoke-AddCertToKeyVault` parancs a PowerShell modul automatikusan a tanúsítvány titkos kulcs alakítja JSON karakterlánc és feltölti azt kulcs tárolóból elemre. Azt felvétele segítségével a fürt tanúsítványt, és bármilyen további alkalmazás tanúsítványok kulcs tárolóból elemre. Bármilyen további tanúsítványok a fürt telepíteni szeretné, ismételje meg ezt a lépést.

```powershell
 Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"
    
    Switching context to SubscriptionId <guid>
    Ensuring ResourceGroup mycluster-keyvault in West US
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    Using existing valut myvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret to myvault in vault myvault
    
    
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

Az előző karakterláncok összes az előfeltételei kulcs tárolóból elemre szolgáltatás háló fürt erőforrás-kezelő megfelelő sablont, és a telepítést a csomópont-hitelesítés, a kezelés végpont biztonsági és a hitelesítéshez tanúsítványok és bármely X.509 tanúsítványok használó további alkalmazás biztonsági szolgáltatások konfigurálásával. Ezen a ponton most már rendelkeznie kell a következő beállítási Azure-ban:

 - Erőforráscsoport kulcs tárolóból elemre.
   - Fő tárolóból elemre
     - Fürt kiszolgálói hitelesítési tanúsítvány
     - Alkalmazás tanúsítványok

## <a name="set-up-azure-active-directory-for-client-authentication"></a>Azure Active Directory ügyfél-hitelesítés beállítása

AAD lehetővé teszi, hogy a szervezetek (más néven bérlők) alkalmazások, amelyek egy webes bejelentkezési Felhasználóifelület-alkalmazások vannak osztva, és olyan natív ügyfele megoldást alkalmazások felhasználó hozzáférését kezelheti. A jelen dokumentum feltételezzük, hogy már létrehozott bérlői webhelyet. Ha nem, olvassa el [az Azure Active Directory-bérlői beszerzése][active-directory-howto-tenant].

A szolgáltatás háló fürtre kínál az adatkezelési szolgáltatásait, például a webes [Szolgáltatás háló Explorer] számos belépési pontról[ service-fabric-visualizing-your-cluster] és a [Visual Studio][service-fabric-manage-application-in-visual-studio]. Eredményt adja hozzon létre két AAD alkalmazások való hozzáférés korlátozása a fürt, egy webalkalmazás és egy tartozó alkalmazásban.

Egyszerűsítése érdekében néhány lépést AAD konfigurálása a szolgáltatás háló fürtre részt, azt a Windows PowerShell-parancsfájlokat halmazának hozott létre.

>[AZURE.NOTE] El kell végeznie ezeket a lépéseket *előtt* a fürt létrehozása így azokban az esetekben, ahol a parancsfájlok számíthat, fürt nevét és a végpontok, ezek a tervezett értéket, nem szegélyei már létrehozott kell lennie.

1. [Töltse le a parancsfájlok] [ sf-aad-ps-script-download] a számítógépen.

2. Kattintson a jobb gombbal a zip-fájl, válassza a **Tulajdonságok**, majd a **Tiltás feloldása** jelölőnégyzetet, és alkalmazhat.

3. Bontsa ki a zip-fájl.

4. Futtatása `SetupApplications.ps1`, a TenantId ClusterName és WebApplicationReplyUrl nyújtó paraméterként. Példa:

    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```

    A **TenantId** találhatja meg végrehajtása a PowerShell-parancsot "Get-AzureSubscription" ". Ekkor megjelenik a **TenantId** minden előfizetés.

    A **fürtnév** hozta létre a parancsfájl AAD alkalmazások előtag szolgál. A tényleges fürt nevének megfelelően, pontosan úgy, ahogyan csak célja, hogy könnyebben AAD eltérések feleltesse meg a szolgáltatás háló fürt van használatban, nem szükséges.

    A **WebApplicationReplyUrl** az alapértelmezett végpontot, amely a bejelentkezési folyamat befejezése után a felhasználók AAD ad eredményül. Meg kell beállítani a szolgáltatás háló Explorer végpont a fürt, amely alapértelmezés szerint:

    https://&lt;cluster_domain&gt;: 19080/Explorer

    Jelentkezzen be a AAD bérlői rendszergazdai jogosultságokkal rendelkező fiók kéri. Miután tesz, a parancsfájl hozhat létre a webhely és a szolgáltatás háló fürt ábrázolásához natív alkalmazások folytatódik. Ha megnézi az [Azure klasszikus portált]a bérlő alkalmazások[azure-classic-portal], meg kell jelennie két új bejegyzés:

    - *ClusterName*\_fürthöz
    - *ClusterName*\_ügyfél

    A követel meg az erőforrás-kezelő Azure-sablon, a következő szakaszban a fürt létrehozásakor a Json így megtartása ablakban nyissa meg a PowerShell parancsprogramot nyomtat.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Fürt erőforrás-kezelő szolgáltatás háló sablon létrehozása

Ebben a részben a kimenet az előző PowerShell-parancsok a fürt erőforrás-kezelő szolgáltatás háló sablont használják.

Erőforrás-kezelő minta sablonok érhetők el az [Azure rövid webhelyén található sablontár GitHub][azure-quickstart-templates]. Ezek a sablonok a fürt sablon használható kiindulási pontként. 

### <a name="create-the-resource-manager-template"></a>Az erőforrás-kezelő-sablon létrehozása

Ez az útmutató használja az [5-csomópont biztonságos fürt] [ service-fabric-secure-cluster-5-node-1-nodetype-wad] példa sablon és a paraméterek sablont. Töltse le a `azuredeploy.json` és `azuredeploy.parameters.json` a számítógépre, és nyissa meg a fájlokat a kedvenc szövegszerkesztőben.

### <a name="add-certificates"></a>Adja hozzá a tanúsítványok

Tanúsítványok a kulcs tárolóból elemre, amely tartalmazza a tanúsítvány billentyűk hivatkozó hozzáadódnak fürt erőforrás-kezelő sablon. Javasolt, hogy ezeket az értékeket a kulcs tárolóból elemre egy erőforrás-kezelő paraméterek sablonfájl megtartása az erőforrás-kezelő sablon fájl újból felhasználható, és a telepítéshez tartozó értékek ingyenes kerül.

#### <a name="add-all-certificates-to-the-vmss-osprofile"></a>Az összes tanúsítványok hozzáadása a VMSS osProfile

Minden tanúsítvány, amely a fürt telepíteni kell az VMSS erőforrás (Microsoft.Compute/virtualMachineScaleSets) osProfile részében kell beállítania. Ez a tulajdonság utasítja az erőforrás-szolgáltató a tanúsítvány telepítése a VMs. Ide tartoznak a fürt tanúsítvány, valamint a minden alkalmazás biztonsági tanúsítványok, az alkalmazások használni kívánt:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

#### <a name="configure-service-fabric-cluster-certificate"></a>Szolgáltatás háló fürt-tanúsítvány beállítása

A fürt hitelesítési tanúsítványt is konfigurálni kell a Service háló fürt erőforrás (Microsoft.ServiceFabric/clusters) és a szolgáltatás háló bővítmény VMSS a VMSS erőforrás. A szolgáltatás háló erőforrás szolgáltatót szeretne konfigurálni fürtre hitelesítési és végpontok management server-hitelesítés így.

##### <a name="vmss-resource"></a>VMSS erőforrás:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "thumbprint": "[parameters('clusterCertificateThumbprint')]",
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

##### <a name="service-fabric-resource"></a>Szolgáltatás háló erőforrás:

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
    },
    ...
  }
}
```

### <a name="insert-aad-config"></a>AAD config beszúrása

A korábban létrehozott AAD konfiguráció beilleszthető közvetlenül az erőforrás-kezelő sablon, de ajánlott ki kell olvasni az értékeket az első egy fájlba paraméterek arra, hogy az erőforrás-kezelő sablon újrafelhasználható és a szabad, az értékek adott telepítéshez paraméterek.

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStorevalue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

### < egy "konfigurálása-arm" ></a>beállítása erőforrás-kezelő sablon paraméterei

Végül a kimeneti értékeket a kulcs tárolóból elemre, és a AAD PowerShell parancs segítségével a paraméterek fájl feltöltése:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```
Ezen a ponton most már rendelkeznie kell a következőket:

 - Erőforráscsoport kulcs tárolóból elemre.
    - Fő tárolóból elemre
    - Fürt kiszolgálói hitelesítési tanúsítvány
    - Adatok rejtjelezése tanúsítvány
 - Azure Active Directory-bérlői 
    - Webes kezelése és a szolgáltatás háló Explorer AAD alkalmazás
    - A natív ügyfele kezeléséhez AAD alkalmazás
    - Szerepkörökhöz rendelt rendelkező felhasználók 
 - Szolgáltatás háló fürt erőforrás-kezelő sablon
    - Kulcs tárolóra keresztül konfigurált tanúsítványok
    - Azure Active Directory konfigurálva 

Az alábbi ábra mutatja be, ahol kulcs tárolóból elemre, és AAD konfigurációs hogyan illeszkednek az erőforrás-kezelő sablon.

![Erőforrás-kezelő függőség térkép][cluster-security-arm-dependency-map]

## <a name="create-the-cluster"></a>A csoport létrehozása

Most már készen [ARM telepítési]használatával hozza létre[resource-group-template-deploy].

#### <a name="test-it"></a>Tesztelje

Az alábbi PowerShell-parancsot használja az erőforrás-kezelő sablon tesztelje a paraméterek-fájl segítségével:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

#### <a name="deploy-it"></a>Üzembe

Az erőforrás-kezelő sablon állítás, igaz, ha használja az alábbi PowerShell-parancsot az erőforrás-kezelő sablon üzembe paraméterek-fájl segítségével:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

<a name="assign-roles"></a>
## <a name="assign-users-to-roles"></a>Felhasználók hozzárendelése szerepkörök

Miután létrehozta a csoportját képviseli az alkalmazásokat, a felhasználók hozzárendelése a szolgáltatás háló által támogatott szerepkörök szükséges: csak olvasható és a rendszergazda csempét. Ehhez az [Azure klasszikus portál][azure-classic-portal].

1. Keresse meg a bérlői, és válassza az alkalmazások.
2. Válassza a van neve webalkalmazás például `myTestCluster_Cluster`.
3. Kattintson a felhasználók fülre.
4. Válassza ki a felhasználót, hogy rendelni, majd kattintson a **hozzárendelése** gomb a képernyő alján.

    ![Felhasználók hozzárendelése szerepkörök gomb][assign-users-to-roles-button]

5. Jelölje ki a szerepkör hozzárendelése a felhasználóhoz.

    ![Felhasználók hozzárendelése szerepkörök][assign-users-to-roles-dialog]

>[AZURE.NOTE] Szerepkörökhöz szolgáltatás háló kapcsolatos további tudnivalókért olvassa el a [szolgáltatás háló ügyfelek szerepköralapú hozzáférés-vezérlés](service-fabric-cluster-security-roles.md)című témakört.

 <a name="secure-linux-cluster"></a> 
##  <a name="create-secure-clusters-on-linux"></a>Biztonságos fürt Linux létrehozása

A folyamat könnyebb segítő parancsfájl kapott [Itt](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux). A segítő parancsfájl, feltételezett értéke a, hogy már Azure CLI telepítve van, és a Path. Győződjön meg arról, hogy a parancsprogram engedélyei futtatásával végrehajtásához `chmod +x cert_helper.py` letölti után. Az első lépésként jelentkezzen be az Azure-fiók használatával, a CLI a `azure login` parancsot. A naplózás az Azure-fiókjába, használja a segítő az aláírt tanúsítvány, az alábbi parancs látható:

```sh
./cert_helper.py [-h] CERT_TYPE [-ifile INPUT_CERT_FILE] [-sub SUBSCRIPTION_ID] [-rgname RESOURCE_GROUP_NAME] [-kv KEY_VAULT_NAME] [-sname CERTIFICATE_NAME] [-l LOCATION] [-p PASSWORD]

The -ifile parameter can take a .pfx or a .pem file as input, with the certificate type (pfx or pem, or ss if it is a self-signed cert).
The parameter -h prints out the help text.
```

Ez a parancs a kimeneti adja eredményül a következő három karakterláncot: 

1. Egy SourceVaultID, amely az Ön készült új KeyVault ResourceGroup azonosítója. 

2. A tanúsítvány elérni egy CertificateUrl.

3. Egy CertificateThumbprint, amelyet az authentication.


A következő példa bemutatja a parancs használata:

```sh
./cert_helper.py pfx -sub "fffffff-ffff-ffff-ffff-ffffffffffff"  -rgname "mykvrg" -kv "mykevname" -ifile "/home/test/cert.pfx" -sname "mycert" -l "East US" -p "pfxtest"
```
Az előző parancs végrehajtása biztosít a három karakterláncot az alábbi képlettel történik:

```sh
SourceVault: /subscriptions/fffffff-ffff-ffff-ffff-ffffffffffff/resourceGroups/mykvrg/providers/Microsoft.KeyVault/vaults/mykvname
CertificateUrl: https://myvault.vault.azure.net/secrets/mycert/00000000000000000000000000000000
CertificateThumbprint: 0xfffffffffffffffffffffffffffffffffffffffff
```

 A tanúsítvány tulajdonosneve egyeznie kell a Service háló fürt eléréséhez használt tartomány. Az SSL nyújt a fürt HTTPS-kezelés végpontok és szolgáltatás háló Explorer szükséges. Az SSL-tanúsítvány (CA) tanúsítványszolgáltatótól nem szerezze be a `.cloudapp.azure.com` tartományt. Egyéni tartománynevet a fürt kell beszereznie. Ha egy tanúsítványt kérése Hitelesítésszolgáltatótól a tanúsítvány tulajdonosneve egyeznie kell az egyéni tartománynevet a fürthöz.

Ezek a bejegyzések (nélkül AAD) biztonságos szolgáltatás háló fürt létrehozása a [sablon-paraméterek beállítása erőforrás-kezelő](#configure-arm)leírtak szükséges. A biztonságos fürt útmutatás [ügyfélelérési fürtre hitelesítése](service-fabric-connect-to-secure-cluster.md)keresztül csatlakozhat. Linux előzetes fürt nem támogatják a AAD hitelesítést. Hozzárendelheti a rendszergazdai és az ügyfél szerepkörök, a [felhasználók szerepköröket rendelhet hozzájuk](#assign-roles)szakaszban leírt módon. Rendszergazdai és az ügyfél szerepkörök Linux előzetes fürt megadásakor kell adnia a tanúsítvány thumbprints hitelesítés (nem pedig a tulajdonos nevét, mivel visszavonási sem lánc érvényességi történik ez előzetes verzióval).


Tesztelés önaláírt tanúsítványt használni szeretné, ha segítségével az azonos parancsfájl önaláírt tanúsítvány létrehozása, és töltse fel KeyVault, mert a jelölőre `ss` kezeléséről a tanúsítvány elérési utat és a tanúsítvány neve helyett. Például látható létrehozása és feltöltése önaláírt tanúsítványt az alábbi parancsot:

```sh
./cert_helper.py ss -rgname "mykvrg" -sub "fffffff-ffff-ffff-ffff-ffffffffffff" -kv "mykevname"   -sname "mycert" -l "East US" -p "selftest" -subj "mytest.eastus.cloudapp.net" 
```

Ez a parancs ugyanaz három karakterláncok, SourceVault, CertificateUrl és CertificateThumbprint, biztonságos Linux fürt együtt a helyet, ahol az önaláírt tanúsítványt helyezte létrehozásához használt adja eredményül. Szüksége lesz a fürthöz való kapcsolódás önaláírt tanúsítványt.  A biztonságos fürt útmutatás [ügyfélelérési fürtre hitelesítése](service-fabric-connect-to-secure-cluster.md)keresztül csatlakozhat. A tanúsítvány tulajdonosneve egyeznie kell a Service háló fürt eléréséhez használt tartomány. Az SSL nyújt a fürt HTTPS-kezelés végpontok és szolgáltatás háló Explorer szükséges. Az SSL-tanúsítvány (CA) tanúsítványszolgáltatótól nem szerezze be a `.cloudapp.azure.com` tartományt. Egyéni tartománynevet a fürt kell beszereznie. Ha egy tanúsítványt kérése Hitelesítésszolgáltatótól a tanúsítvány tulajdonosneve egyeznie kell az egyéni tartománynevet a fürthöz.

A segítő parancsfájl által megadott paramétereket is kitölthető a portál [létrehozása az Azure-portálon fürt](service-fabric-cluster-creation-via-portal.md#create-cluster-portal)szakaszban leírt módon.

## <a name="next-steps"></a>Következő lépések

Ezen a ponton Ha hitelesítéssel Azure Active Directory nyújtó management biztonságos fürt. [Csatlakozzon a fürthöz](service-fabric-connect-to-secure-cluster.md) Tovább gombra, és bemutatja, hogy [alkalmazás titkos kulcsok](service-fabric-application-secret-management.md)kezelése.

## <a name="troubleshoot-setting-up-azure-active-directory-for-client-authentication"></a>Ügyfél-hitelesítés Azure Active Directory beállításának – problémamegoldás

Ha során az ügyfél-hitelesítés Azure Active Directory beállításának probléma lép fel, olvassa el a következő suggestings lehetséges megoldásairól.

### <a name="service-fabric-explorer-prompts-for-selecting-certificate"></a>Szolgáltatás háló Explorer kéri a tanúsítvány kiválasztása

#### <a name="problem"></a>A probléma

AAD bejelentkezési lapja a szolgáltatás háló Intézőben sikeresen bejelentkezéshez, után a a böngészőben adja vissza a kezdőlapra, de a kéri a tanúsítvány kiválasztása párbeszédpanel.

![SFX a tanúsítvány kiválasztása párbeszédpanel][sfx-select-certificate-dialog]

#### <a name="reason"></a>Oka

A felhasználó AAD fürt alkalmazásban nem lett szerepkörhöz. Így AAD hitelesítési szolgáltatás háló fürt sikertelen lesz. Szolgáltatás háló Explorer visszavált hitelesítési tanúsítványt.

#### <a name="solution"></a>Megoldás

Kövesse az utasításokat AAD beállításának és felhasználói szerepköröket rendelhet hozzájuk. "Felhasználó HOZZÁRENDELÉS szükséges az ACCESS-alkalmazás" ajánlott is be van kapcsolva hogy `SetupApplications.ps1` tartalmaz.

### <a name="connect-with-powershell-fails-with-error-the-specified-credentials-are-invalid"></a>Csatlakozás PowerShell megszakad, és hiba: A megadott hitelesítő adatok nem érvényesek

#### <a name="problem"></a>A probléma

Amikor PowerShell-lel való "AzureActiveDirectory" biztonsági mód fürthöz csatlakozni, után sikeresen AAD bejelentkezési lapja a bejelentkezés, kapcsolat hibával meghiúsul: a megadott hitelesítő adatok érvénytelenek látható.

#### <a name="solution"></a>Megoldás

Ugyanaz, mint feljebb.

### <a name="service-fabric-explorer-signing-in-return-failure-aadsts50011"></a>Szolgáltatás háló Explorer hiba ismét bejelentkezni: AADSTS50011

#### <a name="problem"></a>A probléma

Után bejelentkezés AAD bejelentkezési lapján a szolgáltatás háló Intézőben, oldal adja vissza bejelentkezési hiba – AADSTS50011: A Válaszcím &lt;URL-cím&gt; értéke nem egyezik meg a válasz címeket be van állítva az alkalmazás: &lt;globálisan egyedi azonosítója&gt;. 

![SFX válaszcím nem egyezik meg][sfx-reply-address-not-match]

#### <a name="reason"></a>Oka

A szolgáltatás háló Explorer kísérletek AAD hitelesítő képviselő cluster(web) alkalmazást, a kérelem részeként biztosít az átirányítási visszatérési URL-cím. De nem szerepel az alkalmazás "Válasz URL-címe" AAD listában.

#### <a name="solution"></a>Megoldás

"Válasz URL-címe" cluster(web) alkalmazás beállítása lapján szolgáltatás háló Explorer URL-címének hozzáadása vagy cseréje a listában az elemek egyikére. Mentse.

![Webes alkalmazás válasz URL-címe][web-application-reply-url]

### <a name="can-i-reuse-the-same-aad-tenant-for-multiple-clusters"></a>Felhasználhatja több fürtre a azonos AAD bérlői webhelyemen?

#### <a name="answer"></a>Válasz

igen. De ne felejtse el a szolgáltatás URL-cím háló Intéző felvenni a cluster(web) alkalmazás, különben a program szolgáltatás háló Explorer nem fog működni.

### <a name="why-do-i-still-need-server-certificate-while-aad-enabled"></a>Miért továbbra is szükségem kiszolgálói tanúsítvány AAD engedélyezése közben?

#### <a name="answer"></a>Válasz

FabricClient és FabricGateway kölcsönös hitelesítés végrehajtása. Abban az esetben, ha AAD hitelesítés AAD integrációs itt server és a kiszolgálói tanúsítvány ügyfél-azonosító kiszolgáló személyazonosságának ellenőrzésére szolgál. Tanúsítvány működése szolgáltatás háló kapcsolatos további tudnivalókért olvassa el [X.509 tanúsítványok és szolgáltatás][x509-certificates-and-service-fabric]

<!-- Links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-classic-portal]: https://manage.windowsazure.com
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype-wad]: https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype-wad/
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[assign-users-to-roles-button]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-button.png
[assign-users-to-roles-dialog]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png
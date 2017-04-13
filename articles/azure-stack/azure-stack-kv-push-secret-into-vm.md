<properties
    pageTitle="A tanúsítvány használatával Azure Papírhalom kulcs tárolóból elemre egy virtuális üzembe helyezéséhez |} Microsoft Azure"
    description="Megtudhatja, hogyan telepítheti a egy virtuális, és a Azure Papírhalom kulcs tárolóból elemre egy tanúsítványt behelyezése"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="create-vms-and-include-certificates-retrieved-from-key-vault"></a>VMs létrehozása és a kulcs tárolóból elemre beolvasott tanúsítvány belefoglalása

Egymást fedő Azure VMs Azure erőforrás-kezelővel van telepítve, és most tárolhatja a tanúsítványok Azure Papírhalom kulcs tárolóból elemre. Ezután Azure Papírhalom (külön Microsoft.Compute erőforrás szolgáltató) verembe küldi őket a VMs be, amikor a VMs van telepítve. Tanúsítványok sok esetben SSL, a titkosítási és a tanúsítvány-alapú hitelesítés használható.

Ez a módszer használatával tarthatja a tanúsítvány megbízható. Most már nem szerepel a virtuális kép, illetve az alkalmazás-konfiguráció fájlokat vagy a nem biztonságos máshol. A fő tárolóra megfelelő hozzáférési házirend megadásával is megadhatja, hogy ki kerülhet a tanúsítvány való hozzáférést. Egy másik előnye, hogy egy helyen Azure Papírhalom kulcs tárolóból elemre az összes a tanúsítványok kezelheti.

Az alábbiakban áttekintheti, a folyamat:

-   A tanúsítvány .pfx formátumban van szüksége.

-   Hozzon létre egy fő tárolóból (vagy egy sablont, vagy a következő mintaparancsfájl használatával) elemre.

-   Győződjön meg arról, hogy be van kapcsolva a EnabledForDeployment kapcsolót.

-   Töltse fel a tanúsítványt, a titkos.

## <a name="deploying-vms"></a>VMs üzembe helyezése

A mintaparancsfájl létrehoz egy fő tárolóból elemre, és a .pfx fájl a, a titkos kulcs tárolóból elemre kattintva egy helyi könyvtárban tárolt tanúsítvány majd tárolja.

    $vaultName = "contosovault"
    $resourceGroup = "contosovaultrg"
    $location = "local"
    $secretName = "servicecert"
    $fileName = "keyvault.pfx"
    $certPassword = "abcd1234"

    # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

    $fileContentBytes = get-content \$fileName -Encoding Byte

    $fileContentEncoded =
    [System.Convert\]::ToBase64String(\$fileContentBytes)
    $jsonObject = @"
    {
    "data": "\$filecontentencoded",
    "dataType" :"pfx",
    "password": "\$certPassword"
    }
    @$jsonObjectBytes = [System.Text.Encoding\]::UTF8.GetBytes(\$jsonObject)
    $jsonEncoded = \[System.Convert\]::ToBase64String(\$jsonObjectBytes)
    Switch-AzureMode -Name AzureResourceManager
    New-AzureResourceGroup -Name \$resourceGroup -Location \$location
    New-AzureKeyVault -VaultName \$vaultName -ResourceGroupName
    $resourceGroup -Location \$location -sku standard -EnabledForDeployment
    $secret = ConvertTo-SecureString -String \$jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName \$vaultName -Name \$secretName -SecretValue \$secret

A parancsfájl első része beolvassa a .pfx fájl, majd a fájl tartalom base64 kódolású rendelkező objektum, az azt egy JSON tárolja. A JSON-objektum akkor is kódolt base64.

Ezután létrehoz egy új erőforráscsoport, és ekkor létrehoz egy fő tárolóból elemre. Megjegyzés: a New-AzureKeyVault parancs az utolsó paraméter "-EnabledForDeployment", amely hozzáférést biztosít az Azure (kifejezetten az Microsoft.Compute erőforrás szolgáltató) a kulcs titkos kulcsok tárolóra telepítésekhez olvasható.

A legutóbbi parancs egyszerűen a kódolt base64 JSON objektumot tárolja a fő tárolóból elemre a, a titkos.

Az alábbiakban az előző parancsfájl minta kimenete:

    VERBOSE:  – Created resource group 'contosovaultrg' in
    location
    'eastus'
    ResourceGroupName : contosovaultrg
    Location          : eastus
    ProvisioningState : Succeeded
    Tags              :
    Permissions       :
                        Actions NotActions
                        ======= ==========
                        \*
    ResourceId        :
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-dd149b4aeb56/re
    sourceGroups/contosovaultrg
    VaultUri             : https://contosovault.vault.azure.net
    TenantId             : xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47
    TenantName           : xxxxxxxx
    Sku                  : standard
    EnabledForDeployment : True
    AccessPolicies       : {xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47}
    AccessPoliciesText   :
                           Tenant ID              :
                           xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47
                           Object ID              :
                           xxxxxxxx-xxxx-xxxx-xxxx-b092cebf0c80
                           Application ID         :
                           Display Name           : Derick Developer  (derick@contoso.com)
                           Permissions to Keys    : get, create, delete,
                           list, update, import, backup, restore
                           Permissions to Secrets : all
    OriginalVault        : Microsoft.Azure.Management.KeyVault.Vault
    ResourceId           :
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-dd149b4aeb56                 
    /resourceGroups/contosovaultrg/providers/Microsoft.KeyV
                           ault/vaults/contosovault
    VaultName            : contosovault
    ResourceGroupName    : contosovaultrg
    Location             : eastus
    Tags                 : {}
    TagsTable            :
    SecretValue     : System.Security.SecureString
    SecretValueText :
    ew0KImRhdGEiOiAiTUlJSkN3SUJBekNDQ01jR0NTcUdTSWIzRFFFSEFh
    Q0NDTGdFZ2dpME1JSUlzRENDQmdnR0NTcUdTSWIzRFFFSEFhQ0NCZmtF           
    Z2dYMU1JSUY4VENDQmUwR0N5cUdTSWIzRFFFTUNnRUNvSUlFL2pDQ0JQ
    &lt;&lt;&lt; Output truncated… &gt;&gt;&gt;
    Attributes      :
    Microsoft.Azure.Commands.KeyVault.Models.SecretAttributes
    VaultName       : contosovault
    Name            : servicecert
    Version         : e3391a126b65414f93f6f9806743a1f7
    Id              :
    https://contosovault.vault.azure.net:443/secrets/servicecert
                      /e3391a126b65414f93f6f9806743a1f7

Most már vagyunk virtuális sablon telepíthető. Megjegyzés: a titkos (a kiemelt az előző eredményt ad, zöld) eredményéből az URI.

Itt található sablon kell. A paraméterek (mellett a szokásos virtuális paraméterek) érdeklik a tárolóból elemre nevét, a tárolóból elemre erőforráscsoport és a titkos URI. Természetesen is GitHub töltse le és szükség szerint módosítsa az.

A virtuális telepíti, amikor az Azure beszúrhatja a tanúsítvány a virtuális be.
A Windows a tanúsítványok a .pfx fájl nem exportálható titkos kulccsal kerülnek. A tanúsítvány LocalMachine tanúsítványt, a tanúsítvány áruházzal, a felhasználó által megadott helyre kerül. Linux, biztonságitanúsítvány-fájl helyét, /var/lib/waagent könyvtárában fájlnévvel &lt;UppercaseThumbprint&gt;.crt esetében a X509 biztonságitanúsítvány-fájl, és &lt;UppercaseThumbprint&gt;.prv a titkos kulcs.
A fájlok formázva .pem.

Az alkalmazás általában megtalálja a tanúsítvány használatával az ujjlenyomatot, és nem szükséges módosítását.

## <a name="retiring-certificates"></a>Tanúsítványok használatból történő


Az előző szakaszban azt mutatott, hogyan új tanúsítvány tartalomtípusban szeretné a meglévő VMs. De a régi tanúsítvány még a virtuális van, és nem lehet eltávolítani. A nagyobb biztonság érdekében módosíthatja a régi titkos attribútum "Letiltva", így akkor is, ha egy régi sablon létrehozása egy virtuális tanúsítvány ezen régebbi verziójával próbál létrehozni, lesz. A következő módon adhatók meg, hogy egy adott titkos verzió tiltható le:

    Set-AzureKeyVaultSecretAttribute -VaultName contosovault -Name servicecert -Version e3391a126b65414f93f6f9806743a1f7 -Enable 0

## <a name="conclusion"></a>Elfogadásáról


Új ezzel a módszerrel a tanúsítvány is lehet elkülönítve a virtuális kép vagy az alkalmazás tartalom. Így azt távolított információk megjelenítése egy pontját.

A tanúsítvány is megújítani és anélkül, hogy újraépíti a virtuális kép vagy az alkalmazás telepítőcsomag feltöltött kulcs tárolóból elemre. Az alkalmazás ellátni a az új tanúsítvány verzióhoz készült új URI azonban még elvégzendő.

A tanúsítvány elválasztva a virtuális vagy az alkalmazás tartalom, akkor most csökkentett közvetlen érik el a tanúsítvány személyzet számát.

Egy hozzáadott kedvezménye, ekkor egy tetszőleges mappájába, a kulcs tárolóból elemre az összes a tanúsítványok, többek között a mindegyiket időbeli bevezetett kezeléséhez.

## <a name="next-steps"></a>Következő lépések

[A virtuális kulcs tárolóra jelszóval terjesztése](azure-stack-kv-deploy-vm-with-secret.md)

[Az alkalmazások kulcs tárolóra eléréséhez engedélyezése](azure-stack-kv-sample-app.md)
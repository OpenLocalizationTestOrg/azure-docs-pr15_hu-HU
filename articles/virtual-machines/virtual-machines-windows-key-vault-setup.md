<properties
    pageTitle="Virtuális gépeken futó az Azure erőforrás-kezelő kulcs tárolóra beállítása |} Microsoft Azure"
    description="Hogyan lehet kulcs tárolóra beállítása egy erőforrás-kezelő Azure virtuális gépen való használatra."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="singhkay"/>

# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Virtuális gépeken futó az Azure erőforrás-kezelő kulcs tárolóból elemre beállítása

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klasszikus telepítési modell

Egymást fedő Azure erőforrás-kezelő a titkos kulcsok/tanúsítványok kulcs tárolóból elemre az erőforrás-szolgáltató által biztosított erőforrásként vannak modellezni. További tudnivalók a kulcs tárolóból elemre, [Mi az Azure kulcs tárolóra?](../key-vault/key-vault-whatis.md)

>[AZURE.NOTE] 
>
>1. Ahhoz, hogy az erőforrás-kezelő Azure virtuális gépeken futó használandó billentyű tárolóra, a kulcs tárolóból elemre a **EnabledForDeployment** tulajdonság meg kell igaz. Ez a különböző ügyfélprogramokban teheti meg.
>
>2. A kulcs tárolóra kell az előfizetést és a helyen, ahol a virtuális gép hozható létre.

## <a name="use-powershell-to-set-up-key-vault"></a>Állítsa be a kulcs tárolóból elemre a PowerShell használatával
Hozzon létre egy fő tárolóból elemre PowerShell használatával, lásd: az [első lépések az Azure kulcs tárolóból elemre](../key-vault/key-vault-get-started.md#vault).

Az új kulcs tárolókban ezzel a PowerShell parancsmaggal használható:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

Meglévő kulcs tárolókban a PowerShell-parancsmag is használhatja:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="us-cli-to-set-up-key-vault"></a>Kapcsolatfelvételi CLI állíthat be kulcs tárolóból elemre.
A fő tárolóból elemre a parancssori kezelőfelületről létrehozásához lásd: a [kulcs tárolóra kezelése használata CLI](../key-vault/key-vault-manage-with-cli.md#create-a-key-vault)elemre.

CLI meg kell előtt telepítési házirend hozzárendelése, hozzon létre a fő tárolóból elemre. Ehhez használja a következő parancsot:

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a>Sablonok be szeretné állítani a kulcs tárolóból elemre.
Miközben olyan sablont használ, be kell állítania a `enabledForDeployment` tulajdonság `true` erőforrás kulcs tárolóból elemre.

    {
      "type": "Microsoft.KeyVault/vaults",
      "name": "ContosoKeyVault",
      "apiVersion": "2015-06-01",
      "location": "<location-of-key-vault>",
      "properties": {
        "enabledForDeployment": "true",
        ....
        ....
      }
    }

Egyéb beállítások konfigurálhatja sablonok használatával hozza létre a főbb tárolóból elemre, ha a [egy kulcs tárolóból elemre létrehozása](https://azure.microsoft.com/documentation/templates/101-key-vault-create/)című témakör tartalmaz.

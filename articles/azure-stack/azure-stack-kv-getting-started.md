<properties
    pageTitle="Első lépések – Azure Papírhalom kulcs tárolóra |} Microsoft Azure"
    description="Azure Papírhalom kulcs tárolóra használatának első lépései"
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
    ms.date="10/18/2016"
    ms.author="ricardom"/>


# <a name="getting-started-with-key-vault"></a>Első lépések – kulcs tárolóból elemre.

Ez a szakasz ismerteti a lépéseket követve hozzon létre egy tárolóból elemre, billentyűparancsok és titkos kulcsok kezelése, valamint engedélyezheti a felhasználók és a Azure Jegyzettömbben tárolóra műveletek meghívásához alkalmazások. A következő lépések feltételezik bérlői előfizetés létezik, és KeyVault szolgáltatás regisztrálva van az adott előfizetés. A rendelkezésre álló KeyVault parancsmagok alapuló minden példa parancs az Azure PowerShell SDK részeként.

## <a name="enabling-the-tenant-subscription-for-vault-operations"></a>A bérlői előfizetés tárolóra műveletekhez engedélyezése 

Bármely tárolóra műveleteket állíthatnak be, mielőtt kell győződjön meg arról, hogy az előfizetés engedélyezve van műveletek tárolóból elemre. Erősítse kiállító az alábbi PowerShell-parancsot:

    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault | ft -AutoSize

A fenti parancs jelentse "Regisztrált" a "Bejegyzés" minden sor állapotát.

    ProviderNamespace RegistrationState ResourceTypes Locations
    Microsoft.KeyVault Registered {operations} {local}
    Microsoft.KeyVault Registered {vaults} {local}
    Microsoft.KeyVault Registered {vaults/secrets} {local}
    

 Nem, amely a helyzet, ha meg kell meghívása a KeyVault szolgáltatás belül az előfizetés regisztrálhatja a következő parancsot:

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault

És az alábbi parancs a kimenet:
    
    ProviderNamespace : Microsoft.KeyVault
    RegistrationState : Registered
    ResourceTypes : {operations, vaults, vaults/secrets}
    Locations : {local}


>[AZURE.NOTE] Ha a hiba jelenik meg: "*az előfizetés nem regisztrált Azure kulcs tárolóból elemre*" Ha a meghívás parancsmagok KeyVault, erősítse meg a fenti utasításokat egy KeyVault erőforrás szolgáltató engedélyezve van.


## <a name="creating-a-hardened-container-a-vault-in-azure-stack-to-store-and-manage-cryptographic-keys-and-secrets"></a>Megerősített tároló (tárolóra) létrehozása az Azure egymást fedő tárolhatják és kezelhetik a titkosítási kulcs és titkos kulcsok

A tárolóból elemre egy bérlői kell először létrehozni, egy erőforrás csoportot. Az alábbi PowerShell-parancsok az erőforráscsoport hozzon létre egy erőforrás csoportot, majd a tárolóból elemre. A példában az adott parancsmag kimenete rendszerint is tartalmaz.

### <a name="creating-a-resource-group"></a>Erőforrás a csoport létrehozása:

    New-AzureRmResourceGroup -Name vaultrg010 -Location local -Verbose -Force

Kimenet:

    VERBOSE: Performing the operation "Replacing resource group ..." on target "".
    VERBOSE: 12:52:51 PM - Created resource group 'vaultrg010' in location 'local'
    ResourceGroupName : vaultrg010
    Location : local
    ProvisioningState : Succeeded
    Tags :
    ResourceId : /subscriptions/fa881715-3802-42cc-a54e-a06adf61584d/resourceGroups/vaultrg010
    

### <a name="creating-a-vault"></a>A tárolóból elemre létrehozása:


    New-AzureRmKeyVault -VaultName vault010 -ResourceGroupName vaultrg010 -Location local -Verbose

Kimenet:

    VaultUri : https://vault010.vault.azurestack.local
    TenantId : 5454420b-2e38-4b9e-8b56-1712d321cf33
    TenantName : 5454420b-2e38-4b9e-8b56-1712d321cf33
    Sku : Standard
    EnabledForDeployment : False
    EnabledForTemplateDeployment : False
    EnabledForDiskEncryption : False
    AccessPolicies : {5454420b-2e38-4b9e-8b56-1712d321cf33}
    AccessPoliciesText :
    Tenant ID : 5454420b-2e38-4b9e-8b56-1712d321cf33
    Object ID : ca342e90-f6aa-435b-a11c-dfe5ef0bfeeb
    Application ID :
    Display Name : Tenant Admin (tenantadmin1@msazurestack.onmicrosoft.com)
    Permissions to Keys : get, create, delete, list, update, import, backup, restore
    Permissions to Secrets : all
    OriginalVault : Microsoft.Azure.Management.KeyVault.Vault
    ResourceId : /subscriptions/fa881715-3802-42cc-a54e-a06adf61584d/resourceGroups/vaultrg010/providers/Microsoft.KeyVault/vaults/vault010
    VaultName : vault010
    ResourceGroupName : vaultrg010
    Location : local
    Tags : {}
    TagsTable :
    
Ez a parancsmag kimenete a fő tárolóra éppen létrehozott tulajdonságainak megjelenítése. A két legfontosabb tulajdonságokat a következők:

-   **Tárolóra neve**: példában, ez az **vault010**. Ezt a nevet más kulcs tárolóra parancsmagok fogja használni.

-   **Tárolóra URI**: példában, ez az https://vault010.vault.azurestack.local. A tárolóból elemre keresztül a REST API-t használó alkalmazások e URI kell használnia.

Az Azure-fiók engedélyezve, hogy a kulcsfontosságú tárolóra bármely műveletek hajthatók végre. A még senki sem egyéb nem.


## <a name="operating-on-keys-and-secrets"></a>Működő kulcsok és titkos kulcsok

Miután létrehozott egy tárolóból elemre, hajtsa végre a alatti létrehozásának lépései kezelése kulcsok és titkos kulcsok:

### <a name="creating-a-key"></a>Kulcs létrehozása

Annak érdekében, hogy hozzon létre egy kulcsot, használja az alábbi példában egy a **Hozzáadás-AzureKeyVaultKey** . Sikeres kulcs a létrehozás után a parancsmag kimeneti lesz az újonnan létrehozott fontos adatokat tartalmazzák.

    Add-AzureKeyVaultKey -VaultName \$vaultName -Name\$keyVaultKeyName -Verbose -Destination Software

Az alábbiakban a *Hozzáadás-AzureKeyVaultKey* parancsmag kimenete:

    Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes
    Key : {"kid":"https://vault010.vault.azurestack.local/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff","kty":"RSA","key\_ops":\["encrypt"
    ,"decrypt","sign","verify","wrapKey","unwrapKey"\],"n":"nDkcBQCyyLnXtbwFMnXOWzPDqWKiyjf0G3QTxvuN\_MansEn9X-91q4\_WFmRBCd5zWBqz671iuZO\_D4r0P25
    Fe2lAq\_3T1gATVNGR7LTEU9W5h8AoY10bmt4e0y66Jn2vUV-UTCz4\_vtKSKoiuNXHFR\_tGZ-6YX-frqKIiC8pbE4Qvz1x-c7E-eM\_Cpu87koL95n-Hl3wQRQRPXEPRR6gcHR5E74D1
    gLEFCWKySTo4nXtLoeBMNK5QYEBZIAS61ACbR4czjHn6ty-tZeVTc7hyK\_UO2EbJovQIAhyayfq018uNtCBzjjkqJKnY34kviVCPoTQqOdpHa0FHrloe5FeIw","e":"AQAB"}
    VaultName : vault010
    Name : keyVaultKeyName001
    Version : 86062b02b10342688f3b0b3713e343ff
    Id : https://vault010.vault.azurestack.local:443/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff
    
A kulcs létrehozott vagy töltenek fel Azure kulcs tárolóból elemre, a saját URI használatával most hivatkozhat. **Https://vault010.vault.azurestack.local:443/kulcsok/keyVaultKeyName001** segítségével mindig az aktuális verziójának; beszerzése és ez a verzió **86062b02b10342688f3b0b3713e343ff https://vault010.vault.azurestack.local:443/kulcsok/keyVaultKeyName001** segítségével.

### <a name="retrieving-a-key"></a>Egy kulcsot beolvasása

A **Get-AzureKeyVaultKey** segítségével beolvashatja egy kulcsot a és a részletek, a következő példa egy:

    Get-AzureKeyVaultKey -VaultName vault010 -Name keyVaultKeyName001

Az alábbiakban a kimenet: a Get-AzureKeyVaultKey

    Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes
    Key : {"kid":"https://vault010.vault.azurestack.local/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff","kty":"RSA","key\_ops":\["encrypt"
    ,"decrypt","sign","verify","wrapKey","unwrapKey"\],"n":"nDkcBQCyyLnXtbwFMnXOWzPDqWKiyjf0G3QTxvuN\_MansEn9X-91q4\_WFmRBCd5zWBqz671iuZO\_D4r0P25
    Fe2lAq\_3T1gATVNGR7LTEU9W5h8AoY10bmt4e0y66Jn2vUV-UTCz4\_vtKSKoiuNXHFR\_tGZ-6YX-frqKIiC8pbE4Qvz1x-c7E-eM\_Cpu87koL95n-Hl3wQRQRPXEPRR6gcHR5E74D1
    gLEFCWKySTo4nXtLoeBMNK5QYEBZIAS61ACbR4czjHn6ty-tZeVTc7hyK\_UO2EbJovQIAhyayfq018uNtCBzjjkqJKnY34kviVCPoTQqOdpHa0FHrloe5FeIw","e":"AQAB"}
    VaultName : vault010
    Name : keyVaultKeyName001
    Version : 86062b02b10342688f3b0b3713e343ff
    Id : https://vault010.vault.azurestack.local:443/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff

### <a name="setting-a-secret"></a>A titkos beállítása

    $secretvalue = ConvertTo-SecureString 'User@123' -AsPlainText -Force
    Set-AzureKeyVaultSecret -Name MySecret-VaultName vault010 -SecretValue $secretvalue
    
Kimenet

    Vault Name : vault010
    Name : MySecret
    Version : 65a387f2ed4a416180e852b970846f5b
    Id : https://vault010.vault.azurestack.local:443/secrets/MySecret/65a387f2ed4a416180e852b970846f5b
    Enabled : True
    Expires :
    Not Before :
    Created : 8/17/2016 10:49:52 PM
    Updated : 8/17/2016 10:49:52 PM
    Content Type :
    Tags : 

### <a name="retrieving-a-secret"></a>A titkos beolvasása

    Get-AzureKeyVaultSecret -VaultName vault010

Kimenet

    Vault Name : vault010
    Name : MySecret
    Version :
    Id : https://vault010.vault.azurestack.local:443/secrets/MySecret
    Enabled : True
    Expires :
    Not Before :
    Created : 8/17/2016 10:49:52 PM
    Updated : 8/17/2016 10:49:52 PM
    Content Type :
    Tags :

Most a fő tárolóból elemre, és a billentyűt vagy a titkos készen áll a alkalmazások használatához.
Engedélyeznie kell az alkalmazások használatát.

## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Az alkalmazás használatához a billentyűt vagy a titkos engedélyezése 

Engedélyezi a billentyűt vagy a tárolóból elemre a titkos eléréséhez az alkalmazás, használja a Set -**AzureRmKeyVaultAccessPolicy** parancsmag.

Ha például a tárolóból elemre neve *ContosoKeyVault* és a hitelesíteni kívánt alkalmazásnak *8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed*egy *Ügyfél-azonosító* , és az alkalmazás fejthető vissza, és jelentkezzen be a tárolóból elemre a billentyűkkel hitelesíteni kívánt, futtassa a következő:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Ha szeretné engedélyezni, hogy ugyanazt a kérelmet a tárolóból elemre a titkos kulcsok olvasható, futtassa a következő:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get


## <a name="next-steps"></a>Következő lépések
[A virtuális kulcs tárolóra jelszóval terjesztése](azure-stack-kv-deploy-vm-with-secret.md)

[A virtuális kulcs tárolóra tanúsítvánnyal terjesztése](azure-stack-kv-push-secret-into-vm.md)
<properties
    pageTitle="Virtuális gépeken futó az Azure erőforrás-kezelő WinRM access beállítása |} Microsoft Azure"
    description="Az erőforrás-kezelő Azure virtuális gépen való használatra WinRM access beállítása"
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
    ms.date="06/16/2016"
    ms.author="singhkay"/>

# <a name="setting-up-winrm-access-for-virtual-machines-in-azure-resource-manager"></a>Virtuális gépeken futó az Azure erőforrás-kezelő WinRM access beállítása

## <a name="winrm-in-azure-service-management-vs-azure-resource-manager"></a>Az Azure Szolgáltatáskezelés viewben Azure erőforrás-kezelő WinRM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klasszikus telepítési modell

* Ez a [cikk](../azure-resource-manager/resource-group-overview.md) bemutatja az Azure erőforrás-kezelő, tanulmányozza
* Azure Szolgáltatáskezelés és Azure erőforrás-kezelő eltérések olvassa el a [cikk](../resource-manager-deployment-model.md)

A fő között a két készleteket WinRM konfiguráció beállítása különbség hogyan a tanúsítvány lekéri a virtuális telepítendő. Az erőforrás-kezelő Azure egymást fedő a tanúsítványok a kulcs tárolóból elemre az erőforrás-szolgáltató által kezelt erőforrásként vannak modellezni. Ezért a felhasználó saját tanúsítvány nyújt, és töltse fel egy kulcsot tárolóból elemre egy virtuális gép használatához szükséges.

Az alábbiakban a lépések szeretné vinni egy virtuális beállítása a WinRM kapcsolatban

1. Hozzon létre egy kulcs tárolóból elemre
2. Önaláírt tanúsítvány létrehozása
3. Töltse fel a önaláírt tanúsítvány kulcs tárolóból elemre.
4. Az URL-cím beszerzése a önaláírt tanúsítványt az a billentyűt tárolóból elemre.
5. Hivatkozás a önaláírt tanúsítványok URL-címe egy virtuális létrehozásakor

## <a name="step-1-create-a-key-vault"></a>Lépés: 1: Hozzon létre egy fő tárolóból elemre

Használhat az alább a kulcs tárolóra létrehozása parancs

```
New-AzureRmKeyVault -VaultName "<vault-name>" -ResourceGroupName "<rg-name>" -Location "<vault-location>" -EnabledForDeployment -EnabledForTemplateDeployment
```

## <a name="step-2-create-a-self-signed-certificate"></a>Lépés: 2: Önaláírt tanúsítvány létrehozása
Önaláírt tanúsítvány használatával a PowerShell-parancsprogramot hozhat létre.

```
$certificateName = "somename"

$thumbprint = (New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation Cert:\CurrentUser\My -KeySpec KeyExchange).Thumbprint

$cert = (Get-ChildItem -Path cert:\CurrentUser\My\$thumbprint)

$password = Read-Host -Prompt "Please enter the certificate password." -AsSecureString

Export-PfxCertificate -Cert $cert -FilePath ".\$certificateName.pfx" -Password $password
```

## <a name="step-3-upload-your-self-signed-certificate-to-the-key-vault"></a>3 lépés: A önaláírt tanúsítvány feltöltése a kulcs tárolóból elemre.

A tanúsítvány feltöltése a kulcs tárolóból elemre lépés: 1 létrehozott, mielőtt kell weblappá alakítva az Microsoft.Compute erőforrás szolgáltató megértse formátumot. Az alábbi PowerShell parancsprogramot lehetővé teszi, hogy az ehhez szükséges lépéseket

```
$fileName = "<Path to the .pfx file>"
$fileContentBytes = Get-Content $fileName -Encoding Byte
$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

$jsonObject = @"
{
  "data": "$filecontentencoded",
  "dataType" :"pfx",
  "password": "<password>"
}
"@

$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText –Force
Set-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>" -SecretValue $secret
```

## <a name="step-4-get-the-url-for-your-self-signed-certificate-in-the-key-vault"></a>Lépés: 4: Az URL-cím beszerzése a önaláírt tanúsítványt az a billentyűt tárolóból elemre.

A Microsoft.Compute erőforrás-szolgáltató a virtuális kiépítési közben van szüksége egy URL-CÍMÉT a titkos belül a kulcs tárolóból elemre. Ezzel a Microsoft.Compute erőforrás-szolgáltató a titkos letöltése, és a egyenértékű tanúsítvány létrehozása a virtuális.

>[AZURE.NOTE]A titkos URL-CÍMÉT is kell tartalmaznia, valamint a verziót. 01h9db0df2cd4300a20ence585a6s7ve https://contosovault.vault.azure.net:443 és titkos kulcsok/contososecret alatti néz ki egy minta URL-cím


#### <a name="templates"></a>Sablonok

Elérheti a hivatkozást be a sablon URL-CÍMÉT az alábbi kód

    "certificateUrl": "[reference(resourceId(resourceGroup().name, 'Microsoft.KeyVault/vaults/secrets', '<vault-name>', '<secret-name>'), '2015-06-01').secretUriWithVersion]"

#### <a name="powershell"></a>A PowerShell

Elérheti e URL-címet használja az alábbi PowerShell-parancsot

    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id

## <a name="step-5-reference-your-self-signed-certificates-url-while-creating-a-vm"></a>5 lépés: Az önaláírt tanúsítványok URL-hivatkozás egy virtuális létrehozásakor

#### <a name="azure-resource-manager-templates"></a>Azure erőforrás-kezelő sablonok

A virtuális sablonok révén létrehozásakor a tanúsítvány kap hivatkozott a titkos kulcsok és a winRM szakaszát, mint az alábbi:

    "osProfile": {
          ...
          "secrets": [
            {
              "sourceVault": {
                "id": "<resource id of the Key Vault containing the secret>"
              },
              "vaultCertificates": [
                {
                  "certificateUrl": "<URL for the certificate you got in Step 4>",
                  "certificateStore": "<Name of the certificate store on the VM>"
                }
              ]
            }
          ],
          "windowsConfiguration": {
            ...
            "winRM": {
              "listeners": [
                {
                  "protocol": "http"
                },
                {
                  "protocol": "https",
                  "certificateUrl": "<URL for the certificate you got in Step 4>"
                }
              ]
            },
            ...
          }
        },

A fenti minta sablon megtalálható itt [201 virtuális – winrm-keyvault – windows -](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows) on

Ezzel a sablonnal forráskódjának [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows) találhatók

#### <a name="powershell"></a>A PowerShell

    $vm = New-AzureRmVMConfig -VMName "<VM name>" -VMSize "<VM Size>"
    $credential = Get-Credential
    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName "<Computer Name>" -Credential $credential -WinRMHttp -WinRMHttps -WinRMCertificateUrl $secretURL
    $sourceVaultId = (Get-AzureRmKeyVault -ResourceGroupName "<Resource Group name>" -VaultName "<Vault Name>").ResourceId
    $CertificateStore = "My"
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore $CertificateStore -CertificateUrl $secretURL

## <a name="step-6-connecting-to-the-vm"></a>Lépés 6: A virtuális csatlakozás
Csak akkor csatlakozhat a virtuális, győződjön meg arról, hogy kell a számítógépen van konfigurálva WinRM távfelügyelet. Indítsa el a PowerShell rendszergazdaként, majd hajtsa végre a alatti parancs, amellyel győződjön meg arról, hogy beállítása.

    Enable-PSRemoting -Force

>[AZURE.NOTE] Szükség lehet, hogy a WinRM szolgáltatás működik, ha a fenti nem működik. Megteheti, hogy segítségével`Get-Service WinRM`

Miután a beállítás befejeződött, lehet csatlakozni a virtuális használatával az alábbi parancs

    Enter-PSSession -ConnectionUri https://<public-ip-dns-of-the-vm>:5986 -Credential $cred -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck) -Authentication Negotiate
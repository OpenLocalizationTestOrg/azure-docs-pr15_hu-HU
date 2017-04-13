<properties
   pageTitle="A PowerShell használatá Azure szolgáltatások fordított DNS-rekordjának kezelését |} Microsoft Azure"
   description="Fordított DNS-rekordok vagy az erőforrás-kezelő PowerShell használatával Azure szolgáltatások PTR-rekordok kezelése"
   services="DNS"
   documentationCenter="na"
   authors="s-malone"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="DNS"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/28/2016"
   ms.author="smalone" />

# <a name="how-to-manage-reverse-dns-records-for-your-azure-services-using-azure-powershell"></a>Fordított DNS-rekordjait a Azure PowerShell használatával Azure szolgáltatások kezelése

[AZURE.INCLUDE [dns-reverse-dns-record-operations-arm-selectors-include.md](../../includes/dns-reverse-dns-record-operations-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [DNS-reverse-dns-record-operations-intro-include.md](../../includes/dns-reverse-dns-record-operations-intro-include.md)]
<BR>
[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][Klasszikus telepítési modell](dns-reverse-dns-record-operations-classic-ps.md).

## <a name="validation-of-reverse-dns-records"></a>Fordított DNS-rekordok ellenőrzése
Annak érdekében, hogy egy harmadik fél nem hozható létre fordított DNS-rekordokat a DNS-tartományok megfeleltetése, Azure csak lehetővé teszi, hogy hol az alábbiak egyike igaz fordított DNS-rekord létrehozása:

- A "ReverseFqdn" megegyezik az nyilvános IP-cím erőforrás, amelynek meg van adva "Fqdn" vagy bármely nyilvános IP-cím belül az azonos előfizetés "Fqdn" pl., "ReverseFqdn" "contosoapp1.northus.cloudapp.azure.com.".

- A "ReverseFqdn" előre oldja fel a nevét vagy a nyilvános IP-címének, amely meg van adva vagy bármely nyilvános IP-cím "Fqdn" vagy az IP-IP ugyanabban az előfizetésben belül például "ReverseFqdn" "app1.contoso.com." melyik a "contosoapp1.northus.cloudapp.azure.com." a CName alias

Adatérvényesítési csak engedélyezné, amikor a fordított DNS tulajdonsága egy nyilvános IP-cím vagy módosítani. Ismétlődő újbóli érvényesítése nem történik.

## <a name="add-reverse-dns-to-existing-public-ip-addresses"></a>Fordított DNS hozzáadása meglévő nyilvános IP-címek
Fordított DNS adhat a meglévő nyilvános IP-címet a "Set-AzureRmPublicIpAddress" parancsmaggal:

    PS C:\> $pip = Get-AzureRmPublicIpAddress -Name PublicIP -ResourceGroupName NRP-DemoRG-PS
    PS C:\> $pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
    PS C:\> Set-AzureRmPublicIpAddress -PublicIpAddress $pip

Ha ki szeretne fordított DNS hozzáadása egy meglévő nyilvános IP-címet, amely még nem rendelkezik a DNS-név, a DNS-név is meg kell adnia. A "Set-AzureRmPublicIpAddress" parancsmaggal elérni is felvehet:

    PS C:\> $pip = Get-AzureRmPublicIpAddress -Name PublicIP -ResourceGroupName NRP-DemoRG-PS
    PS C:\> $pip.DnsSettings = New-Object -TypeName Microsoft.Azure.Commands.Network.Models.PSPublicIpAddressDnsSettings
    PS C:\> $pip.DnsSettings.DomainNameLabel = "contosoapp1"
    PS C:\> $pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
    PS C:\> Set-AzureRmPublicIpAddress -PublicIpAddress $pip

## <a name="create-a-public-ip-address-with-reverse-dns"></a>Hozzon létre egy nyilvános IP-cím fordított DNS-rekordjaival
Új nyilvános IP-címet a fordított DNS tulajdonság megadva, az "Új-AzureRmPublicIpAddress" parancsmag használatával adhat hozzá:

    PS C:\> New-AzureRmPublicIpAddress -Name PublicIP2 -ResourceGroupName NRP-DemoRG-PS -Location WestUS -AllocationMethod Dynamic -DomainNameLabel "contosoapp2" -ReverseFqdn "contosoapp2.westus.cloudapp.azure.com."

## <a name="view-reverse-dns-for-existing-public-ip-addresses"></a>Nézet fordított DNS a meglévő nyilvános IP-címek
A meglévő nyilvános IP-címet a "Get-AzureRmPublicIpAddress" parancsmaggal a beállított értékkel tekintheti meg:

    PS C:\> Get-AzureRmPublicIpAddress -Name PublicIP2 -ResourceGroupName NRP-DemoRG-PS

## <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a>Fordított DNS eltávolítása a meglévő nyilvános IP-címek
A meglévő nyilvános IP-címet a "Set-AzureRmPublicIpAddress" parancsmaggal fordított DNS tulajdonság eltávolítható. Ez a ReverseFqdn tulajdonság értéke blank megadásával történik:

    PS C:\> $pip = Get-AzureRmPublicIpAddress -Name PublicIP -ResourceGroupName NRP-DemoRG-PS
    PS C:\> $pip.DnsSettings.ReverseFqdn = ""
    PS C:\> Set-AzureRmPublicIpAddress -PublicIpAddress $pip


[AZURE.INCLUDE [FAQ1](../../includes/dns-reverse-dns-record-operations-faq-host-own-arpa-zone-include.md)]

[AZURE.INCLUDE [FAQ2](../../includes/dns-reverse-dns-record-operations-faq-arm-include.md)]

<properties
   pageTitle="A PowerShell használatá Azure (klasszikus) szolgáltatások fordított DNS-rekordjának kezelését |} Microsoft Azure"
   description="Fordított DNS-rekordjait, vagy az Azure-szolgáltatások PowerShell használata a klasszikus telepítési modell PTR-rekordok kezelésének módját. "
   services="DNS"
   documentationCenter="na"
   authors="s-malone"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="DNS"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/28/2016"
   ms.author="smalone" />

# <a name="how-to-manage-reverse-dns-records-for-your-azure-services-classic-using-azure-powershell"></a>A Azure szolgáltatások (klasszikus) Azure PowerShell használatával fordított DNS-rekordok kezelése

[AZURE.INCLUDE [dns-reverse-dns-record-operations-arm-selectors-include.md](../../includes/dns-reverse-dns-record-operations-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [DNS-reverse-dns-record-operations-intro-include.md](../../includes/dns-reverse-dns-record-operations-intro-include.md)]
<BR>
[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Megtudhatja, miként [végezheti el ezeket a lépéseket az erőforrás-kezelő modellt használja](dns-reverse-dns-record-operations-ps.md).

## <a name="validation-of-reverse-dns-records"></a>Fordított DNS-rekordok ellenőrzése
Annak érdekében, hogy egy harmadik fél nem hozható létre fordított DNS-rekordokat a DNS-tartományok megfeleltetése, Azure csak lehetővé teszi, hogy hol az alábbiak egyike igaz fordított DNS-rekord létrehozása:

- A DNS FQDN fordítottja a a Felhőbeli szolgáltatástól, amelynek meg van adva, vagy bármely Felhőszolgáltatásba nevét az azonos előfizetésben pl., fordított DNS "contosoapp1.cloudapp.net.".
- A fordított DNS FQDN előre eredménye a nevét vagy a Felhőbeli szolgáltatástól, amely meg van adva, illetve bármilyen Felhőszolgáltatásba névre az IP-címének vagy IP ugyanabban az előfizetésben belül például fordított DNS, "app1.contoso.com." Ez a CName alias contosoapp1.cloudapp.net az.

Érvényességi ellenőrzések csak történik, ha egy felhőalapú szolgáltatás a fordított DNS tulajdonsága vagy módosított. Ismétlődő újbóli érvényesítése nem történik.

## <a name="add-reverse-dns-to-existing-cloud-services"></a>Fordított DNS hozzáadása meglévő Cloud Services
Ha egy meglévő felhőalapú szolgáltatásba, a "Set-AzureService" parancsmaggal felveheti a fordított DNS-rekord:

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="create-a-cloud-service-with-reverse-dns"></a>Hozzon létre egy felhőalapú szolgáltatásba fordított DNS-rekordjaival
Egy új Felhőszolgáltatásba a fordított DNS tulajdonság, a "Set-AzureService" parancsmaggal megadott adhat meg:

    PS C:\> New-AzureService –ServiceName “contosoapp1” –Location “West US” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="view-reverse-dns-for-existing-cloud-services"></a>Meglévő Cloud Services fordított DNS megtekintése
Egy meglévő felhőalapú szolgáltatást, a "Get-AzureService" parancsmaggal a beállított értékkel tekintheti meg:

    PS C:\> Get-AzureService "contosoapp1"

## <a name="remove-reverse-dns-from-existing-cloud-services"></a>Meglévő Cloud Services fordított DNS eltávolítása
Egy meglévő felhőalapú szolgáltatást, a "Set-AzureService" parancsmaggal fordított DNS tulajdonság eltávolítható. Ez a fordított DNS tulajdonság értéke blank megadásával történik:

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “”

[AZURE.INCLUDE [FAQ1](../../includes/dns-reverse-dns-record-operations-faq-host-own-arpa-zone-include.md)]

[AZURE.INCLUDE [FAQ2](../../includes/dns-reverse-dns-record-operations-faq-asm-include.md)]

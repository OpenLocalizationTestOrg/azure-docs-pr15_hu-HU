<properties
   pageTitle="Átméretezheti a ACS fürtre az Azure CLI |} Microsoft Azure"
   description="Ha át kívánja méretezni a Azure tároló szolgáltatás fürt az Azure CLI használatával hogyan."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, a tárolók, Micro-szolgáltatások, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/03/2016"
   ms.author="timlt"/>

# <a name="scale-an-azure-container-service"></a>Az Azure tároló szolgáltatásainak méretezése

Az Azure CLI eszközzel rendelkezik az Azure tároló szolgáltatást (ACS) csomópontok számának megjelenítése méretezheti. Használatakor az Azure CLI méretezni, az eszköz térhet vissza, amely a tároló alkalmazott módosítása új konfigurációs fájl.

## <a name="about-the-command"></a>A parancsról

Az Azure CLI meg kommunikálni az Azure tárolók Azure erőforrás-kezelő módban kell lennie. Hívja fel az erőforrás-kezelő mód válthat `azure config mode arm`. A `acs` parancs rendelkezik a gyermek-parancs nevű `scale` , amely tartalmaz egy tároló szolgáltatás skála műveletek. A méret parancs futtatásával használt különféle paraméterek segítséget `azure acs scale --help`, amely kimenet valami ehhez hasonló:

```azurecli
azure acs scale --help

help:    The operation to scale a container service.
help:
help:    Usage: acs scale [options] <resource-group> <name> <new-agent-count>
help:
help:    Options:
help:      -h, --help                               output usage information
help:      -v, --verbose                            use verbose output
help:      -vv                                      more verbose with debug output
help:      --json                                   use json output
help:      -g, --resource-group <resource-group>    resource-group
help:      -n, --name <name>                        name
help:      -o, --new-agent-count <new-agent-count>  New agent count
help:      -s, --subscription <subscription>        The subscription identifier
help:
help:    Current Mode: arm (Azure Resource Management)
```

## <a name="use-the-command-to-scale"></a>Ha át kívánja méretezni a parancs használata

Ha át kívánja méretezni tároló szolgáltatás, először kell, hogy az **erőforráscsoport** és **Azure tároló szolgáltatást (ACS) nevét**, és anyagokkal új számát is megadhatja. Kisebb vagy nagyobb összegű segítségével átméretezheti a kurzor felfelé vagy lefelé.

Előfordulhat, hogy meg szeretné ismerni az egy tároló szolgáltatás ügynökök milyen az aktuális száma, a méretezés előtt. Használja a `azure acs show <resource group> <ACS name>` való visszatéréshez a ACS config parancsot. Megjegyzés: a <mark>darab</mark> eredményét.

#### <a name="see-current-count"></a>Lásd: az aktuális száma

```azurecli
azure acs show containers-test containerservice-containers-test

info:    Executing command acs show
data:
data:     Id                 : /subscriptions/<guid>/resourceGroups/containers-test/providers/Microsoft.ContainerService/containerServices/containerservice-containers-test
data:     Name               : containerservice-containers-test
data:     Type               : Microsoft.ContainerService/ContainerServices
data:     Location           : westus
data:     ProvisioningState  : Succeeded
data:     OrchestratorProfile
data:       OrchestratorType : DCOS
data:     MasterProfile
data:       Count            : 1
data:       DnsPrefix        : myprefixmgmt
data:       Fqdn             : myprefixmgmt.westus.cloudapp.azure.com
data:     AgentPoolProfiles
data:       #0
data:         Name           : agentpools
data:         <mark>Count          : 1</mark>
data:         VmSize         : Standard_D2
data:         DnsPrefix      : myprefixagents
data:         Fqdn           : myprefixagents.westus.cloudapp.azure.com
data:     LinuxProfile
data:       AdminUsername    : azureuser
data:       Ssh
data:         PublicKeys
data:           #0
data:             KeyData    : ssh-rsa <ENCODED VALUE>
data:     DiagnosticsProfile
data:       VmDiagnostics
data:         Enabled        : true
data:         StorageUri     : https://<storageid>.blob.core.windows.net/
```  

#### <a name="scale-to-new-count"></a>Új darabra méretezése

Már valószínűleg magától értetődő, mint a tároló szolgáltatás méretezheti meghívásával `azure acs scale` és az **erőforráscsoport** **ACS nevét**és **ügynök száma**. Amikor a tároló szolgáltatás méretezéséhez az Azure CLI, amely a tároló szolgáltatás, beleértve az új ügynök száma az új konfiguráció JSON karakterláncot ad vissza.

```azurecli
azure acs scale containers-test containerservice-containers-test 10

info:    Executing command acs scale
data:    {
data:        id: '/subscriptions/<guid>/resourceGroups/containers-test/providers/Microsoft.ContainerService/containerServices/containerservice-containers-test',
data:        name: 'containerservice-containers-test',
data:        type: 'Microsoft.ContainerService/ContainerServices',
data:        location: 'westus',
data:        provisioningState: 'Succeeded',
data:        orchestratorProfile: { orchestratorType: 'DCOS' },
data:        masterProfile: {
data:            count: 1,
data:            dnsPrefix: 'myprefixmgmt',
data:            fqdn: 'myprefixmgmt.westus.cloudapp.azure.com'
data:        },
data:        agentPoolProfiles: [
data:            {
data:                name: 'agentpools',
data:                <mark>count: 10</mark>,
data:                vmSize: 'Standard_D2',
data:                dnsPrefix: 'myprefixagents',
data:                fqdn: 'myprefixagents.westus.cloudapp.azure.com'
data:            }
data:        ],
data:        linuxProfile: {
data:            adminUsername: 'azureuser',
data:            ssh: {
data:                publicKeys: [
data:                    { keyData: 'ssh-rsa <ENCODED VALUE>' }
data:                ]
data:            }
data:        },
data:        diagnosticsProfile: {
data:            vmDiagnostics: { enabled: true, storageUri: 'https://<storageid>.blob.core.windows.net/' }
data:        }
data:    }
info:    acs scale command OK
``` 

## <a name="next-steps"></a>Következő lépések

- [A fürtre terjesztése](container-service-deployment.md)
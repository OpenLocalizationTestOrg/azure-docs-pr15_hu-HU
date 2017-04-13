<properties
   pageTitle="A különálló fürt beállítása |} Microsoft Azure"
   description="Ez a cikk ismerteti, hogy miként konfigurálható az önálló vagy magánjellegű szolgáltatás háló fürt."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="dkshir"/>


# <a name="configuration-settings-for-standalone-windows-cluster"></a>Különálló Windows fürt beállításainak konfigurálása

Ez a cikk ismerteti, hogyan konfigurálása a különálló szolgáltatás háló fürtre _**ClusterConfig.JSON**_ fájl használatával. Ez a fájl segítségével a fürt, biztonsági beállításaitól, valamint a hálózati topológiája értelmez hibafa/frissítés tartományok, például a szolgáltatás háló csomópontok és azok IP-címek, csomópontok különböző típusú információkat meg a különálló fürt.

Amikor [a különálló szolgáltatás háló csomag letöltése](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), néhány minták ClusterConfig.JSON fájl töltődnek le a munkahelyi számítógépen. A nevüket mintáknál *DevCluster* segít hozzon létre egy fürt ugyanazon a gépen, például a logikai csomópontok három csomópontjait. Ezeken kívül legalább egy csomópont jelöléssel kell rendelkeznie egy elsődleges csomópontot. A fürt hasznos fejlesztési vagy próba környezetben, és egy gyártási fürt nem támogatott. Hozzon létre egy gyártási minőség fürt minden csomópont másik számítógépre a nevük mintáknál *MultiMachine* segítséget. Az alábbi fürt elsődleges csomópontok számának [megbízhatósági szint](#reliability)alapul.

1. *ClusterConfig.Unsecure.DevCluster.JSON* és *ClusterConfig.Unsecure.MultiMachine.JSON* megjelenítése nem biztonságos próba vagy gyártási fürt rendre létrehozása. 
    
2. *ClusterConfig.Windows.DevCluster.JSON* és *ClusterConfig.Windows.MultiMachine.JSON* a próba- vagy gyártási fürt védelmének beállítása a [Windows](service-fabric-windows-cluster-windows-security.md)biztonsági létrehozásának módját mutatják.

3. *ClusterConfig.X509.DevCluster.JSON* és *ClusterConfig.X509.MultiMachine.JSON* védett használata próba vagy gyártási fürt létrehozásának módját mutatják [X509 tanúsítvány-alapú biztonsági](service-fabric-windows-cluster-x509-security.md). 


Most egy _**ClusterConfig.JSON**_ fájl neve alatti különböző részei megvizsgálja azt.

## <a name="general-cluster-configurations"></a>Általános fürt konfigurációk
Ez lefedi széles fürt bizonyos beállításokat, a JSON kódtöredékének alább látható módon.

    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2015-01-01-alpha",

A szolgáltatás háló fürthöz bármilyen könnyen megjegyezhető nevet a **név** változó hozzárendelésével adhat meg. A **clusterConfigurationVersion** a fürt; verziószáma minden alkalommal, amikor a szolgáltatás háló fürt frissítenie kell növelnie azt. A **apiVersion** azonban hagyja az alapértelmezett értéket.


<a id="clusternodes"></a>
## <a name="nodes-on-the-cluster"></a>A fürt található csomópontok
A csomópontok **csomópontok** csoportjában, az alábbi kódtöredékének látható használatával adhatja meg a szolgáltatás háló fürt.

    "nodes": [{
        "nodeName": "vm0",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType1",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType2",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],

A szolgáltatás háló fürtre tartalmaznia kell legalább 3 csomópontok. Ez a szakasz további csomópontok vehet a telepítő szerint. A következő táblázat ismerteti az egyes csomópontok beállításait.

|**Csomópont konfigurálása**|**Leírás**|
|-----------------------|--------------------------|
|Csomópontnév|Minden olyan rövid nevet adhat a csomópontra.|
|IP-cím|Meg, hogy a csomópont az IP-címének parancs ablak megnyitása, és írnak `ipconfig`. Megjegyzés: a IPv4-címét, és rendelje hozzá az **IP-cím** változó.|
|nodeTypeRef|Az egyes csomópontok egy másik csomópont típusa rendelhetők hozzá. A [csomópont-típusokat](#nodetypes) az alábbi szakaszt definiálva.|
|faultDomain|Hibafa-tartományok fürt rendszergazdák határozza meg, hogy egy időben való megosztott fizikai függőségek miatt előfordulhat, hogy nem fizikai csomópontok engedélyezése.|
|upgradeDomain|Frissítési tartományok beállítása, hogy az szolgáltatás háló frissítésekhez, egy időben kapcsolatos leállítása csomópontok ismertetik. Nem korlátozza a fizikai vonatkozó követelményeket, mely frissítési tartományok hozzárendelése csomópontok adhatja meg.| 


## <a name="cluster-properties"></a>Fürt tulajdonságai

A ClusterConfig.JSON **Tulajdonságok** szakaszában állítsa be az alábbi képlettel történik a fürt szolgál.

<a id="reliability"></a>
### <a name="reliability"></a>Megbízhatósága 
A **reliabilityLevel** szakaszban határozza meg, hogy a rendszer szolgáltatások, amelyek a fürt elsődleges csomóponton futtatását is lehetővé teszi a példányszámot. Ez növeli az alábbi szolgáltatások megbízhatósága és így a fürt. Beállíthatja, hogy ez a változó *bronz*, *ezüst*, *Gold* vagy *platina* az ezek a szolgáltatások 3, 5, 7 vagy 9-es példányainak rendre. Az alábbi példában látható.

    "reliabilityLevel": "Bronze",
    
Ne feledje, hogy egy elsődleges csomópont egy példányát a rendszer szolgáltatások fut, mivel volna legalább 3 elsődleges csomópontok *bronz*és az 5-ös az *ezüst*, 7 *Gold* és a *platina* megbízhatósági szint a 9.


### <a name="diagnostics"></a>Diagnosztikai
A **diagnosticsStore** szakasz engedélyezése a diagnosztikai és hibaelhárítási csomópont vagy fürt hibák, ahogy az alábbi kódtöredékének paraméterek beállítása teszi lehetővé. 

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

A **metaadat-alapú** a fürt diagnosztika leírását, és beállítható, hogy a telepítő szerint. Ezek a változók gyűjt az esemény-nyomkövetés a nyomkövetési naplók, összeomlik kiírása, valamint teljesítmény számláló segítségével. Olvassa el a [Tracelog](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) és az [Esemény-nyomkövetés nyomkövetési](https://msdn.microsoft.com/library/ms751538.aspx) esemény-nyomkövetés a nyomkövetési naplók további információt. Minden naplók [kiírása összeomolhat](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) , és a [Teljesítmény számláló](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) többek között a **connectionString** mappa a számítógépen lévő rendszer érkező forgalmat. A diagnosztikai tárolásához *AzureStorage* is használhatja. Olvassa el az egy minta kódtöredékének.

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a>Biztonsági 
A **Biztonság** szakaszában szükség egy biztonságos különálló szolgáltatás háló fürt. A következő kódtöredékének ebben a részben egy részét jeleníti meg.

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

A **metaadat-alapú** biztonságos fürt leírását, és beállítható, hogy a telepítő szerint. A **ClusterCredentialType** és **ServerCredentialType** határozza meg, amely a fürt és a csomópontok hajtja végre, biztonsági típusát. Bármelyik *X509* tanúsítvány-alapú értékpapír vagy *Windows* -Azure Active Directory-alapú biztonsági beállíthatók. A **Biztonság** szakaszában a többi tekinti meg a biztonsági alapul. Olvassa el a [tanúsítványok-alapú biztonsági önálló fürt](service-fabric-windows-cluster-x509-security.md) vagy a [Windows biztonsági önálló fürt](service-fabric-windows-cluster-windows-security.md) töltse ki a többi **Biztonság** szakaszában olvashat.


<a id="nodetypes"></a>
### <a name="node-types"></a>Csomópont típusai
A **nodeTypes** szakasz ismerteti a csomópontok, amelynek a fürt típusú. Legalább egy csomópont típusa meg kell adni egy fürtre, a kódtöredék alább látható módon. 

    "nodeTypes": [{
        "name": "NodeType0",
        "clientConnectionEndpointPort": "19000",
        "clusterConnectionEndpointPort": "19001",
        "leaseDriverEndpointPort": "19002"
        "serviceConnectionEndpointPort": "19003",
        "httpGatewayEndpointPort": "19080",
        "applicationPorts": {
            "startPort": "20575",
            "endPort": "20605"
        },
        "ephemeralPorts": {
            "startPort": "20606",
            "endPort": "20861"
        },
        "isPrimary": true
    }]

Meg kell adott csomópontra típus rövid nevét a **név** . [Hozzon létre egy adott csomópont típusú csomópontot, hozzárendelése a rövid nevét a **nodeTypeRef** változó csomópontra, mint a fenti.](#clusternodes) Az egyes csomópont: meghatározása a kapcsolat végpontok használt. Mindaddig, amíg ki nem ütköznek bármely más végpontja a fürt bármely portszámot, a következő kapcsolat végpontokhoz adhatja meg. A több elem csomópont fürt egy vagy több elsődleges csomópont (azaz **isPrimary** értéke *Igaz*), attól függően, hogy a [**reliabilityLevel**](#reliability)lesznek. Olvassa el a [szolgáltatás háló fürt kapacitás tervezési szempontjait](service-fabric-cluster-capacity.md) az adatok **nodeTypes** és **reliabilityLevel** értéket, és szeretné, hogy mik az elsődleges és a nem elsődleges csomópontot. 

#### <a name="endpoints-used-to-configure-the-node-types"></a>A csomópont típusok konfigurálása használt végpontokat

- *clientConnectionEndpointPort* az ügyfélprogram API-k használata esetén a fürt, csatlakozhat az ügyfél által használt port. 
- *clusterConnectionEndpointPort* értéke a port, amelynél a csomópontok egymással.
- *leaseDriverEndpointPort* megtudhatja, hogy a csomópontok még aktívak az fürt bérleti illesztőprogram által használt port. 
- *serviceConnectionEndpointPort* használatával kommunikál, hogy az adott csomópont szolgáltatás háló ügyfél, az alkalmazások és szolgáltatások a csomóponton rendszerbe által használt port.
- *httpGatewayEndpointPort* a portot használja a szolgáltatás háló Intéző a fürthöz.
- *ephemeralPorts* [az operációs rendszer által használt dinamikus portok](https://support.microsoft.com/kb/929851). Szolgáltatás háló alkalmazás portok ezek egy részét használja, és a hátralévő az operációs rendszer érhető el. Azt is leképezése ebben a tartományban a meglévő tartomány szerepel az operációs rendszer így minden szempontból is használhatja az abban a JSON mintafájlok tartományokat. Akkor győződjön meg róla, hogy a kezdő és záró portokat közötti különbség legalább 255. 
- *applicationPorts* a szolgáltatás háló alkalmazások által használt portokat is. Ezek a *ephemeralPorts*, elég hogy lefedje a végpontot követelmény az alkalmazás csak egy részhalmazát kell. Szolgáltatás háló fogja használni ezeket, valahányszor új portokat szükségesek, valamint a tűzfalban a következő portokat megnyitásának gondoskodik. 
- *reverseProxyEndpointPort* választható fordított proxykiszolgáló zárólap. További részleteket lásd: a [Szolgáltatás háló fordított proxykiszolgáló](service-fabric-reverseproxy.md) . 


### <a name="other-settings"></a>Egyéb beállítások
A **fabricSettings** szakasz csoportban adhatja meg a legfelső szintű könyvtárak a szolgáltatás háló adatok és a naplókat. Testre szabható ezek csak az első fürt létrehozása során. Olvassa el az e szakasz egy minta kódtöredékének.

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

Ajánlott-az operációs rendszer meghajtó használatával, valamint a FabricDataRoot FabricLogRoot, mivel ez ellen OS további megbízhatóság összeomlik. Figyelje meg, hogy ha csak az adatok legfelső szintű testreszabásához kattintson a napló legfelső szintű kerül legfelső szintű adatok alatt egy szinttel.


## <a name="next-steps"></a>Következő lépések

Ha már vannak beállítva a különálló fürt beállítási szerinti teljes ClusterConfig.JSON fájlt, a fürt a cikk követve telepítheti [létrehozása az Azure Service háló fürt a helyszíni, vagy a felhőben](service-fabric-cluster-creation-for-windows-server.md) majd folytassa [a fürt háló Intézővel megjelenítése](service-fabric-visualizing-your-cluster.md).



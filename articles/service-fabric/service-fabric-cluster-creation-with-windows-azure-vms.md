<properties
   pageTitle="Hozzon létre egy különálló fürt fut a Windows Azure VMs |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre és kezelhet az Azure Service háló fürt Windows Server operációs rendszert futtató Azure virtuális gépeken."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/05/2016"
   ms.author="dkshir;chackdan"/>



# <a name="create-a-three-node-standalone-service-fabric-cluster-with-azure-virtual-machines-running-windows-server"></a>A Windows Server operációs rendszert futtató Azure virtuális gépeken futó három csomópont különálló szolgáltatás háló fürt létrehozása

Ez a cikk ismerteti a Windows-alapú Azure virtuális gépeken (VMs), a különálló szolgáltatás háló installer használatával a Windows Server fürt létrehozása. A speciális eset: az [létrehozása és kezelése a Windows Server operációs rendszerű fürtre](service-fabric-cluster-creation-for-windows-server.md) a VMs esetén [Windows Server operációs rendszert futtató Azure VMs](../virtual-machines/virtual-machines-windows-hero-tutorial.md), de ne hozzon létre [egy Azure felhőalapú szolgáltatás háló fürthöz](service-fabric-cluster-creation-via-portal.md). A különálló szolgáltatás háló fürt hozta létre az alábbi lépésekkel teljesen kezelik az Ön által, felhőalapú szolgáltatás háló fürt felügyelt és a szolgáltatás háló erőforrás szolgáltató frissített közben az Azure különbség.


## <a name="steps-to-create-the-standalone-cluster"></a>A különálló fürt létrehozásának lépései

1. Jelentkezzen be az Azure-portálra, és hozzon létre egy új Windows Server 2012 R2 adatközponthoz virtuális erőforráscsoport. Olvassa el a [Hozzon létre egy Windows virtuális az Azure-portálon](../virtual-machines/virtual-machines-windows-hero-tutorial.md) további információt.
2. Néhány további Windows Server 2012 R2 adatközponthoz VMs hozzá az erőforrás azonos csoportba. Győződjön meg arról, hogy minden a VMs rendszergazdai felhasználónév és a jelszó létrehozásakor. Miután létrehozott meg kell jelennie összes három VMs virtuális ugyanabba a hálózatba.
3. A VMs kapcsolódni, és kapcsolja ki a Windows tűzfal a [Kiszolgálókezelő, a helyi kiszolgáló irányítópult](https://technet.microsoft.com/library/jj134147.aspx)használatával. Ezzel biztosíthatja, hogy a hálózati forgalmat a gépek között kommunikálhat. Minden gépi csatlakozva első megnyitásával egy parancssort, és írja be az IP-cím `ipconfig`. Azt is megteheti megjelenik az egyes IP-címét az Azure-portálon a virtuális hálózati erőforráshoz tartozó erőforráscsoport kiválasztásával.
4. Csatlakozzon a VMs közül, és tesztelése, hogy a másik két VMs sikeresen is ping.
5. Az új mappa a számítógépen a VMs, és [Töltse le a Windows Server különálló szolgáltatás háló csomag](http://go.microsoft.com/fwlink/?LinkId=730690) egyikét csatlakozzon, és csomag kibontása.
6. Nyissa meg a *ClusterConfig.Unsecure.MultiMachine.json* fájlt a Jegyzettömbben, és a gép három IP-címmel minden csomópont szerkesztése. A képernyő tetején fürt nevének módosítása, és mentse a fájlt.  A részleges példa a fürt jegyzék alább látható.

    ```
    {
        "name": "TestCluster",
        "clusterManifestVersion": "1.0.0",
        "apiVersion": "2015-01-01-alpha",
        "nodes": [
        {
            "nodeName": "vm0",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.5",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD0"
        },
        {
            "nodeName": "vm1",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.4",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc2/r0",
            "upgradeDomain": "UD1"
        },
        {
            "nodeName": "vm2",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.6",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc3/r0",
            "upgradeDomain": "UD2"
        }
    ],
    ```

7. Nyissa meg a [PowerShell ISE ablakot](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise). Nyissa meg azt a mappát, amelyben a letöltött önálló installer-csomag kibontása és fürt nyilvánvalóan mentették. Futtassa az alábbi PowerShell-parancsot.

    ```
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab
    ```

8. Meg kell jelennie a PowerShell futtatása, minden számítógép hozhat létre, és a fürtre. Miután körülbelül egy percig, azt is ellenőrizze, hogy a fürt működési való csatlakozással a szolgáltatás háló Intézőt a gép IP-cím egyik pl. használatával `http://10.7.0.5:19080/Explorer/index.html`. Mivel az Azure VMs, használja a könnyebb lesz [az Azure VMs tanúsítványok telepítenie](service-fabric-windows-cluster-x509-security.md) kell vagy [Windows Server Active Directory (AD) vezérlő Windows-hitelesítés](service-fabric-windows-cluster-windows-security.md), ugyanúgy, mint a helyszíni volna végezze el a gép egyik beállítása biztonságos önálló fürt.


## <a name="next-steps"></a>Következő lépések
- [Különálló szolgáltatás háló fürt létrehozása a Windows Server vagy Linux rendszerhez](service-fabric-deploy-anywhere.md)
- [Hozzáadása vagy eltávolítása a csomópontok egy különálló szolgáltatás háló fürthöz](service-fabric-cluster-windows-server-add-remove-nodes.md)
- [Különálló Windows fürt beállításainak konfigurálása](service-fabric-cluster-manifest.md)
- [A Windows biztonsági használata Windows önálló fürt biztonságos](service-fabric-windows-cluster-windows-security.md)
- [Biztonságos X509 használata Windows önálló fürt tanúsítványok](service-fabric-windows-cluster-x509-security.md)

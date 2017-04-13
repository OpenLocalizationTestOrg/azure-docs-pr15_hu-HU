<properties
   pageTitle="Hozzáadása vagy eltávolítása a különálló szolgáltatás háló fürtre csomópontok |} Microsoft Azure"
   description="Megtudhatja, hogyan lehet hozzáadni vagy eltávolítani a csomópontok az Azure Service háló fürthöz fizikai vagy virtuális gépen futó Windows Serveren, amelyek lehetnek a helyszíni, vagy bármely a felhőben."
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
   ms.date="09/20/2016"
   ms.author="dkshir;chackdan"/>


# <a name="add-or-remove-nodes-to-a-standalone-service-fabric-cluster-running-on-windows-server"></a>Hozzáadása vagy eltávolítása a Windows Server operációs rendszerű különálló szolgáltatás háló fürtre csomópontok

Után [a különálló szolgáltatás háló fürt Windows Server gépeken létrehozása](service-fabric-cluster-creation-for-windows-server.md), az üzleti igények változhat, így szükség lehet hozzáadni vagy eltávolítani a fürthöz több csomópontok. Ebben a cikkben a lépések részletes leírását a cél elérését.


## <a name="add-nodes-to-your-cluster"></a>Csomópontok hozzáadása a fürthöz

1. Felkészülés [a gép felel meg a fürt telepítési Előfeltételek Készítsünk](service-fabric-cluster-creation-for-windows-server.md#preparemachines) részben leírt lépésekkel a fürthöz hozzáadni kívánt virtuális/gépi.
2. Tervezze meg, mely hibafa és frissítési tartomány felvétele a virtuális/gép fogja.
3. Távoli asztal (RDP) hozzáadása a fürthöz kívánt virtuális/gépi be.
4. Másolás vagy [Töltse le a Windows Server szolgáltatás háló a különálló csomagot](http://go.microsoft.com/fwlink/?LinkId=730690) a virtuális gépi és bontsa ki a csomagot.
5. A Powershell futtatása rendszergazdaként, és keresse meg a beszúrandó kicsomagolt csomag.
6. Futtassa a *AddNode.ps1* Powershell a paraméterek azt mutatja be, adja hozzá az új csomópontot. Az alábbi példa összeadja VM5, típusa, IP-cím 182.17.34.52 UD1 és FD1 NodeType0 nevű új csomópontot. A *ExistingClusterConnectionEndPoint* a kapcsolat végpontjának egy már meglévő fürthöz a csomópont. Ezen a végponton megadhatja a fürt *csomópont* IP-címét.

```
.\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain FD1 -AcceptEULA

```

## <a name="remove-nodes-from-your-cluster"></a>A fürt csomópontok eltávolítása

1. Attól függően, hogy a választott a fürt Reliablity szint nem távolítható el az első n (3/5/7/9) csomópontok elsődleges csomópont-típus
2. RemoveNode parancs futtatása a fejlesztők fürthöz nem támogatott.
2. Távoli asztal (RDP) el szeretné távolítani a fürt a virtuális/gépi be.
2. Másolás vagy [Töltse le a különálló csomagot, a Windows Server szolgáltatás háló](http://go.microsoft.com/fwlink/?LinkId=730690) és bontsa ki a virtuális gépi csomag.
3. A Powershell futtatása rendszergazdaként, és keresse meg a beszúrandó kicsomagolt csomag.
4. A PowerShell *RemoveNode.ps1* futnak. Az alábbi példában a fürt eltávolítja az aktuális csomópontot. A *ExistingClientConnectionEndpoint* egy ügyfél kapcsolati végpontot bármely csomópontjának, amely a fürt marad. Válassza a fürt az IP-cím és a végpont port *bármely* **más csomópontot** . A **másik csomópont** viszont frissíti a fürt konfigurálása a eltávolított csomópont. 

```
.\RemoveNode.ps1 -ExistingClientConnectionEndpoint 182.17.34.50:19000
```

Felhívjuk a figyelmét arra, hogy követően törölni egy csomópontot, akkor előfordulhat, hogy megjelennek lekérdezések és SFX nyelvét. Ismert hibának, és egy rövidesen kijavítjuk. 


## <a name="next-steps"></a>Következő lépések
- [Különálló Windows fürt beállításainak konfigurálása](service-fabric-cluster-manifest.md)
- [A Windows biztonsági használata Windows önálló fürt biztonságos](service-fabric-windows-cluster-windows-security.md)
- [Biztonságos X509 használata Windows önálló fürt tanúsítványok](service-fabric-windows-cluster-x509-security.md)
- [A Windows operációs rendszert futtató Azure VMs különálló szolgáltatás háló fürt létrehozása](service-fabric-cluster-creation-with-windows-azure-vms.md)

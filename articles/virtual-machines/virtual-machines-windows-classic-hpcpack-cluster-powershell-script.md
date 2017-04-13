<properties
   pageTitle="PowerShell-parancsprogramot telepítése Windows HPC fürt |} Microsoft Azure"
   description="Egy PowerShell-parancsprogramot telepítése a Windows HPC Pack fürtre Azure virtuális gépeken futó futtatása"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-service-management,hpc-pack"/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="big-compute"
   ms.date="07/07/2016"
   ms.author="danlep"/>

# <a name="create-a-windows-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Hozzon létre egy nagy teljesítményű számítások (HPC) fürt Windows a HPC csomag IaaS telepítési parancsfájlt

Futtassa a HPC csomag IaaS példányban telepítése a Windows-munkaterhelésekből Azure virtuális gépeken futó teljes HPC fürt PowerShell-parancsprogramot. A fürt áll egy Windows Server és a Microsoft HPC csomag központi az Active Directory-tartományhoz csomópontra, és további Windows erőforrások adja számítja ki. A Linux munkaterhelésekből az Azure-ban egy HPC csomag fürthöz üzembe, című témakörben olvashat [a HPC csomag IaaS telepítési forgatókönyvvel Linux HPC fürt létrehozása](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md). Az erőforrás-kezelő Azure-sablon segítségével egy HPC csomag fürthöz telepítése. Példák című témakörben talál [egy HPC fürthöz létrehozása](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) és [létrehozása az egyéni HPC fürtre csomópont kép számítja ki](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-files"></a>Példa konfigurációs fájl

Az alábbi példákban helyett a saját azonosító előfizetéshez tartozó értékek vagy a neve és a a fiókkal és a szolgáltatás neve.

### <a name="example-1"></a>Példa 1

A következő konfigurációs fájl üzembe helyezése a egy HPC Pack fürthöz, amelynek a helyi adatbázisok egy központi csomópontot, és öt kiszámítása a Windows Server 2012 R2 operációs rendszert futtató csomópontot. A felhőszolgáltatások közvetlenül a nyugati USA-beli hely jönnek létre. A központi csomópontot, a tartományerdő tartományvezérlőnek működik.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionId>08701940-C02E-452F-B0B1-39D50119F267</SubscriptionId>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%1000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>5</NodeCount>
    <OSVersion>WindowsServer2012R2</OSVersion>
  </ComputeNodes>
</IaaSClusterConfig>
```

### <a name="example-2"></a>Példa 2

A következő konfigurációs fájl egy-egy meglévő tartományerdő HPC csomag fürt üzembe helyezése. A fürt rendelkezik a helyi adatbázisok 1 központi csomópontot, és 12 alkalmazott BGInfo virtuális kiterjesztésű csomópontok számítja ki.
Windows-frissítések automatikus telepítése a tartományerdő összes VMs le van tiltva. A felhőszolgáltatások közvetlenül a kelet-ázsiai helyen jönnek létre. A számítási csomópontok jönnek létre három felhőszolgáltatások és a három tárterület-fiók: _MyHPCCN-0001_ való _MyHPCCN-0005_ _MyHPCCNService01_ és _mycnstorage01_; _MyHPCCN-0006_ való _MyHPCCN0010_ _MyHPCCNService02_ és _mycnstorage02_; és _MyHPCCN-0011_ _MyHPCCN-0012_ _MyHPCCNService03_ és _mycnstorage03_szeretné). A számítási csomópontok létrehozott képből meglévő magánjellegű rögzített számítási csomópontot. Az automatikus a nagyobb, és a kisebb engedélyezve van a szolgáltatás alapértelmezett a nagyobb és intervallumok kisebb.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>MyDCServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>Large</VMSize>
      </DomainController>
     <NoWindowsAutoUpdate />
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
  </Certificates>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0001%</VMNamePattern>
<ServiceNamePattern>MyHPCCNService%01%</ServiceNamePattern>
<MaxNodeCountPerService>5</MaxNodeCountPerService>
<StorageAccountNamePattern>mycnstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>12</NodeCount>
    <ImageName HPCPackInstalled=”true”>MyHPCComputeNodeImage</ImageName>
    <VMExtensions>
       <VMExtension>
          <ExtensionName>BGInfo</ExtensionName>
          <Publisher>Microsoft.Compute</Publisher>
          <Version>1.*</Version>
       </VMExtension>
    </VMExtensions>
  </ComputeNodes>
  <AutoGrowShrink>
    <CertificateId>1</CertificateId>
  </AutoGrowShrink>
</IaaSClusterConfig>

```

### <a name="example-3"></a>Például: 3

A következő konfigurációs fájl egy-egy meglévő tartományerdő HPC csomag fürt üzembe helyezése. A fürt egy központi csomópontot, egy adatbázis-kiszolgáló 500 GB adatot lemezen, 2, a Windows Server 2012 R2 operációs rendszert futtató ügynök csomópontok és a Windows Server 2012 R2 operációs rendszert futtató öt számítási csomópontok tartalmazza. A felhőbeli szolgáltatástól MyHPCCNService affinitás csoportjának *MyIBAffinityGroup*hoz létre, és a felhőbeli szolgáltatások affinitás csoportjának *MyAffinityGroup*készült. A HPC feladat ütemező REST API-val és a HPC webes portál engedélyezve vannak a központi csomópontot.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>    
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>NewRemoteDB</DBOption>
    <DBVersion>SQLServer2014_Enterprise</DBVersion>
    <DBServer>
      <VMName>MyDBServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>ExtraLarge</VMSize>
      <DataDiskSizeInGB>500</DataDiskSizeInGB>
    </DBServer>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>A8</VMSize>
<NodeCount>5</NodeCount>
<AffinityGroup>MyIBAffinityGroup</AffinityGroup>
  </ComputeNodes>
  <BrokerNodes>
    <VMNamePattern>MyHPCBN-%0000%</VMNamePattern>
    <ServiceName>MyHPCBNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>2</NodeCount>
  </BrokerNodes>
</IaaSClusterConfig>
```



### <a name="example-4"></a>Példa 4

A következő konfigurációs fájl egy-egy meglévő tartományerdő HPC csomag fürt üzembe helyezése. A fürt rendelkezik a helyi adatbázisok két fő csomópont két Azure csomópont-sablonok létrehozása és három méret közepes Azure csomópontok alapján készült Azure csomópont sablon _AzureTemplate1_. Parancsfájl fut a központi csomópontot, a fő csomópont konfigurálása után.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
<VMSize>ExtraLarge</VMSize>
    <PostConfigScript>c:\MyHNPostActions.ps1</PostConfigScript>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
    <Certificate>
      <Id>2</Id>
      <PfxFile>d:\mytestcert2.pfx</PfxFile>
    </Certificate>    
  </Certificates>
  <AzureBurst>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate1</TemplateName>
      <SubscriptionId>bb9252ba-831f-4c9d-ae14-9a38e6da8ee4</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc1</ServiceName>
      <StorageAccount>myteststorage1</StorageAccount>
      <NodeCount>3</NodeCount>
      <RoleSize>Medium</RoleSize>
    </AzureNodeTemplate>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate2</TemplateName>
      <SubscriptionId>ad4b9f9f-05f2-4c74-a83f-f2eb73000e0b</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc2</ServiceName>
      <StorageAccount>myteststorage2</StorageAccount>
      <Proxy>
        <UsesStaticProxyCount>false</UsesStaticProxyCount>     
        <ProxyRatio>100</ProxyRatio>
        <ProxyRatioBase>400</ProxyRatioBase>
      </Proxy>
      <OSVersion>WindowsServer2012</OSVersion>
    </AzureNodeTemplate>
  </AzureBurst>
</IaaSClusterConfig>
```

## <a name="troubleshooting"></a>Hibaelhárítás


* **"VNet nem létezik" hibaüzenet** – egyidejűleg egy előfizetés az Azure-ban több fürtre üzembe futtatása esetén egy vagy több telepítések meghiúsulhat azzal a hiba "VNet *VNet\_neve* nem létezik".
Ez a hiba lép fel, futtassa újra a sikertelen telepítéshez a parancsfájl.

* **Probléma a Azure virtuális hálózatról az internethez** – Ha használatával hozza létre a fürtre az egy új tartományvezérlőnek a telepítési parancsfájlt, vagy meg manuálisan tartományvezérlőnek virtuális központi csomópontjának előléptetése, előfordulhat, hogy problémákat tapasztal a VMs csatlakozhat az internethez. Ez a hiba akkor fordulhat elő, ha továbbító DNS-kiszolgáló automatikusan be van állítva a tartományvezérlőnek a, és a továbbítási DNS-kiszolgáló nem oldja meg megfelelően.

    Probléma megoldásához jelentkezzen be a tartományvezérlőnek és vagy eltávolítása a továbbító konfiguráció beállítása, illetve egy érvényes továbbító DNS-kiszolgáló konfigurálása. A Kiszolgálókezelő adja meg ezt a beállítást, kattintson az **eszközök** >
    **DNS** nyissa meg a DNS-kezelőben, és kattintson duplán a **továbbítókat**.

* **Probléma a számítási igényű VMs RDMA hálózati elérése** – Ha hozzáadása a Windows Server számítási vagy ügynök csomópont VMs például A8 vagy A9 egy RDMA internetezésre alkalmas méret funkcióval, problémákat tapasztalhat ezeket VMs a RDMA alkalmazás hálózathoz csatlakozik. Egy Ez a probléma oka az, ha a HpcVmDrivers bővítmény nincs megfelelően telepítve a VMs a fürthöz felvétele után. Ha például a bővítmény előfordulhat, hogy ragadt telepítésének állapotú.

    Ez a probléma megoldásához először a VMs bővítmény állapotának ellenőrzése. Ha a bővítmény telepítése nem megfelelő, távolítsa el a csomópontok eltávolítása a HPC, és a csomópontok újra fel. Ha például a központi csomópontra a Hozzáadás-HpcIaaSNode.ps1 parancsfájl futtatásával számítási csomópont VMs is hozzáadhat.
    
## <a name="next-steps"></a>Következő lépések

* Futtassa a próba terhelést a fürt. Egy példa című témakörben HPC csomag [használatának megkezdését segítő útmutatót](https://technet.microsoft.com/library/jj884144).

* [Első lépések az Excel és a SOA munkaterhelésekből futtatásához Azure-ban egy HPC csomag fürthöz](virtual-machines-windows-excel-cluster-hpcpack.md)talál a fürt példányban parancsfájl, és egy HPC terhelést futtatásához oktatóanyagban.

* Próbálja meg HPC csomag eszközök indítása és leállítása, hozzáadása, és a számítási csomópontok eltávolítása egy fürt hoz létre. Lásd: a [kezelés egy HPC csomag fürthöz Azure-ban található csomópontok számítja ki](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md).

* Beállításához a helyi számítógépről a fürt feladatok küldje el, olvassa el a [elküldése HPC feladatok egy helyszíni számítógépről egy HPC csomag fürthöz Azure-ban](virtual-machines-windows-hpcpack-cluster-submit-jobs.md)című témakört.

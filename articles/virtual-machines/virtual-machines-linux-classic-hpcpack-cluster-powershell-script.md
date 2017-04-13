<properties
   pageTitle="PowerShell-parancsprogramot Linux HPC fürt telepítése |} Microsoft Azure"
   description="Egy PowerShell-parancsprogramot Azure virtuális gépeken futó Linux HPC csomag fürt üzembe futtatása"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-service-management,hpc-pack"/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="big-compute"
   ms.date="07/07/2016"
   ms.author="danlep"/>

# <a name="create-a-linux-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Hozzon létre egy Linux nagy teljesítményű számítások a HPC csomag IaaS telepítési parancsfájlt fürtre (HPC)

Futtassa a HPC csomag IaaS példányban PowerShell-parancsprogramot az Azure virtuális gépeken futó Linux munkaterhelésekből teljes HPC fürt telepítése. A fürt egy Windows Server és a Microsoft HPC csomag központi az Active Directory-tartományhoz csomópontot, és a számítási csomópontját, a Linux terjesztését HPC csomag által támogatott futó áll. A Windows Azure-munkaterhelésekből egy HPC csomag fürt üzembe, című témakörben olvashat [a HPC csomag IaaS telepítési parancsfájlt a Windows HPC fürt létrehozása](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md). Az erőforrás-kezelő Azure-sablon segítségével egy HPC csomag fürthöz telepítése. Egy példa című témakörben [létrehozása egy HPC fürthöz Linux csomópontok számítja ki](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-file"></a>Példa konfigurációs fájl

A következő konfigurációs fájl létrehoz egy új tartományvezérlőnek és tartományerdő és üzembe helyezése egy HPC csomag fürthöz, amely 1 központi csomópontot, a helyi adatbázisok és 10 Linux számítási csomópontját mellett. A felhőszolgáltatások közvetlenül a kelet-ázsiai helyen jönnek létre. A Linux számítási csomópontok 2 felhőszolgáltatások és a 2 tárterület-fiókok (azaz _MyLnxCN-0001_ való _MyLnxCN-0005_ _MyLnxCNService01_ és _mylnxstorage01_), és _MyLnxCN-0006_ való _MyLnxCN-0010_ _MyLnxCNService02_ és _mylnxstorage02_jönnek létre. A számítási csomópontok OpenLogic CentOS 7.0-s verzió Linux képből jönnek létre. 

A saját értékeket az előfizetése nevére, és a fiók és a szolgáltatás neve helyett.

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
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>MyLnxCN-%0001%</VMNamePattern>
    <ServiceNamePattern>MyLnxCNService%01%</ServiceNamePattern>
    <MaxNodeCountPerService>5</MaxNodeCountPerService>
    <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>10</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325 </ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```
## <a name="troubleshooting"></a>Hibaelhárítás

* **"VNet nem létezik" hibaüzenet** – Ha futtatja a HPC csomag IaaS telepítési parancsfájlt egyidejűleg egy előfizetés az Azure-ban több fürtre telepítése egy vagy több telepítések meghiúsulhat azzal a hiba "VNet *VNet\_neve* nem létezik".
Ez a hiba esetén futtassa újra a parancsfájl sikertelen a telepítéshez.

* **Probléma a Azure virtuális hálózatról az internethez** – Ha létrehoz egy HPC csomag fürthöz tartalmazó egy új tartományvezérlőnek a telepítési parancsfájlt használatával, vagy meg manuálisan tartományvezérlőnek virtuális központi csomópontjának előléptetése, előfordulhat, hogy problémákat tapasztal a VMs az Azure virtuális hálózat csatlakozhat az internethez. Ez akkor fordulhat elő, ha egy továbbító DNS-kiszolgáló automatikusan be van állítva a tartományvezérlőnek a, és a továbbítási DNS-kiszolgáló nem oldja meg megfelelően.

    Probléma megoldásához jelentkezzen be a tartományvezérlőnek és vagy eltávolítása a továbbító konfiguráció beállítása, illetve egy érvényes továbbító DNS-kiszolgáló konfigurálása. Ehhez a Kiszolgálókezelő kattintson az **eszközök** >
    **DNS** nyissa meg a DNS-kezelőben, és kattintson duplán a **továbbítókat**.
    
## <a name="next-steps"></a>Következő lépések

* Című [Linux számítási csomópontok HPC csomag fürt Azure-ban – első lépések](virtual-machines-linux-classic-hpcpack-cluster.md) a támogatott Linux terjesztését: adatok áthelyezése és feladatok Linux HPC csomag fürthöz elküldése információt csomópontok számítja ki.
* A parancsfájl használatával fürt létrehozása és futtatása Linux HPC terhelési oktatóanyagokkal talál:
    * [A Microsoft HPC Pack NAMD futtatni Linux számítási csomópontok Azure-ban](virtual-machines-linux-classic-hpcpack-cluster-namd.md)
    * [A Microsoft HPC Pack OpenFOAM futtatni Linux számítási csomópontok Azure-ban](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)
    * [Futtassa a csillag-ÉKM + Microsoft HPC Pack Linux számítja ki a csomópontok Azure-ban](virtual-machines-linux-classic-hpcpack-cluster-starccm.md)

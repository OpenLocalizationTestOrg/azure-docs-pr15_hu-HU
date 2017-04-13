<properties
 pageTitle="Futtassa a csillag-ÉKM + HPC Pack a Linux VMs |} Microsoft Azure"
 description="Azure a Microsoft HPC csomag fürt telepítése és futtatása egy csillag-feladat ÉKM + több Linux az RDMA hálózaton csomópontok számítja ki."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="xpillons"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="09/13/2016"
 ms.author="xpillons"/>

# <a name="run-star-ccm-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Futtassa a csillag-ÉKM + Microsoft HPC Pack egy Linux RDMA a fürt Azure-ban
Ez a cikk bemutatja, hogyan bevezetését tervezi a Microsoft HPC csomag fürtre Azure és a Futtatás egy [CD-adapco csillag-ÉKM +](http://www.cd-adapco.com/products/star-ccm%C2%AE) több csomóponton Linux számítási InfiniBand az összekapcsolt feladatot.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

A Microsoft HPC csomag számos nagyméretű HPC és a párhuzamos alkalmazásai, köztük a MPI alkalmazások, a Microsoft Azure virtuális gépeken futó meghatározására futtatásához szolgáltatásokat nyújt. HPC csomag futó Linux HPC alkalmazást is támogatja a Linux számítási-csomópont VMs HPC csomag fürt telepített. Egy Linux használata – bevezetés HPC Pack csomópontok kiszámítania című témakör [Linux számítási csomópontok HPC csomag fürt Azure-ban – első lépések](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="set-up-an-hpc-pack-cluster"></a>Egy HPC csomag fürthöz beállítása
Töltse le a HPC Pack IaaS telepítési parancsfájlok a [Letöltőközpontból](https://www.microsoft.com/en-us/download/details.aspx?id=44949) , és helyileg bontsa ki.

Azure PowerShell rendszer. Ha PowerShell nincs beállítva a helyi számítógépen, olvassa el a cikk [telepítése és konfigurálása az Azure PowerShell](../powershell-install-configure.md).

Az írás időben (mely tartalmazza az InfiniBand illesztőprogramok az Azure) Azure piactérről a Linux képek valók SLES 12 CentOS 6.5 és CentOS 7.1. Ez a cikk a SLES 12 használatát alapul. Az összes Linux kép, amely támogatja a piactér HPC neve beolvasásához, futtathatja az alábbi PowerShell-parancsot:

```
    get-azurevmimage | ?{$_.ImageName.Contains("hpc") -and $_.OS -eq "Linux" }
```

A kimenet sorolja fel, a helyet, ahol a képek érhetők el, és a kép neve nevet (**ImageName**) a telepítési sablonban később használandó.

Mielőtt beállítaná a fürt, hogy egy HPC csomag telepítési sablonfájlt összeállítása. Azt használja a kis fürtre célba juttatása, mert a központi csomópontot a tartományvezérlőnek lennie, és egy helyi SQL-adatbázist.

A következő sablon üzembe központi csomópontot, **MyCluster.xml**nevű XML-fájl létrehozása, és cserélje ki az értékeket **SubscriptionId**, **StorageAccount**, **hely**, **VMName**és **ServiceName** Öné.

    <?xml version="1.0" encoding="utf-8" ?>
    <IaaSClusterConfig>
      <Subscription>
        <SubscriptionId>99999999-9999-9999-9999-999999999999</SubscriptionId>
        <StorageAccount>mystorageaccount</StorageAccount>
      </Subscription>
      <Location>North Europe</Location>
      <VNet>
        <VNetName>hpcvnetne</VNetName>
        <SubnetName>subnet-hpc</SubnetName>
      </VNet>
      <Domain>
        <DCOption>HeadNodeAsDC</DCOption>
        <DomainFQDN>hpc.local</DomainFQDN>
      </Domain>
      <Database>
        <DBOption>LocalDB</DBOption>
      </Database>
      <HeadNode>
        <VMName>myhpchn</VMName>
        <ServiceName>myhpchn</ServiceName>
        <VMSize>Standard_D4</VMSize>
      </HeadNode>
      <LinuxComputeNodes>
        <VMNamePattern>lnxcn-%0001%</VMNamePattern>
        <ServiceNamePattern>mylnxcn%01%</ServiceNamePattern>
        <MaxNodeCountPerService>20</MaxNodeCountPerService>
        <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
        <VMSize>A9</VMSize>
        <NodeCount>0</NodeCount>
        <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
      </LinuxComputeNodes>
    </IaaSClusterConfig>

Először a címsor-csomópont létrehozását a PowerShell-parancsot futtató rendszergazda jogú parancssort:

```
    .\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml
```

20 – 30 perc múlva kész kell a központi csomópontot. Akkor csatlakozhat, az Azure portálról a virtuális gép a **Csatlakozás** ikonjára kattint.

Lehet, végül, ha a DNS-továbbító megoldása. Ha igen, indítsa el a DNS-kezelőben.

1.  Kattintson a jobb gombbal a kiszolgáló nevét a DNS-kezelőben, válassza a **Tulajdonságok parancsot**, majd a **továbbítókat** fülre.

2.  A **Szerkesztés** gombra kattintva bármelyik továbbítókat eltávolítása, és kattintson **az OK**gombra.

3.  Győződjön meg arról, hogy van-e jelölve a **legfelső szintű javaslatok használata, ha nincs továbbítókat áll rendelkezésre** jelölőnégyzetet, és kattintson **az OK**gombra.

## <a name="set-up-linux-compute-nodes"></a>Linux számítási csomópontok beállítása
A fő csomópont létrehozásához használt ugyanazon telepítési sablon használatával Linux számítási csomópontok rendszerbe.

A fájl **MyCluster.xml** másolja a helyi számítógép zónában a központi csomópontra, és frissítse a **NodeCount** címke telepítéshez használni kívánt csomópontok számának (< = 20). Ügyeljen arra, hogy van elegendő elérhető magmintákat az Azure kvóta mivel minden A9 példány felhasználja 16 magmintákat előfizetéséhez. Helyett A9 A8 példányok (8 magmintákat) is használhatja, ha ugyanazt a költségvetést további VMs használni szeretne.

A központi csomópontot másolja a vágólapra a HPC csomag IaaS telepítési parancsfájlok.

A következő Azure PowerShell parancsokat a rendszergazda jogú parancssort:

1.  Futtassa a **Hozzáadás-AzureAccount** csatlakozhat az Azure előfizetés.

2.  Ha több előfizetéssel rendelkezik, futtassa a **Get-AzureSubscription** felsoroló őket.

3.  Alapértelmezett előfizetés beállítása futtatásával a **kijelölése-AzureSubscription - SubscriptionName xxxx-alapértelmezett** parancsot.

4.  Futtatása **.\New-HPCIaaSCluster.ps1 - ConfigFile MyCluster.xml** üzembe helyezése indításához Linux csomópontok számítja ki.

    ![Címsor-csomópont telepítési működésben][hndeploy]

Nyissa meg a HPC csomag kezelő eszközt. Néhány perc múlva Linux számítási csomópontok rendszeresen megjelennek fürt számítási csomópontok listája. A klasszikus telepítési mód IaaS VMs létrehozott egymás után. Így csomópontok számának fontos, ha az első őket az összes telepített is időt igénybe vehet egy jelentős.

![Linux csomópontok HPC csomag fürt Manager][clustermanager]

Most, hogy az összes csomópontok lépéseket a fürt, vannak infrastruktúrájának bővítéseit beállításokat, hogy.

## <a name="set-up-an-azure-file-share-for-windows-and-linux-nodes"></a>A Windows és Linux csomópontok Azure fájlmegosztás beállítása
A fájl Azure szolgáltatás parancsfájlok, alkalmazáscsomagok és adatfájlok tárolására is használhatja. Azure fájl fölött Azure Blob-tárolóhoz CIFS funkciókat tartalmaz, egy állandó tároló. Ügyeljen arra, hogy ez nem a legtöbb méretezhető megoldást, de a legegyszerűbb egyetlen és használatához nincs szükség külön VMs.

Hozzon létre egy Azure fájlmegosztás az [első lépések a Windows Azure fájltároló](..\storage\storage-dotnet-how-to-use-files.md)cikk utasításait követve.

Tárterület-fiókja nevét megtartása **saname**, a fájl megosztása neveként **megosztásnév**és a tárhely fiókkulcs, mint **sakey**.

### <a name="mount-the-azure-file-share-on-the-head-node"></a>A központi csomópontot a Azure fájlmegosztás csatlakoztatása
Nyissa meg a rendszergazda jogú parancssort, és futtassa a következő parancsot a helyi számítógép zónában tárolóból elemre a hitelesítő adatok tárolására:

```
    cmdkey /add:<saname>.file.core.windows.net /user:<saname> /pass:<sakey>
```

Ezután az Azure fájlmegosztás csatlakoztatásához futtatása:

```
    net use Z: \\<saname>.file.core.windows.net\<sharename> /persistent:yes
```

### <a name="mount-the-azure-file-share-on-linux-compute-nodes"></a>Azure fájlmegosztás Linux számítási csomópontok csatlakoztatása
Egy hasznos HPC csomag épített eszköze a clusrun eszközt. A parancssori eszközzel meg számítási csomópontok egyidejű futtassa az azonos parancsot. Ebben az esetben használatos csatlakoztatásához az Azure fájlmegosztás, és továbbra is fennáll, akkor is tovább újraindul.
A rendszergazda jogú parancssort a központi csomópontra a következő parancsokat.

A csatlakozási könyvtár létrehozása:

```
    clusrun /nodegroup:LinuxNodes mkdir -p /hpcdata
```

Csatlakoztassa az Azure fájlmegosztás:

```
    clusrun /nodegroup:LinuxNodes mount -t cifs //<saname>.file.core.windows.net/<sharename> /hpcdata -o vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777
```

A csatlakozási megosztás is:

```
    clusrun /nodegroup:LinuxNodes "echo //<saname>.file.core.windows.net/<sharename> /hpcdata cifs vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777 >> /etc/fstab"
```

## <a name="install-star-ccm"></a>Telepítse a csillag-ÉKM +
Azure virtuális példányok A8 és A9 InfiniBand támogatási és RDMA funkciók biztosítanak. A Windows Server 2012 R2, SUSE 12, CentOS 6.5 és CentOS 7.1 képeket a Microsoft Azure piactéren található a kernel illesztőprogramok, amelyekben ezek a funkciók érhetők el. A Microsoft MPI és az Intel MPI (5.x kiadás) lehet a két MPI, amelyek támogatják ezeket illesztőprogramok Azure-ban.

CD-adapco csillag-ÉKM + 11.x engedje fel, és később Intel MPI verzióval van kapcsolt 5.x, így Azure InfiniBand támogatása megtalálható.

Ismerkedés a Linux64 csillag-ÉKM + csomag a [CD-adapco portálon](https://steve.cd-adapco.com). Ebben az esetben azt vegyes pontosság 11.02.010 verzióját használja.

A központi csomópontot a **/hpcdata** Azure fájlmegosztás, hozzon létre egy névvel ellátott **setupstarccm.sh** a következő tartalommal parancsprogram. Ezt a csillag beállítása minden számítási csomóponton parancsfájlt-ÉKM + helyi meghajtóra.

#### <a name="sample-setupstarcmsh-script"></a>Setupstarcm.sh mintaparancsfájl
```
    #!/bin/bash
    # setupstarcm.sh to set up STAR-CCM+ locally

    # Create the CD-adapco main directory
    mkdir -p /opt/CD-adapco

    # Copy the STAR-CCM package from the file share to the local directory
    cp /hpcdata/StarCCM/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz /opt/CD-adapco/

    # Extract the package
    tar -xzf /opt/CD-adapco/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz -C /opt/CD-adapco/

    # Start a silent installation of STAR-CCM without the FLEXlm component
    /opt/CD-adapco/starccm+_11.02.010/STAR-CCM+11.02.010_01_linux-x86_64-2.5_gnu4.8.bin -i silent -DCOMPUTE_NODE=true -DNODOC=true -DINSTALLFLEX=false

    # Update memory limits
    echo "*               hard    memlock         unlimited" >> /etc/security/limits.conf
    echo "*               soft    memlock         unlimited" >> /etc/security/limits.conf
```
Most, állíthat be csillag-ÉKM + összes a Linux kiszámítania csomópontok, nyissa meg a rendszergazda jogú parancssort és futtassa az alábbi parancsot:

```
    clusrun /nodegroup:LinuxNodes bash /hpcdata/setupstarccm.sh
```

A parancs futása közben processzorhasználata figyelheti a hőtérkép fürt-kezelő használatával. Néhány perc múlva csomópontjait megfelelően létre kell hozni.

## <a name="run-star-ccm-jobs"></a>Futtassa a csillag-ÉKM + feladatok
HPC csomag a feladat ütemező képességei csillag futtatásához használt – ÉKM + feladatokat. Ehhez szükséges használatosak elindítani a feladatot, és futtassa a csillag néhány parancsprogramok támogatása – ÉKM +. A bemeneti adatok legyen az Azure fájlmegosztáson első az egyszerűség.

Az alábbi PowerShell parancsprogramot egy csillag sorba használják-ÉKM + feladatot. Három argumentum vesz igénybe:

*  A modell neve

*  A csomópontok használandó száma

*  Minden csomóponton használandó magmintákat száma

Mivel a csillag-ÉKM + kitöltheti a memória sávszélességet, akkor célszerűbb általában egy számítási csomópontok kevesebb magmintákat használja, és adja hozzá az új csomópontok. A pontos szám egyes csomópontok processzoronkénti a processzor család és interconnect sebességétől függ.

A csomópontok kizárólag a feladathoz van rendelve, és nem osztható meg a többi feladatokat. A feladat nem indult el egy MPI feladat közvetlenül. A **runstarccm.sh** parancsprogram megkezdi a MPI megnyitó ikonra.

A beviteli modell és a **runstarccm.sh** parancsfájl korábban csatlakoztatott **/hpcdata** megosztása tárolja.

A feladat azonosítójú elnevezése naplófájlok és a Fájlsablonok tárolási helye a **/hpcdata megosztani**, a csillag együtt – ÉKM + kimeneti fájlokat.


#### <a name="sample-submitstarccmjobps1-script"></a>SubmitStarccmJob.ps1 mintaparancsfájl
```
    Add-PSSnapin Microsoft.HPC -ErrorAction silentlycontinue
    $scheduler="headnodename"
    $modelName=$args[0]
    $nbCoresPerNode=$args[2]
    $nbNodes=$args[1]

    #---------------------------------------------------------------------------------------------------------
    # Create a new job; this will give us the job ID that's used to identify the name of the uploaded package in Azure
    #
    $job = New-HpcJob -Name "$modelName $nbNodes $nbCoresPerNode" -Scheduler $scheduler -NumNodes $nbNodes -NodeGroups "LinuxNodes" -FailOnTaskFailure $true -Exclusive $true
    $jobId = [String]$job.Id

    #---------------------------------------------------------------------------------------------------------
    # Submit the job    
    $workdir =  "/hpcdata"
    $execName = "$nbCoresPerNode runner.java $modelName.sim"

    $job | Add-HpcTask -Scheduler $scheduler -Name "Compute" -stdout "$jobId.log" -stderr "$jobId.err" -Rerunnable $false -NumNodes $nbNodes -Command "runstarccm.sh $execName" -WorkDir "$workdir"


    Submit-HpcJob -Job $job -Scheduler $scheduler
```
**Runner.java** cserélje ki a használni kívánt csillag-ÉKM + Java modell megnyitó ikonja és a naplózás kódot.

#### <a name="sample-runstarccmsh-script"></a>Runstarccm.sh mintaparancsfájl
```
    #!/bin/bash
    echo "start"
    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    echo ${SCRIPT_PATH}
    # Set the mpirun runtime environment
    export CDLMD_LICENSE_FILE=1999@flex.cd-adapco.com

    # mpirun command
    STARCCM=/opt/CD-adapco/STAR-CCM+11.02.010/star/bin/starccm+

    # Get node information from ENVs
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    NBCORESPERNODE=$1

    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$
    echo ${NODELIST_PATH}

    # Get every node name and write into the hostfile file
    I=1
    NBNODES=0
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
        let "NBNODES=${NBNODES}+1"
    done
    let "NBCORES=${NBNODES}*${NBCORESPERNODE}"

    # Run STAR-CCM with the hostfile argument
    #  
    ${STARCCM} -np ${NBCORES} -machinefile ${NODELIST_PATH} \
        -power -podkey "<yourkey>" -rsh ssh \
        -mpi intel -fabric UDAPL -cpubind bandwidth,v \
        -mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0" \
        -batch $2 $3
    RTNSTS=$?
    rm -f ${NODELIST_PATH}

    exit ${RTNSTS}
```

A feltétel egy igény szerint kiemelt licenc token azt használja. Adott jogkivonat, meg kell állítania a **$CDLMD_LICENSE_FILE** környezeti változó **1999@flex.cd-adapco.com** és a kulcsot a parancssorból **- podkey** lehetőséget.

Néhány indítás után a parancsfájl olvas – a **$CCP_NODES_CORES** környezeti változók beállított HPC csomag – össze egy hostfile a MPI megnyitó használó csomópontok listáját. A hostfile soronként egy nevet a projekt használt számítási csomópont nevek listáját tartalmazza.

**$CCP_NODES_CORES** formátumának követi a minta:

```
<Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
```

Ha:

* `<Number of nodes>`a feladat rendelt csomópontok számának van.

* `<Name of node_n_...>`az egyes rendelt a projekt csomópontjának nevét.

* `<Cores of node_n_...>`a csomóponton a projekthez hozzárendelt magmintákat számát jelenti.

Magmintákat (**$NBCORES**) számát is számított (**$NBNODES**) csomópontok számának és az egyes ( **$NBCORESPERNODE**paraméterrel megadott) csomópontok magmintákat száma alapján.

Az MPI lehetőségekről az Intel MPI Azure a felhasznált tartalmazza:

*   `-mpi intel`Intel MPI megadásához.

*   `-fabric UDAPL`Azure InfiniBand műveletek használni.

*   `-cpubind bandwidth,v`sávszélességre optimalizálhatja az MPI a csillag-ÉKM +.

*   `-mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0"`Intel MPI Azure InfiniBand dolgozni, és a szükséges számú egyes csomópontok magmintákat beállítása.

*   `-batch`CSILLAG indításához-ÉKM + köteg módban nincs felhasználói felület.


Végezetül szeretné kezdeni a feladatot, ellenőrizze, hogy, hogy a csomópontok lépéseket, és a kezelő online. A PowerShell parancssorból, futtassa a:

```
    .\ SubmitStarccmJob.ps1 <model> <nbNodes> <nbCoresPerNode>
```

## <a name="stop-nodes"></a>Csomópontok leállítása
Később a Miután befejezte a azt vizsgálja, használhatja a következő HPC csomag PowerShell-parancsokkal leállítása és elindítása a csomópontok:

```
    Stop-HPCIaaSNode.ps1 -Name <prefix>-00*
    Start-HPCIaaSNode.ps1 -Name <prefix>-00*
```

## <a name="next-steps"></a>Következő lépések
Próbálja meg más Linux munkaterhelésekből futtatni. Ha például jelenik meg:

* [A Microsoft HPC csomag NAMD futtatni Linux számítási csomópontok Azure-ban](virtual-machines-linux-classic-hpcpack-cluster-namd.md)

* [Microsoft HPC csomag OpenFOAM futtassa a Linux RDMA fürthöz Azure-ban](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)


<!--Image references-->
[hndeploy]: ./media/virtual-machines-linux-classic-hpcpack-cluster-starccm/hndeploy.png
[clustermanager]: ./media/virtual-machines-linux-classic-hpcpack-cluster-starccm/ClusterManager.png

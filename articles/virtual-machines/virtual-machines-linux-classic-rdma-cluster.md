<properties
 pageTitle="Linux RDMA fürt MPI alkalmazások futtatásához |} Microsoft Azure"
 description="Méretű H16r, H16mr, A8 vagy A9 VMs MPI alkalmazások futtatásához a Azure RDMA hálózati használandó Linux fürt létrehozása"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="set-up-a-linux-rdma-cluster-to-run-mpi-applications"></a>MPI alkalmazások futtatásához a Linux RDMA fürtre beállítása


Megtudhatja, hogy miként állíthatja be a Linux RDMA fürtre [H – vagy számítási igényű A-sorozat VMs](virtual-machines-linux-a8-a9-a10-a11-specs.md) az Azure-ban a párhuzamos üzenet továbbítása felület (MPI) alkalmazások futtatásához. Ez a cikk lépéseit a Felkészülés a fürthöz Intel MPI futtatásához Linux HPC képként. Ezután az ezen a képen és a RDMA internetezésre alkalmas Azure virtuális méretű (jelenleg H16r, H16mr, A8 vagy A9) egy VMs fürt rendszerbe. A fürt üzletfelekkel fölé egy kis késés MPI alkalmazások futtatásához használt, távoli közvetlen memória-hozzáférési (RDMA) technológia alapján magas átviteli hálózati.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="cluster-deployment-options"></a>Fürt telepítési lehetőségek

Az alábbiakban módszerek Linux RDMA fürt létrehozása vagy egy feladat ütemező anélkül is használhatja.


* **Azure CLI parancsfájlok** - később látható módon ebben a cikkben segítségével [Azure parancssori felület](../xplat-cli-install.md) (CLI) parancsfájl RDMA internetezésre alkalmas VMs fürt telepítését. A CLI Szolgáltatáskezelés módban a fürt csomópontok sorozatban hoz létre a klasszikus telepítési modell üzembe helyezése a sok számítási csomópont eltarthat néhány percig. Ha engedélyezni szeretné a RDMA hálózati kapcsolat a klasszikus telepítési modell használatakor, telepítse a VMs ugyanazt a felhőalapú szolgáltatást a.

* **Erőforrás-kezelő azure sablonok** - telepítéshez használni a RDMA hálózathoz kapcsolódik RDMA internetezésre alkalmas VMs fürt is használhatja az erőforrás-kezelő telepítési modell. [Saját sablon létrehozásához](../resource-group-authoring-templates.md), vagy jelölje be a [sablonok Azure quickstart útmutató](https://azure.microsoft.com/documentation/templates/) a sablonok között befizetett Microsoft, mind a Közösség a kívánt megoldás. Megadhatja, hogy az erőforrás-kezelő sablonok Linux fürt üzembe gyors és megbízható lehetőséget. Ha engedélyezni szeretné a RDMA hálózati kapcsolat az erőforrás-kezelő telepítési modell használatakor, telepítse az elérhetőség ugyanazok a VMs.

* **HPC csomag** - Microsoft HPC csomag fürt létrehozása az Azure-ban, és futtassa a RDMA hálózat eléréséhez támogatott Linux eloszlás RDMA internetezésre alkalmas számítási csomópontok hozzáadása. [Első lépések Linux számítási csomópontok HPC Pack fürt Azure-ban](virtual-machines-linux-classic-hpcpack-cluster.md)című témakör tartalmaz.

## <a name="sample-deployment-steps-in-classic-model"></a>Minta telepítési lépéseket Klasszikus modell

A következő lépések bemutatják, hogyan az Azure CLI segítségével SUSE Linux vállalati kiszolgáló (SLES) 12 SP1 HPC virtuális a a Microsoft Azure piactéren lévő telepíthető, testre szabhatja és egyéni virtuális kép készítése. Ezután a kép segítségével parancsfájl RDMA internetezésre alkalmas VMs fürt telepítését. 

>[AZURE.TIP]Kövesse a lépéseket hasonló terjesztése más támogatott HPC képeket a Microsoft Azure piactéren található alapján RDMA internetezésre alkalmas VMs fürt. Néhány lépés a amint némileg eltérhetnek. Intel MPI például részét képező és beállítva a csak néhány ezeket a képeket. És ha telepít egy SLES 12 HPC virtuális helyett egy SLES 12 SP1 HPC virtuális, módosítania kell a RDMA illesztőprogramok. Részletekért olvassa el [a A8, A9, A10, és A11 számítási igényű példányok](virtual-machines-linux-a8-a9-a10-a11-specs.md#rdma-driver-updates-for-sles-12).


### <a name="prerequisites"></a>Előfeltételek

*   **Ügyfélszámítógép** - szüksége egy Mac, Linux vagy Windows-alapú ügyfél számítógép Azure kommunikáljon. Ezek a lépések feltételezik, hogy egy Linux ügyfelet használ.

*   **Azure előfizetés** – Ha nincs telepítve az előfizetést, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/) az mindössze néhány perc. Nagyobb fürt érdemes megfontolni befizetések előfizetés vagy vételi lehetőségeket. 

*   **Virtuális méret elérhetősége** – alkalmas RDMA jelenleg az alábbi példány méretek: H16r, H16mr, A8 és A9. Jelölje be a [termékek terület áll rendelkezésre](https://azure.microsoft.com/regions/services/) az elérhetőség Azure régióban. 

*   **Magmintákat kvóta** - szükség lehet a számítási igényű VMs fürt üzembe magmintákat kvóta növeléséhez. Például legalább 128 magmintákat kell, ha szeretné telepíteni 8 A9 VMs, a jelen cikkben látható módon. Az előfizetés is előfordulhat, hogy az egyes virtuális méretét családok, beleértve a H sorozat terjeszthet magmintákat számának korlátozása. Kérhet kvóta növeléséhez a költség nélkül [Nyissa meg az online ügyfél-támogatási kérelmet](../azure-supportability/how-to-create-azure-support-request.md) . 

*   **Azure CLI** - [telepítse](../xplat-cli-install.md) az Azure CLI, és [az Azure előfizetés csatlakoztatása](../xplat-cli-connect.md) az ügyfélgépen.


### <a name="step-1-provision-a-sles-12-sp1-hpc-vm"></a>Lépés: 1. Egy SLES 12 SP1 HPC virtuális kiépítése

Futtassa az Azure CLI az Azure való bejelentkezés után `azure config list` , hogy a kimenet látható Szolgáltatáskezelés mód megerősítéséhez. Ha nem, ez a parancs futtatásával módjának beállítása:


    azure config mode asm


Írja be a következő használatára jogosult összes előfizetések listáját:


    azure account list

Az aktuális aktív előfizetés-es azonosítható `Current` beállítása `true`. Ha az előfizetés nem a fürt létrehozásához használni kívánt azzal, az aktív előfizetés állítsa be a megfelelő előfizetés azonosítója:

    azure account set <subscription-Id>

Képeket szeretne látni a nyilvánosan elérhető SLES 12 SP1 HPC Azure-ban, az alábbihoz hasonló parancs futtatása, feltéve, hogy a rendszerhéj környezet támogatja **grep**:


    azure vm image list | grep "suse.*hpc"

Most már kiépítése egy RDMA internetezésre alkalmas virtuális SLES 12 SP1 HPC képével hasonlóan, mint a következő parancs futtatásával:

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824

Ha

* A méret (ebben a példában A9), a RDMA internetezésre alkalmas virtuális méretek közül.

* A külső SSH portszámot (ebben a példában az SSH alapértelmezett 22) bármely érvényes portszámot. A belső SSH portszámot 22 értékre van állítva.

* Egy új felhőszolgáltatásba az Azure régió, a hely által megadott jön létre. Adja meg azt a helyet, ahol kiválaszthatja, hogy virtuális méretének elérhető.

* A SLES 12 SP1 kép neve jelenleg lehet `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824` vagy `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-priority-v20160824` SUSE prioritás ügyfélszolgálatával (további tarifa szerint).

### <a name="step-2-customize-the-vm"></a>Lépés: 2. A virtuális testreszabása

Miután a virtuális befejeződik, hogy kiépítési, SSH a virtuális gép a a virtuális külső IP-cím (vagy a DNS-nevet), és a külső port szám, akkor beállítva, és testre szabhatja. További információ a kapcsolat megtudhatja, [hogy miként jelentkezzen be a virtuális gépen futó Linux rendszerhez](virtual-machines-linux-mac-create-ssh-keys.md). Hajthat végre parancsokat a virtuális, konfigurált felhasználóként, kivéve, ha a legfelső szintű hozzáférés lépés befejezéséhez szükséges.

>[AZURE.IMPORTANT]Microsoft Azure nem rendszergazdai hozzáférés biztosítása Linux VMs. Rendszergazdai hozzáféréssel, amikor csatlakozik a virtuális felhasználóként eléréséhez parancsokat használatával `sudo`.

* **Frissítések** - frissítések telepítése a **zypper**használatával. Érdemes azt is, NFS segédprogramok telepítéséhez. 

    >[AZURE.IMPORTANT]Egy SLES 12 SP1 HPC virtuális gép azt javasoljuk, hogy problémákat okozhatják, a Linux RDMA illesztőprogramok kernelfrissítéseket nem alkalmazása.

* **Intel MPI** – Intel MPI a SLES 12 SP1 HPC virtuális kattintson a telepítés befejezéséhez a következő parancs futtatásával:

        sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm

* **Zárolás memória** - MPI tartozó kódok zárolása a rendelkezésre álló memória RDMA, hozzáadása vagy a /etc/security/limits.conf fájl az alábbi beállítások módosíthatók. (Legfelső szintű hozzáférés szükséges a fájl szerkesztéséhez.) 

    ```
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

    >[AZURE.NOTE]Teszteléshez is beállíthatja, hogy memlock korlátlan. Példa: `<User or group name>    hard    memlock unlimited`. További tudnivalókért olvassa el a [Beállítás zárolt memóriaméret ismert legjobb módszerek](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size)című témakört.

* **SLES VMs SSH billentyűparancsok** - készítése SSH kulcsok megbízhatóvá a felhasználói fiók számítási csomópontjai között a SLES fürt MPI feladatok futtatásakor. (Ha egy CentOS-alapú HPC virtuális telepítette, ne kövesse ezt a lépést. Lásd: állíthat be passwordless SSH adatvédelmi fürt csomópontjai között, készítsen egy képet, és a fürt telepítése után a cikk későbbi részében utasításokat.) 

    Futtassa a következő SSH kulcsok létrehozása parancsot. Ha megjelenik egy kérdés beviteli, és nyomja le az Enter billentyűt a billentyűk készítése az alapértelmezett helyre a jelszó beállítása nélkül.

        ssh-keygen

    A nyilvános kulcs hozzáfűzése ismert nyilvános kulcs authorized_keys fájlt.

        cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

    A ~/.ssh címtárban szerkesztése vagy a "config" fájlt. Adja meg az IP-címtartományokat a (ebben a példában 10.32.0.0/16) Azure-ban használni kívánt magánhálózat:

        host 10.32.0.*
        StrictHostKeyChecking no

    Azt is megteheti a lista egyes a fürt virtuális magánhálózaton IP-címét az alábbi képlettel történik:

    ```
    host 10.32.0.1
     StrictHostKeyChecking no
    host 10.32.0.2
     StrictHostKeyChecking no
    host 10.32.0.3
     StrictHostKeyChecking no
    ```

    >[AZURE.NOTE]Konfigurálása `StrictHostKeyChecking no` biztonsági kockázatot hozhat létre, ha egy adott IP-cím vagy tartomány nincs megadva.

* **Alkalmazások** – telepítése előtt, készítsen egy képet, hajtsa végre a további változtatásokat vagy a virtuális a szükséges alkalmazásokat.

### <a name="step-3-capture-the-image"></a>3 a lépést. Készítsen egy képet

Készítsen egy képet, először futtassa a következő parancsot a Linux virtuális a. Ez a parancs a virtuális deprovisions, de a felhasználói fiókok és SSH kulcsok beállított fenntartja.

```
sudo waagent -deprovision
```

Ezután az ügyfélszámítógép a következő parancsokat Azure CLI készítsen egy képet. Megtudhatja, [hogy miként rögzítheti a klasszikus Linux virtuális gép képként](virtual-machines-linux-classic-capture-image.md) további információt.  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

Ezek a parancsok futtatása után a virtuális kép rögzíthető a használatra, és a virtuális törlődik. Most már az egyéni kép fürt telepíthető van.

### <a name="step-4-deploy-a-cluster-with-the-image"></a>Lépés: 4. A képpel egy fürt terjesztése

Módosítsa a következő Bash parancsfájl környezetben megfelelő értékekkel, és futtassa az ügyfélszámítógép. Azure üzembe helyezése a sorozatban a klasszikus telepítési modellben VMs, mert a 8 A9 VMs, javasolt a parancsfájl üzembe néhány percet vesz igénybe.

```
#!/bin/bash -x
# Script to create a compute cluster without a scheduler in a VNet in Azure
# Create a custom private network in Azure
# Replace 10.32.0.0 with your virtual network address space
# Replace <network-name> with your network identifier
# Replace "West US" with an Azure region where the VM size is available
# See Azure Pricing pages for prices and availability of compute-intensive VMs

azure network vnet create -l "West US" -e 10.32.0.0 -i 16 <network-name>

# Create a cloud service. All the compute-intensive instances need to be in the same cloud service for Linux RDMA to work across InfiniBand.
# Note: The current maximum number of VMs in a cloud service is 50. If you need to provision more than 50 VMs in the same cloud service in your cluster, contact Azure Support.

azure service create <cloud-service-name> --location "West US" –s <subscription-ID>

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Define a prefix for external port numbers. If you want to turn off external ports and use only internal ports to communicate between compute nodes via port 22, don’t use this option. Since port numbers up to 10000 are reserved, use numbers after 10000. Leave external port on for rank 0 and head node.

portnumber=101

# In this cluster there will be 8 size A9 nodes, named cluster11 to cluster18. Specify your captured image in <image-name>. Specify the username and password you used when creating the SSH keys.

for (( i=11; i<19; i++ )); do
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>
done

# Save this script with a name like makecluster.sh and run it in your shell environment to provision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a>A CentOS HPC fürtre megfontolandó szempontok

Ha szeretne a HPC SLES 12 helyett az Azure piactéren elérhető CentOS-alapú HPC képek egyike alapján fürt beállítása, kövesse a általános az előző szakaszban. Ha kiépítése, és állítsa be a virtuális, vegye figyelembe az alábbi különbségek:

1. Intel MPI már telepítve van egy virtuális CentOS-alapú HPC képen lévő kiépítéstől. 

2. A virtuális /etc/security/limits.conf fájlban már bekerülnek a zárolási memória beállításait.

2. A virtuális kiépítése, a rögzítési SSH billentyűk hoz létre. Ehelyett javasoljuk felhasználói-alapú hitelesítés beállítása után a cluser rendszerbe. A következő részben.  

### <a name="set-up-passwordless-ssh-trust-on-the-cluster"></a>A fürt passwordless SSH adatvédelmi beállítása

A CentOS-alapú HPC fürtre vannak olyan megbízhatóvá a számítási csomópontok között két módszer: host-alapú hitelesítés és a felhasználó-alapú hitelesítés. Host-alapú hitelesítés Ez a cikk hatókörének kívül esik, és általában kell elvégezni egy bővítmény parancsfájl a telepítés során. Felhasználó-alapú hitelesítés kényelmes a telepítés után megbízhatóvá, és csak a létrehozásához és megosztása a fürt SSH kulcsok számítási csomópontjai között. Ez a módszer gyakran nevezik passwordless SSH bejelentkezési és szükséges MPI feladatok futtatásakor. 

A közösségi járult mintaparancsfájl [GitHub](https://github.com/tanewill/utils/blob/master/user_authentication.sh) ahhoz, hogy könnyen felhasználóhitelesítés CentOS-alapú HPC fürtre érhető el. Töltse le és ezt az alábbi lépésekkel parancsprogramot. Is módosítsa a parancsfájlt vagy bármelyik másik módszert a számítási fürthöz csomópontok között passwordless SSH hitelesítési létrehozásához használható.

    wget https://raw.githubusercontent.com/tanewill/utils/master/ user_authentication.sh
    
A parancsfájl futtatásához kell tudnia a alhálózat IP-címek előtagját. Az előtag első fürt csomópontok egyik a következő parancs futtatásával. A kimenet hasonlóan kell kinéznie 10.1.3.5, és az előtag az 10.1.3 része.

    ifconfig eth0 | grep -w inet | awk '{print $2}'

Most futtassa a három paramétereket: a közös felhasználó nevét a számítási csomóponton, a felhasználó számára a számítási csomópontok és a visszaadott az előző parancs előtagot közös jelszót.


    ./user_authentication.sh <myusername> <mypassword> 10.1.3

Ez a parancsfájl az alábbi műveleteket végzi el:

* A host nevű .ssh, amely szükség a passwordless bejelentkezési csomóponton könyvtár létrehozása 
* Konfigurációs fájl a .ssh könyvtár, amely arra utasítja bármely csomópont bejelentkezési engedélyezni a fürt passwordless bejelentkezési hoz létre. 
* A csomópont nevét és IP-címek csomópontot a fürt csomópontjait tartalmazó fájlokat hoz létre. Ezek a fájlok maradnak, hogy később szükség esetén a parancsfájl futtatása után. 
* Létrehoz egy személyes és nyilvános kulcs pár az egyes fürt csomópontot, beleértve a host csomópontot, és a authorized_keys fájlban tételeket hoz létre.

>[AZURE.WARNING]A parancsfájl futtatása készíthet biztonsági kockázatot. Győződjön meg arról, hogy a ~/.ssh nyilvános főbb információk nem van meghatározva.


## <a name="configure-intel-mpi"></a>Intel MPI konfigurálása

Azure Linux RDMA MPI alkalmazások futtatásához kell bizonyos Intel MPI jellemző környezeti változók konfigurálása. Az alábbiakban az alkalmazás futtatásához szükséges változók konfigurálása Bash mintaparancsfájl. Módosítsa az utat mpivars.sh, szükség szerint az Intel MPI telepítésének.

```
#!/bin/bash -x

# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster 

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh

export I_MPI_FABRICS=shm:dapl

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB
# Setting the variable to shm:dapl gives best performance for some applications
# If your application doesn’t take advantage of shared memory and MPI together, then set only dapl

export I_MPI_DAPL_PROVIDER=ofa-v2-ib0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

export I_MPI_DYNAMIC_CONNECTION=0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

# Command line to run the job

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path to the application exe> <arguments specific to the application>

#end
```

A host fájl formátuma az alábbi képlettel történik. Vegye fel a fürt minden csomópont egy vonalat. Adja meg a VNet a saját IP-címek definiált korábbi verzióiban nem a DNS-neveket. Ha például két állomások 10.32.0.1 és 10.32.0.2 IP-címek a fájlt az alábbiakat tartalmazza:

```
10.32.0.1:16
10.32.0.2:16
```

## <a name="run-mpi-on-a-basic-two-node-cluster"></a>MPI futtassa a csomópont-két alapvető fürthöz

Még nem tette meg, ha először a környezet beállítása az Intel MPI. 

```
# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster 

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-a-simple-mpi-command"></a>Egyszerű MPI parancs futtatása

A számítási csomópontok mutatják, hogy MPI megfelelően telepítve van, és kommunikálhat között legalább két kiszámítania csomópontok egyik egyszerű MPI parancs futtatására. A következő **mpirun** parancsot a **Hostname (állomásnév)** parancsot futtatja két csomópontot.

```
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
A kimenet szerepelnie kell a nevét, az összes olyan csomópontot, mint a bemeneti átadott `-hosts`. Például egy **mpirun** parancs két csomópontokkal kimenetét adja vissza az alábbihoz hasonló:

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a>Egy MPI javasolt futtatása

A következő Intel MPI parancsot futtatja a pingpong javasolt, és ellenőrizze az fürt konfigurálása és a RDMA hálózati kapcsolat.

```
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

A munka fürthöz két csomópontokkal ekkor megjelennek a kimeneti az alábbihoz hasonló. Az Azure RDMA hálózaton vagy az üzenet 3 mikroszekundum alatti késés 512 bájt méretű várnak.

```
#------------------------------------------------------------
#    Intel (R) MPI Benchmarks 4.0 Update 1, MPI-1 part
#------------------------------------------------------------
# Date                  : Fri Jul 17 23:16:46 2015
# Machine               : x86_64
# System                : Linux
# Release               : 3.12.39-44-default
# Version               : #5 SMP Thu Jun 25 22:45:24 UTC 2015
# MPI Version           : 3.0
# MPI Thread Environment:
# New default behavior from Version 3.2 on:
# the number of iterations per message size is cut down
# dynamically when a certain run time (per message size sample)
# is expected to be exceeded. Time limit is defined by variable
# "SECS_PER_SAMPLE" (=> IMB_settings.h)
# or through the flag => -time

# Calling sequence was:
# /opt/intel/impi_latest/bin64/IMB-MPI1 pingpong
# Minimum message length in bytes:   0
# Maximum message length in bytes:   4194304
#
# MPI_Datatype                   :   MPI_BYTE
# MPI_Datatype for reductions    :   MPI_FLOAT
# MPI_Op                         :   MPI_SUM
#
#
# List of Benchmarks to run:
# PingPong
#---------------------------------------------------
# Benchmarking PingPong
# #processes = 2
#---------------------------------------------------
       #bytes #repetitions      t[usec]   Mbytes/sec
            0         1000         2.23         0.00
            1         1000         2.26         0.42
            2         1000         2.26         0.85
            4         1000         2.26         1.69
            8         1000         2.26         3.38
           16         1000         2.36         6.45
           32         1000         2.57        11.89
           64         1000         2.36        25.81
          128         1000         2.64        46.19
          256         1000         2.73        89.30
          512         1000         3.09       157.99
         1024         1000         3.60       271.53
         2048         1000         4.46       437.57
         4096         1000         6.11       639.23
         8192         1000         7.49      1043.47
        16384         1000         9.76      1600.76
        32768         1000        14.98      2085.77
        65536          640        25.99      2405.08
       131072          320        50.68      2466.64
       262144          160        80.62      3101.01
       524288           80       145.86      3427.91
      1048576           40       279.06      3583.42
      2097152           20       543.37      3680.71
      4194304           10      1082.94      3693.63

# All processes entering MPI_Finalize

```



## <a name="next-steps"></a>Következő lépések

* Próbálja meg alkalmazások telepítéséhez és futtatásához a Linux MPI a Linux fürt.

* Intel MPI nézze meg a [Intel MPI Library dokumentációjában](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) találhat.

* Próbálja meg a [quickstart útmutató sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) létrehozása egy Intel fényesség fürthöz CentOS-alapú HPC képként használatával. Részletekért olvassa el a következő [blogbejegyzésből](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).

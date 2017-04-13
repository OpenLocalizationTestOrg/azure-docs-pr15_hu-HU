<properties
 pageTitle="A Microsoft HPC csomag a Linux VMs NAMD |} Microsoft Azure"
 description="Azure a Microsoft HPC csomag fürt telepítése, és futtassa a NAMD szimulációk charmrun több Linux számítási csomóponton"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="10/13/2016"
 ms.author="danlep"/>

# <a name="run-namd-with-microsoft-hpc-pack-on-linux-compute-nodes-in-azure"></a>A Microsoft HPC Pack NAMD futtatni Linux számítási csomópontok Azure-ban

Ebből a cikkből megtudhatja, egyirányú Azure virtuális gépeken Linux nagy teljesítményű számítások (HPC) terhelési futtatásához. Itt, állítsa be a [Microsoft HPC csomag](https://technet.microsoft.com/library/cc514029) fürtre, a Azure Linux számítási csomópontok, és futtassa a [NAMD](http://www.ks.uiuc.edu/Research/namd/) szimulációk alapján számolja ki, és a képi megjelenítés egy nagy biomolekuláris rendszer szerkezete.  

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

* **NAMD** (a program Nanoscale molekuláris Dynamics) csomag egy párhuzamos molekuláris dynamics nagy teljesítményű szimulációk atom akár több millió tartalmazó nagy biomolekuláris rendszerek készült. A fenti rendszerek közé vírusok, a cella szerkezetek és a nagy fehérjék. NAMD méretezze át a szokásos szimulációk fúrásmintáit több száz, és a legnagyobb szimulációk több mint 500 000 fúrásmintáit.

* **A Microsoft HPC csomag** nagyméretű HPC és a párhuzamos alkalmazások futtatásához a helyszíni számítógépek és Azure virtuális gépeken futó fürt szolgáltatásokat nyújt. A Windows HPC-munkaterhelésekből, HPC csomag megoldásként eredetileg fejlesztett futó Linux HPC alkalmazások Linux most támogatja a csomópont VMs HPC csomag fürt rendszerbe számítja ki. Lásd: az [első lépések az Azure-ban egy HPC csomag fürthöz Linux számítási csomópontok](virtual-machines-linux-classic-hpcpack-cluster.md) bevezetőnek.

Más lehetőségek futtatásához Linux HPC munkaterhelésekből Azure-ban a [technikai erőforrásokat a köteget, és a nagy teljesítményű számítások](../batch/batch-hpc-solutions.md)című témakör tartalmaz.




## <a name="prerequisites"></a>Előfeltételek

* **HPC csomag fürt Linux kiszámítania csomópontok** – egy [erőforrás-kezelő Azure-sablon](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) vagy egy [Azure PowerShell-parancsprogramot](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md)Azure-HPC csomag fürt Linux számítási csomópontokkal telepítése. Az [első lépések](virtual-machines-linux-classic-hpcpack-cluster.md) című Linux számítási csomópontokkal Azure-ban HPC csomag fürt Előfeltételek és lépései a vagy beállítást. A PowerShell parancsprogramot telepítési lehetőséget választja, ha a minta konfigurációs fájl Ez a cikk végén a mintafájlok megtekintéséhez. Ez a fájl egy Windows Server 2012 R2 központi csomópontot, és a négy méret nagy CentOS 6.6 számítási csomópontok álló Azure-alapú HPC csomag fürthöz állítja be. Testre szabhatja a fájlt, környezetben szükség szerint.


* **NAMD szoftverek és oktatóprogram fájlok** - Linux (regisztráció szükséges) [NAMD](http://www.ks.uiuc.edu/Research/namd/) webhelyről NAMD töltse le a szoftvert. Ez a cikk NAMD 2.10-es verzió alapul, és használja a [Linux-x86_64 (64 bites Intel vagy AMD Ethernet)](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310) archívumba. Az [oktatóprogram fájlok NAMD](http://www.ks.uiuc.edu/Training/Tutorials/#namd)is letöltheti. A letöltések .tar fájlokat, és a fájlokat az csomóponton központi Windows eszköz szükséges. Bontsa ki a fájlokat, kövesse az útmutatást a jelen cikk. 

* **VMD** (nem kötelező) – az eredmények a NAMD feladat töltse le és a Megjelenítés molekuláris program [VMD](http://www.ks.uiuc.edu/Research/vmd/) telepíteni egy olyan számítógépen, lehetőség. Az aktuális verziója 1.9.2. Lásd: a VMD letöltési webhelye a kezdéshez.  


## <a name="set-up-mutual-trust-between-compute-nodes"></a>Számítási csomópontok között kölcsönös adatvédelmi beállítása
A csomópontok megbízhatónak minősít egymást ( **rsh** vagy **ssh**) keresztté-csomópont feladat futó több Linux csomópontok szükséges. A HPC csomag fürt hoz létre a Microsoft HPC csomag IaaS telepítési parancsfájlt, a parancsfájl automatikusan beállítja a rendszergazdai fiókhoz, akkor adja meg az állandó kölcsönös adatvédelmi. Hoz létre a fürt tartomány rendszergazdai jogokkal nem rendelkező felhasználóknak hogy beállítása ideiglenes kölcsönös adatvédelmi csomópontjai között, amikor egy projekt őket rendelt. A kapcsolat majd, destroy, a feladat befejezése után. Ehhez a minden olyan felhasználóhoz, adja meg egy RSA kulcs pár a fürthöz HPC csomag alkalmazó megbízhatósági kapcsolat létrehozásához. Kövesse az utasításokat.

### <a name="generate-an-rsa-key-pair"></a>Egy RSA kulcs pár készítése
Nagyon egyszerűen egy RSA kulcs pár, amely tartalmazza a nyilvános és személyes kulcs, a Linux **ssh-keygen** parancs futtatásával létrehozásához.

1.  Jelentkezzen be a Linux számítógépre.

2.  A következő parancsot:

    ```bash
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE] Nyomja le az **ENTER billentyűt** az alapértelmezett beállításokat használja a parancs befejeztéig. Ne írjon be egy jelszót. Amikor a rendszer kéri a jelszót, csak nyomja le az **ENTER billentyűt**.

    ![Egy RSA kulcs pár készítése][keygen]

3.  Váltson a címtár ~/.ssh könyvtárra. A titkos kulcs id_rsa és a nyilvános kulcs id_rsa.pub vannak tárolva.

    ![Személyes és nyilvános kulcs][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>A fő pár hozzáadása a HPC csomag fürthöz
1.  [Csatlakozás távoli asztali által](virtual-machines-windows-connect-logon.md) a fő csomópont virtuális használja a tartományt a hitelesítő adatok, feltéve ha telepítette az fürt (például hpc\clusteradmin). A központi csomópont kezelheti a fürt.

2. Általános eljárás a Windows Server használatával tartomány felhasználói fiók létrehozása az Active Directory-tartománya a fürt. A központi csomópontot például használja az Active Directory felhasználói és számítógépek eszközt. Ez a cikk példáiban feltételezik hpcuser a hpclab tartományban (hpclab\hpcuser) nevű tartományban felhasználó létrehozása.

3. A tartomány felhasználó hozzáadása a HPC csomag fürt fürt felhasználó. Című cikkben olvashat [hozzáadása vagy eltávolítása a fürt felhasználó](https://technet.microsoft.com/library/ff919330.aspx).

2.  Hozzon létre egy C:\cred.xml nevű fájlt, és a fő RSA olyan adatokat másol be azt. Példa a a mintafájlok Ez a cikk végén található.

    ```
    <ExtendedData>
        <PrivateKey>Copy the contents of private key here</PrivateKey>
        <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

3.  Nyisson meg egy parancssort, és írja be a hpclab\hpcuser fióknak a hitelesítő adatok beállítása a következő parancsot. A **extendeddata** paraméter használatával adják át a létrehozott C:\cred.xml fájl kulcsfontosságú adatokhoz nevét.

    ```command
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    Ez a parancs befejeződik kimeneti nélkül. Után a hitelesítő adatokat a felhasználói fiókok feladatok futtatásához szükséges beállítást, a cred.xml fájl biztonságos helyen tárolni, vagy törölheti azt.

5.  Ha a RSA kulcs pár a Linux csomópontok egyik hozza létre, ne felejtse el a kulcsok törlése, miután végzett a használatuk. Ha úgy találja, hogy egy meglévő id_rsa vagy id_rsa.pub fájl nincs beállítva HPC csomag kölcsönös adatvédelmi.

>[AZURE.IMPORTANT] Nem javasoljuk Linux feladat futtatása fürt rendszergazdái egy megosztott fürtre, mert a Linux csomóponton a legfelső szintű fiókon fut a rendszergazda által küldött feladat. Rendszergazdai jogokkal nem rendelkező felhasználó által küldött feladatot azonos nevű feladat felhasználóként alatt helyi Linux felhasználói fiók fut. Ebben az esetben HPC csomag beállítja a Linux felhasználó kölcsönös adatvédelmi végig a projekthez hozzárendelt összes csomópontot. Állíthat be manuálisan a Linux csomópontok Linux felhasználó a feladat futtatása előtt, vagy HPC csomag automatikusan hoz létre a felhasználó a feladat elküldésekor. Ha HPC csomag hoz létre, a felhasználót, HPC csomag törli az elemet a feladat befejeződése után. Biztonsági kockázatot csökkentéséhez a billentyűk után a feladat befejezése után kattintson a csomópontok törlődnek.

## <a name="set-up-a-file-share-for-linux-nodes"></a>Linux csomópontok fájlmegosztás beállítása

Most egy kis-és Középvállalatok fájlmegosztás beállítása, és csatlakoztassa a megosztott mappa lehetővé teszi a Linux csomópontok közös útvonallal NAMD fájlok elérése minden Linux csomóponton. Az alábbiakban lépéseket követve csatlakoztassa a megosztott mappa a központi csomópontra. A megosztás, amely jelenleg nem támogatja a Azure szolgáltatás terjesztését CentOS 6.6 például ajánlott. Ha a Linux csomópontok egy Azure fájlmegosztás támogatja, megtudhatja, [hogy miként Azure fájltároló Linux használja](../storage/storage-how-to-use-files-linux.md). További fájlmegosztási beállításai HPC Pack olvassa el a [Linux számítási csomópontok HPC csomag fürt Azure-ban – első lépések](virtual-machines-linux-classic-hpcpack-cluster.md)című témakört.

1.  Hozzon létre egy mappát a központi csomópontra, és ossza meg az mindenki számára olvasási/írási jogosultsággal megadásával. Ebben a példában \\ \\CentOS66HN\Namd pedig a mappát, ahol CentOS66HN a fő csomópont állomásneve neve.

2. A megosztott mappa namd2 nevű mappát hozhat létre. Namd2 hozzon létre egy másik almappát namdsample.

3. Bontsa ki a mappában lévő NAMD fájlok verziójával Windows **tar** vagy egy másik Windows-segédprogram működik a .tar archívumok. 
    * Bontsa ki a NAMD tar archívumba való \\ \\CentOS66HN\Namd\namd2.
    
    * Bontsa ki a oktatóanyag fájlokat a \\ \\CentOS66HN\Namd\namd2\namdsample.

4. Nyissa meg a Windows PowerShell-ablakot, és futtassa az alábbi parancsokat a megosztott mappa a Linux csomóponton csatlakoztatásához.

    ```command
    clusrun /nodegroup:LinuxNodes mkdir -p /namd2

    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS66HN/Namd/namd2 /namd2 -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

Az első parancs létrehoz egy LinuxNodes csoportjában található összes csomóponton /namd2 nevű mappát. A második parancs csatlakoztat a megosztott mappa //CentOS66HN/Namd/namd2 alakzatot a dir_mode tartalmazó mappát, és file_mode bittel állítsa 777. A *felhasználónév* és *jelszó* a parancsot a kell a központi csomópontra a felhasználó hitelesítő adatait.

>[AZURE.NOTE]A "\`" a második parancs a szimbólumok egy PowerShell escape-szimbólumot. "\`," azt jelenti, hogy a "," (vessző karakter) része a parancsot.


## <a name="create-a-bash-script-to-run-a-namd-job"></a>Hozzon létre egy Bash parancsfájlt NAMD feladat futtatása

A NAMD feladatok **charmrun** NAMD folyamatok indításakor használandó csomópontok számának meghatározása *csomópontlista* fájlok van szüksége. Hozza létre a csomópontlista fájlt, és **charmrun** futtatja a csomópontlista fájllal Bash parancsfájl használhatja. Majd elküldheti a HPC felülete, amely felhívja ezt a parancsfájlt NAMD feladatot.

Egy egyszerű szövegszerkesztő programban lehetőség használata esetén a NAMD programfájlok tartalmazó /namd2 mappában Bash parancsfájl létrehozása, és hpccharmrun.sh nevet. Egy rövid igazolására fogalom másolja a hpccharmrun.sh mintaparancsfájl megadva, ez a cikk végén, és nyissa meg a [Küldés NAMD feladatot](#submit-a-namd-job).

>[AZURE.TIP] Mentés a parancsprogram Linux szövegfájlba sorvégeket a (LF csak, nem CR LF). Ezzel biztosíthatja, hogy futása megfelelően a Linux csomóponton.

Az alábbiakban olvashat a bash parancsfájl leírása. 

1.  Néhány változók megadása

    ```bash
    #!/bin/bash

    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    # Charmrun command
    CHARMRUN=${SCRIPT_PATH}/charmrun
    # Argument of ++nodelist
    NODELIST_OPT="++nodelist"
    # Argument of ++p
    NUMPROCESS="+p"
    ```

2.  Csomópont adatok beolvasása a környezeti változók. $NODESCORES: $CCP_NODES_CORES osztott szavak listáját tárolja. $COUNT $NODESCORES mérete.
    ```
    # Get node information from the environment variables
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    ```    
    
    Az a $CCP_NODES_CORES változó esetén az alábbi képlettel történik:

    ```
    <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>…
    ```

    Ez a változó csomópontok, csomópont nevek és a feladat lefoglalt magmintákat minden csomóponton száma száma sorolja fel. Ha például ha a feladat futtatása 10 magmintákat, $CCP_NODES_CORES értéke hasonló:

    ```
    3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 2
    ```
        
3.  Ha nincs beállítva a $CCP_NODES_CORES változó, indítsa el a **charmrun** közvetlenül. (Ez csak történjen a parancsfájl közvetlenül a Linux csomóponton futtatásakor.)

    ```
    if [ ${COUNT} -eq 0 ]
    then
        # CCP_NODES is_CORES is not found or is empty, so just run charmrun without nodelist arg.
        #echo ${CHARMRUN} $*
        ${CHARMRUN} $*
    ```

4.  Vagy hozzon létre egy **charmrun**csomópontlista fájlt.

    ```
    else
        # Create the nodelist file
        NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

        # Write the head line
        echo "group main" > ${NODELIST_PATH}

        # Get every node name and number of cores and write into the nodelist file
        I=1
        while [ ${I} -lt ${COUNT} ]
        do
            echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
            let "I=${I}+2"
        done
```
5.  **Charmrun** futtassa a csomópontlista fájllal, beszerzése a visszaadott érték azt, és távolítsa el a csomópontlista fájlt végén.

    {CCP_NUMCPUS} $ a HPC csomag központi csomópont beállítása egy másik környezeti változó. A feladat rendelt összes magmintákat számát tárolja. Akkor annak segítségével charmrun az eljárások számának megadása.

    ```
    # Run charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
    fi

    ```
6.  Lépjen ki a **charmrun** visszaküldési állapotú.

    ```
    exit ${RTNSTS}
    ```



Az alábbiakban csomópontlista fájlt, és a parancsprogram-hoz létre az adatokat:

```
group main
host <Name of node1> ++cpus <Cores of node1>
host <Name of node2> ++cpus <Cores of node2>
…
```

Példa:

```
group main
host CENTOS66LN-00 ++cpus 4
host CENTOS66LN-01 ++cpus 4
host CENTOS66LN-03 ++cpus 2
```

## <a name="submit-a-namd-job"></a>NAMD feladat elküldése

Most már készen áll a HPC felülete NAMD feladat elküldése.

1.  A fürt fő csomópont csatlakozzon, és indítsa el a HPC felülete.

2.  **Erőforrás-kezelés**győződjön meg róla, hogy a Linux számítási csomópontok **Online** állapotban van. Ha nem, jelölje ki őket, és kattintson az **Online állapotba**.

2.  **Feladatkezelés**kattintson az **Új feladatot**.

3.  Írja be például a *hpccharmrun*feladat nevét.

    ![Új HPC feladat][namd_job]

4.  A **Feladat részletei** lapon a **Feladat erőforrásokat**válassza ki a az erőforrás- **csomópont** , és a **Minimum** beállítása 3. , azt a feladat futtatása a három Linux csomóponton és az egyes csomópontok négy magmintákat.

    ![Feladat-erőforrások][job_resources]

5. A bal oldali navigációs sávon kattintson a **Szerkesztés feladatok** , és kattintson a **Hozzáadás** a tevékenység hozzáadása a projekthez.    


6. A **tevékenység adatai és a I/O átirányítás** lapon állítsa az alábbi értékeket:

    * **Parancssor** -
`/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`

        >[AZURE.TIP] A fenti parancssor sortörés nélkül egyetlen paranccsal. Azt tördelése több sorba **parancsot a vonal**csoportban.

    * **Munkakönyvtár** - /namd2

    * **Minimális** – 3

    ![Feladat részletei][task_details]

    >[AZURE.NOTE] A munkakönyvtár itt beállított mert **charmrun** megpróbálja nyissa meg azt a azonos munkakönyvtár minden csomóponton. A munkakönyvtár értéke nincs beállítva, ha HPC csomag a parancs elindítja a véletlenszerűen nevű mappa a Linux csomópontok egyik létrehozott. Ez a következő hibát okoz a csomóponton: `/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.` a probléma elkerülése érdekében, adjon meg egy mappa elérési útja is elérhető csomópontjait, mint a munkakönyvtár.

5.  Kattintson az **OK gombra** , és kattintson a **Küldés gombra** a feladat futtatása gombra.

    Alapértelmezés szerint a HPC csomag elküldi a feladatot, az aktuális bejelentkezett felhasználó fiók. Egy párbeszédpanel kérheti a felhasználónév és jelszó megadását követően kattintson a **Küldés gombra**.

    ![Feladat hitelesítő adatok][creds]

    Bizonyos körülmények HPC csomag megjegyzi előtt bemeneti, és nem jelenik meg ez a párbeszédpanel a felhasználói adatok. Ha HPC csomag jelenjen meg, a következő parancsot a parancssorba írja be, és küldje a feladatot.

    ```command
    hpccred delcreds
    ```

6.  A feladat befejezéséhez néhány percig tart.

7.  Keresse meg a projekt naplója \\ <headnodeName>\Namd\namd2\namd2_hpccharmrun.log és a kimeneti fájlokat \\ <headnodeName>\Namd\namd2\namdsample\1-2-sphere\.

8.  Ha szükséges indítsa el a VMD a feladat eredmények megtekintéséhez. A lépéseket a NAMD megjelenítése a kimeneti (ebben az esetben egy ubiquitin fehérje molekula egy víz kapcsolataikat) fájlok vannak a jelen cikk túlra. Lásd: [NAMD oktatóanyag](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf) további információt.

    ![Feladat eredménye][vmd_view]

## <a name="sample-files"></a>Mintafájlok

### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Minta XML-konfigurációs fájl által PowerShell-parancsprogramot fürt telepítéshez

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
      <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS66HN</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS66LN-%00%</VMNamePattern>
    <ServiceName>MyLnxCNService</ServiceName>
     <VMSize>Large</VMSize>
     <NodeCount>4</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-66-20150325</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>    
```

### <a name="sample-credxml-file"></a>Mintafájl cred.xml

```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```

### <a name="sample-hpccharmrunsh-script"></a>Hpccharmrun.sh mintaparancsfájl

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
# Charmrun command
CHARMRUN=${SCRIPT_PATH}/charmrun
# Argument of ++nodelist
NODELIST_OPT="++nodelist"
# Argument of ++p
NUMPROCESS="+p"

# Get node information from ENVs
# CCP_NODES_CORES=3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 4
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # If CCP_NODES_CORES is not found or is empty, just run the charmrun without nodelist arg.
    #echo ${CHARMRUN} $*
    ${CHARMRUN} $*
else
    # Create the nodelist file
    NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

    # Write the head line
    echo "group main" > ${NODELIST_PATH}

    # Get every node name & cores and write into the nodelist file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}
```

 





<!--Image references-->
[keygen]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/keygen.png
[keys]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/keys.png
[namd_job]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/namd_job.png
[job_resources]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/job_resources.png
[creds]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/creds.png
[task_details]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/task_details.png
[vmd_view]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/vmd_view.png

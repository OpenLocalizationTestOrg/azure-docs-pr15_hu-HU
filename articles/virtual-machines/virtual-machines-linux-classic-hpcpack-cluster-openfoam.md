<properties
 pageTitle="OpenFOAM futtatni HPC Pack Linux VMs |} Microsoft Azure"
 description="A Microsoft HPC csomag fürtre a Azure telepítése, és egy OpenFOAM feladat futtatása több Linux számítási csomóponton egy RDMA hálózaton keresztül."
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
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="run-openfoam-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Microsoft HPC csomag OpenFoam futtassa a Linux RDMA fürthöz Azure-ban

Ebből a cikkből megtudhatja, egyirányú Azure virtuális gépeken futó OpenFoam futtatásához. Ebben az esetben a Microsoft HPC csomag fürtre Linux számítási csomópontokkal Azure és a Futtatás Intel MPI a projekt- [OpenFoam](http://openfoam.com/) rendszerbe. A számítási csomópontok RDMA internetezésre alkalmas Azure VMs használhatja, hogy a számítási csomópontok kommunikálni az Azure RDMA hálózaton keresztül. Egyéb lehetőségek OpenFoam futtatásához Azure-ban elérhető teljesen konfigurált kereskedelmi képeket a piactér, például UberCloud meg [a CentOS 6 OpenFoam 2.3](https://azure.microsoft.com/marketplace/partners/ubercloud/openfoam-v2dot3-centos-v6/)és [Azure](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/)köteg futtatásával is. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

(Az mező művelet megnyitása és kezelése) OpenFOAM egy nagy népszerűségnek mérnöki és tudományos, kereskedelmi és iskolai is szervezetekben lévő Megnyitás-forrás számítási tételéhez dynamics (CFD) szoftvercsomag. Ellenőrző eszközök meshing, nevezetesen snappyHexMesh, egy parallelized mesher az összetett CAD-geometriákkal, és a előtti és utáni feldolgozása tartalmazza. Szinte minden folyamat futtatása párhuzamosan felhasználók kihasználhatja teljes hardver rendelkezésükre lehetővé teszi.  

A Microsoft HPC csomag nagyméretű HPC és a párhuzamos alkalmazásai, köztük a MPI alkalmazások, a Microsoft Azure virtuális gépeken futó meghatározására futtatásához szolgáltatásokat nyújt. HPC csomag is támogat futó Linux HPC Linux alkalmazások VMs rendszerbe HPC csomag fürt csomópont számítja ki. Az [első lépések](virtual-machines-linux-classic-hpcpack-cluster.md) című Linux számítási csomópontokkal HPC csomag fürt Azure-ban történő használatának Linux számítási csomópontok HPC csomag bevezetőnek.

>[AZURE.NOTE] Ez a cikk bemutatja, hogyan HPC Pack Linux MPI terhelési futtatásához. Azt feltételezi, hogy van-e bizonyos ismerete Linux rendszerben felügyelete és az MPI munkaterhelésekből futó Linux fürt. Ha MPI és eltér a jelen cikkben felsorolt lehetőségekből OpenFOAM verzióját használja, lehet, ha bizonyos telepítési és konfigurálási lépéseket módosítása. 

## <a name="prerequisites"></a>Előfeltételek

*   **RDMA internetezésre alkalmas Linux HPC csomag fürtre kiszámítania csomópontok** - üzembe helyezéséhez egy HPC csomag fürthöz méret A8, A9, H16r vagy H16rm Linux számítási csomópontok egy [erőforrás-kezelő Azure-sablon](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) vagy egy [Azure PowerShell-parancsprogramot](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md)használatával. Az [első lépések](virtual-machines-linux-classic-hpcpack-cluster.md) című Linux számítási csomópontokkal Azure-ban HPC csomag fürt Előfeltételek és lépései a vagy beállítást. A PowerShell parancsprogramot telepítési lehetőséget választja, ha a minta konfigurációs fájl Ez a cikk végén a mintafájlok megtekintéséhez. Ez a beállítás használatával az Azure-alapú HPC csomag fürt A8 Windows Server 2012 R2 központi méret csomópontot, és a 2 méret A8 SUSE Linux vállalati kiszolgáló 12 számítási csomópontok álló. Az előfizetés és szolgáltatás nevek megfelelő értékek helyett. 

    **További tudnivaló a**

    *   [A Linux RDMA hálózati előfeltételeihez Azure-ban, kapcsolatos tudnivalók](virtual-machines-windows-a8-a9-a10-a11-specs.md)című részben H – adatsorok és a számítási igényű A-sorozat VMs.

    *   A Powershell parancsprogramot telepítési beállítást használja, ha üzembe egy felhőalapú szolgáltatásba, ha a RDMA hálózati kapcsolaton belüli összes Linux számítási csomópontot.

    *   Miután üzembe Linux csomópontok, összekötése SSH bármilyen további felügyeleti feladatok. Keresse meg a SSH kapcsolat adatai minden Linux virtuális az Azure-portálon.  
        
*   **Intel MPI** - OpenFOAM futtathatnak SLES 12 HPC kiszámítania csomópontok Azure-ban, telepítenie kell ezeket az Intel MPI tár 5 futtatókörnyezet a [Intel.com webhelyet](https://software.intel.com/en-us/intel-mpi-library/). (Az Intel MPI 5 előre telepítve van CentOS-alapú HPC képek.)  Egy későbbi lépés Ha szükséges, telepíthető Intel MPI a Linux számítási csomópontok. Intel regisztrálását követően Felkészülés ezt a lépést, kövesse a kapcsolódó lap visszaigazoló e-mailben lévő hivatkozásra. Ezután másolja az Intel MPI megfelelő verziójának .tgz fájl letöltése hivatkozására. Ez a cikk az Intel MPI verzió 5.0.3.048 alapul.

*   **OpenFOAM forrás csomag** – letöltés OpenFOAM forrás csomag szoftver Linux a [OpenFOAM Foundation-webhelyeken](http://openfoam.org/download/2-3-1-source/). Ez a cikk a forrás csomag verziója letölthető 2.3.1 OpenFOAM-2.3.1.tgz értéke alapján. Kövesse az útmutatást a jelen cikk kicsomagolni és OpenFOAM összeállítása a Linux számítási csomóponton.

*   **EnSight** (nem kötelező) – lásd: a OpenFOAM szimulációk eredményét, töltse le és telepítse a [EnSight](https://www.ceisoftware.com/download/) képi és elemzési programot. Licencelési és letöltési információk vannak EnSight webhelyén.


## <a name="set-up-mutual-trust-between-compute-nodes"></a>Számítási csomópontok között kölcsönös adatvédelmi beállítása

A csomópontok megbízhatónak minősít egymást ( **rsh** vagy **ssh**) kereszt-csomópont feladat futó több Linux csomópontok szükséges. A HPC csomag fürt hoz létre a Microsoft HPC Pack IaaS telepítési parancsfájlt, a parancsfájl automatikusan beállítja a rendszergazdai fiókhoz, akkor adja meg az állandó kölcsönös adatvédelmi. Rendszergazdai jogokkal nem rendelkező felhasználók hoz létre a fürt tartomány esetén kell beállítania ideiglenes kölcsönös adatvédelmi csomópontjai között, amikor egy projekt hozzárendelt őket, és a kapcsolat destroy, a feladat befejezése után. Megbízhatóvá minden olyan felhasználóhoz, adja meg a fürthöz a meghatalmazás HPC csomag alkalmazó egy RSA kulcs pár.

### <a name="generate-an-rsa-key-pair"></a>Egy RSA kulcs pár készítése

Nagyon egyszerűen egy RSA kulcs pár, amely tartalmazza a nyilvános és személyes kulcs, a Linux **ssh-keygen** parancs futtatásával létrehozásához.

1.  Jelentkezzen be a Linux számítógépre.

2.  A következő parancsot:

    ```
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE] Nyomja le az **ENTER billentyűt** az alapértelmezett beállításokat használja a parancs befejeztéig. Ne írjon be egy jelszót. Amikor a rendszer kéri a jelszót, csak nyomja le az **ENTER billentyűt**.

    ![Egy RSA kulcs pár készítése][keygen]

3.  Váltson a címtár ~/.ssh könyvtárra. A titkos kulcs id_rsa és a nyilvános kulcs id_rsa.pub vannak tárolva.

    ![Személyes és nyilvános kulcs][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>A fő pár hozzáadása a HPC csomag fürthöz
1.  A távoli asztali kapcsolat a központi csomópontra a HPC csomag rendszergazdai fiókjával (a rendszergazdai fiókkal állította be a telepítési parancsfájlt futtatásakor).

2. Általános eljárás a Windows Server használatával tartomány felhasználói fiók létrehozása az Active Directory-tartománya a fürt. A központi csomópontot például használja az Active Directory felhasználói és számítógépek eszközt. Ez a cikk példáiban feltételezik hpclab\hpcuser névvel ellátott tartomány felhasználó létrehozása.

3.  Hozzon létre egy C:\cred.xml nevű fájlt, és a fő RSA olyan adatokat másol be azt. Mintafájl cred.xml Ez a cikk végén található.

    ```
    <ExtendedData>
        <PrivateKey>Copy the contents of private key here</PrivateKey>
        <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

4.  Nyisson meg egy parancssort, és írja be a hpclab\hpcuser fióknak a hitelesítő adatok beállítása a következő parancsot. A **extendeddata** paraméter használatával fázis C:\cred.xml fájl létrehozott kulcsfontosságú adatokhoz nevét.

    ```
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    Ez a parancs befejeződik kimeneti nélkül. Után a hitelesítő adatokat a felhasználói fiókok feladatok futtatásához szükséges beállítást, a cred.xml fájl biztonságos helyen tárolni, vagy törölheti azt.

5.  Ha a RSA kulcs pár a Linux csomópontok egyik hozza létre, ne felejtse el a kulcsok törlése, miután végzett a használatuk. Ha HPC csomag egy meglévő id_rsa vagy id_rsa.pub fájl talál, akkor nem állít be kölcsönös adatvédelmi.

>[AZURE.IMPORTANT] Nem javasoljuk Linux feladat futtatása fürt rendszergazdái egy megosztott fürtre, mert a rendszergazda által küldött feladat fut a legfelső szintű fiókon Linux csomópontok. A feladat rendszergazdai jogokkal nem rendelkező felhasználó által küldött azonban egy helyi Linux felhasználói fiók alatt fut, a feladat felhasználóként azonos nevű. Ebben az esetben HPC csomag beállítja a Linux felhasználó kölcsönös adatvédelmi végig a csomópontok rendelni a feladatot. Állíthat be manuálisan a Linux csomópontok Linux felhasználó a feladat futtatása előtt, vagy HPC csomag automatikusan hoz létre a felhasználó a feladat elküldésekor. Ha HPC csomag hoz létre, a felhasználót, HPC csomag törli az elemet a feladat befejeződése után. Biztonsági kockázatok csökkentéséhez HPC csomag eltávolítja a billentyűket a feladat befejezése után.

## <a name="set-up-a-file-share-for-linux-nodes"></a>Linux csomópontok fájlmegosztás beállítása

Most hozzon létre egy szokásos kis-és Középvállalatok megosztást, a központi csomópontra mappa. Ha engedélyezni szeretné a Linux csomópontok közös útvonallal alkalmazás fájlokat szeretné elérni, csatlakoztatása Linux csomópontok a megosztott mappára. Ha azt szeretné, egy másik fájlmegosztási lehetőséget, például az Azure-fájlok megosztása - sok esetek - vagy egy NFS megosztás ajánlott is használhatja. Lásd: a fájlmegosztási információt és részletes lépéseket az [első lépések a Linux számítási csomópontok HPC Pack fürt Azure-ban](virtual-machines-linux-classic-hpcpack-cluster.md).

1.  Hozzon létre egy mappát a központi csomópontra, és ossza meg az mindenki számára olvasási/írási jogosultsággal megadásával. Például megoszthatja C:\OpenFOAM központi a csomóponton \\ \\SUSE12RDMA-HN\OpenFOAM. *SUSE12RDMA-HN* Íme a fő csomópont állomásneve.

2.  Nyissa meg a Windows PowerShell-ablakot, és futtassa az alábbi parancsokat:

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam

    clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

Az első parancs létrehoz egy LinuxNodes csoportjában található összes csomóponton /openfoam nevű mappát. A második parancs a megosztott mappa //SUSE12RDMA-HN/OpenFOAM csatlakoztatja az dir_mode Linux csomóponton és file_mode bittel 777 értékűre. A *felhasználónév* és *jelszó* a parancsot a kell a központi csomópontra a felhasználó hitelesítő adatait.

>[AZURE.NOTE]A "\`" a második parancs a szimbólumok egy PowerShell escape-szimbólumot. "\`," azt jelenti, hogy a "," (vessző karakter) része a parancsot.

## <a name="install-mpi-and-openfoam"></a>MPI és OpenFOAM telepítése

Futtatásához OpenFOAM egy MPI feladat RDMA hálózat kell OpenFOAM összeállítása a az Intel MPI tárak. 

Először futtassa a Linux csomóponton több **clusrun** parancsok telepítheti az Intel MPI tárak (Ha még nincs telepítve) és OpenFOAM. A korábban beállított megosztására, a telepítőfájlokat Linux csomópontjai között fő csomópont-megosztás használata.

>[AZURE.IMPORTANT]A telepítési és fordítása lépéseket példák. Néhány ismerete Linux rendszerben felügyeleti annak érdekében, hogy függő fordítók bármelyikével és a tárak megfelelően telepítve van szüksége. Előfordulhat, hogy módosítania bizonyos környezeti változók vagy egyéb beállításait az Intel MPI és OpenFOAM verzióit. Részletekért olvassa el [Az Intel MPI tár Linux telepítési útmutatója](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) és [OpenFOAM forrás csomag telepítését](http://openfoam.org/download/2-3-1-source/) környezetben.


### <a name="install-intel-mpi"></a>Intel MPI telepítése

Mentse a letöltött telepítőcsomag az Intel MPI (ebben a példában l_mpi_p_5.0.3.048.tgz) C:\OpenFoam a központi csomópontra, hogy a Linux csomópontok érhető el az ezzel a fájllal /openfoam. Futtassa a **clusrun** Intel MPI tár telepítése összes Linux csomópontot.

1.  A következő parancsokat a telepítőcsomag másolja, és bontsa ki azt az egyes csomóponton /opt/intel.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel

    clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/

    clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
    ```

2.  Intel MPI tár csendes telepítése, használjon silent.cfg fájlt. Példa a a mintafájlok Ez a cikk végén található. Helyezze a fájlt a megosztott mappa /openfoam. A silent.cfg fájllal kapcsolatos részletekért lásd az [Intel MPI tár Linux telepítési útmutató - csendes telepítés](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).

    >[AZURE.TIP]Győződjön meg arról, hogy mentése szövegfájlba Linux silent.cfg fájlját sorvégeket (LF csak, nem CR LF). Ebben a lépésben ellenőrzi, hogy futása megfelelően a Linux csomóponton.

3.  Telepítse az Intel MPI tár csendes módban.
 
    ```
    clusrun /nodegroup:LinuxNodes bash /opt/intel/l_mpi_p_5.0.3.048/install.sh --silent /openfoam/silent.cfg
    ```
    
### <a name="configure-mpi"></a>MPI konfigurálása

A tesztelésre, a következő sorokat kell hozzáadása az egyes Linux csomópontok /etc/security/limits.conf:


    clusrun /nodegroup:LinuxNodes echo "*               hard    memlock         unlimited" `>`> /etc/security/limits.conf
    clusrun /nodegroup:LinuxNodes echo "*               soft    memlock         unlimited" `>`> /etc/security/limits.conf


Indítsa újra a Linux csomópontot, miután frissítette a limits.conf fájlt. Ha például használja a következő **clusrun** parancsot:

```
clusrun /nodegroup:LinuxNodes systemctl reboot
```

Újraindítása után győződjön meg arról, hogy a megosztott mappára, /openfoam csatlakoztatva van-e.

### <a name="compile-and-install-openfoam"></a>Összeállítása és OpenFOAM telepítése

A Mentés a letöltött telepítőcsomag a OpenFOAM forrás csomag (OpenFOAM 2.3.1.tgz ebben a példában) C:\OpenFoam a központi csomópontra, hogy a Linux csomópontok érhető el az ezzel a fájllal /openfoam. Futtassa a OpenFOAM összeállításához minden Linux csomóponton **clusrun** parancsokat.


1.  Hozzon létre egy mappát /opt/OpenFOAM minden Linux csomóponton, másolja a vágólapra a forrás csomag ebbe a mappába, és bontsa ki van.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM

    clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/
    
    clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
    ```

2.  Az Intel-MPI tárral OpenFOAM fordítása, először állítsa be néhány környezeti változók Intel MPI és OpenFOAM. Settings.sh nevű bash parancsfájl használatával adja hozzá a változót. Példa a a mintafájlok Ez a cikk végén található. Helyezze a megosztott mappa /openfoam ezt a fájlt (Linux sorvégződések menti). Ezt a fájlt később egy OpenFOAM feladat futtatásához használt MPI és OpenFOAM-szükséges beállításait is tartalmazza.

3. Függő csomagjai OpenFOAM összeállításához telepíthetők. Attól függően, hogy a Linux terjesztési először meg kell felvenni egy tárban tárolnak. Parancsokat **clusrun** az alábbihoz hasonló:

    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
    
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
    
    Ha szükséges, minden egyes Linux csomópontra SSH kattintva erősítse meg a megfelelő működéséhez a parancsok futtatásához.

4.  A következő parancsot sikerült OpenFOAM lefordítani. A fordítás folyamat néhány időt vesz igénybe, és a normál kimenő naplóadatok nagy mennyiségű hoz létre, így használni a **/ interleaved** lehetőséget a kimenet interleaved megjelenítéséhez.

    ```
    clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
    ```
    
    >[AZURE.NOTE]A "\`" a parancs a szimbólumok egy PowerShell escape-szimbólumot. "\`&" azt jelenti, hogy a "&" része a parancsot.

## <a name="prepare-to-run-an-openfoam-job"></a>Felkészülés a egy OpenFOAM feladat futtatása

Most már felkészítése egy MPI feldolgozás sloshingTank3D, amely a OpenFoam minták egyike nevű a két Linux csomópontot. 

### <a name="set-up-the-runtime-environment"></a>A futási idejű környezet beállítása

Beállítása a futtatókörnyezet környezetben MPI és OpenFOAM Linux csomóponton, futtassa a következő parancsot a Windows PowerShell ablakban a központi csomópontra. (Ez a parancs a SUSE Linux érvényes csak.)

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### <a name="prepare-sample-data"></a>Mintaadatok előkészítése

A fő csomópont megosztás között (mint /openfoam csatlakoztatott) Linux csomópontok fájljait lehet megosztani a korábban beállított használja.

1.  Az egyik a Linux SSH csomópontok számítja ki.

2.  A következő parancsot a OpenFOAM futtatókörnyezet környezet beállításához, ha még nem tette meg ezzel.

    ```
    $ source /openfoam/settings.sh
    ```
    
3.  A sloshingTank3D minta másolja a megosztott mappára, és keresse meg a fájlt.

    ```
    $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/

    $ cd /openfoam/sloshingTank3D
    ```

4.  Ez a példa az alapértelmezett paraméterek használata esetén is eltarthat, tízesre szeretné futtatni, perc, érdemes lehet, hogy legyen gyorsabban egyes paraméterek módosítása. Egy egyszerű választási lehetőséget is módosíthatja a idő lépés változók deltaT és a rendszer/controlDict fájlban writeInterval. A fájl az idő- és olvasása és írása megoldás adatok irányításának kapcsolódó összes bemeneti adatokat tárolja. Például 0,5 0,05 a deltaT értékének és writeInterval 0,05 a 0,5 értékének úgy módosíthatja.

    ![Lépés változóinak módosítása][step_variables]

5.  Adja meg a kívánt értékeket a változók a rendszer/decomposeParDict fájlban. Ez a példa két Linux csomópontok minden 8 magmintákat együtt, és így állítsa numberOfSubdomains 16 és hierarchicalCoeffs az n (1 1 16), ami azt jelenti OpenFOAM futtatása 16 folyamatok párhuzamosan. További tudnivalókért lásd: [OpenFOAM felhasználói útmutatóban: 3.4 futó alkalmazások párhuzamosan](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).

    ![Folyamatok felbontani][decompose]

6.  A következő parancsokat a sloshingTank3D könyvtárból, a mintaadatok létrehozása.

    ```
    $ . $WM_PROJECT_DIR/bin/tools/RunFunctions

    $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict

    $ runApplication blockMesh

    $ cp 0/alpha.water.org 0/alpha.water

    $ runApplication setFields  
    ```
    
7.  A központi csomópontra meg kell jelennie a mintaadatok fájljai C:\OpenFoam\sloshingTank3D bemásolja a program. (C:\OpenFoam a a megosztott mappa a központi csomópontra.)

    ![A központi csomópontra adatfájlok][data_files]

### <a name="host-file-for-mpirun"></a>A Host fájl mpirun

Ebben a lépésben a **mpirun** parancs használó host fájl (számítási csomópontok lista) létrehozása.

1.  A Linux csomópontok egyik nevű /openfoam, a hostfile, így érhető el ez a fájl összes Linux csomóponton /openfoam/hostfile a fájl létrehozása.

2.  Írja be a Linux csomópont nevét a fájlba. Ebben a példában a fájl a következő nevek tartalmazza:
    
    ```       
    SUSE12RDMA-LN1
    SUSE12RDMA-LN2
    ```
    
    >[AZURE.TIP]Ez a fájl a C:\OpenFoam\hostfile a központi csomópontra is létrehozhat. Ha ezt a beállítást választja, mentse ezeket a Linux szövegfájlba sorvégeket (LF csak, nem CR LF). Ezzel biztosíthatja, hogy futása megfelelően a Linux csomóponton.

    **Bash parancsprogram-csomagolópapír**

    Ha sok Linux csomópontok van, és azt szeretné, hogy a feladat futtatása csak néhány, nincs célszerű használni a rögzített host fájlt, mert nem tudja, hogy a projekt mely csomópontok kapják. Ebben az esetben írni egy bash parancsfájl csomagolópapír **mpirun** automatikusan létrehozza a Hosts fájl számára. Egy példa bash parancsfájl csomagolópapír nevű hpcimpirun.sh Ez a cikk végén található, és mentse /openfoam/hpcimpirun.sh. Ez a példa parancsfájl az alábbi műveleteket végzi el:

    1.  A környezet változók **mpirun**és néhány hozzáadása parancs paraméter beállítja a MPI feladat futtatása a RDMA hálózaton keresztül. Ebben az esetben akkor állítja be a következő változók:

        *   I_MPI_FABRICS = shm:dapl
        *   I_MPI_DAPL_PROVIDER levágása-v2-ib0 =
        *   I_MPI_DYNAMIC_CONNECTION = 0

    2.  A környezet szerint host fájl létrehozása a változó $CCP_NODES_CORES, amelyben a fő HPC csomópont szerint be van állítva, a feladat aktiválásakor.

        $CCP_NODES_CORES formátumának követi a minta:

        ```
        <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
        ```

        Ha

        * `<Number of nodes>`-a projekthez hozzárendelt csomópontok számának.  
        
        * `<Name of node_n_...>`-minden rendelt a projekt csomópontjának nevét.
        
        * `<Cores of node_n_...>`-a csomópontra a projekthez hozzárendelt magmintákat számát.

        Ha például ha a feladat futtatása két csomópontot, $CCP_NODES_CORES hasonlít
        
        ```
        2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
        ```
        
    3.  Felhívja a **mpirun** parancsot, és két paraméterrel hozzáfűzi a parancssorból.

        * `--hostfile <hostfilepath>: <hostfilepath>`-a fájl elérési útját a host a parancsprogram-hoz

        * `-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}`-változó beállítása a HPC csomag központi csomópont, amely tárolja a projekthez hozzárendelt összes magmintákat számát. Ebben az esetben **mpirun**folyamatok számát adja meg.


## <a name="submit-an-openfoam-job"></a>Egy OpenFOAM feladat elküldése

Most már elküldheti a HPC felülete feladatot. A parancsprogram hpcimpirun.sh átadni a parancsokat a projektfeladatok egyes szüksége.

1. A fürt fő csomópont csatlakozzon, és indítsa el a HPC felülete.

2. **Erőforrás-kezelés**, biztosítása érdekében, hogy a Linux számítási csomópontok **Online** állapotú. Ha nem, jelölje ki őket, és kattintson az **Online állapotba**.

3.  **Feladatkezelés**kattintson az **Új feladatot**.

4.  Írja be például a _sloshingTank3D_feladat nevét.

    ![Feladat részletei][job_details]

5.  **Feladat erőforrásokat**válassza ki a "Csomópont" erőforrás típusát, és a legkisebb értéke 2. Ebben a konfigurációban fut, két Linux csomópontot, amelyek mindegyikének nyolc magmintákat ebben a példában a feladatot.

    ![Feladat-erőforrások][job_resources]

6. A bal oldali navigációs sávon kattintson a **Szerkesztés feladatok** , és kattintson a **Hozzáadás** a tevékenység hozzáadása a projekthez. Négy tevékenységek hozzáadása a feladatot a következő parancssorokat és beállításokat.

    >[AZURE.NOTE]Futó `source /openfoam/settings.sh` beállítja a OpenFOAM és MPI futtatókörnyezet környezetekben, így az alábbi műveleteket minden egyes meghívja előtt a OpenFOAM parancsot.

    *   A **tevékenység 1**. Futtassa a **decomposePar** adatfájlok **interDyMFoam** párhuzamosan futó létrehozásához.
    
        *   A tevékenység egy csomópont hozzárendelése

        *   **Parancssor** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`
    
        *   **Munkakönyvtár** - / openfoam/sloshingTank3D
        
        Az alábbi ábrán látható. Hasonlóképpen konfigurálása a hátralévő tevékenységek.

        ![1 feladat részletei][task_details1]

    *   A **tevékenység 2**. Futtassa a **interDyMFoam** párhuzamosan a minta számítja ki.

        *   Két csomópontok hozzárendelése a tevékenységekhez

        *   **Parancssor** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`

        *   **Munkakönyvtár** - / openfoam/sloshingTank3D

    *   A **tevékenység 3**. Futtassa a **reconstructPar** egyetlen egyesíti az egyes processor_N_ Directoryból idő könyvtárak csoportja.

        *   A tevékenység egy csomópont hozzárendelése

        *   **Parancssor** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`

        *   **Munkakönyvtár** - / openfoam/sloshingTank3D

    *   A **tevékenység 4**. Futtassa a **foamToEnsight** párhuzamosan OpenFOAM eredmény fájlok konvertálása EnSight formátum, és helyezze a EnSight fájlokat a használatieset címtárban a Ensight nevű könyvtárában található.

        *   Két csomópont hozzárendelése a tevékenységekhez

        *   **Parancssor** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`

        *   **Munkakönyvtár** - / openfoam/sloshingTank3D

6.  Függőségek hozzáadása növekvő sorrendben tevékenység az alábbi műveleteket.

    ![Tevékenységfüggések][task_dependencies]

7.  Kattintson a **Küldés gombra** a feldolgozás futtatásához.

    Alapértelmezés szerint a HPC csomag elküldi a feladatot, az aktuális bejelentkezett felhasználó fiók. **Küldés**gombra kattintás után kéri, a felhasználónév és jelszó megadására szolgáló párbeszédpanel jelenhet meg.

    ![Feladat hitelesítő adatok][creds]

    Bizonyos körülmények HPC csomag megjegyzi előtt bemeneti, és nem jelenik meg ez a párbeszédpanel a felhasználói adatok. Ha HPC csomag jelenjen meg, a következő parancsot a parancssorba írja be, és küldje a feladatot.

    ```
    hpccred delcreds
    ```

8.  A feladat a percet tízesre megnyitja az órákat a minta beállított paramétereknek megfelelően. A hőtérkép a a feladat Linux csomópontok futó láthatók. 

    ![Hőtérkép megjelenítése][heat_map]

    Minden csomóponton nyolc folyamatok elindított.

    ![Linux folyamatok][linux_processes]

9.  Amikor befejezte a feladatot, a C:\OpenFoam\sloshingTank3D és a naplófájlok C:\OpenFoam a mappában megtalálható a feladat között.


## <a name="view-results-in-ensight"></a>Az eredmények EnSight megtekintése

Másik lehetőségként segítségével [EnSight](https://www.ceisoftware.com/) ábrázolása és elemezni az eredményeket az OpenFOAM feladat. Képi megjelenítés és EnSight animációt, bővebben: Ez a [Videó útmutató](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html).

1.  Miután telepítette a EnSight a központi csomópontra, indítsa el.

2.  Nyissa meg a C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.

    Az megjelenítő tartály látni.

    ![A EnSight tartály][tank]

3.  Egy **Isosurface** **internalMesh**létrehozása, és válassza a változó **alpha_water**.

    ![Hozzon létre egy isosurface][isosurface]

4.  A szín megadása az előző lépésben létrehozott **Isosurface_part** . Ha például állítsa be víz kék.

    ![Isosurface színének módosítása][isosurface_color]

5.  Az **Iso-mennyiségi** **Falak** a **kijelzők** panel **Falak** választva hozhat létre, és kattintson az eszköztár **Isosurfaces** gombra.

6.  A párbeszédpanelen ** **Isovolume** , válassza ki** , és állítsa a Min **Isovolume** tartomány 0,5. A isovolume létrehozásához kattintson a **Create a részeket**.

7.  A szín megadása az előző lépésben létrehozott **Iso_volume_part** . Ha például állítsa be mély vízbe kék.

8.  A **Falak**beállítani. Állítsa be átlátszó fehér például.

9. Kattintson a **Lejátszás** a szimulációk eredmények megjelenítéséhez.

    ![Tartály eredménye][tank_result]

## <a name="sample-files"></a>Mintafájlok

### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Minta XML-konfigurációs fájl által PowerShell-parancsprogramot fürt telepítéshez

 ```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>suse12rdmavnet</VNetName>
    <SubnetName>SUSE12RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>SUSE12RDMA-HN</VMName>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>SUSE12RDMA-LN%1%</VMNamePattern>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <NodeCount>2</NodeCount>
      <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
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
### <a name="sample-silentcfg-file-to-install-mpi"></a>Mintafájl silent.cfg MPI telepítése

```
# Patterns used to check silent configuration file
#
# anythingpat - any string
# filepat     - the file location pattern (/file/location/to/license.lic)
# lspat       - the license server address pattern (0123@hostname)
# snpat       - the serial number pattern (ABCD-01234567)

# accept EULA, valid values are: {accept, decline}
ACCEPT_EULA=accept

# optional error behavior, valid values are: {yes, no}
CONTINUE_WITH_OPTIONAL_ERROR=yes

# install location, valid values are: {/opt/intel, filepat}
PSET_INSTALL_DIR=/opt/intel

# continue with overwrite of existing installation directory, valid values are: {yes, no}
CONTINUE_WITH_INSTALLDIR_OVERWRITE=yes

# list of components to install, valid values are: {ALL, DEFAULTS, anythingpat}
COMPONENTS=DEFAULTS

# installation mode, valid values are: {install, modify, repair, uninstall}
PSET_MODE=install

# directory for non-RPM database, valid values are: {filepat}
#NONRPM_DB_DIR=filepat

# Serial number, valid values are: {snpat}
#ACTIVATION_SERIAL_NUMBER=snpat

# License file or license server, valid values are: {lspat, filepat}
#ACTIVATION_LICENSE_FILE=

# Activation type, valid values are: {exist_lic, license_server, license_file, trial_lic, serial_number}
ACTIVATION_TYPE=trial_lic

# Path to the cluster description file, valid values are: {filepat}
#CLUSTER_INSTALL_MACHINES_FILE=filepat

# Intel(R) Software Improvement Program opt-in, valid values are: {yes, no}
PHONEHOME_SEND_USAGE_DATA=no

# Perform validation of digital signatures of RPM files, valid values are: {yes, no}
SIGNING_ENABLED=yes

# Select yes to enable mpi-selector integration, valid values are: {yes, no}
ENVIRONMENT_REG_MPI_ENV=no

# Select yes to update ld.so.conf, valid values are: {yes, no}
ENVIRONMENT_LD_SO_CONF=no


```

### <a name="sample-settingssh-script"></a>Settings.sh mintaparancsfájl

```
#!/bin/bash

# impi
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# openfoam
export FOAM_INST_DIR=/opt/OpenFOAM
source /opt/OpenFOAM/OpenFOAM-2.3.1/etc/bashrc
export WM_MPLIB=INTELMPI
```


###<a name="sample-hpcimpirunsh-script"></a>Hpcimpirun.sh mintaparancsfájl

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"

# Set mpirun runtime evironment
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# mpirun command
MPIRUN=mpirun
# Argument of "--hostfile"
NODELIST_OPT="--hostfile"
# Argument of "-np"
NUMPROCESS_OPT="-np"

# Get node information from ENVs
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # CCP_NODES_CORES is not found or is empty, just run the mpirun without hostfile arg.
    ${MPIRUN} $*
else
    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$

    # Get every node name and write into the hostfile file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the mpirun with hostfile arg
    ${MPIRUN} ${NUMPROCESS_OPT} ${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}

```





<!--Image references-->
[keygen]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/keygen.png
[keys]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/keys.png
[step_variables]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/step_variables.png
[data_files]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/data_files.png
[decompose]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/decompose.png
[job_details]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/job_details.png
[job_resources]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/job_resources.png
[task_details1]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/task_details1.png
[task_dependencies]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/task_dependencies.png
[creds]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/creds.png
[heat_map]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/heat_map.png
[tank]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/tank.png
[tank_result]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/tank_result.png
[isosurface]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/isosurface.png
[isosurface_color]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/isosurface_color.png
[linux_processes]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/linux_processes.png

<properties
 pageTitle="MPI alkalmazások futtatásához a Windows RDMA fürtre beállítása |} Microsoft Azure"
 description="Megtudhatja, hogy miként méretű H16r, H16mr, A8 vagy A9 VMs használni az Azure RDMA hálózati MPI alkalmazások futtatásához a Windows HPC csomag fürtre létrehozásához."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="09/20/2016"
 ms.author="danlep"/>

# <a name="set-up-a-windows-rdma-cluster-with-hpc-pack-to-run-mpi-applications"></a>MPI alkalmazások futtatásához a Windows RDMA fürtre HPC Pack beállítása

Állítsa be a Windows RDMA fürtre [Microsoft HPC csomag](https://technet.microsoft.com/library/cc514029) és [H – vagy számítási igényű A-sorozat példányok](virtual-machines-windows-a8-a9-a10-a11-specs.md) Azure-ban a párhuzamos üzenet továbbítása felület (MPI) alkalmazások futtatásához. Egy HPC csomag fürt RDMA internetezésre alkalmas, a Windows Server-alapú csomópontjai beállításakor MPI alkalmazások üzletfelekkel fölé egy kis késés, a nagy átviteli hálózat távoli közvetlen memória hozzáférési (RDMA) technológiát alapuló Azure-ban.

Ha meg szeretné futtatni a Azure RDMA hálózathoz Linux VMs MPI munkaterhelésekből, lásd: [MPI alkalmazások futtatásához Linux RDMA fürt beállítása](virtual-machines-linux-classic-rdma-cluster.md).


## <a name="hpc-pack-cluster-deployment-options"></a>HPC Pack fürt telepítési lehetőségek
A Microsoft HPC csomag egy olyan eszköz, feltéve hozhat létre további áttérhet HPC fürtök a helyszíni, vagy a Windows vagy Linux HPC alkalmazások futtatásához Azure-ban. HPC csomag tartalmazza a futtatókörnyezet az üzenet továbbítása felület a Windows (MS-MPI) Microsoft végrehajtásához. A támogatott a Windows Server operációs rendszert futtató RDMA internetezésre alkalmas példányok használatakor HPC csomag lehetővé teszi hatékony Azure RDMA hálózatra Windows MPI alkalmazások futtatásához. 

Ez a cikk bemutatja két esetek és hivatkozások állíthatja be a Microsoft HPC Pack Winodws RDMA fürt részletes útmutatást. 

* Eset: 1. Számítási igényű dolgozói szerepkör példányok (PaaS) telepítése

* Eset: 2. A számítási igényű VMs (IaaS) számítási csomópontok terjesztése

[Az általános előfeltételei számítási igényű példányok használata Windows kapcsolatos tudnivalók](virtual-machines-windows-a8-a9-a10-a11-specs.md) című részben H – adatsorok és a számítási igényű A-sorozat VMs.



## <a name="scenario-1-deploy-compute-intensive-worker-role-instances-paas"></a>Eset: 1. Számítási igénylő dolgozó szerepkör példányok (PaaS) telepítése

A meglévő HPC csomag fürthöz erőforrásokat további számítási Azure dolgozó szerepkör esetekben (Azure csomópontok) futó egy felhőalapú szolgáltatásba (PaaS). Ezt a funkciót, más néven "burst az Azure" HPC csomagból, a dolgozók szerepkör-példányok méretű adattartomány támogatja. Azure csomópontok hozzáadásakor egyszerűen adja meg a RDMA internetezésre alkalmas méretek közül.

Az alábbiakban szempontot, és a lépéseket követve burst RDMA internetezésre alkalmas az Azure példányokat egy meglévő (általában a helyszíni) fürt. Szerepkör-példányok dolgozói felvétele az Azure virtuális a telepített fő HPC Pack-csomópont hasonló eljárások segítségével.

>[AZURE.NOTE] Az Azure HPC Pack burst az oktatóanyagot olvassa el a [hibrid fürt HPC Pack beállítása](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)című témakört. Vegye figyelembe az alábbi lépéseket szemléltető vonatkozó kifejezetten RDMA internetezésre alkalmas Azure csomópontot.

![Az Azure burst][burst]

### <a name="steps"></a>Lépések

4. **Üzembe helyezéséhez és egy központi HPC csomag 2012 R2-csomópont konfigurálása**

    Töltse le a legújabb HPC Pack telepítőcsomag a [Microsoft letöltőközpontból](https://www.microsoft.com/download/details.aspx?id=49922). Követelmények és Felkészülés az Azure burst telepítési utasításokat című témakörben talál [HPC csomag – első lépések](https://technet.microsoft.com/library/jj884144.aspx) és [Azure dolgozó példányaiban Microsoft HPC Pack Burst](https://technet.microsoft.com/library/gg481749.aspx).

5. **A kezelés-tanúsítvány beállítása az Azure előfizetés**

    A fő csomópont és Azure közötti kapcsolat biztonságos-tanúsítvány beállítása. Beállítások és eljárásokat című témakörben talál [az Azure-kezelés tanúsítvány beállítása HPC csomag helyzetekben](http://technet.microsoft.com/library/gg481759.aspx). Próba-telepítésekről HPC csomag telepíti az alapértelmezett Microsoft HPC Azure Management tanúsítvány gyorsan feltöltheti Azure-előfizetéséhez.

6. **Hozzon létre egy új, felhőalapú szolgáltatásba, és a tárterület-fiók**

    Az Azure klasszikus portal segítségével hozzon létre egy felhőalapú szolgáltatásba, és a telepítéshez tároló fiókkal hol érhetők el a RDMA internetezésre alkalmas példányok területen.

7. **Azure csomópont-sablon létrehozása**

    Használja a csomópont sablon létrehozása varázsló HPC felülete. Című témakör tartalmazza [az Azure csomópont-sablon létrehozása](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Templ) a "Lépéseket az üzembe Azure csomópontok a Microsoft HPC csomag".

    A kezdeti tesztek ajánlott kézi elérhetősége házirend beállítása a sablonban.

8. **Csomópontok hozzáadása a fürthöz**

    Használja a csomópont hozzáadása varázsló HPC felülete. További tudnivalókért lásd: [Azure csomópontok hozzáadása a Windows HPC fürthöz](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Add).

    A csomópontok méretének megadása esetén jelölje ki azt a RDMA internetezésre alkalmas példány méretét.
    
    >[AZURE.NOTE]HPC Pack minden burst az Azure környezet, amelyben a számítási igényű példányok, automatikusan üzembe helyezése (például A8) 2 RDMA internetezésre alkalmas esetek a minimális proxy csomópontok mellett az Azure dolgozói szerepkör-példányok, adja meg. A proxy csomópontok, amely az előfizetéshez van rendelve, és együtt az Azure dolgozó szerepkör-példányok díjak merülnek fel magmintákat használja.

9. **Kezdés (rendelkezés) a csomópontok füleket online feladat futtatása**

    Jelölje ki a csomópontok, és használja a **indítása** műveletet HPC felülete. Befejeződésekor kiépítési, jelölje be a csomópontok, és a művelettel **Online állapotba** HPC felülete. A csomópontok készen feladatok futtatása.

10. **A fürt feladatok kezdeményezése**

    Eszközeivel HPC csomag feladat Beküldési fürt feladatok futtatásához. Lásd: [Microsoft HPC csomag: adatkezelési feladat](http://technet.microsoft.com/library/jj899585.aspx).

11. **Leállítás (deprovision) a csomópontok**

    Ha be szeretné fejezni futó feladatok, a csomópontok offline állapotba, és használja a **leállítása** műveletet HPC felülete.





## <a name="scenario-2-deploy-compute-nodes-in-compute-intensive-vms-iaas"></a>Eset: 2. A számítási igényű VMs (IaaS) számítási csomópontok terjesztése

Ebben az esetben a HPC csomag fő csomópont rendszerbe, és fürt VMs tartományhoz az Active Directory-Azure virtuális hálózaton található csomópontok számítja ki. HPC csomag többféle [Azure VMs telepítési beállítások](virtual-machines-linux-hpcpack-cluster-options.md), például automatikus telepítési parancsfájlok és Azure quickstart útmutató sablonokat. Példaként szempontot, és végre az alábbi lépéseket segítenek a [HPC csomag IaaS telepítési parancsfájlt](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) használatával automatizálhatja a legtöbb ezt a folyamatot.

![Az Azure VMs fürthöz][iaas]



### <a name="steps"></a>Lépések

1. **Központi csomópont létrehozása és csomópont VMs kiszámítása a HPC csomag IaaS telepítési parancsfájlt futtatásával egy ügyfélszámítógépen**

    Töltse le a HPC Pack IaaS telepítési parancsfájlt csomagot a [Microsoft letöltőközpontból](https://www.microsoft.com/download/details.aspx?id=49922).

    Az ügyfélgép előkészítése a parancsprogram-konfigurációs fájl létrehozása, és futtassa a, [a HPC csomag IaaS telepítési parancsfájlt egy HPC fürtre létrehozása](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md)című témakör tartalmaz. 
    
    Üzembe RDMA internetezésre alkalmas számítási csomópontok, vegye figyelembe az alábbi szempontokat:
    
    * **Virtuális hálózati** – új virtuális hálózat megadása egy tartomány, amelyben a használni kívánt RDMA internetezésre alkalmas példány méret érhető el.

    * A **Windows Server operációs rendszer** - RDMA kapcsolódási támogatja, adja meg a Windows Server 2012 R2 vagy Windows Server 2012 operációs rendszert, a számítási csomópont VMs.

    * **Cloud services** - javasoljuk, hogy a központi csomópontjának egy felhőalapú szolgáltatásba, és a számítási csomópontok valamelyik másik felhőszolgáltatásában üzembe helyezése.

    * **Címsor csomópont méretének** - ebben az esetben, érdemes megfontolni a méret legalább A4 (plusz nagy) számára a központi csomópontot.

    * **HpcVmDrivers kiterjesztés** – a telepítési parancsfájlt telepíti az Azure virtuális ügynök és a HpcVmDrivers bővítmény automatikusan A8 vagy Windows Server operációs rendszerű csomópontok kiszámítania A9 telepítésekor. HpcVmDrivers, hogy a RDMA hálózathoz csatlakozhassanak telepíti a számítási csomóponton VMs illesztőprogramokat.

    * **Fürt hálózati konfigurálásról** – a telepítési parancsfájlt automatikusan beállítja a HPC csomag fürt topológia 5 (a vállalati hálózaton csomópontjait). Ez a topológia szükség VMs telepítési, az összes HPC csomag fürt. Ne módosítsa a fürt hálózati topológiája később.

2. **A számítási csomópontok online előtérbe feladatok futtatása**

    Jelölje ki a csomópontok, és a művelettel **Online állapotba** HPC felülete. A csomópontok készen feladatok futtatása.

3. **A fürt feladatok kezdeményezése**

    A fő csomópont feladatok küldhetik csatlakozni, vagy egy helyi számítógép beállítása ehhez. Tudnivalókért lásd: a [Elküldése feladatok HPC fürthöz Azure-ban](virtual-machines-windows-hpcpack-cluster-submit-jobs.md).

4. **A csomópontok offline állapotba helyezni, és vége (felszabadítása) őket**

    Ha be szeretné fejezni futó feladatokat, hogy kapcsolat nélküli csomópontok HPC felülete. Ezután az Azure-kezelő eszközök segítségével leállítása őket.



## <a name="run-mpi-applications-on-the-cluster"></a>MPI alkalmazások futtatásához a fürthöz

### <a name="example-run-mpipingpong-on-an-hpc-pack-cluster"></a>Példa: Mpipingpong futtatni egy HPC Pack fürthöz

Annak ellenőrzéséhez, egy HPC Pack telepítési RDMA internetezésre alkalmas példányok, futtassa a fürt a HPC Pack **mpipingpong** parancsot. **mpipingpong** küld a csomagok között párosított csomópontok többször késés és az átvitt mérések kiszámítása és az alkalmazás RDMA engedélyező hálózati statisztikai adatok. Ez a példa egy MPI feladat (ebben az esetben az **mpipingpong**) futtatásához a fürt **mpiexec** parancs használatával tipikus mintát mutat.

Ez a példa feltételezi, hogy hozzáadta a Azure csomópontok "burst az Azure" konfigurációban ([esetben 1](#scenario-1.-deploy-compute-intensive-worker-role-instances-(PaaS) in this article). Ha telepítette az Azure VMs fürtre HPC csomagot, kell a parancs szintaxis adjon meg egy másik csomópont csoportot, és további környezeti változók irányítsa át a hálózati forgalmat a RDMA hálózathoz állítsa módosításával.


A fürt mpipingpong futtatása


1. A központi csomópontra, vagy egy megfelelően konfigurált ügyfélszámítógépen nyisson meg egy parancssort.

2. Becsült időtartam-4-csomópontok Azure burst-példányhoz található csomópontok pár között, írja be a következő parancsot a kisvállalati csomag mérete és sok közelítések mpipingpong futtatásához feladat elküldése:

    ```
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 1:100000 -op -s nul
    ```

    A parancs a feladat, amely elküldése Azonosítóját adja vissza.

    Ha a telepített Azure VMs, adjon meg egy csomópont csoportot tartalmazó HPC csomag fürt telepítette számítja ki egy felhőalapú szolgáltatás üzembe VMs csomópontot, és módosítsa a **mpiexec** parancs a következőképpen:

    ```
    job submit /nodegroup:vmcomputenodes /numnodes:4 mpiexec -c 1 -affinity -env MSMPI_DISABLE_SOCK 1 -env MSMPI_PRECONNECT all -env MPICH_NETMASK 172.16.0.0/255.255.0.0 mpipingpong -p 1:100000 -op -s nul
    ```

3. A feladat befejezésekor tekintheti meg a kimenet (ebben az esetben a eredménye 1 feladat a feladat), írja be az alábbi

    ```
    task view <JobID>.1
    ```

    Ha &lt; *JobID* &gt; a feladat beadásának azonosítója.

    A kimenet késés eredménye az alábbihoz hasonló tartalmazni fogja.

    ![Ping pong időtartama][pingpong1]

4. Azure burst csomópontok közötti átviteli becsléséhez, írja be a feladat futtatása **mpipingpong** egy nagy csomag mérete és iterációk száma, kis számmal elküldése a következő parancsot:

    ```
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 4000000:1000 -op -s nul
    ```

    A parancs a feladat, amely elküldése Azonosítóját adja vissza.

    Egy HPC csomag fürtre Azure VMs rendszerre telepíthető módosítsa a parancs, amint lépés: 2.

5. A feladat befejezésekor tekintheti meg a kimenet (ebben az esetben a eredménye 1 feladat a feladat), írja be a következőt:

    ```
    task view <JobID>.1
    ```

  A kimenet átviteli eredménye az alábbihoz hasonló tartalmazni fogja.

  ![Ping pong kapacitása][pingpong2]


### <a name="mpi-application-considerations"></a>MPI alkalmazása előtt megfontolandó kérdések


Az alábbiakban szempontok MPI alkalmazások futtatásához HPC Pack Azure-ban. Néhány csak telepítések Azure csomópontok (dolgozó szerepkör példányok "burst az Azure" konfigurációban hozzáadott) vonatkoznak.

* Egy felhőalapú szolgáltatásba dolgozó szerepkör példányok vannak rendszeres reprovisioned értesítés nélkül Azure által (például karbantartási, vagy egy példány nem jelenik meg). Ha egy példány reprovisioned van, egy MPI feladat futtatása közben, a példány elveszti az adatok, és adja eredményül az állapotot, ha először telepítették, ami okozhat a MPI feladat meghiúsító. A használható MPI egyetlen feladat számára, és annál hosszabb ideig a feladat további csomópontok fut, annál valószínű, hogy egy példányok fog kell reprovisioned közben a feladat futtatása. Is vegye figyelembe a Ha a telepítő egy csomópontjának megjelölése fájlkiszolgálóra.


* Azure MPI feladatok futtatásához nincs RDMA internetezésre alkalmas példányokat használja. Bármilyen példány méretű HPC csomag által támogatott is használhatja. A RDMA internetezésre alkalmas példányok azonban a kis-és nagybetűket a késés és a csomópontok csatlakozó hálózati sávszélesség viszonylag nagyméretű MPI feladatok futó ajánlott. Más méretű használatával időtartama és sávszélesség-érzékeny MPI feladat futtatása, azt javasoljuk kis feladatok, amelyben egyetlen feladat futtat, a csak néhány csomópontok futtatása.

* Azure példányaiban rendszerbe állított alkalmazásokat az alkalmazással társított licencfeltételeket vonatkoznak. Jelölje be a szállítóval bármely kereskedelmi alkalmazás futtatásához a felhőben licencelési vagy más korlátozásokat. Nem minden szállítók kínálnak befizetések licencelési.


* Azure példányok hozzáférést a helyszíni csomópontok, a megosztás és a licenc-kiszolgálók további beállítás szükséges. Ahhoz, hogy az Azure csomópontok egy helyszíni licenc kiszolgáló elérésére, például egy webhely Azure virtuális hálózati konfigurálható tulajdonsággal.


* MPI alkalmazások futtatásához Azure-példányok regisztrálása minden egyes MPI alkalmazást a Windows tűzfal példányokat a a **hpcfwutil** parancs futtatásával. Egy olyan portot, a tűzfal dinamikusan rendelt helyébe MPI kommunikáció lehetővé teszi.

    >[AZURE.NOTE] Az Azure telepítések burst is beállíthatja az új Azure csomópontjait bekerülnek a fürthöz automatikusan futni tűzfal kivétel parancsot. Miután a **hpcfwutil** parancsot, és ellenőrizze, hogy működik-e az alkalmazást, az Azure csomópontok indítási parancsfájl vegye fel a parancsot. További tudnivalókért olvassa el a [Azure csomópontok indítási parancsfájl használata](https://technet.microsoft.com/library/jj899632.aspx)című témakört.



* HPC csomag a CCP_MPI_NETMASK fürt környezeti változó megadhatja a MPI kommunikáció elfogadható címeket használja. HPC csomag 2012 R2 kezdve, a CCP_MPI_NETMASK fürt környezeti változó csak MPI kommunikációt érinti a tartományhoz fürt számítási csomópontok között (vagy a helyszíni, vagy az Azure VMs). A változó hozzáadva Azure konfigurációs burst csomópontok figyelmen kívül hagyja.


* MPI feladatok nem különböző Azure példányok futtatása különböző felhőalapú szolgáltatások (például a különböző csomópont sablonok vagy Azure virtuális számítási csomópontok telepítését több felhőszolgáltatások az Azure telepítések burst) telepített. Ha több különböző csomópont sablonokkal elindító Azure csomópont-telepítés, csak egy meg Azure csomópontok a MPI feladatot kell futtatni.


* Amikor online állapotba és Azure csomópontok hozzáadása a fürthöz, a HPC feladat Feladatütemező szolgáltatás azonnal megpróbálja elindítani feladatok a csomóponton. Ha csak egy részét a terhelést a Azure futtatását is lehetővé teszi, győződjön meg arról, hogy frissítése vagy meghatározásához, hogy milyen típusú a futtathatók Azure feladat sablonok létrehozása. Annak érdekében, hogy a feladat sablonnal benyújtott feladatok csak futtatása Azure csomópontok, például a csomópont csoportok tulajdonság hozzáadása a feladat sablon, és a AzureNodes válassza a szükséges értéket. A Hozzáadás-HpcGroup HPC PowerShell-parancsmag segítségével az Azure csomópontok egyéni csoportokat hozhat létre.


## <a name="next-steps"></a>Következő lépések

* HPC csomag helyett elkészítése az Azure köteg szolgáltatással MPI alkalmazások futtatásához felügyelt készletek számítási csomópontok Azure-ban. Lásd: [használata több példány feladatok Azure köteg az üzenet továbbítása felület (MPI) alkalmazások futtatásához](../batch/batch-mpi.md).

* Ha a használni kívánt Linux MPI alkalmazásokat, amelyek az Azure RDMA hálózat elérését, olvassa el a [MPI alkalmazások futtatásához Linux RDMA fürt beállítása](virtual-machines-linux-classic-rdma-cluster.md)című témakört.

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/burst.png
[iaas]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/iaas.png
[pingpong1]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/pingpong1.png
[pingpong2]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/pingpong2.png
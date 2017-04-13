<properties
 pageTitle="Egy HPC csomag fő csomópont létrehozása az Azure virtuális |} Microsoft Azure"
 description="Megtudhatja, hogy miként használhatja az Azure-portál és az erőforrás-kezelő telepítési modell egy Microsoft HPC Pack fő csomópont létrehozása az Azure virtuális."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="08/17/2016"
 ms.author="danlep"/>

# <a name="create-the-head-node-of-an-hpc-pack-cluster-in-an-azure-vm-with-a-marketplace-image"></a>A központi csomópont HPC csomag fürt létrehozása az Azure virtuális piactér képével


A [Microsoft HPC csomag virtuális gép képe](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) az Azure piactéren elérhető és az Azure portal segítségével HPC fürt a központi csomópontot. Ezen a képen HPC csomag virtuális HPC Pack 2012 R2 frissítés 3 megtalálható előtelepítve a Windows Server 2012 R2 adatközponthoz alapul. A központi csomópont használata egy igazolása fogalmat telepítésének HPC csomag Azure-ban. Számítási csomópontok majd hozzáadása a fürt HPC munkaterhelésekből futtatásához.



>[AZURE.TIP]A teljes HPC csomag fürtre, amely tartalmazza a központi csomópontot, és a számítási csomópontokat Azure-ban üzembe helyezéséhez javasoljuk, automatikus módszert egy. Megadható többek között a [HPC csomag IaaS telepítési parancsfájlt](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) , és a [Windows-munkaterhelésekből HPC csomag fürthöz](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/) erőforrás-kezelő sablont. További sablonok [HPC csomag fürt beállítás Azure](virtual-machines-windows-hpcpack-cluster-options.md) látható. 


## <a name="planning-considerations"></a>Tervezési szempontjait

Ahogy az alábbi ábrán látható, az Active Directory tartományi Azure virtuális hálózatban a HPC Pack központi csomóponthoz rendszerbe.

![Fő csomópont HPC csomag][headnode]

* **Az active Directory tartományi** – a HPC csomag fő csomópont kell csatlakoztatni egy az Azure Active Directory-tartománya HPC szolgáltatások a virtuális a Kezdés előtt. Lásd az ebben a cikkben az igazolás, amely a telepítési előléptetheti a virtuális hoz létre a központi csomópontot a tartományvezérlőnek a HPC szolgáltatások indítása előtt. Egy másik, hogy külön tartományvezérlőnek és erdő, amelyhez csatlakozik a fő csomópont virtuális Azure-ban.

* **Azure virtuális hálózat** - telepítéshez használni a fő csomópont az erőforrás-kezelő telepítési modell használatakor, akkor adja meg, vagy hozzon létre egy Azure virtuális hálózat. Ha módosítani szeretné a fő csomópont csatlakozzon egy meglévő Active Directory tartományi használhatja a virtuális hálózat. Meg kell, hogy később számítási csomópont VMs hozzáadása a fürthöz.

    
## <a name="steps-to-create-the-head-node"></a>A fő csomópont létrehozásának lépései

Az alábbiakban az Azure portal segítségével a HPC csomag központi csomópont-Azure virtuális létrehozása az erőforrás-kezelő telepítési modell használatával a magas szintű lépéseket. 


1. Ha szeretne egy új Active Directory-erdőt létrehozása az Azure külön tartományvezérlőnek VMs, egy, hogy egy [erőforrás-kezelő sablont](https://azure.microsoft.com/documentation/templates/active-directory-new-domain-ha-2-dc/). Egy egyszerű igazolására fogalmat telepítési azt nem kell aggódnia kihagyhatja ezt a lépést, és állítsa be a a központi csomópontot virtuális magát egy tartományvezérlőnek. Ez a beállítás ismertetése a jelen cikk.
    
2. Kattintson a [HPC csomag 2012 R2 a Windows Server 2012 R2 lapon](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) a Microsoft Azure piactéren található, **Hozzon létre virtuális gépen**. 

3. A portál **HPC csomag 2012 R2 a Windows Server 2012 R2** lapon jelölje be az **Erőforrás-kezelő** telepítési modell, és kattintson a **Létrehozás**gombra.

    ![HPC csomag képe][marketplace]

4. A portál használatával adja meg a beállításokat, és a virtuális létrehozása. Ha most használja először a Azure, kövesse az oktatóanyagot, [Hozzon létre egy Windows virtuális gép az Azure-portálon](virtual-machines-windows-hero-tutorial.md). Fogalom telepítési igazolása, általában elfogadhatja az alapértelmezett és ajánlott beállítások.

    >[AZURE.NOTE]Ha egy meglévő Active Directory-tartományhoz Azure-ban a fő csomópont csatlakozni szeretne, ellenőrizze a virtuális létrehozásakor adja meg a tartományhoz tartozó virtuális hálózat.
       
4. Miután létrehozta a virtuális, és a virtuális fut, [a virtuális csatlakozás](virtual-machines-windows-connect-logon.md) távoli asztal. 

5. A virtuális csatlakozzon egy meglévő tartományerdő, vagy hozzon létre egy tartományerdő a virtuális magát a.

    * A virtuális egy meglévő tartományerdő Azure virtuális hálózattal hozta létre, ha bekapcsolódni a virtuális az erdőhöz szabványos Kiszolgálókezelő vagy a Windows PowerShell-eszközöket. Indítsa újra.

    * Ha a virtuális létrehozott új virtuális hálózatban (nélkül egy meglévő tartományerdő), majd a virtuális előléptetése a tartományvezérlőnek. Szabványos lépések segítségével telepítése és konfigurálása az Active Directory tartományi szolgáltatások szerepkör a központi csomópontra. A lépések részletes leírását olvassa el a [egy új Windows Server 2012 Active Directory erdő telepítése](https://technet.microsoft.com/library/jj574166.aspx)című témakört.

5. A virtuális fut, és csatlakozik, az Active Directory erdő, elkezdje HPC csomag szolgáltatások az alábbi képlettel történik:

    egy. A központi csomópontot, amely a helyi Rendszergazdák csoport tagjának tartományi fiók segítségével virtuális csatlakozni. Ha például használja a rendszergazdai fiók beállítása a fő csomópont virtuális létrehozásakor.

    b. Alapértelmezett fő csomópont konfiguráció indítsa el a Windows PowerShell rendszergazdaként, és írja be a következőt:

    ```
    & $env:CCP_HOME\bin\HPCHNPrepare.ps1 –DBServerInstance ".\ComputeCluster"
    ```

    Eltarthat néhány percig, amíg a HPC csomag szolgáltatások indításához.

    További fő csomópont beállításokról, írja be a `get-help HPCHNPrepare.ps1`.


## <a name="next-steps"></a>Következő lépések

* Most már dolgozhat a HPC csomag fürt a központi csomópontot. Például HPC felülete indítása, és fejezze be a [Telepítési feladatlista](https://technet.microsoft.com/library/jj884141.aspx).
* Ha növelni szeretné a fürt kiszámítania kapacitás igény [Azure burst csomópontok](virtual-machines-windows-classic-hpcpack-cluster-node-burst.md) vehet fel egy felhőalapú szolgáltatásba. 

* Futtassa a próba terhelést a fürt. Egy példa című témakörben HPC csomag [használatának megkezdését segítő útmutatót](https://technet.microsoft.com/library/jj884144).

<!--Image references-->
[headnode]: ./media/virtual-machines-windows-hpcpack-cluster-headnode/headnode.png
[marketplace]: ./media/virtual-machines-windows-hpcpack-cluster-headnode/marketplace.png

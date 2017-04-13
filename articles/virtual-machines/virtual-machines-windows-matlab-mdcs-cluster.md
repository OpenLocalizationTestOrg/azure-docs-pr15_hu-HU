<properties
   pageTitle="Virtuális gépeken fürtök MATLAB |} Microsoft Azure"
   description="A számítási igényű párhuzamos MATLAB munkaterhelésekből futtatásához MATLAB elosztott Computing kiszolgáló fürt létrehozása a Microsoft Azure virtuális gépeken futó segítségével"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="mscurrell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="Windows"
   ms.workload="infrastructure-services"
   ms.date="05/09/2016"
   ms.author="markscu"/>

# <a name="create-matlab-distributed-computing-server-clusters-on-azure-vms"></a>Azure VMs MATLAB elosztott számítások kiszolgáló fürt létrehozása 

Microsoft Azure virtuális gépeken futó használatával hozzon létre egy vagy több MATLAB elosztott Computing kiszolgáló fürtre a számítási igényű párhuzamos MATLAB munkaterhelésekből futtatásához. A MATLAB elosztott számítások kiszolgáló szoftver telepítése egy virtuális alap képként és üzembe helyezéséhez és kezeléséhez, a fürt az Azure quickstart útmutató sablon vagy az Azure PowerShell parancsprogramot ( [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster)érhető el) használatával. A telepítés után a fürt futtatásához a munkaterhelésekből csatlakozni. 

## <a name="about-matlab-and-matlab-distributed-computing-server"></a>Tudnivalók a MATLAB és elosztott MATLAB számítások kiszolgáló 

A [MATLAB](http://www.mathworks.com/products/matlab/) platform tárolására van optimalizálva mérnöki és tudományos problémák megoldása Nagyméretű szimulációk és adatkezelési feladatok MATLAB felhasználók MathWorks párhuzamos számítások termékek segítségével a számítási igényű munkaterhelésekből gyorsítása kihasználva számítási fürt és a rács szolgáltatások. [A párhuzamos számítások eszközkészlet](http://www.mathworks.com/products/parallel-computing/) lehetővé teszi a felhasználóknak MATLAB, alkalmazások parallelize és kihasználhatja a több elem core processzorok, grafikai processzor, és fürt számítja ki. [MATLAB elosztott számítások kiszolgáló](http://www.mathworks.com/products/distriben/) lehetővé teszi a felhasználóknak MATLAB számos számítógépére egy számítási fürt csatlakozást. 


Azure virtuális gépeken futó használatával, amelyek azonos küldhetik párhuzamos munkát, a helyszíni fürt, például interaktív feladatok, a köteg feladatok, a független tevékenységek és a kommunikációs feladatok mechanizmusok MATLAB elosztott számítások kiszolgáló fürt is létrehozhat. Számos előnyt összehasonlítva kiépítési Azure használata a MATLAB platform együtt rendelkezik, és a hagyományos használata a helyszíni hardver: virtuális gép adattartomány méretek, a fürt igény szerinti így fizet csak a számítási erőforrásokért meg használni, és az azt jelenti, hogy tesztelje a méretezés modellek létrehozása.  

## <a name="prerequisites"></a>Előfeltételek

* Az **ügyfélszámítógép** - szüksége lesz a Windows-alapú ügyfélszámítógép kommunikáció Azure és a MATLAB elosztott számítások kiszolgáló fürt telepítés után. 

* **Azure PowerShell** - lásd: [Telepítse és állítsa be a Azure PowerShell hogyan](../powershell-install-configure.md) telepítheti az ügyfélgépen beállított. 

* **Azure előfizetés** – Ha nincs telepítve az előfizetést, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/) az mindössze néhány perc. Nagyobb fürt érdemes megfontolni befizetések előfizetés vagy vételi lehetőségeket. 

* **Magmintákat kvóta** - növelése egy nagy fürt vagy több MATLAB elosztott számítások kiszolgáló fürt üzembe core kvóta kell. [Nyissa meg az online ügyfél-támogatási kérelmet](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) ingyenesen kvóta növeléséhez. 

* **MATLAB, a párhuzamos számítások eszközkészlet és MATLAB elosztott számítások kiszolgáló licencek** – a parancsfájlok feltételezik, hogy a [Szolgáltatott licenc Manager MathWorks](http://www.mathworks.com/products/parallel-computing/mathworks-hosted-license-manager/) használja a összes licenc.  

* A virtuális a fürt VMs az alap virtuális képe használt **MATLAB elosztott számítások kiszolgáló szoftver** - lesz telepítve. 


## <a name="high-level-steps"></a>Magas szintű lépések

A MATLAB elosztott számítások kiszolgáló fürt Azure virtuális gépeken futó használ, a következőképpen magas szintű szükség. Részletes útmutatást a quickstart útmutató sablon és parancsfájlok tartozó [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster)a dokumentáció szerepelnek.

1. **Hozzon létre egy alap virtuális képe**  
    * Letöltheti és telepítheti a virtuális MATLAB elosztott számítások kiszolgáló szoftvereinek. 

    >[AZURE.NOTE]Is igénybe vehet az óra, néhány, de csak a teendő, miután MATLAB különböző verzióinak használata van.   
    
2. **Hozzon létre egy vagy több fürthöz**  
    * A megadott PowerShell-parancsprogramot, vagy hozzon létre egy fürt az alap virtuális képről a quickstart útmutató sablonnal.   
    * A használatával a megadott PowerShell-parancsprogramot, amely lehetővé teszi, hogy a listában, mutasson az egérrel, önéletrajz fürt kezelése, és törölje a fürt. 
 
## <a name="cluster-configurations"></a>Fürt konfigurációk 

Jelenleg a fürt létrehozási parancsfájlt, és a sablon lehetővé teszi hozzon létre egy egyetlen MATLAB elosztott számítások kiszolgáló topológia. Ha azt szeretné, hozzon létre egy vagy több további fürtre minden fürt dolgozó VMs használatáról virtuális különböző méretű, különféle problémákat, és így tovább. 

### <a name="matlab-client-and-cluster-in-azure"></a>MATLAB ügyfél- és fürt Azure-ban 

A MATLAB ügyfél csomópontot, a MATLAB feladat ütemező csomópontot, és a MATLAB elosztott számítások kiszolgáló "dolgozó" csomópontok van az összes konfigurálva Azure VMs virtuális hálózatban, az alábbi ábrán látható módon. 

![Fürt topológiája](./media/virtual-machines-windows-matlab-mdcs-cluster/mdcs_cluster.png)

* A fürt használatához csatlakozás távoli asztali által az ügyfél csomópontot. Az ügyfél csomópontot a MATLAB ügyfél fut. 

* Az ügyfél csomópont összes dolgozók úgy is elérhető fájlmegosztás tartalmaz.

* MathWorks üzemeltetett licenc Manager összes MATLAB szoftver a licenc ellenőrzése szolgál. 

* Alapértelmezés szerint egy MATLAB elosztott számítások kiszolgáló dolgozói per core jön létre a VMs dolgozó, de megadhatja, hogy minden szám. 


## <a name="use-an-azure-based-cluster"></a>Az Azure-alapú fürt használata 

Más típusú MATLAB elosztott számítások kiszolgáló fürt, van szüksége a MATLAB ügyfél (a virtuális ügyfél) fürt felhasználóiprofil-kezelő használatával MATLAB feladat ütemező fürt profil létrehozása.

![Profil felülete](./media/virtual-machines-windows-matlab-mdcs-cluster/cluster_profile_manager.png)

## <a name="next-steps"></a>Következő lépések

* Részletes szeretné üzembe helyezéséhez és kezeléséhez MATLAB elosztott számítások kiszolgáló fürt Azure-ban című cikkben olvashat a [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster) tárat, amelyben a sablonok és parancsfájlok. 

* Nyissa meg a [webhely MathWorks](http://www.mathworks.com/) MATLAB és MATLAB elosztott számítások kiszolgáló részletes dokumentációt.

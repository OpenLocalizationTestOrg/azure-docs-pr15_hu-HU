<properties
 pageTitle="Az Excel és a SOA HPC Pack fürt |} Microsoft Azure"
 description="Első lépések a nagy az Excel és a SOA munkaterhelésekből futó egy HPC csomag fürthöz Azure-ban"
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
 ms.date="08/25/2016"
 ms.author="danlep"/>

# <a name="get-started-running-excel-and-soa-workloads-on-an-hpc-pack-cluster-in-azure"></a>Első lépések az Excel és a SOA munkaterhelésekből futó egy HPC csomag fürthöz Azure-ban

Ez a cikk bemutatja, hogyan Azure virtuális gépeken Microsoft HPC csomag fürt telepítse az Azure quickstart útmutató sablon, vagy másik lehetőségként az Azure PowerShell telepítési parancsfájlt segítségével. A fürt futtassa a Microsoft Excel vagy a szolgáltatás készült architektúra (SOA) munkaterhelésekből HPC csomag készült Azure piactér virtuális képek használja. Egyszerű Excel HPC és SOA szolgáltatások futtathat egy helyszíni ügyfélszámítógép a fürt is használhatja. Az Excel HPC szolgáltatások olyan Excel-munkafüzet mert és a felhasználó által definiált függvények az Excel vagy a UDF.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Magas szintű a következő ábrán a HPC csomag fürt hoz létre.

![Excel-munkaterhelésekből futó csomópontok HPC fürtre][scenario]

## <a name="prerequisites"></a>Előfeltételek

*   Az **ügyfélszámítógép** - szüksége van egy Windows-alapú ügyfélszámítógép minta az Excel és a SOA feladatok a fürthöz küldhetik. Windows rendszerű számítógépről (Ha úgy dönt, hogy telepítési mód) az Azure PowerShell fürt telepítési parancsfájlt futtatásához is szükség van, és

*   **Azure előfizetés** – Ha nincs telepítve az Azure előfizetéssel, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/) az mindössze néhány perc.

*   **Magmintákat kvóta** - szükség lehet az egyes kvóta növelése különösen akkor, ha több fürt csomópontokat a multicore virtuális méretű rendszerbe. Az Azure quickstart útmutató sablont használ, az erőforrás-kezelő magmintákat kvóta esetén Azure régió száma. Ebben az esetben szükség lehet egy adott tartomány kvóta növeléséhez. Lásd: [Azure előfizetés korlátozások, kvótákat, és korlátok](../azure-subscription-service-limits.md). [Nyissa meg az online ügyfél-támogatási kérelmet](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) ingyenesen kvóta növeléséhez.

*   Telepítve van a **Microsoft Office-licenc** – Ha a piactér HPC csomag virtuális kép használata a Microsoft Excel, a Microsoft Excel Professional Plus 2013 30 napos próbaverziója számítási csomópontok rendszerbe. A kiértékelés pont után meg kell adnia a Microsoft Office Excel munkaterhelésekből futtatásának folytatásához aktiválása érvényes licenc. A jelen cikk lásd: az [Excel aktiválási](#excel-activation) . 


## <a name="step-1-set-up-an-hpc-pack-cluster-in-azure"></a>Lépés: 1. Azure-ban egy HPC csomag fürthöz beállítása

Bemutatjuk, hogy a fürt beállítása a két lehetőség közül választhat: első, az Azure quickstart útmutató sablon és az Azure portál; és a második, az Azure PowerShell telepítésének parancsfájl használatával.


### <a name="option-1-use-a-quickstart-template"></a>1 lehetőséget. Quickstart útmutató sablon használata
Az Azure quickstart útmutató sablonnal gyorsan és egyszerűen üzembe egy HPC csomag fürthöz az Azure-portálon. Amikor a portálon nyissa meg a sablont, ahol a beállítások megadása a fürt egyszerű felhasználói Felületet kap. Az alábbiakban a lépéseket. 

>[AZURE.TIP]Ha azt szeretné, az [Azure piactéren elérhető sablonok](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) létrehoz egy hasonló fürt kifejezetten az Excel-munkaterhelésekből. A lépések némileg eltérő a következő.

1.  Keresse fel a [GitHub HPC fürt létrehozása sablon lapján](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster). Ha azt szeretné, tanulmányozza a sablon és a forrás kódot.

2.  Kattintson **az Azure Deploy** a telepítés elindításához a sablonnal az Azure-portálon.

    ![Azure sablon telepítése][github]

3.  A portálon kövesse ezeket a lépéseket követve adja meg a paramétereket a HPC fürt sablon.

    egy. A **Paraméterek** lapon adja meg vagy módosítsa a sablon paraméterekkel értékeket. (Kattintson a súgó minden beállítás melletti ikonra.) Az alábbi képen látható mintaértékekre. Ebben a példában a fürtre nevű *hpc01* egy központi csomópont álló *hpc.local* a tartományban hoz létre, és 2 csomópontok számítja ki. A számítási csomópontok képből HPC csomag virtuális, amely tartalmazza a Microsoft Excel jönnek létre.

    ![Adja meg a paraméterek][parameters]

    >[AZURE.NOTE]A fő csomópont virtuális automatikusan létrejön a [legújabb piactér kép](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) HPC csomag 2012 R2 a Windows Server 2012 R2. A kép jelenleg HPC csomag 2012 R2 frissítés 3 alapján.
    >
    >Számítási csomópont VMs a legújabb kép a kiválasztott számítási csomópont családtagok jönnek létre. A legújabb HPC csomag számítási csomópont kép, amely tartalmazza a Microsoft Excel Professional Plus 2013 próbaverziója **ComputeNodeWithExcel** beállítással. Az általános SOA munkamenetek és az Excel UDF, mert a fürtre telepítéséhez válassza a **ComputeNode** (nélkül a telepített Excel).

    b. Válassza ki azt az előfizetést.

    c billentyűkombinációt. Hozzon létre egy erőforrás csoportot a fürt, például *hpc01RG*.

    d. Válasszon egy helyet az erőforráscsoport, például a központi US.

    e. A **jogi feltételek** lapon tekintse át a feltételeket. Ha elfogadja, kattintson a **vásárlás**gombra. Amikor elkészült, állítsa be az értékeket a sablon, kattintson a **Létrehozás**.

4.  A telepítő befejezésekor (általában elég körülbelül 30 perc), a fürt biztonságitanúsítvány-fájl exportálása központi csomópont. Egy újabb lépésben az ügyfélszámítógépen biztonságos HTTP kötés kiszolgálóoldali hitelesítését a nyilvános tanúsítvány importálása.

    egy. Csatlakozás az Azure portálról távoli asztali által a központi csomópontot.

     ![A fő csomópont csatlakoztatása][connect]

    b. Általános eljárás a tanúsítvány kezelő segítségével a fő csomópont-tanúsítvány exportálása (az található a tanúsítvány: \LocalMachine\My) a titkos kulcs nélkül. Ebben a példában exportálása *CN = hpc01.eastus.cloudapp.azure.com*.

    ![A tanúsítvány exportálása][cert]

### <a name="option-2-use-the-hpc-pack-iaas-deployment-script"></a>2 lehetőséget. A HPC csomag IaaS telepítési parancsfájlt használata

A HPC csomag IaaS telepítési parancsfájlt sokoldalú úgy is telepíteni egy HPC csomag fürthöz biztosít. Fürt létrehozza a klasszikus telepítési modell, mivel az Azure erőforrás-kezelő telepítési modell használja a sablon. A parancsprogram is kompatibilis Azure globális rendszergazdának vagy Azure Kína szolgáltatásban előfizetést.

**További vonatkozó követelmények**

* **Azure PowerShell** - [Telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md) (verzió 0.8.10 vagy újabb) az ügyfélgépen beállított.

* **HPC csomag IaaS telepítési parancsfájlt** - töltse le és csomagolja ki a [Microsoft letöltőközpontból](https://www.microsoft.com/download/details.aspx?id=44949)parancsfájl legújabb verzióját. A parancsprogram verziójának futtatásával ellenőrizze `New-HPCIaaSCluster.ps1 –Version`. Ez a cikk 4.5.0 verzióját vagy a parancsfájlt, később alapján.

**A beállítási fájl létrehozása**

 A HPC Pack IaaS telepítési parancsfájlt XML-konfigurációs fájl használja, mint a bemeneti az infrastruktúra a HPC fürt leíró. Egy központi csomópontot, és a 18 számítási csomópontok létrehozott képből a számítási csomópontot, amely tartalmazza a Microsoft Excel álló fürt üzembe helyezéséhez be a következő példa konfigurációs fájl környezetben értékek helyett. A konfigurációs fájl kapcsolatos további tudnivalókért lásd: a Manual.rtf fájlt a parancsprogram mappában és [a HPC csomag IaaS telepítési parancsfájlt egy HPC fürtre létrehozása](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md).

```
<?xml version="1.0" encoding="utf-8"?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>MySubscription</SubscriptionName>
    <StorageAccount>hpc01</StorageAccount>
  </Subscription>
  <Location>West US</Location>
  <VNet>
    <VNetName>hpc-vnet01</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>HPCExcelDC01</VMName>
      <ServiceName>HPCExcelDC01</ServiceName>
      <VMSize>Medium</VMSize>
    </DomainController>
  </Domain>
   <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>HPCExcelHN01</VMName>
    <ServiceName>HPCExcelHN01</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI/>
    <EnableWebPortal/>
    <PostConfigScript>C:\tests\PostConfig.ps1</PostConfigScript>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>HPCExcelCN%00%</VMNamePattern>
    <ServiceName>HPCExcelCN01</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>18</NodeCount>
    <ImageName>HPCPack2012R2_ComputeNodeWithExcel</ImageName>
  </ComputeNodes>
</IaaSClusterConfig>
```

**Megjegyzéseket fűzhet a konfigurációs fájl**

* A központi csomópont **kell** a **VMName** ugyanaz, mint a **ServiceName**, vagy SOA feladatok futtatásához sikertelen lesz.

* Győződjön meg arról, hogy a fő csomópont tanúsítvány létrehozott és exportált **EnableWebPortal** adja.

* A fájl egy utáni PowerShell-parancsprogramot, amely a központi csomópontra fut PostConfig.ps1 adja meg. A következő mintaparancsfájl konfigurálja az Azure tároló kapcsolati karakterláncot, a számítási csomópont szerepkör eltávolítja a központi csomópontot, és online csomópontjait életre, amikor a központi telepítés. 

```
    # add the HPC Pack powershell cmdlets
        Add-PSSnapin Microsoft.HPC

    # set the Azure storage connection string for the cluster
        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'

    # remove the compute node role for head node to make sure the Excel workbook won’t run on head node
        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode

    # total number of nodes in the deployment including the head node and compute nodes, which should match the number specified in the XML configuration file
        $TotalNumOfNodes = 19

        $ErrorActionPreference = 'SilentlyContinue'

    # bring nodes online when they are deployed until all nodes are online
        while ($true)
        {
          Get-HpcNode -State Offline | Set-HpcNodeState -State Online -Confirm:$false
          $OnlineNodes = @(Get-HpcNode -State Online)
          if ($OnlineNodes.Count -eq $TotalNumOfNodes)
          {
             break
          }
          sleep 60
        }
```

**Futtassa a**

1.  Nyissa meg a PowerShell konzol az ügyfélszámítógépen rendszergazdaként.

2.  A címtár váltson a parancsprogram mappára (ebben a példában E:\IaaSClusterScript).

    ```
    cd E:\IaaSClusterScript
    ```
    
3.  A HPC Pack fürt telepítéséhez futtassa a következő parancsot. Ez a példa feltételezi, hogy a konfigurációs fájl E:\HPCDemoConfig.xml található.

    ```
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```

A HPC Pack telepítési parancsfájlt fut, egy kis időt. A parancsprogram jelent egyvalamihez, hogy exportálása és a fürt tanúsítvány letöltése, mentse az ügyfélszámítógépen az aktuális felhasználó Dokumentumok mappában. A parancsprogram hoz létre az alábbihoz hasonló üzenet. A következő lépésben a megfelelő tanúsítvány tárolóban a tanúsítvány importálása.    
    
    You have enabled REST API or web portal on HPC Pack head node. Please import the following certificate in the Trusted Root Certification Authorities certificate store on the computer where you are submitting job or accessing the HPC web portal:
    C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## <a name="step-2-offload-excel-workbooks-and-run-udfs-from-an-on-premises-client"></a>Lépés: 2. Excel-munkafüzetek kiürítése, és futtassa a UDF egy helyszíni ügyfélprogramból

### <a name="excel-activation"></a>Az Excel aktiválása

Ha használja a ComputeNodeWithExcel virtuális kép gyártási munkaterhelésekből, meg kell adnia a Microsoft Office licenc érvényes kulcs Excel aktiválni a számítási csomópontok. Egyéb esetben Excel próbaverziója 30 nap múlva lejár, és a COMException (0x800AC472) az Excel-munkafüzetek futtatása meghiúsul. 

Akkor is újra Excel kiértékelés idő egy másik 30 napig: Jelentkezzen be a fő csomópont és clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` összes Excel számítja ki a csomópontok HPC kezelő keresztül. Kétszer legfeljebb is újra. Ezután meg kell adnia egy érvényes Office-licenc billentyűt.

Az Office Professional Plus 2013 telepítve van a virtuális kép mennyiségi kiadásának a egy általános mennyiségi licenc kulcs (GVLK). Kulcs Management szolgáltatást (Kulcskezelő) keresztül is aktiválhatja / Active Directory-Based aktiválási (Active Directory-BA) vagy aktiválási kulcs mennyiségi. 

    * Szeretné használni a Kulcskezelő/Active Directory-BA, egy meglévő Kulcskezelő kiszolgálót használ, vagy állítsa be a Microsoft Office 2013 mennyiségi licenccel csomag használatával egy újat. (Ha szeretné, állítsa be a kiszolgáló a központi csomópontra.) Ezután aktiválása az interneten vagy telefonon keresztül, a Kulcskezelő host billentyűt. Ezután clusrun `ospp.vbs` Kulcskezelő server és a port beállítása és az Office aktiválása az összes az Excel csomópontok számítja ki. 
    
    * Mennyiségi Aktiválási, első clusrun használandó `ospp.vbs` beviteléhez a billentyűt, és majd aktiválja az összes az Excel csomópontok az interneten vagy telefonon keresztül számítja ki. 

>[AZURE.NOTE]Az Office Professional Plus 2013 kiskereskedelmi termékkulcsok a virtuális képhez nem használhatók. Ha van érvényes kulcsok és az Office és az Excel alkalmazásban kiadásához eltérő Ez az Office Professional Plus 2013 mennyiségi kiadásának telepítési adathordozóját, használhatja őket helyette. Először távolítsa el a mennyiségi kiadásának, és amelyekhez kiadásának telepítéséhez. A telepített Excel számítási csomópont rögzíthetők a méretezés környezetben használni egy testre szabott virtuális képként.

### <a name="offload-excel-workbooks"></a>Excel-munkafüzetek kiürítése

Kövesse ezeket a lépéseket követve kiürítése egy Excel-munkafüzetet, így az Azure-ban a HPC Pack fürt fog futni. Ehhez az Excel 2010 vagy 2013 már telepítve van az ügyfélszámítógépen kell rendelkeznie.

1. Használja az 1 beállítások az Excel-HPC csomag fürt üzembe csomópont kép számítja ki. Szerezze be a fürt biztonságitanúsítvány-fájl (.cer) és fürt felhasználónév és jelszó.

2. Az ügyfélgépen importálja a fürt tanúsítvány tanúsítvány: \CurrentUser\Root csoportban.

3. Győződjön meg arról, hogy telepítve van az Excel. Hozzon létre egy Excel.exe.config-fájlt a következő tartalommal az ügyfélszámítógépen Excel.exe azonos mappában. Ebben a lépésben ellenőrzi, hogy a HPC csomag 2012 R2 Excel COM-bővítmény betöltése sikeresen.

    ```
    <?xml version="1.0"?>
    <configuration>
        <startup useLegacyV2RuntimeActivationPolicy="true">
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
        </startup>
    </configuration>
    ```
    
4.  Állítsa be az ügyfél elküldése a HPC csomag fürthöz feladatokat. Egy, hogy a teljes [HPC Pack 2012 R2 frissítés 3 telepítési](http://www.microsoft.com/download/details.aspx?id=49922) letöltése és telepítése a a HPC Pack ügyfél. Azt is megteheti töltse le és telepítse az [ügyfél-segédprogramok HPC csomag 2012 R2 frissítés 3-as](https://www.microsoft.com/download/details.aspx?id=49923) és a megfelelő vizuális C++ 2010 terjeszthető változata a számítógépen ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555)).

5.  Ebben a példában egy minta Excel-munkafüzet ConvertiblePricing_Complete.xlsb nevű használjuk. Letöltheti azt [az alábbi](https://www.microsoft.com/en-us/download/details.aspx?id=2939).

6.  Másolja például D:\Excel\Run munkakönyvtár létrehozása az Excel-munkafüzetet.

7.  Nyissa meg az Excel-munkafüzetet. Az **elektronikus oktatóanyagok elkészítése** menüszalagon kattintson a **COM-bővítmények** , és győződjön meg arról, hogy a HPC csomag Excel COM-bővítmény betöltése sikeresen.

    ![Excel-bővítmény az HPC csomag][addin]

8.  A VBA-makrók HPCControlMacros az Excelben szerkesztése a megjegyzéssel ellátott sorokat módosításával, a következő parancsfájl látható módon. Cserélje le a megfelelő értékeket környezetben.

    ![Az Excel makrójának HPC csomag][macro]

    ```
    'Private Const HPC_ClusterScheduler = "HEADNODE_NAME"
    Private Const HPC_ClusterScheduler = "hpc01.eastus.cloudapp.azure.com"

    'Private Const HPC_NetworkShare = "\\PATH\TO\SHARE\DIRECTORY"
    Private Const HPC_DependFiles = "D:\Excel\Upload\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.Initialize ActiveWorkbook
    HPCExcelClient.Initialize ActiveWorkbook, HPC_DependFiles

    'HPCWorkbookPath = HPC_NetworkShare & Application.PathSeparator & ActiveWorkbook.name
    HPCWorkbookPath = "ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath
    HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="hpc\azureuser", Password:="<YourPassword>"
```

9.  Másolja az Excel-munkafüzet feltöltése címtárhoz D:\Excel\Upload például. Ezt a címtárat a VBA makró HPC_DependsFiles állandó van megadva.

10. A munkafüzet Azure-ban a fürt futtatásához kattintson a **fürt** gomb a munkalapon.

### <a name="run-excel-udfs"></a>Excel-függvényekben futtatása

Excel-függvényekben futtatásához kövesse a fenti 1 – 3 állíthatja be az ügyfélszámítógép. Az Excel-függvényekben nem kell telepítve van az Excel alkalmazás telepítve van a számítási csomópontok. Igen csomópontok a fürt létrehozása számítja ki, amikor sikerült kiválaszthatja, normál csomópont kép helyett a számítási csomópont kép Excel számítja ki.

>[AZURE.NOTE] Nincs 34 karakterkorlát beállítása az Excel 2010-re és 2013 fürt összekötő párbeszédpanel. Ez a párbeszédpanel segítségével adja meg a fürt, amelyet a UDF futtat. Ha hosszabb, a teljes fürt nevét (például hpcexcelhn01.southeastasia.cloudapp.azure.com), nem fér el a párbeszédpanelen. A megoldás is a hosszú fürt nevét a értékkel például *CCP_IAASHN* gépi szintű-változó beállítása. Ezután adja meg *CCP_IAASHN %* párbeszédpanel a fürt fő csomópont neveként. 

Miután a fürt telepítése sikerült, folytassa a beépített minta futtassa a következő lépéseket az Excel UDF. Testre szabott Excel-függvényekben olvassa el a ezek az [erőforrások](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) az XLL-EK összeállítása és üzembe helyezése a IaaS fürt című témakört.

1.  Nyissa meg az új Excel-munkafüzetet. Az **elektronikus oktatóanyagok elkészítése** menüszalagon kattintson a **Bővítmények**gombra. A párbeszédpanelen kattintson a **Tallózás gombra**, keresse meg a %CCP_HOME%Bin\XLL32 mappát, és válassza ki a minta ClusterUDF32.xll. Ha a ClusterUDF32 az ügyfélgépen nem létezik, másolja azt a központi csomópontra %CCP_HOME%Bin\XLL32 mappából.

    ![Jelölje ki az UDF][udf]

2.  Kattintson a **fájl** > **Beállítások** > **Speciális**. A **képletek**ellenőrizze a **engedélyezése felhasználói XLL-függvények egy számítási fürt futtatásához**. Ezután kattintson a **Beállítások** gombra, és írja be a teljes fürt nevének **fürt fő csomópont nevét**. (Mint feljegyzett korábban a beviteli mezőben korlátozódik 34 karaktereket, így előfordulhat, hogy nem férnek el a hosszú fürt nevét. Előfordulhat, hogy a számítógép egészére változó itt-et hosszú fürt nevét.)

    ![Az UDF konfigurálása][options]

3.  A fürt UDF számítás futtatásához kattintson az érték =XllGetComputerNameC() tartalmazó cellára, és nyomja le az ENTER billentyűt. A függvény egyszerűen átveszi a számítási csomópontot, amelyen az UDF fut. nevét. Az első futtatása a hitelesítő adatok párbeszédpanelen kér, a felhasználónév és jelszó a IaaS fürt csatlakozni.

    ![UDF futtatása][run]

    Ha több cellában számítja ki, nyomja le az Alt-Shift-Ctrl + F9 billentyűkombinációt a számításban futtatásához kattintson az összes cella.

## <a name="step-3-run-a-soa-workload-from-an-on-premises-client"></a>3 a lépést. Egy helyszíni ügyfél SOA terhelési lebonyolítása

Általános SOA alkalmazások futtatásához a HPC csomag IaaS fürt, először a módszerek valamelyikével az 1 bevezetését tervezi a fürt. Adja meg, általános kiszámítania csomópont kép ebben az esetben, mert nincs szükség, az Excel a számítási csomóponton. Ezután kövesse az alábbi lépéseket.

1. Után beolvasása a fürt tanúsítvány, importálja az ügyfélszámítógépen tanúsítvány: \CurrentUser\Root csoportban.

2. Telepítse a [HPC csomag 2012 R2 frissítés 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) és az [ügyfél-segédprogramok HPC csomag 2012 R2 frissítés 3](https://www.microsoft.com/download/details.aspx?id=49923). Ezek az eszközök engedélyezése, valamint SOA ügyfélalkalmazások futtatását.

3. Töltse le a HelloWorldR2 [példakódot](https://www.microsoft.com/download/details.aspx?id=41633). Nyissa meg a HelloWorldR2.sln Visual Studio 2010-es vagy 2012-ben.

4. Először hozza létre a EchoService projekt. Ezután üzembe a szolgáltatást a IaaS fürthöz módon, mint egy helyszíni fürthöz rendszerbe. A lépések részletes leírását a Readme.doc a HelloWordR2 látható. A HellWorldR2 és más projektekben, a következő szakaszban leírt módon szeretné futtatni egy Azure IaaS fürthöz SOA ügyfél-alkalmazások készítése és módosítása.

### <a name="use-http-binding-with-azure-storage-queue"></a>Azure tároló várólista Http kötés használata

Szeretne használni az Azure tároló várólista Http kötés, végezze el a példakódot néhány módosítást.

* Frissítse a csoport nevét.

    ```
// Before
const string headnode = "[headnode]";
// After e.g.
const string headnode = "hpc01.eastus.cloudapp.azure.com";
or
const string headnode = "hpc01.cloudapp.net";
```

* Tetszés szerint használja az alapértelmezett TransportScheme SessionStartInfo vagy explicit módon állítsa be a Http.

```
    info.TransportScheme = TransportScheme.Http;
```

* Alapértelmezett kötés használata a BrokerClient.

    ```
// Before
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session, binding))
// After
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session))
```

    Vagy explicit módon használja a basicHttpBinding.

    ```
BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
```

* Másik lehetőségként a UseAzureQueue jelző IGAZ értékre állítás SessionStartInfo a. Ha nem állít be, akkor állítja be igaz, ha a fürt nevének Azure tartomány utótag, és a TransportScheme Http alapértelmezés szerint.

    ```
    info.UseAzureQueue = true;
```

###<a name="use-http-binding-without-azure-storage-queue"></a>Http kötés Azure tároló várólista nélkül

Http kötések nélkül az Azure tároló várólista használatához explicit módon beállított UseAzureQueue jelölő a SessionStartInfo a hamis értékre.

```
    info.UseAzureQueue = false;
```

### <a name="use-nettcp-binding"></a>NetTcp kötés használata

NetTcp kötések használatához a konfigurációs hasonlít egy helyszíni fürthöz csatlakozik. Nyissa meg a fő csomópont virtuális néhány végpontjait szüksége. Ha a fürt létrehozásához használt a HPC csomag IaaS telepítési parancsfájlt, például beállítása a végpontok az Azure klasszikus portálon az alábbi képlettel történik.


1. Állítsa le a virtuális.

2. Adja hozzá a TCP-portok 9090, 9087, 9091, a munkamenet 9094 ügynök, ügynök dolgozó és Data services, a kurzor

    ![Állítsa be a végpontok][endpoint]

3. Indítsa el a virtuális.

A SOA ügyfélalkalmazás használata nem igényli módosítását kivételével módosítása a központi nevét a IaaS fürt teljes nevét.

## <a name="next-steps"></a>Következő lépések

* [Ezek az erőforrások](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) az Excel-munkaterhelésekből futtatásával HPC csomaggal kapcsolatos további információt talál.

* Lásd: [SOA szolgáltatások kezelése a Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) kapcsolatos további tudnivalók a telepítésével és HPC Pack SOA szolgáltatások kezelése.

<!--Image references-->
[scenario]: ./media/virtual-machines-windows-excel-cluster-hpcpack/scenario.png
[github]: ./media/virtual-machines-windows-excel-cluster-hpcpack/github.png
[template]: ./media/virtual-machines-windows-excel-cluster-hpcpack/template.png
[parameters]: ./media/virtual-machines-windows-excel-cluster-hpcpack/parameters.png
[create]: ./media/virtual-machines-windows-excel-cluster-hpcpack/create.png
[connect]: ./media/virtual-machines-windows-excel-cluster-hpcpack/connect.png
[cert]: ./media/virtual-machines-windows-excel-cluster-hpcpack/cert.png
[addin]: ./media/virtual-machines-windows-excel-cluster-hpcpack/addin.png
[macro]: ./media/virtual-machines-windows-excel-cluster-hpcpack/macro.png
[options]: ./media/virtual-machines-windows-excel-cluster-hpcpack/options.png
[run]: ./media/virtual-machines-windows-excel-cluster-hpcpack/run.png
[endpoint]: ./media/virtual-machines-windows-excel-cluster-hpcpack/endpoint.png
[udf]: ./media/virtual-machines-windows-excel-cluster-hpcpack/udf.png

<properties
    pageTitle="Egy Jegyzetfüzet IPython kiszolgálójával virtuális gép beállítása |} Microsoft Azure"
    description="Fejlett analitikai felfelé az Azure virtuális gép tudományos környezetben használatra IPython kiszolgálóval beállítása"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="set-up-an-azure-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>A fejlett analitikai IPython jegyzetfüzet kiszolgáló-Azure virtuális gép beállítása

Ez a témakör bemutatja, hogyan kiépítése, és állítsa be az Azure virtuális gép fejlett analitikai, annak érdekében, hogy egy adatok tudományos környezet részeként használható. A Windows virtuális gép azokat az eszközöket, például a IPython jegyzetfüzet, a Azure tároló Explorer, a AzCopy, valamint egyéb hasznos fejlett analitikai projektekhez segédprogramot igazoló van beállítva. Azure tároló Explorer és AzCopy, például adja meg a helyi gépről Azure blob-tárolóhoz feltölteni az adatokat, vagy töltse le a helyi számítógép zónában blob-tárolóból kényelmes lehetőségeket.

## <a name="create-vm"></a>Lépés: 1: Hozzon létre egy általános célú Azure virtuális gépen

Ha már van egy Azure virtuális gép, és csak be szeretné állítani egy IPython jegyzetfüzet kiszolgáló rajta, ugorja át ezt a lépést, és folytassa [lépés 2: IPython jegyzetfüzetek végpont hozzáadása egy meglévő virtuális gép](#add-endpoint).

Az Azure virtuális gép létrehozott megkezdése előtt meg kell határoznia, hogy a projekt adatai feldolgozása van szükség a gép méretét. Kisebb gépek kevesebb memória és kevesebb Processzormagok-nál nagyobb gépek, de ezek egyben kevésbé drága. Gépi típusok és árak listáját keresse fel a <a href="http://azure.microsoft.com/pricing/details/virtual-machines/" target="_blank">Virtuális gépeken futó árak</a>

1. Jelentkezzen be a <a href="https://manage.windowsazure.com" target="_blank">Klasszikus Azure</a>-portálra, és kattintson az **Új** elemre a bal alsó sarokban. Egy ablakban jelenik meg. Jelölje ki a **KISZÁMÍTANIA** -> **virtuális gép** -> **FROM GYŰJTEMÉNY**.

    ![Munkaterület létrehozása][24]

2. Az alábbi képek közül választhat:

    * A Windows Server 2012 R2 adatközponthoz
    * A Windows Server Essentials élmény (Windows Server 2012 R2)

    Kattintson a jobb alsó sarkában, Ugrás a következő beállítása lapon a jobbra mutató nyílra.

    ![Munkaterület létrehozása][25]

3. Nevezze el a virtuális gép szeretne létrehozni, jelölje ki a gép méretét (alapértelmezett: A3) az adatokat a gép fog folyamat méretének és hogyan hatékony alapján, amelyet a gép (memóriaméret és a számítási magmintákat számát), írja be a felhasználónevét és jelszavát a gép. Kattintson az Ugrás a következő beállítása lapon jobbra mutató nyíl.

    ![Munkaterület létrehozása][26]

4. Jelölje ki a **Régió/AFFINITÁS csoport/virtuális hálózat** , amely tartalmazza a **TÁRTERÜLET-FIÓKOT** , amely a virtuális számítógéphez használni szeretné, és válassza a tárterület fiókhoz. A a végpont ("IPython" ide) név beírása a képernyő alján a **VÉGPONTOK** mező zárólap adni. Megadhatja, hogy a tetszőleges karakterlánc a végpontot, és közötti egész szám 0 és 65536 **elérhető** meg a **Nyilvános PORT** **nevét** . A **MAGÁNJELLEGŰ PORT** nem lehet **9999**. Felhasználók kell **elkerülése** használatának nyilvános olyan portot már kiosztott internetes szolgáltatások számára. <a href="http://www.chebucto.ns.ca/~rakerman/port-table.html" target="_blank">Az internetes szolgáltatások portok</a> rendelkezik, és el kell kerülni portokat felsorolását.

    ![Munkaterület létrehozása][27]

5. Kattintson a pipa ikonra a virtuális gép kiépítési folyamat indításához.

    ![Munkaterület létrehozása][28]


A folyamat kiépítési virtuális gép 15-25 percig is eltarthat. A virtuális gép létrehozása után meg kell jelennie a számítógép állapotának **futó**.

![Munkaterület létrehozása][29]

## <a name="add-endpoint"></a>Lépés: 2: Zárólap IPython jegyzetfüzetek-hozzáadása egy meglévő virtuális gépen

Ha a virtuális gép az 1 utasításait követve létrehozott, a végpont IPython jegyzetfüzet már szerepel, és ezt a lépést is kihagyja.

Ha a virtuális gép már létezik, és fel kell vennie zárólap a telepíti a 3 alatt első napló az Azure klasszikus portál IPython jegyzetfüzet, jelölje ki a virtuális gép, és adja hozzá az IPython jegyzetfüzet server-végpontot. Az alábbi ábrán a portálon képernyőképe tartalmazza, után a jegyzetfüzet IPython végpont Windows virtuális géphez.

![Munkaterület létrehozása][17]

## <a name="run-commands"></a>Lépés 3: IPython jegyzetfüzet és a többi támogató eszközök telepítése

A virtuális gép létrehozását követően jelentkezzen be a Windows virtuális gép távoli asztali Protocol (RDP) segítségével. További tudnivalókért lásd: [Jelentkezzen be a virtuális gépen futó Windows Server](../virtual-machines/virtual-machines-windows-classic-connect-logon.md). Nyissa meg a **parancssort** (**nem a Powershell-parancsablakban**), hogy a **rendszergazda** , és futtassa a következő parancsot.

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

A telepítés befejezésekor a IPython jegyzetfüzet kiszolgáló, automatikusan elindul az *C:\\felhasználók\\\<felhasználónév\>\\dokumentumok\\IPython jegyzetfüzetek* címtár.

Amikor a rendszer kéri, írjon be egy jelszót a IPython jegyzetfüzetet és a rendszergazdai jogosultságokkal a jelszót. Ezzel a gépen szolgáltatásként IPython jegyzetfüzetet.

## <a name="access"></a>Lépés: 4: Access IPython jegyzetfüzetek webböngészőből
IPython jegyzetfüzet kiszolgáló eléréséhez nyissa meg a webhely böngészőben és a bemeneti *https://&#60;virtual számítógépnév DNS >: & #60; nyilvános portszámot >* a URL-címe mezőben. Ebben az esetben a *& #60; nyilvános portszámot >* kell lennie a portszámot, ha a jegyzetfüzet IPython végpont hozzá lett adva.

A *& #60; virtuális gép tartománynév >* Azure klasszikus portálon megtalálhatók. Után kattintson a naplózás a klasszikus portálra a **virtuális gépeken FUTÓ**, jelölje ki a létrehozott számítógépre, és válassza az **IRÁNYÍTÓPULT**, a tartománynév a következőképpen fog megjelenni:

![Munkaterület létrehozása][19]

Arról, hogy _a webhely biztonsági tanúsítványával probléma van_ (az Internet Explorer) vagy _a kapcsolat számára nem saját_ (Chrome), egy figyelmeztetés fog megjelenni, ahogy az alábbi ábra. Kattintson a **Folytatás gombra a webhelynek (nem javasolt)** (Internet Explorer) vagy a **Speciális** gombra, és ezután * *a Folytatás & #60;* DNS-név*> (nem biztonságos) ** (Chrome) továbbra is. Kattintson a szövegbeviteli a IPython jegyzetfüzetek eléréséhez korábban megadott jelszót.

**Az Internet Explorer:**
![-munkaterület létrehozása][20]

**A Chrome:**
![-munkaterület létrehozása][21]

Bejelentkezés után a IPython jegyzetfüzethez, könyvtárában *DataScienceSamples* jelennek meg a böngészőt. Ezt a címtárat a minta IPython segíti a felhasználókat adatok tudományos feladatokat lebonyolítása a Microsoft által megosztott jegyzetfüzetek tartalmazza. A minta IPython jegyzetfüzetek kivette a [**Github tárházba**](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) a virtuális gépeken futó alatt a jegyzetfüzet IPython kiszolgáló folyamat beállítása. A Microsoft kezeli, és ez a tár gyakran frissíti. Felhasználók használata esetén olvassa el a nemrég frissített minta IPython jegyzetfüzetek eléréséhez a Github tárat.
![Munkaterület létrehozása][18]

## <a name="upload"></a>Lépés 5: Töltse fel a meglévő IPython jegyzetfüzet egy helyi gépről IPython jegyzetfüzet kiszolgálóra

IPython jegyzetfüzeteket legegyszerűbben a kiszolgálóra való feltöltéséhez a helyi számítógépeken meglévő IPython jegyzetfüzet a IPython jegyzetfüzet a virtuális gépeken felhasználóknak adja meg. Követően a felhasználók jelentkezzen be a IPython jegyzetfüzet egy webböngészőben, kattintson a **címtár** IPython jegyzetfüzet feltöltött. Ezután jelölje ki az IPython jegyzetfüzet .ipynb töltse fel a helyi számítógépről a **Fájlkezelőt**, és húzással azt a webböngészőben IPython jegyzetfüzet könyvtárában található. Kattintson a **Feltöltés** gombra kiszolgálóra való feltöltéséhez a .ipynb fájlt a IPython jegyzetfüzetet. Más felhasználók majd használatba vehetik azt a webböngésző használatával.

![Munkaterület létrehozása][22]

![Munkaterület létrehozása][23]


##<a name="shutdown"></a>Leállítás és vonja vissza a virtuális gép használaton rendelése

Azure virtuális gépeken futó árú szerint **fizet csak akkor használja**. Győződjön meg arról, hogy, amelyek nem számlázott a virtuális gép nem használatakor, akkor rendelkezik használaton **Leállítva (Deallocated)** állapotban.

> [AZURE.NOTE] Ha kikapcsolja a virtuális gép belül a virtuális (power beállításai a Windows), a virtuális leállítja, de továbbra is a felosztott. Annak érdekében, hogy nem továbbra is számlát kapni, mindig leállítása virtuális gépeken futó az [Azure klasszikus portálon](http://manage.windowsazure.com/). A virtuális Powershellen keresztül meg is szüntetheti, hívja fel **ShutdownRoleOperation** "PostShutdownAction" a "StoppedDeallocated" egyenlő.

Le és felszabadítása a virtuális gépen:

1. Az [Azure klasszikus portált](http://manage.windowsazure.com/) a fiók használatával jelentkezzen be.  

2. Jelölje ki a **virtuális gépeken FUTÓ** a bal oldali navigációs sávján.

3. Virtuális gépeken futó listájában kattintson a virtuális számítógép nevére, majd válassza az **IRÁNYÍTÓPULT** oldalát.

4. A lap alján kattintson a **LEÁLLÍTÁS**gombra.

![Virtuális leállítása][15]

A virtuális gép lesz felszabadítása, de nem törlődnek. Az Azure klasszikus portálról bármikor előfordulhat, hogy indítsa újra a virtuális gépet.

## <a name="your-azure-vm-is-ready-to-use-whats-next"></a>Az Azure virtuális használatára: következő lépések?

A virtuális gép ettől kezdve készen áll az adatok tudományos gyakorlatok használni. A virtuális gép egyben Azure gépi tanulási és a csoport adatok tudományos folyamat feltárására és feldolgozás, az adatok és más feladatok együtt egy IPython jegyzetfüzet kiszolgálójával használatra kész.

Az adatok tudományos során a következő lépésekkel a [Tanulási javaslat](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) van megfeleltetve, és a lépések, amelyek az adatok áthelyezése HDInsight, folyamat és minta ott tanulási az adatok az Azure gépi tanulási előkészítése az alábbiakat tartalmazhatják.


[15]: ./media/machine-learning-data-science-setup-virtual-machine/vmshutdown.png
[17]: ./media/machine-learning-data-science-setup-virtual-machine/add-endpoints-after-creation.png
[18]: ./media/machine-learning-data-science-setup-virtual-machine/sample-ipnbs.png
[19]: ./media/machine-learning-data-science-setup-virtual-machine/dns-name-and-host-name.png
[20]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning-ie.png
[21]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning.png
[22]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-1.png
[23]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-2.png
[24]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-1.png
[25]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-2.png
[26]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-3.png
[27]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-4.png
[28]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-5.png
[29]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-6.png

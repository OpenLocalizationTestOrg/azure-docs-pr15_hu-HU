
<properties 
    pageTitle="Az Office 365-előfizetéséhez használata Azure RemoteApp |} Microsoft Azure"
    description="Megtudhatja, hogyan használhatja az Office 365-előfizetés az Azure RemoteApp megosztása az Office-alkalmazások."
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-to-use-your-office-365-subscription-with-azure-remoteapp"></a>Az Office 365-előfizetéséhez használata Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Tudta, hogy a is használhatja a meglévő Office 365-előfizetés Azure RemoteApp megosztására a felhőben az Office-alkalmazások? A cikkben olvashat bővebben az Office 365 + Azure RemoteApp beállítások, beleértve az Office 365-tel kapcsolatos cikkek, amelyek segítségével hivatkozásokat győződjön egyszóval kihasználhatja az előfizetés.

## <a name="how-do-i-use-office-365-accounts-for-azure-remoteapp"></a>Hogyan kell használni az Office 365-fiókok az Azure RemoteApp?
Nézze meg az összes információt a Péter új cikk: [az Office 365 felhasználói fiókok és Azure RemoteApp használata](remoteapp-o365user.md)

## <a name="can-i-use-my-office-365-subscription-to-run-office-applications-in-azure-remoteapp"></a>Az Office 365-előfizetésem segítségével futtassa az Office-alkalmazásokat az Azure RemoteApp?

igen! Használja az Office 365-előfizetése valójában csak úgy lehet jelenítse meg az Office-alkalmazások Azure RemoteApp.

(Megjegyzés: Ha a Azure RemoteApp üzembe üzemeltetési partner hozta, azok lehet, hogy egy [Service Provider licencszerződés](http://www.microsoft.com/en-us/Licensing/licensing-programs/spla-program.aspx)alapján Office-licencek biztosítson)


A kiváló megoldás az kapcsolatban az Office 365-előfizetéséhez, hogy akkor is használhatja ugyanazt a felhasználói licenc számos különböző platformokon és környezetben, beleértve az Azure felhő lehetővé. Azure RemoteApp Office-alkalmazások használatakor nem kell további licencek vásárlása, vagy állítsa be a meglévő licencek külön módon. Office 365-előfizetést tartalmazó [Office 365 ProPlus](https://technet.microsoft.com/library/Gg702619.aspx)szükség.

Az Office 365 ProPlus lehetővé teszi, hogy a [megosztott géphez aktiválási](https://technet.microsoft.com/library/Dn782860.aspx) – Ez a szolgáltatás lehetővé teszi, hogy ideiglenes felhasználó alapú aktiválási virtuális az Office és a felhőalapú környezetben, például Azure RemoteApp (és a távoli asztali szolgáltatásokat).

Melyik Office 365-előfizetések tartalmazzák az Office 365 ProPlus? Nézze meg a [szolgáltatások elérhetőségét belül minden terv](https://technet.microsoft.com/library/office-365-plan-options.aspx) táblázatot. Ne feledje, hogy nem minden csomag tartalmazzák az Office 365 ProPlus (például az Office 365 vállalati verzió terv). Ha csomagja nem, fontolja meg, amely tartalmaz (például Office 365 oktatási E3 csomag) csomagra.

## <a name="ok-so-how-are-my-office-365-proplus-licenses-used-with-azure-remoteapp"></a>Jó, de hogyan az Office 365 ProPlus licenceket az Azure RemoteApp használatakor?

Egyes felhasználói licenc az Office 365 ProPlus lehetővé teszi, hogy egy felhasználó Office-alkalmazások maximum 5 számítógépek és táblagépeken és telefonok aktiválása. Minden egyes aktiválási regisztrált a felhasználó csak azok inaktiválta az Office-az eszközön. (A felhasználók kezelhetik a eszközök az [Office 365-portálon](https://portal.office365.com/).)

Azure RemoteApp egyetlen felhasználó előfordulhat, hogy jelentkezzen be számítógépek aznap azt végrehajtása nélkül. Ennek az oka, hogy a szolgáltatás automatikusan kezeli, és közben a felhasználó csak az alkalmazások és a megosztott alkalmazások méretezze át a források a felhőben. Az Office 365 ProPlus ebben az esetben egy megosztott géphez aktiválási mód kínál – Ez azt jelenti, hogy a felhasználó nem kell minden licenc management elérheti ezeket az erőforrásokat, és végezze el a az egyes számítógépekre nem számítanak bele az 5 számítógép aktiválási korlátot.

Mindaddig, amíg az Office 365 ProPlus licencek hozzárendelése a felhasználókhoz, (rendszergazda), az Office használhatják, a személyes eszközök, valamint az Azure RemoteApp webhelycsoport keresztül.

## <a name="which-office-applications-can-i-use-with-office-365-and-azure-remoteapp"></a>Melyik Office-alkalmazásokat is használhatja az Office 365 és az Azure RemoteApp?

Az Office 365-előfizetés aktiválásához és megosztása az Office 2013-ban a Azure RemoteApp környezetekben is használhatja. Jelenleg nem támogatott diagramtípusról Azure RemoteApp Office más verzióit használatát. Ide tartoznak, az Office 2003, az Office 2007, az Office 2010 és az Office 2016-ban.

## <a name="what-about-visio-pro-or-project-pro"></a>Mi a helyzet a Visio Pro vagy a Project Pro?

Az Office 365 ProPlus, az RemoteApp előfizetésében található kép mind a Visio Pro és a Project Pro tartalmazza. De nem használható az Office 365 ProPlus előfizetés ezeket az alkalmazások aktiválása, – minden rendelkeznek a saját licencet. Őket az [Office 365-portálon](https://portal.office365.com/)is aktiválhatja. 

Nem kell licenc ezeket a programokat, ha nem szeretné használni őket. A use -, és ugorja át a többi programok csak aktiválása Továbbra is látni fogja őket a kép, de nem használhat. 

## <a name="how-do-i-get-started-with-office-365-and-azure-remoteapp"></a>Hogyan kezdjek hozzá az Office 365 és az Azure RemoteApp?

Most, hogy az Office 365-licencek részleteit, akkor kezdje az első lépések az Azure RemoteApp használat,-nagyon egyszerűen:

Az **Office 365 ProPlus (előfizetés szükséges)** kép használata: az Azure RemoteApp webhelycsoport létrehozásakor.

![Az Office 365 Pro Plus Azure RemoteApp képpel](./media/remoteapp-officesubscription/remoteapp-officeimage.png)


Ezen a képen a legújabb Windows-kiszolgáló és az Office 365 ProPlus tartalmazza. A webhelycsoport (beleértve a közzétételi alkalmazásokat is), a felhasználók egyszerűen konfigurálása után jelentkezzen be az Azure RemoteApp (a RemoteApp ügyfél), és adja meg az Office 365 hitelesítő bármely Office-alkalmazások. A licencek automatikus érkeznek anélkül, hogy minden olyan, vagy kezelés kötelező.

## <a name="can-i-create-a-custom-image-with-office-365-proplus"></a>Létrehozhatok-e egy egyéni képe az Office 365 ProPlus?

A webhelycsoport tartalmazó Office 365 ProPlus egyéni képet készíthet. 2 választási lehetőségek - kínálunk, amely az Azure gyűjteménye használja vagy ha saját egyéni kép létrehozása, és telepítse az Office 365 ProPlus van.

### <a name="use-the-azure-gallery-image"></a>Használja az Azure gyűjteménye

A telepítéshez használni az Office 365 ProPlus gyűjtemény legegyszerűbben, [több Azure gyűjtemény képre](remoteapp-image-on-azurevm.md) az Azure RemoteApp előfizetésében található. Ellenőrizze, hogy úgy dönt, hogy a **Windows Server távoli asztali munkamenetgazda Office 365 Proplushoz készült előtelepítve** képet. Ezután, amelyet bármilyen egyéb alkalmazások telepítése, hogy a képet, és minden rendben lépjen.

### <a name="use-a-custom-image"></a>Egyéni kép használata:

Bármikor létrehozhat egyéni kép – létrehozhat [Azure virtuális](remoteapp-image-on-azurevm.md) vagy [Hozzon létre helyi meghajtóra a képet](remoteapp-create-custom-image.md) , és töltse fel Azure. Bármelyik lehetőséget választja győződjön meg arról, telepítette az Office 365 ProPlus használata a megosztott géphez aktiválási csomópontot. Az [Office-telepítő eszközzel](http://blogs.technet.com/b/odsupport/archive/2014/07/11/using-the-office-deployment-tool.aspx) , és kövesse a telepítési [utasításokat](https://technet.microsoft.com/library/Dn782858.aspx) .  

### <a name="disable-automatic-updates-for-office-365-proplus-in-your-custom-image---important"></a>Az automatikus frissítések letiltása az Office 365 ProPlus a saját kép - fontos

A saját kép használja Azure RemoteApp sablonként a további erőforrások hozzáadása a igény szerint a felhasználók növeli a. Késések és kapcsolódási problémák megelőzése érdekében tiltsa le az automatikus frissítések Office képe. Ha nem, majd minden erőforrás-sablon segítségével létrehozott automatikusan frissül indításakor. A saját kép frissítéséhez helyett használja a szabványos Azure RemoteApp folyamat. Ezzel a módszerrel frissítése az Office-alkalmazásokkal egyszer a sablon képet, és hagyja az Azure RemoteApp gondoskodik az első a frissítéseket a felhasználóknak.

Az automatikus frissítések letiltása, az alábbi hozzáadása az Office-telepítő eszköz konfigurációs fájl:

        <Updates Enabled="FALSE" />

Most a beállítási fájl tartalmaznia kell ezek azok a sorok:
    
        <Display Level="NONE" AcceptEULA="TRUE" />
        <Property Name="SharedComputerLicensing" Value="1" />
        <Updates Enabled="FALSE" />

## <a name="so-how-can-i-update-an-image-with-office-365-proplus"></a>Igen, hogyan is frissíteni az Office 365 Proplushoz készült képet?

A kép a webhelycsoport frissítése számos oka lehet. Íme néhány:

- A legújabb Windows-frissítések letöltéséhez 
- A legújabb Office 365 ProPlus alkalmazás a frissítések beszerzése
- Az egyéni alkalmazás frissítése
- Egyéb beállításait, magát a kép módosítása

Lépjen a végpontok közötti lépések használni a frissített képet a webhelycsoport frissítése, [Itt](remoteapp-update.md). De frissítse a kép és az Office 365 ProPlus tudnivalókért olvassa el az alábbi információkat.

Akkor két lehetőség közül választhat a kép módosítása - cserélje ki a képet egy teljesen új vagy manuálisan frissíteni a meglévő képet.

### <a name="replace-your-image-with-the-latest-azure-gallery-image--add-customizations"></a>A kép cseréje a legújabb Azure gyűjteménye az + add testreszabások
Ezt a beállítást választja hagyja, hogy a Microsoft Windows Server és az Office 365 ProPlus-frissítések gondoskodik. Frissíteni a meglévő képet, hanem kell létrehoznia a legújabb gyűjteménye alapján egy teljesen új kép. Ezután ismételje meg minden testre a képet-egyéni alkalmazások telepítése előtt elvégzett módosítása a kép konfigurációs stb.

A gyűjtemény képek rendszeresen frissíteni, úgy hagyhatja egyszerű, arra, hogy a Windows Server és az Office 365 ProPlus alkalmazások naprakész legyen-e. Természetesen a csökkentés, hogy, hogy a módosítások alkalmazása, minden alkalommal, amikor egy új képet kap. Parancsfájlok automatizálhatja a beállítás a testreszabások hozhat létre.

### <a name="manually-update-your-existing-image"></a>A meglévő képet kézi frissítése

Ezt a beállítást választja, akkor szabványos Windows eszközök segítségével frissítés alkalmazása a képre. Az Office 365 ProPlus használja az Office-telepítő eszközzel töltse le és telepítse a legújabb frissítéseket, vagy az Office 365 ProPlus verzióiban.

> [AZURE.IMPORTANT] Ne feledje, hogy – letiltása az Office 365 ProPlus az automatikus frissítések.

A frissítések keresése az Office-telepítő eszköz használatáról további információra van szüksége?

- [Kattintson a kattintásra telepítése az Office 365-termékekhez az Office-telepítő eszköz használatával](https://technet.microsoft.com/library/JJ219423.aspx)
- [Telepítésével és frissítése az Office 365 ProPlus, az Office-telepítő eszköz használata](https://channel9.msdn.com/Events/Ignite/2015/BRK3168) (videó)
- [Az Office 365 ProPlus frissítési beállításainak konfigurálása](https://technet.microsoft.com/library/dn761708.aspx)

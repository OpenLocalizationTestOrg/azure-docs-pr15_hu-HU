<properties
    pageTitle="Azure Papírhalom eszközök és PaaS szolgáltatásokat |} Microsoft Azure"
    description="Megtudhatja, hogy miként veheti használatba az Azure egymást fedő PaaS szolgáltatások."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="tools-for-azure-stack"></a>Azure Papírhalom ellenőrző eszközök

Az alábbiakban ismertetett eszközök töltheti le. Ha azt szeretné, ha értesítést szeretne kapni az új szolgáltatások és eszközök, a #AzureStack követése a Twitteren.

## <a name="template-tools"></a>Sablon eszközök

### <a name="azure-stack-github-templates"></a>Azure Papírhalom Github sablonok
Ismerkedjen meg az [Azure Papírhalom GitHub sablonok](https://github.com/Azure/AzureStack-QuickStart-Templates)növekvő gyűjteménye. [Azure](https://github.com/Azure/azure-quickstart-templates), mint a "Első lépések" Azure erőforrás-kezelő sablonok célja, hogy egyszerű építőelemek és a példákat, készen áll a Microsoft Azure Papírhalom technikai előzetes verzió vásárlási a fogalmat környezetben üzembe használatának megkezdéséhez. Szereplő példák első fél terhelést az [Active Directory-nem – ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/ad-non-ha), [sql-2014-nem – ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sql-2014-non-ha), [a sharepoint-2013 – nem – ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sharepoint-2013-non-ha), valamint több egyszerű 101 sablonok [101-egyszerű – a windows-virtuális](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-simple-windows-vm)szeretne.


### <a name="marketplace-item-packaging-tool-and-sample"></a>Marketplace elem összecsomagolása eszköz és a minta
[Töltse le és a csomagolást eszközzel](http://www.aka.ms/azurestackmarketplaceitem) marketplace elemek a Papírhalom Azure piactér hozzáadása a saját egyéni sablonok létrehozása. Hozzon létre egy piactéren elérhető elemre, és a bérlők számára elérhető legyen útmutatást a [piactér létrehozása elem](azure-stack-create-and-publish-marketplace-item.md)találhatók.

## <a name="developer-tools"></a>A Fejlesztőeszközök


### <a name="use-visual-studio-and-azure-stack-tp2-on-the-mas-con01-virtual-machine"></a>Visual Studio és Azure Papírhalom TP2 használja, az m/m-CON01 virtuális gépen
Szeretné a konzol virtuális Visual Studio segítségével Azure Papírhalom sablonok használata, ha telepítenie kell a szükséges eszközök megfelelő verzióit. Az alábbi eljárással TP2 verziók telepítése.

1. Távoli asztali kapcsolat használatával jelentkezzen be az m/m-CON01 virtuális gép azurestack\azurestackadmin hitelesítő adataival.
2. Telepítse, és nyissa meg a webes Platform telepítőt.
3. Keresse meg és telepítse **a Microsoft Azure SDK - 2.9.5 Visual Studio közösségi 2015**.
4. Használja a **Programok telepítése és törlése**, távolítsa el a **Microsoft Azure PowerShell (Sept)** a SDK részeként telepítette kap.
5. Nyissa meg a webes Platform-telepítőt.
6. Keresse meg és telepítse a **Microsoft Azure PowerShell - Azure Papírhalom technikai előzetes verzió 2**. 
7. Indítsa újra az m/m-CON01 virtuális gépet.
8. Távoli asztali kapcsolat használatával jelentkezzen be az m/m-CON01 virtuális gép azurestack\azurestackadmin hitelesítő adataival.
9. Nyissa meg a Visual Studio, és ellenőrizze, hogy akkor is csatlakoztatása az Azure Papírhalom környezetben, sablonok, és így tovább. 

### <a name="azure-powershell-sdk"></a>Azure PowerShell SDK
Azure PowerShell, ahol kezelheti Azure és Azure Papírhalom a Windows PowerShell-parancsmagok modul. A parancsmagok használata hozhat létre, tesztelje, telepítése, és megoldásaival és szolgáltatásaival Azure Papírhalom platformon keresztül kézbesítve kezelése.
[Azure PowerShell SDK töltse le](http://aka.ms/azStackPsh) és [telepítse a PowerShell](azure-stack-connect-powershell.md).

### <a name="azure-cross-platform-command-line-interfaces"></a>Azure platform parancs sor felületek tartományok
Gyorsan telepítse az Azure parancssori kezelőfelületről Azure létrehozásához és kezeléséhez, a Microsoft Azure egymást fedő erőforrások Megnyitás-forrás shell-alapú parancsok használ.

[Töltse le a Windows CLI](http://aka.ms/azstack-windows-cli)

[A Mac CLI letöltése](http://aka.ms/azstack-linux-cli)

[Töltse le a Linux CLI](http://aka.ms/azstack-mac-cli)

>[AZURE.NOTE]
>
> + Ha a Mac vagy Linux gépen, is megnyithatja a CLI a parancs használatával`npm install -g azure-cli@0.9.11`</br>
> + Ha tanúsítvány érvényességi problémái érkezik, a parancs futtatása`set NODE_TLS_REJECT_UNAUTHORIZED=0`

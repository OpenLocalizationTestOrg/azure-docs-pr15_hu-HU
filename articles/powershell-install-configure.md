<properties
    pageTitle="Telepítse és állítsa be a Azure PowerShell hogyan"
    description="Megtudhatja, hogy miként telepítheti, állíthatja Azure PowerShell."
    editor="tysonn"
    manager="dongill"
    documentationCenter=""
    services=""
    authors="coreyp-at-msft"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="powershell"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="coreyp"/>

# <a name="how-to-install-and-configure-azure-powershell"></a>Telepítse és állítsa be a Azure PowerShell hogyan

<div class="dev-center-tutorial-selector sublanding"><a href="/manage/install-and-configure-windows-powershell/" title="PowerShell" class="current">A PowerShell</a><a href="/manage/install-and-configure-cli/" title="Azure CLI">Azure CLI</a></div>

##<a name="what-is-azure-powershell"></a>Mi az Azure PowerShell?
Azure PowerShell moduljainak kezelése Azure a Windows PowerShell-parancsmagok nyújtó áll. A parancsmagok használata hozhat létre, tesztelje, telepítése, és megoldásaival és szolgáltatásaival az Azure platform kézbesíti kezelése. A legtöbb esetben a parancsmagok az Azure-portálon, például létrehozása és konfigurálása a felhőszolgáltatások, virtuális gépeken futó, virtuális hálózatok és web Apps alkalmazások megegyező feladatok használható.

## <a name="how-versioning-works"></a>A verziószámozás működése

Azure PowerShell szemantikai verziószámozás, ami azt jelenti, hogy ha használ verzió A > B verzióját, majd a verziójával rendelkezik a legfrissebb API-khoz. Az is, azt jelenti, hogy a fő verzió középérték egy friss változások az egy vagy több parancsmagok módosítása.  Igen például verzió 1.7.0 egy gyorsjavítás Azure PowerShell 1.x verzióiban szakítószilárdságának módosítás címet.

Szemantikai verziószámozás tanácsokat kaphat az Azure PowerShell további tudnivalókért lásd: az szemantikai, a Verziószámozási beállítások: http://semver.org
 
Ha a legújabb API-k, verzióját kell használni 2.x. De ha a parancsfájlok írása ellen verzió 1.x, és nem szeretné felvegye a bekövetkezett változásokkal kapcsolatban verziójában a 2.x [Kibocsátási megjegyzések](https://github.com/Azure/azure-powershell/blob/dev/documentation/release-notes/migration-guide.2.0.0.md), akkor telepítenie kell a 1.7.0 leírt 2.x.

Olyan verziójú eltéréseket okozhat, ha a profil modul legújabb verziója telepítve van, és ezt követően betöltése a modul, amely attól függ, hogy egy korábbi verzióját. A megoldásához legegyszerűbben telepítse a legújabb .msi. Az .msi automatikusan törli a köteggel modulokat régebbi verzióit.
 
###<a name="installing-module-versions-side-by-side"></a>Modul verzióit egymás mellé telepíti

Verzió 2.1.0 (verzió 1.2.6 AzureStack) az első verziója telepítve legyen tervezett és egymás mellett használják. Mivel az Azure PowerShell bináris modult használja, PowerShell új ablak megnyitása, és importálja a AzureRM parancsmag egy adott verziójához **Import-Module** használatával:

    Import-Module AzureRM -RequiredVersion 2.1.0**

2.1.0 (kívül 1.2.6) nem működnek az előzetes verzió csakúgy egymás melletti más Azure PowerShell-modult verzióival. A fenti hasonló paranccsal Azure PowerShell-modulok korábbi verziójában betöltésekor a **AzureRM.Profile** modul nem kompatibilis verziója töltődik, így a parancsmagok kéri, jelentkezzen be, hogy be van-e jelentkezve után jelenik meg, ha végrehajtása parancsmag,.

A legegyszerűbben úgy, hogy a legújabb Azure PowerShell telepítése a WebPI: az adatcsatorna vagy .msi – ezzel a művelettel eltávolítja a modul telepítve van a gyűjteményből korábbi verzióiban. 

Ne feledje, hogy Azure- és AzureRM modulok függőségek közös, így ha mindkét modulokat, amikor frissíti egy használja, frissítenie kell is. Az Azure modul korábbi verzióit egymás mellett modul, hogy a AzureRM modul korábbi verziói betöltése azonos problémája van van.

<a id="Install"></a>
## <a name="step-1-install"></a>Lépés: 1: telepítése

Az alábbiakban a két módszer, amellyel az Azure PowerShell telepítheti. Telepítheti a PowerShell-tárában vagy a WebPI.

###<a name="installing-azure-powershell-from-the-powershell-gallery"></a>Azure PowerShell telepítése a PowerShell-dokumentumtárból

Az előnyben részesített úgy, hogy PowerShell gyűjtemény használatával. A PowerShell gyűjtemény használatával a PowerShellGet modul van szüksége. Ez a lehetőség itt: [PowerShellGallery.com](https://www.powershellgallery.com/)

Telepítse az Azure PowerShell 1.3.0 vagy nagyobb a PowerShell használatával egy jogú PowerShell integrált parancsfájlok környezet (ISE) vagy a Windows PowerShell figyelmeztető üzenet az alábbi parancsokkal gyűjteményből:

    # Install the Azure Resource Manager modules from the PowerShell Gallery
    Install-Module AzureRM

    # Install the Azure Service Management module from the PowerShell Gallery
    Install-Module Azure

####<a name="more-about-these-commands"></a>További információ: ezek a parancsok

- **A modul telepítése AzureRM** az Azure erőforrás-kezelő parancsmagok az összesítő modul telepítése. A AzureRM modul függ. 
- egy adott verzió minden Azure erőforrás-kezelő modulban esik. A mellékelt verzióját tartomány biztosítja, hogy nincs bekövetkezett változásokkal kapcsolatban modul lehet venni AzureRM modulokat ugyanazzal a fő verzióval telepítésekor. A AzureRM modul telepítésekor bármely Azure erőforrás-kezelő modul, amely a korábban nem lett telepítve letöltött, és telepítve van a PowerShell gyűjteményből. Az Azure PowerShell-modulok által használt szemantikai verziószámozás kapcsolatos további tudnivalókért olvassa el a [semver.org](http://semver.org)című témakört. 
- **A modul telepítése Azure** az Azure modul telepítése. Ez a modul a szolgáltatás modul az Azure PowerShell 0.9.x. Ez nem jelentős módosításokat, és az előző verziójában az Azure modul cserélhető kell tartalmaznia.

###<a name="installing-azure-powershell-from-webpi"></a>Azure PowerShell telepítése a WebPI

Azure PowerShell 1,0 és nagyobb telepíti a WebPI megegyezik a 0.9.x volt. Töltse le az [Azure PowerShell](http://aka.ms/webpi-azps) , és indítsa el a telepítést. Ha Azure PowerShell van telepítve 0.9.x, verzió 0.9.x el fogja távolítani a frissítés részeként. Ha telepítette az Azure PowerShell-modulok gyűjteményből PowerShell, a telepítő automatikusan törli a modulokat konzisztens Azure PowerShell környezet biztosítása érdekében a telepítés előtt.

> [AZURE.NOTE] Ha korábban már telepítette Azure modulok a PowerShell gyűjteményből, a telepítő automatikusan eltávolíthatja őket. Így fejetlenséget kapcsolatos melyik verziója telepítve van, és a hol találhatók. PowerShell gyűjtemény modulok **%ProgramFiles%\WindowsPowerShell\Modules**általában telepíti. A WebPI telepítő viszont telepíti az Azure modulok * *% ProgramFiles (x86) %\Microsoft SDKs\Azure\PowerShell\**. Ha hiba történik a telepítés során, eltávolíthatja az Azure manuálisan* mappák a **%ProgramFiles%\WindowsPowerShell\Modules** mappát, és próbálkozzon újra a telepítést.

Ha a telepítés befejeződött, a ```$env:PSModulePath``` beállítást kell tartalmaznia az Azure PowerShell-parancsmagok tartalmazó könyvtárak.

> [AZURE.NOTE] A PowerShell ismert probléma van **$env: PSModulePath** , amely akkor fordulhat elő, WebPI telepítésekor. A számítógép rendszer frissítéseit vagy más telepítések miatt újraindításra lehet szükség, akkor okozhat frissítések **$env: PSModulePath** az elérési utat, amelyen telepítve van-e az Azure PowerShell nem felvenni. Ez akkor fordulhat elő, ha, üzenet jelenhet meg a "parancsmag nem ismerhető fel" megkísérelte Azure PowerShell-parancsmagok használata a telepítést, vagy a frissítés után. Ez akkor fordulhat elő, ha a számítógép újraindítása kell a probléma megoldásához.

Ha megjelenik egy üzenet, például a következő amikor megpróbál betöltése, vagy parancsmagok végrehajtása:

```
    PS C:\> Get-AzureRmResource
    Get-AzureRmResource : The term 'Get-AzureRmResource' is not recognized as the name of a cmdlet, function,
    script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is
    correct and try again.
    At line:1 char:1
    + Get-AzureRmResource
    + ~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : ObjectNotFound: (get-azurermresourcefork:String) [], CommandNotFoundException
        + FullyQualifiedErrorId : CommandNotFoundException
```

Ezt a számítógép újraindítása és az parancsmag importálása C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\ a következőképpen kell javítani (XXXX PowerShell telepített verziója esetén:

```
Import-Module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\azure.psd1"
Import-Module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\expressroute\expressroute.psd1"
```

## <a name="step-2-start"></a>Lépés: 2: indítása
A parancsmagok futtatását is lehetővé teszi, a Windows PowerShell szabványos konzolról, illetve a PowerShell integrált parancsfájlok környezet (ISE).
A módszerrel is nyissa meg bármelyik konzolt attól függ, hogy a számítógépen Windows esetén:

- Legalább futtató számítógépen Windows 8 vagy Windows Server 2012, használhatja a beépített keresési. A **kezdőképernyőn** írja be a power. Ez a hatókörű listájának alkalmazásokat a Windows PowerShell tartalmazó adja eredményül. A konzol megnyitásához kattintson mindkét alkalmazást. (Az alkalmazás rögzítése a **kezdőképernyőn** , kattintson jobb gombbal az ikonjára.)

- Windows 8 vagy Windows Server 2012-nél korábbi verzióját futtató számítógépen használja a **menü megnyitásához**. Kattintson a **Start** menü **Minden program**parancsára, majd a **Kellékek menüpontra**, kattintson a **Windows PowerShell** mappára, és válassza a **Windows PowerShell**parancsra.

A **Windows PowerShell ISE** elvégezhetők azonos feladatokat kell elvégeznie a Windows PowerShell konzolban menüelemek és billentyűparancsok segítségével is futtathatók. A ISE, a Windows PowerShell konzolban Cmd.exe, vagy a **Futtatás** mezőbe írja be, **powershell_ise.exe**.

###<a name="commands-to-help-you-get-started"></a>Parancsok segít első lépések

    # To make sure the Azure PowerShell module is available after you install
    Get-Module –ListAvailable 
    
    # To log in to Azure Resource Manager
    Login-AzureRmAccount

    # You can also use a specific Tenant if you would like a faster log in experience
    # Login-AzureRmAccount -TenantId xxxx

    # To view all subscriptions for your account
    Get-AzureRmSubscription

    # To select a default subscription for your current session
    Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

    # View your current Azure PowerShell session context
    # This session state is only applicable to the current session and will not affect other sessions
    Get-AzureRmContext

    # To select the default storage context for your current session
    Set-AzureRmCurrentStorageAccount –ResourceGroupName “your resource group” –StorageAccountName “your storage account name”

    # View your current Azure PowerShell session context
    # Note: the CurrentStorageAccount is now set in your session context
    Get-AzureRmContext

    # To list all of the blobs in all of your containers in all of your accounts
    Get-AzureRmStorageAccount | Get-AzureStorageContainer | Get-AzureStorageBlob


## <a name="step-3-connect"></a>3 lépés: csatlakozás
A parancsmagok előfizetése van szüksége, így azok kezelhetik a szolgáltatások. Egy Azure-előfizetést vásárolhat, ha még nem rendelkezik. Útmutatásért lásd: [hogyan Azure vásárolható meg](http://go.microsoft.com/fwlink/p/?LinkId=320795).

1. Írja be **Bejelentkezési-AzureRmAccount**

2. Írja be az e-mail címét és a fiókjához tartozó jelszót. Azure hitelesíti menti a hitelesítő adatait, és kattintson az ablak bezárása.

– VAGY –

Jelentkezzen be a munkahelyi vagy iskolai fiókjával:

    $cred = Get-Credential
    Login-AzureRmAccount -Credential $cred
> [AZURE.NOTE] Ha egynél több bérlői a szervezeti fiókkal társított, adja meg a TenantId paraméter:

    $loadersubscription = Get-AzureRmSubscription -SubscriptionName $YourSubscriptionName -TenantId $YourAssociatedSubscriptionTenantId


> [AZURE.NOTE] Ez a módszer nem interaktív napló csak akkor működik, munkahelyi vagy iskolai fiókjával. A munkahelyi vagy iskolai fiókkal, amely a munkahelyi vagy iskolai kezeli, és definiált az Azure Active Directory-példány a munkahelyi vagy iskolai felhasználó. Ha jelenleg nem rendelkeznek a munkahelyi vagy iskolai fiókjával, és segítségével egy Microsoft-fiókkal jelentkezzen be az Azure-előfizetése, akkor egyszerűen létrehozhat egyet az alábbi lépésekkel.

> 1. Jelentkezzen be az [Azure klasszikus portálra](https://manage.windowsazure.com), és **Az Active**Directory gombra.

> 2. Ha nincs könyvtár létezik, jelölje ki **a címtárában létrehozása** , és adja meg a kért adatokat.

> 3. Jelölje ki azt a könyvtárat és új felhasználó hozzáadása. Ezt az új felhasználót is jelentkezzen be a munkahelyi vagy iskolai fiókjával. A felhasználó kibocsátása során fog adni mindkét e-mail címmel rendelkező a felhasználók és az ideiglenes jelszót. Ezt az információt, mentse, ahogy az alábbi 5 használatos.

> 4. Az Azure klasszikus portálról válassza a **Beállítások** , és válassza a **rendszergazda**. Válassza a **Hozzáadás**, majd az új felhasználó hozzáadása közös rendszergazdaként. Ezzel az Azure előfizetés kezeléséhez a munkahelyi vagy iskolai fiókjával.

> 5. Végül az Azure klasszikus portal kijelentkezik, és jelentkezzen be újra a munka használatával, vagy iskolai fiókjával. Ha a első alkalommal naplózás be ehhez a fiókhoz, a rendszer kéri, módosíthatja a jelszavát.

> További információt a Microsoft Azure munkahelyi vagy iskolai fiókkal feliratkozik olvassa el a [Microsoft Azure, mint egy szervezet regisztrálhat](./active-directory/sign-up-organization.md)című témakört.

> Az Azure-hitelesítés és az előfizetés kezelésével kapcsolatos további tudnivalókért lásd: [a fiókok kezelése, az előfizetések elemre, és a rendszergazdai szerepkörök](http://go.microsoft.com/fwlink/?LinkId=324796).

### <a name="view-account-and-subscription-details"></a>Fiók és az előfizetés részletei

Több fiókok és Azure PowerShell által használható előfizetések is. Több fiók **Felvétele – AzureRmAccount** többször futtatásával is hozzáadhat.

A rendelkezésre álló Azure fiókok megjeleníteni, írja be a **Get-AzureAccount**.

Az Azure előfizetések megjeleníteni, írja be a **Get-AzureRmSubscription**.

##<a id="Help"></a>A Súgó használata##

Ezek az erőforrások adott parancsmagok nyújt segítséget:


-   Az a konzolról, használhatja a beépített súgórendszerét. A **Súgó** parancsmag a rendszer hozzáférést biztosít. 

- Segítségre van szüksége a Közösség próbálkozzon a népszerű fórumok:

 - [Azure fórum az MSDN webhelyen]( http://go.microsoft.com/fwlink/p/?LinkId=320212)
 - [Stackoverflow](http://go.microsoft.com/fwlink/?LinkId=320213)

##<a name="learn-more"></a>tudj meg többet


A parancsmagok használatáról további információt az alábbi forrásokban talál:

A Windows PowerShell használatával kapcsolatos alapvető útmutatásért lásd: [A Windows PowerShell használatával](http://go.microsoft.com/fwlink/p/?LinkId=321939).

A parancsmagok hivatkozás információkért olvassa el a [Azure parancsmagjai – referencia](https://msdn.microsoft.com/library/windowsazure/jj554330.aspx)című témakört.

Minta parancsfájlok és útmutatást segítséget, olvassa el a parancsfájlok Azure kezelése című témakörben talál a [Parancsprogram-központban](http://go.microsoft.com/fwlink/p/?LinkId=321940).


<properties
    pageTitle="Első lépések az Azure kulcs tárolóra |} Microsoft Azure"
    description="Ebben az oktatóanyagban segítségével első lépések az Azure kulcs tárolóra megerősített tároló létrehozása az Azure tárolhatják és kezelhetik a titkosítási kulcs és titkos kulcsok Azure-ban."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/24/2016"
    ms.author="cabailey"/>

# <a name="get-started-with-azure-key-vault"></a>Első lépések az Azure kulcs tárolóból elemre #
Azure kulcs tárolóból elemre a legtöbb régióban érhető el. További tudnivalókért olvassa el a [kulcs tárolóra árak, oldal](https://azure.microsoft.com/pricing/details/key-vault/)című témakört.

## <a name="introduction"></a>– Bevezetés  
Ebben az oktatóanyagban segítségével első lépések az Azure kulcs tárolóból elemre (a tárolóból elemre) megerősített tároló létrehozása az Azure tárolhatják és kezelhetik a titkosítási kulcs és titkos kulcsok Azure-ban. Végigvezeti a folyamaton, hozzon létre egy tárolóból elemre, amely tartalmaz egy billentyűt vagy a jelszót, majd használhatja az Azure alkalmazások Azure PowerShell használatával. Majd megtudhatja, hogyan használhatja az alkalmazás a billentyűt, vagy a jelszó.

**Becsült időt vesz igénybe:** 20 perc

>[AZURE.NOTE]  Ebben az oktatóanyagban útmutatót írni az Azure alkalmazást, hogy a lépések egyikét tartalmazza, azaz hogyan engedélyezheti billentyűt vagy titkos használata a fő tárolóból elemre az alkalmazás nem része.
>
>Ebben az oktatóanyagban Azure PowerShell használja. Platformok parancssori felület útmutatásért lásd: [Ebben az oktatóanyagban egyenértékű](key-vault-manage-with-cli.md).

Azure kulcs tárolóra kapcsolatos információ áttekintése [Mi az Azure kulcs tárolóra?](key-vault-whatis.md)

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban befejezéséhez az alábbiakat kell rendelkeznie:

- Microsoft Azure-előfizetést. Ha nem rendelkezik egy, jelentkezzen [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).
- Azure PowerShell **1.1.0 a legkisebb verziószáma**. Azure PowerShell telepítése, és azt társítása Azure-előfizetése, megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md). Ha már telepítette az Azure PowerShell, és nem tudja, hogy a verziót, a Azure PowerShell konzolról, írja be a `(Get-Module azure -ListAvailable).Version`. Ha Azure PowerShell verziója telepítve 0.9.8 keresztül 0.9.1, a kisebb módosításokat továbbra is használhatja a ebben az oktatóanyagban. Ha például kell használnia a `Switch-AzureMode AzureResourceManager` megváltoztak parancs és az egyes parancsok Azure kulcs tárolóból elemre. Verziók 0.9.1 keresztül 0.9.8 kulcs tárolóra parancsmagjai listáját lásd: [Azure kulcs tárolóra parancsmagok](https://msdn.microsoft.com/library/azure/dn868052\(v=azure.98\).aspx). 
- A kulcs vagy ebben az oktatóanyagban létrehozott jelszót konfigurált alkalmazás. Egy minta alkalmazás érhető el a [Microsoft letöltőközpontból](http://www.microsoft.com/en-us/download/details.aspx?id=45343). További tudnivalókért lásd: a kapcsolódó fontos fájl.


Ebben az oktatóanyagban készült Azure PowerShell kezdőknek, de azt feltételezi, hogy megismeri a alapfogalmak, például modulokat, parancsmagok és a munkamenetek. További tudnivalókért lásd: a [Windows PowerShell – első lépések](https://technet.microsoft.com/library/hh857337.aspx).

Részletes bármely parancsmag ebben az oktatóanyagban megjelenik a, használja a **Get-Help** parancsmag.

    Get-Help <cmdlet-name> -Detailed

A **Bejelentkezési-AzureRmAccount** parancsmag súgójának, írja be például a:

    Get-Help Login-AzureRmAccount -Detailed

Az alábbi oktatóanyagok az Ismerkedés az Azure erőforrás-kezelő az Azure PowerShell is erről:

- [Telepítse és állítsa be a Azure PowerShell hogyan](../powershell-install-configure.md)
- [Az erőforrás-kezelő Azure PowerShell használatával](../powershell-azure-resource-manager.md)


## <a id="connect"></a>Az előfizetések csatlakoztatása ##

Indítsa el az Azure PowerShell-munkamenetet, és jelentkezzen be az Azure-fiók a következő parancsot:  

    Login-AzureRmAccount 

Megjegyzendő, hogy ha példányára Azure, például az Azure kormányzati, használja az - környezet paraméter a használja ezt a parancsot. Példa:`Login-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)`

Az előugró böngészőablakához írja be az Azure-fiók felhasználónevét és jelszavát. Azure PowerShell kapja meg az előfizetések alapértelmezés szerint, és az ehhez a fiókhoz tartozó, használja a legelső.

Ha több előfizetéssel rendelkezik, és szeretne megadni egy adott megjegyzés használandó Azure kulcs tárolóból elemre, írja be az alábbi módon lásd: a fiók az előfizetések:

    Get-AzureRmSubscription

Az előfizetés használandó megadásához írja be:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Azure PowerShell beállításával kapcsolatos további tudnivalókért megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md).


## <a id="resource"></a>Hozzon létre egy új erőforráscsoport ##

Azure erőforrás-kezelő használatakor az összes kapcsolódó erőforrások erőforráscsoport belül jönnek létre. Azt hoz létre egy új erőforráscsoport ebben az oktatóanyagban **ContosoResourceGroup** neve:

    New-AzureRmResourceGroup –Name 'ContosoResourceGroup' –Location 'East Asia'


## <a id="vault"></a>Hozzon létre egy fő tárolóból elemre ##

A [New-AzureRmKeyVault](https://msdn.microsoft.com/library/azure/mt603736\(v=azure.300\).aspx) parancsmag használatával hozzon létre egy fő tárolóból elemre. Ezzel a parancsmaggal három kötelező paraméterek tartalmaz: egy **erőforrás csoport neve**, a **fő tárolóra neve**és a **földrajzi hely**.

Ha **ContosoKeyVault**tárolóra nevét, az erőforrás-csoport nevét a **ContosoResourceGroup**és **kelet-ázsiai**helyét, írja be például a:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

Ez a parancsmag kimenete a fő tárolóra éppen létrehozott tulajdonságainak megjelenítése. A két legfontosabb tulajdonságokat a következők:

- **Tárolóra neve**: példában, ez az **ContosoKeyVault**. Ezt a nevet más kulcs tárolóra parancsmagok fogja használni.
- **Tárolóra URI**: példában, ez az https://contosokeyvault.vault.azure.net/. A tárolóból elemre keresztül a REST API-t használó alkalmazások e URI kell használnia.

Az Azure-fiók engedélyezve, hogy a kulcsfontosságú tárolóra bármely műveletek hajthatók végre. A még senki sem egyéb nem.

>[AZURE.NOTE]  Ha **az előfizetés nem regisztrált névtér "Microsoft.KeyVault"** hibaüzenet jelenik meg szeretné az új kulcs tárolóból elemre, futtatása közben `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"` majd futtassa újra a New-AzureRmKeyVault parancsot. További tudnivalókért olvassa el a [Külső.FÜGGV-AzureRmResourceProvider](https://msdn.microsoft.com/library/azure/mt759831\(v=azure.300\).aspx)című témakört.
>

## <a id="add"></a>Billentyű vagy titkos hozzáadása a fő tárolóból elemre ##

Ha azt szeretné, hogy a szoftver védett kulcs létrehozásához, az Azure kulcs tárolóból elemre, [Hozzáadás-AzureKeyVaultKey](https://msdn.microsoft.com/library/azure/dn868048\(v=azure.300\).aspx) parancsmag, és írja be a következőt:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -Destination 'Software'

Jó helyen jár Ha egy meglévő szoftverrel védett kulcs van-e egy. Azure kulcs tárolóból elemre, feltölteni kívánt softkey.pfx nevű fájlban C:\ meghajtóra mentett PFX fájlt, írja be a következőt a változó **securepfxpwd** az **123** -jelszó beállítása a. PFX fájl:

    $securepfxpwd = ConvertTo-SecureString –String '123' –AsPlainText –Force

Írja be a következőt a billentyű importálhatja a. A kulcs tárolóra szolgáltatásban szoftverrel kulcsot védi PFX fájl:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd


A kulcs létrehozott vagy töltenek fel Azure kulcs tárolóból elemre, a saját URI használatával most hivatkozhat. Mindig az aktuális verziójának beszerzése **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** használja, és ez a verzió **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** segítségével.  

A következő kulcsot URI megjelenítéséhez, írja be:

    $Key.key.kid

A titkos hozzáadása a tárolóból elemre, amely SQLPassword nevű jelszó és Pa$ $w0rd és Azure kulcs tárolóra értékkel rendelkezik, először a érték konvertálása a Pa$ $w0rd biztonságos karakterlánc írja be a következő:

    $secretvalue = ConvertTo-SecureString 'Pa$$w0rd' -AsPlainText -Force

Írja be a következőt:

    $secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword' -SecretValue $secretvalue

Ezt a jelszót az Azure kulcs tárolóból elemre, a URI használatával hozzáadott most hivatkozhat. Mindig az aktuális verziójának beszerzése **https://ContosoVault.vault.azure.net/secrets/SQLPassword** használja, és ez a verzió **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** segítségével.

A titkos a URI megjelenítéséhez, írja be:

    $secret.Id

Vegyük megtekintése az kulcs vagy az éppen létrehozott titkos:

- Ha szeretné megtekinteni a termékkulcsot, és írja be a`Get-AzureKeyVaultKey –VaultName 'ContosoKeyVault'`
- A titkos, írja be:`Get-AzureKeyVaultSecret –VaultName 'ContosoKeyVault'`

Most a fő tárolóból elemre, és a billentyűt vagy a titkos készen áll a alkalmazások használatához. Engedélyeznie kell az alkalmazások használatát.  

## <a id="register"></a>Azure Active Directory-alkalmazás regisztrálása ##

Ebben a lépésben rendszerint a másik számítógépen, egy fejlesztő által kész gombra. Nem kötődik Azure kulcs tárolóból elemre, de megtalálható itt teljesség érdekében.


>[AZURE.IMPORTANT] Az oktatóprogram, a fiók, a tárolóból elemre, és az alkalmazást, rögzítheti ebben a lépésben befejezéséhez kell lennie az azonos Azure Directoryban.

A fő tárolóra használó alkalmazások kell hitelesíteni a jogkivonat az Azure Active Directory használatával. Ehhez a tulajdonos, az alkalmazás első regisztrálnia kell az alkalmazás az Azure Active Directory. Regisztráció végén az alkalmazás tulajdonosa módja az alábbi értékeket:


- Egy **Azonosítója** (más néven ügyfél-azonosító) és a **hitelesítés, kulcs** (más néven a megosztott titkos). Az alkalmazás kell bemutatnia mindkét ezeket az értékeket az Azure Active Directory jogkivonat megszerezni. Ehhez az alkalmazás beállításaitól attól függ, hogy az alkalmazás. A kulcs tárolóra minta alkalmazáshoz az alkalmazás tulajdonosa ezeket az értékeket a app.config fájlban állítja be.

Az alkalmazás rögzítése az Azure Active Directory:

1. Jelentkezzen be az Azure klasszikus portálra.
2. A bal oldali **Az Active Directory**gombra, és válassza az alkalmazás regisztrálja könyvtár. <br> <br> **Megjegyzés:** Ki kell választania ugyanabban a könyvtárban, amely tartalmazza az Azure előfizetést, amelyhez létrehozott a fő tárolóból elemre. Ha nem tudja, hogy melyik címtár ez nem, kattintson a **Beállítások**gombra, az előfizetést, amelyhez a fő tárolóra létrehozott azonosítása és jegyezze fel a címtár-az utolsó oszlopban jelennek meg.

3. Kattintson az **alkalmazások**elemre. Ha nincs alkalmazások felvétele a címtárhoz, ezen a lapon megjelenik a csak az **alkalmazás hozzáadása** hivatkozásra. Kattintson a hivatkozásra, vagy másik lehetőségként kattinthat **hozzáadása** a parancssávon.
4.  Az **Alkalmazás hozzáadása** varázslóban kattintson a **milyen feladatot szeretne tenni?** lapján kattintson a **szervezeten elkészítésének az alkalmazás hozzáadása**.
5.  A **mondja el nekünk az alkalmazásról** lapon adja meg az alkalmazás nevét, és válassza a **WEBES API WEB APPLICATION Programming és/vagy** a (alapértelmezett). Kattintson a **következő** ikonra.
6.  Az **alkalmazás tulajdonságai** lapon adja meg a **SIGN-ON URL-CÍMEK** és az **Alkalmazás azonosítója URI** a webes alkalmazáshoz. Ha az alkalmazás nincs ezeket az értékeket, akkor is használhatja őket tagjának ezt a lépést (például megadhatja az mindkettőben http://test1.contoso.com). Nem számít, hogy ezek a webhelyek létezik. Mi az fontos, hogy az alkalmazás azonosítója URI az egyes alkalmazások eltér a minden alkalmazás címtárában. A könyvtár a karakterlánc használja az alkalmazás azonosítására.
7.  Kattintson a **kész** ikonra kattintva mentse a módosításokat a varázslóban.
8.  **Első lépések** lapon kattintson a **KONFIGURÁLÁS**gombra.
9.  Görgetéssel keresse meg a **billentyűk** szakaszt, jelölje ki az időtartamot, és kattintson a **MENTÉS**gombra. Frissíti a lapot, és megjelenik egy kulcsmező értékét. A kulcs értékét, és az **Ügyfél-azonosító** értéke meg kell adnia az alkalmazást. (Ez a beállítás utasításokat az alkalmazás-specifikus is.)
10. Az ügyfél-azonosító értéket másolja a lapot, amely a következő lépésben használandó engedélyeket állíthat be a tárolóból elemre.

## <a id="authorize"></a>Az alkalmazás használatához a billentyűt vagy a titkos engedélyezése ##

Engedélyezi a billentyűt vagy a tárolóból elemre a titkos eléréséhez az alkalmazás, használja a  [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/azure/mt603625\(v=azure.300\).aspx) parancsmag.

Például ha a tárolóból elemre neve **ContosoKeyVault** és az alkalmazás hitelesíteni kívánt van a egy 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed az ügyfél-azonosító, és az alkalmazás fejthető vissza, és jelentkezzen be a tárolóból elemre a billentyűkkel hitelesíteni kívánt, futtassa a következő:


    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Ha szeretné engedélyezni, hogy ugyanazt a kérelmet a tárolóból elemre a titkos kulcsok olvasható, futtassa a következő:


    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get

## <a id="HSM"></a>Ha szeretné használni a hardveres biztonsági modult (HSM) ##

A hozzáadott garancia importálhat és kulcsok létrehozásához a hardver biztonsági modulokat (HSMs), amely a HSM oszlopazonosító soha ne hagyja. A HSMs 140-2 szintet 2 érvényesített FIPS. Ha ez a követelmény nem vonatkozik Önre, átugorja ezt a szakaszt, és nyissa meg [a fő tárolóra törlése, valamint a kapcsolódó kulcsok és titkos kulcsok](#delete).

HSM védett billentyűk létrehozásához az [Azure kulcs tárolóra prémium szolgáltatási réteg HSM védett kulcsok támogatásához](https://azure.microsoft.com/pricing/free-trial/)kell használnia. Ezeken kívül figyelje meg, hogy ez a funkció nem érhető el az Azure Kína.


A fő tárolóra létrehozásakor a **- Termékváltozat** paraméter felvétele


    New-AzureRmKeyVault -VaultName 'ContosoKeyVaultHSM' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -SKU 'Premium'



Szoftverrel védett (ahogy korábbi) és HSM védett kulcsokról hozzáadhatja a fő tárolóból elemre. Hozzon létre egy HSM védett kulcsot, állítsa be a **-cél** "HSM" paramétere:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -Destination 'HSM'

A következő parancs használatával a kulcs importálása egy. PFX fájlt a számítógépen. Ez a parancs a kulcs importálása a kulcs tárolóra szolgáltatásban a HSMs:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd -Destination 'HSM'


A következő parancs importálja a "hozása saját kulcsot" (BYOK) csomagot. Ebben az esetben teszi lehetővé a helyi HSM készítése a termékkulcsot, és átviszi HSMs a kulcs tárolóra szolgáltatásban a kulcs az HSM oszlopazonosító elhagyása nélkül:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\ITByok.byok' -Destination 'HSM'

Részletesebb arról, hogy miként BYOK csomag készítése című cikkben olvashat [létrehozása és a továbbított HSM védett billentyűparancsok Azure kulcs tárolóból elemre](key-vault-hsm-protected-keys.md).

## <a id="delete"></a>A fő tárolóból elemre, és a kapcsolódó kulcsok és a titkos kulcsok törlése ##

Ha már nincs szüksége, a fő tárolóból elemre, és a kulcs vagy titkos, amely tartalmaz, a fő tárolóból elemre az [Eltávolítás-AzureRmKeyVault](https://msdn.microsoft.com/library/azure/mt619485\(v=azure.300\).aspx) parancsmag használatával törölheti:

    Remove-AzureRmKeyVault -VaultName 'ContosoKeyVault'

Vagy törölhet is egy teljes Azure erőforráscsoport, amelyek tartalmazzák még a fő tárolóból elemre, és abba a csoportba más erőforrások:

    Remove-AzureRmResourceGroup -ResourceGroupName 'ContosoResourceGroup'


## <a id="other"></a>Más Azure PowerShell-parancsmagok ##

Más hasznosak lehetnek Azure kulcs tárolóra kezelésére szolgáló parancsok:

- `$Keys = Get-AzureKeyVaultKey -VaultName 'ContosoKeyVault'`: Ez a parancs a táblázatos megjelenítő kulcsok és a kijelölt tulajdonságok kap.
- `$Keys[0]`: Ez a parancs a megadott kulcs tulajdonságainak teljes listáját jeleníti meg.
- `Get-AzureKeyVaultSecret`: Ez a parancs megjeleníti a titkos nevét és a kijelölt tulajdonságok táblázatos beállításra.
- `Remove-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey'`: Példa egy adott kulcs eltávolítása.
- `Remove-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword'`: Példa egy adott titkos eltávolítása.


## <a id="next"></a>Következő lépések ##

A nyomon követési tanulásra Azure kulcs tárolóból elemre a webes alkalmazásokban használó lásd: [Használata Azure kulcs webalkalmazást a tárolóból elemre](key-vault-use-from-web-application.md).

A fő tárolóra használatának talál további [Azure kulcs naplózás tárolóból elemre](key-vault-logging.md).

A legújabb Azure PowerShell-parancsmagok az Azure kulcs tárolóra listáját lásd: [Azure kulcs tárolóra parancsmagok](https://msdn.microsoft.com/library/azure/dn868052\(v=azure.300\).aspx). 
 

Programozási hivatkozások, [fejlesztői](key-vault-developers-guide.md)útmutatójában található az Azure kulcs tárolóból elemre.

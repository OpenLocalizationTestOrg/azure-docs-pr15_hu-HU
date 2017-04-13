<properties
    pageTitle="Kulcs tárolóra CLI használatával kezelése |} Microsoft Azure"
    description="Ebben az oktatóanyagban használatával automatizálhatja a gyakori feladatok a kulcs tárolóból elemre a CLI használatával"
    services="key-vault"
    documentationCenter=""
    authors="BrucePerlerMS"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/26/2016"
    ms.author="bruceper"/>

# <a name="manage-key-vault-using-cli"></a>Kulcs tárolóra CLI használatával kezelése #
Azure kulcs tárolóból elemre a legtöbb régióban érhető el. További tudnivalókért olvassa el a [kulcs tárolóra árak, oldal](https://azure.microsoft.com/pricing/details/key-vault/)című témakört.

## <a name="introduction"></a>– Bevezetés  
Ebben az oktatóanyagban segítségével első lépések az Azure kulcs tárolóból elemre (a tárolóból elemre) megerősített tároló létrehozása az Azure tárolhatják és kezelhetik a titkosítási kulcs és titkos kulcsok Azure-ban. Végigvezeti a folyamaton, hozzon létre egy tárolóból elemre egy billentyűt vagy a jelszót, majd használhatja az Azure alkalmazások tartalmazó Azure-platformok parancssori felület használatával. Majd megtudhatja, hogyan alkalmazás ezután felhasználhatja billentyűt, vagy a jelszó.

**Becsült időt vesz igénybe:** 20 perc

>[AZURE.NOTE]  Ebben az oktatóanyagban nem tartalmaz útmutatást írni az Azure alkalmazást, hogy a lépések egyikét tartalmazza, amely bemutatja, hogyan engedélyezheti egy alkalmazás titkos kulcs vagy a fő tárolóból elemre.
>
>Azure kulcs tárolóból elemre jelenleg nem konfigurálja az Azure-portálon. Ehelyett járjon platformok parancssori felület. Másik lehetőségként Azure PowerShell útmutatásért lásd: [Ebben az oktatóanyagban egyenértékű](key-vault-get-started.md).

Azure kulcs tárolóra kapcsolatos információ áttekintése [Mi az Azure kulcs tárolóra?](key-vault-whatis.md)

## <a name="prerequisites"></a>Előfeltételek
Ebben az oktatóanyagban befejezéséhez az alábbiakat kell rendelkeznie:

- Microsoft Azure-előfizetést. Ha nem rendelkezik egy, jelentkezzen az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial).
- Parancssor verzió 0.9.1 vagy újabb verziójában. Telepítse a legújabb verziót, és csatlakozzon az Azure-előfizetése, lásd: [Telepítse és állítsa be az Azure-platformok parancssort](../xplat-cli-install.md).
- A kulcs vagy ebben az oktatóanyagban létrehozott jelszót konfigurált alkalmazás. Egy minta alkalmazás érhető el a [Microsoft letöltőközpontból](http://www.microsoft.com/download/details.aspx?id=45343). További tudnivalókért lásd: a kapcsolódó fontos fájl.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Azure-platformok parancssori felület és a Súgó használata

Ebben az oktatóanyagban feltételezi, hogy ismeri a parancssort (Bash, Terminálszolgáltatások, parancssor)

A – Súgó és -h paraméter bizonyos parancsok súgójának megtekintéséhez használható. Másik megoldás az azure súgó [parancs] [kapcsolók] formátumban való visszatéréshez ugyanazokat az információkat is használható. A következő parancsok végrehajtása az összes például ugyanazokat az adatokat adja vissza:

    azure account set --help

    azure account set -h

    azure help account set

Ha kétségei vannak a paraméterek szükség szerint egy parancs, a hivatkozott használatával – súgó, -h vagy azure [parancs].

Az alábbi oktatóanyagok az Ismerkedés az Azure erőforrás-kezelő Azure-platformok parancssori felület is erről:

- [Hogyan telepítse és állítsa be az Azure-platformok parancssor](../xplat-cli-install.md)
- [Felhasználói felületén Azure-platformok parancssori a Azure-kezelő eszközzel](../xplat-cli-azure-resource-manager.md)


## <a name="connect-to-your-subscriptions"></a>Az előfizetések csatlakoztatása

Szervezeti fiókkal történő bejelentkezés, használja az alábbi parancsot:

    azure login -u username -p password

vagy ha azt szeretné, interaktív beírásával bejelentkezés

    azure login

>[AZURE.NOTE]  A bejelentkezési módszer csak akkor működik, szervezeti fiókkal. Szervezeti fiókkal a szervezet által felügyelt, és a szervezet Azure Active Directory-ös bérlői definiált felhasználó.


Ha jelenleg nem rendelkeznek a szervezeti fiókkal, és segítségével egy Microsoft-fiókkal jelentkezzen be az Azure-előfizetése, akkor egyszerűen létrehozhat egyet az alábbi lépésekkel.

1.  Jelentkezzen be a bejelentkezési az [Azure Kezelőportálja](https://manage.windowsazure.com/), és kattintson az Active Directory.
2.  Ha nincs könyvtár létezik, válassza a Create a címtárában, és adja meg a kért adatokat.
3.  Jelölje ki azt a könyvtárat és új felhasználó hozzáadása. Új felhasználóhoz is hozzárendeljen szervezeti fiókkal. A felhasználó kibocsátása során fog adni mindkét e-mail címmel rendelkező a felhasználók és az ideiglenes jelszót. Mentse a ezt az információt, egy másik lépésben szempontjából.
4.  A portálon válassza a beállítások, és válassza a rendszergazda. Válassza a Hozzáadás, majd az új felhasználó hozzáadása közös rendszergazdaként. Ezzel a szervezeti fiók az Azure előfizetés kezeléséhez.
5.  Végül az Azure portál kijelentkezik, majd jelentkezzen be újra az új szervezeti fiókkal. Ha a első alkalommal naplózás be ehhez a fiókhoz, a rendszer kéri, módosíthatja a jelszavát.

További információt a Microsoft Azure rendelkező szervezeti fiókkal történő olvassa el a [Microsoft Azure, mint egy szervezet regisztrálhat](../active-directory/sign-up-organization.md)című témakört.

Ha több előfizetéssel rendelkezik, és szeretne megadni egy adott megjegyzés használandó Azure kulcs tárolóból elemre, írja be az alábbi módon lásd: a fiók az előfizetések:

    azure account list

Az előfizetés használandó megadásához írja be:

    azure account set <subscription name>

Azure-platformok parancssor beállításával kapcsolatos további tudnivalókért lásd: [telepítése és beállítása Azure-platformok parancssori felület](../xplat-cli-install.md).


## <a name="switch-to-using-azure-resource-manager"></a>Váltás Azure erőforrás-kezelő használatával

A kulcs tárolóra Azure erőforrás-kezelő elő kell készítenie, tehát írja be a következőt Azure erőforrás-kezelő módra válthat:

    azure config mode arm

## <a name="create-a-new-resource-group"></a>Hozzon létre egy új erőforráscsoport

Azure erőforrás-kezelő használatával az összes kapcsolódó erőforrások erőforráscsoport belül létrejönnek. Ebben az oktatóanyagban azt hoz létre egy új erőforráscsoport "ContosoResourceGroup".

    azure group create 'ContosoResourceGroup' 'East Asia'

Az első paraméterként erőforráscsoport nevét, és a második paraméter helyét. A parancs használata a helyet, `azure location list` azonosítja, hogyan kell megadni ebben a példában egy másik helyre. Ha további információra van szüksége, írja be:`azure help location`

## <a name="register-the-key-vault-resource-provider"></a>Regisztráció a kulcs tárolóból elemre az erőforrás-szolgáltató
Győződjön meg arról, hogy kulcs tárolóból elemre az erőforrás-szolgáltató az előfizetése van regisztrálva:

`azure provider register Microsoft.KeyVault`

Ez csak egyszer kell megtennie előfizetésenként kell.


## <a name="create-a-key-vault"></a>Hozzon létre egy fő tárolóból elemre

Használja a `azure keyvault create` parancs, amellyel hozzon létre egy fő tárolóból elemre. A parancsfájl még három kötelező paraméter: egy erőforrás csoport neve, fő tárolóra nevét és földrajzi helyét.

Ha ContosoKeyVault tárolóra nevét, az erőforrás-csoport nevét a ContosoResourceGroup és kelet-ázsiai helyét, írja be például a:

    azure keyvault create --vault-name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'

Ez a parancs megjeleníti a tulajdonságait, az éppen létrehozott kulcs tárolóból elemre. A két legfontosabb tulajdonságokat a következők:

- **Név**: a példában az ContosoKeyVault. Ezt a nevet más kulcs tárolóra parancsmagok fogja használni.
- **vaultUri**: a példában az https://contosokeyvault.vault.azure.net. A tárolóból elemre keresztül a REST API-t használó alkalmazások e URI kell használnia.

Az Azure-fiók engedélyezve, hogy a kulcsfontosságú tárolóra bármely műveletek hajthatók végre. A még senki sem egyéb nem.


## <a name="add-a-key-or-secret-to-the-key-vault"></a>Billentyű vagy titkos hozzáadása a fő tárolóból elemre

Ha azt szeretné, hogy a szoftver védett kulcs létrehozásához, az Azure kulcs tárolóból elemre, használja a `azure key create` parancsot, és írja be a következőt:

    azure keyvault key create --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --destination software

Azonban Azure kulcs tárolóra feltölteni kívánt softkey.pem nevű fájlban helyi fájlként mentett .pem fájlban ha egy meglévő billentyűt, írja be a következőt a billentyű importálása a. A kulcs tárolóra szolgáltatásban szoftverrel kulcsot védi PEM fájl:

    azure keyvault key import --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --pem-file './softkey.pem' --password 'PaSSWORD' --destination software

Most már hivatkozhat a létrehozott vagy töltenek fel Azure kulcs tárolóból elemre, a saját URI használatával billentyűt. Mindig az aktuális verziójának beszerzése **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** használja, és ez a verzió **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** segítségével.

A titkos hozzáadása a tárolóból elemre, amely SQLPassword nevű jelszót, és a Pa$ $w0rd Azure kulcs tárolóból elemre kattintva értékkel rendelkezik, amely, írja be a következőt:

    azure keyvault secret set --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword' --value 'Pa$$w0rd'

Ezt a jelszót az Azure kulcs tárolóból elemre, a URI használatával hozzáadott most hivatkozhat. Mindig az aktuális verziójának beszerzése **https://ContosoVault.vault.azure.net/secrets/SQLPassword** használja, és ez a verzió **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** segítségével.

Vegyük megtekintése az kulcs vagy az éppen létrehozott titkos:

- Ha szeretné megtekinteni a termékkulcsot, és írja be a`azure keyvault key list --vault-name 'ContosoKeyVault'`
- A titkos, írja be:`azure keyvault secret list --vault-name 'ContosoKeyVault'`


## <a name="register-an-application-with-azure-active-directory"></a>Azure Active Directory-alkalmazás regisztrálása

Ebben a lépésben rendszerint a másik számítógépen, egy fejlesztő által kész gombra. Nem meghatározott Azure kulcs tárolóból elemre kattintva de szerepel ebben az esetben a teljesség.


>[AZURE.IMPORTANT] Az oktatóprogram, a fiók, a tárolóból elemre, és az alkalmazást, rögzítheti ebben a lépésben befejezéséhez kell lennie az azonos Azure Directoryban.

A fő tárolóra használó alkalmazások kell hitelesíteni a jogkivonat az Azure Active Directory használatával. Ehhez a tulajdonos, az alkalmazás első regisztrálnia kell az alkalmazás az Azure Active Directory. Regisztráció végén az alkalmazás tulajdonosa módja az alábbi értékeket:


- Egy **Azonosítója** (más néven ügyfél-azonosító) és a **hitelesítés, kulcs** (más néven a megosztott titkos). Az alkalmazás kell bemutatnia Azure Active Directory jogkivonat megszerezni mindkét ezeket az értékeket. Ehhez az alkalmazás beállításaitól attól függ, hogy az alkalmazás. A kulcs tárolóra minta alkalmazáshoz az alkalmazás tulajdonosa ezeket az értékeket a app.config fájlban állítja be.



Az alkalmazás rögzítése az Azure Active Directory:

1. Jelentkezzen be az Azure-portálra.
2. A bal oldali **Az Active Directory**gombra, és válassza az alkalmazás regisztrálja könyvtár. <br> <br> Megjegyzés: Ki kell választania ugyanabban a könyvtárban, amely tartalmazza az Azure előfizetést, amelyhez létrehozott a fő tárolóból elemre. Ha nem tudja, hogy melyik címtár ez nem, kattintson a **Beállítások**gombra, az előfizetést, amelyhez a fő tárolóra létrehozott azonosítása és jegyezze fel a címtár-az utolsó oszlopban jelennek meg.

3. Kattintson az **alkalmazások**elemre. A címtárhoz felvett nincs alkalmazások, ha ez a lapon megjelenik a csak az **alkalmazás hozzáadása** hivatkozásra. Kattintson a hivatkozásra, vagy másik lehetőségként kattinthat az **hozzáadása** a parancssávon.
4.  Az **Alkalmazás hozzáadása** varázslóban kattintson a **milyen feladatot szeretne tenni?** lapján kattintson a **szervezeten elkészítésének az alkalmazás hozzáadása**.
5.  A **mondja el nekünk az alkalmazásról** lapon adja meg az alkalmazás nevét, és jelölje be a **WEBES API WEB APPLICATION Programming és/vagy** a (alapértelmezett). Kattintson a következő ikonra.
6.  Az **alkalmazás tulajdonságai** lapon adja meg a **SIGN-ON URL-CÍMEK** és az **Alkalmazás azonosítója URI** a webes alkalmazáshoz. Ha az alkalmazás nincs ezeket az értékeket, akkor is használhatja őket tagjának ezt a lépést (például megadhatja az mindkettőben http://test1.contoso.com). Nem számít, hogy létezik webhelyek; Mi az fontos, hogy az alkalmazás azonosítója URI az egyes alkalmazások eltér a minden alkalmazás címtárában. A könyvtár a karakterlánc használja az alkalmazás azonosítására.
7.  Kattintson a kész ikonra kattintva mentse a módosításokat a varázslóban.
8.  Első lépések lapon kattintson a **KONFIGURÁLÁS**gombra.
9.  Görgetéssel keresse meg a **billentyűk** szakaszt, jelölje ki az időtartamot, és kattintson a **MENTÉS**gombra. Frissíti a lapot, és megjelenik egy kulcsmező értékét. A kulcs értékét, és az **Ügyfél-azonosító** értéke meg kell adnia az alkalmazást. (Ez a beállítás utasításokat az alkalmazás-specifikus lesznek.)
10. Az ügyfél-azonosító értéket másolja a lapot, amely a következő lépésben használandó engedélyeket állíthat be a tárolóból elemre.




## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Az alkalmazás használatához a billentyűt vagy a titkos engedélyezése

Engedélyezi a billentyűt vagy a tárolóból elemre a titkos eléréséhez az alkalmazás, használja a `azure keyvault set-policy` parancsot.

Ha például a tárolóból elemre neve ContosoKeyVault és a hitelesíteni kívánt alkalmazásnak az 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed egy ügyfél-azonosító, és az alkalmazás fejthető vissza, és jelentkezzen be a tárolóból elemre a billentyűkkel hitelesíteni kívánt, majd futtassa a következő:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-keys '["decrypt","sign"]'

>[AZURE.NOTE] Ha a Windows parancssor futtatja, érdemes aposztrófok cserélje dupla idézőjelek, és a is escape-a belső dupla idézőjelek. Például: "[\"visszafejtése\",\"bejelentkezési\"]".

Ha szeretné engedélyezni, hogy ugyanazt a kérelmet a tárolóból elemre a titkos kulcsok olvasható, futtassa a következő:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-secrets '["get"]'

## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Ha szeretné használni a hardveres biztonsági modult (HSM) ##

A hozzáadott garancia importálhat és kulcsok létrehozásához a hardver biztonsági modulokat (HSMs), amely a HSM oszlopazonosító soha ne hagyja. A HSMs 140-2 szintet 2 érvényesített FIPS. Ha ez a követelmény nem vonatkozik Önre, átugorja ezt a szakaszt, és nyissa meg [a fő tárolóra törlése, valamint a kapcsolódó kulcsok és titkos kulcsok](#delete-the-key-vault-and-associated-keys-and-secrets).

HSM védett billentyűk létrehozásához rendelkeznie, amely támogatja a billentyűparancsok HSM védett előfizetés tárolóból elemre.

A keyvault létrehozásakor adja hozzá a "termékváltozat" paraméter:

    azure azure keyvault create --vault-name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'

Szoftverrel védett (ahogy korábbi) és HSM védett kulcsokról hozzáadhatja a tárolóból elemre. Hozzon létre egy HSM védett kulcsot, állítsa a cél paraméter "HSM":

    azure keyvault key create --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --destination 'HSM'

A következő parancs használatával importálhat egy kulcsot .pem-fájlból a számítógépen. Ez a parancs a kulcs importálása a kulcs tárolóra szolgáltatásban a HSMs:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --destination 'HSM' --password 'PaSSWORD'

A következő parancs importálja a "hozása saját kulcsot" (BYOK) csomagot. Ezzel a radírral a helyi HSM készítése a termékkulcsot, és átviszi HSMs a kulcs tárolóra szolgáltatásban a kulcs az HSM oszlopazonosító elhagyása nélkül:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --destination 'HSM'

A BYOK csomag készítése kapcsolatos részletes útmutatás elolvashatja, [hogyan HSM-Protected billentyűkkel az Azure kulcs tárolóból elemre](key-vault-hsm-protected-keys.md).


## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>A fő tárolóból elemre, és a kapcsolódó kulcsok és a titkos kulcsok törlése

Ha már nincs szüksége, a fő tárolóból elemre, és a kulcs vagy titkos, amely tartalmaz, a fő tárolóból elemre az azure keyvault Törlés paranccsal törölheti:

    azure keyvault delete --vault-name 'ContosoKeyVault'

Vagy törölhet is egy teljes Azure erőforráscsoport, amelyek tartalmazzák még a fő tárolóból elemre, és abba a csoportba más erőforrások:

    azure group delete --name 'ContosoResourceGroup'


## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Más Azure-platformok parancssor parancs

Más lehet hasznos Azure kulcs tárolóra kezelésére szolgáló parancsok.

Ez a parancs megjeleníti a táblázatos megjelenítő kulcsok és a kijelölt tulajdonságok:

    azure keyvault key list --vault-name 'ContosoKeyVault'

Ez a parancs a megadott kulcs tulajdonságainak teljes listáját jeleníti meg:

    azure keyvault key show --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Ez a parancs megjeleníti a táblázatos megjelenítő titkos nevét és a kijelölt tulajdonságok:

    azure keyvault secret list --vault-name 'ContosoKeyVault'

Íme egy példa egy adott kulcs eltávolítása:

    azure keyvault key delete --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Íme egy példa egy adott titkos eltávolítása:

    azure keyvault secret delete --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword'


## <a name="next-steps"></a>Következő lépések

Programozási hivatkozások, [fejlesztői](key-vault-developers-guide.md)útmutatójában található az Azure kulcs tárolóból elemre.

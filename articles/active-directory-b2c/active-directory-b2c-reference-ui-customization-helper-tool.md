<properties
    pageTitle="Azure Active Directory B2C: Lap felhasználói felület testreszabása segítő eszköz |} Microsoft Azure"
    description="A lap felhasználói felület testreszabása szolgáltatás az Azure Active Directory B2C bemutatására helper eszköz"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-a-helper-tool-used-to-demonstrate-the-page-user-interface-ui-customization-feature"></a>Azure Active Directory B2C: Segítő eszköz bemutatják, oldal felhasználói felhasználóifelület-testreszabási szolgáltatás használatával

Ez a cikk a kereső az Azure Active Directory (Azure Active Directory) B2C a [felhasználói felület testreszabása címlapja](active-directory-b2c-reference-ui-customization.md) . A következő lépések bemutatják, hogyan gyakorolni a lap felhasználói felület testreszabása funkció használatával minta HTML- és CSS tartalom biztosítunk Önnek.

## <a name="get-an-azure-ad-b2c-tenant"></a>Ismerkedés az Azure Active Directory B2C bérlői

Testre szabhatja semmit, mielőtt szüksége lesz [az Azure Active Directory B2C bérlői](active-directory-b2c-get-started.md) szeretnénk, ha még nem rendelkezik egy.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Regisztráció vagy bejelentkezés házirend létrehozása

A minta tartalom biztosítunk Önnek a [regisztráció vagy bejelentkezés házirend](active-directory-b2c-reference-policies.md)customze két lapok használható: az [egységesített bejelentkezési lapja](active-directory-b2c-reference-ui-customization.md) és [önálló magas attribútumok lap](active-directory-b2c-reference-ui-customization.md). Ha [a regisztráció vagy bejelentkezés házirend létrehozása](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy), adja hozzá a helyi fiók (e-mail cím), a Facebookon, a Google és a Microsoft mint **Identitásszolgáltatók**. Ezek a csak IDPs, amely a minta HTML-tartalom akkor fogad el.  Ezek a IDPs csak egy részhalmazát is hozzáadhat, ha azt szeretné, hogy.

## <a name="register-an-application"></a>Egy alkalmazás regisztrálása

Szüksége lesz [az alkalmazás](active-directory-b2c-app-registration.md) rögzítése a végrehajtani a házirend használható B2C bérlőhöz. Az alkalmazás a regisztrálás után, amelyeket ténylegesen futtatásához a regisztrációs irányelvek használhat néhány lehetőség áll rendelkezésére:

- Az Azure Active Directory B2C rövid alkalmazások szerepel az "Első lépések" szakasz egyik összeállítása [Jelentkezzen be, és jelentkezzen be az alkalmazások fogyasztói](active-directory-b2c-overview.md#getting-started).
- A beépített [Azure Active Directory B2C játszótéri](https://aadb2cplayground.azurewebsites.net) alkalmazással. Ha úgy dönt, hogy a játszótéri használja, regisztrálnia kell az alkalmazás a **URI átirányítás** segítségével B2C bérlőhöz `https://aadb2cplayground.azurewebsites.net/`.
- Használja a **Futtatás** gombra a házirendet, az [Azure-portálon](https://portal.azure.com/).

## <a name="customize-your-policy"></a>A házirend testreszabása

A Megjelenés és működés a házirend testreszabásához először a az Azure Active Directory B2C az adott szabályok használatával HTML- és CSS-fájlok létrehozásához szükséges. Ezután feltöltheti a statikus tartalommá nyilvánosan elérhető helyre, hogy Azure Active Directory B2C érhető el. Ez lehet saját dedikált webkiszolgáló, Azure Blob-tárolóhoz, Azure tartalom kézbesítési hálózaton, vagy bármely más statikus erőforrás-szolgáltatója. Csak követelményei, hogy a tartalom HTTPS áll rendelkezésre, és CORS használatával is elérhető. Miután a statikus tartalommá a webes már elérhetővé tett, a házirendet, amely mutasson arra a helyre, és az ügyfelekkel, hogy a tartalom előadása szerkesztheti. A [felhasználói felület testreszabása címlapja](active-directory-b2c-reference-ui-customization.md) részletesen ismerteti, hogyan működik a az Azure Active Directory B2C testreszabási szolgáltatás.

Ebben az oktatóanyagban alkalmazásában azt korábban már létrehozott néhány példa tartalom és Azure Blob-tárolóhoz is azt. Minta tartalma rendkívül egyszerű testreszabási az téma címjegyzékemben kitalált vállalat "Dejójáték" lesz. Próbálja ki az egyéni házirend, kövesse az alábbi lépéseket:

1. Jelentkezzen be a bérlő az [Azure-portálra](https://portal.azure.com/) , és nyissa meg azt a B2C szolgáltatások lap.
2. Kattintson a **regisztráció vagy bejelentkezés házirendek** elemre, majd a házirend (például "b2c\_1\_bejelentkezési\_felfelé\_bejelentkezési\_a").
3. Kattintson a **lap felhasználói felület testreszabása** és **egységesített regisztráció vagy bejelentkezés lapon**.
4. Váltás a **használata egyéni lap** Váltás **Igen**. **Egyéni lap URI** mezőjébe írja be `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/unified.html`. Kattintson az **OK gombra**.
5. Kattintson a **helyi fiók előfizetési lap**. Váltás a **Egyéni sablon használata** Váltás **Igen**. **Egyéni lap URI** mezőjébe írja be `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/selfasserted.html`.
5. Ismételje meg a ugyanazt a **közösségi előfizetési fióklapon**.
 Kattintson kétszer az **OK** zárja be a felhasználói felület testreszabása rögzítéséhez.
6. Kattintson a **Mentés**gombra.

Most, próbálja ki a testre szabott irányelvek. Ha azt szeretné, hogy, de a **Futtatás** parancs a házirendet a lap is egyszerűen rákattintva használhatja a saját alkalmazások vagy az Azure Active Directory B2C játszótéri. Jelölje ki az alkalmazást a lefelé mutató nyílra, majd válassza a megfelelő átirányítási URI. Kattintson a **Futtatás** gombra. Egy új böngészőlapon nyílik meg, és a felhasználói felület, az alkalmazás az új tartalom helyben történő feliratkozás segítségével futtathatja!

## <a name="upload-the-sample-content-to-azure-blob-storage"></a>Töltse fel a minta tartalom Azure Blob-tárolóhoz

Azure Blob-tárolóhoz használni a lap tartalmának tárolni szeretné, ha a saját tárterület-fiók létrehozása, és töltse fel fájljait a B2C helper eszközzel.

### <a name="create-a-storage-account"></a>Tárterület-fiók létrehozása

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com/).
2. Kattintson a **+ Új** > **adatok + tárhely** > **tárterület-fiókot**. Szüksége lesz egy Azure-előfizetést Azure Blob-tárolóhoz fiók létrehozásához. Iratkozzon fel az [Azure webhely](https://azure.microsoft.com/pricing/free-trial/)ingyenes próbaverziót.
3. Adjon **nevet** a tárterület-fiókhoz (például "contoso"), és válassza a **réteg árak**, az **erőforráscsoport** és az **előfizetés**a szükséges beállításokat. Győződjön meg arról, hogy a **PIN-kód Startboard** jelölőnégyzet. Kattintson a **létrehozása**gombra.
4. Térjen vissza az Startboard, és kattintson az éppen létrehozott tárolás fiókra.
5. Az **összefoglaló** szakaszában kattintson a **tárolók**, és válassza a **+ Hozzáadás**.
6. Adjon meg egy **nevet** a tároló (például "b2c"), és válassza ki a **Blob** **hozzáférés típusát**. Kattintson az **OK gombra**.
7. Az Ön által létrehozott tárolót a listában a **BLOB** lap fog megjelenni. Jegyezze fel a tároló; az URL-címe Ha például azt hasonlóan kell kinéznie `https://contoso.blob.core.windows.net/b2c`. Zárja be a **BLOB** lap.
8. A tárterület a fiók lap, a **billentyűk** kattintson, és jegyezze fel a **Tárhely fiók nevét** , és az **Elsődleges hívóbetű** mezők értékét.

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com/).
2. Kattintson a **+ Új** > **adatok + tárhely** > **tárterület-fiókot**. Azure Blob-tárolóhoz fiók létrehozása az Azure előfizetéssel szüksége lesz. Iratkozzon fel az [Azure webhely](https://azure.microsoft.com/pricing/free-trial/)ingyenes próbaverziót.
3. **Blob-tárolóhoz** **Fiók típusa**csoportban jelölje be, és hagyja a más értékek alapértelmezettként.  Ha azt szeretné, hogy szerkesztheti a erőforráscsoport és helyét.  Kattintson a **létrehozása**gombra.
4. Térjen vissza az Startboard, és jelölje ki az éppen létrehozott tárterület-fiókját.
5. Az **összefoglaló** szakaszában kattintson a **+ tároló**.
6. Adjon meg egy **nevet** a tároló (például "b2c"), és válassza ki a **Blob** **hozzáférés típusát**. Kattintson az **OK gombra**.
7. Nyissa meg a **Tulajdonságok**tároló, és jegyezze fel a tároló; az URL-címe Ha például azt hasonlóan kell kinéznie `https://contoso.blob.core.windows.net/b2c`. Zárja be a tárolót lap.
8. Kattintson a **Kulcs ikon** a tárterület a fiók lap, és jegyezze fel a **Tárhely fiók nevét** , és az **Elsődleges hívóbetű** mezők.

> [AZURE.NOTE]
    **Elsődleges hívóbetű** egy fontos biztonsági hitelesítő adatait.

### <a name="download-the-helper-tool-and-sample-files"></a>A helper eszköz és a minta fájlok letöltése

Az [Azure Blob-tárolóhoz segítő eszközt, és a minta .zip fájlként fájlok](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip) letöltése, vagy a GitHub klónozhatja azt:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

Ez a tár tartalmaz egy `sample_templates\wingtip` könyvtár, amely tartalmaz, például a HTML, a CSS és a képeket. A sablonok saját Azure Blob-tárolóhoz fiókját hivatkozni szeretne szüksége lesz a HTML-fájlokat szeretne szerkeszteni. Nyissa meg `unified.html` és `selfasserted.html` és előfordulásainak bármely `https://localhost` saját tároló, amelyet az előző lépésekben URL-címét. Abszolút elérési útját a HTML-fájlokat kell használnia, mert a HTML-kód által Azure Active Directory, a tartomány a kiszolgált ebben az esetben `https://login.microsoftonline.com`.

### <a name="upload-the-sample-files"></a>A minta fájlok feltöltése

Az azonos adattárban unzip `B2CAzureStorageClient.zip` és futtatása a `B2CAzureStorageClient.exe` fájl belül. Ez a program egyszerűen a tárterület-fiókjába a címtárban összes fájl feltöltése, és engedélyezze a hozzáférést CORS azokat a fájlokat. Ha követte a fenti lépéseket, a HTML- és CSS-fájlok most is mutat a tárterület-fiókjába. Ne feledje, hogy a tárterület-fiók neve megelőzi rész `blob.core.windows.net`; Ha például `contoso`. Ellenőrizheti, hogy a tartalom feltöltötte megfelelően által elérni kívánt `https://{storage-account-name}.blob.core.windows.net/{container-name}/wingtip/unified.html` a böngészőben. Győződjön meg arról, hogy a tartalom már engedélyezni CORS is használhatja [http://test-cors.org/](http://test-cors.org/) . (Keressen "XHR állapot: 200" az eredményben.)

### <a name="customize-your-policy-again"></a>A házirend, újra testreszabása

Most, hogy az Ön által feltöltött a minta tartalom a saját tárterület-fiókjába, módosítania kell a regisztrációs házirendet, amely hivatkoznak rá. Ismételje meg a fenti ["Testreszabása a házirend"](#customize-your-policy) szakaszából Ez esetben a saját tároló fiók URL-címek használata. Helyét, például a `unified.html` fájl lenne `<url-of-your-container>/wingtip/unified.html`.

Most is használhatja a **Futtatás** gombra, vagy a saját alkalmazások újra végrehajtani a házirend. Az eredményt kell kinéznie majdnem pontosan azonos – akkor használja a HTML- és CSS az azonos mintában mindkét esetben. Azonban a házirendek most hivatkozik Azure Blob-tárolóhoz saját példányát, és a szabad, szerkesztéséhez, és újra közben a fájlok feltöltése a adja. A HTML- és CSS testreszabásáról további tudnivalókért tekintse át a [felhasználói felület testreszabása címlapja](active-directory-b2c-reference-ui-customization.md).

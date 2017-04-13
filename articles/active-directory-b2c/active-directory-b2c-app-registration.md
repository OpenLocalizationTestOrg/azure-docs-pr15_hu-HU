<properties
    pageTitle="Azure Active Directory B2C: Alkalmazás regisztrációs |} Microsoft Azure"
    description="Hogyan lehet rögzíteni az alkalmazás az Azure Active Directory B2C"
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
    ms.topic="get-started-article"
    ms.date="08/30/2016"
    ms.author="swkrish"/>


# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory B2C: Az alkalmazás regisztrálása

## <a name="prerequisite"></a>Előfeltételek

Fogadja el a fogyasztói előfizetési és a bejelentkezési alkalmazás létrehozásához először kell az alkalmazás regisztrálása az Azure Active Directory B2C bérlői. A saját bérlői Ismerkedés [az Azure Active Directory B2C bérlő létrehozása](active-directory-b2c-get-started.md)című témakörben ismertetett lépésekkel. A cikkben a lépéseket a beállítás után a B2C szolgáltatások lap a kiemelt a Startboard lesz.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="navigate-to-the-b2c-features-blade"></a>Nyissa meg azt a B2C szolgáltatások lap

Ha a B2C szolgáltatások lap a kiemelt a Startboard, látni fogja a lap, amint, jelentkezzen be az [Azure portálra](https://portal.azure.com/) a a B2C bérlő globális rendszergazdaként.

Kattintson a **Tallózás gombra** , majd az **Azure Active Directory B2C** a bal oldali navigációs ablaktáblában az [Azure portálon](https://portal.azure.com/)a lap is elérheti.

> [AZURE.IMPORTANT] A globális rendszergazdája, a B2C bérlőjének fér hozzá a B2C szolgáltatások lap szüksége. A többi bérlői webhelyen globális rendszergazdának vagy bármely bérlői a felhasználó nem tud hozzáférni azt.  Az Azure portál jobb felső sarkában a bérlői kapcsoló használatával válthat az B2C bérlőhöz.

## <a name="register-an-application"></a>Az alkalmazások regisztrálhatja

1. Az Azure portál B2C szolgáltatások lap kattintson az **alkalmazások**parancsra.
2. Válassza a **+ Hozzáadás** a lap tetején.
3. Írja be egy **nevet** az alkalmazás, amely jellemzi vonzóbbak lehetnek az alkalmazást. Ha például "Contoso B2C alkalmazás" sikerült írja be.
4. Ha az éppen írt egy webes alkalmazás, állítsa be, a **vehet fel a web App alkalmazásban, illetve a webes API** Váltás **Igen**. A **Válasz URL-címek** végpontok, ahol Azure Active Directory B2C ad vissza, amely kéri az alkalmazás bármely tokenek. Ha például írja be `https://localhost:44321/`. Ha a webalkalmazásban is fog hívásához néhány webes API védi az Azure Active Directory B2C, célszerű, valamint hozzon létre egy **Alkalmazás titkos** **Kulcs generálása** gombra kattintva.

    > [AZURE.NOTE] Az **Alkalmazás titkos** egy fontos biztonsági hitelesítő adatait, és megfelelően titkosítani kell.

5. Ha az éppen írt egy mobilalkalmazás, állítsa be, a **Felvétel natív ügyfele** Váltás **Igen**. Másolja az alapértelmezett **URI irányítja** , amely automatikusan létrejön lefelé.
6. Kattintson a **Létrehozás** regisztrálhatja az alkalmazást.
7. Kattintson az imént létrehozott alkalmazásra, és másolja le a globálisan egyedi **Ügyfél azonosítója** belül a kódot kell megadnia.

> [AZURE.IMPORTANT] A B2C szolgáltatások lap készült alkalmazások kell kezelni az adott helyen. Amennyiben B2C szerkesztése PowerShell alrendszerrel vagy egy másik portálon alkalmazások nem támogatott lesz, és valószínűleg nem működnek a Azure Active Directory B2C.

## <a name="build-a-quick-start-application"></a>A rövid útmutató az alkalmazás összeállítása

Most, hogy az Azure Active Directory B2C regisztrált alkalmazások, végezhetik el egy saját rövid oktatóanyagok a kezdeti lépéseket. Íme néhány javaslatok:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]

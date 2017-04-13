<properties
    pageTitle="Azure Active Directory authentication az alkalmazás Services alkalmazás beállítása"
    description="Megtudhatja, hogy miként állítsa be az alkalmazás Services alkalmazás Azure Active Directory-hitelesítést."
    authors="mattchenderson"
    services="app-service"
    documentationCenter=""
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="how-to-configure-your-app-service-application-to-use-azure-active-directory-login"></a>Az alkalmazás szolgáltatásalkalmazás használatához az Azure Active Directory bejelentkezési konfigurálása

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Ez a témakör bemutatja, hogyan használhatja az Azure Active Directory-hitelesítés konferenciaszolgáltatóként Azure alkalmazás Services konfigurálása.

## <a name="express"> </a>Konfigurálása Azure Active Directory express beállításokkal

13. Az [Azure portálon]nyissa meg az alkalmazást. Kattintson a **beállítások ikonra**, majd a **Hitelesítési/engedélyt**.

14. Ha a hitelesítés / engedélyezési funkció nincs engedélyezve, kapcsolja be a Váltás **a**.

15. Kattintson az **Azure Active Directory**, és kattintson **Express-** **Kezelési módja**mezőben.

16. Kattintson az **OK gombra** az alkalmazás regisztrálhatja az Azure Active Directory. Ez a hoz létre egy új regisztráció. Ha azt szeretné, válassza inkább egy meglévő bejegyzés, kattintson a **meglévő alkalmazás kijelölése** gombra, és keresse meg a nevét a bérlő belül egy korábban létrehozott regisztráció.
Kattintson a regisztráció kijelölheti, és kattintson az **OK gombra**. Kattintson az Azure Active Directory-beállítások lap kattintson **az OK** gombra.

    ![][0]

    Alapértelmezés szerint az alkalmazás szolgáltatás hitelesítésről, de nem engedélyezett hozzáférés korlátozása a webhelytartalom és az API-hoz. Alkalmazás kódban engedélyeznie kell a felhasználókat.

17. (Nem kötelező) Elérésének korlátozására csak az Azure Active Directory hitelesített felhasználók a webhelyen, állítsa a **műveleteket, ha nem lehet kérelem hitelesíteni** **Jelentkezzen be az Azure Active Directory**. Ehhez az összes kérelmet hitelesíteni, és az összes hitelesített kérés megnyílik a Azure Active Directory-hitelesítést.

17. Kattintson a **Mentés**gombra.

Most már készen áll az alkalmazásban hitelesítéshez használt az Azure Active Directory.

## <a name="advanced"> </a>(Alternatív módszer) manuálisan állítsa be az Azure Active Directory speciális beállításokkal
Úgy is dönt, hogy a szükséges konfigurációs beállítások manuális. Ha a használni kívánt AAD bérlő eltér a bérlő, amellyel jelentkezzen be az Azure az az elsődleges megoldást. Végezze el a konfigurálását, létre kell hoznia először regisztrációkor az Azure Active Directory, és ezután meg kell adnia a regisztráció részletei részét alkalmazás szolgáltatásba.

### <a name="register"> </a>Az alkalmazás regisztrálása az Azure Active Directory címtárral

1. Jelentkezzen be az [Azure-portálra], és nyissa meg az alkalmazást. Másolja a vágólapra az **URL-CÍMÉT**. Az Azure Active Directory-alkalmazás konfigurálása használandó.

3. Jelentkezzen be az [Azure klasszikus portálra] , és kattintson az **Active Directory**.

    ![][2]

4. Jelölje ki a címtárában, és válassza az **alkalmazások** fülre a képernyő tetején. Kattintson az alsó hozzon létre egy alkalmazás új bejegyzés **hozzáadása** gombra.

5. Kattintson a **szervezeten elkészítésének az alkalmazás hozzáadása**gombra.

6. Az alkalmazás felvétele varázslóban írja be egy **nevet** az alkalmazás, és válassza ki a **Webes API Web Application Programming és/vagy** . Kattintson a folytatáshoz.

7. A **SIGN-ON URL-címe** mezőbe illessze be az alkalmazás előbb másolt URL-címet. Írja be, hogy egy URL-CÍMÉT az **Alkalmazás azonosítója URI** mezőbe. Kattintson a folytatáshoz.

8. Az alkalmazás hozzáadása után kattintson a **beállítás** fülre. Szerkessze a **Válasz URL-CÍMÉT** a **Single Sign-on** az alkalmazást, az elérési utat, _/.auth/login/aad/callback_fűzött URL-címe lesz. Ha például `https://contoso.azurewebsites.net/.auth/login/aad/callback`. Győződjön meg arról, hogy a HTTPS sorrendben kell használnia.

    ![][3]

9. Kattintson a **Mentés**gombra. Az alkalmazás az **Ügyfél-azonosító** Ezután másolja. Az alkalmazás későbbi felhasználás céljából fog konfigurálni.

10. Alsó parancssávon kattintson a **Nézet végpontok pontra**, majd az **Összevonási metaadat-dokumentum** URL-cím másolása és, hogy a dokumentum letöltése vagy a böngészőben nyissa meg.

11. A legfelső szintű **EntityDescriptor** elemben kell lennie az űrlap **entityid beállítást** attribútum `https://sts.windows.net/` egy adott globálisan egyedi azonosítója követi az Ön bérlői (más néven "bérlői Azonosítót"). Ez az érték másolása – azt a **Kibocsátó URL-címe**lesz. Az alkalmazás későbbi felhasználás céljából fog konfigurálni.

### <a name="secrets"> </a>Az alkalmazás hozzáadása Azure Active Directory-információk

13. Vissza az [Azure portálon]nyissa meg az alkalmazást. Kattintson a **beállítások ikonra**, majd a **Hitelesítési/engedélyt**.

14. Ha a hitelesítési/engedélyezési szolgáltatás nincs engedélyezve, kapcsolja be a Váltás **a**.

15. Kattintson az **Azure Active Directory**, és kattintson a **Speciális** **Adatkezelési módja**mezőben. Illessze be az ügyfél-azonosító és a kibocsátó URL-címe értéket, amely a korábban szerezte be. Kattintson **az OK**gombra.

    ![][1]

    Alapértelmezés szerint az alkalmazás szolgáltatás hitelesítésről, de nem engedélyezett hozzáférés korlátozása a webhelytartalom és az API-hoz. Alkalmazás kódban engedélyeznie kell a felhasználókat.

17. (Nem kötelező) Elérésének korlátozására csak az Azure Active Directory hitelesített felhasználók a webhelyen, állítsa a **műveleteket, ha nem lehet kérelem hitelesíteni** **Jelentkezzen be az Azure Active Directory**. Ehhez az összes kérelmet hitelesíteni, és az összes hitelesített kérés megnyílik a Azure Active Directory-hitelesítést.

17. Kattintson a **Mentés**gombra.

Most már készen áll az alkalmazásban hitelesítéshez használt az Azure Active Directory.

## <a name="optional-configure-a-native-client-application"></a>(Nem kötelező) Natív ügyfele konfigurálása

Azure Active Directory is lehetővé teszi regisztrálása natív ügyfelek, amely biztosítja jobban kézben tarthatók az engedélyek hozzárendelése. Ez szükséges, ha egy tárba, például az **Active Directory Authentication Library**használatával bejelentkezések végrehajtandó.

1. Nyissa meg **Az Active Directory** az [Azure klasszikus portálon].

2. Jelölje ki a címtárában, és válassza az **alkalmazások** fülre a képernyő tetején. Kattintson az alsó hozzon létre egy alkalmazás új bejegyzés **hozzáadása** gombra.

3. Kattintson a **szervezeten elkészítésének az alkalmazás hozzáadása**gombra.

4. Az alkalmazás felvétele varázslóban írja be egy **nevet** az alkalmazás, és válassza ki a **Natív ügyfélalkalmazás** . Kattintson a folytatáshoz.

5. **Átirányítás URI** mezőbe írja be a webhely _/.auth/login/done_ végpontot, a HTTPS rendszert használ. Ezt az értéket kell _https://contoso.azurewebsites.net/.auth/login/done_hasonló. Windows-alkalmazás létrehozása, ha inkább a [csomag biztonsági AZONOSÍTÓK](app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) használja a URI.

6. Miután hozzáadta az eredeti alkalmazást, kattintson a **beállítás** fülre. Keresse meg az **Ügyfél-azonosító** , és jegyezze le ezt az értéket.

7. Görgessen lefelé az **engedélyek egyéb alkalmazások** csoportban a lapot, és kattintson az **alkalmazás hozzáadása**parancsra.

8. Keresse meg a webes alkalmazás, amely a korábban regisztrálta, és kattintson a pluszjel ikonra. Ezután kattintson a gombra a párbeszédpanel bezárásához. Ha a webes alkalmazás nem található, nyissa meg azt a regisztráció, és adja hozzá az új válasz URL-címet (például a HTTP verziója az aktuális URL-CÍMÉT), kattintson a Mentés gombra, és ismételje meg ezeket a lépéseket – az alkalmazás jelenjen meg a listában.

9. Az új bejegyzés, amelyet nemrég hozzáadott, nyissa meg a **Meghatalmazott engedélyeit** tartalmazó legördülő listára, és jelölje be a **hozzáférés (alkalmazásnév)**. Ezután kattintson a **Mentés**gombra.

Most egy natív ügyfele alkalmazást, amely férhet hozzá az alkalmazás szolgáltatásalkalmazás úgy beállítva.

## <a name="related-content"> </a>Kapcsolódó tartalom

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-express-settings.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-advanced-settings.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-navigate-aad.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-configure.png

<!-- URLs. -->

[Azure portál]: https://portal.azure.com/
[Azure klasszikus portál]: https://manage.windowsazure.com/
[alternative method]:#advanced

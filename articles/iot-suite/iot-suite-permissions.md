<properties
  pageTitle="Azure IoT csomagja és Azure Active Directory |} Microsoft Azure"
  description="Ismerteti, hogyan Azure IoT csomagot használ Azure Active Directory jogosultságok kezelése."
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="10/24/2016"
  ms.author="araguila"/>
  
# <a name="permissions-on-the-azureiotsuitecom-site"></a>A azureiotsuite.com webhely engedélyei

## <a name="what-happens-when-you-sign-in"></a>Mi történik, amikor bejelentkezik

Amikor először bejelentkezik [azureiotsuite.com][lnk-azureiotsuite], a hely határozza meg, hogy a jogosultsági szintek van a kijelölt Azure Active Directory (AAD) bérlői és Azure előfizetés alapján.

1.  A webhely első szerez az Azure mely AAD bérlők, amelyeknek tagja annak érdekében, hogy a bérlők bejelentkezett felhasználónevét mellett látható a lista feltöltéséhez. Ekkor a webhely csak szerezhet be felhasználói tokenek egy bérlői webhelyen egyszerre. Eredményt adja amikor jobb felső sarokban a legördülő lista használata különböző bérlői webhelyre vált, a webhely újra bejelentkezik nyissa meg a beszerzése a tokenek azt az adott bérlői.

2.  Ezután a webhely szerez az Azure előfizetésekről társított a kijelölt bérlő. Megjelenik a rendelkezésre álló előfizetéseket, amikor egy új előre beállított megoldást hoz létre.

3.  Végül a webhely átveszi mind az erőforrások az előfizetések és erőforrás-csoportok, előre definiált megoldások címkézett és feltölti a Kezdőlap lapon a mozaikokat.

Az alábbi szakaszok ismertetik a szerepkörök, a hozzáférés vezérléséhez az előre definiált megoldások.

## <a name="aad-roles"></a>AAD szerepkörök

A AAD szerepkörök képes előre rendelkezést megoldásokkal szabályozhatja, és megoldást előre beállított felhasználók kezelése.

Rendszergazdai szerepkörök további információt a [rendszergazdai]szerepkörök hozzárendelése az Azure Active Directory AAD található[lnk-aad-admin], de ez a cikk koncentrál elsősorban a **Globális rendszergazda** és a **Tartomány felhasználói/tagsági** szerepkör alapján, mint az előre definiált megoldások által használt.

**Globális rendszergazda:** Sok globális rendszergazda AAD bérlőnként lehet. Amikor létrehoz egy AAD-ös bérlői webhelyet, akkor alapértelmezés szerint ki legyenek a globális rendszergazdai az adott bérlői. A globális rendszergazda is kiépítése előre beállított megoldást, és az alkalmazás belül a AAD bérlői **rendszergazdai** szerepkör társíthat. Ha egy másik felhasználó ugyanahhoz a bérlőhöz AAD alkalmazást hoz létre, az alapértelmezett szerepkör a globális rendszergazda adják azonban **IMPLICIT OLVASNI csak**. Globális rendszergazda kioszthatja a szerepköröket az [Azure klasszikus portál]alkalmazások[lnk-classic-portal].

**Tartomány felhasználói/tagja:** Sok tartomány felhasználói/tagjainak AAD bérlőnként lehet. A tartomány felhasználó a [azureiotsuite.com] előre beállított megoldást is kiépítése[ lnk-azureiotsuite] webhelyet. Az alapértelmezett kapnak, hogy az alkalmazás azok kiépítése feladata **rendszergazda**. Az alkalmazás a build.cmd parancsfájl [azure-iot-távoli-ellenőrző] hozhatnak létre[ lnk-rm-github-repo] vagy [azure-iot-prediktív-karbantartási] [ lnk-pm-github-repo] tárházba, de az alapértelmezett szerepkör kapnak megegyezik **IMPLICIT csak OLVASHATÓ**, nincs engedélye szerepköröket rendelhet hozzájuk. Ha egy másik felhasználó AAD bérlőhöz alkalmazást hoz létre, azokat vehetnek **IMPLICIT csak OLVASHATÓ** alapértelmezés szerint az alkalmazás. Nincs lehetősége az alkalmazások; szerepköröket rendelhet hozzájuk így nem vehet fel felhasználók és szerepkörök a felhasználóknak a kérelmet, ha az azok kiépítéstől azt.

**Vendég felhasználói/Vendég:** Sok vendég felhasználók/vendégek AAD bérlőnként lehet. A vendégként való bekapcsolódáshoz felhasználójánál tartalomvédelmi korlátozott számú AAD bérlőhöz. A vendégként való bekapcsolódáshoz felhasználók eredményt adja, nem kiépítése AAD bérlőhöz előre beállított megoldást.

További információt az alábbi forrásokban talál:

- [Felhasználók létrehozása és szerkesztése az Azure Active Directory][lnk-create-edit-users]
- [A AAD alkalmazás szerepköröket rendelhet hozzájuk][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Azure előfizetés rendszergazdai szerepkörök

Az Azure rendszergazdai szerepkörök szabályozhatja, hogy milyen az Azure előfizetéssel hozzárendelése egy Active Directory-ös bérlői webhelyet.

További tudnivalók az Azure közös rendszergazda, a szolgáltatás rendszergazdája és a fiók rendszergazdai szerepkörök [megadásához vagy közös Azure-rendszergazda, a szolgáltatás rendszergazdája és a rendszergazdai fiók módosítása]című témakörben talál[lnk-admin-roles].

## <a name="application-roles"></a>Alkalmazás-szerepkörök

A szerepkörökhöz az előre beállított megoldásban eszközökre való hozzáférés szabályozása.

Vannak két definiált és az alkalmazás telepítésekor, előre beállított megoldás kiépítése létrehozott definiált egy implicit szerepkört.

-   **Rendszergazda:** Adja hozzá, kezelése és távolítsa el az eszközt teljes hozzáféréssel rendelkezik

-   **Csak OLVASHATÓ:** Van eszközök megjelenítése

-   **IMPLICIT OLVASÁSI csak:** Ugyanaz, mint a csak olvasható, de a AAD bérlői webhely összes felhasználójának adják. Ez végezték kényelmesebbé fejlesztés alatt. Eltávolíthatja a szerepkör a [RolePermissions.cs] módosításával[ lnk-resource-cs] forrásfájl.

### <a name="changing-application-roles-for-a-user"></a>A felhasználó alkalmazás szerepköreinek módosítása

A következő eljárással rendszergazdává egy felhasználó az Active Directoryban a előre beállított megoldás.

A felhasználó szerepkörök módosítása egy AAD globális rendszergazdának kell lennie:

1. Nyissa meg az [Azure klasszikus portál][lnk-classic-portal].

2. Jelölje ki **az Active Directory**.

3. Kattintson a nevére a AAD bérlő (Ez az a könyvtár választotta a azureiotsuite.com, a megoldás kiépítése).

4. Kattintson az **alkalmazások**elemre.

5. Kattintson a nevére, az alkalmazás, amely az előre beállított megoldás. Ha nem látja a listában az alkalmazást, váltson a **Megjelenítés** legördülő **vállalaton tulajdonosa alkalmazások** le, és kattintson a pipára.

7. Kattintson a **felhasználók**elemre.

8. Jelölje ki a felhasználót, amelyre váltani szerepkörök.

9. Kattintson a **hozzárendelni** , és válassza ki azt a szerepkört (például **felügyeleti**) szeretne hozzárendelése a felhasználókhoz, kattintson a pipára.

## <a name="faq"></a>GYAKORI KÉRDÉSEK

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-do-this"></a>Szolgáltatás-rendszergazda vagyok, és szeretném módosítani az előfizetést, és egy adott AAD bérlői közötti címtár hozzárendelést. Hogyan tegye meg?

1. Nyissa meg az [Azure klasszikus portál][lnk-classic-portal], a szolgáltatások, a bal oldali listában kattintson a **Beállítások** gombra.

2. Jelölje ki az előfizetés a címtár-hozzárendelés módosítani szeretné.

3. Kattintson a **Szerkesztés címtár**gombra.

4. Jelölje ki a **címtár** szeretné, hogy a legördülő lista használata. Kattintson a továbbítás nyílra.

5. Erősítse meg a könyvtár leképezést, és további rendszergazdák érintett. Figyelje meg, hogy egy másik címtárból helyez át, ha az eredeti címtárból minden további rendszergazdák törlődnek.

### <a name="im-a-domain-usermember-on-the-aad-tenant-and-ive-created-a-preconfigured-solution-how-do-i-get-assigned-a-role-for-my-application"></a>A tartomány felhasználói/tagja a AAD bérlői vagyok, és lehet létrehozott egy előre konfigurált megoldás. Hogyan tegye első kiosztott szerepkör az alkalmazás?

Kérje meg a globális rendszergazdának hozzá szeretné rendelni, a AAD bérlőjének szerezhetek engedélyt szerepkörök hozzárendelése felhasználók saját maga a globális rendszergazdaként, vagy kérje meg a globális rendszergazdának, rendeljen egy szerepkört. Ha szeretné módosítani a AAD bérlő a előre beállított megoldás hogy, a következő kérdés lett telepítve.

### <a name="how-do-i-switch-the-aad-tenant-my-remote-monitoring-preconfigured-solution-and-application-are-assigned-to"></a>Hogyan lehet váltani a AAD bérlő a távoli felügyeleti előre definiált megoldások és az alkalmazás vannak rendelve?

Egy felhőalapú környezetben lebonyolítása <https://github.com/Azure/azure-iot-remote-monitoring> , és telepítsen újra a AAD újonnan létrehozott bérlői webhelyet. Mivel az Ön globális rendszergazdának alapértelmezés szerint új AAD bérlői webhely létrehozásakor, access-felhasználók felvétele és szerepkörök hozzárendelése azoknak a felhasználóknak lesz.

1. Új AAD könyvtár létrehozása az [Azure kezelőportálja][lnk-classic-portal].

2. Nyissa meg a <https://github.com/Azure/azure-iot-remote-monitoring>.

3. Futtatása `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` (például `build.cmd cloud debug myRMSolution`)

4. Amikor a rendszer kéri, adja meg az újonnan létrehozott bérlői webhelyen helyett az előző bérlői webhelyen kell a **tenantid** .


### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organisational-account"></a>Szeretném módosítani a szolgáltatás vagy közös rendszergazdája, ha egy szervezeti fiókkal jelentkezik

[Szolgáltatás-rendszergazda módosítása és közös rendszergazda, amikor egy szervezeti fiókkal bejelentkezett]cikk[lnk-service-admins].

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Miért jelenik meg ez a hiba? "A fiókja nem rendelkezik a megfelelő engedélyekkel megoldás létrehozása. Ellenőrizze a fiók rendszergazda vagy próbálkozzon egy másik fiókkal."

Ajánljuk figyelmébe az alábbi ábrán:

![][img-flowchart]

> [AZURE.NOTE] Ha továbbra is megjelenik a hiba akkor ellenőrzése után a AAD bérlői globális rendszergazdának és a közös rendszergazdája az előfizetéshez, kérje meg a fiók rendszergazdát a felhasználó eltávolítása és újbóli hozzárendelése a szükséges engedélyekkel, a következő sorrendben: a felhasználó hozzáadása a globális rendszergazdaként, és vegye fel az Azure előfizetéshez közös rendszergazdái felhasználói. Ha problémák továbbra is fennáll, lépjen kapcsolatba a [Súgó és támogatás][lnk-help-support].

**Miért jelenik meg ezt a hibát, ha van egy Azure-előfizetést?** *Előre konfigurált megoldások létrehozása az Azure előfizetéssel szükséges. Csak néhány percig is létrehozhat egy ingyenes próba-fiókkal.*

Ha biztos benne, Azure szóló előfizetés, ellenőrzése a bérlő leképezése az előfizetéshez, és ellenőrizze, hogy a helyes bérlő be van-e jelölve a legördülő menü a. Ha már a érvényesített a kívánt bérlői helyes, kövesse a fenti diagramot és az előfizetés, és a AAD bérlői hozzárendelése ellenőrzése.

## <a name="next-steps"></a>Következő lépések

Tanulási kapcsolatos IoT csomagja folytatásához látható, hogyan dolgozhat [előre beállított megoldás testreszabása][lnk-customize].

[img-flowchart]: media/iot-suite-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-aad-admin]: https://azure.microsoft.com/documentation/articles/active-directory-assign-admin-roles/
[lnk-classic-portal]: https://manage.windowsazure.com/
[lnk-create-edit-users]: https://azure.microsoft.com/documentation/articles/active-directory-create-users/
[lnk-assign-app-roles]: https://azure.microsoft.com/documentation/articles/active-directory-application-manifest/
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: https://azure.microsoft.com/documentation/articles/billing-add-change-azure-subscription-administrator/
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md

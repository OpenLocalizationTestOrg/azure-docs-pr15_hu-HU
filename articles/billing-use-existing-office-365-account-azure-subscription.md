<properties
    pageTitle="Egyetlen megosztása az Office 365-ben és az Azure-előfizetésekben Azure AD-bérlő |} Microsoft Azure"
    description="Ismerje meg, hogy miként oszthatja meg az Office 365 Azure AD-bérlő és a felhasználókat az Azure-előfizetéséhez, vagy fordítva"
    services=""
    documentationCenter=""
    authors="JiangChen79"
    manager="mbaldwin"
    editor=""
    tags="billing,top-support-issue"/>

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="cjiang"/>

# <a name="use-an-existing-office-365-account-with-your-azure-subscription-or-vice-versa"></a>Egy meglévő Office 365-fiókot az Azure-előfizetéséhez, vagy fordítva használata
Alkalmazási helyzetek: Már rendelkezik Office 365-előfizetést, és készen áll az Azure előfizetéssel, de a meglévő Office 365 felhasználói fiókok használata Azure előfizetéshez tartozó szeretne. Másik lehetőségként az Azure előfizetői és szeretné, hogy a felhasználók az Office 365-előfizetéssel a meglévő Azure Active Directory-ban. Ebből a cikkből megtudhatja, milyen könnyen mindkét eléréséhez.

> [AZURE.NOTE] Ez a cikk nem vonatkozik, a nagyvállalati szerződés (EA) ügyfelek. Ha további segítségre van bármely pontján ebben a cikkben [Kapcsolatfelvétel az ügyfélszolgálattal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) veheti a probléma megoldódott gyorsan.


## <a name="quick-guidance"></a>Rövid útmutató

- Ha már rendelkezik Office 365-előfizetéssel és Azure regisztrálhat szeretne, adja meg a **Bejelentkezés a szervezeti fiókkal** beállítást. Folytassa a Azure regisztráció az Office 365-ös fiókját. Lásd [a jelen cikk részletes lépéseket](#s1).

- Ha már rendelkezik egy Azure-előfizetést, és szeretné, hogy az Office 365-előfizetéssel, jelentkezzen be az Office 365-az Azure-fiók. Ezután folytassa az regisztrálásának lépéseit. Elvégzése után a regisztráció az Office 365-előfizetés bekerül az azonos Azure Active Directory-példány, amelyhez az Azure-előfizetése tartozik. További tudnivalókért lásd: a szakasz [részletes lépéseket a jelen cikk](#s2).

>[AZURE.NOTE] Úgy juthat az Office 365-előfizetéssel, a fiók-et előfizetési az Azure Active Directory-ös bérlőben globális vagy számlázási rendszergazdai szerepkör tagjának kell lennie. [Útmutató: a szerepkör az Azure Active Directory határozza meg](#how-to-know-your-role-in-your-azure-active-directory).

Megértéséhez, hogy mi történik, ha az előfizetés ad hozzá egy fiókot, olvassa el a [háttér-információk a cikk későbbi részében](#background-information)című témakört.

## <a name="detailed-steps"></a>A lépések részletes leírását
<a id="s1"></a>
### <a name="scenario-1-office-365-users-who-plan-to-buy-azure"></a>Alkalmazási helyzetek 1: Office 365-felhasználók, akik megtervezése vásárlása Azure
Ebben az esetben feltételezzük Kelley fal a felhasználó, aki az Office 365-előfizetéssel rendelkezik, és azt tervezi, hogy az Azure előfizetés. Két további aktív felhasználók vannak, gipsz és Anna. Kelley a fiók admin@contoso.onmicrosoft.com.

![Az Office 365 felhasználói felügyeleti központ](./media/billing-use-existing-office-365-account-azure-subscription/1-office365-users-admin-center.png)

Regisztrálni szeretne az Azure, kövesse az alábbi lépéseket:

1. Regisztráció az Azure [Azure.com](https://azure.microsoft.com/)elemre. Kattintson a **próbálja ki az ingyenes**. A következő lapon kattintson **az Indítás most**gombra.

    ![Próbálja ki az ingyenes Azure.](./media/billing-use-existing-office-365-account-azure-subscription/2-azure-signup-try-free.png)

2. Kattintson a **Bejelentkezés a szervezeti fiókkal**.

    ![Jelentkezzen be az Azure.](./media/billing-use-existing-office-365-account-azure-subscription/3-sign-in-to-azure.png)

3. Jelentkezzen be az Office 365-fiókjába. Ebben az esetben célszerű Kelley meg Office 365-fiókjába.

    ![Jelentkezzen be az Office 365-fiókjába.](./media/billing-use-existing-office-365-account-azure-subscription/4-sign-in-with-org-account.png)

4. Írja be az adatokat, és a regisztrációs folyamat befejezéséhez.

    ![Töltse ki az adatokat, és fejezze be a regisztráció.](./media/billing-use-existing-office-365-account-azure-subscription/5-azure-sign-up-fill-information.png)

    ![Kattintson a Start gombra, a szolgáltatás kezelésére.](./media/billing-use-existing-office-365-account-azure-subscription/6-azure-start-managing-my-service.png)

Most már nem kell tennie semmit. Az Azure-portálon látnia kell a kapott ugyanazokat a felhasználókat. Ennek ellenőrzéséhez kövesse az alábbi lépéseket:

1. Kattintson a **elkezdheti kezelni a szolgáltatást** a korábban képernyőt.
2. Kattintson a **Tallózás gombra**, és válassza az **Active Directory**.

    ![Kattintson a Tallózás gombra, és válassza az Active Directory.](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)

3. Kattintson a **felhasználók**elemre.

    ![A felhasználók lapon](./media/billing-use-existing-office-365-account-azure-subscription/8-azure-portal-ad-users-tab.png)

4. Minden felhasználó, beleértve a Kelley, jelennek meg a várt módon.

    ![Felhasználók listája](./media/billing-use-existing-office-365-account-azure-subscription/9-azure-portal-ad-users.png)

<a id="s2"></a>
### <a name="scenario-2-azure-users-who-plan-to-buy-office-365"></a>2 alkalmazási helyzetek: Azure a felhasználók, akik tervezi, hogy az Office 365 megvásárlása

Ebben az esetben Kelley fal a fiók csoportjában az Azure előfizetéssel rendelkező felhasználó admin@contoso.onmicrosoft.com. Kelley az Office 365-előfizetése, és ugyanabban a könyvtárban kikkel már az Azure használni szeretne.

>[AZURE.NOTE] Úgy juthat az Office 365-előfizetéssel, a használt bejelentkezési fiók az Azure Active Directory-ös bérlőben globális vagy számlázási rendszergazdai szerepkör tagjának kell lennie. [Megtudhatja, hogyan állapítható meg a szerepkör az Azure Active Directory](#how-to-know-your-role-in-your-azure-active-directory).

![Azure portál előfizetési beállítások](./media/billing-use-existing-office-365-account-azure-subscription/10-azure-portal-settings-subscription.png)

![Azure portál Active Directory-felhasználók](./media/billing-use-existing-office-365-account-azure-subscription/11-azure-portal-ads-users.png)

Az Office 365-előfizetéséhez, kövesse az alábbi lépéseket:

1. Nyissa meg az [Office 365-termék lapját](https://products.office.com/business), és válassza a megfelelő meg egy másik csomagra.
2. Miután kiválasztotta a csomagot, a megjelenő lap jelenik meg. Töltse ki az űrlapot. Kattintson a lap jobb felső sarkában kattintson a **Bejelentkezés** gombra.

    ![Az Office 365 próba lap](./media/billing-use-existing-office-365-account-azure-subscription/12-office-365-trial-page.png)

3. Jelentkezzen be a fiók hitelesítő adatait. Ebben a példában az Kelley a fiókot.

    ![Bejelentkezés az Office 365-ben](./media/billing-use-existing-office-365-account-azure-subscription/13-office-365-sign-in.png)

4. Kattintson a **próbálja ki most**gombra.

    ![Erősítse meg a megrendelés az Office 365-höz.](./media/billing-use-existing-office-365-account-azure-subscription/14-office-365-confirm-your-order.png)

5. A sorrend visszaigazolás lapon kattintson a **Folytatás**gombra.

    ![Az Office 365 sorrendben visszaigazolás](./media/billing-use-existing-office-365-account-azure-subscription/15-office-365-order-receipt.png)

Most már nem kell tennie semmit. Az Office 365 felügyeleti központ látnia kell a Contoso címtárból megjelennek, de az aktív felhasználók felhasználók. Ennek ellenőrzéséhez kövesse az alábbi lépéseket:

1. Nyissa meg az Office 365 felügyeleti központot.
2. Bontsa ki a **FELHASZNÁLÓKAT**, és válassza az **Aktív felhasználók**.

    ![Az Office 365 felügyeleti központ felhasználók](./media/billing-use-existing-office-365-account-azure-subscription/16-office-365-admin-center-users.png)

### <a name="how-to-know-your-role-in-your-azure-active-directory"></a>Hogyan állapítható meg az Azure Active Directory szerepköre

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com/).
2. Kattintson a **Tallózás gombra**, és válassza az **Active Directory**.

    ![Az Active Directory az Azure-portálon](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)

3. Kattintson a **felhasználók**elemre.

    ![Portál alapértelmezett Azure Active Directory-felhasználók](./media/billing-use-existing-office-365-account-azure-subscription/17-azure-portal-default-ad-users.png)

4. Kattintson arra a felhasználóra. Ebben a példában a felhasználó nem Kelley falra.

    Figyelje meg a **Szervezeti SZEREP**területén.

    ![Azure portál felhasználóazonosító](./media/billing-use-existing-office-365-account-azure-subscription/18-azure-portal-user-identity.png)

## <a name="background-information-about-azure-and-office-365-subscriptions"></a>Azure és az Office 365-előfizetések háttér-információiról
Az Office 365-ben és az Azure az Azure Active Directory szolgáltatás segítségével kezelhetők a felhasználók és az előfizetéseket. Azure címtárhoz tekinti, amelyben csoportosíthatja a felhasználók és az előfizetéseket tároló. Az Azure és az Office 365-előfizetések ugyanazt a felhasználói fiókot használ, győződjön meg arról, hogy az előfizetések ugyanabban a mappában vannak-e létre kell. Tartsa szem előtt az alábbiakat:

- Előfizetés könyvtárában, nem fordítva jön létre.
- Felhasználók könyvtárak, nem fordítva tartozik.
- Az előfizetés létrehozó felhasználó könyvtárában található előfizetés hajtanak végre. Eredményt adja az Office 365-előfizetése van kötve ugyanazzal a fiókkal, mint az Azure előfizetés létrehozása az Office 365-előfizetéssel, a fiók használatakor.

![Háttér-információk](./media/billing-use-existing-office-365-account-azure-subscription/19-background-information.png)

További tudnivalókért olvassa el a [hogyan Azure előfizetések társítva Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md)című témakört.

>[AZURE.NOTE] Azure előfizetések tulajdonában egyes felhasználó a címtárban.

>[AZURE.NOTE] Az Office 365-előfizetések tulajdonosa a címtárban magát. Ha a felhasználók a címtáron belül a szükséges engedélyekkel, azok ezek az előfizetések is működnek.

## <a name="next-steps"></a>Következő lépések
Ha külön vásárolta meg az Azure és az Office 365-előfizetések múltbeli, és meg szeretné fér hozzá az Office 365-ös bérlői az Azure előfizetésből, olvassa el a [hozzárendelése az Office 365-bérlőből egy Azure előfizetéssel](billing-add-office-365-tenant-to-azure-subscription.md)című témakört.

> [AZURE.NOTE] Ha továbbra is fennáll a probléma megoldódott gyors eléréséhez kérdések, [lépjen kapcsolatba a támogatási](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .

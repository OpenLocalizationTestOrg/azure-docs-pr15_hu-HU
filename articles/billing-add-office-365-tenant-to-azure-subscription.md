<properties
    pageTitle="Az Office 365-ös bérlői használata Azure szóló előfizetéssel |} Microsoft Azure"
    description="Megtudhatja, hogy miként hozzáadása az Office 365-ben címtár (bérlő esetében), hogy a társítás Azure-előfizetésbe."
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
    ms.date="09/16/2016"
    ms.author="cjiang"/>

# <a name="associate-an-office-365-tenant-with-an-azure-subscription"></a>Az Office 365-ös bérlői társítása az Azure előfizetéssel
Külön vásárolta meg Azure és az Office 365-előfizetések múltbeli, és most szeretné az Office 365-ös bérlői elérhessék az Azure előfizetésből, ha erre könnyen. Ebből a cikkből megtudhatja, hogyan.

> [AZURE.NOTE] Ez a cikk nem vonatkozik, a nagyvállalati szerződés (EA) ügyfelek.

## <a name="quick-guidance"></a>Rövid útmutató
Társíthatja az Office 365-ös bérlői Azure-előfizetése, használja az Azure-fiók hozzáadása az Office 365-ös bérlői webhelyen, és kattintson az Azure előfizetés társítása az Office 365-ös bérlői.

## <a name="detailed-steps"></a>A lépések részletes leírását
Ebben az esetben Kelley fal a fiók csoportjában az Azure előfizetéssel rendelkező felhasználó kelley.wall@outlook.com. Kelley is rendelkezik Office 365-előfizetéssel a fiókon kelley.wall@contoso.onmicrosoft.com. Most már Kelley szeretne férni az Office 365-ös bérlői webhelye az Azure előfizetés.

![Képernyőkép az Azure Active Directory állapota](./media/billing-add-office-365-tenant-to-azure-subscription/s31_msa-aad-status.png)

![Az Office 365 felügyeleti központ aktív felhasználók](./media/billing-add-office-365-tenant-to-azure-subscription/s32_office-365-user.png)

### <a name="prerequisites"></a>Előfeltételek
A társítási csak akkor működik helyesen az alábbi előfeltételek szükség:

- Szüksége van a szolgáltatás rendszergazdája az Azure előfizetés hitelesítő adatait. További rendszergazdák nem hajtható végre a lépéseket egy részét.
- Szüksége van az Office 365-bérlőből egy globális rendszergazdai hitelesítő adatait.
- A szolgáltatás-rendszergazda: e-mail címe nem tartalmaznia kell az Office 365-ös bérlői webhelyet.
- A szolgáltatás-rendszergazda: e-mail címe nem egyeznie kell az Office 365-ös bérlői globális rendszergazdái.
- Ha jelenleg a Microsoft-fiók és egy szervezeti fiók e-mail címre használ, ideiglenes módosítása a szolgáltatás-rendszergazda az Azure előfizetése egy másik Microsoft-fiók. A [Microsoft-fiók regisztrációs lapján](https://signup.live.com/)is létrehozhat új Microsoft-fiókkal.


Ha módosítani szeretné a szolgáltatás rendszergazdája, tegye a következőket:

1. Jelentkezzen be a [fiók kezelőportálja segítségével](https://account.windowsazure.com/subscriptions).
2. Jelölje ki a módosítani kívánt előfizetést.
3. Válassza a **Szerkesztés az előfizetés részletei**.

    ![Képernyőkép az Azure előfizetés adatainak, a "Szerkesztés az előfizetés részletei" kiemelve](./media/billing-add-office-365-tenant-to-azure-subscription/s33_azure-edit-subscription-details.png)

4. **Szolgáltatás-rendszergazda** mezőbe írja be az új szolgáltatás-rendszergazda annak az e-mail címét.

    !["Az előfizetés szerkesztése" párbeszédpanel](./media/billing-add-office-365-tenant-to-azure-subscription/s34_change-subscription-service-admin.png)

### <a name="associate-the-office-365-tenant-with-the-azure-subscription"></a>Az Office 365-ös bérlői társítása az Azure előfizetés
Az Office 365-ös bérlői társíthatja az Azure előfizetéshez, kövesse az alábbi lépéseket:

1.  Jelentkezzen be a [fiók kezelőportálja](https://account.windowsazure.com/subscriptions) szolgáltatás rendszergazdai hitelesítő adataival.
2.  A bal oldali ablaktáblában jelölje ki az **ACTIVE DIRECTORY**.

    ![Képernyőkép az Active Directory-bejegyzés](./media/billing-add-office-365-tenant-to-azure-subscription/s35-classic-portal-active-directory-entry.png)

    > [AZURE.NOTE] Az Office 365-ös bérlői nem láthatók. Ha azt látja, ugorja át a következő lépéssel.

    ![Képernyőkép: az alapértelmezett könyvtár az Azure Active Directory](./media/billing-add-office-365-tenant-to-azure-subscription/s36-aad-tenant-default.png)

3. Az Office 365-ös bérlői hozzáadása az Azure előfizetéshez.

    egy. Válassza az **Új** > **KÖNYVTÁRAT** > **egyéni létrehozása**.

    ![Képernyőkép az Azure Active Directory egyéni létrehozása](./media/billing-add-office-365-tenant-to-azure-subscription/s37-aad-custom-create.png)

    b. **Hozzáadás könyvtárat** lapon **KÖNYVTÁRAT**csoportban jelölje ki a **meglévő könyvtár használata**. **Az aláírandó most már készen is állok**válasszon, majd válassza a **teljes** ![befejezése ikonra](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

    ![Képernyőkép a "Meglévő könyvtár használata"](./media/billing-add-office-365-tenant-to-azure-subscription/s39_add-directory-use-existing.png)

    c billentyűkombinációt. Miután kijelentkezett, a globális rendszergazdai hitelesítő adataival jelentkezzen be az Office 365-bérlőjére vonatkozó.

    ![Az Office 365 globális rendszergazdája bejelentkezés](./media/billing-add-office-365-tenant-to-azure-subscription/s310_sign-in-global-admin-office-365.png)

    d. Jelölje ki, hogy **továbbra is**.

    ![Képernyőkép a tartomány ellenőrzéséhez](./media/billing-add-office-365-tenant-to-azure-subscription/s311_use-contoso-directory-azure-verify.png)

    e. Válassza a **Kijelentkezés gombra**.

    ![Képernyőkép a kijelentkezési](./media/billing-add-office-365-tenant-to-azure-subscription/s312_use-contoso-directory-azure-confirm-and-sign-out.png)

    f. Jelentkezzen be a [fiók kezelőportálja](https://account.windowsazure.com/subscriptions) szolgáltatás rendszergazdai hitelesítő adataival.

    ![Képernyőkép az Azure bejelentkezés](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)

    g. Ekkor megjelennek az Office 365-ös bérlői webhelyen az irányítópult.

    ![Irányítópult képernyőképe](./media/billing-add-office-365-tenant-to-azure-subscription/s314_office-365-tenant-appear-in-azure.png)

4. A könyvtár az Azure-előfizetéséhez társított módosítása.

    egy. Válassza a **Beállítások**.

    ![Képernyőkép az Azure klasszikus portál beállításai ikon](./media/billing-add-office-365-tenant-to-azure-subscription/s315_azure-classic-portal-settings-icon.png)

    b. Jelölje ki az Azure-előfizetése, és válassza a **SZERKESZTÉS CÍMTÁR**.
    ![Képernyőkép az Azure előfizetés Szerkesztés címtár](./media/billing-add-office-365-tenant-to-azure-subscription/s316_azure-subscription-edit-directory.png)

    c billentyűkombinációt. Jelölje be a **következő** ![tovább-ikon](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).

    ![Képernyőkép: "A társított könyvtár módosítása"](./media/billing-add-office-365-tenant-to-azure-subscription/s318_azure-change-associated-directory.png)

    > [AZURE.WARNING] Minden további rendszergazdák törlődni fognak figyelmeztető üzenetet kap.

    ![Azure-megerősítése-címtár-hozzárendelés](./media/billing-add-office-365-tenant-to-azure-subscription/s322_azure-confirm-directory-mapping.png)

    >[AZURE.WARNING] Emellett a meglévő erőforrás-csoportok a hozzárendelt hozzáféréssel rendelkező összes [szerepköralapú hozzáférés szerepalapú](./active-directory/role-based-access-control-configure.md) felhasználók is törlődnek. A figyelmeztetést kap csak az eltávolítás, további rendszergazdák az Említések.

    ![hozzárendelt-felhasználók-eltávolítása után-az erőforrás-csoportok](./media/billing-add-office-365-tenant-to-azure-subscription/s325_assigned-users-removed-resource-groups.png)

    d. Válassza a **teljes** ![befejezése ikonra](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

5. Most már vehet az Office 365-ös szervezeti fiókok, további rendszergazdák az Azure Active Directory-ös bérlői webhelyet.

    egy. Válassza a **RENDSZERGAZDÁK** fület, és válassza a **hozzáadni**.

    ![Képernyőkép az Azure klasszikus portál beállítási rendszergazdák lap](./media/billing-add-office-365-tenant-to-azure-subscription/s319_azure-classic-portal-settings-administrators.png)

    b. Írja be az Office 365-ös bérlői a szervezeti fiókkal, jelölje ki azt az Azure előfizetést, és válassza ki a **teljes** ![befejezése ikonra](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

    ![Képernyőkép az Azure hozzáadása közös rendszergazda párbeszédpanel](./media/billing-add-office-365-tenant-to-azure-subscription/s320_azure-add-co-administrator.png)

    c billentyűkombinációt. Térjen vissza a **RENDSZERGAZDÁK** lapra. Meg kell jelennie a szervezeti fiók közös rendszergazdaként jelenik meg.

    ![Képernyőkép: rendszergazdák lap](./media/billing-add-office-365-tenant-to-azure-subscription/s321_azure-co-administrator-added.png)

6. Az access következő tesztelheti a közös rendszergazdájával.

    egy. Jelentkezzen ki a fiók kezelőportálja segítségével.

    b. Nyissa meg a [fiók felügyeleti portál](https://account.windowsazure.com/subscriptions) vagy az [Azure-portálon](https://portal.azure.com/).

    c billentyűkombinációt. Ha az Azure bejelentkezési lapján, **Jelentkezzen be a szervezeti fiókjával**hivatkozást tartalmaz, jelölje ki a hivatkozást. Egyéb esetben ugorja át ezt a lépést.

    ![Képernyőkép az Azure bejelentkezési lapja](./media/billing-add-office-365-tenant-to-azure-subscription/3-sign-in-to-azure.png)

    d. Írja be a közös rendszergazdai hitelesítő adatait, és válassza a **Bejelentkezés**gombra.

    ![Képernyőkép az Azure bejelentkezési lapja](./media/billing-add-office-365-tenant-to-azure-subscription/s324_azure-sign-in-with-co-admin.png)

## <a name="next-steps"></a>Következő lépések
Kapcsolódó esetek:

- Már rendelkezik Office 365-előfizetést, és készen áll az Azure előfizetéssel, de a meglévő Office 365 felhasználói fiókok használata Azure előfizetéshez tartozó szeretne.
- Az Azure előfizetői, és szeretné, hogy a felhasználók az Office 365-előfizetéssel a meglévő Azure Active Directory-példány.

Megtudhatja, hogy miként elvégezheti az alábbi műveleteket, olvassa el a [meglévő Office 365-ös számla, az Azure-előfizetéséhez, vagy fordítva használata](billing-use-existing-office-365-account-azure-subscription.md)című témakört.

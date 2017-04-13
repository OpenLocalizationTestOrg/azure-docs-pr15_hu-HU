<properties
    pageTitle="Egyéni tartományneveket az Azure Active Directory-ban kezelése |} Microsoft Azure"
    description="Adatkezelési fogalmak és az Azure Active Directory tartománynév kezelésére szolgáló how-tos"
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="curtand;jeffsta"/>

# <a name="managing-custom-domain-names-in-your-azure-active-directory-preview"></a>Egyéni tartományneveket az Azure Active Directory-ban kezelése

A tartománynév része fontos sok címtár erőforrás azonosítóját: a felhasználó nevét vagy e-mail címét a felhasználó, csoport, a cím része szerepel, és az alkalmazás azonosítója URI az alkalmazás részei lehetnek. Egy erőforrás az Azure Active Directory (Azure Active Directory) előzetes is elhelyezhet, amely már ellenőrzött lesz a címtárban, amely tartalmazza az erőforráshoz tartozó tartománynév. [Mi az a előzetes verzióban?](active-directory-preview-explainer.md) Csak a globális rendszergazda az Azure Active Directory tartományi felügyeleti feladatok végezheti el.

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>Az Azure Active directory használt elsődleges tartománynévnek beállítása

A címtár létrehozásakor az eredeti tartománynevet, például "contoso.onmicrosoft.com," egyben elsődleges tartománynévnek. Az elsődleges tartomány az alapértelmezett tartománynevet az új felhasználó esetén egy új felhasználót hoz létre. Ez a folyamat, új felhasználók létrehozása a portálon a rendszergazda leegyszerűsíti a. Az elsődleges tartománynévnek módosítása:

1.  Jelentkezzen be az [Azure portál](https://portal.azure.com) , amely a könyvtár egy globális rendszergazdai fiókkal.

2.  Jelölje ki a **További szolgáltatások**, adja meg az **Azure Active Directory** a szövegmezőbe, és válassza az **ENTER billentyűt**.

    ![Nyitó felhasználók kezelése](./media/active-directory-domains-add-azure-portal/user-management.png)

3. A ***Címtár-neve*** lap jelölje be **a tartománynevek**.

4. Válassza a ** *Címtár-neve* - Domain names** lap a tartomány nevét, hogy az elsődleges tartománynévnek szeretné.

5.  A ***tartománynév*** lap (Ez azt jelenti, hogy a lap, amely megnyitja, amelynek címe az új tartománynevet) válassza ki az **elsődleges győződjön** parancsot. Erősítse meg a választási lehetőségek, amikor a rendszer kéri.

    ![Ellenőrizze a tartománynév elsődleges](./media/active-directory-domains-manage-azure-portal/make-primary.png)

A könyvtár bármely igazolt tartomány nem összevont használt elsődleges tartománynévnek módosíthatja. Az elsődleges tartomány a könyvtár módosítása nem változik a bármelyik meglévő felhasználók nevével.

## <a name="add-custom-domain-names-to-your-azure-ad"></a>Egyéni tartománynév hozzáadása az Azure Active Directory

Minden egyes Azure Active directory legfeljebb 900 egyéni tartományneveket vehet. A folyamat [További egyéni tartománynév hozzáadása](active-directory-domains-add-azure-portal.md) megegyezik az első egyéni tartománynevet.

## <a name="add-subdomains-of-a-custom-domain"></a>Vehet fel altartományokat az egyéni tartomány

Ha szeretne egy harmadlagos szintű tartomány neve, például "europe.contoso.com" hozzáadása a címtárhoz, meg kell először hozzáadása és a másodlagos szintű tartomány (például contoso.com). Az altartomány a Azure AD automatikus fog ellenőrizve. Szeretné látni, hogy az imént felvett altartomány ellenőrizte, frissítse a lapot a böngészőben, amely felsorolja a tartományok.

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>Mi a teendő, ha az egyéni tartománynevet módosítja a DNS-szolgáltató

Ha módosítja a DNS-szolgáltató az egyéni tartománynevet, továbbra is az Azure Active Directory magát megszakítása és további beállítási feladatok nélkül egyéni tartománynevét használja. Ha az Office 365-ben, az Intune és más szolgáltatások támaszkodó egyéni tartományneveket az Azure Active Directory használhatja egyéni tartománynevét, keresse meg a szolgáltatások dokumentációjában.

## <a name="delete-a-custom-domain-name"></a>Egyéni tartománynév törlése

Ha szervezete már nem használja a tartománynevét, vagy ha módosítani szeretné, hogy a tartománynév használata az Azure AD egy másik egyéni tartománynevet is törlése az Azure Active Directory.

Ha törölni szeretne egy egyéni tartománynevet, először győződjön meg, hogy nincsenek erőforrások címtárában támaszkodhat a tartomány nevét. Nem lehet törölni a tartomány nevét a címtárból:

-   Minden felhasználó rendelkezik egy felhasználó nevét, e-mail címét vagy proxycím, amely tartalmazza a tartomány nevét.

-   Minden kívánt e-mail címet vagy a tartománynevet tartalmazó proxycím tartalmaz.

-   Az Azure Active Directory minden alkalmazásnak az alkalmazás azonosítója URI, amely tartalmazza a tartomány nevét.

Kell módosítása vagy törlése az Azure Active directory az ilyen az erőforrás, az egyéni tartománynevet törlése előtt.

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a>A PowerShell vagy grafikonon API segítségével tartománynevek kezelése

A legtöbb kezelési feladatok az Azure Active Directory tartományi nevek is fejeződhet, Microsoft PowerShell használatával, illetve programozás útján Azure Active Directory Graph API (használata nyilvános előzetes verzió).

-   [A tartománynevek kezeléséhez az Azure Active Directory PowerShell használatával](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [Kezelheti a tartományneveket az Azure Active Directory Graph API segítségével](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Következő lépések

-   [Egyéni tartománynév hozzáadása](active-directory-domains-add-azure-portal.md)

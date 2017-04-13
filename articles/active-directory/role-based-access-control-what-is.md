<properties
    pageTitle="Szerepköralapú hozzáférés-vezérlés |} Microsoft Azure"
    description="Első lépések az access-kezelés az Azure szerepköralapú hozzáférés az Azure-portálon. Szerepkör-hozzárendelés segítségével a címtárában található engedélyeket."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="get-started-with-access-management-in-the-azure-portal"></a>Első lépések a hozzáférés-kezelés az Azure-portálon

Biztonsági lépések vállalatok kell kiemelése a nekik szükséges jogosultságokkal pontos engedélyeket ad az alkalmazottak. Túl sok engedélyek támadásokkal fiók közzététele. Túl kevés engedélyek, az azt jelenti, hogy az alkalmazottak nem tudják elérni az hatékonyan tudnak dolgozni. Azure szerepköralapú hozzáférés vezérlő (RBAC) segítséget nyújt a probléma megoldása Azure külön access kezelésének biztosításával.

RBAC használ, feladatai újból a munkacsoporton belül, és adjon csak access összegét felhasználóknak munkájuk elvégzéséhez szükségük van. Helyett, ahol a felhasználók engedélyeit korlátlan az Azure előfizetés vagy erőforrásokat, engedélyezheti, ha csak bizonyos műveleteket. Ha például használata a RBAC egy alkalmazott kezelése az előfizetés, virtuális gépeken futó, miközben egy másik kezelheti az azonos előfizetésben SQL-adatbázisait.

## <a name="basics-of-access-management-in-azure"></a>Azure-ban a hozzáférés-kezelés alapjai
Egyes Azure előfizetéshez társítva egyik Azure Active Directory (AD) könyvtár. Felhasználók, csoportok és alkalmazások e könyvtárból kezelheti az erőforrások Azure-előfizetésben. Rendelje hozzá a következő az Azure portál, Azure parancssori eszközök és Azure Management API-k hozzáférési jogokat.

A felhasználók, csoportok és egy bizonyos hatókör alkalmazásokat a megfelelő RBAC szerepkörrel hozzárendelésével hozzáférésének biztosítása. Szerepkör-hozzárendelés hatókörének lehet előfizetés, erőforráscsoport vagy egy erőforrás. A szülő hatókör szerepkört is hozzáférést a gyermekek benne. Ha például erőforráscsoport hozzáféréssel rendelkező felhasználó tartalmaz, például webhelyek, a virtuális gépeken futó és a alhálózat összes erőforrás is kezelheti.

![Kapcsolat közötti elemek Azure Active Directory - diagram](./media/role-based-access-control-what-is/rbac_aad.png)

A RBAC szerepkör hozzárendelése, milyen erőforrások, a felhasználó, csoport vagy alkalmazás kezelhetik a hatálya alá szerint.

## <a name="built-in-roles"></a>Beépített szerepkörök
Azure RBAC alkalmazása az összes erőforrástípus három alapvető szerepköröket foglalja magában:

- **Tulajdonos** jobbra meghatalmazotti hozzáférést másokkal együtt összes erőforrás teljes hozzáféréssel rendelkezik.
- **Közös munka** hozhat létre és Azure erőforrások diagramtípusokat kezelése, de nem tud hozzáférést másokkal.
- **A képernyőolvasók** meglévő Azure erőforrásokat is megtekintheti.

A RBAC szerepkörök Azure-ban a többi adott Azure erőforrások kezelését teszi lehetővé. Ha például a virtuális gép munkatársi szerepkörök lehetővé teszi, hogy a felhasználók létrehozása és kezelése a virtuális gépeken futó. Azt nem neki hozzáférést a virtuális hálózat vagy a alhálózat, amely a virtuális gép csatlakozik.

[Beépített szerepkörök RBAC](role-based-access-built-in-roles.md) Azure-ban elérhető szerepköröket sorolja fel. Adja meg, hogy az egyes beépített szerepköre felhasználók hatókör és műveleteket. Ha még több szabályozási saját szerepkörök megadásához, megtudhatja, hogy miként [Azure RBAC egyéni szerepkörök](role-based-access-control-custom-roles.md)létrehozására.

## <a name="resource-hierarchy-and-access-inheritance"></a>Erőforrás-hierarchia és az access öröklése
- Minden **előfizetés** Azure-ban csak egy címtár tartozik.
- Minden **erőforráscsoport** csak egy előfizetéssel tartozik.
- Csak egy erőforrás csoport minden **erőforrás** tartozik.

A szülő hatókörök megadása az Access a gyermek hatókörök örökli. Példa:

- Az Olvasó szerepkör hozzárendelése egy Azure AD az előfizetés hatókör csoport. A csoport tagjai az előfizetés minden erőforráscsoport és az erőforrás megtekintése.
- A munkatársi szerepkörök hozzárendelése az erőforrás csoport hatóköre az alkalmazást. Az erőforráscsoport, de nem az előfizetés más erőforrás csoportok bármilyen típusú is erőforrások.

## <a name="azure-rbac-vs-classic-subscription-administrators"></a>Azure RBAC klasszikus előfizetés rendszergazdák összehasonlítása
Klasszikus előfizetés rendszergazdák és a további-rendszergazdák az Azure előfizetés teljes hozzáférése van. Ezek az erőforrások az [Azure portal](https://portal.azure.com) segítségével az Azure erőforrás-kezelő API-k és az [Azure klasszikus portál](https://manage.windowsazure.com) és -Azure klasszikus telepítési modell kezelheti. A RBAC modell klasszikus rendszergazdák vehetnek tulajdonosa, az előfizetés hatókör.

Az Azure portál és az új Azure erőforrás-kezelő API-k támogatja az Azure RBAC. Felhasználók és az alkalmazások RBAC szerepkörökhöz rendelt nem használható a klasszikus adatkezelési portál és az Azure klasszikus telepítési modell.

## <a name="authorization-for-management-vs-data-operations"></a>Engedély kezelésére és az adatműveleteket összehasonlítása
Azure RBAC adatkezelési műveletek az Azure erőforrások csak támogatja az Azure portál és Azure erőforrás-kezelő API-khoz. Azt nem engedélyezik a minden szintű adatműveleteket Azure erőforrások. Ha például engedélyezheti tároló fiókokat, ha valaki, de nem a kívánt BLOB vagy a táblázatok belül a tárterület-fiók nem. Hasonlóan egy SQL-adatbázis kezelhető, de nem benne a táblákat.

## <a name="next-steps"></a>Következő lépések
- Első lépések a [szerepköralapú hozzáférés-vezérlés az Azure-portálon](role-based-access-control-configure.md).
- Lásd: a [RBAC beépített szerepkörök](role-based-access-built-in-roles.md)
- A saját [egyéni szerepkörök Azure RBAC](role-based-access-control-custom-roles.md) meghatározása

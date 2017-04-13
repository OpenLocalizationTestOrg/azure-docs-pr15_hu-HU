<properties
   pageTitle="Első lépések a személyes sablonok |} Microsoft Azure"
   description="Hozzáadása, kezelése és megosztása a személyes sablonok az Azure-portálra, az Azure CLI vagy a PowerShell használatával."
   services="marketplace-customer"
   documentationCenter=""
   authors="VybavaRamadoss"
   manager="asimm"
   editor=""
   tags="marketplace, azure-resource-manager"
   keywords=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/18/2016"
   ms.author="vybavar"/>

# <a name="get-started-with-private-templates-on-the-azure-portal"></a>Első lépések a személyes sablonok az Azure-portálon

Az [Erőforrás-kezelő Azure](../resource-group-authoring-templates.md) -sablon használta a üzembe deklaráció sablon. Határozza meg a források megoldást üzembe helyezése, és adja meg a paraméterek és, amely lehetővé teszi a bemeneti értékek különböző környezetekben a változó. A sablont, ahol a JSON és kifejezések, amelyeket értékeket a telepítéshez összeállításához használhat.

Használhatja az új **sablon** lehetőséget az [Azure-portálon](https://portal.azure.com) együtt a **Microsoft.Gallery** erőforrás-szolgáltató a a [Microsoft Azure piactéren](https://azure.microsoft.com/marketplace/) kiterjesztése engedélyezése a felhasználóknak elkészítheti, kezelheti és a személyes sablonok egy személyes tár telepítése.

A dokumentum megismerkedhet hozzáadásával, kezelése és megosztása az Azure-portálon személyes **sablon** keresztül.

## <a name="guidance"></a>Útmutató

Az alábbi javaslatokat segítségével kihasználhatja teljes **sablonok** a megoldásokkal használatakor:

- A **sablon** egy-egy erőforrás-kezelő sablont, és további metaadatokat tartalmazó encapsulating erőforrás. Hasonlóképpen nagyon viselkedik a piactér-elemre. A fő különbség, hogy az nem pedig a nyilvános piactér elemek magánjellegű elemek.
- A **sablonok** tár jól működik a felhasználók, akik testre szeretné szabni a telepítések.
- **Sablonok** jól működik van szüksége egy egyszerű tárházba belül Azure felhasználóknál.
- Nyisson meg egy meglévő erőforrás-kezelő sablont. A meglévő erőforráscsoport talál sablonokat [github](https://github.com/Azure/azure-quickstart-templates) , vagy [exportálja a sablont](../resource-manager-export-template.md) .
- **Sablonok** felhasználó megkapja közzé, azokat is tartozik. A publisher neve olvasható hozzáféréssel rendelkező minden felhasználó számára láthatóvá.
- **Sablonok** erőforrás-kezelő erőforrásokat, és nem nevezhetők át egyszer közzé.

## <a name="add-a-template-resource"></a>Sablon erőforrás hozzáadása

Kétféleképpen **sablon** erőforrás létrehozása az Azure-portálon.

### <a name="method-1--create-a-new-template-resource-from-a-running-resource-group"></a>1 módszer: Hozható létre új sablon erőforrás futó erőforrás

1. Nyissa meg azt a meglévő erőforráscsoport az Azure-portálon. Jelölje be a **sablon exportálása** a **beállításai**.
2. Az erőforrás-kezelő sablon exportálása után a **Sablon mentése** gomb segítségével mentse a **sablonok** tárat. Az Exportálás sablon összes adatának megkeresése [az alábbi](../resource-manager-export-template.md).
<br /><br />
![Erőforrás-csoport exportálása](media/rg-export-portal1.PNG)  <br />

3. Jelölje be a **sablon mentéséhez** parancsgombra.
<br /><br />

4. Írja be az alábbi adatokat:

    - Név – a sablon objektum neve (Megjegyzés: az Azure erőforrás-kezelő alapú neve. Az összes elnevezési korlátozások érvényesek, és nem lehet módosítani, miután létrehozott).
    - Leírás – rövid összefoglaló a sablon.

    ![Sablon mentése](media/save-template-portal1.PNG)  <br />

5. Kattintson a **Mentés**gombra.

    > [AZURE.NOTE] Az Exportálás sablon lap jelenít értesítések hibák van-e az exportált erőforrás-kezelő sablonhoz, de továbbra is mentheti a sablonok erőforrás-kezelő ezzel a sablonnal. Győződjön meg róla, jelölje be, és a elhárításához valamelyik erőforrás-kezelő sablon az exportált erőforrás-kezelő sablon újbóli előtt.

### <a name="b-method-2--add-a-new-template-resource-from-browse"></a>B 2 módszer: Új sablon erőforrás hozzáadása a Tallózás gombra

Üres az új **sablon** is hozzáadhat a + Hozzáadás parancsgomb **Tallózás > sablonok**. Meg kell adja meg a név, leírás és az erőforrás-kezelő sablon JSON.

![Sablon hozzáadása](media/add-template-portal1.PNG)  <br />

> [AZURE.NOTE] Microsoft.Gallery egy bérlői Azure erőforrás-szolgáltató alapján. A sablon erőforrás azt létrehozó felhasználó területhez tartozik. Egyetlen konkrét előfizetéshez nem kötődik. Előfizetés kell csak akkor, amikor üzembe helyezése a sablont választják.

## <a name="view-template-resources"></a>Sablon erőforrások megtekintése

Elérhető az összes **sablonokat** is láthatja a **Tallózás > sablonok**. Ide tartoznak a **sablonok** és szegélyei különböző engedélyszinteket az Önnel megosztott hozott létre. További részleteket a [hozzáférés-vezérlés](#access-control-for-a-tenant-resource-provider) szakaszában.

![Sablonok nézet](media/view-template-portal1.PNG)  <br />

Egy elemet a listában a gombra kattintva megtekintheti a **sablon** részleteit.

![Sablonok nézet](media/view-template-portal2c.png)  <br />

## <a name="edit-a-template-resource"></a>Sablon erőforrás szerkesztése

**Sablon** szerkesztése folyamatábrája a jobb gombbal az adott elemet a Tallózás listában vagy a Szerkesztés parancs gombra kattintva kezdeményezhet.

![Sablon szerkesztése](media/edit-template-portal1a.PNG)  <br />

Szerkesztheti a leírás és erőforrás-kezelő sablon szövegét. A név nem szerkeszthető, mivel ez éppen egy erőforrás-kezelő erőforrás nevét. Amikor, az erőforrás-kezelő sablon szerkesztése JSON azt ellenőrzi, annak érdekében, hogy érvényes JSON. Válassza az **OK gombra** , majd **Mentse** a frissített sablon mentéséhez.

![Sablon szerkesztése](media/edit-template-portal2a.PNG)  <br />

A **sablon** mentése után jelenik meg a jóváhagyást kérő értesítésnél.

![Sablon szerkesztése](media/edit-template-portal3b.png)  <br />

## <a name="deploy-a-template-resource"></a>Sablon erőforrás terjesztése

Egy tetszőleges **sablont** , ha **olvasási** engedéllyel rendelkezik a telepítheti. A telepítési folyamat elindítja a szabványos Azure-sablon a telepítés lap. Töltse ki az értékeket az erőforrás-kezelő sablon paraméterek a telepítés folytatásához.

![Sablon terjesztése](media/deploy-template-portal1b.png)  <br />

## <a name="share-a-template-resource"></a>Sablon erőforrás megosztása

**Sablon** erőforrás is megosztja a Önhöz hasonló felhasználóktól. [Szerepkör-hozzárendelés Azure bármely erőforráshoz](../active-directory/role-based-access-control-configure.md)megosztása viselkedik hasonlóan. A **sablon** tulajdonosi engedélyek más felhasználók, akik használhatók interaktív módon sablon erőforráshoz biztosít. Személy vagy csoport, akikkel megosztja a **sablon** lesz látja az erőforrás-kezelő sablon és a tár tulajdonságait.

### <a name="access-control-for-the-microsoftgallery-resources"></a>A Microsoft.Gallery erőforrások hozzáférés-vezérlés

Szerepkör | Engedélyek
---|----
Tulajdonos | Lehetővé teszi, hogy a sablon erőforrás, például a megosztás a teljes hozzáférés
A képernyőolvasók | Lehetővé teszi az olvasási és Execute(Deploy) sablon erőforrás
Közös munka | A sablon erőforrás lehetővé teszi, hogy szerkesztési és törlési engedélye. Felhasználó nem tud a sablon megosztása másokkal

Az egy adott elemet a nézet lap vagy jobb kattintással válassza a **megosztás** a Tallózás elemre. Ez elindítja a megosztás élmény.

![Megosztás sablon](media/share-template-portal1a.png)  <br />

 Most már megadhatja a szerepkörbe és a felhasználó vagy csoport hozzáférést biztosít az adott **sablont**. A rendelkezésre álló szerepkörök a tulajdonos, a képernyőolvasók és közös munka. További részleteket a fenti [hozzáférés-vezérlés](#access-control-for-a-tenant-resource-provider) szakaszában.

![Megosztás sablon](media/share-template-portal2b.png)  <br />

![Megosztás sablon](media/share-template-portal3b.png)  <br />

**Jelölje ki** és az **Ok**gombra. Ekkor megjelenik a felhasználók vagy csoportok, az erőforrás hozzá.

![Megosztás sablon](media/share-template-portal4b.png)  <br />

> [AZURE.NOTE] Sablon csak felhasználókkal és csoportokkal lesznek megosztva, Azure Active Directory ugyanahhoz a bérlőhöz. Ha egy e-mail címet, amely nem szerepel a bérlő megosztja egy sablont, meghívót küld kérő felhasználó a bérlő vendégként való.

## <a name="next-steps"></a>Következő lépések

- Erőforrás-kezelő sablonok létrehozásával kapcsolatos további tudnivalókért lásd: [sablonok létrehozása](../resource-group-authoring-templates.md)
- Az erőforrás-kezelő sablonban használható függvények, című cikkben talál részletes [sablon függvények](../resource-group-template-functions.md)
- A sablonok tervezése, olvassa el [Gyakorlati tanácsok az Azure erőforrás-kezelő sablonok tervezése](../best-practices-resource-manager-design-templates.md)

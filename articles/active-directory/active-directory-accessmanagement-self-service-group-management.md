<properties
    pageTitle="Az Azure Active Directory access Alkalmazáskezelés önkiszolgáló beállítását |} Microsoft Azure"
    description="Önkiszolgáló csoportok kezelése lehetővé teszi, hogy a felhasználók létrehozása és kezelése a biztonsági csoportok vagy az Office 365 csoportok az Azure Active Directory és kérelem biztonsági csoport vagy az Office 365-csoport tagsági lehetőséget biztosít a felhasználóknak"
    services="active-directory"
    documentationCenter=""
  authors="curtand"
    manager="femila"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/10/2016"
    ms.author="curtand"/>

# <a name="setting-up-azure-active-directory-for-self-service-group-management"></a>Azure Active Directory beállításának önkiszolgáló csoportok kezelése

Önkiszolgáló csoportok kezelése lehetővé teszi a felhasználóknak a biztonsági csoportok vagy az Azure Active Directory (Azure Active Directory) az Office 365-csoportok létrehozása és kezelése. Felhasználók is kérhet biztonsági csoport vagy az Office 365-csoport tagsági, és a csoport tulajdonosának majd jóváhagyása vagy elutasítása tagság. Ezzel a módszerrel csoporttagság napi irányítását is meghatalmazott, akik az adott tagság vállalati környezetben megértéséhez. Önkiszolgáló csoport kezelési funkciók állnak rendelkezésre csak biztonsági csoportokat és az Office 365-csoportokat, de nem levelezési biztonsági csoportoknak vagy terjesztési listáknak.

Önkiszolgáló csoportok kezelése jelenleg magában foglalja a két alapvető felhasználási helyzet: delegált felügyeleti csoport és a csoportok önkiszolgáló kezelése.

- **Csoportok kezelése meghatalmazott** 
   példa olyan rendszergazda, aki a vállalat által használt szoftver alkalmazás kezeli. Ezeket a hozzáférési jogokat kezelése egyre lehető, így a rendszergazdák arra utasítja az új csoport létrehozása a vállalati tulajdonosa. A rendszergazda rendeli hozzá az új csoporthoz az alkalmazás hozzáférést, és hozzáadja a csoport összes személy már az alkalmazás elérése. Az üzleti tulajdonosa majd vehet fel további felhasználókat, és az alkalmazás automatikusan kiépített környezetet azoknak a felhasználóknak. Az üzleti tulajdonosa nem kell várja meg a rendszergazdát, hogy a felhasználók hozzáférésének kezelése. Ha a rendszergazda engedélyt a azonos felettes egy másik üzlet csoportban, majd az adott személy is kezelheti a saját felhasználók hozzáférésének. A vállalati tulajdonosának, sem a kezelő megtekintése vagy felhasználókezelés egymás. A rendszergazda továbbra is láthatja az alkalmazás és a blokk jogokkal szükség esetén hozzáféréssel rendelkező összes felhasználó.

- **Önkiszolgáló csoportok kezelése** 
   példa példánkban két mindkét rendelkező felhasználók SharePoint Online-webhelyek, hogy állítsa be önállóan. Szeretnének egymás csoportok hozzáférési engedély webhelyükön. Ehhez azokat is egy csoport létrehozása az Azure Active Directory, és a SharePoint Online mindegyiket kijelölése, hogy a csoport webhelyükön hozzáférést biztosít. Ha valaki csevegni access, kérése az Access panelről, és a jóváhagyás után azok letöltésére mindkét SharePoint Online-webhelyeken való hozzáférést. Későbbi az egyik úgy dönt, hogy, hogy a webhely eléréséhez szükséges összes személy kell is hozzáférhet az egy adott szoftver alkalmazás. A rendszergazda a szoftver alkalmazás az alkalmazást a SharePoint Online-webhelyre vonatkozó engedélyeket adhat hozzá. Ettől kezdve a bármely első jóváhagyott összehívások hozzáférést biztosít a két SharePoint Online-webhelyekre és a szoftver alkalmazásba.

## <a name="making-a-group-available-for-end-user-self-service"></a>Egy csoport elérhetővé tétele a végfelhasználói önkiszolgáló

1. Az [Azure klasszikus portálon](https://manage.windowsazure.com)nyissa meg az Azure Active directory.

2. A **beállítás** lapon állítsa a **meghatalmazott csoportok kezelése** engedélyezve van.

3. Engedélyezi a **felhasználók a biztonsági csoportokat hozhat létre** , vagy **a felhasználók hozhatnak létre csoportokat az Office** beállítást.

**A felhasználók hozhatnak létre a biztonsági csoportok** engedélyezve van, az összes felhasználó a címtárban szabad hozzon létre új biztonsági csoportokat és a tagok felvételét ezekhez a csoportokhoz. Ezeket a új csoportokat is tenné jelenik meg az Access panel szolgál más felhasználók számára. Ha az adott csoportra a házirend-beállítás lehetővé teszi, más felhasználók az ezekbe a csoportokba való kérések hozhat létre. **A felhasználók hozhatnak létre a biztonsági csoportok** le van tiltva, ha a felhasználók nem hozható létre csoportokat, és nem módosíthatja a meglévő csoportokhoz, amelynek a tulajdonos. Jó helyen jár és továbbra is kezelheti a csoport tagságát be szeretne kapcsolódni, azok a csoportok a többi felhasználó kérelmek jóváhagyása.

**Használhatja a biztonsági csoportok önkiszolgáló felhasználók** elérése több egyedi hozzáférés-vezérlés fölé önkiszolgáló csoportok kezelése a felhasználók számára is használhatja. **A felhasználók hozhatnak létre csoportokat** engedélyezve van, az összes felhasználó a címtárban szabad létrehozhat új csoportokat és a tagok felvételét ezekhez a csoportokhoz. Megadásával is **használhatja a biztonsági csoportok önkiszolgáló felhasználók** néhány, Ön korlátozásáról csak egy korlátozott felhasználócsoportjának-kezelés csoport. Ha ezzel a kapcsolóval néhány van beállítva, hozzá kell adnia felhasználóknak a csoport SSGMSecurityGroupsUsers létrehozhat új csoportokat és a tagok hozzáadása előtt. **Használhatja a biztonsági csoportok önkiszolgáló felhasználók** az összes beállításával engedélyezése minden felhasználó a címtárban új csoportokat hozhat létre.

A **csoport használatára képes biztonsági csoportok önkiszolgáló** listában segítségével egy csoportot, amelynek tagjai használható önkiszolgáló egyéni nevét.

## <a name="additional-information"></a>További információk

Ezek a cikkek Azure Active Directory további tájékoztatást nyújt.

* [Erőforrások való hozzáférés kezelése az Azure Active Directory-csoportok](active-directory-manage-groups.md)

* [Azure Active Directory-parancsmagjai csoport beállításainak konfigurálása](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)

* [Mi az Azure Active Directory?](active-directory-whatis.md)

* [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)

<properties
    pageTitle="Csoportok az Azure Active Directory kezelése |} Microsoft Azure"
    description="Hogyan lehet az Azure Active Directory használatával Azure-felhasználók kezelése: csoportok létrehozása és kezelése."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/29/2016"
    ms.author="curtand"/>


# <a name="managing-groups-in-azure-active-directory"></a>Az Azure Active Directory csoportok kezelése

> [AZURE.SELECTOR]
- [Azure portál](active-directory-groups-create-azure-portal.md)
- [Azure klasszikus portál](active-directory-accessmanagement-manage-groups.md)
- [A PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)


Azure Active Directory (Azure Active Directory) felhasználókezelés funkciói egyik létre felhasználói csoportok. Egy csoport segítségével felügyeleti feladatokat, például a licencek vagy az engedélyek egyszerre hozzárendelése a felhasználókhoz. Használhatja a csoportok hozzáférési engedélyt hozzárendelése

- Erőforrások, például a címtár-objektumok
- A címtárhoz, például a szoftver alkalmazásokat, a Azure szolgáltatások, a SharePoint-webhelyek külső erőforrások, vagy a helyszíni erőforrások

Ezenkívül egy erőforrás tulajdonosa is rendelhet az access egy erőforrást egy valaki más tulajdonában Azure Active Directory-csoportot. A hozzárendelés adja meg, hogy az erőforrás csoport hozzáférését tagjainak. Ezután a csoport tulajdonosának kezeli a csoport tagsága. Hatékony az erőforrás tulajdonosa delegálja a csoport tulajdonosának engedélye arra, hogy a felhasználók hozzárendelése az erőforrás.

## <a name="how-do-i-create-a-group"></a>Hogyan hozhatok létre csoportot?

Attól függően, hogy a szolgáltatások, a szervezet, amelyre előfizetett létrehozhat egy csoport nevében használatával az alábbiak egyikét:
- az Azure klasszikus portálra
- az Office 365-fiók portálon
- a Windows Intune fiók portál

Feladatok azt fogja leírása, az Azure klasszikus portálon elvégzett. Az Azure Active directory kezelése nem Azure portálokról használatával kapcsolatos további tudnivalókért olvassa el [az Azure Active directory felügyelete](active-directory-administer.md)című témakört.

1. Az [Azure klasszikus portálon](https://manage.windowsazure.com)válassza ki **Az Active Directory**, és válassza ki a könyvtár a szervezet neve.

2. Kattintson a **csoportok** fülre.

3. Jelölje be a **csoport hozzáadása**.

4. A **Csoport hozzáadása** ablakban adja meg a nevét, és a csoport leírása.


## <a name="how-do-i-add-or-remove-individual-users-in-a-security-group"></a>Hogyan hozzáadása vagy eltávolítása az egyéni felhasználók a biztonsági csoport?

**Az egyes felhasználók hozzáadása csoporthoz**

1. Az [Azure klasszikus portálon](https://manage.windowsazure.com)válassza ki **Az Active Directory**, és válassza ki a könyvtár a szervezet neve.

2. Kattintson a **csoportok** fülre.

3. Nyissa meg azt a csoportot, amelyhez tagokat szeretne felvenni kívánt. Nyissa meg a kijelölt csoport **tagjai** lapját, ha azt már nem jelennek meg.

4. Jelölje be **a tagok hozzáadása**.

5. A **Tagok hozzáadása** lapon válassza ki a felhasználó vagy csoport kívánt vegye fel tagként a csoport nevét. Győződjön meg arról, hogy ez a név bekerül a **kijelölt** ablak.


**Az egyes felhasználók eltávolítása csoportból**

1. Az [Azure klasszikus portálon](https://manage.windowsazure.com)válassza ki **Az Active Directory**, és válassza ki a könyvtár a szervezet neve.

2. Kattintson a **csoportok** fülre.

3. Nyissa meg azt a csoportot, amelyből el szeretné távolítani a tagok.

4. Jelölje be a **tagok** fülre, jelölje ki a csoportból eltávolítani kívánt tag nevét, és kattintson az **Eltávolítás**.

6. Erősítse meg a parancssorba, hogy a tagok eltávolítása a csoportból.


## <a name="how-can-i-manage-the-membership-of-a-group-dynamically"></a>Hogyan lehet dinamikusan kezelni a csoport tagságát?

Azure Active Directory, a nagyon egyszerűen állíthat be egyszerű szabály annak meghatározásához, hogy mely felhasználók a csoport tagjai. Egyszerű szabály szerepel, amely csak egyetlen összehasonlítása. Például ha csoportot a szoftver alkalmazások van rendelve, beállíthatja, hogy be a szabály vehet fel felhasználókat, akiknek van egy feladat címére "Értékesítő". Ez a szabály majd adott beosztás címtárában rendelkező összes felhasználó szoftver alkalmazás hozzáférést biztosít.

Bármely felhasználó módosítás attribútumok, a rendszer átmenetileg a megjelenítéséhez, ha az attribútum módosítása annak a felhasználónak szeretné elindítani bármelyik csoportot könyvtárában található összes dinamikus csoport szabály hozzáadása vagy eltávolítása. Ha egy felhasználó megfelel egy szabály csoport, hozzáadásuk tagként a csoporthoz. Ha már nem megfelelnek a szabály csoport tagjának, hogy törlődjenek egy tagként csoport.

> [AZURE.NOTE] Beállíthatja a biztonsági csoportok vagy az Office 365-csoportok dinamikus tagsági szabály. Beágyazott csoporttagság jelenleg nem támogatottak, csoport-alapú hozzárendelés alkalmazások.
>
> Dinamikus tagságot-csoportok legyen szükség a hozzárendelni kívánt Azure Active Directory prémium licencre
>
> - A rendszergazda, aki a szabály csoport kezelése
> - A csoport összes tagjának

**Ahhoz, hogy egy csoport dinamikus tagság**

1. Az [Azure klasszikus portálon](https://manage.windowsazure.com)válassza ki **Az Active Directory**, és válassza ki a könyvtár a szervezet neve.

2. Kattintson a **csoportok** fülre, és nyissa meg a szerkeszteni kívánt csoportot.

3. A **beállítás** lapon jelölje ki, és **Dinamikus tagságot engedélyezése** állítsa **Igen**értékre.

4. A csoport szabályozhatja a csoportosító függvények dinamikus tagságát egyszerű egyetlen szabály beállítása. Győződjön meg arról, hogy a **vehet fel felhasználókat hol** beállítás be van jelölve, és válassza ki a listából (például részleg, munkakör stb.), egy tulajdonság

5. Ezután jelölje ki a megadott feltétel (nem egyenlő, egyenlő, nem elindítja a, kezdete, nem tartalmaz, tartalmazza, nem egyezik, hol.van).

6. A kijelölt felhasználó tulajdonság összehasonlító értékének meghatározása.

Arról, hogy miként dinamikus csoporttagság *Speciális* szabályok (szabályokat, amely tartalmazhat több összehasonlítása) létrehozása című témakörben talál [használata attribútumok speciális szabályok létrehozásához](active-directory-accessmanagement-groups-with-advanced-rules.md).

## <a name="additional-information"></a>További információk

Ezek a cikkek Azure Active Directory további tájékoztatást nyújt.

* [Erőforrások való hozzáférés kezelése az Azure Active Directory-csoportok](active-directory-manage-groups.md)

* [Azure Active Directory-parancsmagjai csoport beállításainak konfigurálása](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)

* [Mi az Azure Active Directory?](active-directory-whatis.md)

* [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)

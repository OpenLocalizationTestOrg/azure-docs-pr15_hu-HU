<properties
    pageTitle="Erőforrások való hozzáférés kezelése az Azure Active Directory-csoportok |} Microsoft Azure"
    description="Hogyan használhatja a csoportokat az Azure Active Directory a helyszíni és felhőbeli felhasználásuk erőforrások felhasználó hozzáférését kezelheti."
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
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="curtand"/>


# <a name="managing-access-to-resources-with-azure-active-directory-groups"></a>Erőforrások való hozzáférés kezelése az Azure Active Directory-csoportok

Azure Active Directory (Azure Active Directory) átfogó identitás- és az access kezelési megoldás biztosít robusztus funkciók a hozzáférést a helyszíni és felhőbeli alkalmazások és erőforrások, például a Microsoft online szolgáltatások, például az Office 365-ben és a világon Microsoft SaaS alkalmazások kezeléséhez. Ez a cikk áttekintést nyújt arról, de ha szeretné elindítani az Azure Active Directory-csoportok használatával pillanatban, kövesse a [biztonsági csoportok kezelése a az Azure Active Directory](active-directory-accessmanagement-manage-groups.md). Ha meg szeretné tekinteni, hogyan használhatja a PowerShell kezelheti a csoportokat az Azure Active directory erről további az [előzetes verzió parancsmagok Azure Active Directory-csoportok kezelése](active-directory-accessmanagement-groups-settings-v2-cmdlets.md).


> [AZURE.NOTE] Azure Active Directory használatához az Azure-fiók szükséges. Ha nem rendelkeznek fiókkal, [ingyenes Azure fiókot regisztrálni](https://azure.microsoft.com/pricing/free-trial/)is.


Belül Azure Active Directory a főbb funkciói egyik az azt jelenti, hogy erőforrások való hozzáférés kezelése. Ezek az erőforrások lehet címtár, ahogy a kis-és az objektumok között a címtár vagy információforrások, amelyek a címtárhoz, például a szoftver alkalmazások, a Azure szolgáltatásokat és a SharePoint-webhelyek vagy helyi erőforrások külső szerepkörök kezeléséhez szükséges engedélyek részét. A felhasználó hozzáférési jogosultsággal, és egy erőforrás tevékenységekhez hozzárendelhető négy módja van:


1. Közvetlen hozzárendelés

    A hozzárendelt felhasználók közvetlenül az erőforrás a tulajdonos, az adott erőforrás által.

2. A csoporttagság

    Egy csoport rendelhetők egy erőforrás az erőforrás tulajdonosa, és ezzel a módszerrel, megadása, hogy az erőforrás csoport hozzáférését tagjainak. A csoport tagságát kezelheti a csoport tulajdonosa. Hatékony az erőforrás tulajdonosa delegálja jogosult felhasználók hozzárendelése az erőforrás a csoport tulajdonosának.

3. A szabály alapján

    Az erőforrás tulajdonosa szabály használatával express, hogy mely felhasználók célszerű lehet access az erőforráshoz rendelt. Az egyes felhasználók az adott szabály és az értékük használt attribútumok függ, hogy az így létrejövő a szabályt, és így az erőforrás tulajdonosa hatékony delegálja jobbra a szabály használt attribútum az erőforrás a mérvadó forrást való hozzáférés kezelése. Az erőforrás tulajdonosa továbbra is a szabály maga kezeli, és határozza meg, hogy mely attribútumok és értékeket adja meg az erőforráshoz való hozzáférés.

4. Külső hitelesítésszolgáltató

    Az access, az erőforrás származik külső forrásból; Ha például egy csoportot, amely egy mérvadó forrásból, például egy helyszíni directory vagy a szoftver alkalmazást, például a kalk.munkanap szinkronizálja a rendszer. Az erőforrás tulajdonosa rendeli hozzá az erőforrás hozzáférést biztosít a csoportot, és a külső adatforrás kezeli a csoport tagjainak.

  ![Access-kezelés diagram áttekintése](./media/active-directory-access-management-groups/access-management-overview.png)


## <a name="watch-a-video-that-explains-access-management"></a>Nézzen meg egy videót, amely bemutatja a hozzáférés kezelése

Megtekinthet egy rövid videót, amelyből megtudhatja, további tudnivalók:

**Azure Active Directory: Csoportok dinamikus tagság – bevezetés**

> [AZURE.VIDEO azure-ad--introduction-to-dynamic-memberships-for-groups]

## <a name="how-does-access-management-in-azure-active-directory-work"></a>Hogyan elérni az Azure Active Directory munka management?
Az Azure Active Directory közepére az access-kezelési megoldás a biztonsági csoport. Biztonsági csoport erőforrásokhoz való hozzáférés kezelése használata a jól ismert mintákat, amely lehetővé teszi, hogy az erőforrás hozzáférést biztosít a felhasználók a tervezett csoport rugalmas és könnyen érthető lehetőséget. Az erőforrás tulajdonosa (vagy a rendszergazda a könyvtár) hozzárendelhet egy bizonyos hozzáférést biztosít jobbra az erőforrások őket a saját csoport. A csoport tagjainak biztosítja az access, majd az erőforrás tulajdonosa átadhatja a tagok listájának valaki másnak, például a részleg vezető vagy az IT-részleghez rendszergazda csoport kezelése jobbra.

![Azure Active Directory access management diagram](./media/active-directory-access-management-groups/active-directory-access-management-works.png)

A csoport tulajdonosa is elérhetővé teheti, hogy a csoport kérelmekhez önkiszolgáló. Ezzel a végfelhasználó is kereshet és keresse meg a csoportot és kérelmének, az erőforrásokat, kezelhetők a csoporton keresztül eléréséhez szükséges engedély hatékony kérő. A csoport tulajdonosát a csoport beállíthatja, hogy a csatlakozási kérelmek automatikusan engedélyezett vagy jóvá kell hagynia tulajdonosát a csoport. Ha egy felhasználó csoport kérést, az illesztés kérelmet van továbbítani a tulajdonosok csoport. A tulajdonosok közül a hagyja jóvá a kérelmet, ha a kérő felhasználó értesítést kap, és a felhasználó csatlakozott a csoporthoz. Ha elutasítja a tulajdonosok közül, a kérő felhasználó értesítés ennek ellenére nem csatlakozott a csoporthoz.


## <a name="getting-started-with-access-management"></a>Első lépések a hozzáférés kezelése
Készen áll a kezdéshez? Próbálkozzon meg néhány az Azure Active Directory-csoportok végezhető alapvető műveleteket. Ezek a funkciók használatával a különböző erőforrásokat a szervezet különböző csoportokhoz speciális hozzáférést biztosítanak. Az első alaplépései listáját felsorolása olvasható.

* [Egy csoport dinamikus tagságot konfigurálása egyszerű szabály létrehozása](active-directory-accessmanagement-manage-groups.md#how-can-i-manage-the-membership-of-a-group-dynamically)

* [Csoport használata a szoftver alkalmazások való hozzáférés kezelése](active-directory-accessmanagement-group-saasapps.md)

* [Egy csoport elérhetővé tétele a végfelhasználói önkiszolgáló](active-directory-accessmanagement-self-service-group-management.md)

* [A helyszíni alkalmazás segítségével Azure AD Connect Azure szinkronizálása](active-directory-aadconnect.md)

* [A tulajdonosok csoport kezelése](active-directory-accessmanagement-managing-group-owners.md)


## <a name="next-steps-for-access-management"></a>Következő lépések az access-kezelés
Most, hogy az access-kezelés alapjai kell értelmezni, az alábbiakban néhány további szakértői funkciók érhető el az Azure Active Directory eléréséhez, az alkalmazások és az erőforrások kezelésére szolgáló.

* [Speciális szabályok létrehozása a attribútumok segítségével](active-directory-accessmanagement-groups-with-advanced-rules.md)

* [Az Azure Active Directory a biztonsági csoportok kezelése](active-directory-accessmanagement-manage-groups.md)

* [Azure AD a saját csoportok beállítása](active-directory-accessmanagement-dedicated-groups.md)

* [Csoportok Graph API részletes ismertetése](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#GroupFunctions)

* [Azure Active Directory-parancsmagjai csoport beállításainak konfigurálása](active-directory-accessmanagement-groups-settings-cmdlets.md)

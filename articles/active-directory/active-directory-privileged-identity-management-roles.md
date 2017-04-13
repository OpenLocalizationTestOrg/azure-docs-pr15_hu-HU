<properties
   pageTitle="A személyes Információkezelő szerepkörök |} Microsoft Azure"
   description="Megtudhatja, hogy milyen szerepkörök a Yammerhez Identitáskezelés Azure kiterjesztésű Yammerhez identitások segítségével."
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
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# <a name="roles-in-azure-ad-privileged-identity-management"></a>Azure Active Directory jogosultsággal rendelkező Identitáskezelés szerepkörök

<!-- **PLACEHOLDER: Need description of how this works. Azure PIM uses roles from MSODS objects.**-->

A szervezet különböző rendszergazdai szerepkörök az Azure Active Directory adhatnak a felhasználóknak. A szerepkör-hozzárendelés szabályozhatja, hogy mely tevékenységek hozzáadása, felhasználók eltávolítása vagy módosítása a szolgáltatás beállításai, például a felhasználók csak végre szeretne hajtani Azure AD az Office 365-ben, és más Microsoft Online Services és kapcsolódó alkalmazások.  

A globális rendszergazda frissítheti, mely felhasználók **véglegesen** Azure Active Directory, például a PowerShell-parancsmagok használata a szerepkörökhöz rendelt `Add-MsolRoleMember` és `Remove-MsolRoleMember`, vagy a [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory](active-directory-assign-admin-roles.md), a klasszikus portálon keresztül.

Azure Active Directory jogosultsággal rendelkező Identitáskezelés (személyes Információkezelő) jogosultsággal rendelkező felhasználók hozzáférésének házirendek az Azure Active Directory kezelése Személyes Információkezelő osztja ki a felhasználóknak egy vagy több szerepkörök az Azure Active Directory, és valaki végleges a szerepkört, vagy a szerepkör használatára jogosult rendelhet. Amikor a felhasználó végleges van rendelve egy szerepkört, vagy jogosult szerepkör-hozzárendelés aktiválása, majd azok is kezelése Azure Active Directory, az Office 365-ben és az egyéb alkalmazások a szerepkörökhöz rendelt engedélyeket tartalmazó.

Az access-jogosult szerepkör-hozzárendelés és állandó rendelkező megadott nincs különbség nem. Az egyetlen különbség, hogy bizonyos nincs szükségük a felhasználóknak, hogy mindig. Jogosult a szerep módosításai, és a következőképpen indíthatja el, és ki jelenik meg, ha van szükségük.

## <a name="roles-managed-in-pim"></a>A személyes Információkezelő felügyelt szerepkörök

Kiemelt Identitáskezelés lehetővé teszi a felhasználóknak hozzárendelése általános rendszergazdai szerepköreiről, például:


- **Globális rendszergazda** (más néven vállalati rendszergazdai) az összes felügyeleti funkcióinak hozzáférhet. A szervezet egynél több globális rendszergazda is. A személy, aki regisztrál az Office 365 automatikusan vásárlásához válik a globális rendszergazdáéival.
- **Rendszergazdai szerepkör jogosultsággal rendelkező** Azure Active Directory személyes Információkezelő kezeli, és más felhasználók szerepkör-hozzárendelések frissíti.  
- **Számlázási rendszergazdának** termékeket vásárol, kezeli az előfizetéseket, kezeli a támogatási jegyeket, és figyeli a szolgáltatás állapotát.
- **Jelszókezelő** jelszavak alaphelyzetbe állítása, kezeli a szolgáltatási kérelmeket, és figyeli a szolgáltatás állapotát. Felhasználó jelszavának alaphelyzetbe állítása az korlátozódik a jelszavát.
- **Szolgáltatás-rendszergazda** kezeli a szolgáltatási kérelmeket, és a szolgáltatás állapota képernyőn.

  > [AZURE.NOTE] Alkalmazás használatakor az Office 365-ben, majd a szolgáltatás rendszergazdai szerepkör hozzárendelése egy felhasználóhoz, előtt először rendelhet a felhasználó rendszergazdai engedélyekkel szolgáltatás, például az Exchange online-ban.

- **Felhasználókezelő rendszergazda** jelszavak alaphelyzetbe állítása, figyeli a szolgáltatás állapotát, és a felhasználói fiókokat, a felhasználói csoportok és a szolgáltatási kérelmeket kezeli. A Felhasználókezelő rendszergazda nem globális rendszergazda törlése, más rendszergazdai szerepkörök létrehozása és globális, számlázási és a szolgáltatás-rendszergazdák jelszavának visszaállítása.
- **Exchange-rendszergazda** az Exchange Online rendszergazdai hozzáférése van az Exchange felügyeleti központon keresztül, és feladatot végre tud hajtani szinte bármilyen az Exchange online-ban.
- **SharePoint-rendszergazdájához,** a SharePoint Online rendszergazdai hozzáférése van a SharePoint Online felügyeleti központon keresztül, és szinte bármilyen feladatot végezheti el a SharePoint online-ban.
- **Skype vállalati verziós rendszergazda** a Skype vállalati verzióhoz a Skype vállalati verzió felügyeleti központján keresztül rendszergazdai hozzáférése van, és végre tud hajtani szinte bármilyen feladatot a Skype vállalati online verzió.

Olvassa el az alábbi cikkekben olvashat a [rendszergazdai szerepkörök hozzárendelése Azure Active Directory](active-directory-assign-admin-roles.md) és a [rendszergazdai szerepkörök hozzárendelése az Office 365-ben](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504).

<!--**PLACEHOLDER: The above article may not be the one we want since PIM gets roles from places other that Office 365**-->


A személyes Információkezelő azt is megteheti [hozzárendelése egy felhasználóhoz, a szerepkörök](active-directory-privileged-identity-management-how-to-add-role-to-user.md) , hogy a felhasználó lehet [aktiválni a szerepkört, szükség esetén](active-directory-privileged-identity-management-how-to-activate-role.md).

Szeretne adni egy másik felhasználó hozzáférését kezelheti a személyes Információkezelő magát, a szerepkörök, amely a személyes Információkezelő igényel, hogy a felhasználó esetén a, [hogy miként adhat hozzáférést a személyes Információkezelő](active-directory-privileged-identity-management-how-to-give-access-to-pim.md)további ismerteti.


<!-- ## The PIM Security Administrator Role **PLACEHOLDER: Need description of the Security Administrator role.**-->

## <a name="roles-not-managed-in-pim"></a>A személyes Információkezelő nem felügyelt szerepkörök

Szerepkörök belül az Exchange Online vagy a SharePoint online-ban, kivéve a fent említett nem jelennek meg az Azure Active Directory, és így nem jelennek meg a személyes Információkezelő. Az alábbi Office 365-szolgáltatások külön szerepkör-hozzárendelések módosításával kapcsolatos további tudnivalókért lásd: az [engedélyek az Office 365-ben](https://support.office.com/article/Permissions-in-Office-365-da585eea-f576-4f55-a1e0-87090b6aaa9d).

Azure előfizetések és erőforrás-csoportokat is nem jelennek meg az Azure Active Directory. Azure előfizetések kezelése, megtudhatja, [hogy miként hozzáadásához vagy módosításához az Azure rendszergazdai szerepkörök](../billing-add-change-azure-subscription-administrator.md) és Azure RBAC bővebben lásd: a [Hozzáférés-vezérlés Azure Role-Based](role-based-access-control-configure.md).

<!--**The above links might be replaced by ones that are from within this documentation repository **-->


## <a name="user-roles-and-signing-in"></a>Felhasználói szerepkörök elemre, és bejelentkezés
Az egyes Microsoft-szolgáltatások és alkalmazások hozzárendelése egy felhasználóhoz szerepkörbe nem lehet elegendő, hogy a felhasználó rendszergazdája lesz.

Az Azure klasszikus portal használatához a felhasználónak, ha a szolgáltatás-rendszergazda vagy közös rendszergazda Azure-előfizetéshez, még akkor is, ha a felhasználó nem szükséges az Azure előfizetések kezelése.  Azure AD a klasszikus portálon kezelheti a konfigurációs beállítások, például a felhasználó kell az Azure Active Directory globális rendszergazdának, mind a előfizetés közös rendszergazdája Azure-előfizetéshez.  Megtudhatja, hogy miként vehet fel felhasználókat az Azure előfizetések, megtudhatja, [hogy miként hozzáadásához vagy módosításához az Azure rendszergazdai szerepkörök](../billing-add-change-azure-subscription-administrator.md).

Microsoft Online Services hozzáférést szükség lehet a felhasználó is kiosztani licenc nyissa meg a szolgáltatás portált vagy felügyeleti feladatok elvégzése előtt.

## <a name="assign-a-license-to-a-user-in-azure-ad"></a>Licenc hozzárendelése egy felhasználóhoz az Azure Active Directory

1. Jelentkezzen be a [Azure klasszikus portálra] (http://manage.windowsazure.com) egy globális rendszergazdai fiókot vagy egy közös rendszergazdai fiókkal.
2. A fő menüből válassza a **Minden elem** .
3. Jelölje ki a kívánt használata könyvtárat és licencek társítva van.
4. Válassza a **licencek**lehetőséget. A rendelkezésre álló licencek számának listáját fog megjelenni.
5. Jelölje ki a licenc használ, amely tartalmazza a terjeszteni kívánt licenceket.
6. Jelölje ki a **hozzárendelését a felhasználókhoz**.
7. Jelölje ki a felhasználót, hogy meg szeretné rendelni a licencet.
8. Kattintson a **hozzárendelése** gombra.  A felhasználó is most jelentkezzen be az Azure.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Következő lépések
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

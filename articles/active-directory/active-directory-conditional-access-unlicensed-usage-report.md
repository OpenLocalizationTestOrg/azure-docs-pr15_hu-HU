<properties
    pageTitle="Licenccel nem rendelkező használati jelentés |} Microsoft Azure"
    description="A licenccel nem rendelkező használati jelentés segít azonosíthatja a licenccel nem rendelkező felhasználók által használt fizetett Azure AD-szolgáltatásokat."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="unlicensed-usage-report"></a>Licenccel nem rendelkező használati jelentés

A licenccel nem rendelkező használati jelentés segít azonosíthatja a licenccel nem rendelkező felhasználók által használt kifizetett Azure AD-szolgáltatásokat. Ez lehetővé teszi, hogy jobban használja, hogy a vásárolt licencek, és azonosítása tudja, hogy ha előfordulhat, hogy további licenceket. 

A jelentés a kifizetett funkciók aktív használatát az elmúlt 30 nap jeleníti meg. 

## <a name="report-structure"></a>Jelentés-struktúra
 
| Oszlopnév          |    Leírás |
| :--                  | :--         |
| Licenccel nem rendelkező felhasználók      |    A felhasználó neve |
| A szolgáltatás              | A szolgáltatás neve. Példa: feltételes hozzáférés |
| Alkalmazás érhető el | Az alkalmazás a szolgáltatással használt neve. Példa: az Office 365 SharePoint online-ban |

 
> [AZURE.NOTE] Ha egy felhasználó fiók törlése után a "Nem licencelt felhasználó" oszlop tölti fel, például 1003000090D8B285 azonosító


## <a name="conditional-access-feature"></a>Feltételes hozzáférést biztosító funkció

Licenccel nem rendelkező felhasználók program megjelöli, feltételes hozzáférési házirend alkalmazza, ha az Azure Active Directory prémium licenccel nem rendelkező tartalmazó szolgáltatás elérésekor kérni. 

Ez a cikk MFA / hely házirendek, valamint az eszköz Intune használó házirendeket.
 

## <a name="see-also"></a>Lásd még:

- [Feltételes Access használata az Office 365 és az egyéb Azure Active Directory kapcsolódó alkalmazások](active-directory-conditional-access.md)
- [Azure Active Directory feltételes hozzáférést – első lépések](active-directory-conditional-access-azuread-connected-apps.md) 



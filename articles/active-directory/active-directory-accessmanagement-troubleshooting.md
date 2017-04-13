
<properties
    pageTitle="Dinamikus tagság-csoportok hibaelhárítási |} Microsoft Azure"
    description="Hibaelhárítási tippeket az Azure Active Directory-csoportok dinamikus tagságot."
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


# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Hibaelhárítási dinamikus tagságot-csoportok

**Szabály beállítása csoportban, de nincs tagság frissülnek a csoportban**<br/>Győződjön meg arról **, hogy az **Engedélyezés delegált felügyeleti csoport** beállítása Igen értékre** van állítva a **beállítás** lapon. Ez a beállítás csak akkor, ha be van jelentkezve a be olyan felhasználóként, akinek az Azure Active Directory prémium licenc van-e hozzárendelve jelenik meg. A szabály felhasználói attribútumok értékeinek ellenőrzése: vannak olyan felhasználók, amelyek megfelelnek a szabály?

**E konfigurálva, hogy egy szabályt, de most a szabály a meglévő tagok eltávolítása**<br/>Ez a elvárt működés. A csoport tagjainak meglévő törlődnek szabály engedélyezve van-e megváltozott. A kiértékelés a szabály – által eredményül adott hozzá lesznek adva tagként a csoporthoz.     

**Nem látom a tagság változik instantly hozzáadása vagy egy szabályt, miért nem módosítása esetén?**<br/>Saját tagság kiértékelés rendszeres befejeződött egy aszinkron háttér folyamat. Mennyi ideig tart a folyamat határozza meg a csoport a szabály eredményeként létrejövő méretének és a felhasználó a címtárban száma. Általában a felhasználók kis számokkal könyvtárak jelenik meg a csoport tagságát módosításokat kisebb, mint néhány perc alatt. Nagy számú felhasználó könyvtárak 30 percig is eltarthat vagy hosszú kitöltéséhez.

Ezek a cikkek Azure Active Directory további tájékoztatást nyújt.

* [Erőforrások való hozzáférés kezelése az Azure Active Directory-csoportok](active-directory-manage-groups.md)
* [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)
* [Mi az Azure Active Directory?](active-directory-whatis.md)
* [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)

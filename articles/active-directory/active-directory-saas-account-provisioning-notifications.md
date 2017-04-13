<properties
    pageTitle="Fiókok kiépítésének értesítések |} Microsoft Azure"
    description="Megtudhatja, hogy miként győződjön meg arról, hogy értesítést küld a problémák megoldásához kiépítési felhasználói fiókok kiépítésének értesítések engedélyezésével a figyelmet igénylő."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="account-provisioning-notifications"></a>Fiókbeállítások a kiépítési értesítések

A felhasználó kiépítési, automatizálhatja a folyamaton, szoftver alkalmazásban a külső felhasználók kezelése. <br>
Amikor egy automatizált eljárással, a folyamathoz interakció időről-időre szükség. <br>
Az, például az esetben, ha az adatok és a harmadik exchange állította be a fiók jelszavát fél szoftver alkalmazás lejárt. 

Fiókok kiépítésének értesítések engedélyezésével biztosíthatja, hogy a felhasználó kiépítési kapcsolatos problémákat, a figyelmet igénylő értesítés.

Aktiválása vagy inaktiválása fiókok kiépítésének értesítések a felhasználó konfigurációja egy harmadik fél szoftver alkalmazás kiépítési részeként.

![Felhasználói kiépítése][1] 



Fiókok kiépítésének értesítések aktiválásához jelölje be a **megerősítést kérő** párbeszédpanel lapon a kapcsolódó jelölőnégyzetet, és írja be a címzett az e-mail alias.

![Fiókbeállítások a kiépítési értesítések][2]
 


Terjesztési lista; címzettként is megadhat azonban fontos tudni, hogy az értesítő e-mailt, amelyek csak az Azure Active Directory-rendszergazdák által elérhető jelentések hivatkozásokat tartalmaz.

Ha fiókok kiépítésének értesítések engedélyezve, kap e-mailek kapcsolatos fontos problémákat, amelyek felhasználó létesítése való. Jó helyen jár e-mailek túlterhelés elkerülése érdekében csak kap egy értesítési e-mail az értesítő e-mailt engedélyezve van a napi minden szoftver alkalmazáshoz.


##<a name="related-articles"></a>Kapcsolódó cikkek

- [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)
- [Automatizálhatja a felhasználói kiépítési/megszüntetés szoftver-alkalmazás](active-directory-saas-app-provisioning.md)
- [A felhasználó kiépítéséhez attribútum hozzárendelések testreszabása](active-directory-saas-customizing-attribute-mappings.md)
- [Az attribútum hozzárendelések kifejezések írása](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Hatókör meghatározása felhasználói kiépítési szűrői](active-directory-saas-scoping-filters.md)
- [Engedélyezése a felhasználók és csoportok az Azure Active Directory alkalmazások automatikus kiépítési SCIM használatával](active-directory-scim-provisioning.md)
- [Integrálhatja a szoftver alkalmazások ismertető oktatóanyagok listája](active-directory-saas-tutorial-list.md)



<!--Image references-->
[1]: ./media/active-directory-saas-account-provisioning-notifications/ic766307.png
[2]: ./media/active-directory-saas-account-provisioning-notifications/ic766308.png
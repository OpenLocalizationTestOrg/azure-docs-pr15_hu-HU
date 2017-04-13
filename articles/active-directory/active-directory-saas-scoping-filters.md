<properties
    pageTitle="Hatókör meghatározása a szűrőket a kiépítési attribútum-alapú alkalmazás |} Microsoft Azure"
    description="Megtudhatja, hogy miként megakadályozhatja, hogy a ténylegesen, ha egy objektum nem felel meg a vállalati kiépítve kiépítési automatizált felhasználói támogató objektumainak tartalmazó szűrők használatával."
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


# <a name="attribute-based-app-provisioning-with-scoping-filters"></a>Hatókör meghatározása a szűrőket a kiépítési attribútum-alapú alkalmazás

Ez a szakasz célja tartalmazó szűrők használatával attribútum szabályok, amely meghatározza, hogy mely felhasználók fog kell építenie az alkalmazás definiálása ismertetik.





## <a name="clauses-and-scope-groups"></a>Záradékok és a hatókör-csoportok


![Hatókör meghatározása szűrő][1] 




Egy vagy több **hatókör csoportok**, amelyek tartsa lenyomva az ujját egy vagy több **záradékot**tartalmazó szűrők határozza meg. A záradékok szerepelnek, egy adott hatókör csoport megtekintéséhez bontsa ki a csoport nevét, a balra lévő nyílra kattintva.

Egy **záradék** határozza meg, hogy mely felhasználók szabad kiértékelése minden felhasználó attribútumokat tartalmazó szűrő haladnia. Ha például lehet, hogy egy záradékot, amely szükségessé teszi, hogy a felhasználó "állapot" attribútum egyenlő New York, ami azt jelenti, hogy csak a New York felhasználók az alkalmazásba kell építenie.

![Hatókör meghatározása a csoport neve][2] 



Minden **hatókör csoport** kezdődik egy kötelező **záradék**, ahogy a fenti képernyőképet. Ez a záradék egyszerűen tájékoztat, hogy a felhasználó először meg kell adni az alkalmazás előtt kiértékeli a a tartalmazó szűrőket. Ez a záradék nem törölhető, illetve módosítani.

A megfelelő gombra kattintva hozzáadhatja az új záradékok vagy új hatókör-csoportokat. Minden hatókör csoport egy nevet adhat a **Hatókör csoport neve** tulajdonságának szerkesztésével.





## <a name="how-scoping-filters-are-evaluated"></a>Hatókör meghatározása szűrők kiértékelése hogyan

Során kiépítési, tesztelje azt határozza meg, ha a felhasználó számára az alkalmazás elérhetőségét érdemel szemben a tartalmazó szűrők minden hozzárendelt felhasználó. Érdemes elképzelnie, hogy egy tesztcélú, át kell adni ahhoz, hogy a felhasználó első kiszűrte elkerülése érdekében minden záradék. 

Ha több hatókör csoport meghatározás, minden felhasználónak meg kell felelnie legalább az egyik annak érdekében, hogy az alkalmazás elérése. Minden egyes hatókör csoporton belül azonban a felhasználónak meg kell felelnie minden egyetlen záradék annak érdekében, hogy adják át, hogy bizonyos keresési tartomány csoportban. 

Más szóval, érdemes elképzelnie, hogy a hatókör csoportok közös tenné, illetve érdemes elképzelnie, hogy a bennük lévő záradékok és közös tenné. Például érdemes megvizsgálni a tartalmazó szűrő az alábbi:


![Hatókör meghatározása a csoport neve][2]  


A tartalmazó szűrő szerint felhasználók meg kell felelnie a az alábbi követelményeket, annak érdekében, hogy kiépítése:

1. Azok az alkalmazás kell rendelni.

2. Kattintson a tervezés részleg kell működnek

3. Munka kell lenniük a San Francisco vagy a Kanada.


##<a name="related-articles"></a>Kapcsolódó cikkek

- [Az alkalmazások kezelése az Azure Active Directory cikk indexben](active-directory-apps-index.md)
- [Felhasználói kiépítési, és a szoftver alkalmazások megszüntetés automatizálása](active-directory-saas-app-provisioning.md)
- [A felhasználó kiépítéséhez attribútum hozzárendelések testreszabása](active-directory-saas-customizing-attribute-mappings.md)
- [Az attribútum hozzárendelések kifejezések írása](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Fiókbeállítások a kiépítési értesítések](active-directory-saas-account-provisioning-notifications.md)
- [Engedélyezése a felhasználók és csoportok Azure Active Directory-alkalmazások automatikus kiépítési SCIM használatával](active-directory-scim-provisioning.md)
- [A szoftver alkalmazások integrálása ismertető oktatóanyagok listájában](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-scoping-filters/ic782811.png
[2]: ./media/active-directory-saas-scoping-filters/ic782812.png
[3]: ./active-directory-saas-scoping-filters/ic782813.png

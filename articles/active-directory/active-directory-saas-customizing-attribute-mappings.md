<properties
    pageTitle="Attribútum hozzárendelések testreszabása |} Microsoft Azure"
    description="Megtudhatja, hogy milyen attribútum hozzárendelések-szoftver alkalmazást az Azure Active Directory hogyan módosíthatja azokat az üzleti igények címet."
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


# <a name="customizing-attribute-mappings"></a>Attribútum megfeleltetésének testreszabása


Microsoft Azure Active Directory felhasználói kiépítési külső szoftver alkalmazások például Salesforce, a Google Apps és mások számára nyújt támogatást. Ha egy harmadik fél szoftver alkalmazás engedélyezve van a felhasználó kiépítési, a az Azure adatkezelési portál szabályozza az attribútum értékeit a "attribútum megfeleltetés" nevű beállítások képernyőn.

Van egy olyan előre beállított attribútum hozzárendelések és Azure Active Directory felhasználói minden szoftver alkalmazás felhasználói objektumok között. Egyes alkalmazások más típusú objektumok, például csoportok vagy partnerek kezelése. <br> 
Az üzleti igényeinek megfelelően testre szabhatja attribútum alapértelmezett megfeleltetések. Ez azt jelenti, módosítása vagy törlése a meglévő attribútum leképezést vagy hozzon létre új attribútum hozzárendeléseket.

Az Azure Active Directory-portálon is elérheti az Ez a funkció, attribútumok szoftver alkalmazás eszköztár.

> [AZURE.NOTE] Az **attribútumok** hivatkozás csak akkor használható, ha a felhasználó kiépítési szoftver alkalmazáshoz engedélyezve van. 


![Salesforce][1] 


Ha attribútumok kattint az eszköztár, a szoftver alkalmazáshoz konfigurált aktuális hozzárendelések listáját.

Az alábbi képernyőképen Ez egy példa látható:



![Salesforce][2]  


A fenti példában látható, hogy az **Utónév** attribútum egy felügyelt Salesforce-objektum kitölti a csatolt **givenName** értékének Azure AD-objektumot.

Ha szeretné szabni a attribútum hozzárendelések vagy ha vissza szeretné állítani a testreszabott vissza az alapértelmezett beállítás a beállítások, végezze el ezt az alkalmazások alján lévő eszköztár a kapcsolódó gombra kattintva.


![Salesforce][3]  


Vannak olyan attribútum hozzárendelések a szoftver alkalmazás megfelelő működéséhez szükséges. Az attribútumok táblában kapcsolódó attribútum megfeleltetések van **Igen** érték **kötelező** attribútum. Ha egy attribútum-hozzárendelés szükség, nem törölhetők. Ebben az esetben **törölje** a funkció nem érhető el.

Ha módosítani szeretné egy már meglévő attribútum hozzárendelést, csak kattintson a megfeleltetés, és kattintson a **szerkesztése**gombra. Ekkor megjelenik egy párbeszédpanel lap, amely lehetővé teszi, hogy a kijelölt attribútum hozzárendelés módosítása.


![Attribútum megfeleltetés szerkesztése][4]  



## <a name="understanding-attribute-mapping-types"></a>Attribútum típusainak megfeleltetése ismertetése


Attribútum megfeleltetések, a vezérlő, hogyan attribútumok vannak töltve, egy harmadik fél szoftver alkalmazást. Támogatott négy különböző hozzárendelési típusa létezik:

- **Közvetlen** – a cél attribútum kitölti a csatolt objektumot a tulajdonság értékét az Azure Active Directory.


- **Állandó** – a cél attribútum kitölti egy adott karakterlánc megadott.


- **Kifejezés** – a cél attribútum van töltve a parancsprogram-szerű kifejezés alapján. További részletekért olvassa el [Írni a kifejezéseket attribútum-hozzárendelések az Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).


- **Nincs** - a cél attribútum balra változatlanul van. Jó helyen jár Ha a cél attribútumot minden eddiginél üres, akkor tölti fel az alapértelmezett érték, megadott feltételeknek.



Kívül az alábbi négy alapvető attribútum megfeleltetés típusát egyéni attribútum hozzárendelések támogatja az **alapértelmezett** érték-hozzárendelés fogalmának. Az alapértelmezett érték-hozzárendelés biztosítja, hogy egy-attribútumhoz töltve érték, ha nincs sem egy értéket az Azure Active Directory és nem a cél objektum.

Microsoft Azure Active Directory szinkronizál egy nagyon hatékony végrehajtásának biztosít. Csak azokat az objektumokat igénylő frissítések inicializált környezetben, a szinkronizálási ciklusban feldolgozása. Attribútum hozzárendelések frissítése hatással van a szinkronizálás ciklus teljesítményét. Ennek oka az, a attribútum hozzárendelési konfigurációt frissítésére van szüksége az összes felügyelt objektumok kell reevaluated igényel. Emiatt az ajánlott a legjobb ahhoz, hogy a attribútum hozzárendelések legalább egymást követő módosítások számát.


##<a name="related-articles"></a>Kapcsolódó cikkek

- [Az alkalmazások kezelése az Azure Active Directory cikk indexben](active-directory-apps-index.md)
- [Automatizálhatja a felhasználói kiépítési/megszüntetés szoftver-alkalmazás](active-directory-saas-app-provisioning.md)
- [Az attribútum hozzárendelések kifejezések írása](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Hatókör meghatározása felhasználói kiépítési szűrői](active-directory-saas-scoping-filters.md)
- [Engedélyezése a felhasználók és csoportok Azure Active Directory-alkalmazások automatikus kiépítési SCIM használatával](active-directory-scim-provisioning.md)
- [Fiókbeállítások a kiépítési értesítések](active-directory-saas-account-provisioning-notifications.md)
- [A szoftver alkalmazások integrálása ismertető oktatóanyagok listájában](active-directory-saas-tutorial-list.md)


<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png

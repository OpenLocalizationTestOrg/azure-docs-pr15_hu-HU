<properties
    pageTitle="Az Azure Active Directory kiépítési szoftver alkalmazás felhasználói automatikus |} Microsoft Azure"
    description="Hogyan használható az Azure Active Directory hozhatók létre automatikusan, bemutatása vonja kiépítése, és folyamatosan frissíti a felhasználói fiókok több külső szoftver alkalmazásban."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Felhasználói kiépítési, és a szoftver-alkalmazások és az Azure Active Directory megszüntetés automatizálása

##<a name="what-is-automated-user-provisioning-for-saas-apps"></a>Mi az felhasználói kiépítési automatikus-szoftver alkalmazások?

Azure Active Directory (Azure Active Directory) lehetővé teszi, hogy automatizálhatók létrehozását, karbantartási és felhasználói identitások felhő ([szoftver](https://azure.microsoft.com/overview/what-is-saas/)) alkalmazásokban, például a Dropbox, Salesforce, ServiceNow és az egyéb eltávolítását.

**Alul néhány példát a Mi ez a szolgáltatás lehetővé teszi láthatók:**

- Fiókok automatikus létrehozása új megfelelő szoftver-alkalmazások új felhasználók, ha azokat a csatlakozás a csoporthoz.
- Automatikusan inaktiválja a fiókok szoftver alkalmazásokból, amikor személyek elkerülhetetlenül hagyja a csapat.
- Győződjön meg arról, hogy az identitás-szoftver alkalmazások vannak naprakészek a módosításokat a címtárban alapján.
- Építse nem felhasználói objektumok, például a csoportok, szoftver-alkalmazás, amely támogatja az őket.

**Automatikus felhasználói kiépítési, az alábbi szolgáltatásokat is tartalmazza:**

- Az azt jelenti, hogy egyeznie a meglévő azonosítók között Azure Active Directory és a szoftver alkalmazások.
- Súgó: Azure AD a testreszabási lehetőségek szoftver alkalmazások, amely jelenleg a szervezete használja az aktuális konfigurációk illeszkedik.
- Nem kötelező e-mailben értesítéseket, az áttelepítési hibákkal.
- Jelentések és a tevékenység naplók figyelése és hibaelhárítási segítséget.

##<a name="why-use-automated-provisioning"></a>Miért érdemes használni az automatizált kiépítési?

Néhány gyakori összefüggések Ez a szolgáltatás használatához a következők:

- A költségek, hatékonyság hiánya és folyamatok kiépítési kézi társított emberi hibák elkerülése érdekében.
- A szervezet biztonságos azonnali eltávolítása felhasználói identitások kulcs szoftver-alkalmazásokból, ha elhagyják a szervezet által.
- Több felhasználó tömeges egyszerűen importálja egy adott szoftver alkalmazás.
- Ha az kényelme mellett használják, hogy futtassa az Azure Active Directory egyszeri bejelentkezés a megadott azonos alkalmazás hozzáférést házirendek elhagyja a kiépítési megoldás.

##<a name="frequently-asked-questions"></a>Gyakori kérdések

**Milyen gyakran nem írása Azure Active Directory directory változásait a szoftver alkalmazásba?**

Azure Active Directory 5 és 10 percenként ellenőrizze a módosításokat. A szoftver alkalmazás (például a kis-és érvénytelen rendszergazdai hitelesítő adatok ahogy) számos hibák ad vissza, ha majd Azure Active Directory fokozatosan lassabb lesz a gyakoriság, az mindaddig, amíg a hibák javított naponta egyszer felfelé.

**Mennyi ideig tart a felhasználók létrehozására az?**

Növekményes módosítások majdnem azonnali fordulhat elő, de ha címtárában a legtöbb hozhatók létre, majd azt attól függ, hogy a megadott felhasználók és csoportok, amelyekhez. Kis könyvtárak a csak néhány percig tart, közepes méretű könyvtárak eltarthat néhány percig, és a nagyon nagy könyvtárak több óráig is eltarthat.

**Hogyan tudom nyomon követheti az aktuális kiépítési feladat?**

Ellenőrizheti, hogy a partner kiépítési jelentés csoportban a címtárában jelentések részét. Egy másik, hogy keresse fel az irányítópult lap, hogy kiépítési vannak, és keresse meg a "Integráció állapotát" szakasz a lap alján a szoftver alkalmazáshoz.

**Hogyan fog állapíthatom meg, hogy ha a felhasználók nem megfelelően első kiépítéstől?**

Végén található a kiépítési konfiguráció varázsló van lehetőség a hibák kiépítéséhez értesítő e-mailek előfizetéséhez. Érdemes is a kiépítési hibák jelentést, és ellenőrizze, hogy mely felhasználók kell építenie nem sikerült, és miért.

**Azure Active Directory írhat módosításokat a szoftver alkalmazásból vissza a címtárhoz?**

A legtöbb szoftver alkalmazásokat, a kiépítési csak kimenő, ami azt jelenti, hogy a felhasználók kerülnek a címtárból az alkalmazás, és módosításokat az alkalmazásból vissza nem írható a címtárhoz. A [kalk.munkanap](https://msdn.microsoft.com/library/azure/dn762434.aspx)azonban kiépítési csak bejövő, ami azt jelenti, hogy a felhasználók importálta munkanap könyvtárból az alkalmazásba, és hasonlóan a címtárban módosítások nem első bejegyzi vissza kalk.munkanap.

**Hogyan küldhet visszajelzést a mérnöki csapatnak?**

Kérjük, forduljon nekünk az [Azure Active Directory Visszajelzési fórum](https://feedback.azure.com/forums/169401-azure-active-directory/)keresztül.

##<a name="how-does-automated-provisioning-work"></a>Honnan tudja az automatizált létesítési munkát?

Azure AD a felhasználóknak, hogy a szoftver alkalmazások kiépítése minden alkalmazás szállító által biztosított végpontok kiépítési összekapcsolásával. Ezeket a végpontokat lehetővé teszi az Azure Active Directory programozás útján létrehozása, frissítése és távolíthat el felhasználókat. Az alábbi van, amely Azure Active Directory automatizálhatja a kiépítési különféle lépések rövid áttekintését.

1. Ha engedélyezi, hogy az alkalmazás első indítását kiépítési, az alábbi műveleteket kell végrehajtani:
 - Azure Active Directory megpróbálja felel meg a megfelelő felhasználókkal a címtárban a szoftver alkalmazásban bármelyik meglévő felhasználóknak. Amikor a felhasználó megfeleltetett, hogy azok az egyszeri bejelentkezési *nem* automatikusan engedélyezett. Ahhoz, hogy a felhasználó hozzáférjen az alkalmazást azok kell kifejezetten rendelni az alkalmazást az Azure Active Directory, vagy közvetlenül a csoporttagság keresztül.
 - Ha már adta meg, amelyet a felhasználók kell-e kiosztani az alkalmazás, és ha Azure Active Directory nem sikerül egy meglévő fiókok található azoknak a felhasználóknak, Azure Active Directory fog kiépítése új fiók őket az alkalmazás.
2. A kezdeti szinkronizálás fentebb ismertetett befejeződött, miután Azure Active Directory ellenőrzi a következő módosításokat 10 percenként:
 - Ha az alkalmazás új felhasználók vannak rendelve (közvetlenül vagy csoporttagság), majd ezek lesznek a szoftver alkalmazást az új fiók kiépítve.
 - Ha a felhasználó hozzáférését megszűnt, majd a szoftver alkalmazásban fiókjukat lesz megjelölve letiltása (felhasználók teljesen soha nem törlődnek, amely megakadályozza, hogy az adatok elvesztését megelőzve helytelen beállítása).
 - Ha egy felhasználó nemrégiben van beállítva az alkalmazás és a szoftver alkalmazásban már volt egy fiókot, a fiók lesz megjelölve engedélyezve van, és bizonyos felhasználó tulajdonságainak frissülhetnek, ha elavult a könyvtár és összehasonlítása.
 - Ha a felhasználó adatai (például a telefonszám, irodájának helyét, stb.) a címtárban módosult, majd ezeket az információkat is frissülnek a szoftver alkalmazásban.

További információt a hogyan attribútumok Azure Active Directory között vannak megfeleltetve, illetve a szoftver a cikke [Attribútum hozzárendelések testreszabása](active-directory-saas-customizing-attribute-mappings.md).

##<a name="list-of-apps-that-support-automated-user-provisioning"></a>Automatikus felhasználói kiépítési támogató lista

Kattintson az alkalmazás oktatóanyagot nézze meg, az automatikus kiépítési konfigurálása:

- [Mező](http://go.microsoft.com/fwlink/?LinkId=286016)
- [Citrix GoToMeeting](http://go.microsoft.com/fwlink/?LinkId=309580)
- [Hajózásához](http://go.microsoft.com/fwlink/?LinkId=309575)
- [Docusign](http://go.microsoft.com/fwlink/?LinkId=403254)
- [Dropbox vállalati verziójával](http://go.microsoft.com/fwlink/?LinkId=309581)
- [A Google Apps](http://go.microsoft.com/fwlink/?LinkId=309577)
- [Jive](http://go.microsoft.com/fwlink/?LinkId=309591)
- [Salesforce](http://go.microsoft.com/fwlink/?LinkId=286017)
- [Salesforce védőfalas](http://go.microsoft.com/fwlink/?LinkId=327869)
- [ServiceNow](http://go.microsoft.com/fwlink/?LinkId=309587)
- [Kalk.munkanap](http://go.microsoft.com/fwlink/?LinkId=690250) (a bejövő kiépítési)

Ahhoz, hogy az alkalmazások automatizált felhasználói kiépítése támogatására meg kell adnia a szükséges végpontok, amelyek lehetővé teszik automatizálhatja a létrehozási, a karbantartás és a felhasználók eltávolítása a külső programokat. Emiatt nem az összes szoftver is ezzel a szolgáltatással kompatibilis. Az ezt támogató alkalmazásokat az Azure Active Directory mérnöki csoport így tudják össze egy kiépítési összekötőre, ezeket az alkalmazásokat, és ezt a munkát a jelenlegi és leendő ügyfelek igényeitől van rangsorolt.

Csapattól az Azure Active Directory mérnöki kérhet további alkalmazások kiépítési támogatását, küldjön egy üzenetet az [Azure Active Directory Visszajelzési fórum](https://feedback.azure.com/forums/169401-azure-active-directory/)keresztül.

##<a name="related-articles"></a>Kapcsolódó cikkek

- [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)
- [A felhasználó kiépítéséhez attribútum hozzárendelések testreszabása](active-directory-saas-customizing-attribute-mappings.md)
- [Attribútum hozzárendelések kifejezéseket írása](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Hatókör meghatározása felhasználói kiépítési szűrői](active-directory-saas-scoping-filters.md)
- [Engedélyezése a felhasználók és csoportok az Azure Active Directory alkalmazások automatikus kiépítési SCIM használatával](active-directory-scim-provisioning.md)
- [Fiókbeállítások a kiépítési értesítések](active-directory-saas-account-provisioning-notifications.md)
- [Integrálhatja a szoftver alkalmazások ismertető oktatóanyagok listája](active-directory-saas-tutorial-list.md)

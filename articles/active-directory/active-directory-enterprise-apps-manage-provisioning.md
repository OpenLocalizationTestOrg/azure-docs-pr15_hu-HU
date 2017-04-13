<properties
    pageTitle="Az Azure Active Directory előnézetben vállalati alkalmazások kezelésének kiépítési felhasználói |} Microsoft Azure"
    description="Megtudhatja, hogy miként kezelheti a felhasználói fiók kiépítési a vállalati alkalmazások használata az Azure Active Directory előzetes verzió"
    services="active-directory"
    documentationCenter=""
    authors="asmalser"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/12/2016"
    ms.author="asmalser"/>

#<a name="preview-managing-user-account-provisioning-for-enterprise-apps-in-the-new-azure-portal"></a>Előzetes verziója esetén: A vállalati alkalmazások az új Azure-portálon kiépítési felhasználói fiók kezelése

Ez a cikk ismerteti, hogyan kiépítési, és azt, különösen szegélyei van hozzáadva a "kiemelt" kategóriából az [Azure Active Directory-alkalmazás gyűjtemény](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery)támogató alkalmazások vonja kiépítési automatikus felhasználói fiókjának kezelése az [Azure portal](https://portal.azure.com) segítségével. A adatkezelési folyamatok az új Azure portálon jelenleg nyilvános előzetes verzióban, és ez a cikk ismerteti az új szolgáltatásokat, valamint a néhány ideiglenes korlátozások, amelyek helyen, a betekintő időszak alatt. [Mi az a előzetes verzióban?](active-directory-preview-explainer.md)

További tudnivalók az automatikus felhasználói fiók kiépítési és működéséről, lásd: [automatizálhatja a felhasználói kiépítési és Deprovisioning szoftver-alkalmazások és az Azure Active Directory](active-directory-saas-app-provisioning.md).

##<a name="finding-your-apps-in-the-new-portal"></a>Az alkalmazások keresése az új portálon

Szeptember 2016, kezdve egyetlen konfigurált összes alkalmazás bejelentkezéses az [Azure Active Directory-alkalmazás gyűjtemény](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) belül az [Azure klasszikus portal](https://manage.windowsazure.com)segítségével a címtár-rendszergazda által könyvtárában található, most megnézheti és az új Azure portálon felügyelt.

Ezeket az alkalmazásokat az új, a **További szolgáltatások** menüben, a bal oldali navigációs területen is elérhető Azure portál **Vállalati alkalmazások** szakaszában találhatók. Vállalati számítógépekre telepített és a szervezeten belüli felhasználók által használt alkalmazások is.

![Vállalati alkalmazások lap][0]

A **minden alkalmazás** hivatkozásra kattint, a bal oldali jeleníti meg a beállított, beleértve az alkalmazások, a gyűjteményből felvett összes alkalmazás listáját. Alkalmazás kijelölése betölti az erőforrás lap, hogy-alkalmazásokból, ahol jelentések megtekintheti, hogy az alkalmazás és a beállítások számos kezelhető.

Felhasználói fiók beállításainak kiépítési kezelhető a bal oldali **létesítése** kiválasztásával.

![Alkalmazás az erőforrás lap][1]


##<a name="provisioning-modes"></a>A kiépítési módok

A **létesítése** lap **mód** menüt, amely azt mutatja, hogy milyen kiépítési módok használhatók vállalati alkalmazások, kezdődik, és lehetővé teszi, hogy konfigurálni kell őket. A rendelkezésre álló beállítások a következők:

* Az **automatikus** – Ez a beállítás jelenik meg, ha Azure AD automatikus API-alapú kiépítési és/vagy vonja ennek az alkalmazásnak a felhasználói fiókok kiépítésének támogatja. Ebben az üzemmódban kijelölésekor megjelenik a felületet, amely végigvezeti a rendszergazdák keresztül csatlakozhat az alkalmazás felhasználókezelés API Azure Active Directory konfigurálása, a fiók-hozzárendelések és, amelyek meghatározzák, hogy hogyan felhasználói fiókot kell adatfolyam közötti Azure Active Directory munkafolyamatok létrehozása és az alkalmazás, illetve az Azure Active Directory szolgáltatás kiépítési.

* **Kézi** – Ez a beállítás jelenik meg, ha Azure Active Directory nem támogatja a felhasználói fiókok ennek az alkalmazásnak automatikus kiépítési. Ez a beállítás, az azt jelenti, hogy az alkalmazásban tárolt felhasználói ügyfélrekordok csak kezelhető egy külső folyamat, az adott alkalmazás (amelyek tartalmazhatnak SAML programjának dinamikus fordítója kiépítési) által biztosított felhasználói kezelési és a kiépítési funkciók alapján.


##<a name="configuring-automatic-user-account-provisioning"></a>Az automatikus felhasználói fiók kiépítési konfigurálása

Az **automatikus** lehetőséget választva jeleníti meg a képernyő, amely négy részből oszlik:

###<a name="admin-credentials"></a>Rendszergazdai hitelesítő adatok
Ez a hol API-t a program kitölti az alkalmazás felhasználókezelés csatlakozhat Azure Active Directory hitelesítő adatok szükséges. A szükséges adatokat az alkalmazás függően változik. Hitelesítő adatok típusát és adott alkalmazások követelményei kapcsolatos további tudnivalókért lásd: a [konfigurációs oktatóprogram az adott alkalmazás számára](active-directory-saas-app-provisioning.md#list-of-apps-that-support-automated-user-provisioning).

Jelölje ki a **Kapcsolat tesztelése** gombra lehetővé teszi a hitelesítő adatok vizsgálata úgy, hogy az Azure Active Directory kísérlet csatlakozhat az alkalmazás adatait a kiépítési a megadott hitelesítő adatok alkalmazást.

###<a name="mappings"></a>Hozzárendelések
Most, ahol rendszergazdák megtekinthetik és szerkeszthetik milyen felhasználói attribútumok folyamat közötti Azure Active Directory és a célalkalmazás, a rendszer felhasználói fiókok kiépítéstől vagy frissíteni.

Előre beállított halmazának hozzárendelések Azure Active Directory felhasználói és objektumok között minden szoftver alkalmazás felhasználói nem. Egyes alkalmazások más típusú objektumok, például csoportok vagy partnerek kezelése. Valamelyikének kiválasztásával ezek a hozzárendelések az a táblázat bemutatja, hogy a jobb oldalon, ahol is megtekinthetők és testre szabott a hozzárendelés szerkesztő.

![Alkalmazás az erőforrás lap][2]

Az első villámnézetben támogatott testreszabások az alábbiak:

* Engedélyezése és letiltása az egyes objektumok, például a Azure Active Directory user objektumban, hogy a szoftver alkalmazás user objektumban hozzárendeléseket.

* Szerkesztés, mely attribútumok flow Azure AD-felhasználó objektumból az alkalmazás felhasználói objektumra. Attribútum hozzárendelése a további tudnivalókért lásd: [attribútum megfeleltetés típusainak ismertetése](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-types).

* Azure Active Directory végrehajtsa a célalkalmazás, amely egy új szolgáltatást az Azure-portálon a kiépítési műveletek szűrése. Teljesen-szinkronizálás objektumok Azure Active Directory bízza a végrehajtott műveletek korlátozhatja. Például szerint csak a kiválasztása, **frissítés**, Azure AD csak meglévő felhasználói fiókokat az alkalmazások és a frissítések nem hoz létre újakat. Csak jelölje ki a **Létrehozás**, a Azure csak hoz létre új felhasználói fiókok, de nem frissíti a meglévőket. Ez a szolgáltatás lehetővé teszi, hogy a fiók létrehozása különböző hozzárendeléseinek létrehozása és frissítése a munkafolyamatok rendszergazdák. A teljes azt jelenti, hogy hozzon létre egy alkalmazás több leképezést tervezett belül az előnézeti időszak.

###<a name="settings"></a>Beállítások
Ez a szakasz lehetővé teszi, hogy a rendszergazdák indítása és leállítása az Azure AD a kijelölt alkalmazás szolgáltatás kiépítési, valamint a tetszés szerint a kiépítési gyorsítótár kiürítése, és indítsa újra a szolgáltatást.

Ha a kiépítési éppen engedélyezve van az alkalmazás első alkalommal, kapcsolni a szolgáltatást a **Kiépítési állapot** módosítása **a**. Ennek hatására a végrehajtásához egy kezdeti szinkronizálási hol olvassa be, hogy a felhasználók a **felhasználók és csoportok** szakaszban rendelt szolgáltatás kiépítési Azure AD, kérdezi le őket a célalkalmazás és végrehajtja a kiépítési műveletek Azure Active Directory- **hozzárendelések** szakaszában definiált. A folyamat során a kiépítési szolgáltatás milyen felhasználói fiókokról kezel, gyorsítótárazott adatokat tárolja, így nem felügyelt fiókok belül a célalkalmazások, hogy soha nem voltak hozzárendelés hatókör nincsenek hatással vonja kiépítési művelet. A kezdeti szinkronizálás után a kiépítési szolgáltatás automatikusan szinkronizálja a felhasználók és az objektumok csoportosítása tíz perces időközönként.

Egyszerűen a **Kiépítési állapot** módosítása a **Kikapcsolva** felfüggeszti a kiépítési szolgáltatást. Ez az állapot Azure nem létrehozása, módosítása vagy távolítsa el a felhasználó vagy objektumok csoportosítása az alkalmazás. Az állapot módosítása a vissza az alkalmazás hatására a szolgáltatás, ahol abbahagyta átviteléhez.

Jelölje ki a **törlése az aktuális állapotát, és indítsa újra a szinkronizálás** jelölőnégyzetet, és a Mentés leállítja a kiépítési szolgáltatást, kiírása a gyorsítótárban tárolt adatokat milyen fiókokról Azure Active Directory kezeli, újraindítja a szolgáltatásokat, és újra a kezdeti szinkronizálást hajt végre. Ezzel a beállítással a rendszergazdák kezdje a kiépítési telepítési folyamatot újra.

###<a name="synchronization-details"></a>Szinkronizálás részletei
Ez a témakör a művelet a kiépítési szolgáltatás, beleértve az első és utolsó időpontokat a kiépítési szolgáltatás futtatta ellen hány felhasználó és objektumok csoportosítása kezelhetők alatt, és az alkalmazás hozzáadása olvashat.

Hivatkozások vannak, feltéve hogy **létesítése jelentés a tevékenységek**, amely az összes felhasználó naplózási biztosít, és létrehozott frissített és közötti eltávolított Azure Active Directory-csoportok és a cél alkalmazás, és a **létesítése hibákról szóló jelentéseket** nyújt további részletes hibaüzenetek jelennek meg a felhasználó és el szeretné olvasni, nem sikerült létrehozni, objektumok csoportosítása frissíti, vagy eltávolítja. 

[0]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-blade.PNG
[1]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning.PNG
[2]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning-mapping.PNG

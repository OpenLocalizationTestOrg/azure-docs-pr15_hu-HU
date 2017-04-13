<properties
    pageTitle="Egyszeri bejelentkezés az Azure Active Directory előnézetben vállalati alkalmazások kezelésének |} Microsoft Azure"
    description="Megtudhatja, hogy miként kezelheti a egyszeri bejelentkezési a vállalati alkalmazások használata az Azure Active Directory"
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
    ms.date="09/30/2016"
    ms.author="asmalser"/>

# <a name="preview-managing-single-sign-on-for-enterprise-apps-in-the-new-azure-portal"></a>Előzetes verziója esetén: Egyszeri bejelentkezés a vállalati alkalmazásait tartalmazza az új Azure portál kezelése

> [AZURE.SELECTOR]
- [Azure portál](active-directory-enterprise-apps-manage-sso.md)
- [Azure klasszikus portál](active-directory-sso-integrate-saas-apps.md)

Ez a cikk ismerteti, hogyan egyszeri bejelentkezéses beállításainak kezelése a alkalmazások, különösen szegélyei az [Azure Active Directory (Azure Active Directory) alkalmazás gyűjtemény](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery)felvétele az [Azure portal](https://portal.azure.com) segítségével. Az Azure Active Directory adatkezelési folyamatok az egyszeri bejelentkezési jelenleg nyilvános előzetes verzióban, és ez a cikk ismerteti az új szolgáltatásokat, valamint a néhány ideiglenes korlátozások vonatkoznak, amely csak az előnézeti időszakban helyen lesz. [Mi az a előzetes verzióban?](active-directory-preview-explainer.md)

##<a name="finding-your-apps-in-the-new-portal"></a>Az alkalmazások keresése az új portálon

Szeptember 2016, kezdve egyetlen konfigurált összes alkalmazás bejelentkezéses az [Azure Active Directory-alkalmazás gyűjtemény](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) belül az [Azure klasszikus portal](https://manage.windowsazure.com)segítségével a címtár-rendszergazda által könyvtárában található, is most tekinthetők meg és kezelhetők az Azure-portálon.

Ezeket az alkalmazásokat az Azure portálon, a **További szolgáltatások** listában a [portálon](https://portal.azure.com)található, amelyhez hivatkozást **Vállalati alkalmazások** szakaszában találhatók. Vállalati számítógépekre telepített és a szervezeten belüli felhasználók által használt alkalmazások is.

![Vállalati alkalmazások lap][1]

Jelölje ki az **összes alkalmazás** beállított, beleértve az alkalmazások, a gyűjteményből felvett összes alkalmazás listájának megtekintése. Alkalmazás kijelölése betölti az erőforrás lap, hogy-alkalmazásokból, ahol jelentések megtekintheti, hogy az alkalmazás és a beállítások számos kezelhető.

Egyszeri bejelentkezés beállításainak kezelése, jelölje be **az egyszeri bejelentkezés**.

![Alkalmazás az erőforrás lap][2]


##<a name="single-sign-on-modes"></a>Egyszeri bejelentkezés módok

**Egyszeri bejelentkezés** lap **mód** menüt, amely lehetővé teszi, hogy konfigurálni egyszeri bejelentkezéses mód kezdődik. A rendelkezésre álló beállítások a következők:

* **A SAML-alapú bejelentkezési** – ezt a lehetőséget, ha az alkalmazás által támogatott teljes szövetséges egyszeri bejelentkezés az Azure Active Directory, a SAML 2.0-s protokoll használatával.

* **A jelszó-alapú bejelentkezési** – ezt a lehetőséget, ha az Azure Active Directory támogatja a jelszó-űrlap kitöltése ehhez az alkalmazáshoz.

* **Csatolt bejelentkezés** - korábbi neve "Meglévő egyszeri bejelentkezés", ezt a lehetőséget a rendszergazdák az alkalmazás mutató hivatkozás elhelyezése a felhasználó Azure Active Directory Access Panel vagy az Office 365 alkalmazásindító.

A következő üzemmódok kapcsolatos további tudnivalókért lásd: [hogyan egyszeri bejelentkezés Azure Active Directory munkahelyi](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).


##<a name="saml-based-sign-on"></a>SAML-alapú bejelentkezés

A **SAML-alapú bejelentkezés** lehetőséget a lap, amely a négy részre oszlik jeleníti meg:

###<a name="domains-and-urls"></a>A tartományok és URL-címei
Az összes és részletes tudnivalókat az alkalmazás tartományi URL-címek hol bekerülnek az Azure Active directory. Mivel a **Megjelenítés speciális beállítások URL-cím** jelölőnégyzet bejelölésével megtekintheti az összes választható bemenetben egyszeri bejelentkezéses munka alkalmazásnak szükséges összes bemenetben közvetlenül a képernyő jelennek meg. A támogatott ráfordítások teljes listáját tartalmazza:

* **Bejelentkezési a URL-cím** – Ha a felhasználó megnyitja a való bejelentkezéshez az alkalmazás. Ha az alkalmazás végre szeretne hajtani service provider által kezdeményezett egyszeri bejelentkezési, majd amikor a felhasználó Ugrás az URL-cím van konfigurálva, a szolgáltató hajtja végre a szükséges átirányítást az Azure Active Directory hitelesítő és a felhasználónak bejelentkezés. Ha ez a mező nem üres, majd Azure Active Directory kell használni az URL-cím indítsa el az alkalmazást az Office 365-ben és az Azure Active Directory Access panelen. Ha ebben a mezőben argumentumot, és Azure Active Directory inkább hajt végre identitásszolgáltató-mikor van az alkalmazást az Office 365-ben, az Azure Active Directory Access Panel, vagy az Azure Active Directory kezdeményezett bejelentkezés egyszeri bejelentkezéses URL.

* **Azonosító** – ez URI egyedileg kell határoznia az alkalmazás, mely egyetlen jelentkezzen be rendszer épp konfigurálja a. Ez az érték, amely Azure AD vissza küld a SAML jogkivonat, mint a közönség paraméter alkalmazást, és az alkalmazás várhatóan érvényesítés. Ez az érték is megjelenik a szervezet ID bármely SAML metaadataiban, az alkalmazás által biztosított.

* **Válasz URL** - válasz URL-címe, ha az alkalmazás vár, a SAML jogkivonat fogadásához. Ez is nevezik állítás fogyasztói szolgáltatást (ACS) URL-CÍMÉT. Ezek beírásakor, kattintson a Tovább gombra kattintva jelenítse meg a következő képernyőn. A képernyő mit kell konfigurálni kell ahhoz, fogadja el a SAML jogkivonat az Azure Active Directory alkalmazás oldalán információt tartalmaz.

* **Továbbítási állapotot** – a továbbítási állapotot nem választható paraméter, amely segít, hogy az alkalmazás a felhasználó irányítja a hitelesítési befejeződése után where. A szokásos értéke érvényes URL-címet, az alkalmazás, azonban az egyes alkalmazások másképpen használja ezt a mezőt (lásd az egyszeri bejelentkezési az alkalmazás dokumentációjában a). Az azt jelenti, hogy a továbbítási állapotot beállítva egy olyan új szolgáltatás, az új Azure portál egyedi.

###<a name="user-attributes"></a>Felhasználói attribútumok
Ez a rendszergazdák megtekinthetik és szerkeszthetik a küldött attribútumok a SAML jogkivonat, amely az Azure Active Directory problémák az alkalmazás minden alkalommal felhasználók jelentkezzen be az adott.

Az első előzetes kiadása a támogatott csak szerkeszthető attribútum esetén a **Felhasználói azonosító** attribútum. Az attribútum értéke a mezőt az Azure Active Directory egyedileg azonosító minden felhasználóhoz, az alkalmazáson belül. Például ha az alkalmazás telepítették a felhasználónév és egyedi azonosító "E-mail címét" használ, majd az érték volna állíthatók be a "user.mail" mező az Azure Active Directory.

További attribútumok szerkesztése az ezt követő előnézetét fog támogatja.

###<a name="saml-signing-certificate"></a>SAML-aláíró tanúsítvány
Ebben a részben a az Azure Active Directory használó bejelentkezni, a SAML tokenek, az alkalmazás minden alkalommal, amikor a felhasználó hitelesíti állítanak tanúsítvány adatait. Lehetővé teszi a tulajdonságait vizsgálni, hogy az aktuális tanúsítvány többek között a lejárati dátum.

Az azt jelenti, hogy a tanúsítvány átváltási és beállítások támogatják az előzetes későbbi kiadásokban további tanúsítvány kezelése. Figyelje meg, hogy a tanúsítványok teljes kezelésével továbbra is végrehajtható az [Azure klasszikus portálon](active-directory-sso-certs.md).

###<a name="application-configuration"></a>Alkalmazás-konfiguráció
A végleges rész a dokumentáció, illetve a vezérlők magát az alkalmazás Azure Active Directory-identitásszolgáltató használandó beállításához szükséges.

Az **Alkalmazás konfigurálása** beúszó menü útmutatás új tömör, beágyazott az alkalmazás lehetőségeit. Ez egy másik új szolgáltatás egyedi az új Azure-portálra.

> [AZURE.NOTE] Egy kész példa beágyazott dokumentációt című témakörben a Salesforce.com-alkalmazást. Dokumentációjában találhat további alkalmazások folyamatosan bővülő az előzetes verzióban.

![Beágyazott dokumentumok][3]

##<a name="password-based-sign-on"></a>A jelszó-alapú bejelentkezési
Ha az alkalmazás támogatja, a jelszó-alapú egyszeri bejelentkezés módja, gomb kiválasztásával **mentése** azonnali konfigurálja végezze el a jelszó-alapú egyszeri Bejelentkezést. Jelszó-alapú SSO telepítésével kapcsolatos további tudnivalókért lásd: [hogyan egyszeri bejelentkezés Azure Active Directory munkahelyi](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![A jelszó-alapú bejelentkezési][4]


##<a name="linked-sign-on"></a>A csatolt bejelentkezési
Támogatott az alkalmazás, a csatolt SSO mód kiválasztása lehetővé teszi az Azure Active Directory Access Panel vagy az Office 365 átirányítása, amikor a felhasználó kattint, az alkalmazás a kívánt URL-címet. Csatolt SSO (korábbi nevén "meglévő SSO") kapcsolatos további tudnivalókért lásd [hogyan egyszeri bejelentkezés Azure Active Directory munkahelyi](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Csatolt bejelentkezéses][5]

[1]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade.PNG
[2]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-sso-blade.PNG
[3]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-embedded-docs.PNG
[4]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-password-sso.PNG
[5]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-linked-sso.PNG

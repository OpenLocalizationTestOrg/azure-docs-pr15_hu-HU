<properties 
    pageTitle="Egyszeri bejelentkezés, amelyek nem az Azure Active Directory-alkalmazás gyűjteményben alkalmazások beállítása |} Microsoft Azure" 
    description="Megtudhatja, hogyan önkiszolgáló csatlakoznia alkalmazások Azure Active Directory, a SAML és a jelszó-alapú egyszeri bejelentkezés" 
    services="active-directory" 
    authors="asmalser-msft"  
    documentationCenter="na" manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="02/09/2016" 
    ms.author="asmalser" />

#<a name="configuring-single-sign-on-to-applications-that-are-not-in-the-azure-active-directory-application-gallery"></a>Egyszeri bejelentkezés, amelyek nem az Azure Active Directory-alkalmazás gyűjteményben alkalmazások beállítása

Ez a cikk a szolgáltatása, amely lehetővé teszi a rendszergazdák az egyszeri bejelentkezés nem szerepel az Azure Active Directory alkalmazás gyűjtemény *kódírás nélkül*alkalmazások beállítása az. Ez a funkció a 2015 November 18 a technikai előzetes kiadásának és [Azure Active Directory prémium](active-directory-editions.md)található. Ha a Fejlesztőeszközök találhat egyéni alkalmazások integrálása az Azure Active Directory kód helyett keres, olvassa el a [Az Azure Active Directory Authentication esetek](active-directory-authentication-scenarios.md).

Az Azure Active Directory-alkalmazás gyűjteményben listája, amely a támogatási megjeleníthető a egyszeri bejelentkezés az Azure Active Directory, a [jelen cikkben](active-directory-appssoaccess-whatis.md)leírt módon ismert alkalmazások talál. Ha (mint informatikai szakértői vagy a rendszer integrátor a szervezet) megtalálta a használni kívánt alkalmazást, használatának megkezdéséhez által a a lépésenkénti utasításokat követve az Azure adatkezelési portált a található ahhoz, hogy egyszeri bejelentkezést.

[Azure Active Directory prémium](active-directory-editions.md) licenccel rendelkező felhasználók is megnyithatja a következő további lehetőségeket:

* Önkiszolgáló bármely alkalmazásban, amely támogatja a SAML 2.0 identitásszolgáltató (SP kezdeményezésére vagy IdP kezdeményezésére) integrációja
* Önkiszolgáló integrációs bármely webalkalmazás, amely tartalmaz egy HTML-alapú bejelentkezési lapja [jelszó-alapú egyszeri bejelentkezés](active-directory-appssoaccess-whatis.md#password-based-single-sign-on) használata
* A felhasználó kiépítési ([itt leírt](active-directory-scim-provisioning.md)) SCIM protokollt használó alkalmazások önkiszolgáló kapcsolat
* Hivatkozások hozzáadása az [Office 365 alkalmazásindítójához](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) , vagy az [Azure Active Directory access panel](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) bármely alkalmazásban

Ez nem csak a szoftver alkalmazást használ, de van nem még lett a-szállt az Azure Active Directory-alkalmazás tárába, de Ön, a felhőben, vagy a helyszíni kiszolgálók alkalmazó szervezetek külső webalkalmazások tartalmazza.

E ezekre a lehetőségekre, más néven *alkalmazássablonok integráció*, a SAML SCIM és űrlapalapú hitelesítés, valamint rugalmas beállítások és az alkalmazások széles szám való kompatibilitást szolgáló beállítások alkalmazások szabványos alapú csatlakozási pontok szükséges. 

##<a name="adding-an-unlisted-application"></a>A listán nem alkalmazás hozzáadása

Az alkalmazás-alkalmazássablon integrációs csatlakozni, jelentkezzen be az Azure kezelőportálja segítségével az Azure Active Directory-rendszergazdai fiókkal, és keresse meg a **az Active Directory > [könyvtár] > alkalmazások** szakaszban, jelölje be a **Hozzáadás gombra**, majd **a gyűjteményből alkalmazás hozzáadása**. 

![][1]

Az alkalmazás a gyűjteményben használja az **egyéni** kategóriára, a bal oldalon vagy a **listán nem alkalmazás hozzáadása** hivatkozásra kattint, ha a kívánt alkalmazás nem találta a keresési eredmények között látható a listán nem alkalmazás is hozzáadhat. Miután beírta az alkalmazás nevét, beállíthatja a egyszeri bejelentkezéses beállítások és a működését. 

**Gyors tipp**: a legjobb módszer a szöveg.keres függvénnyel segítségével ellenőrizze, hogy ha az alkalmazás már létezik, az alkalmazás gyűjteményben. Ha az alkalmazás található, és a leírása Említések "egyszeri bejelentkezéses", majd az alkalmazás már támogatott szövetséges egyszeri bejelentkezést. 

![][2]

Az alkalmazás hozzáadása a ily módon használható előre integrált alkalmazások egy nagyon hasonlít felületet nyújt. Szeretné kezdeni, jelölje ki a **Konfigurálása Single Sign-On**. A következő képernyőn forrástáblázat a következő három beállítja az egyszeri bejelentkezési, amelyek az alábbi szakaszok ismertetik.

![][3]


##<a name="azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On

Ezzel a beállítással az alkalmazás SAML-alapú hitelesítés konfigurálása. Ehhez az, hogy az alkalmazás a SAML 2.0 támogatási, és kell vonatkozó információk gyűjtését az alkalmazás, a továbblépés előtt a SAML funkcióinak használatát. Miután kiválasztotta a **következő**, kéri a SAML végpontok az alkalmazáshoz tartozó három különböző URL adja meg. 

![][4]
 
A következők:

* **Bejelentkezési a URL-CÍMÉT (SP kezdeményezésére csak)** – Ha a felhasználó megnyitja a bejelentkezési kísérleteket Ez az alkalmazás. Ha az alkalmazás végre szeretne hajtani service provider által kezdeményezett egyszeri bejelentkezési, majd amikor a felhasználó Ugrás az URL-cím van konfigurálva, a hozzáférés-szolgáltató a szükséges átirányítást hajt végre az Azure Active Directory hitelesítő, és jelentkezzen be a felhasználó. Ha ez a mező nem üres, majd Azure Active Directory kell használni az URL-cím indítsa el az alkalmazást az Office 365-ben és az Azure Active Directory Access panelen. Ha a mező kitöltése nem ommited, majd az Azure Active Directory inkább végez identitásszolgáltató-mikor van az alkalmazást az Office 365-ben, az Azure Active Directory Access Panel, vagy az Azure Active Directory kezdeményezett bejelentkezés egyszeri bejelentkezési URL-cím (copiable az irányítópult lapon).

* Az épp konfigurálja a **Kibocsátó URL** - a kibocsátó URL-CÍMÉT a egyedileg kell azonosító mely egyszeri bejelentkezési kérelmezése. Ez az érték, amely Azure AD vissza küld a SAML jogkivonat, mint a **közönség** paraméter alkalmazását, és az alkalmazás várhatóan érvényesítés. Ezt az értéket is megjelenik az alkalmazás által biztosított bármely SAML-metaadatok a **Szervezet azonosítója** . További információt a Mi az alkalmazás SAML dokumentációjában entitás azonosítója vagy közönség érték van. Az alábbi képen egy példa a közönség URL-cím megjelenése a SAML-jogkivonat adja vissza az alkalmazást:

```
    <Subject>
    <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:unspecificed">chad.smith@example.com</NameID>
        <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
      </Subject>
      <Conditions NotBefore="2014-12-19T01:03:14.278Z" NotOnOrAfter="2014-12-19T02:03:14.278Z">
        <AudienceRestriction>
          <Audience>https://tenant.example.com</Audience>
        </AudienceRestriction>
      </Conditions>
```

* **Válasz URL** - válasz URL-címe, ha az alkalmazás vár, a SAML jogkivonat fogadásához. Ez is nevezik **állítás fogyasztói szolgáltatást (ACS) URL-CÍMÉT**. Jelölje be az alkalmazás SAML dokumentációjában találhat részletes tudnivalókat a SAML jogkivonat válasz URL-cím vagy ACS URL-címe van.
 Ezek beírásakor, kattintson a **Tovább** folytassa a következő képernyőn. A képernyő mit kell konfigurálni kell ahhoz, fogadja el a SAML jogkivonat az Azure Active Directory alkalmazás oldalán információt tartalmaz. 

![][5]

Mely értékek szükségesek az alkalmazás függően változnak, ezért kérjük, az alkalmazás SAML dokumentációjában. A **Bejelentkezés** és **kijelentkezési** szolgáltatás URL-címet is úgy, hogy az azonos végpontot, amely a SAML kérelem hibakezelési végpont az Azure Active Directory példányának. A kibocsátó URL-cím érték jelenik meg a "kibocsátó" belül a SAML jogkivonat ki az alkalmazást. 

Az alkalmazás beállítása után kattintson a **Tovább** gombra, majd válassza a **kész** gombra a párbeszédpanel bezárásához. 

##<a name="assigning-users-and-groups-to-your-saml-application"></a>Felhasználók és csoportok hozzárendelése a SAML-alkalmazás 

Miután az alkalmazás Azure Active Directory tartományként a SAML-alapú identitásszolgáltató használatához konfigurált, majd készen áll a szinte ellenőrzéséhez. Egy biztonsági vezérlő Azure Active Directory nem ad egy token lehetővé teheti, hogy jelentkezzen be az alkalmazásba, kivéve, ha az azok hozzáférést kapott Azure Active Directory használatával. Felhasználók is hozzáférési jogosultsággal közvetlenül, vagy egy csoportot, amely tagja keresztül. 

Felhasználó vagy csoport adni az alkalmazást, kattintson a **Felhasználók hozzárendelése** gombra. Jelölje ki a felhasználót vagy csoportot szeretne hozzárendelni, és válassza ki a **hozzárendelése** gombra. 

![][6]

Hozzárendelése egy felhasználóhoz lehetővé teszi az Azure Active Directory számára a felhasználó, valamint egy jelennek meg a felhasználó hozzáférést Panel alkalmazás csempéjére okoz a jogkivonat kibocsátása. Ha a felhasználó az Office 365-alkalmazás csempéjére is megjelennek az Office 365-alkalmazásindító. 

Az az alkalmazást az **Embléma feltöltése** gombra a **beállítás** lapon az alkalmazás csempéjére embléma feltöltése 

###<a name="customizing-the-claims-issued-in-the-saml-token"></a>A SAML jogkivonat kiadott követelések testreszabása 

Amikor egy felhasználó az alkalmazás hitelesíti, Azure Active Directory fog egy SAML jogkivonat kibocsátása a alkalmazásba, amely tartalmaz információkat (vagy jogcímeken) kapcsolatban a felhasználót, hogy egyedi módon azonosító. Alapértelmezés szerint ez tartalmazza a felhasználó felhasználónevét, e-mail címét, Utónév és Vezetéknév. 

Megtekintheti vagy szerkesztheti a küldi el az alkalmazás az **attribútumok** lapon a SAML jogkivonat jogcímalapú. 

![][7]

Két oka lehet annak, miért kell a SAML jogkivonat kiadott követelések szerkesztése: •a alkalmazás írt állítást URL-címe egy másik funkciókészletére szükség van, vagy igényt értékek •Your alkalmazás lett telepítve oly módon, hogy a NameIdentifier igényel, hogy az a felhasználónév (AKA fő) tárolt az Azure Active Directory-től különböző formál. 

Tájékoztatást arról, hogyan adhat hozzá és szerkeszthet forgatókönyvekben követelések tanulmányozza a [követelések testreszabási jelentkező](active-directory-saml-claims-customization.md). 

###<a name="testing-the-saml-application"></a>A SAML alkalmazás tesztelése 

A SAML URL-EK és a tanúsítvány van beállítva, az Azure Active Directory és az alkalmazás, miután az Azure-alkalmazás hozzárendelt felhasználók vagy csoportok követelések ellenőrzi, és szükség esetén szerkesztett, majd jelentkezzen be az alkalmazás készen áll a felhasználó. 

Ellenőrzi, egyszerűen bejelentkezési-panelre az Azure Active Directory access https://myapps.microsoft.com egy felhasználói fiókot, Ön által kiosztott az alkalmazás, a, és kattintson az egyszeri bejelentkezési folyamat elindításához az alkalmazás csempéje gombjára. Másik megoldás tallózhat közvetlenül a bejelentkezési URL-CÍMÉT az alkalmazás és a bejelentkezési az onnan. 

Tippek a hibakereséshez lásd: Ez a [cikk hogyan szeretné hibakeresése SAML-alapú egyszeri bejelentkezés alkalmazások](active-directory-saml-debugging.md) 

##<a name="password-single-sign-on"></a>Jelszó Single Sign-On

Ezzel a beállítással konfigurálja a [jelszó-alapú egyszeri bejelentkezés a](active-directory-appssoaccess-whatis.md) webes alkalmazáshoz, amely tartalmaz egy HTML bejelentkezési lapja. Jelszó-alapú SSO is említett vaulting, jelszó lehetővé teszi, hogy a felhasználói hozzáférés és a jelszavakat identitás összevonási nem támogató webalkalmazások kezelése. Érdemes emellett esetek, amikor több felhasználó kell oszt meg egy adott fiókhoz, például szervezete közösségi hálózat app-fiókok esetében hasznos lehet. 

Miután kiválasztotta a **következő**, kéri a URL-CÍMÉT az alkalmazás webalapú bejelentkezési lapján írja be. Figyelje meg, hogy ez a lapot, amely tartalmazza a felhasználónév és jelszó beviteli mezők kell lennie. Miután írt be, akkor Azure AD folyamatának elemzésére, a bejelentkezési lapja a szövegbeviteli felhasználónév és jelszó beviteli. A folyamat nem jár sikerrel, ha ezután végigvezet telepítésének (az Internet Explorer, a Chrome vagy a Firefox igényli) böngészőbővítmény, amely lehetővé teszi, hogy a mezők kézi rögzítése egy alternatív folyamatát.

A bejelentkezési lapja rögzíthető, felhasználók és csoportok előfordulhat, hogy kiosztani, és a hitelesítőadat-házirendek állíthat be, mint normál [jelszó-féle egyszeri Bejelentkezést alkalmazások](active-directory-appssoaccess-whatis.md).

Megjegyzés: Is feltölthet a csempe embléma az alkalmazás, a **beállítás** lapon, az alkalmazás használatával az **Embléma feltöltése** gombra. 

##<a name="existing-single-sign-on"></a>Meglévő Single Sign-On

Ezzel a beállítással a hivatkozás hozzáadása az alkalmazáshoz a szervezet Azure Active Directory Access Panel vagy az Office 365-portálra. Használhatja ezt hivatkozások felvétele az Azure Active Directory összevonási szolgáltatások (vagy más összevonási szolgáltatás) jelenleg használó egyéni webalkalmazások helyett hitelesítéshez Azure AD. Másik lehetőségként a mély hivatkozások hozzáadása SharePoint-lapokat vagy más weblapok kívánt ugyanúgy jelennek meg a felhasználói hozzáférés panelek. 

Miután kiválasztotta a **következő**, kéri az alkalmazás a csatolni kívánt URL-címet. Miután befejeződött, a felhasználók és csoportok hatására az alkalmazás megjelenik az [Office 365-alkalmazásindító](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) vagy azoknak a felhasználóknak az [Azure Active Directory access panel](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) alkalmazás hozzárendelhető.

Megjegyzés: Is feltölthet a csempe emblémát az alkalmazás, a **beállítás** lapon, az alkalmazás használatával az **Embléma feltöltése** gombra.

## <a name="related-articles"></a>Kapcsolódó cikkek

- [A cikk az Azure Active Directory Alkalmazáskezelés indexe](active-directory-apps-index.md)
- [A SAML jogkivonat-előre integrált alkalmazások kiadott Jogcímeken testreszabása](active-directory-saml-claims-customization.md)
- [Hibaelhárítási SAML-alapú Single Sign-On](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saas-custom-apps/customapp1.png
[2]: ./media/active-directory-saas-custom-apps/customapp2.png
[3]: ./media/active-directory-saas-custom-apps/customapp3.png
[4]: ./media/active-directory-saas-custom-apps/customapp4.png
[5]: ./media/active-directory-saas-custom-apps/customapp5.png
[6]: ./media/active-directory-saas-custom-apps/customapp6.png
[7]: ./media/active-directory-saas-custom-apps/customapp7.png

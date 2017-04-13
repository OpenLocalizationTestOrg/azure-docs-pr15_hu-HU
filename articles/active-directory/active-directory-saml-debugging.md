<properties 
    pageTitle="Hogyan szeretné hibakeresése SAML-alapú egyszeri bejelentkezés az Azure Active Directory alkalmazások |} Microsoft Azure" 
    description="Megtudhatja, hogy miként szeretné hibakeresése SAML-alapú egyszeri bejelentkezés az Azure Active Directory-alkalmazáshoz " 
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

#<a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Hogyan szeretné hibakeresése SAML-alapú egyszeri bejelentkezés az Azure Active Directory-alkalmazáshoz

A SAML-alapú alkalmazás integrációs hibakeresési, amikor célszerű gyakran eszközzel [Fiddler](http://www.telerik.com/fiddler) például a SAML kérést, a SAML választ és a tényleges SAML jogkivonat, az alkalmazás kibocsátott megjelenítéséhez. A SAML jogkivonat vizsgálatakor biztosítható, hogy minden a szükséges attribútumokat, a SAML tárgyát és a kibocsátó URI felhasználónév érkeznek is.

![][1]

Azure Active Directory, a SAML jogkivonat tartalmazó válasza a szokásos, amely akkor fordul elő, miután egy HTTP 302 https://login.windows.net a irányítani, és a rendszer elküldi a beállított **Válasz URL-CÍMÉT** az alkalmazás. 
 
Jelölje ki a sort, és válassza a megtekintheti a SAML jogkivonat a **ellenőrök > fejlesztő** lapon kattintson a jobb oldali ablaktábla. Itt kattintson a jobb gombbal a **SAMLResponse** értéket, és válassza a **TextWizard küldeni**. Válassza **A Base64** **átalakítása** menüből a token dekódolását és annak tartalmát.
 
**Megjegyzés**: a HTTP-kérés tartalmának megtekintéséhez Fiddler előfordulhat, hogy felajánlja a HTTPS-forgalom kell végrehajtania visszafejtése konfigurálása.

## <a name="related-articles"></a>Kapcsolódó cikkek

- [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)
- [Egyszeri bejelentkezés, amelyek nem az Azure Active Directory-alkalmazás gyűjteményben alkalmazások beállítása](active-directory-saas-custom-apps.md)
- [A SAML jogkivonat-előre integrált alkalmazások kiadott Jogcímeken testreszabása](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ./media/active-directory-saml-debugging/fiddler.png

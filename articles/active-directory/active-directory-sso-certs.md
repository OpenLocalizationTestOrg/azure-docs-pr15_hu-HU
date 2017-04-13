<properties
    pageTitle="Az Azure Active Directory kezelése a tanúsítványok szövetség |} Microsoft Azure"
    description="Megtudhatja, miként szabhatja testre a lejárati dátum, az összevonási tanúsítványok és megújítása tanúsítványok hamarosan lejár."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="managing-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>Tanúsítványok az összevont egyszeri bejelentkezési az Azure Active Directory kezelése

Ez a cikk bemutatja a tanúsítványok, amelyeket Azure Active Directory hoz létre, annak érdekében, hogy a szoftver alkalmazásaihoz szövetséges egyszeri bejelentkezés (SSO) létesítsen kapcsolatos gyakori kérdésekre.

Ez a cikk csak akkor jelentősége alkalmazások beállított **Azure Active Directory Single Sign-On**, az alábbi példában látható módon:

![Azure Active Directory Single Sign-On](./media/active-directory-sso-certs/fed-sso.PNG)

##<a name="how-to-customize-the-expiration-date-for-your-federation-certificate"></a>A lejárati dátum testreszabása az összevonási tanúsítvány

Alapértelmezés szerint a tanúsítványok két év múlva lejár vannak beállítva. Megadhatja a tanúsítvány eltérő lejárati dátumát, az alábbi lépéseket követve. Része a képernyőképek példa az használja a Salesforce, de ezeket a lépéseket minden olyan szövetséges szoftver alkalmazás alkalmazhat.

1. Azure Active Directory az alkalmazást, az első lépések lapján kattintson a **Configure egyszeri bejelentkezés a**.

    ![Nyissa meg az egyszeri bejelentkezés konfigurálása varázsló.](./media/active-directory-sso-certs/config-sso.png)

2. Válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

3. Írja be az alkalmazás **Bejelentkezési URL** , és jelölje be az **konfigurálása a tanúsítvány használható szövetséges egyszeri bejelentkezést**. Kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On](./media/active-directory-sso-certs/new-app-config-sso.PNG)

4. A következő lapon jelölje be az **Új tanúsítvány létrehozása**, és válassza ki, mennyi ideig szeretné a tanúsítvány legyenek érvényesek. Kattintson a **Tovább gombra**.

    ![Új tanúsítvány létrehozása](./media/active-directory-sso-certs/new-app-config-cert.PNG)

5. Ezután kattintson a **tanúsítvány letöltése**. Megtudhatja, hogy miként tölthet fel a tanúsítványt az adott szoftver alkalmazást, kattintson a **Nézet konfigurációs utasításokat**.

    ![Töltse le a, majd a tanúsítvány feltöltése](./media/active-directory-sso-certs/new-app-config-app.PNG)

6. A tanúsítvány a nem engedélyezett, amíg a párbeszédpanel alján a megerősítést kérő jelölőnégyzet bejelölése, és ezután kattintson a Küldés gombra.

##<a name="how-to-renew-a-certificate-that-will-soon-expire"></a>Hamarosan lejár tanúsítvány megújítása

A felhasználók számára nem jelentős állásidőt kell ideális vonhat a megújítás lépéseket az alább látható módon. A példában, de ezeket a lépéseket a szakasz szolgáltatás Salesforce használt számítógépen készített képernyőképeket bármely szövetséges szoftver alkalmazás alkalmazhat.

1. Azure Active Directory az alkalmazás első lépések lapon kattintson **Konfigurálása Single Sign-On**.

    ![Nyissa meg az egyszeri bejelentkezés konfigurálása varázsló](./media/active-directory-sso-certs/renew-sso-button.PNG)

2. A párbeszédpanelen az első oldalon **Azure Active Directory egyszeri bejelentkezés** már legyen kijelölve, ezért kattintson a **Tovább**gombra.

3. A második oldalon jelölje be a **konfigurálása az összevont egyszeri bejelentkezési használt tanúsítvány**. Kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On](./media/active-directory-sso-certs/renew-config-sso.PNG)

4. A következő lapon jelölje be az **Új tanúsítvány létrehozása**, és válassza ki, mennyi ideig szeretné az új tanúsítvány legyenek érvényesek. Kattintson a **Tovább gombra**.

    ![Új tanúsítvány létrehozása](./media/active-directory-sso-certs/new-app-config-cert.PNG)

5. Kattintson a **tanúsítvány letöltése**. A sikeres rewnew a tanúsítvány, végre kell hajtania az alábbi két lépést:

    - Töltse fel az új tanúsítvány a szoftver alkalmazás egyszeri bejelentkezés beállítási képernyőn. Megtudhatja, hogy miként ehhez az adott szoftver számára, kattintson a **Nézet konfigurációs utasításokat**.

    - Az Azure Active Directory jelölje be a megerősítést kérő jelölőnégyzetet a ahhoz, hogy az új tanúsítvány párbeszédpanel alján, és kattintson a **következő** küldhetik.

    > [AZURE.IMPORTANT] Egyszeri bejelentkezés az alkalmazásba letiltjuk időpontjában vagy egy e két lépés végrehajtása, de ezt engedélyezi ismét a második lépés befejezése után. Ezért legrövidebb leállás céljából, adjon előkészítése belül egy rövid időmennyiséget egymástól a két lépés elvégzéséhez.

    ![Töltse le a, majd a tanúsítvány feltöltése](./media/active-directory-sso-certs/renew-config-app.PNG)

## <a name="related-articles"></a>Kapcsolódó cikkek

- [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)
- [Access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md)
- [Hibaelhárítási SAML-alapú Single Sign-On](active-directory-saml-debugging.md)

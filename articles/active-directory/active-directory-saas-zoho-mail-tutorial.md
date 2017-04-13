<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Zoho Mail |} Microsoft Azure" 
    description="Megtudhatja, hogy miként Zoho levelezés használata az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!." 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/09/2016" 
    ms.author="markvi" />

#<a name="tutorial-azure-active-directory-integration-with-zoho-mail"></a>Oktatóprogram: Azure Active Directory-integráció a Zoho levelek
  
Ebben az oktatóanyagban célja integrációja Azure és Zoho levelek megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Zoho Mail bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory felhasználók Zoho Mail rendelt tudnak való egyszeri bejelentkezési a Zoho Mail vállalati webhely (a szolgáltatás által kezdeményezett szolgáltató bejelentkezési) vagy használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az alkalmazásba.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  A Zoho mail alkalmazás integráció engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-zoho-mail-tutorial/IC789600.png "Eset")

##<a name="enabling-the-application-integration-for-zoho-mail"></a>A Zoho mail alkalmazás integráció engedélyezése
  
Ez a szakasz célja, hogyan Zoho mail alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-zoho-mail-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrálását Zoho levelezéshez, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-zoho-mail-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-zoho-mail-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-zoho-mail-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-zoho-mail-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Zoho Mail**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-zoho-mail-tutorial/IC789601.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában válassza a **Levelek Zoho**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Zoho levelek] (./media/active-directory-saas-zoho-mail-tutorial/IC789602.png "Zoho levelek")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja engedélyezése a felhasználóknak, hogy a saját fiók az összevonási alapján a SAML protokoll használatával Azure Active Directory hitelesítő Zoho levelek tagolása.  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány fájlt.  
Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Zoho Mail** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-zoho-mail-tutorial/IC789603.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Zoho levelezési felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-zoho-mail-tutorial/IC789604.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás URL-cím beállítása** lapon hajtsa végre az alábbi lépéseket:

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-zoho-mail-tutorial/IC789605.png "Állítsa be az App URL-címe")

    egy. Az **Zoho Mail bejelentkezési a URL-cím** mezőbe írja be az URL-CÍMÉT az alábbi minta használatával:`http://<company name>.ZohoMail.com`

    b. Kattintson a **Tovább**gombra.


4.  **Konfigurálása az egyszeri bejelentkezés a Zoho levelezés** lapon kattintson a **tanúsítvány letöltése**gombra, és mentse a tanúsítvány fájlt a számítógépen.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-zoho-mail-tutorial/IC789606.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a Posta Zoho vállalati webhely rendszergazdaként.

6.  Nyissa meg a **Vezérlőpult parancsra**.

    ![Vezérlőpult] (./media/active-directory-saas-zoho-mail-tutorial/IC789607.png "Vezérlőpult")

7.  Kattintson a **SAML hitelesítés** fülre.

    ![SAML-hitelesítés] (./media/active-directory-saas-zoho-mail-tutorial/IC789608.png "SAML-hitelesítés")

8.  A **SAML hitelesítési adatai** csoportban hajtsa végre az alábbi lépéseket:

    ![SAML hitelesítési részletei] (./media/active-directory-saas-zoho-mail-tutorial/IC789609.png "SAML hitelesítési részletei")

    1.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Zoho Posta** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **Bejelentkezési URL-cím** mezőben lévő értéket.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Zoho Posta** párbeszédpanel lapon a **Távoli kijelentkezés URL-címe** értéket másolja és illessze be a **Kijelentkezés URL-címe** mezőben lévő értéket.
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Zoho Posta** párbeszédpanel lapon másolja a vágólapra az **URL-cím módosítása jelszó** értéket, és illessze be az **URL-cím módosítása jelszó** szövegmezőt.
    4.  A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása  

        >[AZURE.TIP] További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.

    5.  Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, azt tartalmát a vágólapra másolja és illessze be a **PublicKey** mezőben lévő értéket szeretné.
    6.  Jelölje ki a **RSA** **algoritmus**.
    7.  Kattintson az **OK gombra**.

9.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-zoho-mail-tutorial/IC789610.png "Egyszeri bejelentkezés beállítása")

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory felhasználóknak, hogy jelentkezzen be egy Zoho e-mailbe, hogy ki kell építenie egy Zoho e-mailbe.  
Zoho Mail, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **Zoho Mail** vállalati webhely.

2.  Nyissa meg a **Vezérlőpult \> Posta és a dokumentumok**.

3.  Nyissa meg a **felhasználó adatai \> felhasználó hozzáadása**.

    ![Felhasználó hozzáadása] (./media/active-directory-saas-zoho-mail-tutorial/IC789611.png "Felhasználó hozzáadása")

4.  A **felhasználók hozzáadása** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Felhasználó hozzáadása] (./media/active-directory-saas-zoho-mail-tutorial/IC789612.png "Felhasználó hozzáadása")

    1.  Írja be az **utónevet**, **Vezetéknév**, **E-mailek azonosítója**, egy érvényes Azure Active Directory-fiókját, kiépítése a kapcsolódó szövegdobozok be a kívánt **jelszót** .
    2.  Kattintson az **OK gombra**.  

        >[AZURE.NOTE] Az Azure Active Directory fióktulajdonos kap e-mail megelőzően aktívvá válik, győződjön meg arról, hogy a fiók mutató hivatkozással.

>[AZURE.NOTE] Bármely más Zoho levelezési felhasználói fiók létrehozása eszközöket is használhatja, illetve Zoho Mail rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-zoho-mail-perform-the-following-steps"></a>Felhasználók hozzárendelése Zoho Mail, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  **Zoho Mail **alkalmazás integrációs lapon kattintson a **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-zoho-mail-tutorial/IC789613.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-zoho-mail-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).
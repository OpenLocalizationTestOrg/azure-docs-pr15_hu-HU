<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Dropbox vállalati verzió |} Microsoft Azure" 
    description="Megtudhatja, hogy miként használhatja a Dropbox vállalati az Azure Active Directory címtárral ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Oktatóprogram: Azure Active Directory-integráció a Dropbox vállalati verziójával
  
Ebben az oktatóanyagban célja a Azure és vállalati Dropbox-integrációt megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   A Dropbox vállalati próba bérlői webhelyre
  
Miután elkészült a ebben az oktatóanyagban, a a vonatkozó üzleti tudják egyszeri bejelentkezési az alkalmazásba, a Dropbox vállalati Dropbox rendelt Azure AD-felhasználók vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md).
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  A dropbox vállalati verzió alkalmazás integráció engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769508.png "Eset")



##<a name="enabling-the-application-integration-for-dropbox-for-business"></a>A dropbox vállalati verzió alkalmazás integráció engedélyezése
  
Ez a szakasz célja tagolása a dropbox vállalati verzió alkalmazás integráció engedélyezése.

###<a name="to-enable-the-application-integration-for-dropbox-for-business-perform-the-following-steps"></a>Ha engedélyezni szeretné a Dropbox vállalati verzió alkalmazás integrációját, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Dropbox vállalati verziójával**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC701010.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában válassza a **Dropbox vállalati**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Dropbox vállalati verziójával] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC701011.png "Dropbox vállalati verziójával")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy a Dropbox vállalati hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány feltölteni a Dropbox vállalati bérlői webhelyen. Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon, a **Dropbox vállalati verzió** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** **Beállítása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749323.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be Dropbox vállalati felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749327.png "Beállítás az egyszeri bejelentkezés")

3.  Az **Alkalmazás URL-cím beállítása** lapon hajtsa végre az alábbi lépéseket:

    egy. Bejelentkezés a a Dropbox vállalati bérlői webhelyen. 

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769509.png "Beállítás az egyszeri bejelentkezés")

    b. Kattintson a navigációs ablak bal oldalán, a **Felügyeleti konzolban**. 

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769510.png "Beállítás az egyszeri bejelentkezés")

    c billentyűkombinációt. A **Felügyeleti konzolban**kattintson a bal oldali navigációs **hitelesítési** . 

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769511.png "Beállítás az egyszeri bejelentkezés")

    d. **Egyszeri bejelentkezés** csoportban jelölje be **az egyszeri bejelentkezés engedélyezése**jelölőnégyzetet, és kattintson a **További** Ez a szakasz kibontásához.  

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769512.png "Beállítás az egyszeri bejelentkezés")

    e. Másolja a vágólapra az URL-címet **az e-mail cím beírásával felhasználók jelentkezhetnek be, vagy közvetlenül felkeresik**mellett. 

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769513.png "Beállítás az egyszeri bejelentkezés")

    f. Az Azure klasszikus portálon a **DropBox vállalati jelentkezzen be az** URL-címe mezőbe illessze be az URL-címet. 

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769514.png "Beállítás az egyszeri bejelentkezés")  



4. A **beállítás egyszeri bejelentkezés a Dropbox vállalati** lapon kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen.  

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769515.png "Beállítás az egyszeri bejelentkezés")


5. A Dropbox vállalati bérlői webhelyen, a **hitelesítési** lapon, **az egyszeri bejelentkezés** szakaszában a hajtsa végre az alábbi lépéseket: 

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "Beállítás az egyszeri bejelentkezés")

    egy. Kattintson a **szükséges**.

    b. Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Dropbox vállalati** párbeszédpanel lapon a **bejelentkezési URL-címe** értéket másolja és illessze be a **Jelentkezzen be az URL-címe** mezőben lévő értéket.


    c billentyűkombinációt. A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása 

    > [AZURE.TIP] További részletekért megtudhatja, [hogy miként szövegfájlba bináris tanúsítvány konvertálja](http://youtu.be/PlgrzUZ-Y1o).


    d. **"Válasszon tanúsítvány"** gombra, és majd böngészéssel keresse meg az **Alap-64 kódolású tanúsítványfájl**.


    e. Kattintson a konfigurálás meg a DropBox vállalati bérlő befejezéséhez **"A módosítások mentéséhez"** gombra.


6. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához. 

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749329.png "Beállítás az egyszeri bejelentkezés")



##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ez a szakasz célja, hogyan engedélyezheti a felhasználó a Dropbox vállalati verzió Active Directory felhasználói fiókok kiépítésének tagolása.


### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1. Az Azure klasszikus portálon, a **Dropbox vállalati verzió** alkalmazás integrációs lapon kattintson a **konfigurálása felhasználói kiépítési** a **Felhasználó kiépítési beállítása** párbeszédpanel megnyitásához.

2. Kattintson az engedélyezés felhasználói kiépítési a Dropboxba az üzleti lapon, kattintson a engedélyezése felhasználói kiépítési jelentkezzen be a Dropboxba kapcsolatot létesíteni Azure AD-párbeszédpanel megnyitásához.  

    ![Felhasználói kiépítése] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769517.png "Felhasználói kiépítése")

3. **Jelentkezzen be az Azure Active Directory, amelyre a hivatkozás Dropbox** párbeszédpanelen jelentkezzen be a Dropbox vállalati bérlői webhelyen. 

    ![Felhasználói kiépítése] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769518.png "Felhasználói kiépítése")



4. Kattintson az **Engedélyezés** adja meg az Azure Active Directory, a Dropboxba eléréséhez. 

    ![Felhasználói kiépítése] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769519.png "Felhasználói kiépítése")



5. A konfiguráció befejezéséhez kattintson a **kész** gombra.  

    ![Felhasználói kiépítése] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769520.png "Felhasználói kiépítése")




##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-dropbox-for-business-perform-the-following-steps"></a>Felhasználók hozzárendelése a Dropbox vállalati, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  A **Dropbox vállalati verzió **alkalmazás integrációs lapon kattintson a **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769521.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC767830.png "Igen")
  


Érdemes most Várjon 10 percet, és ellenőrizze, hogy a fiók lett szinkronizálva a Dropbox vállalati verziójával.

Első lépésként ellenőrzése jelölje be a kiépítési állapotát, a **Dropbox vállalati verzió** alkalmazás integrációs lapon az Azure klasszikus portálon **Irányítópult** gombra kattintva.

![Adhatnak a felhasználóknak] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769522.png "Adhatnak a felhasználóknak")


Ciklikus kiépítési sikeresen befejeződött felhasználó kapcsolódó állapotban jelzi.

![Adhatnak a felhasználóknak] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769523.png "Adhatnak a felhasználóknak")


Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához.
Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).




## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)
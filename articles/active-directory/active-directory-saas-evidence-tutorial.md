<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Evidence.com |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és Evidence.com között."
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
    ms.date="02/23/2016"
    ms.author="asmalser"/>


# <a name="tutorial-azure-active-directory-integration-with-evidencecom"></a>Oktatóprogram: Azure Active Directory-integráció a Evidence.com

Ebben az oktatóanyagban célja bemutatják, hogyan állíthatja be az egyszeri bejelentkezés Azure Active Directory (AAD) és Evidence.com között. Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:
    
* Microsoft Azure érvényes előfizetés
* Az egyszeri bejelentkezés Evidence.com előfizetés engedélyezve van (e-mail earlyaccess@evidence.com Ha nincs engedélyezve a SAML-alapú egyszeri bejelentkezéses)

Miután elkészült a ebben az oktatóanyagban, a AAD felhasználók, amelyhez hozzá van rendelve Evidence.com access tudnak egyetlen bejelentkezés az alkalmazásba, a AAD Access panelen.

## <a name="add-evidencecom-to-your-directory"></a>A címtár Evidence.com hozzáadása

Ez a szakasz ismerteti, hogyan Evidence.com az Azure Active Directory az integrált alkalmazások felvétele.

**Ahhoz, hogy az alkalmazás integrálását bizonyítékokra vonatkozóan:**

1.  Az [Azure klasszikus portálon](https://manage.windowsazure.com)kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

4.  Az alkalmazás gyűjtemény megnyitásához kattintson a **Hozzáadás**gombra, és kattintson a **Hozzáadás a gyűjteményből az alkalmazások**gombra.

5.  A Keresés mezőbe írja be a **Evidence.com**.

6.  Az eredmény ablaktáblában jelölje ki a **Evidence.com**, és válassza a **kész** , az alkalmazás hozzáadása gombra.


## <a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz ismerteti, hogyan engedélyezése a felhasználóknak Evidence.com az Azure Active Directory összevonási alapján a SAML protokoll használata a fiók hitelesítést végezni.

**Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:**

1.  Miután megadta a Evidence.com az Azure klasszikus portálon, kattintson a **Konfigurálása Single Sign-On**. 
 
2.  A következő képernyőn válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

3.  Az alkalmazás URL-címet konfigurálni képernyőn írja be az URL-cím hol felhasználók fog bejelentkezni a Evidence.com bérlői webhely URL-CÍMÉT (például: https://yourtenant.evidence.com, és kattintson a **Tovább gombra**. 

4.  A **Tanúsítvány letöltése** hivatkozásra, és mentse a helyi meghajtóra. A tanúsítvány és a metaadat-alapú URL-címét (entitás azonosítója, Egyszeri bejelentkezési az URL-CÍMÉT és a bejelentkezési meg URL-CÍMÉT) használandó egyszeri bejelentkezés beállítása a Evidence.com webhelyen. 

5.  A webes külön böngészőablakban, jelentkezzen be a Evidence.com bérlői rendszergazdaként, és keresse meg azt a **rendszergazda** lap
      
6.  Kattintson a **ügynökség egyszeri bejelentkezés**
 
7.  Válassza a **SAML alapján egyszeri bejelentkezési**
 
8.  Másolja a vágólapra a **Kibocsátó URL-címet**, az Azure klasszikus portálon és Evidence.com megfelelő mezőibe **Az egyszeri bejelentkezés** , és **Egyetlen kijelentkezés** értékeket.

9.  Nyissa meg a tanúsítvány lépés: 4 szövegszerkesztővel Notepad.exe, például a letöltött és másolja és illessze be a **Biztonsági tanúsítványt** mező tartalmát. 

10. Mentse a konfigurációs Evidence.com.
 
11. Jelölje be az Azure klasszikus portálon, **Ellenőrizze, hogy egyszeri bejelentkezés a fent leírt módon állította be**. Ez ellenőrzése lehetővé teszi a dolgozó ezt a jelölőnégyzetet, alkalmazás indítása az aktuális tanúsítványt.
 
12. Az egyszeri bejelentkezés Megerősítés lapon kattintson a **Befejezés**gombra.  


## <a name="creating-an-evidencecom-test-user"></a>Egy Evidence.com Teszt felhasználó létrehozása

Engedélyezni szeretné, hogy jelentkezzen be az Azure Active Directory-felhasználók hogy ki kell építenie belül az Evidence.com alkalmazás hozzáférést. Ez a szakasz ismerteti, hogyan Evidence.com belül Azure AD-felhasználói fiókok létrehozása

**A felhasználói fiókok Evidence.com kiépítése:**

1.  Egy webböngészőablakban jelentkezzen be a Evidence.com vállalati webhely rendszergazdaként.

2.  Nyissa meg a **rendszergazda** fülre.

3.  Kattintson a **felhasználó hozzáadása**.

4.  Kattintson a **Hozzáadás** gombra.

5.  A hozzáadott felhasználó **E-mail címét** meg kell egyezniük a felhasználónév azoknak a felhasználóknak ki, hogy hozzáférést kíván Azure AD. Ha a felhasználónév és e-mail címe nem a szervezet értéke megegyezik, használhatja a **Evidence.com > attribútumok > Single Sign-On** szakaszában az Azure klasszikus portálra a nameidenitifer az e-mail címet kell Evidence.com küldött módosítása.


## <a name="assigning-users-to-evidencecom"></a>Felhasználók hozzárendelése Evidence.com

Az Access csúszkáját Evidence.com láthatja, hogy a kiépített AAD felhasználók azok belül az Azure klasszikus portál access kell rendelni.

**Felhasználók hozzárendelése Evidence.com:**

1.  A rövid útmutató az első lapján Evidence.com az Azure klasszikus portálon kattintson a **felhasználók számára Evidence.com hozzárendelése**.
 
2.  A **Megjelenítés** menüben válassza ki, hogy egy felhasználó vagy csoport hozzárendelése Evidence.com, és kattintson a Pipajeles gombra.
 
3.  A **felhasználók** listában jelölje ki a felhasználókat, akiknek szeretné Evidence.com hozzárendelése csoporthoz.
 
4.  A lábléc kattintson a **hozzárendelése** gombra.


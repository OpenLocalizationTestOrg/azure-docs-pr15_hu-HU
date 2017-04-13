<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Facebookon, a munkahelyén |} Microsoft Azure"
    description="Egyszeri bejelentkezés Azure Active Directory és a Facebook között a munkahelyén konfigurálásának ismertetése."
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
    ms.date="04/26/2016"
    ms.author="asmalser"/>


# <a name="tutorial-azure-active-directory-integration-with-facebook-at-work"></a>Oktatóprogram: Azure Active Directory-integráció a Facebookon, a munkahelyén

Ebben az oktatóanyagban célja mutatja, hogy a munkahelyén Facebook integrálása az Azure Active Directory (Azure Active Directory).

Munkahelyi Facebook integrálása az Azure Active Directory biztosít a következő előnyökkel jár: 

- Az Azure Active Directory hozzáféréssel rendelkező személyek Facebook munkahelyi megadhatja, hogy 
- Akkor is automatikusan kiépítése a felhasználó, aki rendelkezik hozzáféréssel a Facebookon, a munkahelyén fiókjának
- Engedélyezheti a felhasználóknak, hogy automatikusan első aláírással a Facebook-fiókját, munkahelyi (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen 

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).


## <a name="prerequisites"></a>Előfeltételek 

Azure Active Directory integráció a CS csillagok van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egyszeri bejelentkezés a munkahelyi Facebook engedélyezve

Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/). 


## <a name="adding-facebook-at-work-from-the-gallery"></a>Facebook hozzáadása munkahelyi a gyűjteményből
Adja meg a Facebook-integrációt az Azure Active Directory munkahelyi, szüksége a gyűjtemény munkahelyi Facebook hozzáadása a felügyelt szoftver alkalmazások listájában.

**Gyűjtemény használatával adhat hozzá Facebook munkahelyi a, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.
    
    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be **a munkahelyi Facebook**.

7. Az eredmény ablaktáblában jelölje ki a **Facebookon, a munkahelyén**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.


### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz ismerteti, hogyan engedélyezése a felhasználóknak a Facebookon, a munkahelyén Azure Active Directory összevonási alapján a SAML protokoll használata a fiók hitelesítést végezni.

**Állítson be Azure Active Directory egyszeri bejelentkezés a munkahelyi Facebook, hajtsa végre az alábbi lépéseket:**

1.  Miután megadta a Facebook munkahelyi az Azure klasszikus portálon, kattintson a **Konfigurálása Single Sign-On**.

2.  Az **Alkalmazás URL-címet konfigurálni** képernyőn írja be az URL-cím hol felhasználók fog bejelentkezni munkahelyi alkalmazás a Facebook. Ez a munka bérlői webhely URL-címen Facebook (Példa: https://example.facebook.com/). Miután elkészült, kattintson a **Tovább**gombra.

3.  A különböző webes böngészőablakban jelentkezzen be munkahelyi vállalat webhelyén Facebook rendszergazdaként.

4. Kövesse az alábbi URL-címen Facebook konfigurálása a munkahelyén használja az Azure Active Directory-identitásszolgáltató: [https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers)

5.  Miután elkészült, térjen vissza a böngészőablakot, rajta az Azure klasszikus portálon, jelölje be a jelölőnégyzetet be nem fejezi az eljárás megerősítéséhez, majd kattintson a **Tovább gombra** , és a **teljes**.


## <a name="automatically-provisioning-users-to-facebook-at-work"></a>A felhasználók Facebook-fiókját, munkahelyi automatikusan kiépítése

Azure Active Directory támogatja az azt jelenti, hogy automatikusan szinkronizálása saját konfigurációjuk hozzárendelt felhasználók Facebook-fiókját, munkahelyi fiók adatait. Ez az automatikus szinkronizálását bemutató lehetővé teszi, hogy a Facebook, a munkahelyén engedélyezése a felhasználóknak az előtt kísérel meg, hogy jelentkezzen be az első alkalommal őket az access szüksége van az adatok. Azt is vonja kiépítése felhasználók Facebook, a munkahelyén, amikor az access az Azure Active Directory visszavonták.

Az automatikus kiépítési beállítható úgy **konfigurálása fiók kiépítési** az Azure klasszikus portál ablakban gombra kattintva.

Hogyan kell beállítani, hogy az automatikus kiépítési további részleteket lásd: [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)


## <a name="assigning-users-to-facebook-at-work"></a>Felhasználók hozzárendelése a Facebookon, a munkahelyén

Az Access csúszkáját a munkahelyén Facebook láthatja, hogy a kiépített AAD felhasználók azok belül az Azure klasszikus portál access kell rendelni.

**A Facebook-fiókját, munkahelyi hozzárendelését a felhasználókhoz:**

1.  Kattintson a start lap facebookos munkahelyi az Azure klasszikus portálon, **hozzárendelheti a partnereket**.

2.  A **Megjelenítés** menüben válassza ki, hogy egy felhasználó vagy csoport hozzárendelése a Facebookon, a munkahelyén, és kattintson a Pipajeles gombra.

3.  Az eredményül kapott listában válassza a felhasználók vagy csoport, kinek szeretne rendelni munkahelyi Facebook.

4.  A lábléc kattintson a **hozzárendelése** gombra.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_04.png





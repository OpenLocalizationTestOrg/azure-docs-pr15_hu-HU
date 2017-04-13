<properties
    pageTitle="Mik azok a Azure MFA alkalmazás jelszavakat?"
    description="Ezen az oldalon segít a felhasználók megtudhatják, hogy Mik azok a jelszavak alkalmazást, és mire használhatók a tekintettel Azure MFA."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>



# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a>Mik azok a alkalmazás jelszavak Azure többtényezős hitelesítést?

Egyes nem böngésző-alkalmazásokat, például az Apple natív e-mail ügyfél által használt Exchange Active Sync jelenleg nem támogatja a többtényezős hitelesítés. Többtényezős hitelesítés felhasználónként érhető el. Ez azt jelenti, hogy ha engedélyezve van a felhasználó többtényezős hitelesítéshez, és azok nem böngésző alkalmazást használni próbál, lesz sikertelen. Lehetővé teszi a egy alkalmazás jelszót fordul elő.

>[AZURE.NOTE] Az Office 2013 ügyfélprogramhoz a modern hitelesítési
>
> Az Office 2013-ügyfélprogramok (beleértve az Outlookot) most támogatja az új hitelesítési protokollok, és engedélyezhető többtényezős hitelesítés támogatására.  Ez azt jelenti, hogy miután engedélyezte a, alkalmazás jelszavak nem szükségesek az Office 2013-ügyfeleknek való használatra.  További információt [az Office 2013 modern hitelesítési nyilvános mintán bejelentve](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/)követheti.

## <a name="how-to-use-app-passwords"></a>Alkalmazás a jelszavak használatáról

Az alábbiakban néhány dolog, amit emlékezni fog alkalmazás a jelszavak használatáról.

- A tényleges jelszó automatikusan létrejön, és nem adja meg a felhasználót. Mivel az automatikusan generált jelszó nehezebb támadó találat, és biztonságosabb.
- Jelenleg csak 40 jelszavak felhasználónként legfeljebb. Hozzon létre egy újat elérte a maximális után próbál, amikor a rendszer kéri, törölni egyet az alkalmazást a meglévő jelszavak annak érdekében, hogy hozzon létre egy újat.
- Javasolt, hogy alkalmazás jelszavak hozható létre, és nem az alkalmazás egy jutó eszközt. Például egy alkalmazás jelszó létrehozása a laptopján, és használja, hogy a jelszó alkalmazás az összes meg, hogy a laptopon az alkalmazásait.
- Az első alkalommal bejelentkezik kapnak egy alkalmazás jelszót.  Ha a újak van szüksége, létrehozhat őket.

![A telepítő](./media/multi-factor-authentication-end-user-app-passwords/app.png)

Ha befejezte az alkalmazás jelszó, ez helyett használata a nem a böngészőben az alkalmazások az eredeti jelszót.  Így például, ha a többtényezős hitelesítés és az Apple natív e-mail ügyfél a telefonon.  Alkalmazás jelszót használja, így átlépése többtényezős hitelesítés és továbbra is működnek.

## <a name="creating-and-deleting-app-passwords"></a>Létrehozásáról és törléséről alkalmazás jelszavak
Az első bejelentkezés során adják meg az alkalmazás jelszó használható.  Emellett meg is létrehozhat és törölhet alkalmazás jelszavak később.  Hogyan teheti ezt attól függ, hogy hogyan többtényezős hitelesítést használ.  Ezek közül választhat, hogy a legtöbb vonatkozik.

Többtényezős hitelesítési használata|Leírás
:------------- | :------------- |
[Az Office 365-tel használható](#creating-and-deleting-app-passwords-with-office-365)|  Ez azt jelenti, hogy fog szeretne létrehozni a jelszavak alkalmazást az Office 365 portálon keresztül.
[nem tudom](#creating-and-deleting-app-passwords-with-myapps-portal)|Ez azt jelenti, érdemes [https://myapps.microsoft.com](https://myapps.microsoft.com) keresztül alkalmazás jelszavak létrehozása
[A Microsoft Azure használható](#create-app-passwords-in-the-azure-portal)| Ez azt jelenti, hogy érdemes létrehozása az Azure portál alkalmazás jelszavukat.

## <a name="creating-and-deleting-app-passwords-with-office-365"></a>Létrehozásáról és törléséről alkalmazás jelszavak az Office 365-tel

Ha az Office 365-ben használja a többtényezős hitelesítés kívánt létrehozása és törlése az Office 365 portálon keresztül alkalmazás jelszavakat.

### <a name="to-create-app-passwords-in-the-office-365-portal"></a>Alkalmazás jelszavak létrehozása az Office 365 portálon
--------------------------------------------------------------------------------

1. Jelentkezzen be az [Office 365-portálon](https://login.microsoftonline.com/).
2. Jobb felső sarokban válassza ki a listáját megjelenítő vezérlővel, és válassza az Office 365-beállítások.
3. Kattintson a biztonság fokozása ellenőrzése.
4. A jobb oldalon a hivatkozásra, amely szerint **frissítse a fiók biztonsági használt Telefonszámaim.** 
 ![Beállítása](./media/multi-factor-authentication-end-user-manage/o365a.png)
5. Ezzel eljut az oldalt, amely lehetővé teszi, hogy a beállítások módosítása.
![Felhőalapú](./media/multi-factor-authentication-end-user-manage/o365b.png)
6. További biztonsági az ellenőrzés melletti tetején kattintson a **alkalmazás jelszavak.**
7. Kattintson a **létrehozása**gombra.
![Felhőalapú](./media/multi-factor-authentication-end-user-app-passwords-create-o365/apppass.png)
8. Írjon be egy nevet az alkalmazás jelszót, és kattintson a **Tovább**gombra.
![Az alkalmazás jelszó létrehozása](./media/multi-factor-authentication-end-user-app-passwords/create1.png)
9. Az alkalmazás jelszót másolja a vágólapra, és illessze be az alkalmazás.
![Az alkalmazás jelszó létrehozása](./media/multi-factor-authentication-end-user-app-passwords/create2.png)


### <a name="to-delete-app-passwords-using-the-office-365-portal"></a>Az Office 365 portálon alkalmazás jelszavak törlése
--------------------------------------------------------------------------------


1. Jelentkezzen be az [Office 365-portálon](https://login.microsoftonline.com/).
2. Jobb felső sarokban válassza ki a listáját megjelenítő vezérlővel, és válassza az Office 365-beállítások.
3. Kattintson a biztonság fokozása ellenőrzése.
4. A jobb oldalon a hivatkozásra, amely szerint **frissítse a fiók biztonsági használt Telefonszámaim.** 
 ![Beállítása](./media/multi-factor-authentication-end-user-manage/o365a.png)
5. Ezzel eljut az oldalt, amely lehetővé teszi, hogy a beállítások módosítása.
![Felhőalapú](./media/multi-factor-authentication-end-user-manage/o365b.png)
6. További biztonsági az ellenőrzés melletti tetején kattintson a **alkalmazás jelszavak.**
7. A törölni kívánt alkalmazás jelszót, mellett kattintson a **Törlés**parancsra.
![Az alkalmazás jelszó törlése](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)
8. A törlés megerősítéséhez az **Igen**gombra kattintva.
![A törlés megerősítéséhez](./media/multi-factor-authentication-end-user-app-passwords/delete2.png)
9. Alkalmazás jelszavát a törölt kattintson a **Bezárás gombra**.
![Zárja be](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)


## <a name="creating-and-deleting-app-passwords-with-myapps-portal"></a>Létrehozása és törlése az alkalmazások parancsot portál alkalmazás jelszavakat.
Ha nem biztos abban, hogy hogyan használhatja a többtényezős hitelesítés, majd meg is mindig létrehozása és törlése az alkalmazások parancsot portál alkalmazás jelszavukat.

### <a name="to-create-an-app-password-using-the-myapps-portal"></a>Az alkalmazás jelszót, az alkalmazások parancsot portálon létrehozása

1. Jelentkezzen be az- [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. A képernyő tetején jelölje ki a profilt.
3. Válassza a további biztonsági ellenőrzése.
![Felhőalapú](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. Ezzel eljut az oldalt, amely lehetővé teszi, hogy a beállítások módosítása.
![A telepítő](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)
5. További biztonsági az ellenőrzés melletti tetején kattintson a **alkalmazás jelszavak.**
6. Kattintson a **létrehozása**gombra.
![Az alkalmazás jelszó létrehozása](./media/multi-factor-authentication-end-user-app-passwords/create3.png)
7. Írjon be egy nevet az alkalmazás jelszót, és kattintson a **Tovább**gombra.
![Az alkalmazás jelszó létrehozása](./media/multi-factor-authentication-end-user-app-passwords/create1.png)
8. Az alkalmazás jelszót másolja a vágólapra, és illessze be az alkalmazás.
![Az alkalmazás jelszó létrehozása](./media/multi-factor-authentication-end-user-app-passwords/create2.png)

### <a name="to-delete-an-app-password-using-the-myapps-portal"></a>Az alkalmazás jelszót, az alkalmazások parancsot portálon törlése

1. Jelentkezzen be az- [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. A képernyő tetején jelölje ki a profilt.
3. Válassza a további biztonsági ellenőrzése.
![Felhőalapú](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. Ezzel eljut az oldalt, amely lehetővé teszi, hogy a beállítások módosítása.
![A telepítő](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)
5. További biztonsági az ellenőrzés melletti tetején kattintson a **alkalmazás jelszavak.**
6. A törölni kívánt alkalmazás jelszót, mellett kattintson a **Törlés**parancsra.
![Az alkalmazás jelszó törlése](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)
7. A törlés megerősítéséhez az **Igen**gombra kattintva.
![A törlés megerősítéséhez](./media/multi-factor-authentication-end-user-app-passwords/delete2.png)
8. Alkalmazás jelszavát a törölt kattintson a **Bezárás gombra**.
![Zárja be](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)


## <a name="create-app-passwords-in-the-azure-portal"></a>Alkalmazás jelszavak létrehozása az Azure-portálon

Ha többtényezős hitelesítés és Azure fog szeretne létrehozni az Azure portál alkalmazás jelszavukat.

### <a name="to-create-app-passwords-in-the-azure-portal"></a>Alkalmazás jelszavak létrehozása az Azure-portálon

1. Bejelentkezés az Azure adatkezelési portálra.
2. A képernyő tetején kattintson a jobb gombbal a felhasználó nevét, és válassza a további biztonsági ellenőrzési.
3. A proofup lapon, a képernyő tetején jelölje be az alkalmazás jelszavak
4. Kattintson a **létrehozása**gombra.
5. Írjon be egy nevet az alkalmazás jelszót, és kattintson a **Tovább** gombra
6. Az alkalmazás jelszót másolja a vágólapra, és illessze be az alkalmazás.
![Felhőalapú](./media/multi-factor-authentication-end-user-app-passwords-create-azure/app2.png)

### <a name="to-delete-app-passwords-in-the-azure-portal"></a>Az Azure-portálon alkalmazás jelszavak törlése

1. Bejelentkezés az Azure adatkezelési portálra.
2. A képernyő tetején kattintson a jobb gombbal a felhasználó nevét, és válassza a további biztonsági ellenőrzési.
3. További biztonsági az ellenőrzés melletti tetején kattintson a **alkalmazás jelszavak.**
4. A törölni kívánt alkalmazás jelszót, mellett kattintson a **Törlés**parancsra.
5. A törlés megerősítéséhez az **Igen**gombra kattintva.
6. Alkalmazás jelszavát a törölt kattintson a **Bezárás gombra**.
![Zárja be](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)

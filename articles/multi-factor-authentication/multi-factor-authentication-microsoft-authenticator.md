<properties
    pageTitle="Mobiltelefonok Microsoft Authenticator alkalmazás |} Microsoft Azure"
    description="Megtudhatja, hogy miként Azure hitelesítő a legújabb verzióra frissíteni."
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
    ms.date="08/22/2016"
    ms.author="kgremban"/>

# <a name="microsoft-authenticator"></a>A Microsoft hitelesítő

A Microsoft Authenticator alkalmazást egy további szintű biztonságot nyújt az Azure-fiók (például bsimon@contoso.onmicrosoft.com), a helyszíni munkahelyi fiók (például bsimon@contoso.com), vagy a Microsoft-fiókjával (például bsimon@outlook.com).

Az alkalmazás működése a két módszer egyikével:

- **Értesítés**. Az alkalmazás segíthetnek a jogosulatlan hozzáférés elkerülése érdekében a partnerek, és leállítja a csalárd tranzakciók közvetítheti értesítést a okostelefonon vagy táblagépen. Egyszerűen csak az értesítés és a ha jogos, jelölje be a **ellenőrzése**. Egyéb esetben bejelölheti a **Megtagadás**. Szóló értesítések letiltása információkért lásd: a Megtagadás és a jelentés csalás funkció használatával a többtényezős hitelesítést.

- **Jelszó az ellenőrzőkódot**. Az alkalmazás-OAuth ellenőrzőkódot létrehozásához a szoftver jogkivonat is használható. Akkor adja meg a kódot, amikor a rendszer kéri a bejelentkezési képernyőn a felhasználónevet és jelszót, valamint be az alkalmazás által biztosított. Az ellenőrzőkódot hitelesítési második űrlap biztosít.

Az verziójának megjelenése után a Microsoft Authenticator alkalmazás a régi Azure hitelesítő alkalmazás cserél.  Az Azure hitelesítő alkalmazás továbbra is működnek, de ha úgy dönt, helyezze át az új Microsoft Authenticator alkalmazásba, ez a cikk segítséget.  

## <a name="install-the-app"></a>Az alkalmazás telepítése

A Microsoft Authenticator alkalmazás [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)és [IOS](http://go.microsoft.com/fwlink/?Linkid=825073)érhető el.

## <a name="add-accounts-to-the-app"></a>Fiók felvétele az alkalmazásba

A Microsoft Authenticator alkalmazásba felvenni kívánt minden fiókhoz használja az alábbi eljárások egyikét.

### <a name="add-an-account-to-the-app-by-using-the-qr-code-scanner"></a>Fiók felvétele az alkalmazásba a QR-kódot képolvasó használatával

1. Nyissa meg a biztonsági ellenőrzési beállítások képernyőt.  Kapcsolatos tudnivalókat a képernyő eléréséhez olvassa el a [biztonsági beállítások módosítása](multi-factor-authentication-end-user-manage-settings.md)című témakört.

2. Jelölje ki a **beállítani**.

    ![A beállítás gombra a biztonsági-ellenőrzési beállításainak képernyőjén](./media/multi-factor-authentication-azure-authenticator/azureauthe.png)

    Ekkor megjelenik a QR-kódot, rajta a képernyőre.

    ![Képernyő, amely a QR-kódot tartalmaz](./media/multi-factor-authentication-azure-authenticator/barcode2.png)

3. Nyissa meg a Microsoft hitelesítő alkalmazást. A **fiók** képernyőn jelölje ki a **+**, és adja meg, hogy szeretné-e egy munkahelyi vagy iskolai fiókot.

    ![A partnerek képernyőn pluszjel](./media/multi-factor-authentication-azure-authenticator/addaccount3.png)

    ![A munkahelyi vagy iskolai fiókkal megadására szolgáló képernyő](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan.png)

4. A kamera segítségével a QR-kódot, és válassza a **Kész gombra** kattintva zárja be a QR-kódot képernyő.

    ![A beolvasás QR-kódot képernyője](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

    Ha nem működik megfelelően a kamera, adhatja meg a QR-kódot és az URL-cím manuális. További tudnivalókért olvassa el a [manuálisan az App fiók felvétele](#add-an-account-to-the-app-manually)című témakört.

5. Várja meg, amíg a fiók aktiválva van. Amikor befejezte az aktiválás, jelölje be a **Partner**.  Értesítés vagy egy Ellenőrzőkód ez küld a telefonon.  Jelölje ki az **ellenőrizni**.

    ![A képernyő, ahol kiválaszthatja való bejelentkezéshez ellenőrzése](./media/multi-factor-authentication-end-user-first-time-mobile-app/verify.png)

6. Ha a vállalat PIN-kódot a bejelentkezési ellenőrzési jóváhagyásáért van szüksége, adja meg.

    ![A PIN-kód megadása párbeszédpanel](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

7. PIN-kód bejegyzés befejezése után, jelölje be a **bezárása gombra**. Az ellenőrzés ezen a ponton sikeres kell lennie.
8. Azt javasoljuk, hogy a mobiltelefonszám megadása arra az esetre, az alkalmazás elveszíti hozzáférését. Adja meg az Ön országában, a legördülő listából, és írja be a mobiltelefonszámát ország neve melletti jelölőnégyzetet. Kattintson a **Tovább gombra**.
9. Ezen a ponton úgy állította be a kapcsolatfelvételi módot. Most pedig nem böngésző-alkalmazásokat, például az Outlook 2010 vagy régebbi alkalmazás jelszavának beállításához. Ha Ön nem használja az alkalmazások, válassza a **Kész gombra**. Egyéb esetben folytassa a következő lépéssel.

    ![Képernyő-alkalmazás jelszó létrehozása](./media/multi-factor-authentication-end-user-first-time-mobile-app/step4.png)

10. Nem böngésző alkalmazásait használja, ha a megadott alkalmazás jelszó másolása, és illessze be a jelszót az alkalmazásait. Egyes alkalmazások, például az Outlook és a Lyncet, című hogyan alakíthat át az e-mailben a jelszót az alkalmazás jelszót, és az alkalmazást az app jelszót a jelszó módosítása.
11. Válassza a **Kész gombra**.

Ekkor megjelenik az új fiók a **fiókok** képernyőn.

![Fiókok képernyőn](./media/multi-factor-authentication-azure-authenticator/accounts.png)

### <a name="add-an-account-to-the-app-manually"></a>Fiók manuális felvétele az alkalmazásba

1. Nyissa meg a biztonsági ellenőrzési beállítások képernyőt.  Kapcsolatos tudnivalókat a képernyő eléréséhez olvassa el a [biztonsági beállítások módosítása](multi-factor-authentication-end-user-manage-settings.md)című témakört.

2. Jelölje ki a **beállítani**.

    ![A beállítás gombra a biztonsági-ellenőrzési beállításainak képernyőjén](./media/multi-factor-authentication-azure-authenticator/azureauthe.png)

    Ekkor megjelenik a QR-kódot, rajta a képernyőre.  Megjegyzés: a kód és az URL-CÍMÉT.

    ![Képernyő, ahol a QR-kódot és az URL-címe](./media/multi-factor-authentication-azure-authenticator/barcode2.png)

3. Nyissa meg a Microsoft hitelesítő alkalmazást. A **fiók** képernyőn jelölje ki a **+**, és adja meg, hogy szeretné-e egy munkahelyi vagy iskolai fiókot.

    ![A partnerek képernyőn pluszjel](./media/multi-factor-authentication-azure-authenticator/addaccount3.png)

    ![A munkahelyi vagy iskolai fiókkal megadására szolgáló képernyő](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan.png)

4. A képolvasó válassza a **be manuálisan a kódot**.

    ![A beolvasás QR-kódot képernyője](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

5. Írja be a kódot és az URL-címet az alkalmazásban a megfelelő mezőkbe.

    ![Képernyő-kódot és URL-cím beírásával](./media/multi-factor-authentication-azure-authenticator/manual.png)

    ![Képernyő-kódot és URL-cím beírásával](./media/multi-factor-authentication-end-user-first-time-mobile-app/addaccount2.png)

6. Várja meg, amíg a fiók aktiválva van. Amikor befejezte az aktiválást, jelölje be a **Partner**. Értesítés vagy egy Ellenőrzőkód ez küld a telefonon. Jelölje ki az **ellenőrizni**.

Ekkor megjelenik az új fiók a **fiókok** képernyőn.

![Fiókok képernyőn](./media/multi-factor-authentication-azure-authenticator/accounts.png)

### <a name="add-an-account-to-the-app-by-using-touch-id"></a>Fiók felvétele az alkalmazásba érintéses azonosítójával

A Microsoft Authenticator alkalmazásba iOS támogatja az érintéses azonosítójával.  Azure többtényezős hitelesítés segítségével a szervezetek PIN-kód megkövetelése eszközöket. Érintéses azonosítójú iOS-felhasználók nem kell adnia egy PIN kódot. Ehelyett azokat a ujjlenyomat beolvasás, és válassza a **Jóváhagyás**.

Állítsa be a Microsoft Authenticator érintéses azonosító egyszerű. A normál ellenőrzési kérdés PIN-kóddal végrehajtása Ha az eszköz támogatja az érintéses azonosítója, Microsoft Authenticator fog azt automatikusan beállítani az adott fiókhoz.

![Érintéses ID beállítása ellenőrzése](./media/multi-factor-authentication-azure-authenticator/touchid1.png)

Az, hogy ponttól lehetőségre, van szükség, ellenőrizze a bejelentkezés esetén válassza ki a kapott leküldéses értesítést, és a PIN-kód beírása helyett a ujjlenyomat beolvasása.

![Leküldéses értesítést](./media/multi-factor-authentication-azure-authenticator/touchid2.png)

## <a name="uninstall-the-old-azure-authentication-app"></a>A régi Azure hitelesítési alkalmazás eltávolítása

Miután hozzáadta a minden fiók az új alkalmazásba, a régi alkalmazás távolíthatja el a telefonról.

## <a name="delete-an-account"></a>Fiók törlése

A Microsoft Authenticator alkalmazásból a fiók eltávolítását, válassza ki a fiókot, és válassza a **Törlés**.

![Törlése gomb](./media/multi-factor-authentication-azure-authenticator/remove.png)

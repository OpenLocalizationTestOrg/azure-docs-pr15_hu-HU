<properties
    pageTitle="A munkahelyi vagy iskolai fiók kétlépcsős hitelesítési beállítása"
    description="A vállalat Azure többtényezős hitelesítés állítja be, amikor a rendszer kéri, kétlépcsős hitelesítési regisztrálhat. Megtudhatja, hogy miként állította. "
    services="multi-factor-authentication"
    keywords="azure címtár, az active directory, a felhőben, az active directory oktatóprogram használata"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="pblachar"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="set-up-my-account-for-two-step-verification"></a>A két lépésből álló tartomány ellenőrzéséhez a fiók beállítása

Kétlépcsős hitelesítési lépése egy nagyobb biztonságot, amely segít a fiók azáltal, hogy mások bonthatja nehezebb. Ha ez a cikk az épp olvasott, valószínűleg kapott e-mailben a munkahelyi vagy iskolai felügyeleti többtényezős hitelesítés kapcsolatban. Vagy bejelentkezéskor, és esetleg a kapott egy üzenetben kéri, további biztonsági ellenőrzési beállításához. Ha ez az esetben **nem tud bejelentkezni mindaddig, amíg az automatikus-regisztrációs folyamat befejezése**.

Ez a cikk segít beállítása a **munkahelyi vagy iskolai fiókjával**. [Saját, személyes Microsoft-fiók kétlépcsős hitelesítési engedélyezni szeretné, ha kapcsolatos tudnivalók](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification)című részben kétlépcsős hitelesítési.

## <a name="determine-how-you-will-use-multi-factor-authentication"></a>Határozza meg, hogyan fog használni a többtényezős hitelesítés

Kétlépcsős hitelesítési kéri az azonosító két csatolt bejelentkezéskor működik. Első lépésként kérjük a felhasználónevét és jelszavát a szokásos módon. Ezután kapcsolattartó kattint, és azt, hogy tudja, hogy a telefon tartozik ellenőrizzük, hogy a bejelentkezési kísérletet volt jogos.  

Az első lépések a telepítés során, próbáljon bejelentkezni a fiókjába általában működnek, mint. Ha a rendszergazda kétlépcsős hitelesítési fiókját beállította, a rendszer kéri, az automatikus igénylésének megkezdéséhez. Ez a folyamat megkezdéséhez kattintva **most beállítása.**

![A telepítő](./media/multi-factor-authentication-end-user-first-time/first.png)

Az első kérdést a regisztrációs folyamat, hogyan szeretné számunkra, hogy kapcsolatba Önnel. Nézze meg a beállításokat a táblázatban, és nyissa meg a tartománybeállítási lépéseket mindegyik módszernek hivatkozások segítségével.

| Kapcsolatfelvételi mód | Leírás |
| --- | --- |
[Mobilalkalmazásban](#use-a-mobile-app-as-the-contact-method) | - **A tartomány ellenőrzéséhez értesítéseket.** Ez a beállítás verembe küldi, okostelefonon vagy táblagépen értesítést a hitelesítő alkalmazásba. Az értesítés megjelenítése, és ha jogos, válassza a **hitelesítés** az alkalmazásban. A munkahelyi vagy iskolai megkövetelheti PIN-kód megadása előtt, hitelesítést végezni.<br>- **Használja az ellenőrzőkódot.** Ebben a módban a hitelesítő alkalmazást egy Ellenőrzőkód 30 másodpercenként frissítő hoz létre. Írja be a bejelentkezési felület a legfrissebb ellenőrzőkódot.<br>A Microsoft Authenticator alkalmazás [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)és [IOS](http://go.microsoft.com/fwlink/?Linkid=825073)érhető el. |
[Hívás mobiltelefonon vagy szöveg](#use-your-mobile-phone-as-the-contact-method) | - **Telefonhívás** helyez el egy automatizált hanghívás a megadott telefonszámra. Fogadja a hívást, és nyomja le a # a telefon billentyűzet hitelesítést végezni.<br>- **Szöveges üzenet** egy Ellenőrzőkód tartalmazó szöveges üzenet befejeződik. Követően az üzenet szövegében a szöveges üzenet megválaszolása, vagy írja be a bejelentkezési felület számításba ellenőrzőkódot. |  
[Az Office telefonhívás](#use-your-office-phone-as-the-contact-method) | A megadott telefonszámhoz egy automatizált hanghívás helyezi. Fogadja a hívást, és a # megnyomja a telefon billentyűzeten hitelesítést végezni. |

## <a name="use-a-mobile-app-as-the-contact-method"></a>Használni kívánt kapcsolatfelvételi módot mobilalkalmazásban

Ezzel a módszerrel szükséges hitelesítő alkalmazás telepítését mobiltelefonon vagy táblagépen. A cikkben leírt lépésekkel a Microsoft Authenticator alkalmazást, amely a [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)és [IOS](http://go.microsoft.com/fwlink/?Linkid=825073)alapulnak.

1. A legördülő listából válassza ki a **mobilalkalmazásban** .
2. Jelölje ki **az ellenőrzéshez értesítéseket** vagy **használata ellenőrzőkódot**, majd válassza a **állíthat be**.

    ![További biztonsági ellenőrzési képernyő](./media/multi-factor-authentication-end-user-first-time-mobile-app/mobileapp.png)

3. A telefonon vagy táblagépen, nyissa meg az alkalmazást, és válassza a **+** egy fiókot beállítani. (Android-eszközökön, válassza a három pontra, majd a **fiók hozzáadása**.)
4. Adja meg, hogy szeretné-e a munkahelyi vagy iskolai fiók hozzáadásához. Ekkor megnyílik a QR-kódot képolvasó a telefonon. Ha nem működik megfelelően a kamera, jelölje be manuálisan a cég adatainak megadása. További tudnivalókért lásd: a [fiók kézi hozzáadása](#add-an-account-manually).  
5. Tekintse át a QR-kódot képet a mobilalkalmazás beállítása a képernyőn megjelenő.  Válassza a **Kész gombra** kattintva zárja be a QR-kódot képernyő.  

    ![A QR-kódot képernyő](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

6. A telefonos aktiválási befejeződése után jelölje be a **Partner**.  Ebben a lépésben a telefon vagy értesítést, vagy egy Ellenőrzőkód küld. Jelölje ki az **ellenőrizni**.  
7. Ha a vállalat PIN-kódot a bejelentkezési ellenőrzési jóváhagyásáért van szüksége, adja meg.

    ![A PIN-kód megadása párbeszédpanel](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

8. PIN-kód bejegyzés befejezése után, jelölje be a **bezárása gombra**. Az ellenőrzés ezen a ponton sikeres kell lennie.
9. Azt javasoljuk, hogy a mobiltelefonszám megadása arra az esetre, a mobilalkalmazásban elveszíti hozzáférését. Adja meg az Ön országában, a legördülő listából, és írja be a mobiltelefonszámát ország neve melletti jelölőnégyzetet. Kattintson a **Tovább gombra**.
10. Ezen a ponton kéri alkalmazás jelszavának nem böngésző alkalmazások, például az Outlook 2010 vagy a régebbi és az Apple gyártmányú eszközt használóknak a natív levelezőalkalmazást beállítása. Ennek oka az, hogy egyes alkalmazások nem támogatja a kétlépcsős hitelesítési. Ha nem használja az alkalmazások, kattintson a **kész** gombra, és ugorja át a hátralévő lépéseket.
11. Az alkalmazások használatakor másolása az alkalmazás jelszót megadott, és illessze be az alkalmazás helyett a normál jelszavát. Több alkalmazások-et is alkalmazás ugyanazt a jelszót. Ha további információra [alkalmazás jelszavakkal Súgó].
12. Kattintson a **kész**gombra.


### <a name="add-an-account-manually"></a>Fiók manuális felvétele
Ha azt szeretné,-fiók felvétele a mobilalkalmazás manuálisan, a QR-olvasó használata helyett kövesse az alábbi lépéseket.

1. Kattintson a **fiók kézi megadása** gombra.  
2. Írja be a kódot és ugyanazon a lapon megtekintheti a vonalkódot kapott URL-CÍMÉT. Ez az információ a mobilalkalmazás kerül a **kódot** és **URL-cím** mezőbe.

    ![A telepítő](./media/multi-factor-authentication-end-user-first-time-mobile-app/barcode2.png)

3. Az aktiválás befejeződése után jelölje be a **Partner**. Ebben a lépésben a telefon vagy értesítést, vagy egy Ellenőrzőkód küld. Jelölje ki az **ellenőrizni**.

## <a name="use-your-mobile-phone-as-the-contact-method"></a>A mobiltelefon használni kívánt kapcsolatfelvételi módot

1. Jelölje ki a **Hitelesítési telefonját** a legördülő listából.  

    ![A telepítő](./media/multi-factor-authentication-end-user-first-time-mobile-phone/phone.png)  

2. Az Ön országában válassza a legördülő listából, és írja be a mobiltelefonszámát.
3. Válassza ki a telefonra – szöveg vagy -hívás használni szeretne.
4. Válassza a **partner,** ellenőrizze a telefonszámát. Attól függően, hogy a módot választotta, a Microsoft küldjön Önnek, szöveget vagy hívja Önt. Kövesse a képernyőn megjelenő utasításokat, és válassza az **ellenőrzés**.
5. Ezen a ponton kéri alkalmazás jelszavának nem böngésző alkalmazások, például az Outlook 2010 vagy a régebbi és az Apple gyártmányú eszközt használóknak a natív levelezőalkalmazást beállítása. Ennek oka az, hogy egyes alkalmazások nem támogatja a kétlépcsős hitelesítési. Ha nem használja az alkalmazások, kattintson a **kész** gombra, és ugorja át a hátralévő lépéseket.
6. Az alkalmazások használatakor másolása az alkalmazás jelszót megadott, és illessze be az alkalmazás helyett a normál jelszavát. Több alkalmazások-et is alkalmazás ugyanazt a jelszót. Ha további információra [alkalmazás jelszavakkal Súgó].
7. Kattintson a **kész**gombra.

## <a name="use-your-office-phone-as-the-contact-method"></a>Az irodai telefon használni kívánt kapcsolatfelvételi módot

1. Jelölje ki a legördülő lista **Irodai telefon**  

    ![A telepítő](./media/multi-factor-authentication-end-user-first-time-office-phone/office.png)  

2. A telefonszám mezőben a program automatikusan kitölti a vállalat kapcsolattartási adatait. Ha a szám hibás vagy hiányzó, kérje meg a rendszergazdát, hogy a módosításokat.
4. Jelölje ki a **partnert,** ellenőrizze a telefonszámát, és hogy hívja fel a telefonszámot. Kövesse a képernyőn megjelenő utasításokat, és válassza az **ellenőrzés**.
5. Ezen a ponton kéri alkalmazás jelszavának nem böngésző alkalmazások, például az Outlook 2010 vagy a régebbi és az Apple gyártmányú eszközt használóknak a natív levelezőalkalmazást beállítása. Ennek oka az, hogy egyes alkalmazások nem támogatja a kétlépcsős hitelesítési. Ha nem használja az alkalmazások, kattintson a **kész** gombra, és ugorja át a hátralévő lépéseket.
6. Az alkalmazások használatakor másolása az alkalmazás jelszót megadott, és illessze be az alkalmazás helyett a normál jelszavát. Több alkalmazások-et is alkalmazás ugyanazt a jelszót. További információt a [melyek az alkalmazás jelszavak](multi-factor-authentication-end-user-app-passwords.md)látható.
7. Kattintson a **kész**gombra.

## <a name="next-steps"></a>Következő lépések

- A használni kívánt beállításokat, és [két részből álló-ellenőrzési beállításainak kezelése](multi-factor-authentication-end-user-manage-settings.md) módosítása
- Állítsa be az [alkalmazás jelszavak](multi-factor-authentication-end-user-app-passwords.md) natív eszköz appok használatához kétlépcsős hitelesítési nem támogató.
- Nézze meg a [Microsoft hitelesítő alkalmazás](multi-factor-authentication-microsoft-authenticator.md) gyors, biztonságos hitelesítéshez, akkor is, ha nincs telepítve a cella szolgáltatás.

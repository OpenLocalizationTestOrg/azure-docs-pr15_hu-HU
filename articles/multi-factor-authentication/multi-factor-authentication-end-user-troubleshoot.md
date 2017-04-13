<properties
    pageTitle="Kétlépcsős hitelesítési elhárítása |} Microsoft Azure"
    description="A dokumentum felhasználók információt nyújt a Mi a teendő, ha az azok belefutna egy problémába, az Azure többtényezős hitelesítés."
    services="multi-factor-authentication"
    keywords = "többtényezős hitelesítést ügyfél, a hitelesítési probléma, a korrelációs azonosító"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="having-trouble-with-two-step-verification"></a>Gondjai vannak a két lépésből álló ellenőrzése

Ez a cikk azt ismerteti, hogy a kétlépcsős hitelesítési tapasztalható problémák elhárításához. Ha a problémát tapasztal nem szerepel itt, adjon visszajelzést részletes megjegyzések szakaszában, hogy továbbfejlesztését.

## <a name="i-lost-my-phone-or-it-was-stolen"></a>Elvesztettem telefonomon vagy meg lett ellopott

Kétféleképpen visszatérhet fiókjára. Az első, hogy jelentkezzen be a másik hitelesítési telefonszámát, ha egy be van állítva. A második pedig a kérje a rendszergazdát, hogy törölje a beállításokat.

Ha telefonja elvesztésének vagy ellopott, is javasoljuk, hogy van-e a rendszergazda, az alkalmazás jelszavak alaphelyzetbe állítása, és törölje a jelet minden tárolja az eszközök. Ha a rendszergazdát, hogy nem ehhez hogyan, irányítsa munkatársait Ez a cikk: [felhasználók kezelése és eszközöket](multi-factor-authentication-manage-users-and-devices.md#delete-users-existing-app-passwords).


### <a name="use-an-alternate-phone-number"></a>Másodlagos telefonszámot használata

Ha úgy állította be több ellenőrzési beállítások, beleértve a másodlagos telefonszámot vagy egy másik eszközön hitelesítő alkalmazás is használhatja az alábbiak egyike bejelentkezni.

Jelentkezzen be a másodlagos telefonszámot, kövesse az alábbi lépéseket:

1. Jelentkezzen be a szokásos módon.
2. Amikor további-fiók ellenőrzése kéri, válassza a **egy másik ellenőrző lehetőséget**.

    ![Különböző ellenőrzése](./media/multi-factor-authentication-end-user-manage/differentverification.png)

3. Jelölje be a telefonszámot, amelyen van hozzáférése.

    ![Telefonszámok](./media/multi-factor-authentication-end-user-manage/altphone2.png)

4. Miután Ön vissza a fiókját, [a beállítások kezelése](multi-factor-authentication-end-user-manage-settings.md) a hitelesítési telefonszám módosítása.

>[AZURE.IMPORTANT]
>Fontos, hogy állítsa be a hitelesítés másodlagos telefonszámot. Ha az elsődleges telefonszám és a mobilalkalmazásban ugyanazt a telefonon, van szüksége egy harmadik lehetőséget, ha a telefon elveszett vagy ellopott.

### <a name="clear-your-settings"></a>A beállítások törlése

Ha nem konfigurálta hitelesítési másodlagos telefonszámot, majd be kell súgó forduljon a rendszergazdához. Hogy törölje a beállításokat, így a következő alkalommal jelentkezik be, akkor rendszer rákérdez, hogy [a fiók beállításához](multi-factor-authentication-end-user-first-time.md) újra.


## <a name="i-am-not-receiving-a-text-or-call-on-my-phone"></a>E nem érkeznek be a szöveget, vagy felhívhatja őket a telefonon

Több oka miért lehet próbál bejelentkezni, de nem jelenik meg a szöveget vagy a telefonhívás. Ha sikeresen arról szövegek és a hívásokat a telefonra múltbeli, majd az valószínűleg a phone kapcsolatot biztosító szolgáltatónál, nem a fiók problémát. Győződjön meg arról, hogy a helyes cella jel van, és próbálja kap szöveges üzenet ellenőrizze, hogy a telefon és a szolgáltatás csomag támogatja az SMS-EK.

Néhány percig, amíg a szöveg vagy -hívás már megvárta, ha a fiókba első leggyorsabb módja, próbálja ki egy másik lehetőséget.

1. Azon az oldalon, a tartomány ellenőrzéséhez vár, jelölje ki **egy másik ellenőrző lehetőséget** .

    ![Különböző ellenőrzése](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

2. Jelölje ki a használni kívánt telefon számát vagy a kézbesítési módszer.

    Ha több ellenőrzési kódok, akkor működik, csak a legújabb egyet.

Ha nincs beállítva egy másik módszert, kérje a rendszergazda, és kérje meg, hogy törölje a beállításokat. A következő alkalommal jelentkezik be, amikor kéri [többtényezős hitelesítés](multi-factor-authentication-end-user-first-time.md) beállítása újra.


Ha gyakran hibás cella jel miatt késések, azt javasoljuk, a okostelefonon a [Microsoft hitelesítő alkalmazást](multi-factor-authentication-microsoft-authenticator.md) használ. Az alkalmazás véletlen biztonsági kódok való bejelentkezéshez használt hozhat létre, és a következő kódokat nincs szükség a minden cella jel vagy internetes kapcsolat.


## <a name="app-passwords-are-not-working"></a>Alkalmazás jelszó nem működik

Mindenekelőtt győződjön meg arról, hogy helyesen írt az alkalmazás jelszavát.  Ha továbbra sem működik próbálja meg a bejelentkezéskor a és a [jelszót hozhat létre új alkalmazást](multi-factor-authentication-end-user-app-passwords.md).  Ha ez nem működik, forduljon a rendszergazdához, és azokat [az alkalmazást a meglévő jelszavak törlése](multi-factor-authentication-manage-users-and-devices.md#delete-users-existing-app-passwords) , majd meg is hozzon létre egy újat.

## <a name="i-didnt-find-an-answer-to-my-problem"></a>Nem találom a probléma választ.

Ha ezeket a hibaelhárítási lépéseket Megpróbáltam, de továbbra is problémákat tapasztal futtatása, forduljon a rendszergazdához vagy a személy, aki meg többtényezős hitelesítés beállítása. Tudja meg, amely segít kell lennie.

Is egy kérdést az [Azure Active Directory-fórumok](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD) , vagy [lépjen kapcsolatba a támogatási](https://support.microsoft.com/contactus) tehetnek fel, és azt fogja megválaszolása a problémát, amint azt is.

Ha ügyfélszolgálatot, a következő információkat tartalmazza:

- **Felhasználói azonosító** – Mi az, hogy az e-mail címet, próbált bejelentkezéshez?
- **A hiba általános leírása** – milyen pontos hibaüzenet jelent meg látni?  Ha nincs hibaüzenet volt, ismertetik a szokatlan viselkedést tapasztal láthatta, részletesen.
- **Lap** – milyen lap volt a, ha a hiba látta (például az URL-cím)?
- **Hibakód** - Igen hibakód.
- **Munkamenet** - az adott munkamenetben azonosító igen.
- **Korrelációs azonosító** – mi volt a korrelációs azonosító kódot jön létre, amikor a felhasználó a hiba bemutattuk.
- **Időbélyeg** – mi volt a pontos dátum- és a hiba látta (például az időzóna)?

Az információk nagy a bejelentkezési lapon találhatók. Amikor a bejelentkezéskor idő nem ellenőrzi, jelölje be a **Részletek**.

![Jelentkezzen be a részletek](./media/multi-factor-authentication-end-user-troubleshoot/view_details.png)

Következő információk szerepeltetését segít a probléma mielőbbi megoldásában.

## <a name="related-topics"></a>Kapcsolódó témakörök
- [Két részből álló-ellenőrzési beállításainak kezelése](multi-factor-authentication-end-user-manage-settings.md)  
- [A Microsoft Authenticator alkalmazás – gyakori kérdések](multi-factor-authentication-app-faq.md)

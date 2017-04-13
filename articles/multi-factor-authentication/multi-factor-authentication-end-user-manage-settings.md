<properties
    pageTitle="A két lépésből álló-ellenőrzési beállításainak kezelése |} Microsoft Azure"
    description="Azure többtényezős hitelesítés, beleértve a kapcsolattartási adatok módosítását, és az eszközök beállítása használati kezelése."
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

# <a name="manage-your-settings-for-two-step-verification"></a>Két részből álló-ellenőrzési beállításainak kezelése

Ez a cikk használatához kétlépcsős hitelesítési vagy többtényezős hitelesítési beállítások módosításával kapcsolatos kérdésekre ad választ. Ha a fiókomba való bejelentkezéskor problémákat tapasztal, olvassa el az [gondjai vannak a kétlépcsős hitelesítési](multi-factor-authentication-end-user-troubleshoot.md) hibaelhárítási segítséget.


## <a name="where-to-find-the-settings-page"></a>Hol találhatók a beállítások lap
Függően a vállalati Azure többtényezős hitelesítés létezik néhány olyan helyen, ahol bármikor módosíthatja a beállításokat a telefonszám például.

Ha az informatikai rendszergazda meg egy adott URL-címet, vagy a lépések használatához kétlépcsős hitelesítési kezelése küldi el, kövesse ezeket az utasításokat. Egyéb esetben az alábbi lépéseket kell működniük a Mindenki más. Ha kövesse az alábbi lépéseket, de nem látható a ugyanazokat a beállításokat, ez azt jelenti, hogy a munkahelyi vagy iskolai testre szabott saját portálon. Kérje meg a rendszergazdát a hivatkozás az Azure többtényezős hitelesítés portálra.


1. Jelentkezzen be a [https://myapps.microsoft.com](https://myapps.microsoft.com)  
2. A képernyő tetején jelölje ki a **profilt**.  
3. Válassza a **további biztonsági ellenőrzése**.  

    ![Alkalmazások parancsot](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. A biztonság fokozása ellenőrzési lap betölti a beállításokkal.

    ![Proofup](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)


## <a name="i-want-to-change-my-phone-number-or-add-a-secondary-number"></a>A telefonszám módosítása és egy másodlagos szám hozzáadása a kívánt

Fontos, hogy állítsa be a hitelesítés másodlagos telefonszámot.  Mivel az elsődleges telefonszám és a mobilalkalmazásban valószínűleg ugyanazt a telefonon, a másodlagos telefonszámot nem fogja tudni visszatérhet ebbe a fiókjába a telefon elvesztése vagy ellopott csak úgy.

> [AZURE.NOTE]
> Ha nem, van hozzáférésük ahhoz a elsődleges telefonszámát, és segítségre van szüksége a fiókjába első, olvassa el a a súgótémakörök [gondjai vannak a kétlépcsős hitelesítési](multi-factor-authentication-end-user-troubleshoot.md)című témakört.

**Az elsődleges telefonszám módosítása:**  

1. A biztonság fokozása ellenőrzése lapon jelölje ki a szövegdobozt a jelenlegi telefonszámot, és szerkessze azt az új telefonszámot.  
2. Válassza a **Mentés**.  
3. Ha a számot, amelyet a használni kívánt ellenőrzési beállítás-et, van előtt mentheti őket az új számot megerősítéséhez.  


**Másodlagos telefonszámot hozzáadása:**  

1. A biztonság fokozása ellenőrzés lapon jelölje be az **másik hitelesítési phone.**  
2. Írja be a szövegmezőbe a másodlagos telefonszámot.  
3. SELECT **Mentse** a módosításokat elkészült és.  


## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-to-a-new-one"></a>Hogyan lehet Microsoft Authenticator karbantartása: a régi eszközről és áthelyezése egy új?
Amikor az alkalmazás eltávolítása az eszközről, vagy állítsa alaphelyzetbe az eszközt, kattintson a háttér az aktiválás nem távolítja el. A [áthelyezése egy új eszközzel](multi-factor-authentication-microsoft-authenticator.md#how-to-move-to-the-new-microsoft-authenticator-app)című témakörben ismertetett lépéseket kell használni.

## <a name="next-steps"></a>Következő lépések
- Hibaelhárítási tippek és [gondjai vannak a kétlépcsős hitelesítési](multi-factor-authentication-end-user-troubleshoot.md) súgója
- Állítsa be az összes alkalmazás használatához kétlépcsős hitelesítési nem támogató [alkalmazás jelszavak](multi-factor-authentication-end-user-app-passwords.md) .

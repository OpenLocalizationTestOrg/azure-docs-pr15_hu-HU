<properties
    pageTitle="Problémamegoldás: Azure AD-jelszó Management |} Microsoft Azure"
    description="Jelszókezelési Azure Active Directory, például a alaphelyzetbe állítása, módosítása, visszaírást, regisztráció, gyakori hibaelhárítási lépéseit és információk: tartalmazza, amikor segítséget keres."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="asteen"/>

# <a name="how-to-troubleshoot-password-management"></a>Jelszavak kezelését hibák elhárítása

> [AZURE.IMPORTANT] **Itt azért, mert netán problémákat tapasztal bejelentkezéskor?** Ha igen, [az alábbiakban hogyan módosíthatja, és a saját jelszó alaphelyzetbe állítása](active-directory-passwords-update-your-own-password.md).

Jelszó kezelésével kapcsolatos problémák merülnek fel, ha az alábbiakban segítséget. A legtöbb, előfordulhat, hogy felmerülő problémák megoldhatók a néhány egyszerű hibaelhárítási lépések, amelyek olvashat alatti telepítéssel kapcsolatos hibák elhárítása:

* [**Ha meg szeretné jeleníteni, ha segítségre van szüksége információk**](#information-to-include-when-you-need-help)
* [**Az adatkezelési portálon Azure jelszavak kezelését konfigurációval kapcsolatos problémák**](#troubleshoot-password-reset-configuration-in-the-azure-management-portal)
* [**Az adatkezelési portálon Azure jelszó felügyeleti jelentések kapcsolatos problémák**](#troubleshoot-password-management-reports-in-the-azure-management-portal)
* [**Problémát tapasztal a jelszó alaphelyzetbe állítása regisztrációs portál**](#troubleshoot-the-password-reset-registration-portal)
* [**Problémát tapasztal a jelszó alaphelyzetbe állítása portál**](#troubleshoot-the-password-reset-portal)
* [**Jelszó visszaírást tartalmazó kimutatások problémák**](#troubleshoot-password-writeback)
  - [Jelszó visszaírást eseménynaplójának hibakódok esetén](#password-writeback-event-log-error-codes)
  - [Jelszó visszaírást kapcsolódási problémák](#troubleshoot-password-writeback-connectivity)

Ha az alábbi hibaelhárítási lépéseket Megpróbáltam, és továbbra is problémákat tapasztal futtatása, tegye közzé kérdését az [Azure Active Directory-fórumok](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD) , vagy lépjen kapcsolatba a támogatási és azt fogja nézze meg a problémát, amint azt is.

## <a name="information-to-include-when-you-need-help"></a>Ha meg szeretné jeleníteni, ha segítségre van szüksége információk

Ha a probléma az alábbi útmutatást nem oldja meg, a támogatási szakemberei is fordulhat. Ha Ön kapcsolatba léphet az illetővel, ajánlott az alábbi információkat tartalmazzák:

 - **A hiba általános leírása** – milyen pontos hibaüzenet jelent, a felhasználó lásd:?  Ha nincs hibaüzenet volt, ismertetik a szokatlan viselkedést tapasztal láthatta, részletesen.
 - **Lap** – milyen lap volt a, ha a hiba látta (például az URL-cím)?
 - **Dátum / idő és időzóna** – mi volt a pontos dátum- és a hiba látta (például az időzóna)?
 - **Támogatja a kód** – mi volt a támogatási kód jön létre, amikor a felhasználó látta a hiba (a keresett elem, Reprodukálja a hibát, majd kattintson a kód támogatja a hivatkozásra a képernyő alján, és küldje el a támogatási adatbázismodellbe a globálisan egyedi azonosítója, amelynek eredménye).
   - Ha egy lapon a képernyő alján a támogatási kód nélkül, nyomja le az F12 billentyűt, és a biztonsági AZONOSÍTÓK és CID keresése, és a támogatási adatbázismodellbe e két eredmények küldése.

    ![][001]

 - **Felhasználói azonosító** – mi volt a felhasználó, aki a hiba (pl. látta azonosítójauser@contoso.com)?
 - **A felhasználó adatai** : volt a szövetséges felhasználó, a jelszó kivonat szinkronizált, csak a felhő?  Megváltozott a felhasználó rendelkezik-AAD Premium vagy AAD egyszerű licenccel?
 - **Alkalmazás eseménynaplójába** – Ha a jelszó visszaírást és a hiba esetén a helyszíni infrastruktúra, kérjük, zip-az alkalmazás eseménynaplójába az Azure AD Connect kiszolgálóról másolatának mentése és együtt a kérelem küldése.

Következő információk szerepeltetését fog segítse a szolgáltatás minél gyorsabban a probléma megoldására.


## <a name="troubleshoot-password-reset-configuration-in-the-azure-management-portal"></a>Jelszó alaphelyzetbe állítása konfigurációs az adatkezelési portálon Azure – problémamegoldás
Ha hiba alaphelyzetbe a jelszó beállítása, valószínűleg megoldható, hogy az alábbi hibaelhárítási lépéseket követve:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Hiba eset</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Mi a hiba nem látható a felhasználó?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Megoldás</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Nem látható a <strong>Felhasználói jelszó alaphelyzetbe állítása házirend </strong>szakaszban a <strong>beállítás</strong> lapon az Azure adatkezelési portálon</p>
            </td>
            <td>
              <p>A <strong>Felhasználói jelszó alaphelyzetbe állítása házirend </strong>szakasz még nem látható a <strong>beállítás</strong> lapon az adatkezelési portálon Azure.</p>
            </td>
            <td>
              <p>Ez akkor fordulhat elő, ha nem rendelkezik egy AAD Premium vagy AAD egyszerű licenccel a rendszergazda, a művelet végrehajtása. </p>
              <p>Is, meg azzal, AAD Premium vagy AAD egyszerű licenc hozzárendelése a szóban forgó rendszergazdai fiók navigálással, a <strong>licencek</strong> lapra, és próbálkozzon újra.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Nem látható a beállítási lehetőségek a <strong>Felhasználói jelszó alaphelyzetbe állítása házirend</strong> szakaszban ismertetett dokumentációjában közül.</p>
            </td>
            <td>
              <p>A <strong>Felhasználói jelszó alaphelyzetbe állítása házirend </strong>szakaszban látható, de csak jelző jelenik meg, alatta a <strong>Jelszó alaphelyzetbe állítása az engedélyezett felhasználók</strong> jelölőre.</p>
            </td>
            <td>
              <p>A felhasználói felületen a többi fog megjelenni, amikor a <strong>Felhasználók engedélyezve, a jelszó alaphelyzetbe állítása</strong> Üzenetjelölő vált <strong>igen.</strong></p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Egy adott beállítás nem látható.</p>
            </td>
            <td>
              <p>Például nem látható a <strong>felhasználó igazolnia kell a kapcsolattartási adataikat előtt eltelt napok száma</strong> beállítás a <strong>Felhasználói jelszó alaphelyzetbe állítása házirend</strong> (vagy a szakasz további példákat ugyanazt a problémát) keresztül görgetéskor.</p>
            </td>
            <td>
              <p>Sok elemet felhasználói felületének rejtett mindaddig, amíg szükség van rájuk. Próbálja meg, ha meg szeretné tekinteni, amely lehetővé teszi, hogy a lapon a beállításokat.</p>
              <p><a href="active-directory-passwords-customize.md#password-management-behavior">Jelszavak kezelését viselkedést</a> tapasztalta további tudnivalókat az összes elérhető megfelelő vezérlőelemeket.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Az <strong>Írja be újra a jelszavakat a helyszíni</strong> beállítás nem látható</p>
            </td>
            <td>
              <p>A <strong>Írja be újra a jelszavakat a helyszíni</strong> beállítás, nem látható a <strong>beállítás</strong> lapon az Azure adatkezelési portálon</p>
            </td>
            <td>
              <p>Ezt a beállítást csak akkor látható, ha a letöltött Azure AD Connect és beállított jelszó visszaírást. Ha már van szüksége, ezt a beállítást jelenik meg, és lehetővé teszi, hogy engedélyezése vagy letiltása a felhőből visszaírást.</p>
              <p>Lásd: a <a href="active-directory-passwords-getting-started.md#step-2-enable-password-writeback-in-azure-ad-connect">Jelszó visszaírást engedélyezése Azure AD Connect</a> ezzel kapcsolatban további információt.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-management-reports-in-the-azure-management-portal"></a>Jelszó felügyeleti jelentések az adatkezelési portálon Azure – problémamegoldás
Ha a jelszó felügyeleti jelentések használata esetén hibát tapasztal, bizonyára megoldható, hogy az alábbi hibaelhárítási lépéseket követve:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Hiba eset</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Mi a hiba nem látható a felhasználó?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Megoldás</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Nem látom a jelszó felügyeleti jelentések</p>
            </td>
            <td>
              <p>A <strong>jelszó alaphelyzetbe állítása tevékenység</strong> és a <strong>jelszó alaphelyzetbe állítása az nyilvántartási tevékenység</strong> jelentései nem láthatók a az <strong>Tevékenységnapló</strong> jelentéseket a <strong>jelentések</strong> fülre.</p>
            </td>
            <td>
              <p>Ez akkor fordulhat elő, ha nem rendelkezik egy AAD Premium vagy AAD egyszerű licenccel a rendszergazda, a művelet végrehajtása. </p>
              <p>Is, meg azzal, AAD Premium vagy AAD egyszerű licenc hozzárendelése a szóban forgó rendszergazdai fiók navigálással, a <strong>licencek</strong> lapra, és próbálkozzon újra.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Felhasználói regisztrációk többször megjelenítése</p>
            </td>
            <td>
              <p>Ha egy felhasználó regisztrálja a másodlagos e-mail Mobiltelefonról és biztonsági kérdések, hogy minden jelenjenek meg külön sorként helyett egy egyetlen sort.</p>
            </td>
            <td>
              <p>Jelenleg amikor a felhasználó regisztrálja, nem feltételezzük, hogy azok regisztrálja minden bemutató regisztrációs lapon. Emiatt azt jelenleg jelentkezzen minden egyes adatcsoportok egy külön esemény azonosítóként regisztrált.</p>
              <p>Ha szeretné az adatok összesítése, töltse le a jelentést, és nyissa meg az adatokat, mint a kimutatás rugalmasabb van az excel.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-the-password-reset-registration-portal"></a>A jelszó alaphelyzetbe állítása regisztrációs portál – problémamegoldás
Ha hiba felhasználó beállítása a jelszó-visszaállítás regisztrálásakor, valószínűleg megoldható, hogy az alábbi hibaelhárítási lépéseket követve:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Hiba eset</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Mi a hiba nem látható a felhasználó?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Megoldás</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>A címtár engedélyezve van a jelszó-visszaállítás</p>
            </td>
            <td>
              <p>A rendszergazda nem engedélyezte a szolgáltatás használatához.</p>
            </td>
            <td>
              <p>Váltás a <strong>Jelszó alaphelyzetbe állítása az engedélyezett felhasználók</strong> jelző <strong>Igen</strong> , és Azure Kezelőportálja könyvtár konfigurációs lapon kattintson a <strong>Mentés gombra</strong> . Azure Active Directory Premium vagy a rendszergazda, a művelet végrehajtása egyszerű licenccel kell van.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Felhasználó nem rendelkezik egy AAD Premium vagy AAD egyszerű licenccel</p>
            </td>
            <td>
              <p>A rendszergazda nem engedélyezte a szolgáltatás használatához.</p>
            </td>
            <td>
              <p>Azure Active Directory Premium vagy az Azure Active Directory egyszerű licenc hozzárendelése a felhasználó a <strong>licencek</strong> lapon, az adatkezelési portálon Azure. Azure Active Directory Premium vagy a rendszergazda, a művelet végrehajtása egyszerű licenccel kell van.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Hiba a kérelem feldolgozása</p>
            </td>
            <td>
              <p>Felhasználó számára megjelenik egy hibaüzenet arról, hogy:</p>
              <p>

              </p>
              <p>Hiba a kérelem feldolgozása </p>
              <p>Amikor megpróbál a jelszó alaphelyzetbe állítása.</p>
            </td>
            <td>
              <p>Ez számos problémákat okozhatják, de általában vagy egy szolgáltatást üzemszünetek vagy konfigurációs hiba, nem oldható meg ez a hiba oka. </p>
              <p>Ha ez a hibaüzenet jelenik meg, és azt van érintő vállalata, kérjük, forduljon a támogatási, és azt nyújt segítséget az amint lehet. Olvassa el <a href="#information-to-include-when-you-need-help">szeretné hozzáadni, ha segítségre van szüksége az adatok</a> biztosít a támogatási adatbázismodellbe gyors megoldást a támogatási való megtekintéséhez.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-the-password-reset-portal"></a>A jelszó alaphelyzetbe állítása portál – problémamegoldás
Ha hibaüzenetet, amikor egy felhasználó jelszavának alaphelyzetbe állítása, valószínűleg megoldható, hogy az alábbi hibaelhárítási lépéseket követve:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Hiba eset</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Mi a hiba nem látható a felhasználó?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Megoldás</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>A címtár engedélyezve van a jelszó-visszaállítás</p>
            </td>
            <td>
              <p>A fiók nincs engedélyezve a jelszó-visszaállítás</p>
              <p>Sajnáljuk, de a rendszergazdája nem állított be a fiókját, a szolgáltatással való használatra. </p>
              <p>

              </p>
              <p>Ha azt szeretné, hogy is kérje a rendszergazda segítségét a szervezet állítsa alaphelyzetbe jelszavát.</p>
            </td>
            <td>
              <p>Váltás a <strong>Jelszó alaphelyzetbe állítása az engedélyezett felhasználók</strong> jelző <strong>Igen</strong> , és Azure Kezelőportálja könyvtár konfigurációs lapon kattintson a <strong>Mentés gombra</strong> . Azure Active Directory Premium vagy a rendszergazda, a művelet végrehajtása egyszerű licenccel kell van.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Felhasználó nem rendelkezik egy AAD Premium vagy AAD egyszerű licenccel</p>
            </td>
            <td>
              <p>Közben automatikusan nem tud új nem rendszergazda fiókokhoz tartozó jelszavakat, azt is lépjen kapcsolatba a szervezet felügyeleti adunk meg.</p>
            </td>
            <td>
              <p>Azure Active Directory Premium vagy az Azure Active Directory egyszerű licenc hozzárendelése a felhasználó a <strong>licencek</strong> lapon, az adatkezelési portálon Azure. Azure Active Directory Premium vagy a rendszergazda, a művelet végrehajtása egyszerű licenccel kell van.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Directory engedélyezve van a jelszó alaphelyzetbe állítása, de a felhasználónak a hiányzó vagy helytelen formátumú hitelesítési adatait</p>
            </td>
            <td>
              <p>A fiók nincs engedélyezve a jelszó-visszaállítás</p>
              <p>Sajnáljuk, de a rendszergazdája nem állított be a fiókját, a szolgáltatással való használatra. </p>
              <p>

              </p>
              <p>Ha azt szeretné, hogy is kérje a rendszergazda segítségét a szervezet állítsa alaphelyzetbe jelszavát.</p>
            </td>
            <td>
              <p>Győződjön meg arról, hogy a felhasználó rendelkezik megfelelően formázott a naplófájlt a továbblépés előtt könyvtárában a partnerek adatait. Lásd: a <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">milyen adatokat használja, a jelszó-visszaállítás</a> kapcsolatos tudnivalókat konfigurálása hitelesítési adatot a címtárban, hogy a felhasználók nem látják ezt a hibát.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Directory engedélyezve van a jelszó alaphelyzetbe állítása, de a felhasználó csak akkor van kapcsolattartói adatok egy részének fájlon házirend beállítása két ellenőrző lépésben szükség esetén</p>
            </td>
            <td>
              <p>A fiók nincs engedélyezve a jelszó-visszaállítás</p>
              <p>Sajnáljuk, de a rendszergazdája nem állított be a fiókját, a szolgáltatással való használatra. </p>
              <p>

              </p>
              <p>Ha azt szeretné, hogy is kérje a rendszergazda segítségét a szervezet állítsa alaphelyzetbe jelszavát.</p>
            </td>
            <td>
              <p>Győződjön meg arról, hogy a felhasználó legalább két megfelelően konfigurált kapcsolatfelvételi mód (például mobiltelefonra és irodai telefon) a folytatás előtt. Lásd: a <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">milyen adatokat használja, a jelszó-visszaállítás</a> kapcsolatos tudnivalókat konfigurálása hitelesítési adatot a címtárban, hogy a felhasználók nem látják ezt a hibát.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>A jelszó-visszaállítás engedélyezve van a címtár és a felhasználó helyesen van beállítva, de a felhasználó nem tud kapcsolatba lépni </p>
            </td>
            <td>
              <p>Oops!  Váratlan hiba történt léphet kapcsolatba Önnel.</p>
            </td>
            <td>
              <p>Ez egy ideiglenes szolgáltatáshiba eredményét lehet vagy nincs megfelelően konfigurálva a kapcsolattartási adatok, amelyek nem sikerült tökéletesen megállapítani. Ha a felhasználó vár, 10 másodperc, próbálja meg újra, és megjelenik a "forduljon a rendszergazdához" hivatkozás. Gombra kattintva ismét próbáljon újra szállítása a a hívás, mivel a "forduljon a rendszergazdához" kattintva fog egy űrlap e-mailt küldjenek felhasználó, a jelszó vagy a globális rendszergazdák (a elsőbbségi sorrendben) kérése a jelszó alaphelyzetbe állítása az adott felhasználói fiók végre kell hajtani.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Felhasználó soha nem kapja meg a jelszó alaphelyzetbe állítása az SMS vagy telefonhívásban</p>
            </td>
            <td>
              <p>Felhasználói kattintással "szöveg én" vagy "általi felhívást kiváltó", és soha nem kap a semmit.</p>
            </td>
            <td>
              <p>Ez lehet az eredmény helytelen formátumú telefonszám a címtárban. Ügyeljen arra, hogy a telefonszám formátuma "+ ccc xxxyyyzzzzXeeee". Ha többet szeretne tudni a telefon formázás számok jelszó-visszaállítás való használatra, lásd: <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">milyen adatokat használja, a jelszó-visszaállítás</a>.</p>
              <p>Ha a szóban forgó felhasználó átirányítását kiterjesztése van szüksége, vegye figyelembe, hogy a jelszó-visszaállítás nem támogatja a bővítményeket, még akkor is, ha egy, a címtárban (ezek megtörténik a hívás előtt vannak hivatkozásból) ad meg. Próbáljon meg egy számot, kiterjesztés nélkül vagy a kiterjesztés integrálása a házi alközpontba kapcsolt telefonszámot.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Felhasználó soha nem kap e-mailek jelszó alaphelyzetbe állítása</p>
            </td>
            <td>
              <p>Felhasználó "me e-mail" kattint, és soha nem kap a semmit.</p>
            </td>
            <td>
              <p>A leggyakoribb a probléma oka az, hogy az üzenetet a levélszemét szűrés elutasítják. Jelölje be a levélszemét, az e-mailt levélszemétként vagy a törölt elemek mappát.</p>
              <p>Is győződjön meg arról, hogy a megfelelő e-mail vannak-e ellenőrzés az üzenet... rengeteg emberrel nagyon hasonlít e-mail címe van, és ellenőrzés az üzenet a megfelelő Beérkezett üzenetek kerüljenek. Ha az alábbi lehetőségek közül egyik sem működik, is lehetséges, hogy az e-mail címet, a címtárban hibás, ellenőrizze, hogy győződjön meg arról, hogy az e-mail címet a megfelelőt, és próbálkozzon újra. Ha többet szeretne tudni az e-mailek formázása a jelszó-visszaállítás való használatra címek lásd: <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">milyen adatokat használja, a jelszó-visszaállítás</a>.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Van állítható alaphelyzetbe a Jelszóházirend, de amikor egy rendszergazdai fiókkal jelszó-visszaállítás használ, a program nem állítja házirendhez</p>
            </td>
            <td>
              <p>A rendszergazdai fiókok a jelszavak alaphelyzetbe állítása ugyanazokat a beállításokat engedélyezve van a jelszó-visszaállítás, a levelezés és a mobiltelefonjára, függetlenül attól, hogy milyen házirend értéke a <strong>Configure</strong> lap <strong>Felhasználói jelszó alaphelyzetbe állítása házirend</strong> szakaszban látható.</p>
            </td>
            <td>
              <p>A <strong>Felhasználói jelszó alaphelyzetbe állítása házirend</strong> csoportjában a <strong>Configure</strong> lap konfigurált beállítások csak a felhasználóknak a szervezet vonatkoznak.</p>
              <p>A Microsoft kezeli, és vezérlők a rendszergazdai jelszó alaphelyzetbe állítása házirendet a legmagasabb szintű biztonság érdekében</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Felhasználói jelszó alaphelyzetbe állításához, egy nap múlva túl sokszor próbált akadályozni</p>
            </td>
            <td>
              <p>Felhasználó számára megjelenik egy üzenet szerint:</p>
              <p>

              </p>
              <p>Használjon másik lehetőséget.</p>
              <p>Már-fiók ellenőrzése az utolsó 1 óra a túl sokszor próbált. Biztonsági okokból fognak rendelkezésére állni Várjon 24 óra, próbálja meg újra a előtt. </p>
              <p>Ha azt szeretné, hogy is kérje a rendszergazda segítségét a szervezet állítsa alaphelyzetbe jelszavát.</p>
            </td>
            <td>
              <p>Automatikus szabályozási mechanizmusok azt végrehajtja a felhasználók letiltása megpróbálja túl sokszor be egy rövid ideig a a jelszavak alaphelyzetbe állítása. Ez akkor fordul elő, ha:</p>
              <ol class="ordered">
                <li>
Felhasználó megkísérel érvényesítése telefonszám 5 megszorzása egy óra.<br\><br\></li>
                <li>
Felhasználó megkísérel használja a biztonsági kérdések kapu 5 megszorzása egy órával.<br\><br\></li>
                <li>
Felhasználó megkísérel 5 megszorzása az egy órával a egy felhasználói fiókhoz tartozó jelszó alaphelyzetbe állítása.<br\><br\></li>
              </ol>
              <p>Probléma megoldásához kérje meg a felhasználót, hogy, Várjon 24 órát, a legutolsó kísérlet után, és a felhasználó így tudják partnerlistájukra jelszó alaphelyzetbe állítása.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Felhasználó által látott nézetet hiba partnerlistájukra Telefonszám ellenőrzése során</p>
            </td>
            <td>
              <p>Amikor megpróbál ellenőrizze a hitelesítési módot fogja használni a telefonon, megjelenik egy üzenet szerint:</p>
              <p>

              </p>
              <p>Helytelen a megadott telefonszámot.</p>
            </td>
            <td>
              <p>Ez akkor fordul elő, ha a megadott telefonszám nem egyezik meg a telefonszámot a fájlt.</p>
              <p>Győződjön meg arról, hogy a felhasználónak van beírása a teljes telefonszámot, többek között a terület és ország kódot, amikor megpróbál módszert a phone-alapú jelszó alaphelyzetbe állítása.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Hiba a kérelem feldolgozása</p>
            </td>
            <td>
              <p>Felhasználó számára megjelenik egy hibaüzenet arról, hogy:</p>
              <p>

              </p>
              <p>Hiba a kérelem feldolgozása </p>
              <p>Amikor megpróbál a jelszó alaphelyzetbe állítása.</p>
            </td>
            <td>
              <p>Ez számos problémákat okozhatják, de általában vagy egy szolgáltatást üzemszünetek vagy konfigurációs hiba, nem oldható meg ez a hiba oka. </p>
              <p>Ha ez a hibaüzenet jelenik meg, és azt van érintő vállalata, kérjük, forduljon a támogatási, és azt nyújt segítséget az amint lehet. Olvassa el <a href="#information-to-include-when-you-need-help">szeretné hozzáadni, ha segítségre van szüksége az adatok</a> biztosít a támogatási adatbázismodellbe gyors megoldást a támogatási való megtekintéséhez.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-writeback"></a>Jelszó visszaírást – problémamegoldás
Ha hibaüzenet, amikor engedélyezése, letiltása és jelszó visszaírást használ, valószínűleg megoldható, hogy az alábbi hibaelhárítási lépéseket követve:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Hiba eset</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Mi a hiba nem látható a felhasználó?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Megoldás</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Általános bevezetési és indítási hibáit</p>
            </td>
            <td>
              <p>Jelszó alaphelyzetbe állítása nem indul el az Azure AD Connect gépi alkalmazás eseménynaplójában 6800 hibával helyszíni.</p>
              <p>

              </p>
              <p>Bevezetési, összevont vagy jelszó kivonat után szinkronizált felhasználók nem tudják alaphelyzetbe állítani jelszavát.</p>
            </td>
            <td>
              <p>Ha engedélyezve van a jelszó visszaírást, akkor a szinkronizálási motor a felhőben bevezetési szolgáltatás telefonon a visszaírást tár konfigurálása (bevezetési) hívja meg. A hibák feltárása bevezetési során, vagy a WCF-végpont indítása a jelszó visszaírást közben eredményezi hibáinak az Azure AD Connect gépi eseménynaplójában eseménynaplójában talál.</p>
              <p>Az újraindítás ADSync szolgáltatás során visszaírást lett beállítva, ha a WCF-végpont fog elindulni. Azonban nem sikerül az Indítás végpontjának, ha azt fogja egyszerűen jelentkezzen be az esemény 6800 és mondja el a szinkronizálási szolgáltatás indítása. Ez az esemény jelenlétét azt jelenti, hogy a jelszó visszaírást végpontot felfelé nem kezdődött. Esemény naplóbejegyzések együtt az esemény (6800) eseménynaplójában talál részleteket összetevő megjelöli, miért az végpontot nem indítható el PasswordResetService elő. Tekintse át az Eseménynapló a hibák, és újra az Azure AD Connect indításakor, ha a jelszó visszaírást még mindig nem működik. Ha a probléma nem szűnik meg, próbálja meg letiltása és ismételt engedélyezése a jelszó visszaírást.</p>
            </td>
          </tr>
                    <tr>
            <td>
              <p>Amikor a felhasználó megkísérel a jelszó alaphelyzetbe állítása, illetve oldja fel a fiók engedélyezett jelszó visszaírást tartalmazó kimutatások, a művelet sikertelen lesz. Ezeken kívül az Azure AD Connect eseménynaplójának tartalmazó esemény látható: "szinkronizálás motor visszaadott érték: egy hiba hr = 800700CE, üzenet = a fájlnév vagy túl hosszú kiterjesztés" után a zárolás feloldása művelet fordul elő.
                            </p>
            </td>
            <td>
              <p>Ez akkor fordulhat elő, ha az Azure AD Connect vagy DirSync régebbi verziójával frissített volt. Azure AD Connect régebbi verzióiban való frissítés az Azure Active Directory kezelése ügynök fióknál újabb verziója fog 127 karakter hosszúságú jelszó 254 karakter jelszó beállítása. Az ilyen sokáig jelszavak működni az Active Directory-összekötő importálása és exportálása műveletek, de nem támogatják a zárolás feloldása művelet.
                            </p>
            </td>
            <td>
              <p>[Keresse meg az Active Directory-fiókját](active-directory-aadconnect-accounts-permissions.md#active-directory-account) Azure AD Connect és jelszavának alaphelyzetbe állítása legfeljebb 127 karaktereket tartalmaz. Ezután nyissa meg a **Szinkronizálási szolgáltatás** , a Start menüből. Nyissa meg azt az **összekötőket** , és keresse meg az **Active Directory-összekötő**. Jelölje ki azt, és válassza a **Tulajdonságok parancsot**. Nyissa meg a **hitelesítő adatok** lapot, és írja be az új jelszót. Válassza **az OK gombra** kattintva zárja be a lapot.
                            </p>
            </td>
          </tr>
                    <tr>
            <td>
              <p>A hiba visszaírást konfigurálása Azure AD Connect a telepítés során.</p>
            </td>
            <td>
              <p>Az utolsó lépésben az Azure AD Connect telepítési folyamatot, megjelenik egy hiba, amely jelzi, hogy a jelszó visszaírást nem konfigurálható.</p>
              <p>

              </p>
              <p>Az Azure Active Directory csatlakozás alkalmazás eseménynaplójába "Hiba első auth jogkivonat" hiba 32009 szöveggel tartalmazza.</p>
            </td>
            <td>
              <p>Ez a hiba akkor fordul elő, az alábbi két esetekben:</p>
              <ul>
                <li class="unordered">
Az Azure AD Connect telepítési folyamatot az elején megadott globális rendszergazdai fiók helytelen jelszót adott meg.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Szövetséges felhasználó használja a globális rendszergazdai fiókkal az Azure AD Connect telepítési folyamatot az elején megadott próbálta.<br\><br\></li>
              </ul>
              <p>Ez a hiba kijavításához győződjön meg arról, hogy Ön nem a globális rendszergazda szövetséges fiókkal megadott az Azure AD Connect telepítési folyamatot az elején, és a megadott jelszó helyességét.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>A hiba visszaírást konfigurálása Azure AD Connect a telepítés során.</p>
            </td>
            <td>
              <p>Az Azure AD Connect gépi Eseménynapló 32002 a PasswordResetService által jelzett hibát tartalmaz.</p>
              <p>

              </p>
              <p>Beolvassa a hiba: "hiba ServiceBus csatlakozik, a jogkivonat-szolgáltató nem tudott adja meg a biztonsági jogkivonat..."</p>
              <p>

              </p>
            </td>
            <td>
              <p>A legfelső szintű Ez a hiba oka az, hogy a jelszó alaphelyzetbe állítása szolgáltatás fut a helyszíni környezetben nem tud csatlakozni a szolgáltatási bus végpont a felhőben. Ez a hiba általában a szokásos módon okozza, hogy egy adott port vagy a webhely címét a kimenő kapcsolódási blokkolása tűzfal szabály.</p>
              <p>

              </p>
              <p>Győződjön meg arról, hogy a tűzfal lehetővé teszi, hogy a kimenő kapcsolatokat a következő:</p>
              <ul>
                <li class="unordered">
Minden forgalom fölé TCP 443-as (HTTPS)<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Kimenő kapcsolatok <br\><br\></li>
              </ul>
              <p>

              </p>
              <p>Szabályok frissített, indítsa újra az Azure AD Connect számítógépen, és a jelszó visszaírást kell ismét dolgozni.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Jelszó visszaírást végpont helyszíni nem érhető el</p>
            </td>
            <td>
              <p>Egyes összevont idő vagy a jelszó kivonat használata után a szinkronizált felhasználók nem tudják alaphelyzetbe állítani jelszavukat.</p>
            </td>
            <td>
              <p>Néhány ritkán a jelszó visszaírást szolgáltatás meghiúsulhat úgy, hogy megkezdésekor Azure AD Connect ismét indítsa újra. Ebben az esetben először ellenőrizze, hogy e jelszó visszaírást úgy tűnik, hogy engedélyezett prem. Ezt megteheti a Azure AD Connect varázslóval, vagy a powershell (lásd: HowTos szakasz fenti). Ha a funkciót úgy tűnik, hogy engedélyezve, próbálja meg engedélyezése vagy letiltása a szolgáltatás újra a felhasználói felület vagy a PowerShell. Lásd: "lépés: 2: engedélyezése a számítógépen a címtár-szinkronizálás jelszó visszaírást &amp; tűzfal szabályok konfigurálása" <a href="active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords">engedélyezhetik/letilthatják jelszó visszaírást hogyan</a> ezzel kapcsolatban további információt.</p>
              <p>

              </p>
              <p>Ha ez nem működik, próbálja meg a teljes eltávolítással és újratelepítése Azure AD Connect.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Engedélyek hibák</p>
            </td>
            <td>
              <p>Összevont vagy jelszó-szinkronizálás kivonat ellátott próbálják alaphelyzetbe a jelszó lásd: hiba jelző szolgáltatás probléma merült fel a jelszó elküldése után.</p>
              <p>

              </p>
              <p>Mindezeken kívül jelszó alaphelyzetbe állítása műveletek során látható management ügynök kapcsolatos hiba a helyszíni környezetbe a eseménynaplók hozzáférés megtagadva.</p>
            </td>
            <td>
              <p>Ha ezeket a hibákat a eseménynaplójában látható, győződjön meg arról, hogy az Active Directory MA fiókoknak (volt a varázslóban megadott konfigurációs idején) visszaírást jelszó szükséges engedélyekkel rendelkezik-e.</p>
              <p>

              </p>
              <p>FIGYELJE meg, hogy miután ezzel az engedéllyel számítható ki is tovább tarthat az engedélyek lefelé az Adatközpont sdprop háttér tevékenység keresztül trickle legfeljebb 1 óra. </p>
              <p>A jelszó alaphelyzetbe állítása a munkát a engedélyt kell kell jelölni a user objektumban, amelynek a jelszó alaphelyzetbe áll: a biztonsági leíró. Ezzel az engedéllyel a felhasználói objektumon jelenik meg, amíg a jelszó-visszaállítás folytatja a hozzáférés megtagadva, melyről.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Az Azure AD Connect konfigurálása varázsló-jelszó visszaírást megadásakor hiba </p>
            </td>
            <td>
              <p>"Nem lehet keresse meg a MA" hibába varázsló jelszó visszaírást engedélyezése vagy letiltása</p>
            </td>
            <td>
              <p>Azure AD Connect, amely a következő esetben jegyzékfájlok: a végleges verzióját egy ismert hibát tartalmaz:</p>
              <ol class="ordered">
                <li>
A bérlői abc.com (tartomány ellenőrizve) használatával creds Azure AD Connect konfigurálása Ez a név "abc.com – AAD" AAD összekötő eredményez eredményezne.<br\><br\></li>
                <li>
Majd módosítsa a AAD creds az összekötő (régi szinkronizálási felhasználói felület használatával) (jegyezze fel ugyanahhoz a bérlőhöz, de másik tartománynevet). <br\><br\></li>
                <li>
Most már próbálja engedélyezhetik/letilthatják jelszó visszaírást. A varázsló Egyenletszerkesztővel a nevét, az összekötő használata a creds, mint "abc.onmicrosoft.com – AAD" és a jelszó visszaírást parancsmag át. A sikertelen lesz, mert nincs ilyen nevű létrehozott összekötő.<br\><br\></li>
              </ol>
              <p>Ez a legújabb buildjeiben megoldását. Ha egy régebbi fejlesztése, az egyik megoldás is a powershell-parancsmag használatával engedélyezhetik/letilthatják a szolgáltatás. Lásd: "lépés: 2: engedélyezése a számítógépen a címtár-szinkronizálás jelszó visszaírást &amp; tűzfal szabályok konfigurálása" <a href="active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords">engedélyezhetik/letilthatják jelszó visszaírást hogyan</a> ezzel kapcsolatban további információt.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Nem lehet például Domain Admins különleges csoportokban felhasználó jelszavának alaphelyzetbe állítása és vállalati rendszergazdák stb.</p>
            </td>
            <td>
              <p>Összevont vagy jelszó-szinkronizálás kivonat ellátott felhasználók, akik védett csoportok részeként, és próbálja meg alaphelyzetbe a jelszó lásd: hiba jelző szolgáltatás probléma merült fel a jelszó elküldése után.</p>
            </td>
            <td>
              <p>Az Active Directory jogosultsággal rendelkező felhasználók védett AdminSDHolder használatával. Lásd: <a href="https://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx">http://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx</a> további információt. </p>
              <p>

              </p>
              <p>Ez azt jelenti, a biztonsági leíró ezeknek az objektumoknak a rendszeres beadott megfelelően az egyik megadott AdminSDHolder, és ha másik alaphelyzetbe állítása. További engedélyek szükségesek az jelszó visszaírást tehát nem trickle ilyen a felhasználók számára. Ezek a felhasználók nem működik a jelszó visszaírást eredményezi. Következtében nem támogatjuk a jelszavak kezelése ezekhez a csoportokhoz lévő felhasználók, mert az Active Directory biztonsági modell megszakítja a.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Nem található alaphelyzetbe művelet sikertelen objektummal</p>
            </td>
            <td>
              <p>Összevont vagy jelszó-szinkronizálás kivonat ellátott próbálják alaphelyzetbe a jelszó lásd: hiba jelző szolgáltatás probléma merült fel a jelszó elküldése után.</p>
              <p>

              </p>
              <p>Mindezeken kívül jelszó alaphelyzetbe állítása műveletek során előfordulhat, hogy hibaüzenet jelenik meg az eseménynaplók az Azure AD Connect szolgáltatásból jelző "Objektum nem található" hiba történt.</p>
            </td>
            <td>
              <p>Ez a hiba általában azt jelzi, hogy a szinkronizálási motor nem AD összekötő terület objektum vagy a AAD összekötő szóköz vagy a csatolt té Elemet a user objektumban található. </p>
              <p>

              </p>
              <p>Elhárításához győződjön meg arról, hogy a felhasználó valóban szinkronizálva van a helyszíni AAD keresztül Azure AD Connect jelenlegi példányából, és nézze meg az objektumok összekötő szóközt és té Elemet állapotát. Erősítse meg, hogy az Active Directory CS objektum összekötőre, az té Elemet objektumra a "Microsoft.InfromADUserAccountEnabled.xxx" szabály keresztül.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Műveletek megszakad, és több találat található eror alaphelyzetbe állítása</p>
            </td>
            <td>
              <p>Összevont vagy jelszó-szinkronizálás kivonat ellátott próbálják alaphelyzetbe a jelszó lásd: hiba jelző szolgáltatás probléma merült fel a jelszó elküldése után.</p>
              <p>

              </p>
              <p>Mindezeken kívül során a jelszó alaphelyzetbe állítása műveletek, előfordulhat, hogy hibaüzenet jelenik meg a "Több maches található" hibát jelző Azure AD Connect szolgáltatás az eseménynaplók.</p>
            </td>
            <td>
              <p>Ez azt jelzi, hogy a szinkronizálási motor észlelt, hogy az té Elemet objektum-e több kapcsolatot AD CS objektumok "Microsoft.InfromADUserAccountEnabled.xxx" keresztül. Ez azt jelenti, hogy a felhasználó egynél több erdő engedélyezett fiókkal rendelkezik. </p>
              <p>

              </p>
              <p>Ebben az esetben a jelszó visszaírást jelenleg nem támogatott.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Konfigurációs hiba miatt sikertelen jelszó műveletek.</p>
            </td>
            <td>
              <p>Konfigurációs hiba miatt sikertelen jelszó műveletek. Az alkalmazás eseménynaplójába Azure AD Connect hiba 6329 szöveggel tartalmazza: 0x8023061f (a művelet nem sikerült, mert ez Management Agent nincs engedélyezve a jelszó-szinkronizálás.)</p>
            </td>
            <td>
              <p>Ez akkor fordul elő, az Azure AD Connect konfiguráció hozzáadása megváltozásakor&nbsp;egy új Active Directory-erdőt (vagy az eltávolítás, majd vegye fel újból a meglévő erdőben) <strong>után</strong> a jelszó visszaírást szolgáltatás már engedélyezve van. Jelszó műveletek, felhasználók ilyen újonnan hozzáadott erdőkben sikertelen lesz. A probléma megoldásához tiltsa le, és újra a jelszót visszaírást szolgáltatás engedélyezése után a erdő konfigurációs módosítások elvégzése után.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>A felhasználó által módosított írása vissza lett visszaállítva felhasználók Works megfelelően jelszavakat, de vissza jelszavak írása, vagy alaphelyzetbe egy felhasználó rendszergazda hiba jelenik meg.</p>
            </td>
            <td>
              <p>Amikor megpróbál a jelszó alaphelyzetbe állítása az Azure adatkezelési portálról felhasználó nevében, megjelenik egy üzenet arról: "a jelszó alaphelyzetbe állítása szolgáltatás fut a helyszíni környezet nem támogatja a rendszergazdák felhasználói jelszó alaphelyzetbe állítása. Frissítsen az Azure AD Connect megoldása legújabb verzióját."</p>
            </td>
            <td>
              <p>Ez akkor fordul elő, ha a szinkronizálás motor verziója nem támogatja az adott jelszó visszaírást művelet, amelyet használt. Azure AD Connect 1.0.0419.0911 támogatja az összes jelszó felügyeleti műveleteket, mint újabb verzióiban, például a jelszó alaphelyzetbe állítása, visszaírást, a jelszó módosítása visszaírást és a rendszergazda által kezdeményezett jelszó alaphelyzetbe állítása visszaírást az Azure adatkezelési portálról.&nbsp; DirSync verziókkal később mint 1.0.6862 támogatja a jelszó alaphelyzetbe állítása csak visszaírást. Ez a probléma megoldása érdekében azt javasoljuk, hogy telepítse a legújabb Azure AD Connect és Azure Active Directory csatlakoztatása (további tudnivalókért lásd: <a href="active-directory-aadconnect">Címtár-integrációs eszközök</a>) Ez a probléma megoldásához, és a szervezet jelszó visszaírást hatékony kihasználása.</p>
            </td>
          </tr>
        </tbody></table>


## <a name="password-writeback-event-log-error-codes"></a>Jelszó visszaírást eseménynaplójának hibakódok esetén
A legjobb ha jelszó visszaírást tartalmazó kimutatások problémáinak vizsgálata, hogy az Azure AD Connect gépi alkalmazás eseménynaplójában talál. Az Eseménynapló jelszó visszaírást az érdeklődésre számot tartó két forrásból származó események tartalmazni fogja. A PasswordResetService forrás leírja, műveletek és a jelszó visszaírást működésével kapcsolatos problémákat. A ADSync forrás leírja, műveletek és jelszavak beállítása az Active Directory környezetben kapcsolatos problémákat.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Kód</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Név / üzenet</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Forrás</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Leírás</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>6329</p>
            </td>
            <td>
              <p>Nyújtott ÓVADÉKOT: MMS(4924) 0x80230619 – "korlátozása megakadályozza, hogy a jelszó nem módosíthatók az aktuális egy megadott."</p>
            </td>
            <td>
              <p>ADSync</p>
            </td>
            <td>
              <p>A jelszó visszaírást szolgáltatás jelszó beállítása, kattintson a helyi Directoryban, amely nem felel meg a jelszó kora, előzmények, összetettsége vagy tartomány szűrési követelmények alkalommal, amikor az esemény megtörténik.</p>
              <ul>
                <li class="unordered">
Ha a jelszó minimális életkor, és nemrég megváltozott adott idő ablakon belül a jelszót, nem tudja módosítani a jelszót újra a tartományban megadott kora eléréséig. Tesztelési célú, minimális életkor pedig 0 meg kell.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Ha van engedélyezve előzmények jelszókövetelmények, majd jelöljön ki egy jelszót az utolsó N időpontok, a nem használt Ha N a jelszó előzmények beállítást. Ha bejelöli az utolsó N beállítások a használt jelszót, majd ekkor megjelenik egy hiba ebben az esetben. Tesztelési célú, 0 előzmények kell beállítani.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Ha összetettsége jelszókövetelmények, mindegyiket szabályon alkalommal, amikor a felhasználó módosítása vagy a jelszó alaphelyzetbe állítása.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Ha a jelszó szűrők engedélyezve van, és egy felhasználó kijelöli a jelszót, amely nem felel meg a szűrési feltételek, majd a alaphelyzetbe állítása vagy módosítása a művelet sikertelen lesz.<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>HR 8023042</p>
            </td>
            <td>
              <p>Szinkronizálás motor visszaadott érték: egy hiba hr = 80230402, üzenetet nem sikerült, mert az azonos horgony az ismétlődő bejegyzések objektum megpróbálja =</p>
            </td>
            <td>
              <p>ADSync</p>
            </td>
            <td>
              <p>Ez az esemény fordul elő, ha az azonos felhasználóazonosító engedélyezve van-e több tartományt.  Például ha fiók/erőforrás erdők vannak szinkronizál, és ugyanazzal a felhasználói azonosítóval jelenlegi és minden egyes engedélyezett, ez a hiba akkor fordulhat elő.  </p>
              <p>Ez a hiba akkor is fordulhat elő, ha egy nem egyedi horgony attribútum (például alias vagy UPN) használ, és két felhasználó oszt meg, hogy ugyanazon horgony attribútum.</p>
              <p>A probléma megoldásához, győződjön meg arról, hogy nem rendelkezik-e be a felhasználók ismétlődő belül a tartományok és használt egy egyedi horgony attribútumot minden felhasználó számára.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31001</p>
            </td>
            <td>
              <p>PasswordResetStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Az esemény, az azt jelenti, hogy a helyszíni szolgáltatás észleli a jelszó alaphelyzetbe állítása egy összevont kérése vagy kivonat jelszó-szinkronizálás a felhőbe származó felhasználói ellátott. Ez az esemény, az első esemény minden a jelszó alaphelyzetbe állítása visszaírást művelet.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31002</p>
            </td>
            <td>
              <p>PasswordResetSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ebben az esetben azt jelzi, hogy a felhasználó kijelölt egy új jelszót a jelszó alaphelyzetbe állítása művelet során, azt határozza meg, hogy megfelel vállalati jelszót a jelszó és, hogy a jelszó írása sikeresen befejeződött vissza a helyi Active Directory-környezetet.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31003</p>
            </td>
            <td>
              <p>PasswordResetFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ebben az esetben azt jelzi, hogy a felhasználó kijelölt egy jelszót, hogy a jelszó sikeresen megérkezett a helyszíni környezetbe, de a kísérlet történt adja meg a jelszót a helyi Active Directory-környezetben, amikor hiba történt. Ez akkor fordulhat elő, több ok miatt:</p>
              <ul>
                <li class="unordered">
A felhasználók jelszava nem felel meg az életkor, előzmények, összetettsége, vagy a tartomány követelményei szűrése. Teljesen új jelszót megoldásához próbálkozzon.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
A MA szolgáltatás fiókja nem rendelkezik a szükséges engedélyeket az új jelszót a szóban forgó felhasználói fiók.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
A felhasználói fiók szerepel a védett csoport, például a tartomány vagy a vállalati rendszergazdák, amely letiltja a jelszó megadása műveletek.<br\><br\></li>
              </ul>
              <p>Lásd: a <a href="#troubleshoot-password-writeback">Jelszó visszaírást kapcsolatos hibák elhárítása</a> megtudhatja, hogy több milyen más situtions kapcsolatos okozhatják a hibát.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31004</p>
            </td>
            <td>
              <p>OnboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Az esemény akkor fordul elő, ha engedélyezi az Azure AD Connect jelszó visszaírást, és láthatja a bevezetési szeretné, hogy a jelszó visszaírást webszolgáltatás azt lépések.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31005</p>
            </td>
            <td>
              <p>OnboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ebben az esetben azt jelzi, a bevezetési folyamat sikeres volt, és jelszó visszaírást képesség használatára.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31006</p>
            </td>
            <td>
              <p>ChangePasswordStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ebben az esetben azt jelzi, hogy a helyszíni szolgáltatás talált a jelszó módosítása kér a szövetséges vagy kivonat jelszó-szinkronizálás a felhőbe származó felhasználói ellátott. Ebben az esetben az első esemény minden jelszó módosítása visszaírást művelet.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31007</p>
            </td>
            <td>
              <p>ChangePasswordSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ebben az esetben azt jelzi, hogy a felhasználó kijelölt egy új jelszót a jelszó módosítása közben, azt határozza meg, hogy megfelel vállalati jelszót a jelszó és, hogy a jelszó írása sikeresen befejeződött vissza a helyi Active Directory-környezetet.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31008</p>
            </td>
            <td>
              <p>ChangePasswordFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ebben az esetben azt jelzi, hogy a felhasználó kijelölt egy jelszót, hogy a jelszó sikeresen megérkezett a helyszíni környezetbe, de a kísérlet történt adja meg a jelszót a helyi Active Directory-környezetben, amikor hiba történt. Ez akkor fordulhat elő, több ok miatt:</p>
              <ul>
                <li class="unordered">
A felhasználók jelszava nem felel meg az életkor, előzmények, összetettsége, vagy a tartomány követelményei szűrése. Teljesen új jelszót megoldásához próbálkozzon.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
A MA szolgáltatás fiókja nem rendelkezik a szükséges engedélyeket az új jelszót a szóban forgó felhasználói fiók.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
A felhasználói fiók szerepel a védett csoport, például a tartomány vagy a vállalati rendszergazdák, amely letiltja a jelszó megadása műveletek.<br\><br\></li>
              </ul>
              <p>Lásd: a <a href="#troubleshoot-password-writeback">Jelszó visszaírást kapcsolatos hibák elhárítása</a> megtudhatja, hogy több milyen más helyzetekben kapcsolatos okozhatja ezt a hibát.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31009</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>A helyszíni szolgáltatás észlelt a jelszó alaphelyzetbe állítása egy összevont kérése vagy jelszó-szinkronizálás kivonat ellátott felhasználói a rendszergazda származó felhasználó nevében. Ebben az esetben az első minden rendszergazda által kezdeményezett jelszó alaphelyzetbe állítása visszaírást művelet esemény.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31010</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>A rendszergazda új jelszót kijelölt rendszergazda által kezdeményezett jelszó alaphelyzetbe állítása művelet közben, és azt határozza meg, hogy megfelel vállalati jelszót a jelszó és, hogy a jelszó írása sikeresen befejeződött vissza a helyi Active Directory-környezetet.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31011</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>A rendszergazdai jelszó kijelölt nevében egy felhasználó, és, hogy a jelszó sikeresen megérkezett a helyszíni környezetbe, de amikor kísérlet történt adja meg a jelszót a helyi Active Directory-környezetben, hiba történt. Ez akkor fordulhat elő, több ok miatt:</p>
              <ul>
                <li class="unordered">
A felhasználók jelszava nem felel meg az életkor, előzmények, összetettsége, vagy a tartomány követelményei szűrése. Teljesen új jelszót megoldásához próbálkozzon.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
A MA szolgáltatás fiókja nem rendelkezik a szükséges engedélyeket az új jelszót a szóban forgó felhasználói fiók.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
A felhasználói fiók szerepel a védett csoport, például a tartomány vagy a vállalati rendszergazdák, amely letiltja a jelszó megadása műveletek.<br\><br\></li>
              </ul>
              <p>Lásd: a <a href="#troubleshoot-password-writeback">Jelszó visszaírást kapcsolatos hibák elhárítása</a> megtudhatja, hogy több milyen más situtions kapcsolatos okozhatják a hibát.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31012</p>
            </td>
            <td>
              <p>OffboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ez az esemény akkor fordul elő, ha letiltja az Azure AD Connect jelszó visszaírást, és azt jelzi, offboarding szeretné, hogy a jelszó visszaírást webszolgáltatás azt lépések.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31013</p>
            </td>
            <td>
              <p>OffboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ebben az esetben azt jelzi, hogy a program sikeres letiltotta jelszó visszaírást lehetőséget, és a offboarding folyamat sikeres volt.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31014</p>
            </td>
            <td>
              <p>OffboardingEventFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ebben az esetben azt jelzi, hogy a offboarding folyamat nem sikerült. Ez lehet az a felhő vagy a helyszíni rendszergazdafiók során a konfigurációban megadott engedélyekkel kapcsolatos hiba miatt, vagy mert szövetséges felhő globális rendszergazdának használja, ha a jelszó visszaírást letiltása kísérel meg. A hiba kijavításához ellenőrizze a rendszergazdai engedélyek és a nem használt bármelyik összevont fiók beállításakor a jelszó visszaírást lehetőséget.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31015</p>
            </td>
            <td>
              <p>WriteBackServiceStarted </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ebben az esetben azt jelzi, hogy a jelszó visszaírást szolgáltatás sikeresen elindult készen áll a felhőből jelszó-kezelés összehívások.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31016</p>
            </td>
            <td>
              <p>WriteBackServiceStopped </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ebben az esetben azt jelzi, hogy a jelszó visszaírást szolgáltatás leállt, hogy minden olyan jelszót management kérelmek a felhőből nem lesz a sikeres.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31017</p>
            </td>
            <td>
              <p>AuthTokenSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ez az esemény beolvasni, hogy azt sikeresen egy jóváhagyási jogkivonat, a globális rendszergazda annak érdekében, hogy a offboarding vagy bevezetési megkezdéséhez Azure AD Connect a telepítés során megadott.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31018</p>
            </td>
            <td>
              <p>KeyPairCreationSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ebben az esetben azt jelzi, hogy sikeresen létrehozott használt jelszavak a helyszíni környezet kapni a felhőből titkosítása jelszó-titkosítási kulcs.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32000</p>
            </td>
            <td>
              <p>UnknownError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ebben az esetben azt jelzi, ismeretlen hiba jelszó management művelet során. Nézze meg az esemény további információt a kivétel szöveget. Ha problémába ütközik, letiltása és ismételt engedélyezése a jelszó visszaírást próbálja meg. Ha ez nem segít, belefoglalásával az Eseménynapló a nyomon követés azonosítója, a megadott együtt szeretné a támogatási adatbázismodellbe bennfentes.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32001</p>
            </td>
            <td>
              <p>ServiceError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ebben az esetben azt jelzi, hogy hibát csatlakoztatása az a felhő jelszó alaphelyzetbe állítása szolgáltatás volt, és általában fordul elő, ha a helyszíni szolgáltatás nem tud csatlakozni a jelszó alaphelyzetbe állítása webszolgáltatás. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32002</p>
            </td>
            <td>
              <p>ServiceBusError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ebben az esetben azt jelzi, hiba történt a bérlő szolgáltatás bus példány csatlakozik. Ennek oka a helyszíni környezetben blokkolja a kimenő kapcsolatokat. Jelölje be a tűzfalat, annak biztosítására, engedélyezze a kapcsolatokat, TCP 443-as fölé, és <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>, és próbálkozzon újra. Ha a rendszer továbbra is problémákat tapasztal, próbálkozzon a letiltása és ismételt engedélyezése jelszó visszaírást.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32003</p>
            </td>
            <td>
              <p>InPutValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Az esemény, az azt jelenti, hogy a saját webhely szolgáltatás API átadott bemeneti érvénytelen. Ismételje meg a műveletet.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32004</p>
            </td>
            <td>
              <p>DecryptionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Az esemény, az azt jelenti, hogy a jelszót, amelyet a felhőből érkezett visszafejtése hiba történt. Ez lehet a felhőbeli szolgáltatástól és a helyszíni környezet közötti eltérés visszafejtés kulcs miatt. Annak érdekében, hogy a probléma megoldásához letiltása és ismételt engedélyezése a jelszó visszaírást a helyszíni környezetben.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32005</p>
            </td>
            <td>
              <p>ConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Során bevezetési a helyszíni környezet konfigurációja fájl bérlői adatait mentse azt. Ebben az esetben azt jelzi, hiba történt a fájl mentése vagy, hogy ha a szolgáltatás nem indult el lett hiba olvasása a fájlt. Ez a probléma megoldásához próbálkozzon letiltása és ismételt engedélyezése jelszó visszaírást kényszerítése konfigurációs fájl újból írni. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32006</p>
            </td>
            <td>
              <p>EndPointConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>ELAVULT – Ez az esemény nem szerepel a Azure AD Connect, csak nagyon korai kapható DirSync visszaírást támogatott.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32007</p>
            </td>
            <td>
              <p>OnBoardingConfigUpdateError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Bevezetési, során elküldjük Önnek a helyszíni jelszót a felhőből adatok szolgáltatás alaphelyzetbe állítása. Az adatokat a memóriában fájlba majd írt a biztonságosan tárolásához ezt az információt a lemezen a szinkronizálási szolgáltatás elküldése előtt. Ez az esemény írni, és a memóriában adatok frissítése a hibát jelez. Ez a probléma megoldásához próbálkozzon letiltása és ismételt engedélyezése jelszó visszaírást lehet váltani az ebben a konfigurációban újból írni.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32008</p>
            </td>
            <td>
              <p>ValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ebben az esetben azt jelzi, hogy a jelszó alaphelyzetbe állítása webszolgáltatás érvénytelen választ kapott azt. Ez a probléma megoldásához próbálkozzon letiltása és ismételt engedélyezése jelszó visszaírást.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32009</p>
            </td>
            <td>
              <p>AuthTokenError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Az esemény, az azt jelenti, hogy azt nem sikerült az engedély jogkivonat a globális rendszergazdai fiók megadott Azure AD Connect a telepítés során. Ez a hiba oka lehet rossz felhasználónevet és jelszót adott-e a globális rendszergazdai fiókkal, vagy mert adott a globális rendszergazdai fiókkal összevont. Ez a probléma megoldásához futtassa újra a helyes a felhasználónév és jelszó beállítását, és győződjön meg arról a rendszergazda egy felügyelt (felhőben vagy jelszó-szinkronizálás volna) fiókot.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32010</p>
            </td>
            <td>
              <p>CryptoError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ebben az esetben azt jelzi, hiba történt a jelszó létrehozásakor titkosítási kulcs vagy egy jelszót, amely a felhőbeli szolgáltatástól megérkezik visszafejtése. Ez a hiba valószínűleg a környezet hibát jelez. Nézze meg az Eseménynapló további és a probléma megoldásához részleteit. Előfordulhat, hogy is próbálkozhat letiltása és ismételt engedélyezése a jelszó visszaírást szolgáltatás megoldásához.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32011</p>
            </td>
            <td>
              <p>OnBoardingServiceError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Az esemény, az azt jelenti, hogy a helyszíni szolgáltatás nem tudott megfelelően kommunikálni a bevezetési megkezdéséhez a jelszó alaphelyzetbe állítása webszolgáltatás. Ez a szabály vagy egy auth jogkivonat beszerzése a bérlő probléma miatt lehet. A hiba kijavításához győződjön meg arról, hogy nem, amelyek gátolják kimenő kapcsolatok, TCP 443-as és TCP 9350-9354 vagy <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>, és nem szövetséges az a szolgáltatást használ beépített AAD rendszergazdai fiókjával. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32012</p>
            </td>
            <td>
              <p>OnBoardingServiceDisableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>ELAVULT – Ez az esemény nem szerepel a Azure AD Connect, csak nagyon korai kapható DirSync visszaírást támogatott.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32013</p>
            </td>
            <td>
              <p>OffBoardingError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Az esemény, az azt jelenti, hogy a helyszíni szolgáltatás nem tudott megfelelően kommunikálni a offboarding megkezdéséhez a jelszó alaphelyzetbe állítása webszolgáltatás. Ez a szabály vagy egy jóváhagyási jogkivonat beszerzése a bérlő probléma miatt lehet. A hiba kijavításához győződjön meg arról, hogy nem, amelyek gátolják kimenő kapcsolatok, vagy <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>443-as fölé, és nem, hogy a AAD rendszergazdai fiók segítségével offboard identitásszolgáltatóval összevont. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32014</p>
            </td>
            <td>
              <p>ServiceBusWarning </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Az esemény, az azt jelenti, hogy azt kellett ismételje meg a bérlői szolgáltatás bus példányához. Normál feltételek mellett ez célszerű nem érinti, de ha látható az esemény sokszor, fontolja meg a szolgáltatás bus, hogy a hálózati kapcsolat ellenőrzése különösen akkor, ha egy nagy késést, vagy ha kis sávszélességű kapcsolat.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32015</p>
            </td>
            <td>
              <p>ReportServiceHealthError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>A jelszó visszaírást szolgáltatás állapotának ellenőrzéséhez elküldjük szívveréséhez az adatokat a jelszó alaphelyzetbe állítása webszolgáltatás 5 percenként. Ebben az esetben azt jelzi, hogy egy hiba a rendszerállapot adatait vissza az a felhő webszolgáltatás küldésekor. A rendszerállapot adatait nem tartalmaz egy OII vagy adat adatokat, és pusztán egy szívveréséhez és egyszerű szolgáltatás statisztikai adatokat, hogy kínálunk, amely a szolgáltatásállapot adatainak a felhőben.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33001</p>
            </td>
            <td>
              <p>ADUnKnownError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ebben az esetben azt jelzi, hogy ismeretlen hibát AD vissza, ez a hiba kapcsolatos további tudnivalók a ADSync forrásból származó eseményekre vonatkozóan: Azure AD Connect kiszolgáló eseménynaplójában talál.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33002</p>
            </td>
            <td>
              <p>ADUserNotFoundError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Az esemény, az azt jelenti, hogy a felhasználó, aki próbál alaphelyzetbe állítása vagy módosítása a jelszó nem található a helyszíni címtárban. Ez akkor fordulhat elő, amikor a felhasználó már törölt a helyszínen, de nem a felhőben, ha szinkronizálási problémát. Jelölje be a szinkronizálás naplók, valamint az utolsó néhány szinkronizálás futtatása további információt a részletek.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33003</p>
            </td>
            <td>
              <p>ADMutliMatchError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Jelszó alaphelyzetbe állítása vagy módosítása kérelem a felhőben származik, ha a felhőben horgony a telepítés során az Azure AD Connect megadott használjuk annak összekapcsolása kérelem egy felhasználó a helyszíni környezetben. Az esemény, az azt jelenti, hogy a helyszíni címtárában azonos felhő horgony attribútumot tartalmazó két felhasználó talált. Jelölje be a szinkronizálás naplók, valamint az utolsó néhány szinkronizálás futtatása további információt a részletek.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33004</p>
            </td>
            <td>
              <p>ADPermissionsError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Az esemény, az azt jelenti, hogy a kezelés ügynök szolgáltatás fiókja nem rendelkezik a megfelelő engedélyeket az új jelszót állíthat be a szóban forgó ügyfél. Gondoskodjon arról, hogy a felhasználó erdőben a MA fiók alaphelyzetbe állítása, és új jelszót engedélyek erdőben összes objektumokon.  További információt olvashat végezze el a, lásd: <a href="active-directory-passwords-getting-started.md#step-4-set-up-the-appropriate-active-directory-permissions">lépés: 4: állítsa be a megfelelő engedélyekkel az Active Directory</a>.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33005</p>
            </td>
            <td>
              <p>ADUserAccountDisabled </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Az esemény, az azt jelenti, hogy azt megpróbált alaphelyzetbe állítása vagy egy helyszíni letiltott fiók jelszavának módosítása. Engedélyezze a fiókot, majd próbálkozzon újra.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33006</p>
            </td>
            <td>
              <p>ADUserAccountLockedOut </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Esemény azt jelzi, hogy azt kísérelte meg vissza szeretne állítani vagy módosítani szeretné, hogy a helyszíni lett zárolva fiók jelszavát. Zárolások akkor fordulhat elő, amikor a felhasználó rendelkezik módosítás próbálkozott, vagy állítsa alaphelyzetbe a jelszó művelet túl sokszor rövid idő. A zárolás feloldásához, és próbálkozzon újra.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33007</p>
            </td>
            <td>
              <p>ADUserIncorrectPassword </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Az esemény, az azt jelenti, hogy a felhasználó által megadott helytelen aktuális jelszót, amikor a művelet végrehajtása a jelszó módosítása. Adja meg a megfelelő aktuális jelszót, és próbálkozzon újra.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33008</p>
            </td>
            <td>
              <p>ADPasswordPolicyError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>A jelszó visszaírást szolgáltatás jelszó beállítása, kattintson a helyi Directoryban, amely nem felel meg a jelszó kora, előzmények, összetettsége vagy tartomány szűrési követelmények alkalommal, amikor az esemény megtörténik.</p>
              <ul>
                <li class="unordered">
Ha a jelszó minimális életkor, és nemrég megváltozott adott idő ablakon belül a jelszót, nem tudja módosítani a jelszót újra a tartományban megadott kora eléréséig. Tesztelési célú, minimális életkor pedig 0 meg kell.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Ha van engedélyezve előzmények jelszókövetelmények, majd jelöljön ki egy jelszót az utolsó N időpontok, a nem használt Ha N a jelszó előzmények beállítást. Ha bejelöli az utolsó N beállítások a használt jelszót, majd ekkor megjelenik egy hiba ebben az esetben. Tesztelési célú, 0 előzmények kell beállítani.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Ha összetettsége jelszókövetelmények, mindegyiket szabályon alkalommal, amikor a felhasználó módosítása vagy a jelszó alaphelyzetbe állítása.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Ha a jelszó szűrők engedélyezve van, és egy felhasználó kijelöli a jelszót, amely nem felel meg a szűrési feltételek, majd a alaphelyzetbe állítása vagy módosítása a művelet sikertelen lesz.<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>33009</p>
            </td>
            <td>
              <p>ADConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ebben az esetben azt jelzi, egy helyszíni címtárában egy konfigurációs probléma miatt az Active Directoryval, hogy biztonsági jelszó probléma írni. Jelölje be az Azure AD Connect gépi alkalmazás eseménynaplójában talál további információt a Mi a hiba történt a ADSync szolgáltatás érkező üzeneteket. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34001</p>
            </td>
            <td>
              <p>ADPasswordPolicyOrPermissionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>ELAVULT – Ez az esemény nem szerepel a Azure AD Connect, csak nagyon korai kapható DirSync visszaírást támogatott.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34002</p>
            </td>
            <td>
              <p>ADNotReachableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>ELAVULT – Ez az esemény nem szerepel a Azure AD Connect, csak nagyon korai kapható DirSync visszaírást támogatott.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34003</p>
            </td>
            <td>
              <p>ADInvalidAnchorError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>ELAVULT – Ez az esemény nem szerepel a Azure AD Connect, csak nagyon korai kapható DirSync visszaírást támogatott.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-writeback-connectivity"></a>Jelszó visszaírást kapcsolat hibaelhárítása

Szolgáltatás kiküszöbölése, hogy a jelszó visszaírást összetevő az Azure AD Connect tapasztal, ha az alábbiakban néhány megoldásához készíthet a rövid lépéseket:

 - [Indítsa újra az Azure Active Directory szinkronizáló szolgáltatás csatlakoztatása](#restart-the-azure-AD-Connect-sync-service)
 - [Tiltsa le, és újra a jelszót visszaírást szolgáltatás engedélyezése](#disable-and-re-enable-the-password-writeback-feature)
 - [Telepítse a legújabb Azure AD Connect verzióból](#install-the-latest-azure-ad-connect-release)
 - [Jelszó visszaírást – problémamegoldás](#troubleshoot-password-writeback)

Általánosságban elmondható azt javasoljuk, hogy a fenti sorrendben annak érdekében, hogy a szolgáltatás helyreállítása a leggyorsabb módon hajtsa végre ezeket a lépéseket.

### <a name="restart-the-azure-ad-connect-sync-service"></a>Indítsa újra az Azure Active Directory szinkronizáló szolgáltatás csatlakoztatása
Indítsa újra az Azure Active Directory csatlakozás szinkronizálási szolgáltatás segíthet csatlakozással vagy más a szolgáltatással átmeneti problémák megoldásához.

 1. Rendszergazdaként kattintson a **Start** menü **Azure AD Connect**rendszert futtató kiszolgáló.
 2. Írja be a **"services.msc"** a Keresés mezőbe, és nyomja le az **ENTER billentyűt**.
 3. Keresse meg a **Microsoft Azure AD Connect** tételt.
 4. Kattintson a jobb gombbal a szolgáltatás tétel, az **Újraindítás**gombra, és várja meg a művelet elvégzéséhez.

    ![][002]

Ezeket a lépéseket az a felhő szolgáltatással kapcsolatot létesíteni, és a semmiféle MEGSZAKADÁSÁÉRT tapasztalhat megoldásához.  A szinkronizálási szolgáltatás újraindítás nem oldja meg a problémát, azt javasoljuk, hogy próbálja letiltása és ismételt engedélyezése a jelszó visszaírást funkció a következő lépésben.

### <a name="disable-and-re-enable-the-password-writeback-feature"></a>Tiltsa le, és újra a jelszót visszaírást szolgáltatás engedélyezése
Letiltása és ismételt engedélyezése a jelszó visszaírást szolgáltatás segíthet kapcsolódási problémák megoldásához.

 1. Rendszergazdaként nyissa meg az **Azure AD Connect konfigurálása varázsló**.
 2. Az **Azure ad Connect** párbeszédpanelen adja meg az **Azure Active Directory globális rendszergazdai hitelesítő** adatait.
 3. A **Csatlakozás az Active Directory tartományi szolgáltatások** párbeszédpanelen adja meg az **Active Directory tartományi szolgáltatások rendszergazdai hitelesítő adataival**.
 4. **A felhasználók egyedileg azonosító** párbeszédpanelen kattintson a **Tovább** gombra.
 5. A **választható funkció** párbeszédpanelen törölje a jelet a **jelszó írási-vissza** jelölőnégyzetet.

    ![][003]

 6. Kattintson a **Tovább gombra** a párbeszédpanel fennmaradó oldalakra figyelmen kívül hagyásával bármi amíg el nem **kész konfigurálása** lapon.
 7. Biztosítani, hogy a beállítás lapon látható-e a **Letiltva jelszó írási-vissza lehetőséget** , és kattintson a a zöld **beállítás** gombra a módosítások véglegesítése gombra.
 8. **Befejezett** párbeszédpanelen törölje a **Szinkronizálás most** lehetőséget, és kattintson a **Befejezés gombra** kattintva zárja be a varázslót.
 9. Nyissa meg újra az **Azure AD Connect konfigurálása varázsló**.
 10.    **Ismételje meg a lépéseket a 2-8**, azzal a különbséggel győződjön meg róla, **Jelölje be a jelszót írási-vissza lehetőséget** a **választható funkció** használata esetén újra be kell kapcsolnia a szolgáltatást.

    ![][004]

Ezeket a lépéseket az a felhő szolgáltatás a kapcsolatot létesíteni, és a semmiféle MEGSZAKADÁSÁÉRT tapasztalhat megoldásához.

Ha a letiltása és ismételt engedélyezése a jelszó visszaírást funkció nem oldja meg a problémát, azt javasoljuk, próbálja meg újra kell telepítenie az Azure AD Connect a következő lépésben.

### <a name="install-the-latest-azure-ad-connect-release"></a>Telepítse a legújabb Azure AD Connect verzióból
Újratelepítése az Azure AD Connect csomag feloldása konfigurációs problémák megoldásához, amely az Ön számára vagy hatással lehetnek a felhőbeli szolgáltatások vagy a helyi Active Directory-környezetben jelszavak kezelése csatlakozhat.
Azt javasoljuk, a lépés végrehajtása után csak az első két lépést a fent leírt kísérel meg.

 1. Töltse le a legújabb Azure AD Connect [Itt](active-directory-aadconnect.md#install-azure-ad-connect).
 2. Mivel már telepítette az Azure AD Connect, akkor csak kell a Azure AD Connect-telepítés frissítése a legújabb verzióra helyi frissítés végrehajtása.
 3. A letöltött csomagot, és kövesse a képernyőn megjelenő utasításokat követve frissítse az Azure AD Connect gépi.  Nincsenek további kézi lépésekre szükség, kivéve, ha a testre szabott az elévült mezőben szinkronizálási szabályokat, ebben az esetben célszerű a **kevésbé ezek előtt folytatása frissítése kézzel újra üzembe helyezése befejezése után**.

Ezeket a lépéseket az a felhő szolgáltatás a kapcsolatot létesíteni, és a semmiféle MEGSZAKADÁSÁÉRT tapasztalhat megoldásához.

Ha az Azure AD Connect legújabb verziójának telepítése nem oldja meg a problémát, azt javasoljuk, hogy próbálja letiltása és ismételt engedélyezése jelszó visszaírást utolsó lépésként a legújabb szinkronizálás QFE telepítése után.

Ha ez nem oldja meg a problémát, majd azt javasoljuk, hogy Ön egy pillantást tekintheti meg, ha nincs a problémát kell tárgyalt [Jelszó visszaírást kapcsolatos hibák elhárítása](#troubleshoot-password-writeback) és az [Azure Active Directory-jelszó kezelése – gyakori kérdések](active-directory-passwords-faq.md) .


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Jelszó mutató hivatkozások dokumentáció alaphelyzetbe állítása
Az alábbiakban minden Azure Active Directory-jelszó-visszaállítás dokumentáció mutató hivatkozások:

* **Itt azért, mert netán problémákat tapasztal bejelentkezéskor?** Ha igen, [az alábbiakban hogyan módosíthatja, és a saját jelszó alaphelyzetbe állítása](active-directory-passwords-update-your-own-password.md).
* , [**Hogy hogyan működik**](active-directory-passwords-how-it-works.md) - megismerheti a szolgáltatást, és mit hat különböző összetevői minden tartalmaz
* [**Első lépések**](active-directory-passwords-getting-started.md) – megtudhatja, hogy miként lehetővé teszi a felhasználóknak, hogy alaphelyzetbe állítása és a felhő vagy a helyszíni jelszavukat
* [**Testreszabása**](active-directory-passwords-customize.md) – a Megjelenés és működés és a szolgáltatást, hogy a szervezet igényei működésének testreszabása
* [**Ajánlott eljárások**](active-directory-passwords-best-practices.md) – útmutató telepítését gyorsan és hatékonyan a jelszavak a szervezet kezelése
* [**Háttérismeretek get**](active-directory-passwords-get-insights.md) - az integrált jelentéskészítési képességeinek ismertetése
* [**Gyakori kérdések**](active-directory-passwords-faq.md) – válaszok a gyakori kérdésekre
* [**Tudjon meg többet**](active-directory-passwords-learn-more.md) - lépjen a mély be a technikai részleteket az, hogy a szolgáltatás működése



[001]: ./media/active-directory-passwords-troubleshoot/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-troubleshoot/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-troubleshoot/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-troubleshoot/004.jpg "Image_004.jpg"

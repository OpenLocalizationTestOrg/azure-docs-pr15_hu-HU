<properties
    pageTitle="További információ: Azure AD-jelszó Management |} Microsoft Azure"
    description="Azure Active Directory-jelszó-jelszó visszaírást működését visszaírást biztonsági jelszó, hogy a jelszó alaphelyzetbe a portál és milyen adatokat használja, a jelszó-visszaállítás kezelése a haladó felhasználóknak."
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
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="learn-more-about-password-management"></a>További információ a jelszavak kezelését

> [AZURE.IMPORTANT] **Itt azért, mert netán problémákat tapasztal bejelentkezéskor?** Ha igen, [az alábbiakban hogyan módosíthatja, és a saját jelszó alaphelyzetbe állítása](active-directory-passwords-update-your-own-password.md).

Ha már telepítette a jelszavak kezelését, vagy egyszerűen csak vendégfelhasználókat szeretne többet megtudhat a technikai kövecses, mielőtt működéséről, ez a szakasz képet ad a technikai fogalmak mögött a szolgáltatást a áttekintése műszaki. Azt fogja a következőkre terjed ki:

* [**Jelszó visszaírást – áttekintés**](#password-writeback-overview)
  - [Hogyan működik a jelszavak visszaírást](#how-password-writeback-works)
  - [A jelszó visszaírást támogatott felhasználási területei](#scenarios-supported-for-password-writeback)
  - [Jelszó visszaírást biztonsági modell](#password-writeback-security-model)
* [**Hogyan állítsa alaphelyzetbe a jelszó portál munkát?**](#how-does-the-password-reset-portal-work)
  - [Milyen adatokat a jelszó-visszaállítás használja?](#what-data-is-used-by-password-reset)
  - [Elsajátíthatja a jelszó alaphelyzetbe állítása az adatokat a felhasználók számára](#how-to-access-password-reset-data-for-your-users)

## <a name="password-writeback-overview"></a>Jelszó visszaírást – áttekintés
Jelszó visszaírást összetevő az [Azure Active Directory csatlakozni](active-directory-aadconnect.md) , hogy engedélyezve van, és Azure Active Directory-támogatás az aktuális előfizetőinek által használt. További tudnivalókért lásd: az [Azure Active Directory kiadásai](active-directory-editions.md).

Jelszó visszaírást lehetővé teszi a jelszavak vissza szeretné a helyszíni Active Directory írni a felhőben bérlő konfigurálása.  Az, hogy beállítása és kezelése az önkiszolgáló jelszó alaphelyzetbe állítása bonyolult a helyszíni megoldás obviates meg, és felhőalapú kényelmesen a felhasználóknak, hogy a helyszíni jelszavak alaphelyzetbe állítása, bárhol legyenek is biztosít.  Néhány fontosabb funkciói jelszó visszaírást Olvasson tovább:

- **Nulla késleltetés visszajelzések.**  Jelszó visszaírást egy olyan szinkron művelet.  A felhasználók a program azonnal figyelmeztet, ha jelszavukat nem felelt meg a házirendet, vagy nem volt képes alaphelyzetbe állítása, vagy valamilyen okból megváltozott.
- **Támogatja az Active Directory összevonási szolgáltatások vagy más szövetséges technológiát használó felhasználók számára a jelszavak alaphelyzetbe állítása.**  A jelszó visszaírást, mindaddig, amíg a szövetséges felhasználói fiókok vannak szinkronizálva a Azure AD-bérlő be lesz lehetőségük kezelheti a helyszíni Active Directory jelszavakat a felhőből.
- **Támogatja a jelszó-szinkronizálás kivonat használó felhasználók számára a jelszavak alaphelyzetbe állítása.** Ha a jelszó alaphelyzetbe állítása szolgáltatás észleli, hogy szinkronizált felhasználói fiók kivonat jelszó-szinkronizálás engedélyezve van, alaphelyzetbe állításakor egyaránt ehhez a fiókhoz a helyszíni és felhőbeli jelszót egyszerre.
- **Támogatja a jelszavak módosítása a access panel, és az Office 365-ben.**  Ha a szövetséges vagy jelszó-szinkronizálás felhasználók jár a lejárt vagy nem járt le jelszavukat, azt kell írni ezeket a jelszavakat vissza a helyi Active Directory-környezetben.
- **Vissza jelszavak írása, rendszergazda alaphelyzetbe azokat támogatja a** [**Azure Kezelőportálja segítségével**](https://manage.windowsazure.com).  Amikor egy rendszergazda alaphelyzetbe állítása az [Azure Kezelőportálja](https://manage.windowsazure.com), ha a felhasználó számára identitásszolgáltatóval összevont vagy jelszó-szinkronizálás egy felhasználó jelszavát szeretné azt fogja megadása a helyi Active Directory, valamint a kijelöli a rendszergazdai jelszavát.  Ez jelenleg nem támogatja az Office felügyeleti központjában.
- **Kényszeríti a helyszíni Active Directory jelszóházirendek.**  Amikor egy felhasználó az oktatók jelszó alaphelyzetbe állítása, azt győződjön meg arról, hogy azt megfelel-e a helyszíni Active Directory-házirend azt, hogy a címtár elvégzése előtt.  Ide tartoznak a előzmények, összetettsége, kor, jelszó szűrők és egyéb adta meg a helyi Active Directory jelszó korlátozásokat.
- **Minden bejövő tűzfalszabályokat használatához nincs szükség.**  Jelszó visszaírást az Azure Service Bus továbbítási használja, mint a mögöttes kommunikációs csatornát, tehát, hogy nem kell nyissa meg bármelyik bejövő portokat a tűzfal ehhez a szolgáltatáshoz való használatra.
- **Felhasználói fiókok megtalálható a helyszíni Active Directoryban védett csoportokon belül nem támogatott.** Védett csoportok kapcsolatos további tudnivalókért olvassa el a [védett fiókok és csoportok, az Active Directory](https://technet.microsoft.com/library/dn535499.aspx)című témakört.

### <a name="how-password-writeback-works"></a>Hogyan működik a jelszó visszaírást
Jelszó visszaírást három fő összetevői foglalja magában:

- Jelszó alaphelyzetbe állítása felhőszolgáltatásában (ez is beépül az Azure Active Directory jelszó módosítása lap)
- Bérlői-specifikus Azure Service Bus továbbító
- Helyszíni jelszó alaphelyzetbe állítása végpont

Azok a illeszkedjenek egymáshoz leírtak szerint az alábbi ábra:

  ![][001]

Ha a szövetséges vagy jelszó kivonat szinkronizálni szeretné felhasználói érkezik vissza szeretne állítani vagy módosítani a jelszavát a felhőben, a következő történik:

1.  Akkor ellenőrizze, hogy milyen típusú jelszót a felhasználó rendelkezik.  Ha azt látja, helyszíni kezeli a jelszót, majd azt győződjön meg róla a visszaírást szolgáltatás lépéseket.  Ha igen, azt a felhasználót, a folytatáshoz engedélyezni, akkor sem, ha azt, hogy a felhasználó, amely nem itt alaphelyzetbe a jelszavát.
2.  Ezután a felhasználó adja meg a megfelelő hitelesítési gates, és a jelszó alaphelyzetbe állítása képernyő eléri.
3.  A felhasználó kijelöli az új jelszót, és megerősíti, hogy.
4.  A Küldés gombra kattintva, azt titkosítás az egyszerű szöveges szimmetrikus használatával létrehozott az visszaírást beállításakor lett megadva.
5.  Után a titkosított jelszót, akkor foglalja bele kapja a bérlői adott szolgáltatás bus továbbítási (beállított azt is, az visszaírást beállításakor lett megadva) egy HTTPS csatornán keresztül küldött hasznos.  A továbbítás védett, hogy tudja, hogy csak a helyszíni telepítésének véletlenszerűen létrehozott jelszót.
6.  Az üzenet szolgáltatás bus eléri, a jelszó alaphelyzetbe állítása végpont automatikusan üzemmódba, és láthatja, hogy rendelkezik-e egy alaphelyzetbe kérelem függőben.
7.  A szolgáltatás majd keres a felhasználó a szóban forgó a felhőben horgony attribútum használatával.  Sikeres, keresőnél a user objektumban léteznie kell, az Active Directory-összekötő helyen, a megfelelő té Elemet objektum lesz csatolva, és a megfelelő AAD összekötő objektumot kell csatolva. Végezetül ahhoz, hogy szinkronizálását, keresse meg a felhasználói fiókot, az Active Directory-összekötő objektumból hivatkozás té Elemet kell rendelkeznie a szinkronizálási szabály `Microsoft.InfromADUserAccountEnabled.xxx` hivatkozásra.  Mivel a felhőből érkezik a hívást, ha a sync engine használja a cloudAnchor attribútum lépések elvégzésével keresheti meg a AAD összekötő terület objektum, majd a hivatkozás követi a té Elemet objektummá, és ezután követi a hivatkozás visszatér az Active Directory-objektum. Több AD objektum (több erdőre) az adott felhasználó lehet, mert a szinkronizálási motor támaszkodik a `Microsoft.InfromADUserAccountEnabled.xxx` válassza a megfelelő hivatkozásra.
8.  Miután a felhasználói fiók található, azt próbálja meg közvetlenül a megfelelő AD erdőben a jelszó alaphelyzetbe állítása.
9.  Ha a jelszó beállítása a művelet sikerült, a felhasználó jelszavának módosult, és boldog úton felkeresik biztosítunk.
10. A jelszó beállítása a művelet sikertelen lesz, ha azt a hiba visszatérhet a felhasználót, és mondja el őket újra.  A művelet előfordulhat, hogy sikertelen lesz, mert a szolgáltatás nem lefelé, mert a jelszót, azok a kijelölt nem felelt szervezeti házirendek, mert azt nem található a felhasználó a helyi Active Directory, vagy tetszőleges számú okok miatt.  Hogy telepítve van egy adott üzenet ezekben az esetekben számos, és a felhasználó megállapítani, hogy mire alkalmas a probléma megoldásához.

### <a name="scenarios-supported-for-password-writeback"></a>A jelszó visszaírást támogatott felhasználási területei
Az alábbi táblázat ismerteti, hogy melyik esetek a szinkronizálási funkciók melyik verziója támogatott.  Az általános javasoljuk, hogy ha jelszó visszaírást használni kívánt telepítse az [Azure AD Connect](active-directory-aadconnect.md#install-azure-ad-connect) legújabb verzióját.

  ![][002]

### <a name="password-writeback-security-model"></a>Jelszó visszaírást biztonsági modell
Jelszó visszaírást egy olyan erősen biztonságos és robusztus szolgáltatás.  Annak érdekében, hogy a adatainak védelme érdekében azt engedélyezése 4 többszintű biztonsági modell, amely az alábbiakban olvashat bővebben.

- **Bérlői adott szolgáltatás-bus továbbítási** – beállításakor a szolgáltatást, hogy beállítása egy bérlői-specifikus szolgáltatás bus továbbítási, amely a Microsoft nem fér hozzá tízjegyű erős jelszóval védett.
- **Zárolt lefelé, kriptográfiailag erős jelszót titkosítókulcs** – a szolgáltatás bus továbbítás létrehozását követően, hozzunk létre egy erős szimmetrikus kulcsot, amely ahhoz, hogy titkosítja a jelszót, tetszés szerint a hálózaton.  A kulcs csak abban a vállalat titkos áruházból a felhőben, amely erősen rögzítve és ellenőrzött, hasonlóan a címtárban minden jelszót.
- **Iparágban szabványos TLS** – Ha a jelszó alaphelyzetbe állítása vagy módosítása a művelet fordul elő, az a felhő megtekintheti, hogy készítése a titkosított jelszót, és a nyilvános kulccsal titkosítás.  Azt majd plop, amely egy HTTPS üzenetbe, amely küldi el egy titkosított csatornán a szolgáltatás bus listától a Microsoft SSL-tanúsítványok használatával.  Után az üzenet megérkezik szolgáltatás Bus be, a helyszíni ügynök üzemmódba, volt korábban létrehozott erős jelszót használ szolgáltatási Bus hitelesíti, felveszi a titkosított üzenettel, használatával visszafejti a titkos kulcs generált azt, majd a kísérletek használatával adja meg a jelszót az Active Directory Tartományi SetPassword API-k.  Ez a lépés nem mi lehetővé teszi, hogy a hivatkozási az Active Directory helyszíni jelszóházirend (összetettsége, kor, előzmények, szűrők, stb.), hogy mi a felhőben.
- **Üzenet jelszólejárati házirendekről** – végül valamilyen oknál fogva az üzenet helyezkedik el szolgáltatás Bus, mert a helyszíni szolgáltatás nem működik, ha azt fogja időtúllépési és biztonsági még tovább növelése érdekében néhány perc után eltávolítja.

## <a name="how-does-the-password-reset-portal-work"></a>Hogyan állítsa alaphelyzetbe a jelszó portál munkát?
Amikor egy felhasználó a jelszó alaphelyzetbe állítása portálon, a munkafolyamat megrúgni annak megállapításához, hogy a felhasználói fiókhoz érvényes, milyen szervezeti tartozó felhasználók, hol kezelik a felhasználó jelszavát, és attól függetlenül, hogy a felhasználó jogosult a funkcióval.  Olvassa el az alábbi lépésekkel Ha többet szeretne tudni a logika a jelszó alaphelyzetbe állítása oldal mögött.

1.  Felhasználó kattint a nem érhető el a partner hivatkozására vagy Ugrás közvetlenül az [https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com).
2.  Felhasználói beírja a felhasználói azonosító, és átadja a képen megjelenített Ellenőrzőkód.
3.  Azure Active Directory ellenőrzi, hogy a felhasználó ezt a szolgáltatást használhatja az alábbiak szerint:
    - Ellenőrzi, hogy a felhasználó rendelkezik-e ez a funkció engedélyezve van, és az Azure Active Directory-licenccel.
        - Ha a felhasználó nem rendelkezik, ezzel a funkcióval vagy a hozzárendelt licenc, a felhasználónak meg kell adnia partnerlistájukra rendszergazdához kell fordulnia partnerlistájukra jelszó alaphelyzetbe állítása.
    - Annak vizsgálata, hogy a felhasználó rendelkezik-e a jobbra partnerlistájukra fiók rendszergazdai házirend szerint meghatározott adatokat kéri.
        - Ha csak egy kérdés a házirendhez, majd biztosítható, hogy a felhasználó rendelkezik-e a megfelelő adatokat a problémáit, a rendszergazda házirend által engedélyezett egyik definiált.
          - Ha a felhasználónak nincs konfigurálva, a felhasználó ajánlatos kérje partnerlistájukra rendszergazdáját, hogy állítsa alaphelyzetbe jelszavát.
        - Ha a házirend két kihívásokkal kapcsolatban van szüksége, majd biztosítható, hogy a felhasználó rendelkezik-e a megfelelő adatokat a problémáit, a rendszergazda házirend által engedélyezett közül legalább két definiált.
          - Ha a felhasználónak nincs beállítva, akkor azt a felhasználót, azt javasoljuk, lépjen kapcsolatba a partnerlistájukra rendszergazdáját, hogy állítsa alaphelyzetbe jelszavát.
    - Annak vizsgálata, hogy e vagy sem a felhasználók jelszava kezelik a helyszíni telepítésű (szövetséges vagy jelszó-szinkronizálás kivonat volna).
       - Ha a felhasználók jelszava kezelik helyszíni visszaírást telepítve van, a felhasználó jogosult hitelesítést végezni, és a jelszó alaphelyzetbe állítása.
       - Ha nincs telepítve az visszaírást, és a felhasználók jelszava kezelik helyszíni, majd a felhasználónak meg kell adnia a jelszó alaphelyzetbe állítása partnerlistájukra rendszergazdához kell fordulnia.
4.  Ha annak meghatározása, hogy a felhasználó nem tud sikeresen állítsa alaphelyzetbe jelszavát, majd a felhasználó az interaktív alaphelyzetbe folyamatát.

További tudnivalók a jelszó visszaírást telepítéséről [első lépések: Azure Active Directory jelszavak kezelését](active-directory-passwords-getting-started.md).

### <a name="what-data-is-used-by-password-reset"></a>Milyen adatokat a jelszó-visszaállítás használja?
Az alábbi táblázat a körvonal hol és hogyan használják a jelszó-visszaállítás során az adatokat, és célja, hogy könnyebben eldöntheti, hogy milyen hitelesítési beállítások megfelelőek a szervezet számára. Ebben a táblázatban láthatók a formázási követelményeit azokra az esetekre, ha meg van adva adatok nevében, amely nem az adatok érvényesítése beviteli út felhasználóit is.

> [AZURE.NOTE] Irodai telefon nem jelennek meg a regisztrációs portálon, mert felhasználók jelenleg nem szerkeszthető ezt a tulajdonságot a címtárban.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Kapcsolatfelvételi mód neve</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Azure Active Directory adatelem</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Használt / állítható hol?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Formátum vonatkozó követelmények</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Irodai telefon</p>
            </td>
            <td>
              <p>Telefonszám</p>
              <p>Set-MsolUser - UserPrincipalName pl. JWarner@contoso.com - telefonszám "+ 1 1234567890 x 1234"</p>
            </td>
            <td>
              <p>Használja:</p>
              <p>Jelszó alaphelyzetbe állítása portál</p>
              <p>A üzemmód:</p>
              <p>Telefonszám állítható be a PowerShell, DirSync, Azure adatkezelési portál és az Office-felügyeleti portál</p>
            </td>
            <td>
              <p>+ ccc xxxyyyzzzz (például + 1 1234567890)</p>
              <ul>
                <li class="unordered">
Biztosítania kell az országkódot<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Kell adnia a körzetszámot, (ha alkalmazható)<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Kell adnia egy + előtt ország kódot.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Rendelkeznie kell országkódot és a számot a többi között egy szóköz<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Bővítmények nem támogatottak, ha a megadott bővítések, azt törli azt a számot a előtt a telefon hívást.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Mobiltelefon</p>
            </td>
            <td>
              <p>AuthenticationPhone</p>
              <p>VAGY</p>
              <p>MobilePhone</p>
              <p>(Hitelesítés telefon használja, ha az adatok találhatók, egyéb esetben ez visszavált a mobiltelefonszám mezőjében).</p>
              <p>Set-MsolUser - UserPrincipalName pl. JWarner@contoso.com - MobilePhone "+ 1 1234567890 x 1234"</p>
            </td>
            <td>
              <p>Használja:</p>
              <p>Jelszó alaphelyzetbe állítása portál</p>
              <p>Regisztráció portál</p>
              <p>A üzemmód: </p>
              <p>AuthenticationPhone állítható be a jelszó alaphelyzetbe állítása regisztrációs portál vagy MFA regisztrációs portálon.</p>
              <p>MobilePhone állítható be a PowerShell, DirSync, Azure adatkezelési portál és az Office-felügyeleti portál</p>
            </td>
            <td>
              <p>+ ccc xxxyyyzzzz (például + 1 1234567890)</p>
              <ul>
                <li class="unordered">
Adjon meg egy országkódot.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Kell adnia a körzetszámot, (ha alkalmazható).<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Adjon egy + előtt ország kód van.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Rendelkeznie kell országkódot és a számot a többi között egy szóköz található.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Bővítmények nem támogatottak, ha a megadott bővítések, akkor figyelmen kívül hagyja a telefonhívás elküldésekor.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Másodlagos E-mail</p>
            </td>
            <td>
              <p>AuthenticationEmail</p>
              <p>VAGY</p>
              <p>AlternateEmailAddresses [0] </p>
              <p>(Hitelesítés E-mail használja, ha az adatok találhatók, egyéb esetben ez visszavált a másodlagos E-mail mező).</p>
              <p>Megjegyzés: a másodlagos e-mail mező a címtárban karakterláncok tömbje van megadva.  Ebben a tömbben fogjuk kiszámolni az első tételt.</p>
              <p>Set-MsolUser - UserPrincipalName pl. JWarner@contoso.com - AlternateEmailAddresses"email@live.com"</p>
            </td>
            <td>
              <p>Használja:</p>
              <p>Jelszó alaphelyzetbe állítása portál</p>
              <p>Regisztráció portál</p>
              <p>A üzemmód: </p>
              <p>AuthenticationEmail állítható be a jelszó alaphelyzetbe állítása regisztrációs portál vagy MFA regisztrációs portálon.</p>
              <p>AlternateEmail állítható be a PowerShell, az Azure adatkezelési portál és az Office-felügyeleti portál</p>
            </td>
            <td>
              <p>
                <a href="mailto:user@domain.com">user@domain.com</a>vagy甲斐@黒川.日本</p>
              <ul>
                <li class="unordered">
E-mailek kell követniük, mint egy szabványos formázást.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Unicode-e-mailek támogatottak.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Biztonsági kérdések és válaszok</p>
            </td>
            <td>
              <p>Nem érhető el a módosítása közvetlenül a címtárban.</p>
            </td>
            <td>
              <p>Használja:</p>
              <p>Jelszó alaphelyzetbe állítása portál</p>
              <p>Regisztráció portál </p>
              <p>A üzemmód: </p>
              <p>A csak biztonsági kérdések beállítása módja az Azure adatkezelési portálon keresztül.</p>
              <p>A csak az adott felhasználó számára biztonsági kérdésekre adott válaszok beállítása módja a regisztrációs portálon keresztül.</p>
            </td>
            <td>
              <p>Biztonsági kérdések van egy 200 karakterek maximális és a 3 karakteres min</p>
              <p>Válaszok a max 40 karakterből áll, és a min 3 karakteres van</p>
            </td>
          </tr>
        </tbody></table>

###<a name="how-to-access-password-reset-data-for-your-users"></a>Elsajátíthatja a jelszó alaphelyzetbe állítása az adatokat a felhasználók számára
####<a name="data-settable-via-synchronization"></a>Adatok állítható be a szinkronizálás keresztül
Az alábbi mezők szinkronizálható a helyszíni:

* Mobiltelefon
* Irodai telefon

####<a name="data-settable-with-azure-ad-powershell"></a>Adatok állítható be az Azure Active Directory PowerShell
Az alábbi mezőket az Azure Active Directory PowerShell és a diagram API érhetők el:

* Másodlagos E-mail
* Mobiltelefon
* Irodai telefon
* Hitelesítés telefonon
* Hitelesítés e-mailben

####<a name="data-settable-with-registration-ui-only"></a>Állítható be a felhasználói felület regisztrációs adatok
A SSPR regisztráció felhasználói felület (https://aka.ms/ssprsetup) keresztül csak érhetők el az alábbi mezőket:

* Biztonsági kérdések és válaszok

####<a name="what-happens-when-a-user-registers"></a>Mi történik, amikor a felhasználó regisztrálja?
Amikor a felhasználó regisztrálja, a regisztráció lap fog **mindig** beállítása az alábbi mezőket:

* Hitelesítés telefonon
* Hitelesítés e-mailben
* Biztonsági kérdések és válaszok

Ha a **mobiltelefon** vagy a **Másodlagos E-mail**érték van megadva, felhasználók azonnal segítségével azokat, azok a jelszavak alaphelyzetbe állítása, ha az azok a szolgáltatás még nem regisztrált.  Ezeken kívül felhasználók ezeket az értékeket jelenik meg, amikor először regisztrálása, és azokat módosítani, ha szeretné, hogy.  Jó helyen jár Miután sikeresen regisztrálja, ezeket az értékeket maradnak a **Hitelesítési telefon** és a **Hitelesítési E-mail** mezőket a kurzor.

Ez akkor lehet hasznos nagyszámú felhasználók használhassák a SSPR miközben továbbra is lehetővé teszi a felhasználóknak, hogy ezt az információt a regisztrációs folyamat lépésein ellenőrzése letiltásának feloldása lehetőséget.

####<a name="setting-password-reset-data-with-powershell"></a>Jelszó beállítása a PowerShell adatok alaphelyzetbe állítása
Beállíthatja, hogy az Azure Active Directory PowerShell az alábbi mezők értékét.

* Másodlagos E-mail
* Mobiltelefon
* Irodai telefon

Első lépésként először kell [Töltse le és telepítse az Azure Active Directory PowerShell-modult](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule).  Ha már telepítve van, kövesse az alábbi lépéseket követve állítsa be az egyes mezők.

#####<a name="alternate-email"></a>Másodlagos E-mail
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
```

#####<a name="mobile-phone"></a>Mobiltelefon
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
```

#####<a name="office-phone"></a>Irodai telefon
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"
```

####<a name="reading-password-reset-data-with-powershell"></a>Olvasási jelszó alaphelyzetbe állítása PowerShell adataival
Erről az Azure Active Directory PowerShell az alábbi mezők értékét.

* Másodlagos E-mail
* Mobiltelefon
* Irodai telefon
* Hitelesítés telefonon
* Hitelesítés e-mailben

Első lépésként először kell [Töltse le és telepítse az Azure Active Directory PowerShell-modult](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule).  Ha már telepítve van, kövesse az alábbi lépéseket követve állítsa be az egyes mezők.

#####<a name="alternate-email"></a>Másodlagos E-mail
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
```

#####<a name="mobile-phone"></a>Mobiltelefon
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
```

#####<a name="office-phone"></a>Irodai telefon
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber
```

#####<a name="authentication-phone"></a>Hitelesítés telefonon
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
```

#####<a name="authentication-email"></a>Hitelesítés e-mailben
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

## <a name="links-to-password-reset-documentation"></a>Jelszó mutató hivatkozások dokumentáció alaphelyzetbe állítása
Az alábbiakban minden Azure Active Directory-jelszó-visszaállítás dokumentáció mutató hivatkozások:

* **Itt azért, mert netán problémákat tapasztal bejelentkezéskor?** Ha igen, [az alábbiakban hogyan módosíthatja, és a saját jelszó alaphelyzetbe állítása](active-directory-passwords-update-your-own-password.md).
* [**Működéséről**](active-directory-passwords-how-it-works.md) - megismerheti a szolgáltatást, és mit hat különböző összetevői minden tartalmaz
* [**Első lépések**](active-directory-passwords-getting-started.md) – megtudhatja, hogy miként lehetővé teszi a felhasználóknak, hogy alaphelyzetbe állítása és a felhő vagy a helyszíni jelszavukat
* [**Testreszabása**](active-directory-passwords-customize.md) – a Megjelenés és működés és a szolgáltatást, hogy a szervezet igényei működésének testreszabása
* [**Ajánlott eljárások**](active-directory-passwords-best-practices.md) – útmutató telepítését gyorsan és hatékonyan a jelszavak a szervezet kezelése
* [**Háttérismeretek get**](active-directory-passwords-get-insights.md) - az integrált jelentéskészítési képességeinek ismertetése
* [**Gyakori kérdések**](active-directory-passwords-faq.md) – gyakori kérdések a válaszok
* [**Hibaelhárítás**](active-directory-passwords-troubleshoot.md) – ismerje meg, a szolgáltatással gyorsan problémáinak megoldása



[001]: ./media/active-directory-passwords-learn-more/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-learn-more/002.jpg "Image_002.jpg"

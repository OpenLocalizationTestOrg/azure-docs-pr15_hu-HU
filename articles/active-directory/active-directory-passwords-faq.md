<properties
    pageTitle="Gyakori kérdések: Azure AD-jelszó Management |} Microsoft Azure"
    description="Gyakori kérdések kezelésről jelszót az Azure Active Directory, beleértve jelszó alaphelyzetbe állításához regisztrációs, jelentések és visszaírást a helyszíni Active Directory."
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

# <a name="password-management-frequently-asked-questions"></a>Gyakori kérdések a jelszó kezelése

> [AZURE.IMPORTANT] **Itt azért, mert netán problémákat tapasztal bejelentkezéskor?** Ha igen, [az alábbiakban hogyan módosíthatja, és a saját jelszó alaphelyzetbe állítása](active-directory-passwords-update-your-own-password.md).

Az alábbiakban az összes elhárításukhoz jelszó kezelésével kapcsolatos gyakori kérdésekre.

Ha megtalálta a saját maga a kérdést, amely nem tudja, hogy a választ, vagy egy adott probléma van a néző segítséget keres, erről a alatti megtekintéséhez, ha azt már szereplő azt már.  Ne aggódjon, ha azt még nem tette meg! Nyugodtan az [Azure Active Directory-fórumok](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD) nem szereplő itt van, és azt fogja vissza szeretne lépni, amint azt is, kérje meg bármely kérdést.

Ez a cikk az alábbi szakaszok oszlik:

- [**Jelszó alaphelyzetbe állítása regisztrációs kapcsolatos kérdések**](#password-reset-registration)
- [**A jelszó-visszaállítás kapcsolatos kérdések**](#password-reset)
- [**Jelszó felügyeleti jelentések kapcsolatos kérdések**](#password-management-reports)
- [**Jelszó visszaírást kapcsolatos kérdések**](#password-writeback)

## <a name="password-reset-registration"></a>Az új jelszó kérésének regisztráció
 - **Kérdés: van lehetőség a felhasználók számára a saját jelszó alaphelyzetbe állítása adatok regisztrálni?**

 > **A:** Igen, mindaddig, amíg a jelszó-visszaállítás engedélyezve van, és azok licence, azok is lépjen a jelszó alaphelyzetbe állítása regisztrációs portálra a http://aka.ms/ssprsetup regisztrálhatja a hitelesítési adatot a jelszó-visszaállítás használható. Felhasználók által az access panelen a http://myapps.microsoft.com, kattintson a profil lapon, és kattintson a Regisztrálás, a jelszó alaphelyzetbe állítása lehetőséget is rögzítheti. További tudnivalók a felhasználók beállított jelszó alaphelyzetbe állításához, hogyan szerezhető be van állítva a jelszó-visszaállítás felhasználók olvasásával beszerzése.

 - **K: jelszó alaphelyzetbe állítása adatokat is definiáló nevében a felhasználók számára?**

 > **A:** Igen, meg tehetné DirSync vagy PowerShell, illetve az [Azure felügyeleti portál](https://manage.windowsazure.com) vagy az Office felügyeleti portálon keresztül. További tudnivalók az ezt a szolgáltatást a blogbejegyzés továbbfejlesztett adatvédelmi az Azure Active Directory MFA és a jelszó alaphelyzetbe állítása telefonszámok és olvasási megtudhatja, hogyan adatokat használja, jelszó alaphelyzetbe állítása.

 - **Kérdés: van lehetőség a biztonsági kérdésekre a helyszíni adatok szinkronizálása?**

 > **A:** Nem, ez azonban nem lehet ma, de azt is, figyelembe véve.

 - **Probléma: a felhasználók számára regisztrálhatnak adatok oly módon, hogy más felhasználók nem látják az adatok?**

 > **A:** Igen, amikor a felhasználók regisztrálása az adatoknak a jelszó alaphelyzetbe állítása regisztrációs portál azt tartalmaznak, amelyek csak láthatók a magánjellegű hitelesítési mezőkbe globális rendszergazdák és a felhasználó saját maga által. További tudnivalók az ezt a szolgáltatást a blogbejegyzés továbbfejlesztett adatvédelmi az Azure Active Directory MFA és a jelszó alaphelyzetbe állítása telefonszámok és olvasási megtudhatja, hogyan adatokat használja, jelszó alaphelyzetbe állítása.

 - **Probléma: a felhasználók számára a számítógépemen regisztrálását, mielőtt használni tudja a jelszó-visszaállítás?**

 > **A:** Nem, ha elég hitelesítési adatait más nevében, felhasználók nem lesz regisztrálni. Mindaddig, amíg a megfelelő mezőket a címtárban tárolt adatok megfelelően formázott jelszó-visszaállítás rendben működik. További tudnivalók a többet által olvasási megtudhatja, hogyan adatokat a jelszó-visszaállítás használja.

 - **Kérdés: szinkronizálás, illetve -e a hitelesítési telefonon, a hitelesítési e-mailbe vagy a hitelesítési telefonszámok mező beállítása a felhasználók nevében?**

 > **A:** Jelenleg nem de azt is figyelembe véve engedélyezése Ezt a lehetőséget.

 - **Kérdés: hogyan tudja a regisztrációs portál kattintva jelenítse meg a felhasználók milyen lehetőségekre?**

 > **A:** A jelszó alaphelyzetbe állítása regisztrációs portál csak jeleníti meg a beállításokat, hogy engedélyezte, akkor a felhasználók csoportban a címtárában konfigurálása lapon a felhasználói jelszó alaphelyzetbe állítása házirend című szakaszát. Ez azt jelenti, hogy ha nem engedélyezi, azaz biztonsági kérdések, majd felhasználók fog nem tudja regisztrálni ezt a beállítást.

 - **Probléma: amikor egy felhasználó számít regisztrálni?**

 > **A:** Egy felhasználó regisztrált, ha az illető kikkel legalább tekinthető hitelesítési információ N hardvereszközöket definiálva, ahol "N" a számot a hitelesítési módszerek szükséges az [Azure Kezelőportálja](https://manage.windowsazure.com)beállított. További tudnivalókért olvassa el a testreszabása felhasználói jelszó alaphelyzetbe állítása házirend című témakört.


## <a name="password-reset"></a>Jelszó alaphelyzetbe állítása

 - **Kérdés: mennyi ideig várjon fogadni jelszó-visszaállítás egy e-mail, SMS vagy telefonhívásban?**

 > **A:** E-mail, SMS-üzenetek és a hívásokat kell kapják meg üzeneteiket az 1 perc alatt, a normál esetben, 5-20 másodperc alatt. Ha nem kap értesítést a időkeret, jelölje be a Levélszemét mappába, amely a szám / alatt kapcsolatba lépni Önnel e-mailek, hogy megfelelően formázott a címtárban hitelesítési adatait, és a várt egy. Ha többet szeretne tudni a formázást, telefonszámok és e-mail címeket jelszó-visszaállítás való használatra ismerje meg, hogy milyen adatokat használja, a jelszó-visszaállítás lásd:.

 - **Kérdés: milyen nyelveken jelszó-visszaállítás által támogatott?**

 > **A:** A jelszó alaphelyzetbe állítása felhasználói felülete SMS-üzenetek és hanghívásokat honosítva legyenek-e az Office 365 által támogatott nyelvekhez azonos 40. Láthatóvá válnak: arab, bolgár, egyszerűsített kínai, hagyományos kínai, horvát, Cseh, dán, holland, angol, észt, finn, francia, német, görög, héber, Hindi, magyar, indonéz, olasz, japán, kazak, koreai, lett, litván, maláj (Malajzia), norvég (Bokmål), lengyel, portugál (brazíliai), portugál (portugáliai), román, orosz, szerb (latin betűs), szlovák, szlovén, spanyol, svéd, Thai, török, ukrán, és vietnami.

 - **Kérdés: milyen részeket, a jelszó alaphelyzetbe állítása, amikor első márka, amikor állíthatom be, a címtárban védjegyzés szervezeti adatait a konfigurálása lapon?**

 > **A:** A jelszó alaphelyzetbe állítása portál jelennek meg a szervezeti embléma, és lehetővé teszi, hogy a partner konfiguráljon URL-cím vagy egyéni e-mailek átirányítása a rendszergazda hivatkozásra. Minden olyan e-mailt, kapja a jelszó-visszaállítás küldte tartalmazni fogja a szervezet emblémáját, a színek (ebben az esetben piros), az e-mailt törzsében nevét, és név az egyéni. Példa a a védjegyzett az alábbi elemek. További tudnivalókért olvassa el a jelszó-visszaállítás testreszabása megjelenését és működését.

  ![][001]

 - **Kérdés: hogyan lehet tájékoztassa a felhasználókat helyre látogasson el a jelszavak alaphelyzetbe állítása?**

 > **A:** Elküldheti a felhasználók https://passwordreset.microsoftonline.com közvetlenül vagy is kérje meg őket, kattintson a nem tud hozzáférni a fiókkal hivatkozásra bármelyik iskolai vagy munkahelyi azonosító bejelentkezési képernyője található. Akkor is nyugodtan közzététele ezeket a hivatkozásokat (vagy létrehozása URL-cím átirányítások el őket), hogy a felhasználók könnyen hozzáférhető helyen.

 - **Kérdés: van lehetőség a mobileszközön lévő lapon?**

 > **A:** Igen, ezen az oldalon mobileszközökön működik.

 - **Kérdés: támogatják a zárolás feloldása helyi active directory-fiókok esetén a felhasználók a jelszavak alaphelyzetbe állítása?**

 > **A:** Igen, amikor a felhasználó visszaállítása jelszavas vagy jelszó visszaírást már telepített összes verziójának Azure AD Connect vagy Azure AD-szinkronizálás 1.0.0485.0222 verziói vagy újabb verzió, akkor adott felhasználói fiók automatikusan nem zárolt lesz, amikor a felhasználó számára partnerlistájukra jelszó alaphelyzetbe állítása.

 - **Kérdés: hogyan integrálhatja a jelszó alaphelyzetbe állításához, közvetlenül a felhasználó asztali bejelentkezés módja?**

 > **A:** Ez nem lehetséges ma. Jó helyen jár ha feltétlenül szükséges ezt a lehetőséget, és Azure Active Directory prémium verziót használ, telepítésével Microsoft Identitáskezelő külön költség nélkül és megoldása a követelmény az ott található helyszíni jelszó alaphelyzetbe állítása megoldást üzembe helyez.

 - **Kérdés: elérhető különböző biztonsági kérdések a különféle nyelvekhez beállítani?**

 > **A:** Nem, ez azonban nem lehet ma, de azt is, figyelembe véve.

 - **Kérdés: hogyan számos kérdésre azt adhatja biztonsági kérdések hitelesítési beállítással?**

 > **A:** Legfeljebb 20 egyéni biztonsági kérdések beállíthatja az [Azure Kezelőportálja segítségével](https://manage.windowsazure.com).

 - **Kérdés: hogyan hosszú biztonsági kérdések lehet?**

 > **A:** Biztonsági kérdések között 3 és 200 karakter hosszú lehet.

 - **Kérdés: hogyan hosszú biztonsági kérdésekre adott válaszok lehet?**

 > **A:** Válaszok a 3-40 karakter hosszú lehet.

 - **Probléma: ismétlődő kérdéseire választ kaphat az biztonsági elutasítva vannak?**

 > **A:** Igen, ismétlődő biztonsági kérdésekre adott válaszok elutasítja azt.

 - **Kérdés: május a felhasználó regisztrálni egynél több ugyanazt a biztonsági kérdést?**

 > **A:** Nem, amikor a felhasználó regisztrálja az adott kérdést, akkor hatékonyabbnak nem regisztrálhat, a kérdésre adott másodszori.

 - **Kérdés: van erre lehetőség minimális korlátozhatja a regisztráció biztonsági kérdések és alaphelyzetbe?**

 > **A:** Igen, egy korlát beállítható, hogy a regisztrációs és egy másik alaphelyzetbe állítása. regisztráció szükség lehet a 3-5 biztonsági kérdések, és a 3-5 alaphelyzetbe szükség lehet.

 - **Probléma: Ha egy felhasználó regisztrált több, mint a maximális száma kérdések alaphelyzetbe kell állítani, hogyan biztonsági kérdések kijelöljön alaphelyzetbe során?**

 > **A:** N biztonsági kérdések véletlenszerűen kiválasztott kívül az összes felhasználó regisztrálva van, ha az N a kérdésekre, a jelszó-visszaállítás szükséges minimális számú kérdések számát. Például ha egy felhasználó regisztrált 5 biztonsági kérdések rendelkezik, de csak 3 van szükség, hozzon létre egy új, az adott 5 3 fog kell véletlenszerűen kiválasztott és a felhasználónak a alaphelyzetbe idején megjelenő. Ha a felhasználó megkapja a kérdésekre adott válaszok nem megfelelő, a kijelölés folyamat újra jelentkezik, hogy a kérdés beütés.

 - **Kérdés: ne, hogy felhasználók kísérel meg a jelszó alaphelyzetbe állításához, hány alkalommal a rövid időre?**

 > **A:** Igen, a jelszó-visszaállítás épített számos biztonsági szolgáltatások vannak. Előfordulhat, hogy csak próbálnak 5 jelszó alaphelyzetbe állítása próbálkozás után egy órával 24 órát jelentő zárolt előtt. Felhasználók előfordulhat, hogy csak próbálja meg a telefonszám után egy órával előtt 24 órát jelentő zárolt 5 megszorzása ellenőrzése. Előfordulhat, hogy csak próbálnak egy egyetlen hitelesítési módszer 5 megszorzása belül egy óra 24 órát jelentő zárolt előtt.

 - **Kérdés:, hogy mennyi ideig, hogy az e-mail és SMS egyszer használatos hitelesítő kódot érvényes?**

 > **A:** A jelszó-visszaállítás munkamenet élettartamának 105 perc. Ez azt jelenti, hogy az elejétől a jelszó alaphelyzetbe művelet a felhasználónak 105 perc partnerlistájukra jelszó alaphelyzetbe állítása. Az e-mail és SMS egyszer használatos hitelesítő kódot érvénytelenek, ez az időszak lejárta után.


## <a name="password-management-reports"></a>Jelszó felügyeleti jelentések

 - **Kérdés: hogyan ideig tart az adatok jelennek meg a jelszót felügyeleti jelentések?**

 > **A:** Adatok 5-10 percen belül megjelennek a jelszó management jelentésekben. Akkor bizonyos esetekben igénybe vehet órává jelenik meg.

 - **Kérdés: hogyan szűrheti a jelszó felügyeleti jelentések?**

 > **A:** A rendkívüli jobbra az oszlopfejléceket, a jelentés legfelül lévő kis nagyítóra kattintva szűrheti a jelszó felügyeleti jelentések (lásd: a kép). Ha szeretne tenni a gazdagabb szűrés, letöltheti a jelentést az excel és a kimutatás létrehozása.

  ![][002]

 - **Kérdés: Mi az, hogy események maximális száma a jelszó felügyeleti jelentések tárolódnak?**

 > **A:** Legfeljebb 1000 jelszó alaphelyzetbe állítása vagy a jelszó alaphelyzetbe állítása regisztrációs események a jelszó felügyeleti jelentések tárolja.  Dolgozunk, ennek a számnak további eseményekre kibontásához.

 - **Probléma: a mennyivel újra a jelszót felügyeleti jelentések lépjen?**

 > **A:** A jelszó felügyeleti jelentések megjelenítése az elmúlt 30 nap előforduló műveletek. Jelenleg vizsgáljuk hogyan teheti ezt egy hosszabb időszakra. Most Ha módosítani szeretné az adatok archiválása letöltheti a jelentések rendszeres és mentheti őket külön helyen.

 - **Kérdés: van-e a jelszó management jelentéseken megjelenő sorok maximális száma?**

 > **A:** Igen, legfeljebb 1000 sorok előfordulhat, hogy jelenik meg a jelszavak kezelését jelentések közül választhat, hogy a felhasználói felületén jelennek alatt vagy letöltődő. Jelenleg vizsgáljuk növeléséről ezt a korlátot.

 - **Kérdés: van-e egy API-t, a jelszó alaphelyzetbe állítása vagy jelentésadatainak regisztrációs eléréséhez?**

 > **A:** Igen, tanulmányozza a következő dokumentáció megtudhatja, hogyan adatfolyam jelentése a jelszó-visszaállítás érheti el.  [Ismerje meg, hogy elsajátíthatja a jelszó alaphelyzetbe állítása jelentéskészítési események programozás útján](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).

## <a name="password-writeback"></a>Jelszó visszaírást
 - **Kérdés: hogyan működik a jelszó visszaírást Bepillantás a színfalak mögé?**

 > **A:** Lásd: [hogyan jelszó visszaírást működik,](active-directory-passwords-learn-more.md#how-password-writeback-works) mi történik, ha a jelszó visszaírást, miként vissza a helyszíni környezetbe a rendszer folyik adatok, valamint engedélyezése részletes leírását. Lásd: hogyan jelszó visszaírást [jelszó visszaírást biztonsági modell](active-directory-passwords-learn-more.md#password-writeback-security-model) megtudhatja, hogyan azt legyen jelszó visszaírást nagyon biztonságos szolgáltatás működik.

 - **Kérdés: hogyan hosszú jelszó visszaírást tart a munkát?  Van-e egy szinkronizálási késleltetés, például a jelszó-szinkronizálás kivonat?**

 > **A:** Jelszó visszaírást azonnali. Egy szinkronizált folyamat, amely kivonat jelszó-szinkronizálás alapvetően eltérően működik. Jelszó visszaírást lehetővé teszi, hogy a felhasználók számára beolvasása a jelszó-visszaállítás egyik valós idejű visszajelzést, vagy módosítsa a művelet. A sikeres visszaírást jelszó átlagos ideje alatt 500 ms.

 - **Kérdés: milyen típusú fiókokhoz jelszó visszaírást esetén működik?**

 > **A:** Jelszó visszaírást külső működik, és jelszó-szinkronizálás kivonat ellátott felhasználók.

 - **Kérdés: eladja a jelszó visszaírást hivatkozási jelszóházirendek a tartomány?**

 > **A:** Igen, a jelszó visszaírást kényszeríti jelszó kora, előzmények, összetettsége, szűrők és bármely más korlátozás, előfordulhat, hogy helyezi el a jelszavak hely a helyi tartományban.

 - **Kérdés: van a biztonságos jelszó visszaírást?  Hogyan lehet, hogy e nem első megtámadott?**

 > **A:** Igen, a jelszó visszaírást rendkívül biztonságos. Bővebben is olvashat arról, hogy a 4 rétegek biztonsági jelszó visszaírást szolgáltatás által megvalósított, olvassa el a jelszó visszaírást-hogyan működik a [jelszó visszaírást biztonsági modell](active-directory-passwords-learn-more.md#password-writeback-security-model) .




## <a name="links-to-password-reset-documentation"></a>Jelszó mutató hivatkozások dokumentáció alaphelyzetbe állítása
Az alábbiakban minden Azure Active Directory-jelszó-visszaállítás dokumentáció mutató hivatkozások:

* **Itt azért, mert netán problémákat tapasztal bejelentkezéskor?** Ha igen, [az alábbiakban hogyan módosíthatja, és a saját jelszó alaphelyzetbe állítása](active-directory-passwords-update-your-own-password.md).
* [**Működéséről**](active-directory-passwords-how-it-works.md) - megismerheti a szolgáltatást, és mit hat különböző összetevői minden tartalmaz
* [**Első lépések**](active-directory-passwords-getting-started.md) – megtudhatja, hogy miként lehetővé teszi a felhasználóknak, hogy alaphelyzetbe állítása és a felhő vagy a helyszíni jelszavukat
* [**Testreszabása**](active-directory-passwords-customize.md) – a Megjelenés és működés és a szolgáltatást, hogy a szervezet igényei működésének testreszabása
* [**Ajánlott eljárások**](active-directory-passwords-best-practices.md) – útmutató telepítését gyorsan és hatékonyan a jelszavak a szervezet kezelése
* [**Háttérismeretek get**](active-directory-passwords-get-insights.md) - az integrált jelentéskészítési képességeinek ismertetése
* [**Hibaelhárítás**](active-directory-passwords-troubleshoot.md) – ismerje meg, a szolgáltatással gyorsan problémáinak megoldása
* [**Tudjon meg többet**](active-directory-passwords-learn-more.md) - lépjen a mély be a technikai részleteket az, hogy a szolgáltatás működése


[001]: ./media/active-directory-passwords-faq/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-faq/002.jpg "Image_002.jpg"

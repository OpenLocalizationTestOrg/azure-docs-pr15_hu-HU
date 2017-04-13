<properties 
    pageTitle="Azure többtényezős hitelesítés és az Active Directory címtár integrációja"
    description="Ez a ismerteti, hogyan integrálása az Azure többtényezős hitelesítést kiszolgáló az Active Directoryval, hogy szinkronizálhatja a könyvtárak Azure többtényezős hitelesítést weblapot."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="directory-integration-between-azure-mfa-server-and-active-directory"></a>Azure MFA-kiszolgáló és az Active Directory címtár integrációja

A címtár-integrációs szakasz lehetővé teszi, a kiszolgáló konfigurálása való integráció az Active Directory vagy egy másik LDAP-címtár.  Lehetővé teszi a jellemzőket felel meg a könyvtár sémának, és állítsa be a felhasználók automatikus szinkronizálás beállítása.

## <a name="settings"></a>Beállítások
Az Azure többtényezős hitelesítést kiszolgáló alapértelmezés szerint be van állítva importálásához vagy szinkronizál felhasználókat az Active Directoryból.  A lap lehetővé teszi a írja felül az alapértelmezett működés és egy másik LDAP-címtár, egy ADAM címtár vagy az adott Active Directory tartományi vezérlő.  Itt is proxyhoz LDAP LDAP hitelesítési használatra vagy LDAP kötni célként RADIUS, előtti hitelesítést az IIS vagy felhasználói portál elsődleges hitelesítés.  Az alábbi táblázat ismerteti az egyes beállításokat.

![A beállítások](./media/multi-factor-authentication-get-started-server-dirint/dirint.png)

| A szolgáltatás | Leírás |
| ------- | ----------- |
| Az Active Directory használatával | Válassza az Active Directory használható az importálás és a szinkronizálás a használja az Active Directory lehetőséget.  Ez az alapértelmezett. <br>Megjegyzés: A számítógép kell csatlakoztatni egy tartományt, és be kell jelentkezni tartomány fiókkal az Active Directory-integráció működik megfelelően. |
| Megbízható tartomány felvétele | A megbízható tartomány felvétele jelölőnégyzetet, hogy a ügynök kísérlet tartományok csatlakozhat az aktuális tartomány, egy másik a erdő vagy tartományok Megbízható jelölőnégyzet erdőszintű megbízhatóság részt.  Nem importálása és szinkronizálása megbízható tartomány felhasználóit, törölje a jelet a teljesítmény javítása érdekében jelölőnégyzet.  Alapértelmezés szerint be van jelölve. |
| Adott LDAP konfiguráció használata | Válassza az importálás és a szinkronizálás megadott LDAP beállításait a LDAP használata lehetőséget. Megjegyzés: Ha használata LDAP be van jelölve, a felhasználói felület módosításaira hivatkozások Active Directoryból LDAP. |
| Gomb képe | A Szerkesztés gomb lehetővé teszi, hogy az aktuális LDAP konfigurációs beállítások módosítani. |
| Attribútum hatókör lekérdezések használata | Azt jelzi, hogy attribútum hatókör lekérdezést kell használni.  Attribútum hatókör lekérdezések lehetővé teszi a bejegyzéseket, egy másik rekord attribútumból alapuló rekordok minősítése hatékony címtárban való keresést.  Az Azure többtényezős hitelesítést kiszolgáló attribútum hatókör lekérdezések hatékony a felhasználókat a biztonsági csoport tagjának lekérdezés használja.   <br>Megjegyzés: Vannak bizonyos esetekben, ahol attribútum hatókör lekérdezések használata támogatott, de ne használható.  Ha például az Active Directory beállíthatja, hogy attribútum hatókör lekérdezések kapcsolatos problémák biztonsági csoport tagjai egynél több tartományból tartalmazó.  Ebben az esetben a jelölőnégyzet be legyen bejelölve. |

Az alábbi táblázat ismerteti a LDAP beállításai.

| A szolgáltatás | Leírás |
| ------- | ----------- |
| Kiszolgáló | Adja meg a hostname (állomásnév) az LDAP-címtár rendszert futtató kiszolgáló IP-címet.  A biztonsági kiszolgálón is is meg kell adni egy pontosvesszővel elválasztva. <br>Megjegyzés: Ha típusa kötést létrehozni az SSL, egy teljesen minősített hostname (állomásnév) kell általában. |
| Alap megkülönböztető | Adja meg az alap directory-objektum, amelytől kezdve minden címtár lekérdezés elkezdenek megkülönböztető nevét.  Ha például adatközpont = abc, adatközpont = hu. |
| Kötést típusa - lekérdezések | Válassza ki a megfelelő kötési használatra kötés az LDAP-címtár kereséséhez.  Import, szinkronizálása és felhasználónév felbontás szolgál. <br><br>  Névtelen – egy névtelen kötés fog történni.  Kötés DN és a jelszó kötni nem fogja használni.  Ez csak akkor fog működni, ha az LDAP-címtár lehetővé teszi, hogy a névtelen kötés, és engedélyek, hogy a megfelelő rekordok és attribútumok lekérdezésére.  <br><br> Egyszerű - kötés DN és a jelszó kötni szeretné kötni az LDAP-címtár egyszerű szövegként át kell adni.  Ez csak használandó tesztelésre, és ellenőrizze, hogy a kiszolgálón is elérhető, és, hogy a kötés fiók rendelkezik-e a megfelelő hozzáférési.  Javasoljuk, hogy az SSL használható helyette a megfelelő tanúsítványának telepítése után.  <br><br> SSL - DN kötés, és a jelszó kötni titkosítja az LDAP-címtár kötést létrehozni az SSL használatával.  Ehhez a tanúsítvány telepített helyi meghajtóra, hogy megbízik-e az LDAP-címtár.  <br><br> Windows - kötés felhasználónév és jelszó kötni használandó az Active Directory tartományi vezérlő vagy ADAM címtár biztonságosan csatlakoztathassa.  Ha üresen hagyja kötni felhasználónevét, a bejelentkezett felhasználó fiókjának kötést lesz. |
| Kötést típusa - hitelesítés | Válassza ki a megfelelő kötési használatra LDAP kötés hitelesítése során.  Lásd: a kötés, írja be a kötés típusa - lekérdezések leírások.  Például ez lehetővé teszi, hogy a névtelen kötés lekérdezések használható, bár SSL kötés LDAP kötés hitelesítési biztonságos. |
| Kötés DN vagy kötési felhasználónév | Adja meg a fiók használhatók, amikor az LDAP-címtár kötése a felhasználói rekord megkülönböztető név.<br><br>Kötés megkülönböztető név csak böngészik kötni típus egyszerű vagy SSL.  <br><br>Adja meg a Windows felhasználói fiók használhatók, amikor az LDAP-címtár kötése kötni típus Windows esetén a felhasználónevet.  Ha üresen hagyja a mezőt, a bejelentkezett felhasználó fiókjának kötést lesz. |
| Kötést létrehozni a jelszó | Írja be a kötés jelszavát a kötést DN vagy az LDAP-címtár kötni használt felhasználónév.  Többtényezős hitelesítés kiszolgáló AdSync szolgáltatás adja meg a jelszót, szinkronizálás engedélyezve kell lennie és futnia kell a szolgáltatást a helyi számítógépen.  A jelszó a fiókon többtényezős hitelesítés kiszolgáló AdSync szolgáltatás működik, mint a Windows tárolt felhasználóneveket és jelszavakat menti.  A jelszó is menti, kattintson a fiókot, a többtényezős hitelesítés kiszolgáló felhasználói felület fut, és a fiókot, a többtényezős hitelesítés kiszolgáló szolgáltatás működik, mint a.  <br><br> Megjegyzés: A jelszó csak az a helyi kiszolgáló a Windows-tárolt felhasználóneveket és jelszavakat tárolja, mivel ebben a lépésben fog kell végrehajtania-kiszolgálókon többtényezős hitelesítés a jelszót a hozzáférést igénylő. |
| Lekérdezés méretkorlátja | Adja meg a könyvtár keresés ad vissza, felhasználók maximális száma méretkorlátot.  Ezt a korlátot meg kell egyeznie a konfigurációt a az LDAP-címtár.  Nagy keresésekhez, ahol nem támogatott a lapozás importálása és szinkronizálása megkísérli beolvasni kötegekben felhasználók.  Ha méretén megadva, az alábbiakban a korlátot konfigurált az LDAP-címtár-nál nagyobb, az egyes felhasználók elmulasztottam. |
| Képteszt gomb | A vizsgálat gombra kattintva tesztelje az LDAP-kiszolgáló kötés.  <br><br> Megjegyzés: A használata LDAP beállítás nem szükséges kötés tesztelése aktívnak.  Ezzel a kötést vizsgálni az LDAP konfiguráció használata előtt. |

## <a name="filters"></a>Szűrők
Szűrők ahhoz, hogy rekordok címtár keresés végrehajtásakor feltétel megadása teszi lehetővé.  A szűrő megadásával is korlátozhatja a szinkronizálni kívánt objektumokat.  

![Szűrők](./media/multi-factor-authentication-get-started-server-dirint/dirint2.png)

Azure többtényezős hitelesítés rendelkezik az alábbi 3 beállítások.

- **A tároló szűrő** - ahhoz, hogy a tároló rekordok címtár keresés végrehajtása során használt szűrőfeltételek.  Az Active Directory és ADAM (|} () objectClass=organizationalUnit)(objectClass=container)) általában használatos.  Más LDAP könyvtárak attól függően, hogy a címtár-sémát, amely megfelel a különböző típusú objektum szűrőfeltételeket kell használni.  <br>Megjegyzés: Ha üresen hagyja, ((objectClass=organizationalUnit)(objectClass=container)) lesz alapértelmezés szerint.

- **Biztonsági csoport szűrő** - ahhoz, hogy biztonsági csoport rekordok címtár keresés végrehajtása során használt szűrőfeltételek.  Az Active Directory és ADAM (&(objectCategory=group) (groupType:1.2.840.113556.1.4.804:=-2147483648)) általában használatos.  Más LDAP könyvtárak, amelyek minden típusú biztonsági csoport objektumának minősíti szűrőfeltételeket attól függően, hogy a címtár-séma kell használni.  <br>Megjegyzés: Ha üresen hagyja, (&(objectCategory=group) (groupType:1.2.840.113556.1.4.804:=-2147483648)) alapértelmezés szerint lesz.

- **Felhasználói szűrő** - ahhoz, hogy felhasználói bejegyzések címtár keresés végrehajtása során használt szűrőfeltételek.  Az Active Directory és ADAM, (és (objectClass=user)(objectCategory=person)) általában használják.  Más LDAP könyvtárak (objectClass = inetOrgPerson) és hasonló attól függően, hogy a címtár-séma nem használható. <br>Megjegyzés: Ha üresen hagyja, (és (objectCategory=person)(objectClass=user)) lesz alapértelmezés szerint.

## <a name="attributes"></a>Attribútumok
Egy adott könyvtár szükség szerint testre szabható attribútumok.  Lehetővé teszi, hogy hozzáadása egyéni attribútumok és finomhangolása a szinkronizálás csak a szükséges attribútumokat.  Minden egyes attribútum mező értékét a címtár-séma megadottak attribútum nevét kell lennie.  További információt az alábbi táblázatban használja.

![Attribútumok](./media/multi-factor-authentication-get-started-server-dirint/dirint3.png)

| A szolgáltatás | Leírás |
| ------- | ----------- |
| Egyedi azonosító | Adja meg, hogy az egyedi azonosító tároló, a biztonsági csoport és a felhasználók bejegyzéseit szolgál az attribútum attribútum nevét.  Az Active Directory Ez az általában objectGUID.  Más LDAP szerezhet entryUUID vagy hasonló lehet.  Az alapértelmezett érték objectGUID. |
| ... (Válassza attribútum) gomb | Az attribútum mezők van (...) gomb mellett, amelyek a attribútum kiválasztása párbeszédpanel lehetővé teszi a listából kiválasztott attribútum jelennek meg. <br><br>Jelölje ki a attribútum párbeszédpanel.<br><br>Megjegyzés: A attribútumok manuálisan kell megadni, és nem szükséges, a megfelelő attribútum az attribútum listájában. |
| Egyedi azonosító típusa | Jelölje ki az egyedi azonosító attribútum típusú.  Az Active Directory a objectGUID attribútum nem típusú globálisan egyedi azonosítója.  Más LDAP szerezhet lehet ASCII bájt tömb vagy a karakterlánc típusú.  Az alapértelmezett érték globálisan egyedi azonosítója. <br><br>Megjegyzés: Fontos meg megfelelően az egyedi azonosító által hivatkozott szinkronizálás elemet, és az egyedi azonosító típusa a címtárban az objektum közvetlenül keresésére szolgál.  Beállítás a karakterlánc, amikor a címtár ténylegesen tárolja az értéket, mint ASCII-karakterek bájt tömbje megakadályozza, hogy megfelelően működik-e a szinkronizálási. |
| Megkülönböztető név | Adja meg a attribútumot tartalmazó rekordokat megkülönböztető név attribútum nevét.  Az Active Directory Ez az általában distinguishedName.  Más LDAP szerezhet entryDN vagy hasonló lehet.  Az alapértelmezett érték distinguishedName. <br><br>Megjegyzés: Ha csak a megkülönböztető név tartalmazó tulajdonság nem létezik, az adspath attribútum használható.  A "LDAP: / /<server>/" az elérési út része automatikusan hivatkozásból fog ki az objektumot csak a megkülönböztető név elhagyása. |
| Tároló neve | Adja meg az attribútum rekordban tárolóban nevét tartalmazó attribútum nevét.  Az érték az attribútum jelenik meg a tároló hierarchia importálásakor Active Directoryból és szinkronizálás elemek felvétele.  Az alapértelmezett érték nevét. <br><br>Megjegyzés: Ha eltérő tárolók különböző attribútumok a nevük, több tároló nevük is kell megadni pontosvesszővel elválasztva.  Az első tárolóban található tároló objektum neve attribútum használandó megjelenítése a nevét. |
| Biztonsági csoport neve | Adja meg a biztonsági csoport rekordban nevét tartalmazó attribútum attribútum nevét.  Az attribútum értékének jelenik meg a biztonsági csoport lista importálásakor Active Directoryból és szinkronizálás elemek felvétele.  Az alapértelmezett érték nevét. |
| Felhasználók | A következő attribútumok keres, megjelenítését, importálása és szinkronizálása a címtárból a felhasználói adatok használhatók. |
| Felhasználónév | Írja be a felhasználó rekordban felhasználónév tartalmazó attribútum attribútum nevét.  Többtényezős hitelesítés kiszolgáló felhasználónév attribútum értéke lesz.  A második attribútum az első másolatként adható meg.  A második attribútum csak használható, ha az első attribútum nem tartalmaz egy értéket a felhasználó számára.  Az alapértelmezett beállításokat, hogy userPrincipalName és sAMAccountName. |
| Utónév | Adja meg, amely tartalmazza az Utónév felhasználói rekordban a attribútum attribútum nevét.  Az alapértelmezett érték givenName. |
| Vezetéknév | Írja be a felhasználó rekordban a Vezetéknév tartalmazó attribútum attribútum nevét.  Az alapértelmezett érték funkcióra. |
| E-mail cím | Írja be a felhasználó rekordban e-mail címét tartalmazó attribútum attribútum nevét.  E-mail cím küldése Üdvözöljük, és frissítse az e-mailek a felhasználó használatával.  Az alapértelmezett érték mail. |
| Felhasználócsoport | Írja be a felhasználó rekordban a felhasználói csoportot tartalmazó attribútum attribútum nevét.  Felhasználócsoport szeretné szűrni a felhasználók a agent, majd a jelentések az adatkezelési portálon többtényezős Auth kiszolgáló használható. |
| Leírás | Írja be a felhasználó rekordban a leírását tartalmazó attribútum attribútum nevét.  Leírás való kereséshez használható.  Az alapértelmezett érték a leírást. |
| Hangposta hívása nyelvi | Adja meg a beszélgetésre, a felhasználó használni kívánt nyelvet a rövid nevét tartalmazó attribútum attribútum nevét. |
| Az SMS szöveg nyelvét | Adja meg a szöveg az SMS a felhasználó használni kívánt nyelvet a rövid nevét tartalmazó attribútum attribútum nevét. |
| Phone alkalmazás nyelve | Adja meg a Telefon alkalmazás SMS-EK a felhasználó a használni kívánt nyelvet a rövid nevét tartalmazó attribútum attribútum nevét. |
| ELFOGADHATÓ jogkivonat nyelvi | Adja meg a elfogadható jogkivonat SMS-EK a felhasználó a használni kívánt nyelvet a rövid nevét tartalmazó attribútum attribútum nevét. |
| Telefonok | A következő attribútumok importálása és szinkronizálása felhasználói telefonszámok szolgálnak.  Ha egy attribútumnév telefontípusnak nincs megadva, a telefon típus nem lesz elérhető mikor importálása az Active Directory vagy a szinkronizálás elemek hozzáadása. |
| Üzleti | Írja be a felhasználó rekordban a munkahelyi telefonszám tartalmazó attribútum attribútum nevét.  Az alapértelmezett érték telephoneNumber. |
| Kezdőlap | Adja meg a felhasználó rekordban otthoni telefonszámát tartalmazó attribútum attribútum nevét.  Az alapértelmezett érték Lakástelefon. |
| Személyhívó | Írja be a felhasználó rekordban a személyhívó számot tartalmazó attribútum attribútum nevét.  Az alapértelmezett érték személyhívó. |
| Mobil | Írja be a felhasználó rekordban a mobiltelefonszámát tartalmazó attribútum attribútum nevét.  Az alapértelmezett érték mobil. |
| Faxfedőlap | Írja be a felhasználó rekordban a faxszám tartalmazó attribútum attribútum nevét.  Az alapértelmezett érték facsimileTelephoneNumber. |
| IP-telefon | Írja be a felhasználó rekordban IP-telefon számát tartalmazó attribútum attribútum nevét.  Az alapértelmezett érték ipPhone. |
| Egyéni | Írja be az attribútum nevét, amely tartalmazza az egyéni telefonszám attribútum |
|  | egy felhasználó rekordot.  Az alapértelmezett érték üres. |
| Bővítmény | Adja meg, amely tartalmazza a mellékszáma felhasználói bejegyzés attribútum attribútum nevét.  A bővítmény mező értékét a bővítmény csak az elsődleges telefonszám lesz.  Az alapértelmezett érték üres. <br><br>Megjegyzés: Ha a bővítmény attribútum nincs megadva, bővítmények lehet a telefon attribútum része.  A bővítmény, "x" kell előtt, hogy elemezhető.  Ha például 555-123-4567 x890 eredményezne telefonszámként 555-123-4567 és a bővítményként 890. |
| Visszaállítása az alapértelmezett gomb | Kattintson az összes attribútum térjen vissza az alapértelmezett érték alapértékek visszaállítása gombra.  Az alapértelmezett beállítások az Active Directory vagy ADAM normál sémával megfelelően működik. |

Attribútumok szerkesztése, egyszerűen kattintson a Szerkesztés gombra a attribútumok lapon.  Ez a Windows, amely lehetővé teszi, hogy tulajdonságainak szerkesztése állapotba kerül.

![Attribútumok szerkesztése](./media/multi-factor-authentication-get-started-server-dirint/dirint4.png)

## <a name="synchronization"></a>A szinkronizálás
Szinkronizálás az Azure többtényezős felhasználói adatbázis szinkronizálja az Active Directory vagy egy másik Lightweight Directory Access Protocol (LDAP) Lightweight Directory Access Protocol directory a felhasználók továbbra is.  A folyamat hasonlít a felhasználók manuálisan importálása az Active Directory, de rendszeres lekérdezi az Active Directory-felhasználók és a biztonsági csoport módosítások feldolgozása.  Azt is biztosít letiltása vagy tároló vagy a biztonsági csoport eltávolítja, és a felhasználók eltávolítása a felhasználók eltávolítása törli az Active Directoryból.

A többtényezős hitelesítés ADSync szolgáltatás nem a Windows-szolgáltatás, amely az Active Directory ciklikus hajt végre.  Ez a nem tud összezavarodik Azure AD-szinkronizálás vagy Azure AD Connect.  a többtényezős hitelesítés ADSync bár hasonló kódolása épül az Azure többtényezős hitelesítést kiszolgálóra.  Azt leállítva állapotban telepítve van, és azonnal elindul, a többtényezős hitelesítés kiszolgáló által konfigurálásakor futtatásához.  Ha a több elem kiszolgáló többtényezős hitelesítés kiszolgáló konfiguráció, a többtényezős hitelesítés ADSync csak futtathatnak egy kiszolgálón.

A többtényezős hitelesítés ADSync szolgáltatás módosítások hatékony lekérdezik az DirSync LDAP-bővítmény a Microsoft által nyújtott használja.  A DirSync vezérlő hívó kell az "első módosítások címtár" jobbra és DS-replikáció-Get-módosítások bővített hozzáférés szabályozása jobbra.  Mindezen jogok alapértelmezés szerint a tartomány vezérlők rendszergazdája és a helyi rendszerfiók fiókok vannak rendelve.  A többtényezős hitelesítés AdSync szolgáltatás alapértelmezés szerint helyi rendszerfiók alatt futtatásához van beállítva.  Ezért a legegyszerűbb futtassa a szolgáltatást a tartományvezérlőnek a.  A szolgáltatás futtatását is lehetővé teszi-fiókként kisebb engedélyekkel rendelkező Ha úgy állítja be, hogy mindig a teljes szinkronizálást.  Kevésbé hatékony, de kevesebb fiók jogosultság szükséges.

Ha használatára konfigurált LDAP és az LDAP-címtár támogatja a DirSync vezérlő, majd a felhasználó és a biztonsági csoport módosítások lekérdezési működnek ugyanúgy, ahogyan azt az Active Directory.  Ha az LDAP-címtár nem támogatja a DirSync vezérlőt, majd teljes szinkronizálást fog történni minden ciklusban.

![A szinkronizálás](./media/multi-factor-authentication-get-started-server-dirint/dirint5.png)

Használja az alábbi táblázat az egyes beállításokat a szinkronizálás lapon minden egyes további információt.

| A szolgáltatás | Leírás |
| ------- | ----------- |
| Az Active Directory-szinkronizálás engedélyezése | Ha be van jelölve, a többtényezős hitelesítés kiszolgáló szolgáltatás rendszeres lekérdezik az Active Directory módosítások fog elindulni. <br><br>Megjegyzés: legalább egy szinkronizálási elem hozzá kell adnia, és a szinkronizálás most kell végrehajtani, mielőtt a többtényezős hitelesítés szolgáltatás indul a módosítások feldolgozása. |
| Szinkronizálása minden | Adja meg a időintervallum lekérdezési és a módosítások feldolgozása között vár a többtényezős hitelesítés Server szolgáltatás. <br><br> Megjegyzés: A megadott időközönként az egyes ciklus elejére között eltelt idő.  A módosítások feldolgozása idő meghaladja az intervallumra, ha a szolgáltatás nyomban újrakezdődik lekérdezik. |
| Felhasználók eltávolítása már nem az Active Directoryban | Ha be van jelölve, a többtényezős hitelesítés kiszolgáló szolgáltatás törölték az Active Directory felhasználó kijelölése törlésre folyamat, és távolítsa el a kapcsolódó többtényezős hitelesítés kiszolgáló felhasználót. |
| A teljes szinkronizálást mindig | Ha be van jelölve, a többtényezős hitelesítés kiszolgáló szolgáltatás mindig hajtsa végre a teljes szinkronizálást.  Ha nincs kijelölve, a többtényezős hitelesítés kiszolgáló szolgáltatás végez növekményes szinkronizálás újdonsággal felhasználók csak lekérdezésével.  Alapértelmezés szerint nincs bejelölve. <br><br> Megjegyzés: Ha nincs bejelölve, növekményes szinkronizálás csak hajtható végre, ha a címtárban támogatja a DirSync vezérlő a kötést létrehozni a címtárhoz használt fiók DirSync növekményes lekérdezések hajtsa végre a megfelelő engedélyeket.  Ha a fiókja nem rendelkezik a megfelelő engedélyekkel, vagy több tartományt a szinkronizálás játszik szerepet, végezze el a teljes szinkronizálást ajánlott. |
| Rendszergazdai jóváhagyás, ha több mint X felhasználók lesz tiltva vagy eltávolítja megkövetelése | Szinkronizálás elemek beállítható úgy, hogy a felhasználók, akik már nem tagja az elemet tároló vagy biztonsági csoport törlése vagy letiltása.  Védelmet rendszergazdai jóváhagyás lehet szükség esetén a felhasználók letiltása és eltávolítása száma meghaladja a küszöbértéket.  Ha be van jelölve, a jóváhagyási megadott küszöbértékét szükséges lesz.  Az alapértelmezett érték 5 és a tartománya 1 és 999. <br><br> Jóváhagyás megkönnyítheti első küldése e-mailben értesítést a rendszergazdák számára. Az értesítő e-mailt az itt leírt lépésekkel megtekintésével és jóváhagyásáért letiltása és eltávolítása a felhasználók adja vissza.  Többtényezős hitelesítés kiszolgáló kezelőfelület indításakor jóváhagyásra kéri. |

A **Szinkronizálás most** gombra a szinkronizálási elemek megadott teljes szinkronizálást futtatása teszi lehetővé.  A teljes szinkronizálás szükség, szinkronizálási elemek vannak hozzáadott, módosított, eltávolítani vagy rendelni.  Azt is előtt szükség a többtényezős hitelesítés AdSync szolgáltatás működési lesz, mivel a kezdőpont, ahonnan a szolgáltatás növekményes módosítások fog lekérdezik állítja.  Ha szinkronizálási elemek elvégzett módosításokat, és nem lett végrehajtva teljes szinkronizálást, kéri a szinkronizálás most való navigáció közben a másik szakaszra, és a felhasználói felület bezárásakor.

Az **Eltávolítás** gomb lehetővé teszi, hogy a rendszergazdát, hogy egy vagy több szinkronizálási elem törlése a többtényezős hitelesítés Server szinkronizálási elem listájából.

>[AZURE.WARNING]Szinkronizálás elem rekord eltávolítása után nem állíthatók helyre. Szüksége lesz, vegye fel újból a szinkronizálás elemet rekord ha véletlenül töröl.

A szinkronizálás elemre vagy a szinkronizálás elemek el lett távolítva többtényezős hitelesítés kiszolgáló.  A többtényezős hitelesítés kiszolgáló szolgáltatás már nem dolgozza fel a szinkronizálás elemet.

A fel és le gomb lehetővé teszi a rendszergazdát, hogy a szinkronizálás elemek sorrendjének módosítása.  A sorrend fontos, mivel lehet, hogy az adott felhasználó egynél több szinkronizálás elemet (például a tároló és biztonsági csoport) tagja.  A szinkronizálás során a felhasználó alkalmazott beállításokat az első szinkronizálás elem a listában, amelyhez a felhasználó nem lesz származik.  Emiatt a szinkronizálás elemek elsőbbségi sorrendben kell elhelyezni.

>[AZURE.TIP]Szinkronizálás elemek eltávolítása után teljes szinkronizálást kell elvégezni.  Szinkronizálás elemek rendezés után teljes szinkronizálást kell elvégezni.  Kattintson a szinkronizálás most gombra a teljes szinkronizálást.

## <a name="multi-factor-auth-servers"></a>Többtényezős hitelesítés kiszolgálók
További többtényezős hitelesítés kiszolgálók biztonsági RADIUS-proxy, LDAP-proxy, vagy a hitelesítést az IIS szolgáló kell beállítása. A szinkronizálási beállítások között a ügynökök lesz megosztva. Azonban csak az egyik ügynökök lehetnek a többtényezős hitelesítés Server szolgáltatást. Ezen a lapon jelölje ki a többtényezős hitelesítés kiszolgálót, amelyet a szinkronizáláshoz engedélyezni teszi lehetővé.

![Többfázisú multi-factor Auth kiszolgálók](./media/multi-factor-authentication-get-started-server-dirint/dirint6.png)

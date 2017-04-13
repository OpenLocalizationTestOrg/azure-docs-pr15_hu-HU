<properties
    pageTitle="Azure Active Directory hibrid identitás tervezési szempontjait - megadása egy hibrid identitás átálláshoz |} Microsoft Azure"
    description="Feltételes hozzáférés-vezérlés az Azure Active Directory ellenőrzi az adott feltételeknek választhat, amikor a felhasználó hitelesítése és előtt, hogy az alkalmazás hozzáférést. Ha ezen feltételek teljesülése esetén a felhasználó a hitelesített és az alkalmazás férhet hozzá."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="define-a-hybrid-identity-adoption-strategy"></a>A hibrid identitás átálláshoz meghatározása

Ebben a feladatban lesz a hibrid identitás megoldás a tárgyalt üzleti követelményeknek hibrid identitás átálláshoz definiálása:

- [Az üzleti igények meghatározása](active-directory-hybrid-identity-design-considerations-business-needs.md)
- [Címtár-szinkronizálás követelmények meghatározására](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)
- [Többtényezős hitelesítés követelmények meghatározása](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="define-business-needs-strategy"></a>Üzleti igények stratégia meghatározása
Annak megállapítása, a szervezetek üzleti első tevékenység címeit van szüksége.  Ez nagyon nagy vonalakban lehet, és hatóköre kötés akkor fordulhat elő, ha nem óvatos.  A kezdete legyen egyszerű, de ne felejtsen, amely beleférjenek és módosítása a jövőben megkönnyítése látványterv megtervezése.  Függetlenül attól, hogy egy egyszerű látványterv vagy egy rendkívül összetett használ az Azure Active Directory a Microsoft Identity platform, amely támogatja az Office 365-ben, a Microsoft Online Services és a felhő tartsa szem előtt alkalmazásokat.

## <a name="define-an-integration-strategy"></a>Egy integrációs stratégia meghatározása
A Microsoft helyzeteket három fő integrációs felhő identitások, szinkronizált identitások és összevont identitások.  Meg kell terveznie, a fogadjanak el az alábbi integrálási stratégiák közül.  A kiválasztott eltérőek lehetnek, és a kiválaszt egyet a döntéseket tartalmazhatnak, milyen típusú szeretne megadni, van néhány a meglévő infrastruktúra már helyi, és mi az a költség leghatékonyabb felhasználói felület stratégia.  
 
![](./media/hybrid-id-design-considerations/integration-scenarios.png)

Az jelenik meg a fenti ábrán definiált a következők:

- **Felhőalapú Identitások**: ezek a identitások megtalálható kizárólag a felhőben.  Azure AD, ha azok volna tárolnak kifejezetten az Azure Active directory.
- **Synchronized**: ezek a identitások megtalálható a helyszíni és felhőbeli.  Azure AD Connect használja, ezeket a felhasználókat vagy létrehozott vagy meglévő Azure AD-fiókokkal csatlakozott.  A felhasználó jelszavát kivonat a jelszó kivonat úgynevezett a felhőbe szinkronizálja a helyszíni környezet.  Szinkronizált használatával az egyik bővítve esetén, hogy a felhasználó le van tiltva a helyszíni környezetben, ha óráig is eltarthat legfeljebb 3 adott fiók állapotának megjeleníthető az Azure Active Directory számára.  Ennek oka, hogy a szinkronizálás időszakot.
- **Külső**: ezek identitások létezik, mind a helyszíni és felhőbeli.  Azure AD Connect használja, ezeket a felhasználókat vagy létrehozott vagy meglévő Azure AD-fiókokkal csatlakozott.  
 
>[AZURE.NOTE]
További információt a szinkronizálási beállítások további [integrálása az Azure Active Directory címtárral a helyszíni identitások](active-directory-aadconnect.md).

Az alábbi táblázat segítséget nyújt meghatározásában előnyei és hátrányai az egyes az alábbi szempontokat:

| Stratégia         | Előnyei                                                                                                                                                                                                                                                  | Hátrányai                                                                                                                                                                                                                                                                                                                                                  |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Felhőalapú Identitások** | Könnyebben kezelhető kis szervezete számára. <br> Semmi sem helyileg nincs szükség további hardver telepítéséhez<br>Könnyen érhető el, ha a felhasználó elhagyja a vállalat                                                                                                   | Felhasználók kell bejelentkezés elérése a felhőben munkaterhelésekből közben <br> Előfordulhat, hogy jelszavakat, és nem lehet azonos a felhőben, és a helyszíni identitások                                                                                                                                                                                                                     |
| **Szinkronizált**     | A helyszíni jelszó fog mindkét helyszíni hitelesítő és a felhő könyvtárak <br>Könnyebben kezelhető kicsi, közepes vagy nagy méretű szervezetek számára <br>Felhasználók is telepítve van az egyszeri bejelentkezés (SSO) források <br> A Microsoft elsődleges módszer-szinkronizálás <br> Könnyebben kezelhető | Lehet, hogy egyes ügyfelek szívesen a könyvtárak szinkronizálni a felhőben adott vállalat rendőrségi határidő                                                                                                                                                                                                                                                  |
| **Összevont**        | Felhasználók egyszeri bejelentkezés (SSO) is van. <br>Ha egy felhasználó megszakad, vagy hagyja, a fiók azonnal is tiltható és visszavonva access,<br> Támogatja a speciális esetek, amelyek nem lehet, segítségével megy végbe szinkronizálása                                           | További lépések beállítása <br> Nagyobb karbantartása <br> További hardver kérhet STS infrastruktúrát <br> Előfordulhat, hogy szükség további hardver az összevonási kiszolgáló telepítéséhez. Külön szoftver szükség, ha használja az Active Directory összevonási szolgáltatások <br> Egyszeri bejelentkezés teljes körű beállítási megkövetelése <br> Kritikus pontok sikertelen, ha az összevonási kiszolgáló nem működik a felhasználók nem fogják tudni hitelesítést végezni |

### <a name="client-experience"></a>Ügyféloldali felhasználói élmény
Az Ön által használt stratégia szabja a felhasználónak bejelentkezés módja.  Az alábbi táblázatokban nyújtanak információkat a felhasználók milyen lesz a bejelentkezés módja kell számítania.  Figyelje meg, hogy nem az összes szövetséges Identitásszolgáltatók minden esetben SSO támogatja.

**A tartomány csatlakozott és magánhálózati alkalmazások**:
 

|                              | Szinkronizált azonosító      | Összevont identitás                                           |
|------------------------------|----------------------------|--------------------------------------------------------------|
| Webböngésző                 | Űrlapalapú hitelesítés | egyszeri bejelentkezési néha, adja meg a szervezet azonosítója |
| Az Outlook                      | Hitelesítő adatok kérése     | Hitelesítő adatok kérése                                       |
| A Skype vállalati verzió (Lync)    | Hitelesítő adatok kérése     | egyszeri bejelentkezés a Lync, hitelesítő adatokat kér Exchange   |
| A SkyDrive Pro                 | Hitelesítő adatok kérése     | egyszeri bejelentkezés                                               |
| Az Office Pro Plus előfizetés | Hitelesítő adatok kérése     | egyszeri bejelentkezés                                               |

**Külső vagy a nem megbízható forrásból**:

|                              | Szinkronizált azonosító      | Összevont identitás                                           |
|------------------------------|----------------------------|--------------------------------------------------------------|
| Webböngésző                 | Űrlapalapú hitelesítés |  Űrlapalapú hitelesítés |
| Az Outlook, a Skype vállalati verzió (Lync), a Skydrive Pro, Office-előfizetés| Hitelesítő adatok kérése     | Hitelesítő adatok kérése                                       |
| Az Exchange ActiveSync    | Hitelesítő adatok kérése     | egyszeri bejelentkezés a Lync, hitelesítő adatokat kér Exchange   |
| Mobilalkalmazások                 | Hitelesítő adatok kérése     | Hitelesítő adatok kérése                                               |

Ha úgy határozott, hogy rendelkezik-e egy segítségével adja meg az Azure Active Directory összevonási IdP vagy méretű Ugrás fél 3rd 1 tevékenységből, kell lennie, vegye figyelembe az alábbi támogatott funkciók:
- Bármely az SP Lite profil megfelelő SAML 2.0-szolgáltató támogatja a Azure AD-hitelesítés és kapcsolódó alkalmazások
- Támogatja a passzív hitelesítést, ami megkönnyíti a auth az Outlook Web App és a SharePoint online, a stb.
- Az Exchange Online-ügyfélprogramok használható keresztül a SAML 2.0-s fokozott ügyfél profil (ECP)

Meg kell is érdemes szem előtt tartania, hogy mely funkciók nem lesznek elérhetők:

- Webszolgáltatás-adatvédelmi/összevonás-támogatás nélkül aktív ügyfelek megszakad
 - Ez azt jelenti, nem Lync-ügyfél, a OneDrive-ügyfél, Office-előfizetés, az Office Mobile Office 2016-ban előtt
- Áttérés az Office passzív hitelesítést lehetővé teszi a tiszta IdPs a SAML 2.0 támogatási, de továbbra is a ügyfél-ügyfél alapon támogatás


>[AZURE.NOTE]
A legfrissebb lista olvassa el a cikk http://aka.ms/ssoproviders.

## <a name="define-synchronization-strategy"></a>Szinkronizálás stratégia meghatározása
Az eszközök szinkronizálásához használt kell megadni ebben a feladatban a szervezet helyszíni adatok a felhőben, és mit kell használnia a topológiában.  Mivel a legtöbb szervezet Active Directory használatához használatáról az Azure AD Connect megoldása a fenti kérdések információ néhány részletesen.  Nem rendelkezik az Active Directory környezetek van használatáról FIM 2010 R2 vagy MIM 2016 segíti a vonatkozó stratégia megtervezése.  Azonban az Azure AD Connect későbbi kiadásokban támogatja az LDAP könyvtárak, így függően az ütemtervre, előfordulhat, hogy tud segítséget nyújt az információk.

###<a name="synchronization-tools"></a>Szinkronizálás eszközök
Az évek során több szinkronizálási eszközök létezik és különböző forgatókönyvekben használható.  Azure AD Connect jelenleg a nyissa meg az összes támogatott forgatókönyvek tetszés eszköz.  AAD Sync DirSync továbbra is körül és is lehet a környezetben bemutató gombra. 

>[AZURE.NOTE]
Egyes eszközök legfrissebb információkat a támogatott funkciók kapcsolatban olvassa el a [címtár-integrációs eszközök összehasonlító](active-directory-hybrid-identity-design-considerations-tools-comparison.md) cikk.  

### <a name="supported-topologies"></a>Támogatott topológiák
A szinkronizálás stratégia megadásakor a topológia használt kell meghatározni. Attól függően, hogy az információkat, amelyek lépésben határozták 2 megállapítható amelyekre topológiát lesz a megfelelő használni. Egyetlen erdőn Azure Active Directory topológia a leggyakoribb, és az Active Directory erdőn és Azure Active Directory egyetlen példánya áll.  Ez egy az esetek többségében használandó fog és a várt topológia Azure Active Directory csatlakozás Express telepítésével használatakor, az alábbi ábrán látható módon.
 
![](./media/hybrid-id-design-considerations/single-forest.png)Egyetlen erdő esetben célszerű gyakori, a nagy- és még szervezetek több erdők, hogy 5 ábrán látható módon.

>[AZURE.NOTE]
További információt a különféle helyszíni és Azure Active Directory topológiák az Azure AD Connect szinkronizálási olvassa el a cikk [az Azure AD Connect topológiában](active-directory-aadconnect-topologies.md).


![](./media/hybrid-id-design-considerations/multi-forest.png) 

Több elem erdő eset

Ha ez az esetet, majd a csoportos-forest-egyetlen Azure Active Directory topológia tekintendő teljesülésének az alábbiakat:

- A felhasználók csak 1 identitás rendelkeznek az összes erdők – az egyedi módon azonosító felhasználók az alábbi Csatornakezelési ez részletesebben.
- A felhasználó hitelesíti a személyazonosságot a erdőt
- Ebben a fog érkezni UPN és a forrás horgony (megváltoztatható azonosító)
- Az összes erdők érhetők el Azure AD Connect – Ez azt jelenti, hogy nem kell lennie tartomány csatlakozott, és ha ez elősegíti Ez egy DMZ elhelyezhető.
- A felhasználók csak egy postafiókot rendelkeznek
- A felhasználó postaládáját tároló erdő rendelkezik a legjobb adatok minőségének attribútumok látható az Exchange globális cím lista (globális Címlistában a)
- Ha nincs postaláda a felhasználó, majd bármely erdőjének használható ezeket az értékeket közreműködés
- Ha egy csatolt postaládához, majd található is másik fiókba való bejelentkezéshez használt eltérő erdőben.

>[AZURE.NOTE]
Olyan objektumok, amelyek mindkét a helyszíni és felhőbeli létezik "csatlakoztatott" keresztül egy egyedi azonosítót. A címtár-szinkronizálás keretében ez az egyedi azonosító nevezik a SourceAnchor. Az egyszeri bejelentkezés a környezetben ezt nevezzük a ImmutableId. [Azure AD Connect tervezés elképzeléseket](active-directory-aadconnect-design-concepts.md#sourceanchor) SourceAnchor használatával kapcsolatos további szempontokat.

Ha a fenti nem igaz és több aktív fiókot vagy több postaláda, Azure AD Connect válasszon egyet, és figyelmen kívül hagyja a másik.  Kapcsolta postaládák, de nincs más fiókkal, ha az ezekben a fiókokban nem exportálja az Azure Active Directory, és a felhasználó nem lesz csoportokat tagja.  Ez különbözik hogyan a Dirsync használatakor múltbeli volt, és azt szándékos jobb támogatási több erdőre forgatókönyvekben. Több elem erdő példa az alábbi ábrán látható.
 
![](./media/hybrid-id-design-considerations/multiforest-multipleAzureAD.png) 
 
**Több elem erdő több Azure AD-használatával**

Támogatja, de azt ajánljuk, hogy csak egy könyvtárból az Azure Active Directory-szervezet 1:1 kapcsolat folyamatosan az Azure AD Connect szinkronizálási-kiszolgáló és az Azure Active directory között.  Azure Active Directory minden példányában szüksége lesz az Azure AD Connect telepített.  Is Azure Active Directory, szándékosan legyen elkülönítve, és egy példánya az Azure Active Directory-felhasználók nem tudnak a felhasználók egy másik példányában című cikkben.

Lehetőség de támogatott egy példányát a helyszíni Active Directory csatlakozhat több Azure AD-könyvtárak, az alábbi ábrán látható módon:

![](./media/hybrid-id-design-considerations/single-forest-flitering.png) 
 

**Egyetlen erdőn szűrési eset**

Az alábbi művelet kell lennie igaz:

- Azure AD Connect szinkronizálási kiszolgálók kell beállítania, hogy egyes objektumok egymást kölcsönösen kizáróak csoportja szűrés.  Ki végzi, például egy adott tartomány vagy szervezeti minden kiszolgáló hatókör meghatározása.
- A DNS-tartomány csak egyetlen regisztrálását Azure Active directory, a UPN AD a felhasználók a helyszíni külön névtér kell használnia
- Azure AD egy példányát a felhasználók csak akkor láthatja a példány felhasználóit.  Nem lesznek láthatók, egyéb esetben a felhasználók
- Csak az Azure Active Directory könyvtárak közül engedélyezheti, hogy a helyszíni és hibrid Exchange Active Directory
- Kölcsönös kizárólagosság írási-vissza is érinti.  Emiatt nem támogatott a topológia, mivel ezek feltételezik, hogy a helyszíni egyetlen konfiguráció írási-vissza funkciókkal.  Ide tartoznak:
 - Csoport írási vissza az alapértelmezett beállítás
 - Eszköz írási-vissza


Tartsa szem előtt, hogy a következő nem támogatott, és nem kell választani, egy végrehajtása:

- Kapcsolódás az azonos Azure AD-címtár, akkor is, ha az objektum egymást kölcsönösen kizáró beállítása a szinkronizálandó konfigurálhatók több Azure AD Connect szinkronizálási kiszolgáló nem támogatott
- Ugyanaz a felhasználó több Azure AD-mappák szinkronizálása nem támogatja. 
- Azt is, hogy a konfiguráció módosítása, hogy a felhasználók egy Azure AD a névjegyek a másik Azure AD-címtárban jelennek meg a nem támogatott. 
- Érdemes emellett Azure AD Connect szinkronizálási több Azure AD-könyvtárak csatlakozhat módosítása nem támogatott.
- Azure Active Directory könyvtárak elszigetelt tervezés szerint is. Még nem támogatott adatainak beolvasása egy másik Azure Active directory közös és egységesített globális Címlistában a mappák között létrehozása az Azure AD Connect szinkronizálási beállításainak módosítása. Azt is nem támogatja a partnerek a felhasználók exportálása egy másik a helyszíni Active Directory sync Azure AD Connect használatával.


>[AZURE.NOTE]
Ha a szervezet korlátozza a számítógépek, a hálózaton csatlakozzon az internethez, ez a cikk felsorolja a végpontok (teljes tartományneve, IPv4 és az IPv6-címtartományokat), hogy meg kell-e foglalni a kimenő listák és engedélyezése az Internet Explorer Megbízható helyek zónában ügyfél annak érdekében, hogy a számítógép az Office 365 sikeresen használata számítógépeken. További információt [az Office 365 URL-EK és IP-címtartományok](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US)olvasni.

## <a name="define-multi-factor-authentication-strategy"></a>Többtényezős hitelesítés stratégia meghatározása
Ebben a feladatban határozza meg a többtényezős hitelesítés stratégiát használatához.  Azure többtényezős hitelesítés két különböző verzióiban megtalálható.  Egyik egy felhőalapú, a másik a helyszíni alapú Azure MFA-kiszolgálóval.  A kiértékelés alapján did fölött, megállapítható melyik megoldás felel meg a megfelelő fiókot a stratégia.  Az alábbi táblázat segítségével határozza meg, melyik legjobb Tervező lehetőséget a vállalat biztonsági követelménye teljesíteni:

Többtényezős látványtervezési lehetőségről:

| A digitális eszköz kiválasztása biztonságossá tétele                                               | MFA a felhőben | MFA helyszíni: |
|---------------------------------------------------------------|------------------|----------------|
| A Microsoft-alkalmazások                                                | igen              | igen            |
| Az alkalmazás gyűjteményben szoftver-alkalmazások                                  | igen              | igen            |
| Azure Active Directory-alkalmazás proxyn keresztül közzétett IIS-alkalmazások         | igen              | igen            |
| Az Azure Active Directory-alkalmazás proxyn keresztül nem közzétett IIS-alkalmazások | nem               | igen            |
| Távoli hozzáférés VPN, RDG szerint                                     | nem               | igen            |

Annak ellenére, hogy akkor is rendelkezik rendezni a probléma megoldásán a stratégia, még akkor használja a fentiektől kiértékelésétől a hol találhatók a felhasználók.  Ez a megoldást módosítása jelenhet meg.  Használja az alábbi táblázat, amely segít annak megállapítása, hogy ez:

| Felhasználók helye                                                       | Elsődleges Tervező lehetőséget.                 |
|---------------------------------------------------------------------|-----------------------------------------|
| Azure Active Directory                                              | Több-FactorAuthentication a felhőben |
| Azure Active Directory és a helyszíni Active Directory összevonási használata az Active Directory összevonási szolgáltatások             | Mindkét                                    |
| Azure Active Directory és a helyszíni Active Directory használatával az Azure Active Directory csatlakozás nem jelszó-szinkronizálás | Mindkét                                    |
| Azure Active Directory és a helyszíni Azure AD Connect használata jelszó-szinkronizálás  | Mindkét                                    |
| A helyszíni Active Directory                                                      | Többtényezős hitelesítés kiszolgáló      |

>[AZURE.NOTE]
Biztosítania kell, hogy a kijelölt többtényezős hitelesítés Tervező lehetőséget támogatja-e a szolgáltatások, a tervezés szükséges.  További információt olvassa el a [Választás a többtényezős biztonsági megoldás](../multi-factor-authentication-get-started.md#what-am-i-trying-to-secure).

## <a name="multi-factor-auth-provider"></a>Többtényezős hitelesítés szolgáltató
Többtényezős hitelesítés globális rendszergazdái Azure Active Directory-bérlő alapértelmezés szerint érhető el. Azonban szeretne többtényezős hitelesítés kiterjesztése a felhasználóit és/vagy a globális rendszergazdák szeretné engedélyezni szeretné, hogy kihasználhassam szolgáltatások, például az adatkezelési portál, egyéni Üdvözlések és jelentéseket készíthet, ha ezután kell vásárolnia és többtényezős hitelesítést szolgáltató beállítása.

>[AZURE.NOTE]
Biztosítania kell, hogy a kijelölt többtényezős hitelesítés Tervező lehetőséget támogatja-e a szolgáltatások, a tervezés szükséges. 

##<a name="next-steps"></a>Következő lépések
[Határozza meg az adatok védelmére vonatkozó követelmények](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)

## <a name="see-also"></a>Lásd még:
[Tervezési szempontjait – áttekintés](active-directory-hybrid-identity-design-considerations-overview.md)

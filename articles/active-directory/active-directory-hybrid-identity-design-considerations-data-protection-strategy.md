<properties
    pageTitle="Azure Active Directory hibrid identitás tervezési szempontjait - adatok védelme stratégia definiálása |} Microsoft Azure"
    description="Az adatok védelme stratégia a hibrid identitás megoldás megadott üzleti követelményeknek fogja határozza meg."
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


# <a name="define-data-protection-strategy-for-your-hybrid-identity-solution"></a>A hibrid identitás megoldás adatok védelme stratégia meghatározása

Ebben a feladatban lesz a hibrid identitás megoldás az üzleti megfeleljen a megadott adatok védelme stratégia definiálása:

- [Határozza meg az adatok védelmére vonatkozó követelmények](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)
- [Tartalomkezelés követelmények meghatározása](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)
- [Az access vezérlő követelmények meghatározása](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)
- [Az esemény válasz követelmények meghatározása](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="define-data-protection-options"></a>Adatok védelem beállításainak megadása
Ez volt című cikkben ismertetett módon [meghatározása címtár](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)-szinkronizálás követelmények, a Microsoft Azure Active Directory szinkronizálhatja az Active Directory tartományi szolgáltatások (AD DS) található, a helyszíni. Segítségével a szervezetek Azure Active Directory ellenőrizze a felhasználó hitelesítő adatait, amikor megpróbálnak hozzáférni a vállalati erőforrások használhatja. A két környezetben erre: a többi a helyszíni és felhőbeli adatokat.  Az Azure Active Directory adatokhoz való hozzáférés szükséges a felhasználói hitelesítés (STS) biztonsági jogkivonat szolgáltatáson keresztül.

Miután a hitelesítése, az egyszerű felhasználónév (UDN) van olvassa el a hitelesítési jogkivonat, és a replikált partíciót és a felhasználó tartományát megfelelő tároló határozza meg. Információ a felhasználó jelenléte, engedélyezett állapot és szerepkör határozza meg, hogy a célhely bérlőjén eléréséhez a kért jogosult-e a felhasználó ebben a munkamenetben az engedélyezési rendszer használja. Bizonyos engedélyezett műveletek (a felhasználó, a jelszó-visszaállítás pontosabban létrehozása) létrehozása a megfelelőségi erőfeszítéseket vagy nyomozásokban kezelése a bérlő rendszergazdájának használható könyvvizsgálati.

A helyszíni adatközponthoz Azure tárolóba Internet-kapcsolaton keresztül adatok áthelyezése nem mindig lehet valósítható adatok kötet, sávszélesség áll rendelkezésre vagy egyéb szempontok miatt. Az [Importálás/Exportálás Tárhelyszolgáltatáshoz Azure](../storage/storage-import-export-service.md) egy hardver-alapú beállítás, a nagy mennyiségű adattal blob-tárolóban lévő úgy, hogy a/retrieving biztosít. Lehetővé teszi a merevlemez-meghajtók [BitLocker titkosított](https://technet.microsoft.com/library/dn306081#BKMK_BL2012R2) közvetlenül az Azure adatközponthoz, amelyen felhő operátorok feltöltődnek tartalmát tároló fiókjához, vagy azok az Azure adatok letöltheti a meghajtókon való visszatéréshez, küldje el. Csak a titkosított lemezen elfogadott a folyamat (kulccsal BitLocker szolgáltatás magát a feladatot a telepítés során létre). A BitLocker kulcsot kapja meg Azure külön-külön, így biztosítva sáv kulcs megosztása ki.

Mivel a hálózaton átvitt adatok különböző forgatókönyvekben sor kerülhet, található is fontos tudni, hogy a Microsoft Azure [virtuális hálózat](https://azure.microsoft.com/documentation/services/virtual-network/) használja elkülönítése bérlők forgalom egymástól, például a host és vendég szintű tűzfalak, IP-csomagok szűrését, port blokkolása és HTTPS-végpont mértékek alkalmazó. Azure a belső kommunikáció infrastruktúra-infrastruktúrát és infrastruktúra-ügyfél (helyszíni), beleértve a legtöbb azonban is titkosítva. Egy másik fontosságát az Azure adatközpontokkal; belüli kommunikáció A Microsoft kezeli a hálózatok biztosítása érdekében, hogy nincs virtuális megszemélyesítés, vagy kihallgassa a másikra IP-címét. Az SSL/TLS használatos Azure tároló vagy SQL-adatbázisait elérésekor, vagy ha a Felhőbeli szolgáltatásokhoz csatlakozzon. Ebben az esetben a vevő rendszergazda felelős a TLS/SSL-tanúsítvány beszerzése és üzembe helyezése a bérlői infrastruktúra. Adatok forgalom áthelyezése a azonos példányban virtuális gépeken futó vagy a Microsoft Azure virtuális hálózaton keresztül egyetlen környezetben bérlők között védhetők titkosított kapcsolati protokollok, például a HTTPS-, az SSL/TLS lehetőséget, vagy mások keresztül.

Attól függően, hogy hogyan érkezett válasz [meghatározása adatok védelmére vonatkozó követelmények](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)kérdéseket látnia kell megállapíthatja, hogyan szeretné az adatok védelme és hogyan a hibrid identitás megoldást nyújt segítséget, amely a. A táblázat mutatja az Azure által támogatott használható beállításokat egyes adatok védelme forgatókönyvet.


| Adatok védelme beállításai         | A többi a felhőben | A többi a helyszíni | A hálózaton átvitt |
|---------------------------------|----------------------|---------------------|------------|
| A BitLocker titkosítás:      | X                    | X                   |            |
| SQL Server-adatbázisok titkosítása | X                    | X                   |            |
| Virtuális virtuális gép titkosítás:             |                      |                     | X          |
| AZ SSL/TLS                         |                      |                     | X          |
| VIRTUÁLIS MAGÁNHÁLÓZATI                             |                      |                     | X          |


>[AZURE.NOTE]
Olvassa el a [szolgáltatás által megfelelőségi](https://azure.microsoft.com/support/trust-center/services/) kattintson [A Microsoft Azure az Adatvédelmi központ](https://azure.microsoft.com/support/trust-center/) , további tudnivalók a tanúsítványok, amely megfelel az egyes Azure szolgáltatás.
Az adatvédelmi beállítások a többrétegű megközelítés használja, mivel ezek a lehetőségek összehasonlítása nem használhatók ehhez a feladathoz. Győződjön meg arról, hogy az egyes állapotok, amely az adatok lesz elérhető összes beállítások feljebb helyezése.

## <a name="define-content-management-options"></a>Tartalomkezelés beállítások megadása
Egy használatának Azure AD egy hibrid identitás kezelését előnye, hogy a folyamat nem teljesen átlátszó a végfelhasználó perspektívából. A felhasználó megpróbál egy megosztott erőforrás eléréséhez, az erőforrás-hitelesítést igényel, a felhasználónak kell hitelesítési kérelem küld szerezze be a token és hozzáférni az erőforrás Azure AD. Ez a teljes folyamat történik a háttérben felhasználói beavatkozás nélkül. Akkor is engedélyt szeretne adni egy [csoport](active-directory-manage-groups.md#getting-started-with-access-management) annak érdekében, hogy az egyes általános műveletek végrehajtása teszi lehetővé.

Azoknak a szervezeteknek, adatok adatvédelmi aggodalomra rendszerint adatok besorolás megkövetelése a megoldást. Ha a jelenlegi helyszíni infrastruktúra már használja a adatok besorolás, akkor lehet értekezletállapota megjelenjen a felhasználói azonosító, a fő tárházba Azure AD. A Windows Server 2012 R2, hogy az használt a helyszíni adatok osztályozási közös eszköz meghívott [Adatok besorolás eszközkészlet](https://msdn.microsoft.com/library/Hh204743.aspx) . Ez az eszköz azonosítása, osztályozásához és a személyes felhőben fájl kiszolgálókon adatok védelme biztonsági segítséget. Ajánlatos is kihasználhatja a Windows Server 2012 ehhez [Automatikus fájl osztályozás](https://technet.microsoft.com/library/hh831672.aspx) .

Ha a szervezet adatok besorolás nem már a helyükön, de kell bizalmas fájlok védelmére új kiszolgálók helyszíni felvétele nélkül, a Microsoft [Azure tartalomvédelmi szolgáltatás](https://technet.microsoft.com/library/JJ585026.aspx)segítségével.  Azure Rights Management szolgáltatást használ titkosítási identitás és engedélyezési házirendek biztonságossá a fájlokat és a levelezéshez, és keresztül különféle eszközökön működik – telefonon, táblagépen és PC-re. Mivel az Azure Rights Management szolgáltatást egy felhőalapú szolgáltatásba, nincs szükség az kifejezetten konfigurálása meghatalmazások más szervezetek, mielőtt védett tartalmat megoszthatja őket. Ha már rendelkeznek az Office 365 vagy az Azure Active directory, szervezetek különböző együttműködési automatikusan támogatott. Csak az Azure Rights Management szolgáltatást kell egy közös identitás Azure Active Directory-szinkronizálási szolgáltatások (AAD Sync) vagy az Azure AD Connect támogatása a helyszíni Active Directory-fiókok címtár attribútumok is szinkronizálhatja.

Olyan elengedhetetlen Tartalomkezelés része megértéséhez, hogy melyik erőforrás hozzáférhet, ezért a multimédiás naplózási szolgáltatás fontos a identitáskezelési megoldást. Azure AD a több mint 30 nap, beleértve a napló nyújtja:

- Tagsági szerepkör változásai (ex: résztvevő hozzáadva a globális rendszergazdai szerep)
- A hitelesítő adatok frissítések (ex: jelszó-módosítások)
- Tartománykezelés (ex: tartomány eltávolítása egyéni tartomány ellenőrzése)
- Hozzáadás és eltávolítás alkalmazások
- Felhasználókezelés (ex: hozzáadása, eltávolítása, a felhasználó frissítése)
- Hozzáadásáról és eltávolításáról a licenc

>[AZURE.NOTE]
Olvassa el [a Microsoft Azure biztonsági és naplózási napló Management](http://download.microsoft.com/download/B/6/C/B6C0A98B-D34A-417C-826E-3EA28CDFC9DD/AzureSecurityandAuditLogManagement_11132014.pdf) további tudnivalók a naplózás-funkciók az Azure.
Attól függően, hogy hogyan érkezett válasz [meghatározása Tartalomkezelés követelmények](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)kérdéseket látnia kell határozza meg, hogy miként kell kezelni a hibrid identitás megoldás a tartalmat. Minden beállítás a táblázat 6 elérhetővé képesek integrálása az Azure Active Directory, azt is fontos definiálása, amelyek megfelelőbb az igényeinek.

| Tartalomkezelési lehetőségeivel                                                               | Előnyei                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Hátrányai                                                                                                                                                                                                                               |
|------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Központi helyszíni (az Active Directory tartalomvédelmi szolgáltatások)                      | A kiszolgáló infrastruktúra felelős az adatok osztályozásához teljes hozzáféréssel <br> Beépített lehetőség a Windows Server rendszeren, nincs szükség külön licenc vagy -előfizetésre <br> Az Azure Active Directory hibrid helyzetben integrálhatók <br> Információk tartalomvédelmi (szolgáltatás IRM) szolgáltatásairól támogatja a Microsoft Online szolgáltatások, például az Exchange online-ban és a SharePoint online-ban, valamint az Office 365-ben <br> A helyszíni Microsoft kiszolgálói termékekkel, például az Exchange Server, a SharePoint Server és a fájl besorolás infrastruktúra (FCI) és a Windows Server rendszerű fájlkiszolgálókhoz támogatja. | Nagyobb, karbantartási (megőrzése felfelé frissítések, a konfigurációs és az esetleges frissítési), óta informatikai tulajdonosa a kiszolgáló <br> A kiszolgáló infrastruktúra a helyszíni megkövetelése<br> Doesn'tleverage Azure funkciók natív módon                                     |
| A központi a felhőben (Azure RMS)                                                     | A helyszíni megoldás könnyebben kezelheti és összehasonlítása <br> Az Active Directory tartományi szolgáltatások hibrid helyzetben integrálhatók <br>  Teljes mértékben integrálva van az Azure Active Directory <br> Használatához nincs szükség a kiszolgáló helyszíni annak érdekében, hogy a szolgáltatás telepítése <br> Támogatja a helyszíni Exchange Server, a SharePoint, a kiszolgáló és a fájl osztályozás, infrastruktúra (FCI) és a Windows Server rendszerű fájlkiszolgálókhoz például Microsoft-kiszolgálói termékek <br> Informatikai, beállíthatja, hogy a bérlői kulcs BYOK lehetőséggel szabályozható.                                                                                    | A szervezet, amely támogatja a Tartalomvédelmi felhő előfizetéssel kell rendelkeznie. <br> A szervezet kell rendelkeznie a felhasználói hitelesítés támogatására RMS egy Azure AD-címtár                                                                                  |
| Hibrid (Azure Tartalomvédelmi integrálódik, a helyszíni Active Directory tartalomvédelmi szolgáltatások) | Ebben az esetben az előnnyel mindkét központi a helyszíni és felhőbeli összegzi.                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | A szervezet, amely támogatja a Tartalomvédelmi felhő előfizetéssel kell rendelkeznie. <br> A szervezet kell rendelkeznie a felhasználói hitelesítés támogatására RMS, egy Azure AD-címtár <br> Azure felhőalapú szolgáltatás közötti kapcsolatot, és a helyszíni infrastruktúra |

## <a name="define-access-control-options"></a>Access ellenőrzési beállítások megadása
Feljebb helyezése a hitelesítés, hitelesítés és az access kezelni fogja tudni ahhoz, hogy a központi identitás folyamattárház használata közben, így a felhasználók és a partnerek egyszeri bejelentkezés (SSO) használja az alábbi ábrán látható vállalaton belüli Azure Active Directory lehetőségeit:

![](./media/hybrid-id-design-considerations/centralized-management.png)

Központi kezelése és teljesen más könyvtárak integrációja

Egyszeri bejelentkezés a szoftver alkalmazások ezreihez nyújt Azure Active Directory és a helyszíni webalkalmazások. Olvassa el a [Azure Active Directory összevonási kompatibilitási lista: külső Identitásszolgáltatók egyszeri bejelentkezés a végrehajtásához használható](https://msdn.microsoft.com/library/azure/jj679342.aspx) cikkben olvashat bővebben az egyszeri bejelentkezés külső a Microsoft által ellenőrzött. Ez a képesség lehetővé teszi a szervezet vezérlő az identitás- és az access-kezelés miközben B2B esetek számos végrehajtásához. Azonban a B2B során folyamat tervezése fontos megértéséhez fogja használni a partnert, és ellenőrizze, ha ez a módszer Azure által támogatott hitelesítési módszert. Jelenleg Azure Active Directory által támogatott módszerek a következők:

- Biztonsági állítás Markup Language (SAML)
- OAuth
- Kerberos
- Tokenek
- Tanúsítványok


>[AZURE.NOTE] olvassa el az [Azure Active Directory Authentication protokollok](https://msdn.microsoft.com/library/azure/dn151124.aspx) , hogy minden egyes protokoll és Azure-ban képességeit olvashat bővebben.

Használja az Azure Active Directory-támogatási, mobil üzleti alkalmazások segítségével a azonos egyszerűen Mobile szolgáltatások hitelesítési felület lehetővé teszi az alkalmazottaknak, hogy jelentkezzen be a mobilalkalmazások a vállalati az Active Directory hitelesítő adatokkal. Ezzel a szolgáltatással Azure AD-identitásszolgáltató használatához, a mobil szolgáltatások mellett a más identitásszolgáltatók már támogatjuk (amelyek lehetnek a Microsoft Accounts, Facebook azonosítója, a Google-azonosító vagy Twitter azonosító) szerint támogatott. Ha a helyszíni alkalmazás a felhasználó hitelesítő adatait a vállalat Tartományi található használ, az access a partnerek és a felhasználók a felhőben érkező átlátszóvá kell lennie. Felhasználó (felhőalapú) webalkalmazások, webes API, Microsoft-felhőszolgáltatások, 3 szoftver alkalmazásait és natív (mobil) ügyfélalkalmazásokban feltételes hozzáférés-vezérlés kezelésére, és biztonság, előnyei van a naplózást, a jelentési egy helyről. Azonban nem üzemi környezetben, vagy a felhasználók korlátozott mennyiségű érvényesítéséhez ez ajánlott.


>[AZURE.TIP] Fontos, hogy Azure Active Directory nem rendelkezik csoportházirend Active Directory tartományi szolgáltatások van említeni. Annak érdekében, hogy a hivatkozási eszközök házirend szüksége lesz egy mobileszközön projektmenedzsment megoldás, például a [Microsoft Intune](https://technet.microsoft.com/library/jj676587.aspx).

Amikor a felhasználó Azure Active Directory hitelesítése, fontos, hogy a felhasználó lesz rá hozzáférési szintet ki szeretné számítani. Eltérő lehet, hogy a felhasználó fog egy erőforrás hozzáférési szintet Azure AD-további biztonsági réteg kiegészítése egyes erőforrásokhoz való hozzáférés szabályozása, miközben még maradnia kell szem előtt, hogy az erőforrás maga is saját hozzáférés-vezérlési listája külön-külön, például a hozzáférés-vezérlés fájlkiszolgálón található fájlok. Az alábbi ábrán látható a hibrid helyzetben lehet hozzáférés-vezérlés szintjéről:

![](./media/hybrid-id-design-considerations/accesscontrol.png)


A diagram az ábra X mutatott minden kapcsolati access vezérlőelem egy helyzetleírás Azure Active Directory szereplő jelöli. Az alábbi minden esetben leírását van:

1. feltételes alkalmazásokat ismertetik a hozzáférést a helyszíni is:-et is bejegyzett eszközök há használatához Windows Server 2012 R2 rendszer AD FS beállított alkalmazásokat. Feltételes hozzáférést a helyszíni telepítésű beállításával kapcsolatos további tudnivalókért olvassa el a [helyszíni feltételes elérésének Azure Active Directory eszköz regisztrációs beállítása](active-directory-conditional-access-on-premises-setup.md)című témakört.
2. hozzáférés-vezérlés, Azure adatkezelési portált: Azure is képes az adatkezelési portál való hozzáférés szabályozása (szerepkör alapú hozzáférés-vezérlés) RBAC használatával. Ez a módszer lehetővé teszi, hogy a vállalatot, amely egy adott hajthatja végre, miután Azure adatkezelési portálra hozzáférése van műveletek mennyiségét korlátozni. RBAC való hozzáférés korlátozása a portál használatával az informatikai rendszergazdák hitelesítésszolgáltató hozzáférés átadása használatával az alábbi módszerekkel a hozzáférés kezelése:

 - A csoport alapú szerepkör-hozzárendelés: rendelhet access Azure Active Directory-csoportok kialakítására is szinkronizálható a helyi Active Directoryból. Engedélyezi, használhatja a meglévő befektetések, hogy a szervezet összes dia megnézéséhez szerszámok és csoportok kezelésére szolgáló eljárásokat. A meghatalmazott csoport management szolgáltatás Azure Active Directory-támogatás is használható.
 - Azure-ban szerepkörök beépített emelés: használhatja a három szerepkörök – tulajdonosa, közreműködői és olvasó, győződjön meg arról, hogy felhasználók és csoportok csak a feladatokat munkájuk elvégzéséhez szükségük van engedélye van.
 - Erőforrások részletes elérése: a felhasználók és csoportok, ha egy előfizetéshez, erőforráscsoport vagy egy adott Azure erőforrás, például egy webhelyre vagy egy adatbázis kioszthatja a szerepköröket. Ezzel a módszerrel biztosíthatja, hogy felhasználó rendelkezik-e szükségük van az összes erőforrások elérését, és nem lehet hozzáférni információforrások, amelyek nem szükséges kezelése.

>[AZURE.NOTE] olvassa el az [Azure szerepköralapú hozzáférés-vezérlés](https://azure.microsoft.com/updates/role-based-access-control-in-azure-preview-portal/) , hogy ez a képesség olvashat bővebben. Fejlesztőknek alkalmazások létrehozásába vannak, és szeretné szabni a hozzáférés-vezérlés az őket hogy az is lehetséges Azure Active Directory-alkalmazás szerepkörök használandó engedélyezési. Tekintse át a [webalkalmazás-RoleClaims-DotNet példa](https://github.com/AzureADSamples/WebApp-RoleClaims-DotNet) hogyan lehet az alkalmazást használni ezt a lehetőséget.

3.a az Office 365-alkalmazások Microsoft Intune feltételes hozzáférés: informatikai rendszergazdák is kiépítése feltételes hozzáférés biztonságossá tétele a vállalati erőforrások, miközben egy időben lehetővé teszi az infomunkások a megfelelő eszközök elérése a szolgáltatások házirendeket. További tudnivalókért olvassa el a [Feltételes Access eszköz házirendek az Office 365 szolgáltatásainak](active-directory-conditional-access-device-policies.md)című témakört.

4. feltételes Access-alkalmazások szoftver: [ezzel](http://blogs.technet.com/b/ad/archive/2015/06/25/azure-ad-conditional-access-preview-update-more-apps-and-blocking-access-for-users-not-at-work.aspx) a funkcióval alkalmazásonként többtényezős hitelesítés hozzáférési szabályok és az azt jelenti, hogy tiltani a nem megbízható hálózaton felhasználók hozzáférésének konfigurálását. Többtényezős hitelesítés szabályok alkalmazhat az alkalmazást, vagy csak a megadott biztonsági csoportok belüli felhasználók rendelt összes felhasználóra. Felhasználók lehet zárni többtényezős hitelesítés kötelező, ha az alkalmazás az IP-címet, hogy a szervezeten belüli hálózati férnek hozzá.

A hozzáférés-vezérlés lehetőségeit használni a többrétegű megközelítés, mivel ezek a lehetőségek összehasonlítása nem használhatók a feladat végrehajtásához. Győződjön meg arról, hogy az összes lehetőségei minden esetben igénylő az erőforrások való hozzáférés szabályozása feljebb helyezése.

## <a name="define-incident-response-options"></a>Az esemény visszajelzési beállítások megadása
Azure Active Directory segítheti IT-identitás potenciális biztonsági kockázatot felhasználó tevékenységét, informatikai lehet megfigyelésével környezetben kihasználhatja az Azure Active Directory Access és használati jelentések betekintést kap abba, hogy a integritás és a szervezet címtár biztonsága eléréséhez lehetőséget. Ezekkel az információkkal az informatikai rendszergazda jobban megállapítható hol lehetséges biztonsági kockázatot elhelyezkednie előfordulhat, hogy azok megfelelően tervezi, hogy e kockázatok csökkentésében.  [Azure Active Directory prémium verzióra szóló előfizetéssel](active-directory-get-started-premium.md) rendelkezik, amelyekkel biztonsági jelentések informatikai az adatok beolvasásához. [Azure AD-jelentések](active-directory-view-access-usage-reports.md) kategorizálják alább látható módon:

- **Rendellenességet jelentések**: tartalmaznia kell anomalous talált események bejelentkezés. Célunk, ilyen tevékenység tudomást és lehetővé teszi, győződjön meg arról, hogy az esemény gyanús meghatározása láthatja.
- **Integrált alkalmazások jelentés**: háttérismeretek be, hogy hogyan használják a szervezetben a felhő alkalmazások biztosít. Azure Active Directory-integráció a felhőben alkalmazások ezer kínál.
- **Hibajelentések**: külső alkalmazásokban fiókok kiépítésének esetleg előforduló hibák jelzésére.
- **Felhasználó-specifikus jelentések**: az adott felhasználó tevékenység adatainak megjelenítése a eszköz/bejelentkezési.
- **Tevékenység naplók**: az összes ellenőrzött események rekord tartalmaz a az elmúlt 24 óra, a utóbbi 7 napból vagy a 30 napnál, valamint a csoport tevékenység módosításokat és a jelszó alaphelyzetbe állítása és nyilvántartási tevékenység belül.

>[AZURE.TIP]
Egy másik, amelyek segíthetnek a esemény válasz csapatának az esetek jelentésről van szó a [felhasználó elvesztett hitelesítő adatokkal](http://blogs.technet.com/b/ad/archive/2015/06/15/azure-active-directory-premium-reporting-now-detects-leaked-credentials.aspx) .  Ez a jelentés felületek a találatokat e elvesztett hitelesítő adatok listája és a bérlő között.

Egyéb fontos jelentések az Azure Active Directory-az esemény válasz vizsgálat alatt használható beépített és:

- **Az új jelszó kérésének tevékenység**: küldje el a rendszergazda hogyan aktívan jelszó-visszaállítás használja a szervezet az összefüggéseket.
- **Jelszó-nyilvántartási tevékenység**: mélyebb, amelybe a felhasználók regisztrálta a jelszó-visszaállítás módszerei biztosít, és milyen módszerekkel van jelölve.
- **Csoport tevékenység**: módosítási előzményeit jelenít meg a csoport (ex: a felhasználóknak, illetve el), amely az Access panel kezdeményezett.

Az Azure Active Directory prémium verzióban, amelyek valószínűleg kihasználják egy esemény válasz vizsgálat folyamat során, informatikai lehet is elérhető core jelentéskészítési lehetőség mellett kihasználhatja a naplójelentés információkhoz juthat:

- Tagsági szerepkör változásai (ex: résztvevő hozzáadva a globális rendszergazdai szerep)
- A hitelesítő adatok frissítések (ex: jelszó-módosítások)
- Tartománykezelés (ex: tartomány eltávolítása egyéni tartomány ellenőrzése)
- Hozzáadás és eltávolítás alkalmazások
- Felhasználókezelés (ex: hozzáadása, eltávolítása, a felhasználó frissítése)
- Hozzáadásáról és eltávolításáról a licenc

Az esemény válasz lehetőségei a többrétegű megközelítés használja, mivel ezek a lehetőségek összehasonlítása nem használhatók a feladat végrehajtásához. Győződjön meg arról, hogy minden egyes forgatókönyv igénylő használata Azure Active Directory jelentéskészítési lehetőség a vállalat az esemény válasz folyamatának részeként használható beállítások feljebb helyezése.

## <a name="next-steps"></a>Következő lépések
[Hibrid identitás adatkezelési feladatok meghatározása](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)


## <a name="see-also"></a>Lásd még:
[Tervezési szempontjait – áttekintés](active-directory-hybrid-identity-design-considerations-overview.md)

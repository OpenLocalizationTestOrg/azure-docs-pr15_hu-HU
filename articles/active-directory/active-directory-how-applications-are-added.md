<properties
   pageTitle="Alkalmazások hogyan Azure Active Directory vehetők fel."
   description="Ez a cikk azt ismerteti, hogyan vehetők fel az alkalmazást az Azure Active Directory-példány."
   services="active-directory"
   documentationCenter=""
   authors="shoatman"
   manager="kbrint"
   editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="02/09/2016"
      ms.author="shoatman"/>

# <a name="how-and-why-applications-are-added-to-azure-ad"></a>Hogyan és miért alkalmazások hozzáadódnak az Azure Active Directory

Az eredetileg értelmezhető lehet dolgot alkalmazások listájának megtekintésekor az Azure Active Directory-példányban van ismertetése, hol az alkalmazások származik, és miért van.  Ez a cikk nyújt az alkalmazások jelennek meg a könyvtár és módja nyújt segítséget bemutatása, hogyan az alkalmazás el szeretné helyezni a címtárában volt környezetben magas szintű áttekintése.

## <a name="what-services-does-azure-ad-provide-to-applications"></a>Mely szolgáltatásokat nyújt Azure Active Directory alkalmazások?

Azure Active Directory értekezletállapota megjelenjen egy vagy több szolgáltatásokról alkalmazások ad hozzá.  Azokat a szolgáltatásokat tartalmazza:

* Alkalmazás hitelesítése és engedélyezése
* Felhasználói hitelesítés és engedélyezése
* Egyszeri bejelentkezés (SSO) összevonási vagy jelszó használatával
* Felhasználói kiépítési és szinkronizálása
* Szerepköralapú hozzáférés-vezérlés; Alkalmazás szerepkörök szerepkörök végrehajtásához meghatározása a címtár-alkalmazásokban hitelesítés ellenőrzése alapuló.
* oAuth engedélyezési szolgáltatásokat (által használt Office 365-ben és az egyéb Microsoft-alkalmazások API-k és erőforrások elérésének engedélyezése.)
* Közzététel alkalmazás és a proxy; Az internetes magánhálózat alkalmazás közzététele

## <a name="how-are-applications-represented-in-the-directory"></a>Hogyan jelző a címtárban alkalmazások?

Alkalmazások jelennek meg az Azure AD-2-objektumok használatával: alkalmazás objektum és egy egyszerű-objektum.  Nincs alkalmazása egy objektumra, regisztrált "otthoni" / "tulajdonosa" vagy "közzététel" könyvtár és egy vagy több a szolgáltatás fő objektumok, amely az alkalmazás minden könyvtárban, amelyben működik.  

Az alkalmazás objektum ismerteti az alkalmazás Azure Active Directory (a több elem bérlői szolgáltatás) és az alábbiak bármelyikét tartalmazhatják: (*Megjegyzés*: Ez nem egy teljes lista.)

* Név, embléma és a Publisher
* Titkos kulcsok (szimmetrikus és/vagy aszimmetrikus billentyűk hitelesíti az alkalmazás)
* API függőségek (oAuth)
* API-k és erőforrások/hatókörök közzétett (oAuth)
* Alkalmazás szerepkörök (RBAC)
* Egyszeri bejelentkezés metaadat- és konfigurációs (SSO)
* Metaadat- és konfigurációs kiépítési felhasználói
* A proxykiszolgáló metaadatok és a konfigurációs

A szolgáltatás egyszerű az alkalmazás minden könyvtárban, ahol az alkalmazás dolgozza fel, beleértve az otthoni címtár rekordot.  A szolgáltatás egyszerű:

* Vissza az alkalmazás azonosítója tulajdonság keresztül alkalmazás objektum hivatkozik
* Rekordok helyi felhasználó, és az alkalmazás szerepkör-hozzárendelések csoport
* Helyi felhasználó és a rendszergazdai engedélyekkel nyújtott rekordok az alkalmazásba
    * Példa: ngedéllyel egy adott felhasználók levelezésének eléréséhez az alkalmazás
* Rekordok feltételes hozzáférési házirend például helyi házirendek
* Rekordok helyi alternatív helyi beállítások-alkalmazáshoz
    * Követelések átalakítása szabályok
    * Attribútum megfeleltetésének (felhasználói kiépítési)
    * Bérlői az adott alkalmazás szerepkörök (ha az alkalmazás támogatja az egyéni szerepkör)
    * Név/embléma

### <a name="a-diagram-of-application-objects-and-service-principals-across-directories"></a>Az alkalmazás-objektumok és szolgáltatás rendszerbiztonsági keresztül könyvtárak diagram létrehozása

![A diagram, mely bemutatja, hogyan alkalmazás objektumokat és Azure Active Directory-példány a meglévő rendszerbiztonsági szolgáltatás.][apps_service_principals_directory]

Amint látható a diagramról.  A Microsoft fenntartja két könyvtárak belső (a bal oldalon) alkalmazások közzététele használja.

* Egy Microsoft Apps (a Microsoft címtár)
* Egy előre integrált 3 fél alkalmazások (Alkalmazásgyűjteményébe könyvtár)

A közzétételi könyvtár alkalmazás közzétevők/szállítók integrálása az Azure Active Directory van szüksége.  (Néhány szoftver könyvtár).

Alkalmazások, ad hozzá a következők:

* Ön által fejlesztett alkalmazások (AAD integrálva)
* Alkalmazások, a kapcsolat single-sign-on
* Alkalmazások tett közzé az Azure Active Directory-alkalmazás proxy használatával.

### <a name="a-couple-of-notes-and-exceptions"></a>Jegyzetek és a kivételek néhány

* Nem minden szolgáltatás rendszerbiztonsági vissza az alkalmazás-objektumokra mutatnak.  Huh? Ha eredetileg készült Azure Active Directory a nyújtott alkalmazásokhoz szolgáltatások lettek korlátozottabb és a szolgáltatás egyszerű elegendő az alkalmazás identitás létrehozásáról.  Az eredeti szolgáltatás fő volt az alakzat közelebb a Windows Server Active Directory-szolgáltatási fiók.  Emiatt a továbbra is létre lehet hozni szolgáltatás alapelvei az Azure Active Directory PowerShell használata az alkalmazás-objektum létrehozása nélkül.  A diagram API alkalmazás objektum szolgáltatás fő létrehozása előtt van szükség.
* A fenti információk közül nem mindegyik jelenleg megjelenő programozás útján.  Az alábbiakban csak felhasználói felület érhető el:
    * Jogcímeken átalakítási szabályok
    * Attribútum megfeleltetésének (felhasználói kiépítési)
* További részletes információkat a szolgáltatás egyszerű, és az alkalmazás objektumok olvassa el a Azure Active Directory Graph REST API-hivatkozás dokumentációt.  *Tipp*: az Azure Active Directory Graph API dokumentáció áll a séma hivatkozást a legközelebbi dolog az Azure Active Directory jelenleg elérhető.  
    * [Alkalmazás](https://msdn.microsoft.com/library/azure/dn151677.aspx)
    * [Egyszerű](https://msdn.microsoft.com/library/azure/dn194452.aspx)


## <a name="how-are-apps-added-to-my-azure-ad-instance"></a>Alkalmazások hogyan vehetők fel az Azure Active Directory-példány?
Számos módon alkalmazás manuálisan is hozzáadhatók Azure Active Directory:

* Alkalmazás hozzáadása az [Azure Active Directory Alkalmazásgyűjteményébe](https://azure.microsoft.com/updates/azure-active-directory-over-1000-apps/)
* Bejelentkezési feljebb/egy 3rd az Azure Active Directory integrálódik fél alkalmazás (például: [Smartsheet](https://app.smartsheet.com/b/home) vagy [DocuSign](https://www.docusign.net/member/MemberLogin.aspx))
    * Jelentkezzen be, és a felhasználók során a rendszer kéri hozzáférése a alkalmazásba a profilját, és más engedélyeket adhat.  Az első személy hozzájárulása adjon hatására megjelenjen a címtárhoz szolgáltatás egyszerű, amely az alkalmazás.
* Jelentkezzen be/a Microsoft online szolgáltatások, például [az Office 365](http://products.office.com/) -be
    * Ha az Office 365-előfizetés vagy egy vagy több szolgáltatás rendszerbiztonsági létrehozásakor a címtárban, amely a különböző szolgáltatások összes Office 365-tel társított funkció előadásához használt próbaverzió megkezdéséhez.
    * Néhány Office 365-szolgáltatásokkal, például a SharePoint szolgáltatás rendszerbiztonsági létrehozása, összetevők, beleértve a munkafolyamatok közötti biztonságos kommunikáció engedélyezése a folyamatos kombinálásával.
* Fejleszt alkalmazás hozzáadása az Azure Kezelőportálja segítségével című témakörben talál: https://msdn.microsoft.com/library/azure/dn132599.aspx
* Adja hozzá a Visual Studio fejleszt alkalmazás lásd:
    * [ASP.Net hitelesítési módszerek](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions)
    * [Kapcsolt szolgáltatások](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx)
* Való használatáról az [Azure Active Directory szolgáltatásalkalmazás-Proxy](https://msdn.microsoft.com/library/azure/dn768219.aspx) alkalmazás hozzáadása
* Csatlakozás a SAML vagy jelszó egyszeri bejelentkezés egyszeri bejelentkezés az alkalmazásba
* Számos más többek között a különböző fejlesztői szolgáltatások Azure-ban és/az API explorer találkozik keresztül Fejlesztőeszközök központok

## <a name="who-has-permission-to-add-applications-to-my-azure-ad-instance"></a>Ki jogosult az alkalmazások az Azure Active Directory-példány hozzá?

Csak a globális rendszergazdák végezheti el:

* Alkalmazások hozzáadása az Azure Active Directory alkalmazás gyűjteményből (előre integrált 3 fél alkalmazások)
* Az alkalmazás használatáról az Azure Active Directory-alkalmazás Proxy közzététele

Az összes felhasználó a címtárban jogosultságok alkalmazások fejlesztéséhez azok és tetszése szerint fölé mely alkalmazások azok megosztás/adja meg az access a szervezeti adatokat.  *Ne feledje, hogy a felhasználónak bejelentkezés felfelé /-alkalmazásba, és engedélyek megadása a létrehozandó szolgáltatás egyszerű vonhat.*

A elképzelhető, hogy eredetileg hang vonatkozó, de kell szem előtt a következőket:

* Alkalmazások kaptak tudja kihasználhatja a Windows Server Active Directory felhasználói hitelesítés évek anélkül, hogy az alkalmazás a címtárban regisztrált/felvett lesz.  Most már a szervezet fog van továbbfejlesztett láthatóságát, hogy pontosan hány alkalmazások használata a könyvtár és what for.
* Nincs szükség rendszergazdai alapú alkalmazás közzététel/regisztrációs folyamat.  Az Active Directory összevonási szolgáltatások volt valószínű, hogy a rendszergazda alkalmazást szeretne hozzáadni a fejlesztők nevében megbízó fél mint volt.  Most már a fejlesztők önkiszolgáló is.
* Bejelentkezés a/alkalmazások használata a szervezet fiókjuk üzleti célra felfelé felhasználók azért hasznos.  Ha elhagyják a szervezet ezt követően ezeket az access az alkalmazás csak a fiókjára elvesznek.
* Hogy milyen adatokat, amelyekkel alkalmazás egy jó már megosztva egy rekordot.  További hordozható eddiginél adatai, és hogy ki milyen adatokat oszt meg, mely alkalmazások törlése rekord hasznos.
* Azure AD-oAuth használó alkalmazások pontosan milyen engedélyekkel, hogy a felhasználók csak biztosítani kívánt alkalmazást, és igénylő engedélyek fogadja el a rendszergazda döntse el.  Az Ugrás a következő, hogy csak a rendszergazdák is hozzájárul nagyobb hatókörök és több lényeges engedélyek értetődő kell.
* Felhasználók hozzáadása, és lehetővé teszi az alkalmazások hozzáférhetnek az adataikhoz naplózott eseményeket, így megállapíthatja, hogyan hozzá lett adva az alkalmazás a címtárhoz belül a Azure felügyeleti portál naplójelentések megtekintése.

**Megjegyzés:** *Magát Microsoft működő a következő az alapértelmezett beállítás használatával számos hónapig.*

Az összes, amely said, lehetséges, hogy megakadályozza, hogy a felhasználó a címtárban alkalmazások hozzáadása és a gyakorló tetszése szerint milyen adatok szigorúbb információikat alkalmazásokkal Directory konfigurálása az Azure felügyeleti portál módosításával.  Az alábbi beállításokkal belül az Azure kezelőportálja segítségével a könyvtár "Konfigurálása" lapján is elérhető.

![Képernyőkép a felhasználói felületének az integrált alkalmazások beállításainak konfigurálása][app_settings]


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Következő lépések

További tudnivalók a hozzáadásuk Azure AD az alkalmazások és -alkalmazások szolgáltatások konfigurálása.

* A fejlesztők: [tudnivalók AAD kérelmet](https://msdn.microsoft.com/library/azure/dn151122.aspx)
* A fejlesztők: [Azure Active Directory Github integrálva Véleményezés minta kód-alkalmazások](https://github.com/AzureADSamples)
* A fejlesztők és az informatikai szakemberek számára: [REST API dokumentációjában találhat az Azure Active Directory Graph API áttekintése](https://msdn.microsoft.com/library/azure/hh974478.aspx)
* Informatikai szakemberek számára:, hogy [miként használhatja az Azure Active Directory előre integrált alkalmazások az alkalmazás-dokumentumtárból](https://msdn.microsoft.com/library/azure/dn308590.aspx)
* Az informatikai szakemberek: [oktatóprogramokat az adott előre integrált alkalmazások beállítása](https://msdn.microsoft.com/library/azure/dn893637.aspx)
* Az informatikai szakemberek: [megtudhatja, hogy miként teheti közzé egy alkalmazást a Azure Active Directory szolgáltatásalkalmazás-Proxy](https://msdn.microsoft.com/library/azure/dn768219.aspx)

## <a name="see-also"></a>Lásd még:

- [Az alkalmazások kezelése az Azure Active Directory cikk indexben](active-directory-apps-index.md)

<!--Image references-->
[apps_service_principals_directory]:media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg
[app_settings]:media/active-directory-how-applications-are-added/IntegratedAppSettings.jpg

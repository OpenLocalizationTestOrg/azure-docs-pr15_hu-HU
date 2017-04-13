<properties 
    pageTitle="Azure többtényezős hitelesítés - működése"
    description="Azure többtényezős hitelesítés segít védelmét hozzáférés az adatokhoz és alkalmazások felhasználói igény szerint egy egyszerű bejelentkezési folyamat az értekezlet közben. Nagyobb biztonságot nyújt a második hitelesítési mód kötötten és a könnyen ellenőrzési beállítások adattartomány keresztül erős hitelesítés biztosítja."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

#<a name="how-azure-multi-factor-authentication-works"></a>Hogyan működik a Azure többtényezős hitelesítés

Többtényezős hitelesítés biztonsága annak réteges megközelítésben helyezkedik el. Jelentős bonyolulttá a támadók több hitelesítési tényezők betörjön mutatja be. Akkor is, ha a támadó kezeli a felhasználók jelszava megtudhatja, érdemes használhatatlan a megbízható eszköz birtokában nélkül is. Elveszítik a felhasználónak az eszközt, a személy, aki találja nem ahhoz, hogy használhassa, kivéve, ha az illető kikkel is tudja, hogy a felhasználók jelszava.

![Proofup](./media/multi-factor-authentication-how-it-works/howitworks.png)



Azure többtényezős hitelesítés segít védelmét hozzáférés az adatokhoz és alkalmazások felhasználói igény szerint egy egyszerű bejelentkezési folyamat az értekezlet közben.  Nagyobb biztonságot nyújt a második hitelesítési mód kötötten és a biztosítja erős hitelesítés keresztül egyszerűen ellenőrzési lehetőségek közül:

- Telefonhívás
- szöveges üzenetben
- értesítés mobilalkalmazás – így a felhasználók kiválaszthatja azt a módot előnyben részesített
- ellenőrzőkódot mobilalkalmazásban
- 3 fél elfogadható tokenek

Talál további tudnivalókat nahát hogyan működik az alábbi videóban.

>[AZURE.VIDEO multi-factor-authentication-deep-dive-securing-access-on-premises]

##<a name="methods-available-for-multi-factor-authentication"></a>Többtényezős hitelesítés rendelkezésre álló módszerek
Amikor a felhasználó bejelentkezik, egy további ellenőrzési a felhasználónak küldi.  Az alábbiakban a második az ellenőrzéshez használt módszerek listáját.

Ellenőrzési módszer  | Leírás
------------- | ------------- |
Telefonhívás | Hívás kezdeményezése a felhasználó okostelefonon érdeklődje meg, ellenőrizze, hogy azok van bejelentkezve az billentyűkombináció lenyomásával a # jel megadásához kerül.  Ez a tartományigazolási folyamatot befejezi.  Ez a beállítás állítható be, és módosíthatják a megadott kódot.
Szöveges üzenetben | Szöveges üzenetet küld egy felhasználó okostelefonon 6 jegyű kóddal.  Írja be a kód az ellenőrzési folyamat befejezéséhez.
Értesítés mobilalkalmazásban | Ellenőrzés kérelem küld egy felhasználó okostelefonon mintaüzenetet teljes az igazolás kattintva ellenőrizze a mobilalkalmazásban. Ez akkor fordul elő, ha alkalmazás értesítést választotta az elsődleges ellenőrzési módszer.  Kapnak a történő nem bejelentkezés során, ha csalás jelentés választhatnak.
Ellenőrzőkódot Mobile alkalmazásban | A felhasználó okostelefonon futtató mobilalkalmazás küld egy Ellenőrzőkód.  Ez akkor fordul elő, ha egy Ellenőrzőkód választotta az elsődleges ellenőrzési módszer.


##<a name="available-versions-of-azure-multi-factor-authentication"></a>Rendelkezésre álló verziók Azure többtényezős hitelesítés
Azure többtényezős hitelesítés három különböző verzióiban érhető el.  Az alábbi táblázat ismerteti az egyes az alábbiak részletesebben.

Verzió  | Leírás
------------- | ------------- |
Többtényezős hitelesítés Office 365-höz | Ez a verzió kizárólag az Office 365-alkalmazások működik, és az Office 365 portálon kezelheti. Így rendszergazdák most segítséget az Office 365-forrásokat biztonságos többtényezős hitelesítés használatával.  Ez a verzió megtalálható az Office 365-előfizetéséhez.
Többtényezős hitelesítés Azure rendszergazdák számára | Többtényezős hitelesítés lehetőségei az Office 365-höz egy részhalmazát áttérhet az összes Azure rendszergazdák számára elérhető lesz. Az Azure előfizetés minden rendszergazdai fiók most kaphat további védelmet, mivel a core többtényezős hitelesítés funkció. A rendszergazda, amely egy virtuális, a webhely létrehozása az Azure portál elérhető szeretne tárhely kezelése, így mobil szolgáltatások vagy bármely más Azure szolgáltatás hozzáadhat többtényezős hitelesítés be rendszergazdai fiókjával.
Azure többtényezős hitelesítés | Azure többtényezős hitelesítés kínál funkciókat richest csoportja. <br><br>További beállítások az Azure kezelőportálja, speciális jelentéskészítés és támogatási keresztül biztosít a helyszíni tartomány és a felhő alkalmazásokat. Azure többtényezős hitelesítés vásárolhatja meg önálló licenc, és Azure Active Directory prémium és vállalati mobilitás csomagja belül kapcsolt van. <br><br>Azt is vásárolhatja felhasználási alapon: hozzon létre egy Azure többtényezős hitelesítést szolgáltatóval az Azure-előfizetésben.
##<a name="feature-comparison-of-versions"></a>A verziók webhelyfunkciók összehasonlítása
A következő az alábbi táblázat az Azure többtényezős hitelesítés különböző verzióiban elérhető szolgáltatások listáját.


A szolgáltatás  | Többtényezős hitelesítés Office 365-höz (Office 365 termékváltozatban része)|Többtényezős hitelesítés (Azure előfizetéssel rendelkező része) Azure-rendszergazdák számára | Azure többtényezős hitelesítés (az Azure Active Directory prémium és vállalati mobilitás csomagja tartalmazza)
------------- | :-------------: |:-------------: |:-------------: |
A rendszergazdák megvédheti MFA fiókok| * | * (Csak az Azure rendszergazdafiókjához érhető el)|*
A második tényező, mobilalkalmazásban|* | * | *
Telefonhívás, mint a második tényező|* | * | *
Az SMS egy második tényező|* | * | *
Alkalmazás jelszavak MFA nem támogató ügyfélalkalmazások|* | * | *
Rendszergazdai szabályozható hitelesítési módszerek| *| *| *
PIN-kód mód| | | *
Csalás értesítés| | | *
MFA-jelentések| | | *
Egyszeri figyelmen kívül hagyása| | | *
Egyéni Üdvözlések esetében a telefonhívások| | | *
Hívó azonosítója az telefonhívások testreszabása| | | *
Esemény megerősítése| | | *
Megbízható IP-címei| | | *
MFA felfüggesztése megjegyzett eszközök esetén (nyilvános előzetes verzió)| | | *
MFA SDK| | | *
MFA MFA-kiszolgáló használata esetén a helyszíni alkalmazáshoz| | | *


##<a name="how-to-get-azure-multi-factor-authentication"></a>Azure többtényezős hitelesítés beszerzése

Ha szeretné, hogy a teljes funkcionalitását, csak azokat az Office 365-felhasználóknak és Azure rendszergazdák helyett Azure többtényezős hitelesítés által kínált, több lehetőség el:

1.  Azure többtényezős hitelesítés licencek vásárlása és hozzárendelése a felhasználókhoz.
2.  Azure többtényezős hitelesítés, például az Azure Active Directory Premium vagy vállalati Mobility csomagja határértékeket kapcsolt rendelkező licencek vásárlása és hozzárendelése a felhasználókhoz.
3.  Hozzon létre egy Azure többtényezős hitelesítést szolgáltatóval az Azure előfizetéssel belül. Ha még nem rendelkezik az Azure előfizetéssel, jelentkezzen Azure próba-előfizetésre. Próba-előfizetések kell rendszeres előfizetés próbaverzió lejárat előtt alakulnak.

Ha egy Azure többtényezős hitelesítést szolgáltató két használatát modell áll rendelkezésre, hogy a program a számlát kapni az Azure-előfizetés révén használata:


- **Felhasználónként**. Általában nagyvállalatoknak szeretnék többtényezős hitelesítés engedélyezése az adott számú alkalmazottak, akik rendszeres a hitelesítés szükséges.
- **Hitelesítés per**. Általában nagyvállalatoknak, amely engedélyezni szeretné a külső felhasználók, akik ritkán hitelesítési nagy létszámú csoportnak többtényezős hitelesítés.

Részletek árak című [Azure MFA árak.](https://azure.microsoft.com/pricing/details/multi-factor-authentication/)

Válassza ki a licencszámra vagy a szervezet számára legjobb felhasználási-alapú modell.   Jelölje be az első lépések lásd [Első lépések](multi-factor-authentication-get-started.md)

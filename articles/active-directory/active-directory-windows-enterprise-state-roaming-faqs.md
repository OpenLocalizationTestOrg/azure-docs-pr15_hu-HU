<properties
    pageTitle="Beállítások és az adatok gyakran ismételt kérdések barangoló |} Microsoft Azure"
    description="Itt választ kaphat néhány kérdésre rendszergazdák előfordulhat, hogy beállítások és az alkalmazás adatok szinkronizálása."
    services="active-directory"
    keywords="Vállalati állam központi beállításait, windows felhőben, gyakori kérdéseket vállalati állam barangoló"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="settings-and-data-roaming-faq"></a>És az adatfájl barangoló – gyakori kérdések

Ez a témakör néhány kérdésre rendszergazdák lehet beállítások és az alkalmazás adatok szinkronizálása.

## <a name="what-data-roams"></a>Adatok mozoghat?
**A Windows beállításai**: a gépház a Windows operációs rendszer beépített. Általánosságban elmondható ezek a beállítások testre szabásához PC-n, és a következő fő kategóriába beállítások az alábbiak:

- *Téma*, amelyek tartalmazzák még a szolgáltatások, például az asztali téma és a tálca beállítások.
- *Az Internet Explorer beállításainak*a lapok és a Kedvencek többek között a legutóbb megnyitott.
- *Edge böngésző beállításait*, például a Kedvencek és olvasási listában.
- A *jelszavak*, többek között az Internet jelszavak, Wi-Fi profilok és más.
- *Nyelvi beállításokat*, amelyek billentyűzetkiosztások, rendszer nyelvét, dátum és idő és az egyéb beállításait.
- *Az access szolgáltatások Kezeléstechnikai*, például a kontrasztos témáját, a Narrátor és a Nagyító.
- *Egyéb Windows beállításai*, például a parancssor beállításai és az alkalmazás listában.


**Alkalmazás adatok**: univerzális Windows alkalmazások beállítások adatokat tud írni egy központi mappában, és az alkalmazás automatikusan szinkronizálja a semmilyen adatot ír ebbe a mappába. Azt,, hogy az egyes alkalmazás fejlesztője megtervezéséhez alkalmazás kihasználhatja ezt a lehetőséget. Univerzális Windows kapcsolatos használó alkalmazás központi, lásd: az [appdata tárolás API-val](https://msdn.microsoft.com/library/windows/apps/mt299098.aspx) és a [Windows 8 appdata központi Fejlesztőeszközök blog](http://blogs.msdn.com/b/windowsappdev/archive/2012/07/17/roaming-your-app-data.aspx).

## <a name="what-account-is-used-for-settings-sync"></a>Milyen fiókot szolgál a szinkronizálási beállításokat?
A Windows 8 és Windows 8.1 a szinkronizálási beállítások mindig használja a fogyasztói Microsoft-fiók. Vállalati felhasználóknak az azt jelenti, hogy egy Microsoft-fiók csatlakoztatása az Active Directory tartományi fiók eléréséhez beállítások szinkronizálása volt. A Windows 10-ben kapcsolt Microsoft-fiók egy elsődleges és másodlagos fiók keretrendszer cseréli, a használható funkciók körét.

Az elsődleges fiók definíciója a Windows rendszerbe való bejelentkezéshez használt fiók. Ez lehet a Microsoft-fiókkal, az Azure Active Directory (Azure Active Directory), egy helyszíni Active Directory-fiókját, vagy helyi-fiókkal. Az elsődleges fiók mellett a Windows 10-felhasználók fiókot egy vagy több másodlagos felhő is az eszköze. Egy másodlagos fiókot a Microsoft-fiókkal, Azure AD-fiókkal, vagy néhány más fiókkal, például Gmail- vagy Facebook általában. Ezek a másodlagos fiókok hozzáférést biztosítanak további szolgáltatások, például az egyszeri bejelentkezés és a Windows áruházból, de még nem képes a szinkronizálási beállítások – áttekintés.

A Windows 10, a beállítások szinkronizálása az eszköz csak az elsődleges fiók használható (lásd: [hogyan frissíthetem a Microsoft-fiók beállítások szinkronizálása a Windows 8 rendszerben az Azure Active Directory beállítások szinkronizálása a Windows 10-ben?](active-directory-windows-enterprise-state-roaming-faqs.md#How-do-I-upgrade-from-Microsoft-account-settings-sync-in-Windows-8-to-Azure-AD-settings-sync-in Windows-10?)).

Adatok soha nem vegyes között a különböző felhasználói fiókokat az eszközön. Beállítások szinkronizálása két szabályok vonatkoznak:

- Windows beállításait a program mindig hordozása az elsődleges fiókkal.
- Alkalmazás adatokat fog címkézve szerezheti be az alkalmazás használt fiókkal. Az elsődleges fiók címkézve csak alkalmazások szinkronizálva lesznek. Alkalmazás tulajdonjogot címkézés egymás terhelt mobileszköz-kezelés (MDM) vagy a Windows áruházból keresztül alkalmazás esetén határozza meg.

Ha az alkalmazás tulajdonosa nem azonosíthatók, azt hordozása lesz az elsődleges fiókkal. Ha egy eszközt frissítését a Windows 8 vagy Windows 8.1 és Windows 10, minden alkalmazás, a Microsoft-fiók megvásárolta fog címkézve. Ennek az oka, hogy a legtöbb felhasználó – a Windows áruházból alkalmazásokat töltsenek, és nem a Windows áruházból támogatja az Azure Active Directory-fiókok Windows 10-es előtt volt. Ha az alkalmazás telepítve van egy offline licenc keresztül, az alkalmazás címkét kapnak az elsődleges fiók használata az eszközön.

>[AZURE.NOTE]  
> Vállalati tulajdonában, és Azure Active Directory csatlakozik a Windows 10 eszközök már nem lehet csatlakozni a Microsoft-fiók a tartományi fiók. A Microsoft-fiók csatlakoztatása a tartományi fiók és a felhasználói adatok szinkronizálása a Microsoft-fiókba (Ez azt jelenti, hogy a Microsoft-fiók a csatlakoztatott Microsoft-fiók és az Active Directory funkcióinak keresztül barangoló) van a Windows 10-eszközökön tartományhoz csatlakoztatott az Active Directory és Azure Active Directory-környezet törlődik.

## <a name="how-do-i-upgrade-from-microsoft-account-settings-sync-in-windows-8-to-azure-ad-settings-sync-in-windows-10"></a>Hogyan frissíthetem a Microsoft-fiók beállítások szinkronizálása a Windows 8 rendszerben az Azure Active Directory beállítások szinkronizálása a Windows 10-ben?
Ha a tartományhoz csatlakoztatott Microsoft-fiók Windows 8 vagy Windows 8.1 rendszerrel Active Directory-tartománya, a Microsoft-fiókon keresztül szinkronizálva lesznek beállítások. Miután frissített a Windows 10-re mindaddig, amíg az Active Directory tartományi nem csatlakozik az Azure Active Directory és a tartományhoz tartozó felhasználó szinkronizálni a felhasználói beállításokat a Microsoft-fiók használatával akkor is.

Azure AD a helyszíni Active Directory tartományi kapcsolódni, ha az eszköz beállítások használatával a csatlakoztatott szinkronizálni próbálja Azure AD-fiókot. Ha az Azure Active Directory-rendszergazda nem engedélyezte a vállalati állam központi, a csatlakoztatott Azure AD-fiók fogja állítani a szinkronizálást a beállításai. Ha a Windows 10-felhasználó, és Azure Active Directory-identitás bejelentkezett, akkor elkezdi szinkronizálni az windows beállításai, amint a rendszergazda lehetővé teszi, hogy a beállítások szinkronizálása Azure AD-e.

A vállalati eszközön tárolt személyes adatokat, ha meg kell szem előtt, hogy Windows operációs rendszer és az alkalmazás adatokat is kezdi a szinkronizálást az Azure Active Directory. Ez a következő szempontokat foglalja magában:

- A személyes Microsoft-fiók beállításainak eltolódás mellett a beállítások, a a munkahelyi vagy iskolai fiók Azure AD lesz. Ennek oka, hogy a Microsoft-fiók és Azure Active Directory-beállítások szinkronizálása most külön fiókot használ.
- A személyes adatok, például Wi-Fi jelszavak webes hitelesítő adatait, és az előzőleg szinkronizált keresztül csatlakoztatott Microsoft-fiókkal az Internet Explorer Kedvencek alkalmazás szinkronizálja Azure AD-e.


## <a name="how-do-microsoft-account-and-azure-ad-enterprise-state-roaming-interoperability-work"></a>Hogyan, Microsoft-fiók és Azure Active Directory vállalati állam központi interoperability munkát?
A Windows 10-es November 2015-ös vagy újabb verziókban vállalati állam barangoló csak a támogatott egy adott fiókhoz egyszerre. Ha Windows jelentkezzen be a munkahelyi vagy iskolai Azure Active Directory-fiókját, az összes adat Azure Active Directory keresztül szinkronizálva lesznek. Ha bejelentkezik Windows egy személyes Microsoft-fiók használatával, az összes adat a Microsoft-fiók keresztül szinkronizálva lesznek. Univerzális appdata hordozása-e a csak az elsődleges bejelentkezési fiók használata az eszközön, és meg fog hordozása csak akkor, ha az alkalmazás licencet az elsődleges fiók tulajdonában van. Az appok bármely másodlagos fiókok által birtokolt univerzális appdata nem lehet megnyitni.

## <a name="do-settings-sync-for-azure-ad-accounts-from-multiple-tenants"></a>Az Azure Active Directory-fiókok a több bérlők szinkronizálhatom beállítások?
Ha több Azure Active Directory-fiókok a különböző Azure AD-bérlők ugyanarra az eszközre, frissítenie kell az eszköz rendszerleíró minden Azure AD-bérlő az Azure Rights Management (Azure RMS) kommunikálni.  

1. Keresse meg a globálisan egyedi azonosítója minden Azure AD-bérlő. Nyissa meg az Azure klasszikus portált, és válassza az Azure AD-bérlő. A bérlő Globálisan URL-CÍMÉT a böngésző címsorában szerepel. Példa: `https://manage.windowsazure.com/YourAccount.onmicrosoft.com#Workspaces/ActiveDirectoryExtension/Directory/Tenant GUID/directoryQuickStart`
2. Után a globálisan egyedi azonosítója, meg kell adni a beállításkulcsot **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\SettingSync\WinMSIPC\<GUID azonosítója bérlői >**.
A **bérlői GUID azonosítója** kulcs hozzon létre egy új több karakterláncértéket (REG TÖBBSZÖRÖS, SZ) nevű **AllowedRMSServerUrls**. Az adatok adja meg a licencelés terjesztési pont az URL-címét a többi Azure bérlők, az eszköz hozzáférő.
3. A **Get-AadrmConfiguration** parancsmagot a licencelési terjesztési pont URL-címeit is megkeresheti. Ha az értékek **LicensingIntranetDistributionPointUrl** és **LicensingExtranetDistributionPointUrl** eltérő, adja meg mindkét értéket. Ha az érték azonos, csak egyszer adja meg a értéket.


## <a name="what-are-the-roaming-settings-options-for-existing-windows-desktop-applications"></a>Mik a már meglévő Windows asztali alkalmazások központi beállítások?
Barangoló csak akkor működik, az univerzális Windows-alkalmazások. Két lehetőség érhető el a meglévő Windows asztali alkalmazás központi engedélyezése:

- Az [Asztal híd](http://aka.ms/desktopbridge) segít a meglévő Windows asztali alkalmazások előtérbe az univerzális Windows platformra. Innen kiindulva minimális kód módosításai lesz szükség Azure AD-alkalmazás adatok barangoló előnyeit. Az asztal híd-alkalmazások identitással alkalmazás, amely szükséges ahhoz, hogy a meglévő asztali alkalmazások barangoló alkalmazás adatokat tartalmaz.
- [Felhasználói élmény virtualizációs (UE – V)](https://technet.microsoft.com/library/dn458947.aspx) segít a meglévő asztali Windows-alkalmazások egyéni beállítások sablont hozhat létre, és engedélyezze a központi Win32 alkalmazásai. Ez a beállítás nincs szükség az alkalmazás fejlesztője módosíthatja az alkalmazás-kódot. UE-V szolgáltatást úgy a helyszíni Active Directory barangoló olyan ügyfeleknek, akik a Microsoft asztali optimalizálási csomag vásárolt korlátozódik.

Rendszergazdák UE-V számára a Windows asztali alkalmazás adatok Windows operációs rendszer beállítások és a [UE-V csoport házirendek](https://technet.microsoft.com/itpro/mdop/uev-v2/configuring-ue-v-2x-with-group-policy-objects-both-uevv2), adatoknak univerzális alkalmazás központi módosításával beállíthatja többek között:

- A Windows beállításai csoportházirend hordozása
- Nem szinkronizálja a Windows-alkalmazások csoportházirend
- Az Internet Explorer barangoló alkalmazások szakaszában

A jövőben Microsoft előfordulhat, hogy vizsgálja meg a módszert, amellyel ellenőrizze a Windows rendszerbe szorosan integrálódik UE V és kiterjesztése UE-V számára a beállításokat az Azure Active Directory felhő keresztül.


## <a name="can-i-store-synced-settings-and-data-on-premises"></a>Tárolhatom szinkronizált beállítások és az adatok helyszíni?
Az Azure felhő vállalati állam barangoló az összes szinkronizált adatokat tárolja. UE-V kínálja-e egy helyszíni megoldás barangoló.

## <a name="who-owns-the-data-thats-being-roamed"></a>Ki a tulajdonosa van folyamatban forrásul adatokat?
Saját nagyvállalatoknak szolgáltatás részét képező keresztül vállalati állam barangoló forrásul az adatokat. Adatok egy Azure adatközponthoz vannak tárolva. Minden felhasználói adat titkosítva van, mind a hálózaton átvitt a többi az Azure Rights Management szolgáltatást használ, a felhőben. Ez a javulást titkosítja a például felhasználói hitelesítő adatokat csak bizonyos bizalmas adatokat, mielőtt az eszköz elhagyja a Microsoft ügyfélalapú beállítások szinkronizálása képest.

A Microsoft elkötelezett ügyféladatok védelme. Mielőtt elhagyja a Windows 10-es eszköz, így a többi felhasználó nem tudja olvasni ezeket az adatokat egy vállalati felhasználói beállítások adatok automatikusan titkosított által az Azure Rights Management szolgáltatást. Ha szervezete a fizetős verzióra az Azure Rights Management szolgáltatást, használata más Azure Tartalomvédelmi szolgáltatások, például a nyomon követése és visszavonása a dokumentumok, automatikusan védhetők e-mailek bizalmas információkat tartalmazó, és kezelheti a saját kulcsok (a "saját kulcsot hozása" megoldást, más néven BYOK). Ezeket a funkciókat és hogyan működik a Azure Tartalomvédelmi kapcsolatos további tudnivalókért olvassa el a [Mi az Azure Rights Management](https://technet.microsoft.com/jj585026.aspx)című témakört.

## <a name="can-i-manage-sync-for-a-specific-app-or-setting"></a>Kezelhetik a szinkronizálása egy adott alkalmazás vagy beállítás?
A Windows 10-ben nem letiltása, az egyes alkalmazás központi nincs MDM vagy csoportházirendet alkalmaz beállítást. Bérlői rendszergazdák letilthatja az appdata szinkronizálása az összes alkalmazás felügyelt eszközön, de nincs szabályozásához alkalmazás vagy belül alkalmazás szinten van.

## <a name="how-can-i-enable-or-disable-roaming"></a>Hogyan tudom engedélyezheti vagy letilthatja a központi?
A **Beállítások** alkalmazást, válassza a **fiókok** > **a beállítások szinkronizálása**. Ezen a lapon megtekintheti, hogy melyik fiók van használatban számára a beállításokat, és, engedélyezheti vagy letilthatja a beállításokat, hogy kell-e forrásul különálló csoportjait.

## <a name="what-is-microsofts-recommendation-for-enabling-roaming-in-windows-10"></a>Mi az a Microsoft ajánlási engedélyezésének barangoló a Windows 10-ben?
A Microsoft megoldások, beleértve a központi felhasználói profilok, UE-V és vállalati állam központi barangoló néhány különböző beállításokat tartalmaz.  A Microsoft kötelességének tekinti, hogy egy befektetés vállalati barangoló jövőbeli verzióiban Windows megye. Ha a szervezet nem áll készen vagy kényelmes az adatok áthelyezés a felhőbe, majd azt javasoljuk, hogy az elsődleges központi technológia UE-V szolgáltatást úgy használja. Ha a szervezet meglévő Windows asztali alkalmazások támogatása barangoló igényel, de az áthelyezés a felhőbe belevágna, azt javasoljuk, vállalati állam központi és UE-V szolgáltatást úgy is használható. Habár UE-V és a vállalati állam központi technológiák nagyon hasonlóak, nem egymást kölcsönösen kizáróak. Egymást érdekében győződjön meg arról, hogy a szervezet szolgáltatásai a központi, hogy a felhasználók kiegészíti.  

Vállalati állam barangoló és UE-V használata esetén az alábbi szabályok érvényesek:

- Vállalati állam barangoló az elsődleges központi ügynök az eszközön. UE-V szolgáltatást úgy van használatban kiegészítése "Win32 távolság."
- A Windows-beállítások és modern UWP alkalmazás adatok barangoló UE V kell kell kikapcsolja, ha a UE-V csoport használata házirendeket. Ezek már elfednek vállalati állam barangoló.

## <a name="how-does-enterprise-state-roaming-support-virtual-desktop-infrastructure-vdi"></a>Hogyan vállalati állam barangoló támogatja a virtuális asztali infrastruktúra (VDI)?
Vállalati állam barangoló támogatja a Windows 10-es ügyféleszközök termékváltozatok, de nem található a kiszolgálón termékváltozatok. Ha távolról jelentkezzen be a virtuális gép virtuális ügyfél hipervizor gépre üzemelteti, az adatok fog hordozása. Ha több felhasználó megoszthatja az azonos operációs rendszer, és a felhasználó távolról jelentkezzen be a kiszolgáló egy teljes asztali élmény érdekében, barangoló előfordulhat, hogy nem működnek. A munkamenet-alapú ez utóbbi esetben hivatalos nem támogatott.


## <a name="what-happens-when-my-organization-purchases-azure-rms-after-using-roaming"></a>Mi történik, ha a szervezet Azure Tartalomvédelmi vásárol barangoló használata után?
Ha szervezete már használja az Azure Rights Management szolgáltatást korlátozott használható ingyenes verzióra szóló előfizetéssel, a Windows 10-ben barangoló Azure Tartalomvédelmi kifizetett előfizetés vásárlásához nem lesz bármely hatását a a központi funkció működését, és konfigurációs módosítások nem kötelező, és a rendszergazda által.

## <a name="known-issues"></a>Ismert problémák

- Ha megpróbálja jelentkezzen be az intelligens kártya-virtuális intelligens kártya vagy Windows-eszközén, beállítások szinkronizálása használhatatlanná válnak. A Windows 10 jövőbeli frissítéseiben megoldhatja a problémát.
- Szüksége lesz az július összesítő frissítés a Windows 10 (build 10586.494 vagy újabb) az Internet Explorer Kedvencek szinkronizálását a munkát.
- Windows-információk védelmét védett adatok nem szinkronizálja keresztül vállalati állam barangoló. Ezeken kívül engedélyezve van a Windows-információk védelem rendelkező nem fog tapasztalni téma szinkronizálási. 
- Bizonyos körülmények vállalati állam barangoló lehet, hogy nem szinkronizálja az adataimat, ha Azure többtényezős hitelesítés van beállítva.
    - Ha az eszköz [Többtényezős hitelesítést](multi-factor-authentication.md) igényel az Azure Active Directory-portálon van konfigurálva, akkor meghiúsulhat, beállítások szinkronizálása a Windows 10-es eszköz jelszóval bejelentkezés közben. Többtényezős hitelesítés beállítása az ilyen típusú védelemmel Azure rendszergazdai fiókkal szolgál. Rendszergazdai felhasználók továbbra is szinkronizálhatja a bejelentkezés a PIN-kód [Microsoft Passport munkához](active-directory-azureadjoin-passport.md) a Windows 10 eszközökhöz, illetve többtényezős hitelesítés befejezése más Azure szolgáltatásokkal, például az Office 365 használata közben.
    - Ha a rendszergazda konfigurálja az Active Directory összevonási szolgáltatások többtényezős hitelesítés feltételes hozzáférési házirendet, és a hozzáférési jogkivonat, az eszközön lejár történhet a szinkronizálás.  Győződjön meg arról, hogy Ön jelentkezzen be, és jelentkezzen ki, használja a [Microsoft Passport munkához](active-directory-azureadjoin-passport.md) PIN-kód vagy többtényezős hitelesítés befejezéséhez más Azure szolgáltatásokkal, például az Office 365 használata közben.

- Ha a számítógép tartományhoz tartozó automatikus regisztrálása az Azure Active Directory-eszközök, azt tapasztalhatnak szinkronizálása sikertelen, ha a számítógép helyszínen hosszabb ideig, és nem sikerül elvégezni a tartomány hitelesítése. A probléma megoldásához csatlakozzon a gép a vállalati hálózathoz, hogy a szinkronizálás folytatása is.


## <a name="related-topics"></a>Kapcsolódó témakörök
- [Vállalati állam központi – áttekintés](active-directory-windows-enterprise-state-roaming-overview.md)
- [Engedélyezze az Azure Active Directory barangoló vállalati állam](active-directory-windows-enterprise-state-roaming-enable.md)
- [Csoport házirendek és a beállítások szinkronizálása MDM beállításai](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
- [A Windows 10 központi beállítások hivatkozás](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)

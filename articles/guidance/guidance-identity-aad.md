<properties
   pageTitle="Azure Active Directorynak |} Microsoft Azure"
   description="Hogyan lehet egy biztonságos hibrid hálózat architektúrája, használja az Azure Active Directory végrehajtásához."
   services="guidance,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="telmos"/>

# <a name="implementing-azure-active-directory"></a>Azure Active Directory végrehajtása

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Ez a cikk ismerteti a gyakorlati tanácsok a helyszíni Active Directory (AD) tartományok és erdők integrálása az Azure Active Directory felhőalapú identitás hitelesítését.

> [AZURE.NOTE] Azure van két különböző telepítési modellek: [Erőforrás-kezelő] [ resource-manager-overview] és klasszikus. A hivatkozás architektúra erőforrás-kezelő Microsoft javasolja új telepítési használja.

Könyvtár és -Identitáskezelés szolgáltatások, például az Active Directory-szolgáltatások (AD DS) identitások hitelesítést végezni által biztosított is használhatja. Ezek a identitások tartozhat felhasználók, számítógépek, alkalmazások és más erőforrások: a biztonsági oszlopazonosító jobboldali részét képező. Directory üzemeltetheti és identitás szolgáltatások a helyszíni, de a hol található az alkalmazások elemei Azure hibrid helyzetben ezt a funkciót, az a felhő meghosszabbítása hatékonyabbá tehető. Ezt a megközelítést csökkentheti az a késés okozta hitelesítési küldése, és a helyszíni futó könyvtár és -Identitáskezelés szolgáltatások biztonsági helyi kéri a felhőből.

Azure két megoldásokat könyvtár és -Identitáskezelés szolgáltatások végrehajtásához a felhőben:

1. [Azure Active Directory (AAD)] használható[ azure-active-directory] az Active Directory tartományi létrehozása a felhőben, és kapcsolja az elemet egy helyszíni Active Directory tartományi. AAD alkalmazások használatot a felhőben rendszert futtató felhasználók számára lehetővé teszi az egyszeri bejelentkezés (SSO) beállítása. AAD használja az [Azure AD Connect] [ azure-ad-connect] a helyszíni tárházba való replikáció, objektumok és identitások helyszíni futó tartott a felhőbe.

    A helyszíni címtárhoz használata nélkül AAD is alkalmazhat. Ebben az esetben AAD működik-e minden azonosító információkat, hanem egy helyszíni címtárból replikált adatokat tartalmazó elsődleges forrását.

    Megjegyzendő, hogy a könyvtár (a felhőben) **nem** egy helyszíni könyvtár kiterjesztését. Inkább objektumok és a identitások tartalmazó másolatot. Ezen elemek a helyszíni végrehajtott módosítások másolja a felhőbe, de az elvégzett módosításokat a felhőben **nem** lehet replikált vissza a helyszíni tartomány.

    >[AZURE.NOTE] Az Azure Active Directory Premium kiadásában támogatja a felhasználói jelszavak engedélyezése a helyszíni felhasználók a felhőben önkiszolgáló jelszó-visszaállítást végrehajtásához írási hátterében. Ez a AAD szinkronizálja a helyszíni címtárban való adatait. AAD és azok a funkciók a különböző kiadásai kapcsolatos további tudnivalókért lásd a [Azure Active Directory kiadásai][aad-editions].

2. A virtuális fut az Active Directory tartományi szolgáltatások Azure, a meglévő Active Directory-infrastruktúrát (beleértve az Active Directory tartományi szolgáltatások és AD FS) bővítése tartományvezérlőnek a helyszíni adatközponthoz üzembe azt. Ez a módszer gyakrabban, ahol a helyszíni és Azure virtuális hálózat csatlakoztatott készült ExpressRoute, illetve virtuális Magánhálózati kapcsolat által felhasználási területei. Ez a megoldást is támogat a kétirányú replikációs, amely lehetővé teszi a módosítások elvégzése a felhőben, és a helyszíni, ahol nem leginkább megfelelő.

Ez a felépítés 1 megoldás koncentrál. A második megoldást kapcsolatos további tudnivalókért lásd a [Azure Active Directory bővítése][guidance-adds].

Ez a felépítés esettel jellemző használata a következők:

- Egyszeri bejelentkezés használata a hitelesítő adatait, adja meg, hogy a helyszíni környezetbe applications a felhőben, szoftver- és egyéb alkalmazások operációs rendszert futtató végfelhasználók kezeléséről.

- Közzététel a helyszíni webalkalmazások ahhoz, hogy az access a szervezet tartozó távoli felhasználók a felhőben keresztül.

- A végrehajtási önkiszolgáló funkciók a végfelhasználók számára, például a jelszó alaphelyzetbe állítása és csoportok kezelése delegálása. 

    >[AZURE.NOTE] Ezek a funkciók a rendszergazda csoportházirenden keresztül vezérelhető.

- Azokról a helyzetekről, amikor a a helyszíni és felhőbeli alkalmazások szolgáltatója Azure virtuális hálózat nem kapcsolódnak közvetlenül VPN-csatorna vagy készült ExpressRoute áramkör használatával.

Ne feledje, hogy AAD nem ad az Active Directory a funkciókat. Ha például AAD jelenleg csak kezeli a számítógép-hitelesítés helyett felhasználói hitelesítéssel. Egyes alkalmazások és szolgáltatások, például SQL Server megkövetelheti számítógép-hitelesítés, amely esetben a megoldás értéke nem megfelelő. Ezenkívül AAD előfordulhat, hogy nem alkalmas rendszerek, ahol összetevők végig az a – helyi és felhőbeli oszlopazonosító tudta áttelepíteni, hanem csak a másolt.

> [AZURE.NOTE] Megtekintés részletes leírását működése az Azure Active Directory, a [Microsoft Active Directory magyarázata][aad-explained].

## <a name="architecture-diagram"></a>Architektúra diagramja

Az alábbi ábra kiemeli a fontos e architektúra összetevőket. Azure terhelést elemeinek kapcsolatos további tudnivalókért olvassa el [Az Azure-N szintű architektúrája operációs rendszert futtató VMs][implementing-a-multi-tier-architecture-on-Azure]:

> [AZURE.NOTE] Az egyszerűség kedvéért ez az ábra csak a kapcsolatok AAD közvetlenül kapcsolódó mutatja, illetve nem adja vissza web böngészővel kérelem átirányítások, vagy más protokollt, akkor fordulhat elő a hitelesítési és -Identitáskezelés összevonási folyamat részeként kapcsolódó forgalmat. Például a felhasználó (helyi vagy távoli) általában fér hozzá egy webalkalmazás böngészőn keresztül, és előfordulhat, hogy a web app átlátszó átirányítása a böngészőn keresztül AAD hitelesítést végezni. Hitelesítését követően a kérelem juthat el újra a web app és a megfelelő azonosító információkat.

[! [0]][0]

- **Azure Active Directory-bérlői**. Ez a szervezet által létrehozott AAD egy példánya. Akkor működik-e egy egyszerű címtár-szinkronizálás eszközének felhő alkalmazások (tart fenn, amelyet a helyszíni az objektumok AD), és identitás szolgáltatásokat nyújt.

- **Webes réteg alhálózat**. Az alhálózathoz tárolja, amelyek esetében, amelyek AAD működhet az identitás-ügynök, illetve a szervezet által készített egyéni felhőalapú alkalmazás megvalósítása VMs.

- **a helyszíni Active Directory tartományi szolgáltatások kiszolgálón**. Szervezete valószínűleg már van egy meglévő címtárszolgáltatásába, például az Active Directory tartományi szolgáltatások. Az elemek közül számos szinkronizálhatja az Active Directory tartományi szolgáltatások címtárban, (például a felhasználó vagy csoport információk) AAD ahhoz, hogy AAD ezen információk segítségével identitások hitelesítést végezni.

- **Azure AD Connect szinkronizálási kiszolgáló**. Ez a egy helyszíni futtató az Azure AD Connect szinkronizálási szolgáltatás. Ez a szolgáltatás telepítése az Azure AD Connect szoftverrel. Az Azure AD Connect szinkronizálási szolgáltatás szinkronizálja a helyszíni tárolt információk (felhasználók, csoportok, névjegyek, stb.) AD a AAD a felhőben. Például kiépítése vagy a felhasználók és csoportok a helyszíni deprovision és a módosítások átkerülnek AAD. Az Azure AD Connect szinkronizálási szolgáltatás információt a AAD bérlő továbbítja.

    >[AZURE.NOTE] Biztonsági okokból felhasználói jelszavak nem közvetlenül a AAD tárolja. Ehelyett AAD mentesítésről kivonat minden jelszót. Ez a elegendő ahhoz, hogy ellenőrizze a jelszavát. Ha egy felhasználó a jelszó alaphelyzetbe állítása igényel, ennek elvégzett a helyszíni és az új kivonat AAD küldött kell lennie. AAD prémium kiadások tartalmazzák az szolgáltatások automatizálhatja ezt a feladatot engedélyezése a felhasználóknak saját jelszavak alaphelyzetbe állítása.

## <a name="recommendations"></a>Javaslatok

Ebben a szakaszban a következőképpen végrehajtási AAD javaslatok foglalja össze.

- AD Connect
- Biztonsági

### <a name="azure-ad-connect-sync-service-recommendations"></a>Azure AD Connect szinkronizálási szolgáltatás javaslatok

Szinkronizálás érintett biztosítva, hogy a felhasználók azonosító információkat a felhőben, amely a helyben tárolt követik. Az Azure AD Connect szinkronizálási szolgáltatás célja megőrzéséhez a bemutatókat. A következő pontok javaslatok az Azure AD Connect szinkronizálási szolgáltatás üzembe Összefoglalva:

- Azure AD Connect szinkronizálási végrehajtása, előtt meg kell határoznia a szinkronizálást a szervezet igényeivel. (Mit szeretne szinkronizálni, mely tartományokból és milyen gyakran. A cikk [meghatározása címtár-szinkronizálási követelmények] [ aad-sync-requirements] mutatja be a címzett pontok is érdemes figyelembe venni.

- Azure AD Connect szinkronizálási szolgáltatás egy virtuális futtatását is lehetővé teszi, vagy a számítógépen futó helyszíni. Attól függően, hogy az adatokat az Active directory volatilitásától Miután a kezdeti szinkronizálás AAD végeztek a betöltés az Azure AD Connect szinkronizálási szolgáltatása valószínű, hogy jó. Egy virtuális használatával lehetővé teszi, hogy könnyebben méretezheti a kiszolgáló (a tevékenység a [Figyelés előtt megfontolandó kérdések](#monitoring-considerations) szakaszban leírt módon-képernyő határozza meg, hogy szükség-e meg).

- Esetén több a helyszíni tartomány erdőben tárolhatja és szinkronizálni az adatokat az egész erdő egyetlen AAD bérlői webhelyre (Ez a javasolt). Egynél több tartományban, hogy minden egyes identitás csak egyszer AAD, hanem az éppen létrejön egy újabb példánya Ez vezethet következetlenségekre, amikor a rendszer szinkronizálja az adatokat, megjelenik a előforduló identitások információk szűréséhez. Ez a módszer igényli több Azure AD Connect szinkronizálási kiszolgálók végrehajtása. További tudnivalókért lásd: a több AAD forgatókönyv a [topológia előtt megfontolandó kérdések](#topology-considerations) szakaszban. 

- Szűréssel szeretné korlátozni, amely csak az AAD tárolt adatok szükséges. Ha például a szervezet esetleg nem kívánt AAD az inaktív és nem személyes ügyfelek adatainak tárolása. Szűrés lehet, hogy csoport, tartomány, szervezeti vagy attribútum-alapú, és egyesítheti a szűrők összetettebb szabályok létrehozásához. Ha például sikerült válassza csak azokat az objektumokat tartományban tartani, hogy van-e egy adott értéket a kijelölt attribútum szinkronizálása. Részletes információ [Azure AD Connect szinkronizálása: szűrés konfigurálása][aad-filtering].

- Alkalmazza a AD Connect szinkronizálási szolgáltatás nagy elérhetőségét, futtassa a másodlagos átmeneti tárolásra szolgáló kiszolgálót. További információért [topológia szempontok](#topology-considerations) című.

### <a name="security-recommendations"></a>Javaslatok biztonsági

Az alábbi elemek az elsődleges biztonsági javaslatok végrehajtási AAD, attól függően, hogy a szervezet igényeivel Összefoglalva:

- Felhasználói jelszavak kezelése.

    Egy AAD prémium kiadásának használatakor Azure AD Connect az egyéni telepítés végrehajtásával engedélyezheti jelszó írási-vissza AAD a helyszíni címtárában:

    [! [9]][9]

    Ez a szolgáltatás lehetővé teszi a felhasználóknak saját belül az Azure-portálra a jelszavak alaphelyzetbe állítása, de csak engedélyezhető a szervezet biztonsági jelszóházirend ellenőrzése után. Például korlátozhatja, hogy melyik felhasználója képes legyen módosítani a jelszavát, és a jelszó adatkezelési folyamatok szabhatja. További tudnivalókért lásd: a [Jelszavak kezelését testreszabása a szervezet igényei][aad-password-management].

- Külső felhasználókkal is elérhető a helyszíni alkalmazásokhoz védelem fenntartása.

    Az Azure Active Directory-alkalmazás Proxy segítségével a helyszíni webalkalmazások ellenőrzött hozzáférést biztosítanak a külső felhasználók AAD keresztül. Ez a módszer az alkalmazások úgy, hogy nem közvetlenül az interneten közzéteszi védi. Csak azok a felhasználók érvényes hitelesítő adatokkal rendelkező Azure címtárában is tudják az alkalmazások eléréséhez. További tudnivalókért olvassa el [Szolgáltatásalkalmazás-Proxy engedélyezése az Azure-portálon][aad-application-proxy].

- A gyanús tevékenység jeleinek aktívan figyelése a AAD című témakört.

    Fontolja meg inkább edition AAD prémium P2 AAD identitás védelem tartalmazó. Identitás védelem rendellenességeinek észleli és kockázati esemény, amelyek jelezheti, hogy az identitás napvilágra kerülhetett adaptív gépi tanulási algoritmusok és heurisztikus használja. Ha például felismeri a potenciálisan anomalous tevékenységét, például szabálytalan bejelentkezési, bejelentkezési bővítmények ismeretlen forrásból, vagy a gyanús tevékenység IP-címek vagy tevékenységek bejelentkezési bővítmények, lehet, hogy eszközökről. Ezeket az adatokat használ, identitás védelem hoz létre, jelentések és figyelmeztetések, amelyek lehetővé teszi, hogy vizsgálja meg ezeket a kockázat események és készíthet remediation megfelelő vagy a kezelési műveletet. További tudnivalókért lásd: az [Azure Active Directory identitás védelem][aad-identity-protection].

    Az Azure-portálon AAD a jelentéskészítési funkciót a Lync-belül a rendszer előforduló gyanús és más biztonsággal kapcsolatos műveletek is használhatja. AAD sorozata összesítő jelentéseket hozhat létre:

    [! [17]][17]

    A jelentések használatával kapcsolatos további tudnivalókért lásd: az [Azure Active Directory jelentéskészítés útmutató][aad-reporting-guide].

## <a name="topology-considerations"></a>Topológia kapcsolatos szempontok

Ha van egy helyszíni könyvtár integrálása AAD, állítsa be az Azure AD Connect megvalósítását a topológia leginkább megfelelő a szervezet igényeivel.. Azure AD Connect támogató topológiák közé tartoznak az alábbiak:

- **Egyetlen forest, egyetlen AAD címtár**. Ebben a topológiában Azure AD Connect használatával az objektumok szinkronizálása, és egy egy vagy több tartományt azonosító adatokat a helyszíni akinek egyetlen AAD bérlői webhelyre. Ez az alapértelmezett topológia végrehajtott Azure AD Connect express telepítése.

    [! [1]][1]

    Több Azure AD Connect szinkronizálási kiszolgálók ugyanabban a helyszíni tartományban, különböző tartományok csatlakozhat AAD ugyanahhoz a bérlőhöz, kivéve, ha futtatja az Azure AD Connect szinkronizálási kiszolgáló átmeneti tárolására mód nem hoz létre (lásd: a kiszolgáló átmeneti tárolására alább).

- **Több erdők, egyetlen AAD címtár**. A topológia használata, ha egynél több helyszíni erdő van. Is összevonhatja az azonosító információkat, hogy az egyes egyedi felhasználói egyszer jeleníti meg a AAD könyvtárban, akkor is, ha az adott felhasználó egynél több erdőben. Az összes erdők azonos Azure AD Connect szinkronizálási kiszolgáló használata. Az Azure AD Connect szinkronizálási kiszolgáló nem kell minden olyan tartomány része, de legyen elérhető összes erdőkből:

    [! [2]][2]

    Ebben a topológiában nem használja az külön Azure AD Connect szinkronizálása kiszolgálókon való csatlakozáshoz minden egyes helyszíni erdőben egyetlen AAD bérlői webhelyre. Ez eredményezhet AAD identitást másolt adatokat, ha egynél több erdő felhasználók.

- **Több erdők, külön topológiák**. Ezt a megközelítést lehetővé teszi, hogy külön erdőkből azonosító adatok egyesítése egyetlen AAD bérlői webhelyre. Ez a beállítás akkor hasznos, ha a együttes megtekintsék más erdők (egyesülés vagy, pl. WIA) után alkalmazása, és csak egy erdőben tartják minden felhasználó számára az azonosító információkat:

    Ha a rendszer szinkronizálja a globális címlisták minden egyes erdőben, akkor lehet, hogy egy felhasználó az egyik erdő bemutató egy másik partnerként. Ez akkor fordulhat elő, ha például a szervezet végrehajtott GALSync Forefront identitáskezelő 2010 vagy Microsoft Identitáskezelő 2016-ban. Ebben az esetben megadhatja, hogy felhasználók által a *levelezési* attribútum kell azonosítania kell magát. Használja a *ObjectSID* és *msExchMasterAccountSID* attribútumok identitások is összevetheti. Ez akkor hasznos, ha van egy vagy több erőforrás erdők a letiltott partnerek.

- A **kiszolgáló átmeneti**. Ebben a konfigurációban a másodfokú az Azure AD Connect szinkronizálási kiszolgáló és az első párhuzamosan futtatása. Ez a struktúra esetek például támogatja:

    - Magas elérhető.

    - Tesztelés, és egy új a konfigurációja Azure AD Connect szinkronizáló telepítése.

    - Az új kiszolgáló bemutatása, és leszerelje egy régi beállítást. 

    A másodfokú forgatókönyvekben fut *átmeneti tárolására módban*. A kiszolgáló importált objektumok és szinkronizálás adatok rekordok az adatbázisban, de nem felel meg az adatok AAD. Csak akkor, ha letiltja a fejlesztői üzemmód nem a kiszolgáló kezdje el beírni adatok AAD és is indítást végrehajtása jelszó írási-biztonsági készít a helyszíni könyvtárak az adott esetben:

    [! [4]][4]

    További tudnivalókért lásd: [Azure AD Connect szinkronizálása: műveleti feladatok és szempontok][aad-connect-sync-operational-tasks].

- **Több AAD könyvtárak**. Javasoljuk, hogy hoz létre a szervezet egyetlen AAD könyvtárában, de lehetnek olyan esetek, amikor hol kell partíciót adatokat külön AAD könyvtárak keresztül. Ebben az esetben célszerű ellenőrizze, hogy látható-e a helyszíni erdőből egyes objektumokra csak az egyik AAD könyvtár, szinkronizálása és a jelszavát írási-vissza problémák elkerülése érdekében.

    Ebben az esetben végrehajtásához külön Azure AD Connect konfigurálása szinkronizálása minden AAD címtár-kiszolgálók és szűréssel, így minden Azure AD Connect szinkronizálási kiszolgáló objektumok egymást kölcsönösen kizáró csoportja a működik: 

    [! [5]][5]

Az alábbi topológiák kapcsolatos további tudnivalókért lásd: [topológiák az Azure AD Connect][aad-topologies].

## <a name="user-sign-in-considerations"></a>Felhasználónak bejelentkezés előtt megfontolandó kérdések

Alapértelmezés szerint a AAD szolgáltatás feltételezi, hogy felhasználók bejelentkezés az azáltal, hogy ugyanazt a jelszót, hogy használják a helyszíni és az Azure AD Connect szinkronizálási kiszolgáló állítja be a jelszó-szinkronizálás a helyszíni tartomány és a AAD között. Sok szervezetben szükség, de érdemes lehet a szervezet meglévő házirendek és infrastruktúra. Példa:

- A biztonsági házirendet a szervezet tilthatják előfordulhat, hogy a szinkronizálás a felhőbe jelszó tiltva.

- Előfordulhat, hogy a felhasználók (nélkül kér, további jelszó) tapasztalható zökkenőmentes SSO amikor gépeken futó felhőalapú erőforrások elérése tartományból csatlakozott a vállalati hálózaton.

- Előfordulhat, hogy a szervezet már ADFS vagy harmadik fél összevonási szolgáltató rendszerbe. Ez az infrastruktúra megvalósítása hitelesítésről és az egyszeri bejelentkezés helyett a felhőben tárolt jelszóadatokat használatával használandó AAD is beállíthatja.

További tudnivalókért lásd: az [Azure Active Directory csatlakozás bejelentkezési beállításai][aad-user-sign-in].

## <a name="azure-ad-application-proxy-considerations"></a>Azure Active Directory alkalmazás proxy kapcsolatos szempontok

Azure Active Directory segítségével a helyszíni környezetbe applications hozzáférést biztosít.

- A helyszíni webalkalmazások használata az összekötők kezelheti az Azure Active Directory-alkalmazás proxy összetevő szolgáltatásalkalmazás-proxy jelenítik meg. Az alkalmazás proxy összekötő megnyílik az Azure Active Directory-alkalmazás proxy egy kimenő hálózati kapcsolat. Távoli felhasználói kérések webalkalmazások résztvevőkhöz vissza az AAD ezen a kapcsolaton keresztül. Ez az eljárás szükséges a nyissa meg a tűzfal, a szervezet által elérhetővé tett homonimaszerű webcímmel felület csökkentése bejövő portokat eltávolítja.

További tudnivalókért lásd: a [Közzététel-alkalmazások használata az Azure Active Directory szolgáltatásalkalmazás-proxy][aad-application-proxy].

## <a name="object-synchronization-considerations"></a>Objektum szinkronizálás előtt megfontolandó kérdések

Az alapértelmezett beállítása Azure AD Connect-címtárból a helyi Active Directory a cikkben meghatározott szabálykészlet alapján objektumok szinkronizálása [Azure AD Connect szinkronizálása: az alapértelmezett beállítások ismertetése][aad-connect-sync-default-rules]. Csak olyan objektumok, amelyek megfelelnek a szabályok vannak szinkronizálva, mások figyelmen kívül hagyja. Például felhasználói objektumok egyedi *sourceAnchor* attribútummal kell rendelkeznie, és a *accounEnabled* attribútum kell feltölteni. Felhasználói objektumok, amelyek nem rendelkezik egy *sAMAccountName* attribútummal vagy kezdődő szöveg *AAD_* vagy *MSOL_* nem szinkronizálódnak. Azure AD Connect több szabályok felhasználói, a partner, a csoport, a ForeignSecurityPrincipal és a számítógép objektumokra vonatkozik. Ha az alapértelmezett szabálykészletet módosítani kell a szerkesztővel szinkronizálási szabályok telepítve van az Azure AD Connect (is dokumentált [Azure AD Connect szinkronizálási: az alapértelmezett beállítások ismertetése][aad-connect-sync-default-rules]).

A saját szűrők korlátozhatja a tartomány vagy szervezeti szinkronizálandó az objektumok határozhatja meg. Illetve végrehajtja a összetettebb egyéni szűrés, ismertetett módon [Azure AD Connect szinkronizálása: szűrés konfigurálása][aad-filtering].

## <a name="security-considerations"></a>Biztonsági megfontolások

Feltételes hozzáférés-vezérlés segítségével hitelesítési kérések megtagadja váratlan forrásból:

- [Azure többtényezős hitelesítés (MFA)] elindítása[ azure-multifactor-authentication] Ha egy felhasználó nem megbízható helyről csatlakozni próbál (például az interneten keresztül) egy megbízható hálózati nem pedig annak.

- Az eszköz platform típusát a felhasználó (iOS, Android és Windows Mobile-Windows) segítségével határozza meg az access-alkalmazások és szolgáltatások házirend.

- Rögzítse a felhasználók eszközalapú engedélyezett/letiltott állapotát, és ezt az információt részévé tenni a hozzáférési házirend ellenőrzése. Például ha egy felhasználó telefon elveszett vagy ellopott meg kell jegyezni való hozzáféréshez használt megakadályozásához tiltva.

- Vezérlőelem egy felhasználóhoz, csoporttagság alapján hozzáférési szintet. [AAD dinamikus tagsági] szabályokkal[ aad-dynamic-membership-rules] felügyelete csoport egyszerűsítése érdekében. Rövid áttekintését hogyan működik, című témakörben [-csoportok dinamikus tagságot][aad-dynamic-memberships].

- AAD identitás védelemmel feltételes hozzáférés kockázata házirendek segítségével szokatlan bejelentkezési tevékenységeket vagy más események alapján speciális védelmet nyújtanak.

További tudnivalókért lásd: az [Azure Active Directory feltételes access][aad-conditional-access].

## <a name="scalability-considerations"></a>Méretezhetőség kapcsolatos szempontok

Méretezhetőség a AAD szolgáltatás és az Azure AD Connect szinkronizálási kiszolgáló konfigurációjától címzettjei:

- A AAD szolgáltatás nincs konfigurálása méretezhetőség végrehajtásához beállításokat. A AAD szolgáltatás lehetővé teszi a példányon alapú méretezhetőség. AAD egyetlen elsődleges replikát, mely fogópontok írási műveletek, és több csak olvasható másodlagos kópiák alkalmazza. AAD átlátszó átirányítja az elsődleges replikával másodlagos kópiák terhére kísérlete írások. AAD esetleges konzisztencia biztosít. Az elsődleges replika minden változtatás a másodlagos replikák átkerülnek. Mivel a legtöbb olyan AAD elleni művelet helyett írás olvasása, ez a felépítés és méretezze át.

    További tudnivalókért lásd: [Azure Active Directory: a motorháztető alatt fülre a geo felesleges, könnyen hozzáférhető, elosztott felhő könyvtár][aad-scalability].

- Az Azure AD Connect szinkronizálási kiszolgáló meg kell határoznia, valószínűleg szinkronizálni a helyi címtárból hány objektumok. Ha legfeljebb 100 000 objektumok, az alapértelmezett SQL Server Express LocalDB szoftverre az Azure AD Connect is használhatja. Ha nagyszámú objektumok, gyártási verzió az SQL Server telepítése, és végezze el az Azure AD Connect, adja meg, hogy meg kell használnia az SQL Server meglévő példány egyéni telepítése.

## <a name="availability-considerations"></a>Elérhetőség kapcsolatos szempontok

Méretezhetőség aggályokat, az elérhetőség az AAD szolgáltatás és Azure AD Connect konfigurációja fogja át:

- A AAD szolgáltatás magas rendelkezésre állás lett tervezve. Nincsenek felhasználó konfigurálható elérhetősége beállítások. Geo meghatározva, és több adat futtatja a világon a automatizált feladatátvételi várható középre igazítása. Ha egy adatközpont elérhetetlenné válik, AAD biztosítja, hogy a címtár-adatok elérhető példány eléréséhez, az legalább két a további regionális szétszórt adatközpontokban.

    >[AZURE.NOTE] A SZOLGÁLTATÁSISZINT-AAD egyszerű és támogatási szolgáltatások garanciákkal legalább 99,9 % elérhetőségét. Nincs szolgáltatásiszint-szerződés a AAD az ingyenes réteg számára nem. További tudnivalókért lásd: [az Azure Active Directory SLA][sla-aad].

- Azure AD Connect szinkronizálási kiszolgáló elérhetőségét növeléséhez futtathatja a másodfokú átmeneti tárolására módban, a [topológia előtt megfontolandó kérdések](#topology-considerations) szakaszban leírt módon. 

    Ezenfelül, ha nem használ az, hogy az Azure AD Connect az SQL Server Express LocalDB-példányt, majd vegye figyelembe magas elérhetőség az SQL Server. Ne feledje, hogy a magas csak elérhetőség megoldást támogatott SQL fürtözés. Megoldások például tükrözési és a mindig a Azure AD Connect által nem támogatott.

    A Azure AD Connect szinkronizálási kiszolgáló, és hogy hogyan állíthatja helyre egy hiba után az elérhető fenntartása kapcsolatos további szempontok című témakörben talál [Azure AD Connect szinkronizálása: műveleti feladatok és szempontok - vészhelyreállítás][aad-sync-disaster-recovery].

## <a name="management-considerations"></a>Adatkezelési kapcsolatos szempontok

Van két szempontból lehet AAD kezelése:

1. Felügyelt AAD a felhőben.

2. Az Azure AD Connect szinkronizálási kiszolgálók fenntartása.

AAD a tartományok és a felhőben könyvtárak kezelésére szolgáló a következő lehetőségeket nyújtja:

- [Azure Active Directory PowerShell-modult][aad-powershell]. Ha módosítani szeretné a gyakori Azure Active Directory felügyeleti feladatok parancsfájl például a felhasználók kezelése, tartománykezelés és az egyszeri bejelentkezés beállítása a modul használatát.

- Azure Active Directory kezelése a lap az Azure-portálon. Ez a lap egy interaktív management nézetben jeleníti meg a könyvtár, és lehetővé teszi, hogy határozzák meg, és állítsa be a legtöbb jellemzőjét AAD.

    [! [10]][10]

Azure AD Connect telepíti, az alábbi eszközök, amelyek segítségével az Azure AD Connect szinkronizálási szolgáltatásai a helyszíni gépek karbantartása:

- A Microsoft Azure Active Directory csatlakozás konzol. Ez az eszköz lehetővé teszi az Azure AD-szinkronizálás kiszolgáló konfigurációjától módosítása hogyan szinkronizálást, engedélyezése vagy fejlesztői üzemmód letiltása és testreszabása válthat a felhasználó bejelentkezési mód (engedélyezheti az AD FS-bejelentkezés használatáról a helyszíni infrastruktúra).

- A szinkronizálás szolgáltatáskezelő. Az eszközben a *Műveletek* lap használatával kezelheti a szinkronizálást, és észleli, hogy bármely részét a folyamat nem sikerült. Szinkronizálás manuálisan ezzel az eszközzel válthat. 

    [! [12]][12]

    Az *összekötők* lap lehetővé teszi a kapcsolatok, a tartományok kezelése (helyszíni és felhőbeli) a szinkronizálási motor csatolva van:

    [! [13]][13]

-  A szinkronizálás szabályok szerkesztőben. Ez az eszköz segítségével testre szabhatja, amelyben objektum egy helyszíni könyvtár és a felhőben AAD közötti másolásakor átalakítási. Ez az eszköz lehetővé teszi, hogy adja meg a további attribútumok és objektumok-szinkronizálás, és a szűrők előfordulások határozza meg kell, vagy nem szinkronizálódnak.

    További információ című szinkronizálási szabály szerkesztő a dokumentum [Azure AD Connect szinkronizálása: az alapértelmezett beállítások ismertetése][aad-connect-sync-default-rules].

Az oldal [Azure AD Connect szinkronizálása: gyakorlati tanácsok az alapértelmezett beállítások módosításával] [ aad-sync-best-practices] tartalmaz további információkat és Azure AD Connect kezelésével kapcsolatos tippek.

## <a name="monitoring-considerations"></a>Megfigyeléssel kapcsolatos szempontok

Rendszerállapot figyelése a helyszíni telepített ügynökök adatsorokkal végrehajtott:

- Azure AD Connect ügynökkel, amely jellemzi a szinkronizálás adatait telepíti. Az Azure-portálon az Azure Active Directory csatlakozás rendszerállapot lap segítségével az állapot és Azure AD Connect teljesítményének figyelése. További tudnivalókért lásd: [Segítségével Azure Active Directory csatlakozás állapota szinkronizálás][aad-health].

- Lync-állapota az Active Directory tartományi szolgáltatások tartományok és az Azure könyvtárak, telepítse az Azure Active Directory csatlakozás állapota az Active Directory tartományi szolgáltatások ügynök belül a helyszíni tartomány egy gépen. Az Azure Active Directory csatlakozás rendszerállapot lap használja az Azure Active Directory tartományi szolgáltatások monitor portal. További tudnivalókért lásd: [Segítségével Azure Active Directory csatlakozás állapota az Active Directory tartományi szolgáltatások][aad-health-adds]

- Telepítse az Azure Active Directory csatlakozás állapot AD FS-Agent futó helyszíni Active Directory összevonási szolgáltatások állapotát jelző figyelheti, és az Azure Active Directory csatlakozás rendszerállapot lap az Azure-portálon használatával figyelheti az Active Directory tartományi szolgáltatások. További tudnivalókért olvassa el a [Használatával Azure Active Directory csatlakozás állapota az AD FS] című témakört.[aad-health-adfs]

Az Active Directory csatlakozás állapot ügynökök és igényeik telepítésével kapcsolatos további tudnivalókért lásd: az [Azure Active Directory csatlakozás állapot ügynök telepítése][aad-agent-installation].

## <a name="sample-solution"></a>Minta megoldás

Ez a szakasz dokumentumok megoldás kiépítése olyan minta tesztelje a konfigurációt a AAD használható lépéseket. A megoldás a következő elemekből áll:

- Egy olyan n szintű webalkalmazást, a szerkezet [VMs fut az Azure-N szintű architektúrája]leírt követő[implementing-a-multi-tier-architecture-on-Azure].

- A próba ügyfélgép. Azt javasoljuk, hogy egy másik virtuális használata ezen a számítógépen. A virtuális konfigurációja lényegtelen, feltéve rendszergazdai hozzáférése van, és az n szintű webalkalmazás csatlakozhat.

- Egy szimulált a helyszíni környezettel, amely tartalmazza a saját tartomány beépített Active Directory tartományi szolgáltatások használatával.

Az jelenik meg, amelyek bemutatják a lépéseket a következők:

- Hozzáférés a felhőben futó külső felhasználókat, hogy a jelszó-hitelesítést nyújtó AAD n szintű webalkalmazás.

- Hozzáférés a szervezeten belül, a jelszó-hitelesítést nyújtó AAD operációs rendszert futtató felhasználók a felhőben futó n szintű webalkalmazás.

### <a name="prerequisites"></a>Előfeltételek

A következő lépések feltételezik, az alábbi előfeltételek:

- Ha egy meglévő Azure előfizetésbe erőforrás csoportokat hozhat létre.

- Telepítve van az [Azure parancssor][azure-cli].

- Letöltött és a legutóbbi szerkesztés telepítve. Lásd: [Itt] [ azure-powershell-download] utasításokat.

- Telepítve van egy példányát a [makecert] [ makecert] ügyfél tesztszámítógépen segédprogramot.

- Fejlesztési számítógépre, amelyen telepítve van a Visual Studio hozzáférése van.

### <a name="create-the-n-tier-web-application-architecture"></a>Az n szintű webes alkalmazás architektúra létrehozása

1. Hozza létre a `Scripts` , amely tartalmaz egy almappát `Parameters`.

2. Töltse le a [Központi telepítés-ReferenceArchitecture.ps1] [ solution-script] PowerShell-parancsprogramot a parancsfájlok mappát.

3. Hozzon létre egy másik almappát Windows a paraméterek mappában.

4. Paraméterek és Windows-mappába, töltse le a következő fájlokat:

    - [virtualNetwork.parameters.json][vnet-parameters-windows]

    - [networkSecurityGroup.parameters.json][nsg-parameters-windows]

    - [webTierParameters.json][webtier-parameters-windows]

    - [businessTierParameters.json][businesstier-parameters-windows]

    - [dataTierParameters.json][datatier-parameters-windows]

    - [managementTierParameters.json][managementtier-parameters-windows]

5. A **Központi telepítés-ReferenceArchitecture.ps**1 fájl szerkesztése a a parancsfájlok mappát, és módosítsa a következő sort az erőforráscsoport kell létrehozott vagy a virtuális és a parancsfájl által létrehozott erőforrások tárolására szolgáló megadásához:

        ```powershell
        # PowerShell
        $resourceGroupName = "ra-aad-ntier-rg"
        ```

6. Szerkessze a következő fájlokat a **Paraméterek/ablak**s mappát, és állítsa a `resourceGroup` az az érték a `virtualNetworkSettings` szakasz minden ezeket a fájlokat lehet ugyanaz, mint a **Központi telepítés-ReferenceArchitecture.ps1** parancsprogram megadott.

    - networkSecurityGroup.parameters.json

    - webTierParameters.json

    - businessTierParameters.json

    - dataTierParameters.json

    - managementTierParameters.json

7. Nyisson meg egy Azure PowerShell ablakot, helyezze át a parancsfájlok mappába, és futtassa a következő parancsot:

        ```powershell
        .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Windows ntier
        ```

    Csere **<subscription id>** az Azure előfizetés azonosítójával.

    A **<location>**, adjon meg egy Azure régiót, például *eastus* vagy *westus*.

8. Amikor befejeződött a parancsfájlt, az Azure portal segítségével szerezze be a nyilvános IP-címét a webhely szintű terheléselosztó (*TS-aad-ntier-web-lb*):

    [! [a 18]][18]

9. Jelentkezzen be a próba ügyfél számítógép (vagy egy virtuális), és ellenőrizze, hogy a webes réteg tallózással keresse meg a webes szintű terheléselosztó nyilvános IP-címét az Internet Explorer segítségével elérheti. Az alapértelmezett IIS lapján jelenjen meg.

### <a name="simulate-configuration-of-a-public-web-site"></a>Hasonlóan a nyilvános webhely beállítása

>[AZURE.NOTE] Az alábbi lépésekkel szimulálhatja a webes réteg társítása a DNS-neve www.contoso.com a hosts fájl ügyfél tesztszámítógépen módosításával. A webes szintű VMs ezenkívül önaláírt tanúsítványok és a hitelesítésszolgáltató szolgáltatója hamis van beállítva. Ez a célra csak és **, ne használja ezeket technikákat üzemi környezetben**.

1. A próba ügyfélszámítógépen **C:\Windows\System32\drivers\etc\hosts** fájl szerkesztése a Jegyzettömb használatával, és egy bejegyzést, amely összekapcsolja a név www.contoso.com a webes szintű terheléselosztó nyilvános IP-címe:

        ```text
        # Copyright (c) 1993-2009 Microsoft Corp.
        #
        # This is a sample HOSTS file used by Microsoft TCP/IP for Windows.
        #
        # This file contains the mappings of IP addresses to host names. Each
        # entry should be kept on an individual line. The IP address should
        # be placed in the first column followed by the corresponding host name.
        # The IP address and the host name should be separated by at least one
        # space.
        #
        # Additionally, comments (such as these) may be inserted on individual
        # lines or following the machine name denoted by a '#' symbol.
        #
        # For example:
        #
        #      102.54.94.97     rhino.acme.com          # source server
        #       38.25.63.10     x.acme.com              # x client host
        
        # localhost name resolution is handled within DNS itself.
        #   127.0.0.1       localhost
        #   ::1             localhost
        
        52.165.38.64    www.contoso.com
        ```

2. Győződjön meg arról, hogy most tallózhat www.contoso.com a próba ügyfélszámítógép. Alapértelmezett IIS oldalon hasonlóan jelennek meg.

3. A próba ügyfélszámítógépen nyisson meg egy parancssorablakot rendszergazdaként, és a makecert segédprogrammal c
4. hamis legfelső szintű hitelesítésszolgáltató tanúsítványának létrehozása:

        ```
        makecert -sky exchange -pe -a sha256 -n "CN=MyFakeRootCertificateAuthority" -r -sv MyFakeRootCertificateAuthority.pvk MyFakeRootCertificateAuthority.cer -len 2048
        ```

    Győződjön meg arról, hogy létrejönnek-e a következő fájlokat:

    - MyFakeRootCertificateAuthority.cer

    - MyFakeRootCertificateAuthority.pvk

4. A következő parancsot a próba legfelső szintű hitelesítésszolgáltató telepítése:

        ```
        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

5. A próba hitelesítésszolgáltató segítségével www.contoso.com tartozó tanúsítvány létrehozása:

        ```
        makecert -sk pkey -iv MyFakeRootCertificateAuthority.pvk -a sha256 -n "CN=www.contoso.com" -ic MyFakeRootCertificateAuthority.cer -sr localmachine -ss my -sky exchange -pe
        ```

6. Az **mmc** parancsot, és adja hozzá a tanúsítványok a fiókot a helyi számítógép számára beépülő modult.

7. Az a */Certificates (helyi számítógép) / személyes/tanúsítvány/* tárolhatja, és a titkos kulcs www.contoso.com.pfx nevű fájlt a www.contoso.com tanúsítvány exportálása:

    [! [20]][20]

8. Térjen vissza az Azure-portálra, és a kezelés réteg virtuális (*TS-aad-ntier-kezelése – vm1*) csatlakozzon. Az alapértelmezett felhasználóneve *tesztfelhasználó nevet* jelszóval *AweS0me@PW*:

    [! [21]][21]
    
9. Csatlakozás a kezelés réteg virtuális egyes a webes réteg VMs viszont a *tesztfelhasználó nevet* felhasználónév és jelszó *AweS0me@PW*, és hajtsa végre az alábbi műveleteket. Ne feledje, hogy a VMs a privát címek IP 10.0.1.4 10.0.1.5 és 10.0.1.6:

    >[AZURE.NOTE] A webes réteg VMs csak a saját IP-címek van. Csak csatlakozhat őket a felügyeleti réteg virtuális használatával.

    1. A fájlok *www.contoso.com.pfx* és *MyFakeRootCertificateAuthority.cer* másolja a próba ügyfélszámítógép.
    
    2. Nyisson meg egy parancssorablakot rendszergazdaként, helyezze át a mappába a fájlok másolt tartó, és futtassa az alábbi parancsokat:
    
        ```
        certutil.exe -privatekey -importPFX my www.contoso.com.pfx NoExport

        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

    3. Futtassa a `mmc` parancsot, adja hozzá a tanúsítványok a fiókot a helyi számítógép számára beépülő modult, és ellenőrizze, hogy a következő tanúsítványok telepítve van:

        - \Certificates (helyi számítógép) \Personal\Certificates\ www.contoso.com

        - \Certificates (helyi számítógép) \Trusted legfelső szintű hitelesítésszolgáltató Authorities\Certificates\MyFakeRootCertificateAuthority

    4. Indítsa el az Internet Information Services (IIS) Manager konzolt és *Sites\Default webhelyén* nyissa meg a számítógépen.

    5. Kattintson a *Műveletek* ablaktáblában kattintson a *kötések*, és egy https-kötés, használja a www.contoso.com SSL-tanúsítvány felvétele:

        [! [22]][22]

10. Térjen vissza az próba ügyfélszámítógépen, és ellenőrizze, hogy most tallózhat https://www.contoso.com.

### <a name="create-an-azure-active-directory-tenant"></a>Az Azure Active Directory-bérlő létrehozása

1. Az Azure portálon létrehozása az Azure Active Directory bérlői *myaadname*például az. onmicrosoft.com, hol található a *myaadname* az Ön által kiválasztott címtár nevét.

2. *Rendszergazdai* szerepkör a címtárhoz GlobalAdmin néven felhasználó hozzáadása. Jegyezze fel az újonnan létrehozott jelszót.

3. Az Internet Explorerben https://account.activedirectory.windowsazure.com/ tallózással keresse meg, és jelentkezzen be azzal admin@ *myaadname*. onmicrosoft.com. Módosíthatja a jelszavát, amikor a rendszer kéri.

### <a name="create-and-deploy-a-test-web-application"></a>Készíthetnek és helyezhetnek üzembe a próba webalkalmazás

1. Visual Studio segítségével a fejlesztői számítógépen létrehozása egy olyan névvel ellátott ContosoWebApp1 ASP.NET webalkalmazást (használja a .NET-keretrendszer 4.5.2.):

    [! [23]][23]

2. Jelölje ki a *MVC* sablont, módosítsa a hitelesítés *munkahelyi és iskolai fiókokat*, és adja meg a nevét az AAD bérlői webhelyen. Az alkalmazás szolgáltatásainak nem hoz létre a felhőben.

    [! [24]][24]

3. Építse fel, és futtassa az alkalmazást, tesztelje a hitelesítés. A bejelentkezési lapon adja meg a fióknév admin@ *myaadname*. onmicrosoft.com, adja meg a jelszót, és kattintson a *Bejelentkezés*gombra:

    [! [25]][25]

4. Ellenőrizze, hogy AAD, bejelentkezés, és olvassa el a profil engedélyt kér, megkezdi a webalkalmazás rendszerrel azonosítója rendszergazda.

5. Zárja be az Internet Explorerben, és térjen vissza a Visual Studio.

6. Nyissa meg a fájlt, és a `<appSettings>` szakaszban, a *ida: PostLogoutRedirectUri* billentyű lenyomásával értékének módosítása *https://www.contoso.com:443 /*. Mentse a fájlt.

7. A *Megoldás-kezelő* ablakban kattintson a jobb gombbal a ContosoWebApp1 projektet, kattintson a *Közzététel*gombra.

8. A *Webhely közzététele* ablakában kattintson az *egyéni*elemre. Névvel ellátott *ContosoWebApp1*egyéni profil létrehozása.

9. A *kapcsolat* lapon állítsa a *Közzététel módszer* *Fájlrendszerben* , és adja meg a *ContosoWebApp*, egy helyen fejlesztési a számítógépen található mappába.

10. A *Beállítások* lapon állítsa a *konfigurációs* *kiadásra*.

11. A *kép* lapon kattintson a *Közzététel*gombra.

12. Minden webkiszolgáló viszont (a kezelés réteg virtuális) keresztül csatlakozhat, és végezze el az alábbi műveleteket:

    1. Másolja a *ContosoWebApp* mappát és tartalmát a fejlesztői számítógép *C:\inetpub* mappába.

    2. Az Internet Information Services (IIS) Manager konzolon nyissa meg *Sites\Default webhely* azon a számítógépen.

    3. Kattintson a *Műveletek* ablaktáblában *Alap beállítások*gombra, és a webhely fizikai elérési útjának *%SystemDrive%\inetpub\ContosoWebApp*módosítása:

        [! [26]][26]

### <a name="publish-the-test-web-application-through-aad"></a>A próba webalkalmazás AAD keresztül közzététele

1. Jelentkezzen be az Azure-portálra, és nyissa meg azt a AAD címtárban.

2. Kattintson az *alkalmazások* fülre a ContosoWebApp1 alkalmazást.

3. Ellenőrizze, hogy az alkalmazás a címtárhoz sikeresen hozzáadta, és kattintson a *beállítás* fülre.

4. A *SIGN-ON URL-cím* módosítása https://www.contoso.com:443, és a *Válasz URL-címe* értékűre https://www.contoso.com:443 (azonos URL-CÍMÉT).

5. A konfiguráció mentéséhez.

6. A próba ügyfélszámítógépen nyissa meg azt a https://www.contoso.com. Ellenőrizze, hogy AAD meg a hitelesítő adatokat kér, és jelentkezzen be.

>[AZURE.NOTE] Ez az első eset a végpont: az n szintű webalkalmazás külső felhasználóknak, a jelszó-hitelesítést nyújtó AAD fut a felhőben való hozzáférés engedélyezése.

### <a name="create-a-simulated-on-premises-environment-with-active-directory"></a>Az Active Directoryval szimulált a helyszíni környezet létrehozása

A helyszíni környezet két tartomány vezérlő az tartalmazza a `contoso.com` tartomány és az Azure AD Connect szolgáltatója két kiszolgálóinak szinkronizálása szolgáltatást. Azure AD Connect szolgáltatója kiszolgálóinak vannak nem a tartományhoz.

1. A Fájlkezelőben lépjen vissza az N szintű webalkalmazás létrehozására szolgáló parancsfájlt tartalmazó mappát, a parancsfájlok.

2. A paraméterek mappa hozzáadása egy másik sub mappájában `Onpremise`.

3. Paraméterek/helyi mappába, töltse le a következő fájlokat:

    - [hozzáadása – ad-tartomány-controller.parameters.json][add-adds-domain-controller-parameters]

    - [Hozzon létre – ad-erdő-extension.parameters.json][create-adds-forest-extension-parameters]

    - [virtualMachines-adc.parameters.json][virtualMachines-adc-parameters]

    - [virtualMachines-lépett-joindomain.parameters.json][virtualMachines-adc-joindomain-parameters]

    - [virtualMachines-adds.parameters.json][virtualMachines-adds-parameters]

    - [virtualNetwork.parameters.json][virtualNetwork-parameters]

    - [virtualNetwork hozzáadása dns.parameters.json][virtualNetwork-adds-dns-parameters]

5. A központi telepítés-ReferenceArchitecture.ps1 fájl szerkesztése a a parancsfájlok mappát, és módosítsa a következő sort az erőforráscsoport kell létrehozott vagy a virtuális és a parancsfájl által létrehozott erőforrások tárolására szolgáló megadásához:

        ```powershell
        # PowerShell
        $resourceGroupName = "ra-aad-onpremise-rg"
        ```

6. Szerkessze a következő fájlokat a paraméterek/helyi mappában, és állítsa a `resourceGroup` az az érték a `virtualNetworkSettings` szakasz minden ezeket a fájlokat lehet ugyanaz, mint a központi telepítés-ReferenceArchitecture.ps1 parancsprogram megadott.

    - virtualMachines-adds.parameters.json

    - virtualMachines-adc.parameters.json

    - virtualNetwork.parameters.json

    - virtualNetwork hozzáadása dns.parameters.json

7. Nyisson meg egy Azure PowerShell ablakot, helyezze át a parancsfájlok mappába, és futtassa a következő parancsot:

        ```powershell
        .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Windows onpremise
        ```

    Csere `<subscription id>` az Azure előfizetés azonosítójával.

    A `<location>`, például: Adja meg, az Azure régió `eastus` vagy `westus`.

### <a name="install-and-configure-the-azure-ad-connect-sync-service"></a>Az Azure AD Connect szinkronizálási szolgáltatás telepítése és beállítása

A konfiguráció illusztrált az alábbi lépéseket a Azure AD Connect szinkronizálási szolgáltatás a két példánya áll. Az első olyankor, miközben a második gyors feladatátvevő és magas elérhetősége megadására, ha nem sikerül egy az első kiszolgáló átmeneti tárolásra szolgáló módban van konfigurálva.

1. Az Azure portálon nyissa meg azt az erőforráscsoport, eközben tartsa a VMs az Azure AD Connect szinkronizálási szolgáltatások (*TS-aad-helyi-rg* alapértelmezés szerint), és a virtuális *TS-aad-helyi-lépett-vm1* csatlakozni. Jelentkezzen be azzal *tesztfelhasználó nevet* jelszóval *AweS0me@PW*.

2. Töltse le a Azure AD Connect [Itt][aad-connect-download].

3. A Azure AD Connect telepítő futtatása és a *Testreszabás* parancs telepíteni.

    >[AZURE.NOTE] Mivel a virtuális az Azure AD Connect szinkronizálási szolgáltatója nem tartományhoz nem használhatja a *Express beállításai* lehetőséget.

4. A *szükséges összetevők telepítése* lapon a választható beállítások üresen hagyja fogadja el az alapértelmezett beállításokat.

5. Válassza ki a *felhasználó bejelentkezési* lapján, a *Jelszó-szinkronizálás*.

6. A *Csatlakozás az Azure Active Directory* lapon adja meg a admin@ *myaadname*. onmicrosoft.com, hol *myaadname* -e a AAD bérlő nevét. Írja be jelszavát a rendszergazdai fiókjával.

7. *A könyvtárak csatlakoztatása* lapon adja meg a contoso.com erdőhöz (írja be az értéket, mert nem jelenik meg a legördülő lista), CONTOSO\testuser megadása a felhasználó nevét, adja meg AweS0me@PW a jelszót, és kattintson a *Címtár hozzáadása*.

8. Az *Azure Active Directory bejelentkezési beállítása* lapon fogadja el az alapértelmezett érték egyszerű felhasználónév. Jelölje be a *bármely igazolt tartományt nélkül a Folytatás gombra*.

    >[AZURE.NOTE] A contoso.com könyvtár szerepelni *Nincs ellenőrizve*. Ellenőrizni a tartománynevet, létre kell hoznia egy TXT rekordot a tartománynév szolgáltató. Ebben a példában a helyszíni tartomány nem regisztrált külső felhasználókkal. További tudnivalókért lásd: az [Azure Active Directory egyéni tartománynév hozzáadása][aad-custom-directory].

9. A *tartomány és a szervezeti egységre szűrés* lapon válassza a *tartományok és a szervezeti szinkronizálása* (alapértelmezett).

10. *A felhasználók egyedileg azonosító* lapon elfogadhatja az alapértelmezett értékeket.

11. A *felhasználó és eszköz szűrő* lapon válassza a *minden felhasználó és eszköz szinkronizálása* (alapértelmezett).

12. A *választható funkció* lapon válassza a *jelszó visszaírást*.

13. *Készen áll a konfigurálása* lapon jelölje ki, *Indítsa el a szinkronizálást konfigurációs befejezésekor*, de hagyja, *fejlesztői üzemmód engedélyezése* beállítás kijelölve.

14. Ellenőrizze, hogy a beállítási folyamat befejeződik, hibátlan, és zárja be, hogy a telepítő.

15. Az Azure portálról *TS-aad-helyi-lépett-vm2* virtuális csatlakozni. Jelentkezzen be azzal *tesztfelhasználó nevet* jelszóval *AweS0me@PW*.

16. Töltse le az Azure AD Connect, és futtassa a telepítő.

17. A varázsló lépéseit, és válaszoljon leírt lépések 3 – 12.

18. *Készen áll a konfigurálása* lapon jelölje ki a *Indítsa el a szinkronizálást konfigurációs befejezésekor*, és a **is** *fejlesztői üzemmód engedélyezése*elemre. Ennek hatására az Azure AD Connect szinkronizálási szolgáltatás fiókok és az objektumok lekérhetők a contoso.com tartományt, majd a saját adatbázisához tárolja őket, de azt nem továbbítja az Ön AAD bérlői ezeket az adatokat.

14. Ellenőrizze, hogy a beállítási folyamat befejeződik, hibátlan, és zárja be, hogy a telepítő.

### <a name="test-the-aad-configuration"></a>A AAD beállítások tesztelése

1. Az Azure portál használatával, lépjen a AAD mappába, nyissa meg az Azure Active Directory-lap, kattintson a *felhasználók és csoportok*, és kattintson a *minden felhasználónak* a felhasználók és csoportok a directory címtárral szinkronizált listájának megjelenítéséhez. Meg kell jelennie a következő fiókok felhasználók:

    - felügyeleti (admin@ *myaadname*. onmicrosoft.com)

    - A helyszíni címtár-szinkronizálási szolgáltatásfiók (Sync_ADC1_*nnnnnnnnnnnn*@*myaadname*. onmicrosoft.com)

    - A helyszíni címtár-szinkronizálási szolgáltatásfiók (Sync_ADC2_*nnnnnnnnnnnn*@*myaadname*. onmicrosoft.com)

2. Az Azure-portálon nyissa meg azt az erőforráscsoport, eközben tartsa a VMs, az Active Directory tartományi szolgáltatások tartomány vezérlők (*TS-aad-helyi-rg* alapértelmezés szerint), és csatlakozzon a *TS-aad-helyi-Active Directory-vm1* virtuális. Jelentkezzen be azzal *tesztfelhasználó nevet* jelszóval *AweS0me@PW*.

3. Kocsis Tibbot, névvel ellátott bejelentkezési új tartomány felhasználó hozzáadása az *Active Directory-felhasználók és -számítógépek* konzol használata dianet@contoso.com. Adja meg egy tetszés szerinti jelszót. Ellenőrizze a felhasználó a helyi Rendszergazdák csoport tagjának (Ez a helyzet csak bejelentkezhet helyileg a felhasználók később – a csak gép a tartományban DCs).

4. *TS-aad-helyi-lépett-vm1* virtuális váltani, nyisson meg egy PowerShell-ablakot, és futtassa az alábbi parancsokat:

        ```[powershell]
        Import-Module ADSync
        Start-ADSyncSyncCycle -PolicyType Delta
        ```

    Győződjön meg arról, hogy a parancs *sikerrel*tér vissza.

    >[AZURE.NOTE] Ez a lépés nem szükséges, mert alapértelmezés szerint a szinkronizálás közötti 30 percenként van ütemezve. Ezek a parancsok szinkronizálná azonnal megtörténik.

5. Térjen vissza az Azure-portálra, váltson a AAD címtárban, nyissa meg az Azure Active Directory-lap, kattintson a *felhasználók és csoportok*, és válassza a *minden felhasználónak*. Ekkor a kocsis Tibbot kell jelennie (dianet@ *myaadname*. onmicrosoft.com) jelennek meg a felhasználók listáját.

6. Az Azure Active Directory-lap kattintson a *Vállalati alkalmazások*, és kattintson az *összes alkalmazás*. Kattintson a *ContosoWebApp1* alkalmazásra, és válassza a *felhasználók és csoportok ablakot*. Kattintson az eszköztáron kattintson a *Hozzáadás*gombra. Kattintson a *felhasználók és csoportok ablakot*, és válassza a *Kocsis Tibbot*. Kattintson a *hozzárendelni*.

7. Az Internet Explorer használata esetén keresse meg a webhely https://account.activedirectory.windowsazure.com. Jelentkezzen be azzal dianet@ *myaadname*. onmicrosoft.com és a korábban megadott jelszót.

    >[AZURE.NOTE] Ne kattintson az alkalmazások listájának ContosoWebApp ikonra. AAD próbálja keresése a webes alkalmazás a nyilvánosan felsorolt DNS címen www.contoso.com, ami eltér a webes szintű terheléselosztó címét.

8. Kattintson a *profil* fülre. A részletek, a felhasználó (Ha a megadott bármelyik létrehozásakor jött létre automatikusan) kell látszania.

    >[AZURE.NOTE] Ha a *jelszó módosítása*gombra kattint, nem engedélyezettek ezt a műveletet engedélyeznie kell a rendszergazda által először a feladat végrehajtásához. További tudnivalókért lásd: a [jelszavak kezelését – első lépések][aad-password-management].

### <a name="enable-on-premises-users-to-run-the-application-by-using-authentication-through-aad"></a>A helyszíni felhasználói hitelesítéssel AAD – az alkalmazás futtatásához engedélyezése

1. Vissza a *TS-aad-helyi-Active Directory-vm1* tartományvezérlőnek virtuális.

2. Nyissa meg a *DNS-kezelőben* konzolt, és új host rekord beállítása a www.contoso.com. Adja meg a webes szintű terheléselosztó nyilvános IP-címét.

3. Másolja a fájl *MyFakeRootCertificateAuthority.cer* (létrehozott a fájlokat az eljárás [a nyilvános webhely Simulate konfigurációs](#simulate-configuration-of-a-public-web-site) teszt-ügyfélszámítógépen
    
4. Nyisson meg egy parancssorablakot rendszergazdaként, helyezze át a oszlopából kimásolt a fájlt tároló mappába, és futtassa az alábbi parancsot:

        ```
        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

5. Az Internet Explorerben navigáljon a https://www.contoso.com. Ellenőrizze, hogy a AAD bejelentkezési lapja a ContosoWebApp1 webalkalmazás jelenik meg.

6. Jelentkezzen be azzal dianet@ *myaadname*. onmicrosoft.com. Futtassa az alkalmazást, és jelentkeztetni megfelelően.

## <a name="next-steps"></a>Következő lépések

- További tudnivalók a gyakorlati tanácsok [a helyszíni ÖSSZEADJA a tartomány Azure bővítése][adds-extend-domain]

- További tudnivalók a gyakorlati tanácsok [ÖSSZEADJA az erőforrás erdőn létrehozásához] [ adds-resource-forest] Azure-ban

<!-- links -->
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[script]: #sample-solution-script
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[active-directory-domain-services]: https://technet.microsoft.com/library/dd448614.aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[azure-active-directory]: ../active-directory-domain-services/active-directory-ds-overview.md
[azure-ad-connect]: ../active-directory/active-directory-aadconnect.md
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[aad-explained]: https://youtu.be/tj_0d4tR6aM
[aad-editions]: ../active-directory/active-directory-editions.md
[guidance-adds]: ./guidance-iaas-ra-secure-vnet-ad.md
[sla-aad]: https://azure.microsoft.com/support/legal/sla/active-directory/v1_0/
[azure-multifactor-authentication]: ../multi-factor-authentication/multi-factor-authentication.md
[aad-conditional-access]: ../active-directory//active-directory-conditional-access.md
[aad-dynamic-membership-rules]: ../active-directory/active-directory-accessmanagement-groups-with-advanced-rules.md
[aad-dynamic-memberships]: https://youtu.be/Tdiz2JqCl9Q
[aad-user-sign-in]: ../active-directory/active-directory-aadconnect-user-signin.md
[aad-sync-requirements]: ../active-directory/active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md
[aad-topologies]: ../active-directory/active-directory-aadconnect-topologies.md
[aad-filtering]: ../active-directory/active-directory-aadconnectsync-configure-filtering.md
[aad-scalability]: https://blogs.technet.microsoft.com/enterprisemobility/2014/09/02/azure-ad-under-the-hood-of-our-geo-redundant-highly-available-distributed-cloud-directory/
[aad-connect-sync-default-rules]: ../active-directory/active-directory-aadconnectsync-understanding-default-configuration.md
[aad-identity-protection]: ../active-directory/active-directory-identityprotection.md
[aad-password-management]: ../active-directory/active-directory-passwords-customize.md
[aad-application-proxy]: ../active-directory/active-directory-application-proxy-enable.md
[aad-connect-sync-operational-tasks]: ../active-directory/active-directory-aadconnectsync-operations.md#staging-mode
[aad-custom-domain]: ../active-directory/active-directory-add-domain.md
[aad-powershell]: https://msdn.microsoft.com/library/azure/mt757189.aspx
[aad-sync-disaster-recovery]: ../active-directory/active-directory-aadconnectsync-operations.md#disaster-recovery
[aad-sync-best-practices]: ../active-directory/active-directory-aadconnectsync-best-practices-changing-default-configuration.md
[aad-adfs]: ../active-directory/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs
[aad-health]: ../active-directory/active-directory-aadconnect-health-sync.md
[aad-health-adds]: ../active-directory/active-directory-aadconnect-health-adds.md
[aad-health-adfs]: ../active-directory/active-directory-aadconnect-health-adfs.md
[aad-agent-installation]: ../active-directory/active-directory-aadconnect-health-agent-install.md
[aad-reporting-guide]: ../active-directory/active-directory-reporting-guide.md
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-powershell-download]: ../powershell-install-configure.md
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/Deploy-ReferenceArchitecture.ps1
[vnet-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/virtualNetwork.parameters.json
[nsg-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/networkSecurityGroups.parameters.json
[webtier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/webTier.parameters.json
[businesstier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/businessTier.parameters.json
[datatier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/dataTier.parameters.json
[managementtier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/managementTier.parameters.json
[makecert]: https://msdn.microsoft.com/library/windows/desktop/aa386968(v=vs.85).aspx
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/add-adds-domain-controller.parameters.json
[create-adds-forest-extension-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/create-adds-forest-extension.parameters.json
[virtualMachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adds.parameters.json
[virtualNetwork-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualNetwork.parameters.json
[virtualNetwork-adds-dns-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualNetwork-adds-dns.parameters.json
[virtualMachines-adc-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adc.parameters.json
[virtualMachines-adc-joindomain-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adc-joindomain.parameters.json
[aad-connect-download]: http://www.microsoft.com/download/details.aspx?id=47594
[aad-custom-directory]: ../active-directory/active-directory-add-domain.md
[aad-password-management]: ../active-directory/active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords
[adds-extend-domain]: ./guidance-identity-adds-extend-domain.md
[adds-resource-forest]: ./guidance-identity-adds-resource-forest.md
[0]: ./media/guidance-identity-aad/figure1.png "Azure Active Directory használatával felhő identitás architektúra"
[1]: ./media/guidance-identity-aad/figure2.png "Egy a erdő, egyetlen AAD címtár topológiája"
[2]: ./media/guidance-identity-aad/figure3.png "Több erdők, egyetlen AAD címtár topológiája"
[4]: ./media/guidance-identity-aad/figure5.png "Kiszolgáló topológia előkészítése"
[5]: ./media/guidance-identity-aad/figure6.png "Több AAD könyvtárak topológiája"
[6]: ./media/guidance-identity-aad/figure7.png "Jelölje ki az Azure Active Directory csatlakozás szinkronizálás a következővel példányára SQL Server egy egyéni telepítése"
[7]: ./media/guidance-identity-aad/figure8.png "Egyszeri bejelentkezés módszer a felhasználónak bejelentkezés megadása"
[8]: ./media/guidance-identity-aad/figure9.png "Tartomány és a szervezeti egységre, szűrési beállítások megadása"
[9]: ./media/guidance-identity-aad/figure10.png "Jelszó írási-vissza engedélyezése"
[10]: ./media/guidance-identity-aad/figure11.png "Az Azure Active Directory kezelése lap a portálon"
[11]: ./media/guidance-identity-aad/figure12.png "Az Azure AD Connect konzol"
[12]: ./media/guidance-identity-aad/figure13.png "A műveletek fülre a szinkronizálás szolgáltatáskezelő"
[13]: ./media/guidance-identity-aad/figure14.png "Az összekötők lapon a szinkronizálás szolgáltatáskezelő"
[14]: ./media/guidance-identity-aad/figure15.png "A szinkronizálás szabályok-szerkesztő"
[15]: ./media/guidance-identity-aad/figure16.png "Az Azure-portálon szinkronizálási állapot megjelenítése az Azure Active Directory csatlakozás állapot lap"
[16]: ./media/guidance-identity-aad/figure17.png "Az Azure Active Directory csatlakozás rendszerállapot lap az Azure Active Directory tartományi szolgáltatások állapotát megjelenítő portálon"
[17]: ./media/guidance-identity-aad/figure18.png "Elérhető az Azure-portálon biztonsági-jelentések"
[18]: ./media/guidance-identity-aad/figure19.png "A Kiemelés a nyilvános IP-címét a webhely szintű terheléselosztó Azure portál"
[19]: ./media/guidance-identity-aad/figure20.png "Tallózással keresse meg a webes szintű terheléselosztó nyilvános IP-címét az Internet Explorer használata"
[20]: ./media/guidance-identity-aad/figure21.png "A tanúsítványok beépülő modul a www.contoso.com tanúsítvány megjelenítő"
[21]: ./media/guidance-identity-aad/figure22.png "Kapcsolódás a kezelés réteg virtuális"
[22]: ./media/guidance-identity-aad/figure23.png "A HTTPS-kötés, az alapértelmezett webhely létrehozása"
[23]: ./media/guidance-identity-aad/figure24.png "A ContosoWebApp1 webalkalmazás létrehozása"
[24]: ./media/guidance-identity-aad/figure25.png "A ContosoWebApp1 webalkalmazás hitelesítési tulajdonságainak beállítása"
[25]: ./media/guidance-identity-aad/figure26.png "Bejelentkezés az Azure AAD a ContosoWebApp1 webalkalmazásból"
[26]: ./media/guidance-identity-aad/figure27.png "Az alapértelmezett webhely a mappa módosítása"
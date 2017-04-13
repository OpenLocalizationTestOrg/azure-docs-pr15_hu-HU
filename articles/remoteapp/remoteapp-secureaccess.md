
<properties 
    pageTitle="És túl az Azure RemoteApp való hozzáférés biztonságossá tétele |} Microsoft Azure"
    description="Megtudhatja, hogyan biztonságos hozzáférés Azure RemoteApp az Azure Active Directory feltételes access programmal"
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />

# <a name="securing-access-to-azure-remoteapp-and-beyond"></a>És túl az Azure RemoteApp való hozzáférés biztonságossá tétele

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Ebben a cikkben nyújtunk hogyan be rendszergazdaként a végfelhasználó keresztül Azure RemoteApp kezdési és befejezési, például egy SQL-adatbázisban vagy egy másik alkalmazás háttéradatbázist biztonságos erőforráshoz biztonságos hozzáférés csatorna áttekintése. A cél, győződjön meg arról, hogy a feltételeknek megfelelő csak hitelesített felhasználó hozzáférhet-e a távoli alkalmazásokat, és, hogy a biztonságos háttéradatbázist csak érhetők el a ellenőrzött Azure RemoteApp környezetben, és más helyekről nem.

Létezik a rendszergazdának is megtekintheti a 3 fő terület:

![Azure RemoteApp feltételes access kapcsolatos szempontok](./media/remoteapp-secureaccess/ra-conditionalenvironment.png)

Az alábbi kérdésekre adott válaszok és az információk a olvasható.

## <a name="who-can-access-the-collection"></a>A webhelycsoport hozzáférő?
A rendszergazda úgy dönt, hogy a távoli alkalmazásokat a gyűjteményben elérésére jogosult felhasználók. Azure Active Directory (Azure Active Directory) vagy munkahelyi vagy iskolai fiók (korábbi nevén, "szervezeti fiókok") Microsoft esetén használható (pl. @outlook.com). Vállalati találatokkal Azure Active Directory-fiókok; használata segítségükkel feltételes access szolgáltatások később tárgyalt és szintén csak a választási lehetőségek, a tartományhoz tartozó használata. A cikk a többi feltételezi, hogy a Azure RemoteApp Azure Active Directory-fiókok használja.

**Mi a Microsoft hajtotta végre:**

Azure RemoteApp való hozzáférés szabályozása a Azure AD-fiókokkal, annál us két dolog:

1.  Mindig képben ki férhet hozzá az alkalmazásokat, hogy közzétették és hozzáférés vissza részek alkalmazásokon csatlakozni.
2.  Az alapul szolgáló Azure Active Directory azt szabályozzák, akkor hozhat létre és törlés felhasználói fiókok beállítása jelszóházirendek, használja a többtényezős hitelesítés stb. 

## <a name="how-is-the-collection-accessed-from-where"></a>Hogyan érhető el a webhelycsoport? Honnan?
A rendszergazdák gyakran szeretne összeállítani házirendek nyilvános internetes környezetben, például az Azure RemoteApp eléréséhez. Ha például szeretnének gondoskodhat arról, hogy a felhasználók a környezet a vállalati hálózaton kívüli elérése többtényezős hitelesítés (MFA) segítségével hozzáférhet; vagy akár azok blokkolni kell teljesen.

Azure RemoteApp rendszergazdái az Azure Active Directory prémium verzióban elérhető funkciókat a Azure RemoteApp környezetben feltételes access házirendek beállítása. Is segítségével gazdag jelentése és szolgáltatások riasztási figyelheti a környezet elérhető hogyan.

### <a name="how-to-set-up-conditional-access-for-azure-remoteapp"></a>Hogyan állíthat be feltételes hozzáférés az Azure RemoteApp
Egy példa forgatókönyv – a rendszergazda a környezet elérésének letiltása, amikor a felhasználók nem tagjai a vállalati hálózathoz Azure RemoteApp végigvezetik fogjuk.

>[AZURE.NOTE] Feltételezzük, hogy a prémium réteg frissítette a Azure Active Directory és a létrehozott legalább egy Azure RemoteApp webhelycsoport.

1.  Az Azure-portálon kattintson a az **Active Directory** fülre. Kattintson a címtárban be szeretné állítani.

    Ne feledje: Feltételes teljes egészében egy tulajdonság, a címtárában és nem Azure RemoteApp, így minden konfigurálás befejeződött, a címtár-szintjén. Ez azt is jelenti kell lennie a címtár-rendszergazdától, hogy a módosítások.

2.  Kattintson az **alkalmazások**elemre, és kattintson a **Microsoft Azure RemoteApp** feltételes hozzáférés beállítása gombra. Figyelje meg, hogy beállíthatja az egyes "szolgáltatásként" szoftveralkalmazás feltételes access címtárában külön-külön.
![Azure RemoteApp feltételes hozzáférés beállítása](./media/remoteapp-secureaccess/ra-conditionalaccessscreen.png)
 

3.  A **beállítás** lapon állítsa a **Hozzáférési szabályok engedélyezése** be értékre.
![Hozzáférési szabályok engedélyezése az Azure RemoteApp](./media/remoteapp-secureaccess/ra-enableaccessrules.png)
 

4.  Most különböző szabályok konfigurálása, és válassza ki a alkalmazza őket, hogy:

    1. Válassza ki, **Ha nem a munkahelyén hozzáférésének letiltása az** teljesen Azure RemoteApp kívül a hálózati környezet, adja meg, hogy hozzáférjen a felhasználók számára letiltott.
    2. Kattintson az alábbi lehetőségek valamelyikére meghatározása az IP-címtartományok, amely a "megbízható hálózat" alkotnak. Minden alkalmazáson kívüli elutasításra.

5.  Tesztelje a konfigurációt indítása az Azure RemoteApp ügyfél, a megadott tartomány-Ön kívüli IP-címet a. Miután bejelentkezett az Azure Active Directory hitelesítő adatok meg kell jelennie egy üzenet jelenik meg:

![Azure RemoteApp való hozzáférés letiltva](./media/remoteapp-secureaccess/ra-accessdenied.png)
 

### <a name="future-conditional-access-features"></a>Jövőbeli feltételes hozzáférési funkciók 
Az Azure Active Directory csoport feltételes Access új funkciók a működik. A rendszergazdák létrehozhatnak új típusú hálózaton túli szabályok alapú hely szabályai lesz. Nyilvános előnézete az új funkciók hamarosan érhető el kell.

### <a name="how-to-monitor-access-to-azure-remoteapp"></a>Azure RemoteApp hozzáférést figyelése
Egy nagy használata mellett feltételes access funkció az Azure Active Directory prémium jelentéskészítési funkciót. Személy éri el a környezet figyelése jelentések használata, és feltárása a gyanús tevékenységeket.

Például láthatja azoknak a felhasználók számára elérhető Azure RemoteApp, azok jelent meg, hogy hány alkalommal, amikor.

1.  Az Azure-portálon kattintson **Az Active Directory**, és válassza a címtárban.

2.  Nyissa meg a **jelentések** fülre.

3.  Jelentések a listából jelölje ki az **Alkalmazáshasználat** **integrált alkalmazások**csoportban.

    Azure RemoteApp megjelenik az összesített statisztikai adatokat. 
![Összesített Azure RemoteApp access stat](./media/remoteapp-secureaccess/ra-accessstats.png)
 
5.  Kattintson az alkalmazás felhasználóját Azure RemoteApp információt szeretne.
![Felhasználói hozzáférés statisztikájának Azure RemoteApp](./media/remoteapp-secureaccess/ra-userstats.png)
 
### <a name="summary"></a>Összefoglalás
Az Azure Active Directory prémium beállíthatja a hozzáférési szabályok Azure RemoteApp (és más programokat egy szolgáltatásalkalmazások Azure Active Directory keresztül érhető el). Szabályok jelenleg csak a hálózati hely alapú házirendek, de a jövőben egyéb szempontok enterprise Management terjeszteni fog.

Azure Active Directory-támogatás is lehetővé teszi a jelentéskészítő és az további kiterjesztése a rendszergazdák az a vezérlőelem-funkciókkal figyelése Azure RemoteApp környezetükben fölé.

## <a name="how-do-i-make-sure-my-secure-resource-is-accessible-only-from-my-azure-remoteapp-environment"></a>Hogyan minősíthetek meg arról, hogy a biztonságos erőforrás elérhető csak a Azure RemoteApp használok?
Az előző szakaszokban Ez a cikk azt csak az Azure RemoteApp környezetben való hozzáférés biztonságossá tétele a. Azt hajtotta végre, válassza a felhasználók, akik számára engedélyezett az access, és állítsa be a hozzáférési szabályok további vezérlőelemre, hogyan használhatják a szolgáltatást.

A leggyakoribb helyzet Azure RemoteApp telepítésekhez, hogy a távoli alkalmazások háttéradatbázist erőforrás, például egy SQL-adatbázis kommunikálniuk kell. Ez az erőforrás üzemelteti használatát vagy a helyszíni (például a vállalati hálózaton) vagy a felhőben (például a Azure IaaS). A rendszergazdák gyakran gondoskodni szeretne arról, hogy a háttéradatbázist erőforrás csak azok webböngészőn keresztül Azure RemoteApp telepített alkalmazások, és nem például egy alkalmazást, közvetlenül a felhasználó személyi számítógépen futó és nyilvános interneten elérése. Azure RemoteApp gyakran tekinteni a központilag felügyelt és biztonságos környezet, és így keresztül, amely-lapok használata a háttéradatbázist erőforrás kell felhasználók csak elérési útját.

A megoldás, ha el szeretne helyezni az Azure RemoteApp környezet és a biztonságos erőforrás a az azonos Azure virtuális hálózati (VNET). Ha egy másik helyre az erőforrás van, a webhely virtuális Magánhálózati kapcsolat, például a hozzon létre egy felölelő az Azure adatok center az ügyfélnél a helyszíni környezet VNet hozhat létre.

Azure RemoteApp két típusa, ahol megadhatja a saját VNET webhelycsoport telepítések támogatja:

-   Nem-tartományhoz: az alkalmazások lesz "sort az Oszlopcímek rögzítése" egyéb erőforrások a VNET. Például ez használható alkalmazások által használt SQL-alapú hitelesítés SQL-adatbázis csatlakoztatása (alkalmazások csatlakozó felhasználók hitelesítését közvetlenül az adatbázisra)

-   A tartományhoz: a virtuális gépeken futó Azure RemoteApp által használt adatbázis a tartományvezérlőnek a VNET a. Ez akkor hasznos, amikor az alkalmazások kell a Windows tartományvezérlőnek hitelesítést háttéradatbázist erőforrás való hozzáférés érdekében.
![Azure RemoteApp a tartományhoz tartozó gyűjteménye](./media/remoteapp-secureaccess/ra-domainjoined.png)
 
### <a name="how-to-create-a-secure-connection-between-azure-and-my-on-premises-environment"></a>Azure és a helyszíni környezet közötti biztonságos kapcsolat létrehozása
Nincsenek számos beállítási lehetőségei az Azure- és a helyszíni környezetben csatlakozik. Itt érhető el a beállítások áttekintése.

Azure RemoteApp kell állítsa be először a VNet, és írja be azt a webhelycsoport a létrehozási folyamat során. 

## <a name="the-complete-solution"></a>Az ideális megoldás.
Az alábbi ábrán látható a ideális megoldás, ahol azt rendelkezik beépített biztonságos hozzáférés csatorna a végfelhasználó keresztül Azure RemoteApp (ARA), a kódmentes erőforrás.
![Azure RemoteApp biztonságos](./media/remoteapp-secureaccess/ra-secureoverview.png) a szakasz 1-es azt a felhasználót jelölt és hozzáférési szabályokat, amelyek meghatározzák, hogy hogyan férhet hozzá ARA létrehozott. Az alábbi példában azt csak elérésének engedélyezése a felhasználóknak a vállalati hálózathoz dolgozik. Nem kompatibilis a felhasználók nem tudnak a ARA környezet egyáltalán eléréséhez.
Szakaszában"2" azt van téve a kódmentes erőforráshoz csak a VNet/VPN konfigurációja, amelyek azt szabályozzák. Az azonos VNet Azure RemoteApp került. A végeredmény, hogy az erőforrás csak azok webböngészőn keresztül a ARA környezetet.



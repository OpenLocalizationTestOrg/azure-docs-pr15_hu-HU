<properties
    pageTitle="Azure Active Directory tartományi szolgáltatások: Telepítési forgatókönyvek |} Microsoft Azure"
    description="Telepítési forgatókönyvek az Azure Active Directory tartományi szolgáltatások"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="maheshu"/>


# <a name="deployment-scenarios-and-use-cases"></a>Telepítési forgatókönyvek és használati-esetek
Ebben a részben azt meg, néhány esetek összekapcsolhatók az Azure Active Directory (AD) tartományi szolgáltatásokból használati eset.

## <a name="secure-easy-administration-of-azure-virtual-machines"></a>Azure virtuális gépeken futó biztonságos, egyszerűen felügyelete
Azure Active Directory tartományi szolgáltatások kezelése az Azure virtuális gépeken futó egyesíti módon is használhatja. Azure virtuális gépeken futó is lehet a tartományhoz csatlakoztatott felügyelt, így amely lehetővé teszi bejelentkezni a vállalati Active Directory hitelesítő adatait. Ezt a megközelítést segítségével elkerülheti a hitelesítő adatok kezelése bajlódnia, például helyi rendszergazdai fiókkal az egyes az Azure virtuális gépeken futó fenntartása.

A felügyelt tartományban, a kiszolgáló virtuális gépeken futó is felügyelt és biztonságos csoportházirend használatával. Szükséges biztonsági alaptervek alkalmazása az Azure virtuális gépeken futó, és zárolhatja az összeset lefelé vállalati biztonsági irányelveknek megfelelően. Ha például szeretné korlátozni, amely a virtuális gépeken indulhat alkalmazások típusú csoport házirend-kezelési lehetőségek is használhatja.

![Azure virtuális gépeken futó egyesíti felügyelete](./media/active-directory-domain-services-scenarios/streamlined-vm-administration.png)

Kiszolgálók és más infrastruktúra eléri a elhasználódott, mint a Contoso a felhőbe helyszíni jelenleg üzemeltetik számos alkalmazás áthelyezi. Az aktuális informatikai standard arra, hogy a vállalati alkalmazások üzemeltető kiszolgálókon tartományhoz és a felügyelt csoportházirend segítségével kell lennie. Contoso tartomány illesztés virtuális gépeken futó telepítését az Azure-rendszergazdai inkább felügyelet megkönnyítése céljából. Emiatt rendszergazdák és a felhasználók is jelentkezzen be a vállalati hitelesítő adataival. Egy időben gépek beállítható úgy, hogy ahhoz szükséges biztonsági alaptervek csoportházirend használatával. A Contoso inkább nem szeretné, hogy telepíthető, figyelésére és kezelheti a tartomány vezérlők biztonságossá tétele Azure virtuális gépeken futó Azure-ban. Azure Active Directory tartományi szolgáltatások ezért egy remek igazítás, a használatieset.

**Telepítési jegyzetek**

Vegye figyelembe az alábbi fontos tudnivalókra telepítési példánkban:

- Azure Active Directory tartományi szolgáltatások által felügyelt tartományok egy egyetlen strukturálatlan (szervezeti egység) szervezeti struktúra alapértelmezés szerint adja meg. Egyetlen strukturálatlan szervezeti egység összes tartományhoz gépek tárolnak. Azonban létrehozása egyéni szervezeti dönthet.

- Azure Active Directory tartományi szolgáltatások egyszerű csoportházirend támogatja a beépített csoportházirend-Objektumot minden a felhasználók és számítógépek formájában tárolók. Nem csoportházirend OU/részleg által érintett, végezze el a szűrés WMI vagy egyéni csoportházirend-objektum létrehozása.

- Azure Active Directory tartományi szolgáltatások támogatja az alap AD számítógép objektum séma. A számítógép-objektum séma nem bővítése.


## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-bind-authentication-to-azure-infrastructure-services"></a>Felvonó-és-shift LDAP kötés hitelesítés Azure infrastruktúrájának szolgáltatásai használó helyszíni alkalmazás

![LDAP kötés](./media/active-directory-domain-services-scenarios/ldap-bind.png)

A Contoso vásárolta egy külső sok évvel ezelőtti megjelenésével a helyszíni alkalmazást tartalmaz. Az alkalmazás jelenleg a külső karbantartási mód, és az alkalmazás módosításainak kérő elfogadhatatlanul magas, a Contoso-e. Ez az alkalmazás webalapú időtúllépést, amelyek a felhasználói hitelesítő adatokat egy webes űrlap használatával gyűjti össze, és ezután hitelesíti a felhasználókat a vállalati Active Directory-LDAP kötés végrehajtásával tartalmaz. Ez az alkalmazás Azure infrastruktúrájának szolgáltatásai áttelepítendő contoso szeretné. Célszerű az, hogy az alkalmazás működik, mint, anélkül, hogy el a módosításokat. Ezenkívül felhasználók kell fognak tudni hitelesítést végezni, a meglévő vállalati hitelesítő adataival és anélkül, hogy a felhasználók eljárásra Újraépítés. Ez azt jelenti kell, hogy a végfelhasználók oblivious, ahol az alkalmazást futtató, és az áttelepítés átlátszó rájuk kell lennie.

**Telepítési jegyzetek**

Vegye figyelembe az alábbi fontos tudnivalókra telepítési példánkban:

- Győződjön meg arról, hogy az alkalmazás nem szükséges, módosítsa/írási a címtárhoz. LDAP írási hozzáféréssel Azure Active Directory tartományi szolgáltatások által felügyelt tartományok nem támogatott.

- Jelszavak közvetlenül szemben kezelt tartomány nem módosítható. Záró felhasználója képes legyen módosítani a jelszavát vagy használja az Azure Active Directory önkiszolgáló jelszó módosítása mechanizmusa vagy a helyszíni címtár. Ezek a változások a felügyelt tartomány automatikusan szinkronizált állapotban.


## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-read-to-access-the-directory-to-azure-infrastructure-services"></a>Felvonó shift LDAP használó helyszíni alkalmazás olvassa el a könyvtár Azure infrastruktúrájának szolgáltatásai eléréséhez
A Contoso kifejlesztett szinte egy korábbi évtizede a helyszíni sor üzleti (üzleti) alkalmazást tartalmaz. Az alkalmazás könyvtár tartsa szem előtt, és Windows Server AD készült. Az alkalmazás LDAP (Lightweight Directory Access Protocol) használja az Active Directory adatokat vagy attribútumokat felhasználókról olvasni. Az alkalmazás nem attribútumok módosítása vagy más módon írja be a címtárhoz. Contoso szeretné áttelepíteni Azure infrastruktúrájának szolgáltatásai alkalmazás, és az alkalmazás éppen szolgáltatója elévülési helyszíni hardver kivonni. Az alkalmazás nem kell írni, modern directory API-khoz, például a többi-alapú Azure Active Directory Graph API használatához. Ezért egy felvonó shift lehetőséget van szükség, amellyel az alkalmazás futtatásához a felhőben kód módosítása és címek átírása az alkalmazás nélkül telepíthető át.

**Telepítési jegyzetek**

Vegye figyelembe az alábbi fontos tudnivalókra telepítési példánkban:

- Győződjön meg arról, hogy az alkalmazás nem szükséges, módosítsa/írási a címtárhoz. LDAP írási hozzáféréssel Azure Active Directory tartományi szolgáltatások által felügyelt tartományok nem támogatott.

- Győződjön meg arról, hogy az alkalmazás nem szükséges, az Active Directory egyéni/bővített séma. Séma bővítmények nem támogatottak az Azure Active Directory tartományi szolgáltatások.


## <a name="migrate-an-on-premises-service-or-daemon-application-to-azure-infrastructure-services"></a>Áttelepítése a helyszíni szolgáltatást vagy démont kérelmet Azure infrastruktúrájának szolgáltatásai
Egyes alkalmazások többszintű, ahol a réteg egyik elvégzéséhez szüksége van egy kódmentes réteg, például egy adatbázis réteg hitelesített hívásainak állnak. Active Directory-fiókok használata – ilyenkor általában arra használják. Felvonó és a SHIFT billentyűt is Azure infrastruktúrájának szolgáltatásai, és ezeket az alkalmazásokat identitás szükségleteinek Azure Active Directory tartományi szolgáltatások kérelmekhez. Megadhatja, hogy ugyanazt a fiókot, hogy a rendszer szinkronizálja a helyszíni címtárból Azure Active Directory használni. Másik megoldás először hozzon létre egy egyéni szervezeti, és ezután hozzon létre külön szolgáltatásfiók a szervezeti egységre, ezek az alkalmazások terjesztése.

![WIA használatával szolgáltatásfiók](./media/active-directory-domain-services-scenarios/wia-service-account.png)

A Contoso tartalmaz egy custom-built tárolóra alkalmazás, amely tartalmaz egy előtér, SQL server és a kódmentes FTP-kiszolgáló. Windows-hitelesítést szolgáltatás fiókok használatos előtér az FTP-kiszolgálóhoz hitelesítést végezni. Az előtér szolgáltatásfiók futtatása van beállítva. Az előtér-szolgáltatás-fiókból hozzáférés engedélyezése a kódmentes kiszolgáló van beállítva. A Contoso szeretnék nem kell üzembe tartomány vezérlő virtuális gép Azure infrastruktúrájának szolgáltatásai alkalmazás áthelyezése a felhőben. Contoso rendszergazdai telepítheti az előtér, SQL server és a FTP-kiszolgálót, Azure virtuális gépeken futó üzemeltető kiszolgálókon. A számítógépek majd tartományhoz az Azure Active Directory tartományi szolgáltatások felügyelt. Ezután használhatják ugyanazt a fiókot a helyszíni könyvtárban az alkalmazás hitelesítési céljából. Ez a szolgáltatás fiók szinkronizálja az Azure Active Directory tartományi szolgáltatások felügyelt tartománynak, és használható.

**Telepítési jegyzetek**

Vegye figyelembe az alábbi fontos tudnivalókra telepítési példánkban:

- Győződjön meg arról, hogy az alkalmazás használja-e a hitelesítést felhasználónevével és jelszavával. Tanúsítvány/intelligens-alapú hitelesítés Azure Active Directory tartományi szolgáltatások által nem támogatott.

- Jelszavak közvetlenül szemben kezelt tartomány nem módosítható. Záró felhasználója képes legyen módosítani a jelszavát vagy használja az Azure Active Directory önkiszolgáló jelszó módosítása mechanizmusa vagy a helyszíni címtár. Ezek a változások a felügyelt tartomány automatikusan szinkronizált állapotban.


## <a name="azure-remoteapp"></a>Azure RemoteApp
Azure RemoteApp lehetővé teszi, hogy a Contoso rendszergazdáját, hogy a tartományhoz tartozó webhelycsoport létrehozása. Ez a szolgáltatás lehetővé teszi, hogy a távoli alkalmazások Azure RemoteApp, melyet a tartományhoz számítógépen, és más forrásokat, használja a Windows-hitelesítést. A Contoso Azure Active Directory tartományi szolgáltatások segítségével adja meg a felügyelt Azure RemoteApp a tartományhoz tartozó által használt tartomány.

![Azure RemoteApp](./media/active-directory-domain-services-scenarios/azure-remoteapp.png)

Telepítési példánkban kapcsolatos további tudnivalókért lásd a című témakört a távoli asztali szolgáltatásokat Blog [felvonó és a SHIFT billentyűt az Azure RemoteApp és Azure Active Directory tartományi szolgáltatások munkaterhelésekből](http://blogs.msdn.com/b/rds/archive/2016/01/19/lift-and-shift-your-workloads-with-azure-remoteapp-and-azure-ad-domain-services.aspx).

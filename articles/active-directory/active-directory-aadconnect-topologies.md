<properties
    pageTitle="Azure AD Connect: Támogatott topológiák |} Microsoft Azure"
    description="Ez a témakör részletezi támogatott és a nem támogatott topológiák az Azure AD Connect"
    services="active-directory"
    documentationCenter=""
    authors="AndKjell"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="billmath"/>

# <a name="topologies-for-azure-ad-connect"></a>Az Azure Active Directory topológiák csatlakoztatása
Ez a témakör célja különféle helyszíni és a fő integrációs megoldásként Azure AD Connect szinkronizálása az Azure Active Directory topológiák mutatják be. Támogatott és a nem támogatott beállításokat írja le.

A dokumentum képek Jelmagyarázat:

Leírás | Ikon
-----|-----
A helyszíni Active Directory-erdő| ![ACTIVE DIRECTORY](./media/active-directory-aadconnect-topologies/LegendAD1.png)
Az Active Directory szűrt importálással| ![ACTIVE DIRECTORY](./media/active-directory-aadconnect-topologies/LegendAD2.png)
Azure AD Connect szinkronizálási kiszolgáló| ![Szinkronizálás](./media/active-directory-aadconnect-topologies/LegendSync1.png)
Azure AD Connect szinkronizálási "átmeneti kiszolgálóban"| ![Szinkronizálás](./media/active-directory-aadconnect-topologies/LegendSync2.png)
FIM2010 vagy MIM2016 GALSync| ![Szinkronizálás](./media/active-directory-aadconnect-topologies/LegendSync3.png)
Azure AD Connect szinkronizálási kiszolgáló részletes| ![Szinkronizálás](./media/active-directory-aadconnect-topologies/LegendSync4.png)
Azure Active directory |![AAD](./media/active-directory-aadconnect-topologies/LegendAAD.png)
Nem támogatott forgatókönyv | ![Nem támogatott](./media/active-directory-aadconnect-topologies/LegendUnsupported.png)

## <a name="single-forest-single-azure-ad-tenant"></a>Egy a erdő, egyetlen Azure AD-bérlő
![Egyetlen erdő egyetlen bérlői](./media/active-directory-aadconnect-topologies/SingleForestSingleDirectory.png)

A leggyakoribb topológia egyetlen erdőszinten a helyszíni egy vagy több tartományt, és egy egyetlen Azure AD-bérlői. Azure AD-hitelesítés a jelszó-szinkronizálás használatos. Azure AD Connect expressz telepítésének csak ez topológia támogatja.

### <a name="single-forest-multiple-sync-servers-to-one-azure-ad-tenant"></a>Erdőn, egy Azure AD-bérlő több szinkronizálási kiszolgáló
![Egyetlen erdő szűrt nem támogatott](./media/active-directory-aadconnect-topologies/SingleForestFilteredUnsupported.png)

Több Azure AD Connect szinkronizálási kiszolgáló csatlakozik a azonos Azure AD-bérlő kivételével a [kiszolgáló átmeneti](#staging-server)nem támogatott. Nem támogatott, még akkor is, ha ezek vannak beállítva, hogy szinkronizálja az objektumok egymást kölcsönösen kizáróak csoportja. Előfordulhat, hogy tekintette meg, ha egy kiszolgálóról vagy betöltése több kiszolgáló elosztása erdőben található összes tartományok nem érheti el.

## <a name="multiple-forests-single-azure-ad-tenant"></a>Több erdők, egyetlen Azure AD-bérlő
![Több erdő egyetlen bérlői](./media/active-directory-aadconnect-topologies/MultiForestSingleDirectory.png)

Sok szervezetek környezetekben több a helyszíni Active Directory-erdők rendelkezik. Különböző okból egynél több a helyszíni Active Directory-erdő szemléltetik. Jellemző példák a fiók-erőforrás erdők és eredményként látványtervek egyesülés vagy WIA után.

Ha több erdők, az összes erdők van elérhető egyetlen Azure AD Connect kell lennie szinkronizálási kiszolgáló. Nem kell a kiszolgáló csatlakozik a tartományhoz. Ha minden erdő eléréséhez szükséges, a kiszolgáló DMZ-hálózatban lehet ügyfélwebhelyre.

Az Azure AD Connect telepítővarázslóban több erdők ábrázolt felhasználók összesítése több lehetőséget kínál. Az célja, hogy a felhasználó csak egyszer az Azure Active Directory jelöli. Létezik néhány gyakori topológiák beállíthatja az egyéni telepítési elérési út az telepítővarázslóban. Jelölje ki a megfelelő beállítást, amely a topológia **egyedileg azonosító a felhasználók**lapon. Az Összesítés csak van konfigurálva a felhasználók számára. Ismétlődő csoportok nem kell az alapértelmezett beállításokat tartalmazó összevonni.

Gyakori topológiák tárgyalt, a következő szakaszban: [külön topológiák](#multiple-forests-separate-topologies), a [teljes háló](#multiple-forests-full-mesh-with-optional-galsync)és a [Fiók-erőforrás](#multiple-forests-account-resource-forest).

Az alapértelmezett beállítások Azure AD Connect szinkronizálás feltételezi, hogy:

1. Felhasználók csak egy engedélyezett fiókja van, és a hol található az ehhez a fiókhoz erdő csatlakozó felhasználók hitelesítését szolgál. Ez a feltételezésen mindkét jelszó-szinkronizálás és az összevonási van. UserPrincipalName és sourceAnchor/immutableID ebben származik.
2. A felhasználók csak egy postafiókot rendelkeznek.
3. A felhasználó postafiók üzemeltető erdő legjobb adatok minőségének attribútumok látható az Exchange globális cím lista (globális Címlistában a) tartalmaz. Ha nincs postaláda a felhasználó, majd bármely erdőjének használható szeretné küldeni a attribútum ezeket az értékeket.
4. Ha egy csatolt postaláda, majd található is másik fiókba való bejelentkezéshez használt eltérő erdőben.

A környezet nem egyezik meg az e feltételezések, az alábbi történik, ha:

- Ha egynél több aktív fiókot vagy több postaláda, a szinkronizálási motor választja ki az egyik és figyelmen kívül hagyja a másik.
- Azure Active Directory nem exportálja a csatolt postaláda, amelyben nincs más aktív fiókot. A felhasználói fiók tagja bármelyik csoport nem jelöli. A DirSync csatolt postafiók mindig a egy normál postaládából része képviseli. Ez a változás szándékosan egy másik viselkedés több erdőre hatékonyabb támogatása.

További részletek találhatók [az alapértelmezett configuation ismertetése](active-directory-aadconnectsync-understanding-default-configuration.md).

### <a name="multiple-forests-multiple-sync-servers-to-one-azure-ad-tenant"></a>Több erdők, egy Azure AD-bérlő több szinkronizálási kiszolgáló
![Több erdő több szinkronizálási nem támogatott](./media/active-directory-aadconnect-topologies/MultiForestMultiSyncUnsupported.png)

Nem támogatott egynél több Azure AD-csatlakozás szinkronizálás kiszolgáló csatlakozik egy Azure AD-bérlő. A kivétel ez alól a [kiszolgáló átmeneti](#staging-server)használatát.

### <a name="multiple-forests--separate-topologies"></a>Több erdők – külön topológiák
**Felhasználók csak egyszer jelző összes könyvtárak keresztül**

![Több erdő felhasználók egyszer](./media/active-directory-aadconnect-topologies/MultiForestUsersOnce.png)

![Több erdő Seperate topológiák](./media/active-directory-aadconnect-topologies/MultiForestSeperateTopologies.png)

Ebben a környezetben összes erdők helyszíni külön szervezetek számít és lenne, hogy a felhasználó nem bemutató más erdőben.
Egyes erdőnek van a saját Exchange-szervezetbe, és nincs GALSync a erdők között van. Ez a topológia lehet a helyzet egyesülés/WIA után, vagy a szervezeten belüli hol részlegek üzemel egymástól elszigetelt. Adott erdők az Azure Active Directory szervezete, és egy egyesített globális Címlistában jelennek meg.
Ezen a képen az egyes objektumok minden erdőben egyszer ábrázolt a metaverse és összesíti a cél Azure AD-bérlő a.

### <a name="multiple-forests--match-users"></a>Több erdők – egyezés felhasználók
**Felhasználói identitások léteznek több könyvtárak keresztül**

Ez a helyzet a közös, terjesztési és a biztonsági csoportok tartalmazhat kombinálja a felhasználók, a névjegyalbumhoz és a FSP (idegen rendszerbiztonsági)

FSP ÖSSZEADJA a tagok a biztonsági csoport más erdőkből ábrázolásához használják. Az Azure Active Directory összes FSP megoldani a tényleges objektumra.

### <a name="multiple-forests--full-mesh-with-optional-galsync"></a>Több erdők – a választható GALSync háló
**Felhasználói identitások létezik több könyvtárak keresztül. Megfelelő használatával: levelezési attribútum**

![Több erdő felhasználók levelek](./media/active-directory-aadconnect-topologies/MultiForestUsersMail.png)

![Több erdő háló](./media/active-directory-aadconnect-topologies/MultiForestFullMesh.png)

Egy teljes topológia lehetővé teszi a felhasználók és erőforrások bármely erdőjének található, és gyakran lesz kétirányú a erdők között.

Ha az Exchange jelen egynél több erdőben, tetszés szerint lehet egy helyszíni GALSync megoldás. Minden felhasználónak szeretné képviselteti magát az összes többi erdőkben partnerként. GALSync Forefront identitáskezelő 2010 vagy Microsoft Identitáskezelő 2016 segítségével gyakran történik. Azure AD Connect helyszíni GALSync esetében nem használhatók.

Ebben az esetben identitás objektumok adatbázis használata a levelezési attribútum. Felhasználó postafiók az egyik erdő csatlakozik, a többi erdőkben partnerekkel.

### <a name="multiple-forests--account-resource-forest"></a>Több erdők – fiók Erőforráserdő
**Felhasználói identitások létezik több könyvtárak keresztül. Megfelelő használatával: ObjectSID és msExchMasterAccountSID attribútumok**

![Több erdő felhasználók ObjectSID](./media/active-directory-aadconnect-topologies/MultiForestUsersObjectSID.png)

![Több erdő AccountResource](./media/active-directory-aadconnect-topologies/MultiForestAccountResource.png)

A fiók-erőforrás erdő topológiában van egy vagy több fiók erdők aktív felhasználói fiókok. Letiltott fiók egy vagy több erőforrás erdők is telepítve van.

Ebben az esetben egy (vagy több) **Erőforráserdő** minden **fiók erdők**megbízik. A Erőforráserdő általában egy bővített Active Directory-sémát az Exchange és a Lync tartalmaz. Összes Exchange és a Lync szolgáltatást, valamint a más megosztott szolgáltatások ebben találhatók. Felhasználójánál ebben a letiltott felhasználói fiókok és a postaláda kapcsolódik a fiókerdők.

## <a name="office-365-and-topology-considerations"></a>Az Office 365 és a topológia kapcsolatos szempontok
Egyes Office 365-munkaterhelésekből kell támogatott topológiák bizonyos korlátozások. Ha használja ezek egyikét, olvassa el a terhelést a támogatott topológiák témakör.

Terhelést |  
--------- | ---------
Exchange Online | Ha egynél több Exchange szervezet helyszíni (Ez azt jelenti, hogy az Exchange lett telepítve egynél több erdőbe), majd kell használnia, az Exchange 2013 SP1 vagy újabb verziójában. Részletek megtalálható itt: [az Active Directory-erdők több hibrid telepítések](https://technet.microsoft.com/library/jj873754.aspx)
A Skype vállalati verzió | Ha több erdők helyszíni használja, csak a fiók-erőforrás erdő topológiában funkció használható. A támogatott topológiák megtalálható itt részletek: [a Skype 2015 vállalati kiszolgáló környezeti követelményei](https://technet.microsoft.com/library/dn933910.aspx)

## <a name="staging-server"></a>A kiszolgáló átmeneti
![A kiszolgáló átmeneti](./media/active-directory-aadconnect-topologies/MultiForestStaging.png)

Azure AD Connect támogatja a második kiszolgáló telepítése **fejlesztői**módban. Ebben az üzemmódban kiszolgáló adatokat olvas be az összes csatlakoztatott könyvtárak, de ne írjon valamit csatlakoztatott könyvtárak. Akkor használja a normál szinkronizálási ciklus és így a személyes adatok frissített másolatának. Az amikor az elsődleges kiszolgáló nem sikerül, katasztrófa is átveszi az átmeneti tárolásra szolgáló kiszolgáló. Ehhez az Azure AD Connect varázsló. A második kiszolgáló lehetőleg is található egy másik adatközponthoz, mivel nincsenek infrastruktúra megosztja az elsődleges kiszolgáló. A második kiszolgálóra az elsődleges kiszolgálón bármilyen konfigurációs változás manuálisan kell másolnia.

A fejlesztői kiszolgálóra is használható tesztelje az új egyéni konfiguráció és az adatok általa hatása. Megtekintheti a módosításokat, és beállításainak módosítására. Ha elégedett az új konfiguráció, teheti az átmeneti tárolásra szolgáló kiszolgáló az active server és a régi active server átmeneti tárolására üzemmód beállítása.

Ez a módszer is használható a active sync-kiszolgáló helyére. Az új kiszolgáló előkészítése és beállítja átmeneti tárolására mód. Ellenőrizze, hogy jó állapotban letiltása átmeneti tárolására mód (aktiválás), és leállítja a jelenleg aktív kiszolgálót.

Ahhoz, hogy egynél több átmeneti tárolásra szolgáló kiszolgáló, ha másik adatközpontokban van-e több biztonsági másolatot szeretne lehetőség.

## <a name="multiple-azure-ad-tenants"></a>Több Azure AD-bérlők
A Microsoft javasolja egyetlen bérlői webhelyre kellene az Azure Active Directory egy szervezet számára.
Mielőtt több Azure AD-bérlők használni kívánja, az alábbi témakörök tárgyalja a gyakori alkalmazási területek, lehetővé téve egy bérlői webhelyre használatát.

A témakör |  
--------- | ---------
Felügyeleti egységek használatával meghatalmazás | [Az Azure Active Directory felügyeleti egységek kezelése](active-directory-administrative-units-management.md)

![Több erdő több bérlői](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectory.png)

Az Azure AD Connect szinkronizálási-kiszolgáló és az Azure AD-bérlő közötti 1:1 kapcsolat áll fenn. Minden egyes Azure AD-bérlő szüksége van egy Azure AD Connect szinkronizálási server telepítése. Az Azure AD-bérlő példányok elszigetelt szándékosan és egyet a felhasználók nem látják a felhasználók a másik bérlőn. Ha a elkülönítésének készült, majd ez a beállítás egy támogatott, de egyébként kell használni az egyetlen Azure AD-bérlő modell.

### <a name="each-object-only-once-in-an-azure-ad-tenant"></a>Minden objektumhoz csak egyszer Azure AD-ös bérlői
![Egyetlen erdő szűrve](./media/active-directory-aadconnect-topologies/SingleForestFiltered.png)

Ebben a topológiában Azure AD Connect szinkronizálási Kiszolgálónként minden Azure AD-bérlő kapcsolódik. Szűrés, így minden kell a működtetéséhez objektumok egymást kölcsönösen kizáró csoportja a Azure AD Connect szinkronizálási kiszolgálókat kell beállítania. Ha például minden-kiszolgálót, egy adott tartomány vagy szervezeti is korlátozhatja. A DNS-tartomány csak egyetlen regisztrálását Azure AD-bérlő. Az UPN AD a felhasználók a helyszíni külön névtér, valamint kell használnia. Például a képen a fenti három külön egyszerű Felhasználónévi utótag regisztrált a helyszíni Active Directory: contoso.com fabrikam.com és wingtiptoys.com. Az egyes felhasználók a helyszíni Active Directory tartományi egy másik névtér használata.

Az Azure AD-bérlő példányok között nincs GALsync nem. A címjegyzék az Exchange Online és a Skype vállalati verziós csak látható felhasználók ugyanahhoz a bérlőhöz.

Ebben a topológiában rendelkezik-e a következő egyéb korlátozások esetek támogatott:

- Csak az egyik az Azure AD-bérlők engedélyezheti a helyszíni Active Directoryval való hibrid Exchange.
- A Windows 10-eszközökön csak társítható egy Azure AD-bérlő.

Az objektumok egymást kölcsönösen kizáró megadása kötelező visszaírást is érinti. Néhány visszaírást funkciók nem támogatottak a topológia, mivel ezek a funkciók feltételezik helyszíni egyetlen konfigurációja:

-   Csoport visszaírást alapértelmezett beállításokkal
-   Eszköz visszaírást

### <a name="each-object-multiple-times-in-an-azure-ad-tenant"></a>Az egyes objektumok többször az Azure AD-bérlő
![Egyetlen erdő több bérlői nem támogatott](./media/active-directory-aadconnect-topologies/SingleForestMultiDirectoryUnsupported.png) ![Egyetlen erdő több összekötők nem támogatott](./media/active-directory-aadconnect-topologies/SingleForestMultiConnectorsUnsupported.png)

- Ugyanaz a felhasználó több Azure AD-bérlők szinkronizálása nem támogatja.
- Még nem támogatott, hogy a konfiguráció módosítása, hogy a felhasználók egy Azure AD a névjegyek a másik Azure AD-bérlő jelennek meg.
- Azure AD Connect szinkronizálási több Azure AD-bérlők csatlakozhat módosítása nem támogatja.

### <a name="galsync-by-using-writeback"></a>GALsync visszaírást használatával
![MultiForestMultiDirectoryGALSync1Unsupported](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync1Unsupported.png) ![MultiForestMultiDirectoryGALSync2Unsupported](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync2Unsupported.png)

Azure AD-bérlők elszigetelt tervezés szerint is.

- Adatok beolvasása egy másik Azure AD-bérlő Azure AD Connect szinkronizálási beállításainak módosítása nem támogatja.
- Azt nem támogatott a partnerek a felhasználók exportálása egy másik a helyszíni Active Directory sync Azure AD Connect használatával.

### <a name="galsync-with-on-premises-sync-server"></a>A helyszíni szinkronizálási kiszolgálóval GALsync
![MultiForestMultiDirectoryGALSync](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync.png)

FIM2010/MIM2016 helyszíni GALsync felhasználók között két Exchange-szervezetek használandó támogatja. A felhasználók egy szervezet névjegyként külső felhasználók és a szervezeten belül jelenik meg. A különféle helyszíni reklámok majd szinkronizálható a saját Azure AD-bérlők.

## <a name="next-steps"></a>Következő lépések
Ez a helyzet az Azure AD Connect telepítése című témakörben talál [: Azure AD Connect egyéni telepítés](./connect/active-directory-aadconnect-get-started-custom.md).

További tudnivalók az [Azure AD Connect szinkronizálni](active-directory-aadconnectsync-whatis.md) konfigurációt.

További tudnivalók [a helyszíni identitások Azure Active Directoryval való integrálásának](active-directory-aadconnect.md).

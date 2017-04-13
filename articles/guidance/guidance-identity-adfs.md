<properties
   pageTitle="Végrehajtási ADFS Azure-ban |} Microsoft Azure"
   description="Hogyan lehet egy biztonságos hibrid hálózat architektúrája, az Active Directory összevonási szolgáltatás engedélyezési megvalósítása Azure-ban."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
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
   ms.date="10/13/2016"
   ms.author="telmos"/>

# <a name="implementing-active-directory-federation-services-adfs-in-azure"></a>Azure Active Directory összevonási szolgáltatások (ADFS) végrehajtása

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Ez a cikk ismerteti, hogy a helyszíni hálózaton Azure kiterjed, és [Az Active Directory összevonási szolgáltatások (ADFS)] használó biztonságos hibrid hálózati alkalmazásával kapcsolatos gyakorlati tanácsokat[ active-directory-federation-services] szövetséges hitelesítési és engedélyezési végrehajtásához összetevők futtatása a felhőben. Ez a felépítés kiterjeszti [az Azure Active]Directory bővítése ismertetett struktúrát[implementing-active-directory].

> [AZURE.NOTE] Azure van két különböző telepítési modellek: [Erőforrás-kezelő] [ resource-manager-overview] és klasszikus. A hivatkozás architektúra erőforrás-kezelő Microsoft javasolja új telepítési használja.

ADFS futtatását is lehetővé teszi, hogy a helyszíni, de a hibrid helyzetben hol található az alkalmazások Azure-ban ezt a funkciót a felhőben végrehajtásához hatékonyabbá tehető. Ez a felépítés esettel jellemző használata a következők:

- Hol munkaterhelésekből Futtatás részben a helyszíni hibrid alkalmazásokat és részben az Azure.

- Szövetséges engedélyezési kattintva jelenítse meg a partner szervezetek webalkalmazások kihasználó megoldásokat.

- A rendszer támogatja a hozzáférést a szervezeti tűzfal kívüli futó böngészők.

- Rendszerek, engedélyezése a felhasználók férhetnek hozzá a webalkalmazások hivatalos külső eszközökről, például a távoli számítógépek, a jegyzetfüzetek és a más mobileszközökre útján. 

ADFS működésével kapcsolatos további tudnivalókért lásd: az [Active Directory összevonási szolgáltatások áttekintése][active-directory-federation-services-overview]. Emellett a cikk [az Azure ADFS telepítési] [ adfs-intro] tartalmaz részletes, lépésenkénti bemutatása a ADFS végrehajtási Azure-ban.

## <a name="architecture-diagram"></a>Architektúra diagramja

Az alábbi ábra kiemeli a fontos e (*kattintson a Nagyítás*) architektúra összetevőket. Bármely elemének, nem ADFS kapcsolatos további információkért olvassa el az [Azure egy biztonságos hibrid hálózat architektúrája végrehajtási][implementing-a-secure-hybrid-network-architecture], [egy biztonságos hibrid hálózat architektúrája Azure-ban Internet-hozzáféréssel rendelkező végrehajtási][implementing-a-secure-hybrid-network-architecture-with-internet-access], és [a biztonságos hibrid hálózat architektúrája az Azure Active Directory-felhasználókkal végrehajtásához][implementing-active-directory]:

[! [0]][0]

>[AZURE.NOTE] Ez az ábra szemlélteti használja a következő esetekben:
>
>- Egy partner szervezeten belüli futó alkalmazás kód segítségével érheti el az Azure VNet belül is webalkalmazás.
>
>- Egy külső, bejegyzett felhasználót (belül ÖSSZEADJA tárolt hitelesítő adatokhoz) szeretne hozzáférni az Azure VNet belül is webalkalmazás.
>
>- A felhasználó által engedéllyel rendelkező eszközt használ, és az Azure VNet belül is webalkalmazás fut a VNet kapcsolódás.
>
>Emellett a e architektúra koncentrál passzív Föderáció, ahol az összevonási kiszolgálók döntse el, hogyan és mikor egy felhasználó hitelesítse magát. A felhasználó bejelentkezési adatok megadására, futó alkalmazás indításakor várhatóan. Ez a leggyakrabban használt webböngészők mechanizmusa és csapattól kell beszerezni az protokoll, amely egy webhelyen, ahol a felhasználó hitelesítő tartalmaznak irányítja át a böngészőben. ADFS támogatja az aktív összevonási, amellyel további felhasználói beavatkozás nélküli hitelesítő adatok megadásával felelősséget veszi fel az alkalmazás, de ebben az esetben ez architektúra hatókörén kívüli is.

- **ÖSSZEADJA alhálózat.** A ÖSSZEADJA kiszolgálók saját alhálózat található. A ÖSSZEADJA kiszolgálók védelme és biztosíthat a forgalmat a váratlan források ellen tűzfalat NSG szabályokat.

- **ÖSSZEADJA a kiszolgálón.** Ezek a tartomány vezérlők futtatott VMs a felhőben. Megadhatja, hogy az alábbi kiszolgálókon helyi azonosítók a tartományban a hitelesítési.

- **ADFS-alhálózat.** Az ADFS-kiszolgálók elhelyezhetők a saját alhálózat tűzfalat eljáró NSG szabályokkal.

- **ADFS-kiszolgálók.** Az ADFS-kiszolgálók adja meg a hitelesítési és szövetséges engedélyezési. Ez a architektúrában őket a következő feladatokat:

    - Egy partner felhasználó nevében partner összevonási kiszolgáló által végzett követelések tartalmazó biztonsági tokenek kaphatnak. ADFS ellenőrizheti, hogy tokenek érvényesek a jogcímalapú webalkalmazáshoz Azure-ban futó előtt. A vállalati webalkalmazás (Azure) használatával e követelések kérések engedélyezése. Ebben az esetben a vállalati webalkalmazást *fél használna*, és a felelős a partner összevonási kiszolgáló kibocsátása követelések, amely a vállalati webalkalmazás által alatt érteni. A partner az összevonási kiszolgálók nevezik *számla partnereket* , mert azok elküldése a hozzáférési kérelmek a partner szervezeti fiókok hitelesített nevében. Mivel az erőforrásokhoz (ebben az esetben a webes alkalmazások) való hozzáférést biztosítanak a az ADFS-kiszolgálók *partnerek erőforrás* neve.

    - Hitelesíteni tudja (hozzáadása és az [Active Directory eszköz regisztrációs szolgáltatás][ADDRS]) és engedélyezése a külső felhasználók eszköz a vállalati webalkalmazások hozzáférést igénylő vagy webböngészőben fut, bejövő felkérést. 

    Az ADFS-kiszolgálók van konfigurálva a farm az Azure terheléselosztó keresztül érhető el. Elérhetőség és méretezhetőség segít a struktúra. Figyeljen arra, hogy az ADFS-kiszolgálók ne legyenek kitéve közvetlenül az interneten, inkább az összes internetes forgalmat szűrt ADFS webes alkalmazás proxykiszolgálók és a DMZ keresztül.

- **ADFS-proxy alhálózat.** Az ADFS proxykiszolgálók védelme NSG szabályokkal saját alhálózat belül is tartalmaz. Az alhálózathoz kiszolgálóinak az interneten keresztül hálózati virtuális készülékek, amelyek magyarázatot adnak a tűzfalat Azure virtuális hálózatához, és az Internet között kell kitenni.

- **ADFS-proxy (WAP) alkalmazáskiszolgálók webes.** Ezek a számítógépek használni a kívánt partner szervezetek és a külső eszközökön bejövő felkérést ADFS kiszolgálóinak. A WAP kiszolgálók szűrőként, az ADFS-kiszolgálók közvetlen hozzáférés nyilvános internetkapcsolat a sugárvédelmi tároló működésbe lépnek. Az ADFS-kiszolgálók, az üzembe helyezése a WAP terheléselosztás a farm kiszolgálókon lépve nagyobb elérhetőség és méretezhetőség, mint a önálló kiszolgálók gyűjteménye.

    >[AZURE.NOTE] WAP kiszolgálók telepítésével kapcsolatos részletes tudnivalókért lásd: [Telepítse és állítsa be a webes alkalmazás proxykiszolgáló][install_and_configure_the_web_application_proxy_server]

- **Partner szervezeti.** Ez az, hogy egy partner hozzáférést kér webalkalmazás fut példa szervezet ad az Azure-ban futó webes alkalmazáshoz. A partner szervezetnél az összevonási kiszolgáló hitelesíti kérelmeket helyben, és elküldi az Azure-ban futó ADFS jogcímalapú tartalmazó biztonsági tokenek. Azure-ban ADFS ellenőrzi a biztonsági tokenek, és ha érvényes továbbíthatja-e a jogcímalapú webalkalmazás Azure-ban futó engedélyezi őket.

    >[AZURE.NOTE] A virtuális Magánhálózati alagutas Azure átjáró segítségével közvetlen hozzáférés biztosítása az ADFS megbízható partnerek számára is beállítható. Ezek a partnerek érkező kérések nem haladnia WAP kiszolgálókhoz.

## <a name="recommendations"></a>Javaslatok

Ez a szakasz összefoglalja javaslatok ADFS Azure-végrehajtási kiterjedő:

- Virtuális javaslatok.

- Hálózati javaslatok.

- Elérhetőség javaslatok.

- Javaslatok biztonsági.

- ADFS-telepítés javaslatok.

- Javaslatok ADFS a megbízható gombra.

### <a name="vm-recommendations"></a>Virtuális javaslatok

Kezelheti a várható forgalmat mennyiségű kellő erőforrások VMs hozhat létre. Használja a meglévő gépek kiindulási pontként helyszíni ADFS tároló méretét. Az erőforrás-kihasználtság figyelése Méretezze át a VMs, és méretezni, ha túl nagy.

Kövesse a javaslatok [fut a Windows Azure virtuális gép]felsorolt[vm-recommendations].

### <a name="networking-recommendations"></a>Hálózati javaslatok

Állítsa be a hálózati kapcsolaton minden egyes az ADFS- és WAP kiszolgálók statikus magánjellegű IP-címeket tároló VMs.

Nem ADFS-VMs nyilvános IP-címek ad. További tudnivalókért lásd: a [biztonsági megfontolások][security-considerations].

Az IP-cím, az elsődleges és másodlagos DNS-kiszolgálóiról az egyes ADFS és WAP virtuális hivatkozást a FELVESZI VMs (amely futnia kell DNS) a hálózati kapcsolatok beállítása. Ez a lépés nem szükséges ahhoz, hogy minden virtuális csatlakozni a tartományhoz.

### <a name="availability-recommendations"></a>Elérhetőség javaslatok

Hozzon létre egy ADFS-farm legalább két kiszolgálók növelik a ADFS szolgáltatás elérhetőségét.

A farmban minden ADFS virtuális eltérő tárolási fiókokba. Ezt a megközelítést lehetővé teszi, hogy egy egyetlen tárolási fiókban hibába nem javítja a teljes farm nem érhető el.

Hozzon létre külön Azure elérhetőségének beállítása az ADFS- és WAP VMs. Győződjön meg arról, hogy nincsenek-e legalább két VMs minden egyes készletben. Egyes elérhetőségének beállítása legalább két frissítés tartományok és két hibafa tartományokat kell rendelkeznie.

Konfigurálja a terheléselosztókkal az ADFS VMs és WAP VMs az alábbi képlettel történik:

- Olvassa el az Azure terheléselosztó ahhoz, hogy a külső hozzáférést a WAP VMs és a betöltés terjesztheti az ADFS farm kiszolgálóin található az ADFS-belső terheléselosztó.

- Csak a sikeres forgalom jelenjen meg a 443-as port (HTTPS) ADFS/WAP kiszolgálókhoz.

- Adjon meg egy statikus IP-címet a terheléselosztó.

- Hozzon létre egy állapot vizsgálati HTTPS helyett a TCP protokoll használatával. Ellenőrizze, hogy működik-e az ADFS-kiszolgálóra 443-as port is küldjön ping parancsot.

    >[AZURE.NOTE] ADFS-kiszolgálók a kiszolgáló neve megjelölése (SNI) protokoll, és úgy kísérel meg a betöltés terheléselosztó sikertelen HTTPS zárólap használatával probe használják.

- *A* DNS-rekord hozzáadása a tartományt az ADFS-terheléselosztó. 

    Adja meg a terheléselosztó IP-címét, és adja meg a tartomány (például adfs.contoso.com) nevet. Ez a, a nevet, amellyel ügyfelek és a WAP kiszolgálók elérni az ADFS kiszolgálófarm.

### <a name="security-recommendations"></a>Javaslatok biztonsági

Megakadályozhatja, hogy az ADFS-kiszolgálók közvetlen módon kapcsolódik az internethez. ADFS-kiszolgálók azok a tartományhoz tartozó számítógépek, amelyek a teljes engedélyezési biztonsági tokenek megadását. ADFS-kiszolgálóra sérül meg, ha egy rosszindulatú felhasználó állíthatnak teljes hozzáférési jogkivonat webalkalmazások és az összevonási kiszolgálók ADFS eső. Ha a rendszer a külső felhasználók nem feltétlenül csatlakozás megbízható partnertől webhelyekről kérelmeket kell kezelni, WAP kiszolgálók használatával ezeket kérések kezelésére. További tudnivalókért lásd: [az összevonási kiszolgálóproxyk helye][where-to-place-an-fs-proxy].

Hely ADFS és saját tűzfallal különálló alhálózathoz WAP kiszolgálók. NSG szabályok segítségével tűzfalszabályokat határozza meg. Ha szüksége van átfogóbb védelmet alkalmazhat egy további biztonsági külső kiszolgálók körül alhálózat és NVAs, használatával leírtak szerint a dokumentum [egy biztonságos hibrid hálózat architektúrája Azure-ban Internet-hozzáféréssel rendelkező végrehajtási][implementing-a-secure-hybrid-network-architecture-with-internet-access]. Figyelje meg, hogy az összes tűzfalak lehetővé teszi a (HTTPS) 443-as port forgalmat.

Bejelentkezés közvetlen hozzáférés korlátozása ADFS- és WAP kiszolgálókhoz. Csatlakozás csak DevOps oktatói láthatja.

A WAP kiszolgálók nem csatlakozik a tartományhoz.

### <a name="adfs-installation-recommendations"></a>ADFS-telepítés javaslatok

A cikk [telepíti az összevonási kiszolgáló Farm] [ Deploying_a_federation_server_farm] részletes útmutatás való telepítéséről és konfigurálásáról ADFS. Az első ADFS-kiszolgálóra konfigurálása a farmban előtt, hajtsa végre az alábbi műveleteket:

1. Nyilvánosan megbízható elvégzéséhez kiszolgálói hitelesítési tanúsítvány beszerzése. A *Tárgy nevét* a nevet, amellyel az ügyfelek hozzáférni az összevonási szolgáltatás kell tartalmaznia. Ez lehet a DNS-terheléselosztó, például *adfs.contoso.com* regisztrált nevét (például: a helyettesítő nevek használatának kerülése **. contoso.com*, biztonsági okokból). A minden ADFS-kiszolgálóra VMs tanúsítványt használja. Egy tanúsítványt egy megbízható hitelesítésszolgáltató vásárolhat, de ha szervezete használja az Active Directory-tanúsítvány szolgáltatások is létrehozhatja saját. 

    A *helyettesítő tulajdonosneve* külső eszközökről hozzáférés engedélyezése a DRS használja. Ez az űrlap *enterpriseregistration.contoso.com*kell lenniük.

    Lásd: További információ [beszerzése és az ADFS SSL-tanúsítvány beállítása][adfs_certificates].

2. Kattintson a tartományvezérlőnek a kulcs terjesztési szolgáltatás új legfelső szintű kulcs generálása. Adja meg a pontos idő (Ez a beállítás csökkenti a késleltetés, amely akkor fordulhat elő, terjesztése, és a billentyűk szinkronizálása végig a tartomány) 10 óra mínusz kell hatékony idővel. Ez a lépés nem támogatja az ADFS-szolgáltatás futtatásához használt csoport-szolgáltatási fiók létrehozása szükséges. Az alábbi Powershell-parancsot az ennek módjáról látható:

    ```powershell
    Add-KdsRootKey -EffectiveTime (Get-Date).AddHours(-10)
    ```

3. Minden egyes ADFS-kiszolgálóra virtuális hozzáadása a tartomány.

>[AZURE.NOTE] Telepítheti az ADFS, a tartományvezérlőnek, az elsődleges irányító FSMO szerepkört, a tartomány futtató futtatása és az ADFS VMs elérhető kell lennie.

### <a name="adfs-trust-recommendations"></a>ADFS megbízható javaslatok

Összevonás megbízhatónak a ADFS-telepítés és bármely partner szervezetek az összevonási kiszolgálók között. Szűrés követelések és -megfeleltetés szükséges beállítása. Ez a folyamat van szükség:

- DevOps dolgozói **minden partner szervezetnél** hozzáadása egy megbízó felek adatvédelmi az ADFS-kiszolgálókon keresztül érhető el a webalkalmazások esetében.

- DevOps dolgozói **a szervezet** konfigurálása jogcímalapú-szolgáltató adatvédelmi ahhoz, hogy a partner szervezetek nyújtó jogcímalapú megbízhatónak az ADFS-kiszolgálókon.

- DevOps dolgozói **a szervezet** konfigurálása jogcímalapú webalkalmazások a szervezet megjelenítheti átadni ADFS.

    További tudnivalókért lásd: a [Létrehozó összevonási megbízható][establishing-federation-trust].

Tegye közzé a szervezet webalkalmazások és elérhetővé tétele a külső partnerek használatával előhitelesítésre a WAP kiszolgálón keresztül. További tudnivalókért lásd: a [alkalmazások közzététele ADFS előhitelesítés használata][publish_applications_using_AD_FS_preauthentication]

Figyelje meg, hogy ADFS jogkivonat átalakítási és vázak támogatja-e. Azure Active Directory nem nyújt ezt a szolgáltatást. ADFS-alapú a bizalmi kapcsolatok beállításakor van lehetősége:

- Állítsa be a állítást átalakítások engedélyezési szabályokat. Például az, amit a ÖSSZEADJA a szervezet engedélyezheti egy-Microsoft partnerrel szervezet által használt ábrázolhatók csoport biztonsági képezhető.

- Átalakítás követelések az egyik formátumról a másikra. Például hozzárendelheti a SAML 2.0-s SAML 1.1, ha az alkalmazás csak a SAML 1.1 követelések támogatja. 

## <a name="availability-considerations"></a>Elérhetőség kapcsolatos szempontok

SQL Server- vagy a Windows belső adatbázis (WID) segítségével ADFS konfigurációs adatok tárolásához. WID egyszerű redundancia biztosít. Módosítások kerülnek közvetlenül az ADFS-adatbázisok a ADFS fürt közül csak a többi kiszolgáló ki replikációs használatával az adatbázisát naprakészen tartása közben. Adja meg az SQL Server használatával is teljes adatbázist redundancia és magas elérhetősége feladatátvevő fürtözés vagy tükrözési használatával.

## <a name="security-considerations"></a>Biztonsági megfontolások

ADFS a HTTPS protokollt használja, ezért ügyeljen, hogy a NSG szabályokat tartalmazó a webes alhálózat az első csoportba tartozó VMs engedély HTTPS-kérések. Ezek a kérelmek a alhálózat, amelyben a webes réteg, üzleti réteg, adatok réteg, személyes DMZ, nyilvános DMZ és az ADFS-kiszolgálókat tartalmazó alhálózat is származnak a helyszíni hálózaton.

Fontolja meg inkább hálózati virtuális készülékek csoportja, amely naplózza a forgalmat a virtuális hálózat széle célra naplózásának áthaladó részletes tájékoztatást.

## <a name="scalability-considerations"></a>Méretezhetőség kapcsolatos szempontok

A következő szempontokat mérlegelve, a dokumentumból [ADFS megtervezése]összegzett[plan-your-adfs-deployment], adjon kiindulási pont ADFS farmok méretezési:

- Ha 1000-nél kevesebb felhasználó ne hozzon létre saját ADFS-kiszolgálók, de inkább ADFS telepítése minden ÖSSZEADJA a felhőben kiszolgálót (Győződjön meg arról, hogy legalább két ÖSSZEADJA kiszolgálók, hogy rendelkezésre álljon). Egyetlen WAP kiszolgálót hozhat létre.

- Ha 1000 és 15000 felhasználók között, hozzon létre két dedikált ADFS és két dedikált WAP kiszolgálókon.

- Ha 15000 és 60000 felhasználók között, hozzon létre három-öt dedikált ADFS, és legalább két dedikált WAP kiszolgálók között.

Ezek az adatok feltételezik kettős quad-core VMs (normál D4_v2 vagy jobb) szeretné üzemeltetni a kiszolgálókat Azure-ban használni.

Ne feledje, hogy a Windows belső adatbázis használatakor ADFS konfigurációs adatok tárolására, korlátozott, a farmban nyolc ADFS-kiszolgálóra. Ha várhatóan sok erre szolgáló egyebek, majd az SQL Server használja. További tudnivalókért lásd: [A szerepkör a ADFS konfigurációs adatbázis][adfs-configuration-database].

## <a name="management-considerations"></a>Adatkezelési kapcsolatos szempontok

Az alábbi feladatok elvégzéséhez DevOps oktatói kell készíteni:

- Az összevonási kiszolgálók (az ADFS-farm kezelése, az összevonási kiszolgálók adatvédelmi házirendjének kezeléséhez és kezelése az összevonási szolgáltatások által használt tanúsítványokat) kezelése.

- A WAP kiszolgálók (WAP tanúsítványok kezelése WAP farm kezelése) kezelése.

- (Megbízó felek, hitelesítési módszereket és követelések hozzárendelések konfigurálása) webalkalmazások kezelése.

- ADFS-összetevők mentéséről.

## <a name="monitoring-considerations"></a>Megfigyeléssel kapcsolatos szempontok

A [Microsoft System Center felügyeleti csomag az Active Directory összevonási szolgáltatások 2012 R2] [ oms-adfs-pack] tartalmaz, mind a megelőző, és a érzékeny figyelemmel kísérése, az összevonási kiszolgáló az ADFS-telepítés. A felügyeleti csomag figyeli:

- Az események, hogy az ADFS-alapú szolgáltatási az ADFS-eseménynaplók rekordjaihoz.

- A teljesítményt az ADFS-teljesítmény számláló gyűjtött adatokat. 

- Az általános állapotát, az ADFS rendszerben és webalkalmazások (megbízó felek), és lehetővé teszi a riasztások fontos problémákat és figyelmeztetések.

## <a name="solution-components"></a>Megoldás-összetevők

Megoldás mintaparancsfájl, [Deploy-ReferenceArchitecture.ps1][solution-script], érhető el, hogy is használhatja, amely követi a jelen cikkben ismertetett javaslatok architektúrája végrehajtása. Ez a parancsfájl Azure erőforrás-kezelő sablonok használja. A sablonok állnak rendelkezésre, alapvető építőelemek csoportja, amelyek hajt végre egy adott műveletet, például egy VNet létrehozásához, vagy egy NSG konfigurálása. A parancsprogram célja téve a sablon telepítésének.

A sablonok vannak paraméteres, külön JSON-fájlokban tárolt paraméterekkel. A paraméterek ezeket a fájlokat a saját igényeinek kielégítéséhez telepítésének konfigurálásához módosíthatók. Nem kell módosítani kell a sablonok saját maguk. Figyelje meg, hogy a sémák az objektumok paraméter fájlokban nem módosítható.

A sablonok szerkesztésekor létrehozása objektumok [Elnevezési szabályai ajánlott Azure erőforrások]ismertetett elnevezési szabályokat követő[naming-conventions].

A minta megoldást hoz létre, és a felhőben webes réteg, üzleti réteg, és adat-hozzáférési szint összetevőket, virtuális Magánhálózati átjáró és kezelési réteg hozzáadása alhálózat és kiszolgálók, az ADFS-alhálózat és, ADFS-proxy alhálózat és kiszolgálók DMZ magában foglaló állítja be a környezetet. A minta megoldást is hozhat létre egy szimulált a helyszíni környezet választható konfiguráció.

Az alábbi szakaszok ismertetik a helyszíni elemei, és a felhő konfigurációk.

### <a name="on-premises-components"></a>A helyszíni összetevők

>[AZURE.NOTE] Ezek az összetevők nem a fő fókusz a architektúra, a jelen dokumentum ismerteti, és állnak rendelkezésére egyszerűen ad tesztkörnyezetben a felhőben biztonságosan, nem oszlophivatkozás használatával valós munkakörnyezetben lehetőséget. Emiatt a szakasz csak a kulcs paraméter fájlokat foglalja össze. Módosíthatja a beállításokat, például az IP-címek vagy a VMs méretű, de célszerű változatlan marad a további paramétereket számos hagyja.

Ez a környezet AD egy tartományt, akkor a contoso.com nevű erdőn magába foglalja. A tartomány két ÖSSZEADJA kiszolgáló IP-címek 192.168.0.4 és 192.168.0.5 tartalmazza. Ezek a két kiszolgálók futtatása a DNS-szolgáltatás is. A helyi rendszergazdafiók mindkét VMs a meghívott `testuser` jelszóval `AweS0me@PW`. Emellett a konfigurációs hoz létre egy virtuális Magánhálózati átjárót a felhőbeli VNet csatlakozhat. A következő JSON-fájlokat a [**Paraméterek/helyi**] található szerkesztésével módosíthatja a konfigurációs[ on-premises-folder] mappa:

- ** [virtualNetwork.parameters.json][on-premises-vnet-parameters]**. Ez a fájl határozza meg, hogy a hálózati címterületet a helyszíni környezetben.

- ** [virtualMachines-adds.parameters.json][on-premises-virtualmachines-adds-parameters]**. A fájl tartalmaz a helyszíni VMs szolgáltatója szolgáltatások ÖSSZEADJA az adatokat. Alapértelmezés szerint két *Standard-DS3-v2* VMs jönnek létre.

- ** [virtualNetworkGateway.parameters.json] [ on-premises-virtualnetworkgateway-parameters] ** és ** [connection.parameters.json][on-premises-connection-parameters]**. Ezeket a fájlokat a felhőben, beleértve az átjáró áthaladó forgalom védelmére használandó megosztott kulcs tartsa lenyomva a virtuális Magánhálózati kapcsolatot az Azure virtuális Magánhálózati átjáró beállításait.

A hátralévő a mappában lévő fájlok a helyszíni tartomány Ez az infrastruktúra létrehozásához használt konfigurációs adatokat tartalmazzák. Segítségükkel ÖSSZEADJA telepítése, beállítása DNS, hozzon létre egy erdőt és az erdő a replikáció webhelyek konfigurálása.

### <a name="cloud-components"></a>Felhőalapú összetevők

Ezek az összetevők e architektúra alapvető alkotnak. A [**Paraméterek/azure**] [ azure-folder] mappa tartalmazza a következő paraméterek fájlok konfigurálásához az alábbi összetevőket:

- ** [virtualNetwork.parameters.json][vnet-parameters]**. Ez a fájl a VNet felépítése a VMs és más összetevőket a felhőben határozza meg. Beállítások, például a nevet, címterület, alhálózat és minden szükséges DNS-kiszolgáló címét tartalmazza. Figyelje meg, hogy a DNS-címek, ebben a példában látható hivatkozást a helyszíni DNS-kiszolgálók, valamint az alapértelmezett Azure DNS-kiszolgáló IP-címét. Ezeket a címeket, ha nem használ a minta a helyszíni környezetben hivatkozni saját DNS-beállításainak módosítása:

    ```json
    {
        "virtualNetworkSettings": {
            "value": {
                "name": "ra-adfs-vnet",
                "resourceGroup": "ra-adfs-network-rg",
                "addressPrefixes": [
                    "10.0.0.0/16"
                ],
                "subnets": [
                    {
                        "name": "dmz-private-in",
                        "addressPrefix": "10.0.0.0/27"
                    },
                    {
                        "name": "dmz-private-out",
                        "addressPrefix": "10.0.0.32/27"
                    },
                    {
                        "name": "dmz-public-in",
                        "addressPrefix": "10.0.0.64/27"
                    },
                    {
                        "name": "dmz-public-out",
                        "addressPrefix": "10.0.0.96/27"
                    },
                    {
                        "name": "mgmt",
                        "addressPrefix": "10.0.0.128/25"
                    },
                    {
                        "name": "GatewaySubnet",
                        "addressPrefix": "10.0.255.224/27"
                    },
                    {
                        "name": "web",
                        "addressPrefix": "10.0.1.0/24"
                    },
                    {
                        "name": "biz",
                        "addressPrefix": "10.0.2.0/24"
                    },
                    {
                        "name": "data",
                        "addressPrefix": "10.0.3.0/24"
                    },
                    {
                        "name": "adds",
                        "addressPrefix": "10.0.4.0/27"
                    },
                    {
                        "name": "adfs",
                        "addressPrefix": "10.0.5.0/27"
                    },
                    {
                        "name": "proxy",
                        "addressPrefix": "10.0.6.0/27"
                    }
                ],
                "dnsServers": [
                    "192.168.0.4",
                    "192.168.0.5",
                    "168.63.129.16"
                ]
            }
        }
    }
    ```

- ** [virtualMachines-adds.parameters.json ] [ virtualmachines-adds-parameters] **. Ez a fájl konfigurálja a VMs operációs rendszert futtató ÖSSZEADJA a felhőben. A konfiguráció két VMs áll. A rendszergazdai felhasználónevével és a jelszót a módosítania kell a `virtualMachineSettings` szakaszra, és tetszés szerint módosíthatja a virtuális méretét annak a tartománynak az igényeknek megfelelően alakíthatja:

    További tudnivalókért lásd: az [Azure Active Directory bővítése][extending-ad-to-azure].

    ```json
            "virtualMachinesSettings": {
                "value": {
                    "namePrefix": "ra-adfs-ad",
                    "computerNamePrefix": "aad",
                    "size": "Standard_DS3_v2",
                    "osType": "Windows",
                    "adminUsername": "testuser",
                    "adminPassword": "AweS0me@PW",
                    "osAuthenticationType": "password",
                    "nics": [
                        {
                            "isPublic": "false",
                            "subnetName": "adds",
                            "privateIPAllocationMethod": "Static",
                            "startingIPAddress": "10.0.4.4",
                            "enableIPForwarding": false,
                            "dnsServers": [
                            ],
                            "isPrimary": "true"
                        }
                    ],
                    "imageReference": {
                        "publisher": "MicrosoftWindowsServer",
                        "offer": "WindowsServer",
                        "sku": "2012-R2-Datacenter",
                        "version": "latest"
                    },
                    "dataDisks": {
                        "count": 1,
                        "properties": {
                            "diskSizeGB": 127,
                            "caching": "None",
                            "createOption": "Empty"
                        }
                    },
                    "osDisk": {
                        "caching": "ReadWrite"
                    },
                    "extensions": [
                    ],
                    "availabilitySet": {
                        "useExistingAvailabilitySet": "No",
                        "name": "ra-adfs-as"
                    }
                }
            },
            "virtualNetworkSettings": {
                "value": {
                    "name": "ra-adfs-vnet",
                    "resourceGroup": "ra-adfs-network-rg"
                }
            },
            "buildingBlockSettings": {
                "value": {
                    "storageAccountsCount": 2,
                    "vmCount": 2,
                    "vmStartIndex": 1
                }
            }
        }
    ```

- ** [hozzáadása – ad-tartomány-controller.parameters.json][add-adds-domain-controller-parameters]**. Ezt a fájlt tartalmazza a beállításait a CONTOSO tartomány felvétele kiszolgálókon tartó hozhat létre. A tartomány létrehozása és hozzáadása kiszolgálókon hozzáadása egyéni bővítmények használ. Amíg nem hoz létre a további hozzáadása kiszolgálók (ebben az esetben meg kell adhat hozzá a `vms` tömb), a nevük módosíthatja az alapértelmezett, vagy hozzon létre egy tartományt, nem kell ezzel a fájllal módosítása más névvel szeretné.

- ** [loadBalancer-adfs.parameters.json] [loadbalancer-adfs-parameters] ** A fájl tartalmaz a beállítandó két csoportját. A `virtualMachineSettings` szakaszban határozza meg a VMs, amely a felhőben az ADFS-szolgáltatás üzemelteti. Alapértelmezés szerint a parancsfájl hoz létre két az alábbiak VMs azonos elérhetősége megadása:

    ```json
        "virtualMachinesSettings": {
            "value": {
                "namePrefix": "ra-adfs-adfs",
                "computerNamePrefix": "adfs",
                "size": "Standard_DS1_v2",
                "osType": "windows",
                "adminUsername": "testuser",
                "adminPassword": "AweS0me@PW",
                "osAuthenticationType": "password",
                "nics": [
                    {
                        "isPublic": "false",
                        "subnetName": "adfs",
                        "privateIPAllocationMethod": "Static",
                        "startingIPAddress": "10.0.5.4",
                        "isPrimary": "true",
                        "enableIPForwarding": false,
                        "dnsServers": [ ]
                    }
                ],
                "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version": "latest"
                },
                "dataDisks": {
                    "count": 1,
                    "properties": {
                        "diskSizeGB": 128,
                        "caching": "None",
                        "createOption": "Empty"
                    }
                },
                "osDisk": {
                    "caching": "ReadWrite"
                },
                "extensions": [ ],
                "availabilitySet": {
                    "useExistingAvailabilitySet": "No",
                    "name": "ra-adfs-adfs-vm-as"
                }
            }
        }
        ...
        "buildingBlockSettings": {
            "value": {
                "storageAccountsCount": 2,
                "vmCount": 2,
                "vmStartIndex": 1
            }
        }
    ```

    A `loadBalancerSettings` a témakör a leírását a terheléselosztó ezek VMs. A terheléselosztó átadja a forgalmat, amely akkor jelenik meg (HTTPS) 443-as port valamelyikének a VMs:

    ```json
        "loadBalancerSettings": {
            "value": {
                "name": "ra-adfs-adfs-lb",
                "frontendIPConfigurations": [
                    {
                        "name": "ra-adfs-adfs-lb-fe",
                        "loadBalancerType": "internal",
                        "internalLoadBalancerSettings": {
                            "privateIPAddress": "10.0.5.30",
                            "subnetName": "adfs"
                        }
                    }
                ],
                "backendPools": [
                    {
                        "name": "ra-adfs-adfs-lb-bep",
                        "nicIndex": 0
                    }
                ],
                "loadBalancingRules": [
                    {
                        "name": "https-rule",
                        "frontendPort": 443,
                        "backendPort": 443,
                        "protocol": "Tcp",
                        "backendPoolName": "ra-adfs-adfs-lb-bep",
                        "frontendIPConfigurationName": "ra-adfs-adfs-lb-fe",
                        "probeName": "https-probe",
                        "enableFloatingIP": false
                    }
                ],
                "probes": [
                    {
                        "name": "https-probe",
                        "port": 443,
                        "protocol": "Tcp",
                        "requestPath": null
                    }
                ],
                "inboundNatRules": [ ]
            }
        },
        "virtualNetworkSettings": {
            "value": {
                "name": "ra-adfs-vnet",
                "resourceGroup": "johns-adfs-network-rg"
            }
        }
    ```

- ** [adfs-farm-tartomány-join.parameters.json ] [ adfs-farm-domain-join-parameters] **. A fájl tartalmaz a beállításokat, amelyek az ADFS-kiszolgálók hozzáadása a CONTOSO tartományban. Csak akkor van szüksége ezzel a fájllal módosíthatja, ha a létrehozott további ADFS-kiszolgálók (frissítse a `vms` ebben az esetben a tömb), vagy módosította a tartomány nevét.

- ** [gmsa.parameters.json][gmsa-parameters]**, ** [adfs-farm-first.parameters.json][adfs-farm-first-parameters]**, és ** [adfs-farm-rest.parameters.json][adfs-farm-rest-parameters]**. A parancsprogram beállításai a ezeket a fájlokat az ADFS kiszolgálófarm létrehozásához. 

    A *gmsa.parameters.json* fájlt tartalmazza a felügyelt csoport szolgáltatásfiók az ADFS-szolgáltatás által használt beállításait. Ez a fájl módosíthatja, ha másnak szeretné módosítani a fiók vagy a tartomány nevét.

    Az *adfs-farm-first.parameters.json* fájl létrehozásához a ADFS kiszolgálófarm és az első kiszolgáló szükséges adatokat tartalmazza. Ha úgy módosította, a tartomány vagy a csoport felügyelt szolgáltatásfiók nevét, frissítenie kell a fájlt.

    Az *adfs-farm-rest.parameters.json* fájl adja hozzá a fennmaradó ADFS-kiszolgálókat a farm szolgál. Újra Ha megváltoztatta a tartomány vagy a csoport felügyelt szolgáltatásfiók nevét, frissítenie kell a fájlt. Frissítés a `vms` tömb, ha további ADFS-kiszolgálók létre.

- ** [loadBalancer-adfsproxy.parameters.json][loadBalancer-adfsproxy-parameters]**. Ez a fájl hasonlít a szerkezetére és tartalmára a *loadBalancer-adfs.parameters.json* fájlt. Az ADFS proxykiszolgálók és terheléselosztó készítéséhez adatokat tartalmazza.

- ** [adfsproxy-farm-first.parameters.json][adfsproxy-farm-first-parameters]**, és ** [adfsproxy-farm-rest.parameters.json][adfsproxy-farm-rest-parameters]**. A parancsprogram beállításai a ezeket a fájlokat az ADFS-proxy kiszolgálófarm létrehozásához. 

- ** [virtualNetworkGateway.parameters.json][virtualnetworkgateway-parameters]**. A fájl tartalmaz a beállításokat, amelyek az Azure virtuális Magánhálózati átjáró létrehozása a felhőben, használja a helyszíni hálózaton. Módosítania kell a `sharedKey` az az érték a `connectionsSettings` megfelelően, hogy a helyszíni VPN eszköz szakaszban. További tudnivalókért lásd: a [végrehajtási egy hibrid hálózat architektúrája Azure és a helyszíni VPN][hybrid-azure-on-prem-vpn].

- ** [dmz-private.parameters.json] [ dmz-private-parameters] ** és ** [dmz-public.parameters.json ] [ dmz-public-parameters] **. Ezeket a fájlokat konfigurálása (nyilvános) bejövő és kimenő (személyes) oldalán a VMs, amely tartalmazza a DMZ védelme a kiszolgálókat, a felhőben. Ezeket az elemeket és a konfigurációs kapcsolatos további tudnivalókért lásd: a [végrehajtási Azure és az Internet között a DMZ][implementing-a-secure-hybrid-network-architecture-with-internet-access].

- ** [loadBalancer-web.parameters-json][loadBalancer-web-parameters]**, ** [loadBalancer-biz.parameters-json][loadBalancer-biz-parameters]**, és ** [loadBalancer-data.parameters-json][loadBalancer-data-parameters]**. A paraméterek fájlok az adatok, üzleti és web access rétegek virtuális specifikációi, és állítsa be az egyes réteg terheléselosztókkal. Ezek a VMs a web Apps alkalmazások és az adatbázisok, és végezze el a vállalati feladatok a szervezet számára. A méret és a VMs minden réteg igényeinek megfelelően módosíthatja.

- ** [virtualMachines-mgmt.parameters.json][virtualMachines-mgmt-parameters]**. Ez a fájl az Ugrás párbeszédpanel/adatkezelési VMs adatokat tartalmazza. Csak akkor lehet bejelentkezési és felügyeleti hozzáférés szükséges a webes, üzleti és adatok rétegek VMs való az Ugrás mezőből. Alapértelmezés szerint a parancsfájl hoz létre egy egyetlen *Standard_DS1_v2* virtuális, de a fájl méretét, illetve további VMs létrehozása: Ha a terhelést a kezelés valószínűleg jelentős módosíthatja.

## <a name="solution-deployment"></a>Megoldás üzembe helyezése

A megoldás feltételezi, hogy az alábbi előfeltételek:

- Ha egy meglévő Azure előfizetésbe erőforrás csoportokat hozhat létre.

- Letöltött és Azure Powershell legújabb változatát telepítette. Lásd: [Itt] [ azure-powershell-download] utasításokat.

Futtatása, amely a megoldást üzembe helyezése:

1. A helyi számítógépen kényelmes mappa áthelyezése és a következő almappák létrehozása:

    - Parancsfájlok

    - Parancsfájlok és paraméterek

    - Parancsfájlok/paraméterek/helyi

    - Parancsfájlok/paramétereket és Azure

2. Töltse le a [Központi telepítés-ReferenceArchitecture.ps1] [ solution-script] fájlt a parancsfájlok mappába

3. Töltse le a [Paraméterek/helyi] tartalmának[ on-premises-folder] parancsfájlok/paraméterek/helyi mappa:

4. Töltse le a [Paraméterek/azure] tartalmának[ azure-folder] parancsfájlok/paramétereket és Azure mappa.

5. A parancsfájlok mappát a Deploy-ReferenceArchitecture.ps1 fájl, és a következő sorokat megadhatja az erőforrás-csoportokat kell létrehozott vagy az erőforrások hozta létre a parancsfájl tárolására szolgáló módosítása:

    ```powershell
    # Azure Onpremise Deployments
        $onpremiseNetworkResourceGroupName = "ra-adfs-onpremise-rg"
        
        # Azure ADDS Deployments
        $azureNetworkResourceGroupName = "ra-adfs-network-rg"
        $workloadResourceGroupName = "ra-adfs-workload-rg"
        $securityResourceGroupName = "ra-adfs-security-rg"
        $addsResourceGroupName = "ra-adfs-adds-rg"
        $adfsResourceGroupName = "ra-adfs-adfs-rg"
        $adfsproxyResourceGroupName = "ra-adfs-proxy-rg"
    ```

6. A parancsfájlok/paraméterek/helyi rendszerekhez és parancsfájlok/paramétereket és Azure mappák a paraméter fájlok szerkesztése. Frissítse az erőforrás csoport hivatkozások ezeket a fájlokat a központi telepítés-ReferenceArchitecture.ps1 fájlban a változók hozzárendelt erőforrás csoportokat azoknak megfelelően. Az alábbi táblázat bemutatja, hogy mely paraméter fájlok melyik erőforráscsoport hivatkozást. Figyelje meg, hogy az *TS-adfs-terhelés-rg*, *TS-adfs-biztonsági-rg*, *TS-adfs-felveszi-rg*, *TS-adfs-adfs-rg*és *TS-adfs-proxy-rg* csak a PowerShell-parancsprogramot használt és fordul elő a paraméter fájlokat.

  	|Erőforráscsoport|Paraméter fájlokat|
  	|--------------|--------------|
  	|TS-adfs-helyi-rg|parameters\onpremise\connection.Parameters.JSON<br /> parameters\onpremise\virtualMachines-ADDS.Parameters.JSON<br />parameters\onpremise\virtualNetwork-ADDS-DNS.Parameters.JSON<br />parameters\onpremise\virtualNetwork.Parameters.JSON<br />parameters\onpremise\virtualNetworkGateway.Parameters.JSON<br />parameters\azure\virtualNetworkGateway.Parameters.JSON
  	|TS-adfs-hálózaton-rg|parameters\onpremise\connection.Parameters.JSON<br />parameters\azure\dmz-private.Parameters.JSON<br />parameters\azure\dmz-public.Parameters.JSON<br />parameters\azure\loadBalancer-ADFS.Parameters.JSON<br />parameters\azure\loadBalancer-adfsproxy.Parameters.JSON<br />parameters\azure\loadBalancer-biz.Parameters.JSON<br />parameters\azure\loadBalancer-Data.Parameters.JSON<br />parameters\azure\loadBalancer-Web.Parameters.JSON<br />parameters\azure\virtualMachines-ADDS.Parameters.JSON<br />parameters\azure\virtualMachines-Mgmt.Parameters.JSON<br />parameters\azure\virtualNetwork-with-OnPremise-and-Azure-DNS.Parameters.JSON<br />parameters\azure\virtualNetwork.Parameters.JSON<br />parameters\azure\virtualNetworkGateway.Parameters.JSON (*két előfordulás*)

    Ezenkívül állítsa át a konfigurációt a helyszíni telepítésű és a felhő összetevők, a [megoldás összetevők], [megoldás-összetevők] szakaszban leírt módon.

7. Nyisson meg egy Azure PowerShell ablakot, helyezze át a parancsfájlok mappába, és futtassa a következő parancsot:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> <mode>
    ```

    Csere `<subscription id>` az Azure előfizetés azonosítójával.

    A `<location>`, például: Adja meg, az Azure régió `eastus` vagy `westus`.

    A `<mode>` paraméter a következő értékek egyike lehet:

    - `Onpremise`, és a szimulált a helyszíni környezet létrehozása.

    - `Infrastructure`, a VNet infrastruktúra létrehozása és Ugrás a mező a felhőben.

    - `CreateVpn`, Azure virtuális hálózati átjáró létre, és csatlakoztassa a helyszíni hálózaton.

    - `AzureADDS`, a ÖSSZEADJA kiszolgálók eljáró VMs Egyenletszerkesztővel, telepítse az Active Directory ezek VMs, és hozza létre a tartományt a felhőben.

    - `AdfsVm`az ADFS VMs létrehozásához, és vegye fel őket a tartományba a felhőben.

    - `ProxyVm`az ADFS-proxy VMs létre, és vegye fel őket a tartományba a felhőben.

    - `Prepare`, amely végrehajtja a fenti feladatokat. **Az ajánlott a lehetőség, ha egy teljesen új telepítési készítésekor, és nem rendelkezik egy meglévő helyszíni infrastruktúra.**

    >[AZURE.NOTE] Parancsfájlt futtatása is egy `<mode>` paramétere: `Workload` a webes, a vállalati, és az adatok réteg VMs és a hálózati létrehozásához. Ez a beállítás nem része a `Prepare` módban.

    Ha a `Prepare` beállítást választja, a parancsfájl befejezéséhez több óráig tart, és az üzenettel *előkészítése befejeződik, nincs befejezve. Telepítse a tanúsítvány ADFS, és a proxy VMs.*

8.  Engedélyezi a DNS-beállítások csak akkor lépnek érvénybe, indítsa újra az Ugrás párbeszédpanel (*TS-adfs-kezelése-vm1* *TS-adfs-biztonsági-rg* csoportjának).

9.  [SSL-tanúsítvány beszerzése ADFS] [ adfs_certificates] és a tanúsítványának telepítése az ADFS VMs. Figyelje meg, hogy az Ugrás párbeszédpanel keresztül az ADFS VMs csatlakozhat. Az IP-címei *10.0.5.4* és *10.0.5.5*. Az alapértelmezett felhasználónév szó *contoso\testuser* jelszóval *AweSome@PW*.

    >[AZURE.NOTE] A megjegyzéseket a központi telepítés-ReferenceArchitecture.ps1 parancsfájl ezen a ponton tartalmaz egy próba önaláírt tanúsítvány és a hitelesítésszolgáltató használatával hozhat létre részletes útmutatást a `makecert` parancsot. Azonban nem használható a tanúsítványok munkakörnyezetben makecert által generált.

10. Futtassa az alábbi Powershell-parancsot a ADFS kiszolgálófarm beállítása:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Adfs
    ```

11. Az Ugrás párbeszédpanelen tallózással keresse meg *https://adfs.contoso.com/adfs/ls/idpinitiatedsignon.htm* való (jelenhet meg a tanúsítvány figyelmeztetés figyelmen kívül hagyhatja, a vizsgálathoz) ADFS-telepítés tesztelése. Ellenőrizze, hogy a Contoso Corporation bejelentkezési lapja jelenik meg. Jelentkezzen be, mint *contoso\testuser* jelszóval *AweS0me@PW*.

12. A hitelesítésszolgáltató tanúsítványának telepítése az ADFS-proxy VMs. Az IP-címei *10.0.6.4* és *10.0.6.5*.

13. Futtassa az alábbi Powershell parancsot az első ADFS proxykiszolgáló beállítása:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Proxy1
    ```

14. Kövesse az utasításokat, a parancsfájl jelenik meg az első ADFS proxykiszolgáló-telepítés tesztelése.

15. Futtassa az alábbi Powershell-parancsot a második első ADFS proxykiszolgáló beállítása:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Proxy2
    ```

16. Kövesse az utasításokat a parancsfájl jelenik meg a teljes ADFS-proxy beállítások tesztelése.

## <a name="next-steps"></a>Következő lépések

- [Azure Active Directory][aad].

- [Azure Active Directory B2C][aadb2c].

<!-- links -->

[vm-recommendations]: ./guidance-compute-single-vm.md#Recommendations
[naming-conventions]: ./guidance-naming-conventions.md
[implementing-active-directory]: ./guidance-identity-adds-extend-domain.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[DRS]: https://technet.microsoft.com/library/dn280945.aspx
[where-to-place-an-fs-proxy]: https://technet.microsoft.com/library/dd807048.aspx
[ADDRS]: https://technet.microsoft.com/library/dn486831.aspx
[plan-your-adfs-deployment]: https://msdn.microsoft.com/library/azure/dn151324.aspx
[ad_network_recommendations]: #network_configuration_recommendations_for_AD_DS_VMs
[domain_and_forests]: https://technet.microsoft.com/library/cc759073(v=ws.10).aspx
[adfs_certificates]: https://technet.microsoft.com/library/dn781428(v=ws.11).aspx
[create_service_account_for_adfs_farm]: https://technet.microsoft.com/library/dd807078.aspx
[import_server_authentication_certificate]: https://technet.microsoft.com/library/dd807088.aspx
[adfs-configuration-database]: https://technet.microsoft.com/en-us/library/ee913581(v=ws.11).aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[security-considerations]: #security-considerations
[recommendations]: #recommendations
[claims-aware applications]: https://msdn.microsoft.com/en-us/library/windows/desktop/bb736227(v=vs.85).aspx
[active-directory-federation-services-overview]: https://technet.microsoft.com/en-us/library/hh831502(v=ws.11).aspx
[establishing-federation-trust]: https://blogs.msdn.microsoft.com/alextch/2011/06/27/establishing-federation-trust/
[Deploying_a_federation_server_farm]: https://technet.microsoft.com/library/dn486775.aspx
[install_and_configure_the_web_application_proxy_server]: https://technet.microsoft.com/library/dn383662.aspx
[publish_applications_using_AD_FS_preauthentication]: https://technet.microsoft.com/library/dn383640.aspx
[managing-adfs-components]: https://technet.microsoft.com/library/cc759026.aspx
[oms-adfs-pack]: https://www.microsoft.com/download/details.aspx?id=41184
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[aad]: https://azure.microsoft.com/documentation/services/active-directory/
[aadb2c]: https://azure.microsoft.com/documentation/services/active-directory-b2c/
[adfs-intro]: ../active-directory/active-directory-aadconnect-azure-adfs.md
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/Deploy-ReferenceArchitecture.ps1
[on-premises-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adfs/parameters/onpremise
[on-premises-vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualNetwork.parameters.json
[on-premises-virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualMachines-adds.parameters.json
[on-premises-virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualNetworkGateway.parameters.json
[on-premises-connection-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/connection.parameters.json
[azure-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adfs/parameters/azure
[vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualNetwork.parameters.json
[dmz-private-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/dmz-private.parameters.json
[dmz-public-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/dmz-public.parameters.json
[virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualNetworkGateway.parameters.json
[hybrid-azure-on-prem-vpn]: ./guidance-hybrid-network-vpn.md
[virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualMachines-adds.parameters.json
[extending-ad-to-azure]: ./guidance-identity-adds-extend-domain.md
[loadbalancer-adfs-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-adfs.parameters.json
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/add-adds-domain-controller.parameters.json
[gmsa-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/gmsa.parameters.json
[adfs-farm-first-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-first.parameters.json
[adfs-farm-rest-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-rest.parameters.json
[adfs-farm-domain-join-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-domain-join.parameters.json
[loadBalancer-web-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-web.parameters.json
[loadBalancer-biz-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-biz.parameters.json
[loadBalancer-data-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-data.parameters.json
[virtualMachines-mgmt-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualMachines-mgmt.parameters.json
[loadBalancer-adfsproxy-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-adfsproxy.parameters.json
[adfsproxy-farm-first-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfsproxy-farm-first.parameters.json
[adfsproxy-farm-rest-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfsproxy-farm-rest.parameters.json
[0]: ./media/guidance-iaas-ra-secure-vnet-adfs/figure1.png "Hibrid hálózat architektúrája, az Active Directoryval, biztonságos"

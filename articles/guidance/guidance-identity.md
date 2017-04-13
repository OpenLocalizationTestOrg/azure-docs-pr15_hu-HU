<properties
   pageTitle="Azure-ban identitás kezelése |} Microsoft Azure"
   description="Ebből a cikkből megtudhatja, és összehasonlítja az identitás kezelése az Azure a-helyszíni és felhőbeli határ átterjedő hibrid rendszerben rendelkezésre álló különféle módon."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>
<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/26/2016"
   ms.author="telmosampaio"/>
   
# <a name="managing-identity-in-azure"></a>Azure-ban identitás kezelése

A legtöbb vállalati rendszerekben Windows alapján de kell használni az Active Directory (AD) szolgáltatásokat nyújtanak az identitás kezelése az alkalmazások. Active Directory működik jól a helyszíni környezetben, de a hálózati infrastruktúrára a felhőbe bővítésekor van néhány fontos döntéseket, hogy az identitás kezelése vonatkozóan. Kell a helyszíni tartomány, amelyekkel beépíthetők a felhőben VMs kibontása? Érdemes hoz létre új tartományokat a felhőben, és ha igen, hogyan? Érdemes alkalmazhat a saját erdő a felhőben és kell úgy, hogy az Azure Active Directory (AAD) használni?

Ez a cikk ismerteti az egyes általános beállításainak megadása, hogy a problémáit, ebben az esetben okozta értekezlet, és elemzéssel könnyebben megállapítható, melyik megoldást a legjobb felel meg az igényeinek, saját igényeknek megfelelően alakítható.

## <a name="using-azure-active-directory"></a>Azure Active Directory használata

AAD használhatja az Active Directory tartományi létrehozása a felhőben, és kapcsolja az elemet egy helyszíni Active Directory tartományi. AAD alkalmazások használatot a felhőben rendszert futtató felhasználók számára lehetővé teszi az egyszeri bejelentkezés (SSO) beállítása.

[! [0]][0]

AAD egyszerű módja a biztonsági tartomány megvalósítását a felhőben. Ez számos Microsoft-alkalmazások, például a Microsoft Office 365 van használni. 

AAD használatának előnyei:

- Nincs szükség a felhőben, az Active Directory-infrastruktúrát karbantartása nem. Teljes egészében felügyelt és a Microsoft által fenntartott AAD.

- AAD, amely a rendelkezésre álló helyszíni azonos identitás az adatokat tartalmazza.

- Hitelesítés akkor fordulhat elő, az Azure-csökkentése külső alkalmazásokban és a felhasználók kapcsolatba lépni a helyszíni tartomány van szükség.

Tudnivaló AAD használata esetén:

- Identitás szolgáltatások korlátozódik felhasználókhoz és csoportokhoz. Nem áll szolgáltatás és számítógépfiókok hitelesítést végezni.

- Kapcsolat a helyszíni tartomány megtartása a szinkronizált AAD könyvtár és meg kell adnia. 

- Ön a felelős, hogy a felhasználók hozzá tudnak férni a felhőben keresztül AAD alkalmazások közzététele.

Részletes tudnivalókért olvassa el az [Azure Active Directory végrehajtási][implementing-aad].

## <a name="using-active-directory-in-the-cloud-joined-to-an-on-premises-forest"></a>A helyszíni erdőbe csatlakozott az Active Directory használata a felhőben

Is megbízhat a helyszíni Active Directory-szolgáltatások (AD DS), de a hibrid helyzetben hol található az alkalmazások elemei Azure-ban való replikáció ezt a funkciót, és az Active Directory tárházba a felhőbe hatékonyabbá tehető. Ezt a megközelítést csökkentheti az a késés okozta hitelesítési küldése, és a helyi kéri a felhőből futó helyszíni Active Directory tartományi szolgáltatások biztonsági másolatot. 

[! [1]][1]

Ez a módszer, hogy a saját tartomány létrehozása a felhőben, és csatlakoztathatja a helyszíni erdő igényli. Az Active Directory Tartományi szolgáltatások tárolni VMs hoz létre.

Külön tartomány használata a felhőben előnyei:

- Lehetővé teszi a hitelesíteni a felhasználó, a szolgáltatás és a számítógép fiókokat helyszíni és felhőbeli.

- Rendelkezésre álló helyszíni azonos azonosító információkat hozzáférést biztosít.

- Nincs szükség külön AD erdő; kezelése a tartomány a felhőben a helyszíni erdő tartozhat.

- A helyszíni csoportházirend-objektum objektumok a tartományhoz a felhőben által meghatározott csoportházirend alkalmazhat.

A felhőben egy külön tartomány használatával kapcsolatos szempontok

- Megköveteli létrehozása és kezelése, az Active Directory tartományi szolgáltatások kiszolgálók és a tartomány a felhőben.

- Előfordulhat, hogy bizonyos szinkronizálási késés a tartomány kiszolgálóin a felhőben, és a kiszolgálón a helyszíni között.

A architektúra konfigurálásával kapcsolatos további tudnivalókért lásd [Bővítése az Active Directory címtár szolgáltatások (ÖSSZEADJA) az Azure][extending-adds].

## <a name="using-active-directory-with-a-separate-forest"></a>Az Active Directory egy külön erdővel használata

Előfordulhat, hogy egy szervezet helyszíni Active Directory (AD) futtató számos különböző tartományok alkotó erdő. Elkülönítési fokát, esetleg biztonsági okokból külön kell tartani funkcionális területen adja meg a tartományok is használhatja, de megoszthatja bizalmi kapcsolatok létrehozásával tartományok között.

[! [2]][2]

Egy szervezet, amelyik a külön tartományok kihasználhatja Azure szerint egy vagy több ezeket a tartományokat áthelyezése egy külön erdőbe a felhőben. Azt is megteheti egy szervezet előfordulhat, hogy kívánt összes felhő erőforrás adott tárolt helyszíni logikailag elkülönülő megtartása, és a felhő erőforrások adatainak tárolása saját címtárban is a felhőben tárolt erdő részeként.

Egy másik erdőben használata a felhőben előnyei:

- A helyszíni identitások végrehajtása, illetve távolítsa egymástól csak Azure identitások.

- Nem szükséges való replikáció a helyszíni Active Directory erdő Azure, hálózati késés hatásai csökkentése.

Szempontok

- Helyszíni identitások a felhőben hitelesítési hajt végre további hálózati *Ugrás* a helyszíni Active Directory-kiszolgálók.

- Az Active Directory tartományi szolgáltatások kiszolgálók és a erdő végrehajtja a felhőben, és erdők között megfelelő bizalmi kapcsolatok létesítése szükséges.

A dokumentum [létrehozása az Azure Active Directory címtár szolgáltatások (ÖSSZEADJA) erőforráserdők] [ adds-forest-in-azure] ezt a megközelítést megvalósításáról részletesen ismerteti.

## <a name="using-active-directory-federation-services-adfs-with-azure"></a>Használja az Active Directory összevonási szolgáltatások (ADFS) az Azure

ADFS futtatását is lehetővé teszi, hogy a helyszíni, de a hibrid helyzetben hol található az alkalmazások Azure-ban azt hatékonyabbá tehető Ez a funkció a felhőben végrehajtásához alább látható módon.

[! [3]][3]

Ez a felépítés különösen hasznos:

- Szövetséges engedélyezési kattintva jelenítse meg a partner szervezetek webalkalmazások kihasználó megoldásokat.

- A rendszer támogatja a hozzáférést a szervezeti tűzfal kívüli futó böngészők.

- Rendszerek, engedélyezése a felhasználók férhetnek hozzá a webalkalmazások hivatalos külső eszközökről, például a távoli számítógépek, a jegyzetfüzetek és a más mobileszközökre útján. 

ADFS használata Azure előnyei:

- Követelések alkalmazások is élvezheti.

- Az azt jelenti, hogy a külső partnerek hitelesítéshez megbízható biztosít.

- Nagy hitelesítési protokollok való kompatibilitás biztosít.

Azure ADFS használatával kapcsolatos szempontok

- Szükséges, hogy a saját ÖSSZEADJA ADFS és ADFS webes szolgáltatásalkalmazás-Proxy kiszolgálók végrehajtja a felhőben.

- Lehet, hogy a architektúra összetett konfigurálása.

Részletes információkért olvassa el a [Végrehajtási Active Directory összevonási szolgáltatások (ADFS) Azure-ban][adfs-in-azure].

## <a name="next-steps"></a>Következő lépések

Az alábbi forrásokban bemutatják, hogyan tudnak megvalósítani a jelen cikkben ismertetett architektúrákban.

- [Azure Active Directory végrehajtása][implementing-aad]
- [Azure Active Directory szolgáltatásaihoz (ÖSSZEADJA) kiterjesztés][extending-adds]
- [Azure Active Directory címtár szolgáltatások (ÖSSZEADJA) erőforráserdők létrehozása][adds-forest-in-azure]
- [Azure Active Directory összevonási szolgáltatások (ADFS) végrehajtása][adfs-in-azure]

<!-- Links -->
[0]: ./media/guidance-identity/figure1.png "Azure Active Directory használatával felhő identitás architektúra"
[1]: ./media/guidance-identity/figure2.png "Hibrid hálózat architektúrája, az Active Directoryval, biztonságos"
[2]: ./media/guidance-identity/figure3.png "Hibrid hálózat architektúrája külön Active Directory – tartományok és erdők biztonságos"
[3]: ./media/guidance-identity/figure4.png "Hibrid hálózat architektúrája az ADFS biztonságos"
[implementing-aad]: ./guidance-identity-aad.md
[extending-adds]: ./guidance-identity-adds-extend-domain.md
[adds-forest-in-azure]: ./guidance-identity-adds-resource-forest.md
[adfs-in-azure]: ./guidance-identity-adfs.md
<properties
   pageTitle="Gyakorlati tanácsok az adatok biztonsági és titkosítás |} Microsoft Azure"
   description="Ebben a cikkben egy sor olyan gyakorlati tanácsok az adatok biztonsági és Azure funkciók beépített titkosítást használ."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="yuridio"/>

#<a name="azure-data-security-and-encryption-best-practices"></a>Gyakorlati tanácsok Azure adatvédelem és titkosítás:

Adatvédelem a felhőben billentyűk közül a számlázást a lehetséges állapotok, amelyben az adatok akkor fordulhat elő, és milyen beállításokat, hogy az állapot érhetők el. Azure adatok céljából biztonság és titkosítás gyakorlati tanácsok a javaslatok lesz, az alábbi adatokat államok körül:

- A többi: Tárterület-objektumok, tárolók és létezik statikus fizikai adathordozón adattípusa mágneses vagy optikai lemez kell minden információt tartalmaz.

- A-hálózaton átvitt: Ha adatátvitel összetevők, a helyek vagy a programok között például a hálózaton keresztül (a felhőbe helyszíni és fordítva, ideértve például készült ExpressRoute hibrid kapcsolatok), a szolgáltatás bus vagy egy bemeneti és kimeneti folyamat során, akkor van gondolatot az, hogy a mozgásvonalak.

Ebben a cikkben ölelik fog Azure adatok biztonsági és titkosítási ajánlott eljárások gyűjteménye. Ezekkel az ajánlott eljárásokkal származik, a felület Azure adatvédelem és titkosítás és a változat, saját maga hasonló felhasználóktól.

Az egyes a legjobb megismerheti:

- Mi a legjobb módszer az
- Miért szeretné engedélyezni, hogy a legjobb
- Mi lehet az eredményt, ha nem sikerül ahhoz, hogy a legjobb módszer
- Célszerű lehet kiváltása
- Hogyan talál ahhoz, hogy a legjobb módszer

Azure adatvédelem és titkosítás: gyakorlati tanácsok a cikkben egy konszenzus véleményt és Azure platform funkciók és alapul szolgáltatás beállítása, ez a cikk készült időben léteznek. Vélemények és -technológiák időbeli változását, és ez a cikk a változások követése érdekében rendszeresen frissül.

Azure adatok biztonsági és titkosítási gyakorlati tanácsok a jelen cikkben tárgyalt a következők:

- Követelni többtényezős hitelesítést
- Használat szerepköralapú hozzáférés-szerepalapú
- Azure virtuális gépeken futó titkosítása
- Hardveres makróbiztonsági modelljei használata
- A biztonságos munkaállomások kezelése
- SQL-titkosítás engedélyezéséhez
- A hálózaton átvitt adatok védelme
- Fájl szintű adatok titkosítás kényszerítése


## <a name="enforce-multi-factor-authentication"></a>Követelni többtényezős hitelesítést

Az első lépés az adatokhoz való hozzáférés és a Microsoft Azure szabályozása a felhasználó hitelesítést végezni. [Azure többtényezős hitelesítés (MFA)](../multi-factor-authentication/multi-factor-authentication.md) a felhasználói azonosító ellenőrzése mint csak felhasználónévvel és jelszóval egy másik módszerrel módszer. A hitelesítési módot segít védelmét hozzáférés az adatokhoz és alkalmazások felhasználói igény szerint egy egyszerű bejelentkezési folyamat az értekezlet közben.

A felhasználók Azure MFA engedélyezésével hozzáadni biztonsági második réteg felhasználói bejelentkezések, és a tranzakciók. Ebben az esetben a tranzakciók lehetséges, hogy használja fájlkiszolgálóra vagy a SharePoint Online-ban található dokumentumok. Azure MFA szintén gondoskodhat informatikai csökkentheti a valószínűségét, hogy egy biztonságos hitelesítő van-e a szervezet adatokhoz való hozzáférés.

Példa: Ha a felhasználók számára a Azure MFA hivatkozási, és konfigurálja úgy, hogy használni egy telefonhívást vagy szöveges üzenet ellenőrzése, ha a felhasználó hitelesítő adatok sérül, a támadó nem minden erőforrás eléréséhez, mivel az általa hozzáférés a telefon felhasználói nem lesz. Azoknak a szervezeteknek, nem hoz létre a további rétegen identitás védelem további az adatok biztonságos vezethet hitelesítő adatok eltulajdonításának támadások.

[Azure többtényezős hitelesítést Server](../multi-factor-authentication/multi-factor-authentication-get-started-server.md), más néven helyszíni MFA egy alternatív azoknak a szervezeteknek szól, amelyek szeretné tartani a hitelesítési vezérlő a helyszíni környezetbe. Ez a módszer használatával alkalmazásait továbbra is képes a hivatkozási többtényezős hitelesítés, miközben a MFA kiszolgáló helyszíni.

Azure MFA kapcsolatos további tudnivalókért olvassa el a cikk [Ismerkedés az Azure többtényezős hitelesítés a felhőben](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="use-role-based-access-control-rbac"></a>Használat szerepköralapú hozzáférés-szerepalapú
Az access a [tudnivalók](https://en.wikipedia.org/wiki/Need_to_know) és a [legalacsonyabb jogosultság](https://en.wikipedia.org/wiki/Principle_of_least_privilege) biztonsági elvek alapján korlátozza. Ez a azoknak a szervezeteknek szól, amelyet szeretne biztonsági házirendek az adatokhoz való hozzáférés hivatkozási feltétlenül szükséges. Engedélyeket társíthat a felhasználók, csoportok és egy bizonyos hatókör alkalmazásokat Azure szerepköralapú hozzáférés vezérlő (RBAC) használható. Szerepkör-hozzárendelés hatókörének lehet előfizetés, erőforráscsoport vagy egy erőforrás.

[Beépített RBAC szerepkörök](../active-directory/role-based-access-built-in-roles.md) Azure jogosultságok hozzárendelése a felhasználókhoz is élvezheti. Fontolja meg inkább *Tárterület-fiók közös munka* a felhő operátorok, amely a tárterület-fiókok és *Klasszikus tárterület-fiók munkatársi* szerepkörök klasszikus tároló fiókok kezelése kezeléséhez. VMs és a tárterület-fiók kezelése kell, hogy a felhőben operátorok érdemes megfontolni *Virtuális gép munkatársi* szerepkörök fel azokat.

Előfordulhat, hogy szervezeteknek szól, amelyek nem a hivatkozási hozzáférés-vezérlés lehetőségek, például RBAC használata révén kell biztosítva mint szükséges azok a felhasználók számára a további jogosultságokkal. Ez úgy, hogy bizonyos adatok, amelyek az elsőként kerülni a Webhelyfiókok rendelkeznek hozzáféréssel rendelkező felhasználók adatok biztonságos vezethet.

Is tudjon meg többet az Azure RBAC a cikk [Azure Role-Based hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md)elolvasásával.

## <a name="encrypt-azure-virtual-machines"></a>Azure virtuális gépeken futó titkosítása
Sok szervezetek [a többi adatot titkosítási](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) adatok védelmét, megfelelőség és adatszuverenitás felé kötelező lépés. Azure lemez titkosítás lehetővé teszi, hogy a Windows-és a Linux IaaS virtuális gép (virtuális) titkosítása rendszergazdák. Azure lemez titkosítás a iparágban szabványos BitLocker funkció a Windows és a mennyiségi titkosítási az operációs rendszer és az adatok lemez megadására Linux a titkosítási DM szolgáltatását használja.

Azure lemez titkosítási védelme és a szervezeti szintű biztonsági és a megfelelőségi követelmények teljesítése érdekében az adatok védelme érdekében is élvezheti. Szervezetek is érdemes megfontolni titkosítással csökkentésében kockázatok jogosulatlan kapcsolódó adatokhoz való hozzáférés érdekében. Ajánlott is, hogy a bizalmas adatokat írandó előtt meghajtók titkosítása.

Ellenőrizze, hogy a virtuális adaton és indítási mennyiségi titkosítása a többi Azure tárterület-fiókját az adatok védelme érdekében. A titkosítási kulcs és titkos kulcsok védelme által feljebb helyezése [Azure kulcs tárolóból elemre](../key-vault/key-vault-whatis.md).

A helyszíni Windows-kiszolgálók vegye figyelembe a a következő titkosítási ajánlott eljárások:

- Adatok titkosítása [BitLocker](https://technet.microsoft.com/library/dn306081.aspx) használatával
- Active Directory tartományi szolgáltatásokban helyreállítási adatainak tárolása.
- Ha bármilyen problémát, hogy BitLocker kulcsok van kerülhetett, azt javasoljuk, formázhatja, vagy az összes előfordulását BitLocker metaadatok eltávolítása a meghajtó meghajtó vagy visszafejtése, és a újra a teljes meghajtó titkosítására.

Szervezeteknek szól, amelyek nem a hivatkozási adatok titkosítása nagyobb valószínűséggel adatintegritási problémák, például adatok ellopásának megakadályozására rosszindulatú vagy engedélyezetlen felhasználók kitenni, és ezzel az illetéktelen hozzáférjenek a formátum törlése adatok fiókok sérül. Amellett, hogy ezek a kockázatok kell az üzleti szabályoknak, cégek igazolnia kell, hogy azok szokott, használja a megfelelő biztonsági vezérlők adatok biztonság fokozása érdekében.

Akkor talál további információt Azure lemez titkosítás a cikk a [lemez titkosítás a Windows Azure és Linux IaaS VMs](azure-security-disk-encryption.md)olvasásával.

## <a name="use-hardware-security-modules"></a>Hardveres biztonsági modulokat használata

Üzleti titkosítási megoldások titkos kulcsok segítségével adatok titkosítása. Ezért fontos, hogy a billentyűk biztonságos tárolása. Kulcskezelő adatvédelem, szerves része lesz, mivel fog kell kapcsolatos károkozásra adatainak titkosítására használt titkos kulcsokat tárolásához.

Azure lemez titkosítás szabályozhatja, és kezelheti a lemez titkosítási kulcs és titkos kulcsok kulcs tárolóból elemre az előfizetésben, biztosítva, hogy a virtuális gép lemezen az összes adat titkosítva vannak a Azure-tárolóban lévő többi használja [Azure kulcs tárolóból elemre](https://azure.microsoft.com/services/key-vault/) . Azure kulcs tárolóra kulcsok és a házirend-használati naplózandó kell használni.

Vannak olyan sok kockázatokkal kapcsolódó nem rendelkezik a megfelelő biztonsági funkciók védelme a titkos kulcsokat adatainak titkosítására használt helyen. Ha a támadók rendelkezik hozzáféréssel a titkos kulcsok, tudja visszafejteni az adatokat, és esetleg bizalmas adatokhoz való hozzáférés lesznek.

A témakör elolvasásával talál további kapcsolatos általános ajánlások az Azure-ban tanúsítványkezelés [tanúsítványkezelés Azure-ban: tennivalók és intelmet](https://blogs.msdn.microsoft.com/azuresecurity/2015/07/13/certificate-management-in-azure-dos-and-donts/).

Azure kulcs tárolóra kapcsolatos további tudnivalókért olvassa el az [első lépések az Azure kulcs tárolóból elemre](../key-vault/key-vault-get-started.md).

## <a name="manage-with-secure-workstations"></a>A biztonságos munkaállomások kezelése

Mivel a támadások többsége a végfelhasználó cél, a végpont egyik támadások elsődleges pont lesz. Ha támadó gyengíti a végpontot, ő is kihasználhatja a felhasználó hitelesítő adatait, és a szervezet adatok eléréséhez szükséges. A legtöbb végpont támadások a tény, hogy a végfelhasználók a helyi munkaállomást rendszergazdái hasznát.

Biztonságos management munkaállomás használatával e kockázatok csökkentheti. Azt javasoljuk, hogy a [Yammerhez Access munkaállomások (PAW)](https://technet.microsoft.com/library/mt634654.aspx) használatával csökkenti a munkaállomások homonimaszerű webcímmel felület. A biztonságos kezelési munkaállomások segítséget nyújtanak az ezek közül néhány támadások biztosíthatja az adatok biztonságosabb. Ellenőrizze, hogy védekezés megerősítésére és munkaállomástól zárolásához PAW használatával. Ez a magas biztonsági biztosítékai bizalmas fiókok, feladatok és az adatvédelem megadására fontos lépést.

Az endpoint protection hiánya helyezze el az adatokat a kockázat, ügyeljen arra, hogy minden az összes eszközön, amellyel adatokat, függetlenül attól, az adatok helyet (a felhőben vagy a helyszíni) felhasználni a biztonsági házirendek hivatkozási.

További információ a jogosultsággal rendelkező elérheti munkaállomás, ha a [Yammerhez hozzáférés biztonságossá tétele](https://technet.microsoft.com/library/mt631194.aspx)cikk olvasási talál.

## <a name="enable-sql-data-encryption"></a>SQL-titkosítás engedélyezéséhez

[Azure SQL-adatbázis átlátszó adatok titkosítása](https://msdn.microsoft.com/library/dn948096.aspx) (TDE) segítségével rosszindulatú tevékenység veszélyével vírusvédelmet anélkül, hogy az alkalmazás módosításainak valós idejű titkosítás és az adatbázis, társított biztonsági mentés és a többi tranzakció naplófájlok visszafejtés végrehajtásával.  TDE titkosítja a teljes adatbázisra tárolására nevű adatbázist titkosítókulcs szimmetrikus kulcs használatával.

Akkor is, ha a teljes tárterület titkosítva van, nagyon fontos is titkosíthatja az adatbázis magát. Az adatok védelmére vonatkozó mélység megközelítésben a védelmet megvalósítása. Ha [Azure SQL-adatbázis](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) használ, és védelme a bizalmas adatokat, például hitelkártya vagy TAJ számokat szeretne, titkosíthatja FIPS 140-2 érvényesített 256 bites AES titkosítást, amely megfelel a sok iparági szabványokat (például HIPAA, PCI) az adatbázisokat.

Fontos, ha meg szeretné érteni, hogy [pufferelési készlet bővítmény](https://msdn.microsoft.com/library/dn133176.aspx) (BPE) kapcsolódó fájlok nem titkosított adatbázis titkosított TDE használatával. Szintű titkosítás Rendszereszközök fájl BitLocker hasonlóan kell használnia, vagy a [Fájlrendszer](https://technet.microsoft.com/library/cc700811.aspx) (titkosított fájlrendszer) BPE kapcsolódó fájlokat.

Egy hitelesített felhasználó óta például egy értékpapír vagy egy adatbázis rendszergazdája hozzáférhet az adatokat, még akkor is, ha az adatbázis titkosítása TDE, is kövesse az alábbi javaslatok:

- Az adatbázis szintjén SQL-hitelesítés
- Azure Active Directory authentication RBAC szerepkörök használata
- Felhasználók és az alkalmazások kell fiókokba külön hitelesítést végezni. Ezzel a módszerrel korlátozhatja a felhasználók és az alkalmazások engedélyeket és rosszindulatú tevékenység kockázatok csökkentése
- Megvalósítása adatbázis szintű biztonsági rögzített adatbázis szerepkörök (például db_datareader vagy db_datawriter), vagy használatával hozhat létre a kijelölt adatbázis-objektumok explicit engedélyeket szeretne adni az alkalmazás egyéni szerepkörök

Azoknak a szervezeteknek, nem használja az adatbázis titkosítás: Előfordulhat, hogy kell több tekinteni, az SQL-adatbázisait található adatok veszélyezteti támadások ellen.

Akkor talál további információt SQL TDE titkosítási a cikk [Az Azure SQL-adatbázis átlátszó adatok titkosítása](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx)elolvasásával.

## <a name="protect-data-in-transit"></a>A hálózaton átvitt adatok védelme

A hálózaton átvitt adatok védelme kell lennie a adatok védelmére vonatkozó stratégia lényeges részét. Mivel a másikba adatokat fog áthelyezése számos helyről, az általános ajánlást, mindig az SSL/TLS protokollok való használható különböző helyeken keresztül adatcsere. Bizonyos körülmények között, előfordulhat, hogy szeretné azonosítani a helyszíni és felhőbeli között a teljes kapcsolati csatorna infrastruktúra virtuális magánhálózaton (VPN) használatával.

Az adatok között a helyszíni infrastruktúra és Azure például a HTTPS- vagy VPN megfelelő biztosítékokat figyelembe.

Azoknak a szervezeteknek szól, biztonságos hozzáférés több munkaállomásokról kell található helyszíni Azure, hogy a [Azure - webhely VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md)használja.

Azoknak a szervezeteknek, biztonságos módon elérhetők az Azure, a helyszíni található egy munkaállomás kell használatával [pont-pont VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md).

Nagyobb készletek áthelyezhető fölé egy dedikált nagy sebességű WAN adatkapcsolat például [készült ExpressRoute](https://azure.microsoft.com/services/expressroute/). Ha úgy dönt, hogy készült ExpressRoute használja, titkosítható az alkalmazás szintű adatok mindegyikére [SSL/TLS](https://support.microsoft.com/kb/257591) vagy más protokollok használatával hozzáadott védelmét.

Közötti kommunikáció során az Azure-portálon keresztül Azure adathordozós, ha az összes tranzakció keresztül HTTPS fordul elő. [Tároló REST API -val](https://msdn.microsoft.com/library/azure/dd179355.aspx) HTTPS is használható [Azure-tárhely](https://azure.microsoft.com/services/storage/) és [Azure SQL-adatbázis](https://azure.microsoft.com/services/sql-database/)használata.

Azoknak a szervezeteknek, a hálózaton átvitt adatok védelme nem több [ember középső támadások](https://technet.microsoft.com/library/gg195821.aspx), [lehallgatás](https://technet.microsoft.com/library/gg195641.aspx) és munkamenet kihasználása. Ezek a támadások lehet az első lépés a bizalmas adatokhoz való hozzáférés.

Akkor talál további információt Azure virtuális Magánhálózati lehetőséget a cikk [Tervezés és a virtuális Magánhálózati átjáró tervezés](../vpn-gateway/vpn-gateway-plan-design.md)elolvasásával.

## <a name="enforce-file-level-data-encryption"></a>Fájl szintű adatok titkosítás kényszerítése

Egy másik réteg, amelyek növelhetik a adatok biztonsági szintjét védelemmel van titkosítja magát, a fájlt a fájl helyének függetlenül.

[Azure Tartalomvédelmi](https://technet.microsoft.com/library/jj585026.aspx) titkosítási identitás és engedélyezési házirendek biztonságossá a fájlok és e-mail használja. Azure Tartalomvédelmi keresztül különféle eszközökön működik – telefonon, táblagépen és PC-re, mind a szervezeten belül, és a szervezeten kívüli. Ez a képesség Azure Tartalomvédelmi hozzáadja az adatokat, még akkor is, ha a szervezet határai elhagyja a szintű oka lehetséges.

Azure Tartalomvédelmi használatával fájljai védelmét, ha a teljes körű támogatása [FIPS 140-2](http://csrc.nist.gov/groups/STM/cmvp/standards.html)szabványos titkosítási használja. Azure Tartalomvédelmi akkor kihasználhatja az adatok védelme, ha még akkor is, ha a tárhely, amely nem felügyelete alatt a program átmásolja van, amely a védelmet a fájlt, az maradjon a garancia IT, például egy felhőalapú tárhelyszolgáltatáshoz. Ugyanúgy történik az e-mailben, megosztott fájlok a fájlt mellékletként egy e-mailen utasításokat védett miként nyithatja meg a védett mellékletet.

Az Azure Rights Management szolgáltatást elfogadása tervezésekor javasoljuk az alábbiakat:

- Telepítse a a [Tartalomvédelmi alkalmazás megosztása](https://technet.microsoft.com/library/dn339006.aspx). Az alkalmazás együttműködik az Office telepítése az Office-alkalmazások beépülő modul, hogy a felhasználók is egyszerűen fájlok védelmére közvetlenül.
- Állítsa be az Azure Rights Management szolgáltatást támogató alkalmazások és szolgáltatások
- Hozzon létre [egyéni sablonokat](https://technet.microsoft.com/library/dn642472.aspx) , amely az üzleti igényeknek megfelelően. Példa: egy sablont, amely az összes legfelső titkos vonatkoznak felső titkos adatok kapcsolódó e-maileket.

Lehet, hogy több adat szivárgás téve a szervezeteknek, amelyek az [adatok osztályozás](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) és fájl védelme a gyenge. Anélkül, hogy megfelelő fájlvédelmi szervezetek nem szerezze be a vállalati hírcsatornájában, abuse figyelje és rosszindulatú hozzáférés elkerülése érdekében a fájlok.

További tudnivalók az Azure Rights Management szolgáltatást talál az [Első lépések az Azure Rights Management](https://technet.microsoft.com/library/jj585016.aspx)témakör elolvasásával.

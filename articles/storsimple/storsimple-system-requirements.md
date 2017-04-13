<properties
   pageTitle="Rendszerkövetelmények StorSimple |} Microsoft Azure"
   description="Szoftver, a hálózatot, és a magas elérhetősége követelmények és a gyakorlati tanácsok a Microsoft Azure StorSimple megoldást ismerteti."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/31/2016"
   ms.author="alkohli"/>

# <a name="storsimple-software-high-availability-and-networking-requirements"></a>StorSimple szoftver, a magas rendelkezésre állásának és a hálózati követelmények

## <a name="overview"></a>– Áttekintés

Üdvözli a Microsoft Azure StorSimple. Ez a cikk ismerteti a fontos Rendszerkövetelmények és a gyakorlati tanácsok az StorSimple eszköz és a tárhely ügyfelei az eszközön. Azt javasoljuk, hogy tekintse át az információkat gondosan előtt a StorSimple rendszer üzembe helyezése, és kattintson a hivatkozott vissza, ha az szükséges telepítési és a későbbi művelet során.

A rendszerkövetelmények a következők:

- **Tárterület-ügyfelek szoftverkövetelményei** – a támogatott operációs rendszerek és bármilyen további követelményeknek e operációs rendszerek ismerteti.
- Kell lennie, nyissa meg a tűzfalat, hogy engedélyezze az iSCSI, felhőalapú vagy kezelési forgalmat a portokat információt a **hálózat StorSimple eszköz követelményei** - tartalmaz.
- **Magas elérhetősége követelményei StorSimple** – magas elérhetősége követelmények és a gyakorlati tanácsok a StorSimple eszköz és host számítógép ismerteti. 


## <a name="software-requirements-for-storage-clients"></a>Tárterület-ügyfelek szoftverkövetelményei

A következő szoftverkövetelményekkel vannak, StorSimple eszköze hozzáféréssel tároló ügyfelei számára.

| Támogatott operációs rendszerek | Szükséges verzió | További követelmények/megjegyzések |
| --------------------------- | ---------------- | ------------- |
| A Windows Server              | 2008R2 SP1 2012, 2012R2 |StorSimple iSCSI kötet csak a következő Windows lemez típusú használata támogatott:<ul><li>Egyszerű kötet egyszerű lemezen</li><li>Egyszerű és tükrözött mennyiségi dinamikus lemezen</li></ul>Windows Server 2012 vékony kiépítési és ODX szolgáltatások StorSimple iSCSI kötet használata támogatott.<br><br>StorSimple hozhat létre, vékonyan kiépített és teljesen kiépített kötet. Részben kiépített kötet nem hozható létre.<br><br>Formázott vékonyan kiépített kötet hosszú időt vehet igénybe. Azt javasoljuk, törléséről a hangerőt, és majd létrehoz egy új formázott helyett. Ha még mindig szívesebben kötet újraformázni:<ul><li>Futtassa a következő parancsot a formázhatja előtt elejét veheti a hely visszanyerése: <br>`fsutil behavior set disabledeletenotify 1`</br></li><li>A formázás befejeződése után a következő parancsot használja a hely visszanyerése ismételt engedélyezése:<br>`fsutil behavior set disabledeletenotify 0`</br></li><li>A Windows Server 2012 javítás alkalmazni, a Windows Server számítógépen [KB 2878635](https://support.microsoft.com/kb/2870270) leírt módon.</li></ul></li></ul></ul> Ha állítja be StorSimple pillanatkép Manager vagy StorSimple kártya SharePoint, nyissa meg [a választható összetevők szoftverkövetelményei](#software-requirements-for-optional-components).|
| VMWare ESX | 5.5 és 6.0 alkalmazásban | Támogatott VMWare vSphere iSCSI ügyfél. Támogatott VAAI blokk szolgáltatás VMware vSphere StorSimple eszközökön.
| Linux RHEL/CentOS | 5, 6 és 7 | Linux iSCSI ügyfelek Megnyitás-iSCSI kezdeményező verzióival 5, 6 és 7 támogatása. |
| Linux | SUSE Linux 11 | |
 > [AZURE.NOTE] IBM AIX jelenleg nem támogatja a StorSimple.

## <a name="software-requirements-for-optional-components"></a>Választható összetevők szoftverkövetelményei

A következő szoftverkövetelményekkel valók a választható StorSimple összetevők (StorSimple pillanatkép Manager és a SharePoint-StorSimple kártya).

| Összetevő | A Host platform | További követelmények/megjegyzések |
| --------------------------- | ---------------- | ------------- |
| StorSimple pillanatkép Manager | A Windows Server 2008R2 SP1 2012, 2012R2 | Használja a StorSimple pillanatkép Manager a Windows Server szükség, biztonsági mentési és visszaállítási tükrözött dinamikus lemezre és az összes alkalmazás egységes biztonsági másolatot.<br> Csak a Windows Server 2008 R2 SP1 (64 bites), a Windows 2012 R2 és a Windows Server 2012-ben támogatott StorSimple pillanatkép Manager.<ul><li>Ablak Server 2012 használja, ha telepítenie kell .NET 3.5-ös – 4.5 StorSimple pillanatkép Manager telepítése előtt.</li><li>A Windows Server 2008 R2 SP1 használja, ha telepítenie kell a Windows Management Framework 3.0 StorSimple pillanatkép Manager telepítése előtt.</li></ul> |
| SharePoint-StorSimple kártya | A Windows Server 2008R2 SP1 2012, 2012R2 |<ul><li>SharePoint-StorSimple kártya csak a SharePoint 2010-ben és a SharePoint 2013-ban támogatott.</li><li>RBS SQL Server Enterprise Edition, 2008 R2 vagy 2012 verziója szükséges.</li></ul>|

## <a name="networking-requirements-for-your-storsimple-device"></a>Hálózati StorSimple eszközére vonatkozó követelmények

A StorSimple zárolt eszközről. Portok azonban kell nyitható meg a tűzfalat, hogy engedélyezze az iSCSI, felhőalapú és kezelési forgalmat. Az alábbi táblázatban meg kell nyitni a tűzfal portokat sorolja fel. Az alábbi táblázatban *, vagy *a bejövő* * utal, amely ügyfelek bejövő kéréseit hozzáférni az eszköz irányának. *Ki* vagy a *kimenő* hivatkozik, amelyben az StorSimple eszköz külső felekkel, adatokat küld a telepítést túl irányának: például kimenő az internethez.

| Port száma<sup>1,2</sup> | Vagy kicsinyítés | Port hatóköre | Szükséges | Jegyzetek |
|------------------------|-----------|------------|----------|-------|
|TCP 80-as (HTTP)<sup>3</sup>|  Ki |  WAN | nem |<ul><li>Kimenő port az Internet-hozzáférés frissítések beolvasásához használt.</li><li>A kimenő webes proxy konfigurálható felhasználói.</li><li>Ahhoz, hogy a rendszer frissítéseket, a port is kell nyitni az IP-címei rögzített vezérlőhöz.</li></ul> |
|TCP 443-as (HTTPS)<sup>3</sup>| Ki | WAN | igen |<ul><li>Kimenő portot használja a felhőben eléréséhez.</li><li>A kimenő webes proxy konfigurálható felhasználói.</li><li>Ahhoz, hogy a rendszer frissítéseket, a port is kell nyitni az IP-címei rögzített vezérlőhöz.</li><li>A port is használják a mind a vezérlők a szemétgyűjtő.</li></ul>|
|UDP 53 (DNS) | Ki | WAN | Bizonyos esetekben; jegyzetek témakörben talál. |A port csak egy internetes DNS-kiszolgáló esetén szükség. |
| UDP 123 (NTP) | Ki | WAN | Bizonyos esetekben; jegyzetek témakörben talál. |A port csak egy internetes NTP kiszolgálóval szükség. |
| TCP 9354 | Ki | WAN | igen |A kimenő portot használja a StorSimple eszköz használatával kommunikál a StorSimple kezelő szolgáltatás. |
| 3260 (iSCSI) | A | LAN | nem | A port az adatokhoz való hozzáféréshez használt iSCSI fölé.|
| 5985 | A | LAN | nem | Bejövő port kommunikálni az StorSimple eszközzel StorSimple pillanatkép kezelő által szolgál.<br>A port használatos távolról való csatlakozáskor a Windows PowerShell-StorSimple HTTP is. |
| 5986 | A | LAN | nem | A port nem távolról csatlakozni a Windows PowerShell-StorSimple HTTPS használatos. |

<sup>1</sup> nem bejövő portokat kell lehet megnyitni a nyilvános internetkapcsolat.

<sup>2</sup> több portok végrehajtása egy átjáró konfigurációt, ha a kimenő adatforgalomra sorrendben határozzák meg a port útválasztási sorrendben, a [Port útválasztás](#routing-metric), az alábbiakban leírtak alapján.

A vezérlő IP-címei rögzített StorSimple eszközén <sup>3</sup> kell továbbíthatók és csatlakozhat az internethez. A rögzített IP-címek karbantartási a frissítéseket, hogy az eszköz segítségével. Az eszköz vezérlők az interneten keresztül a rögzített IP-címei nem tud csatlakozni, ha nem tudja frissíteni az StorSimple eszközt.

> [AZURE.IMPORTANT] Győződjön meg arról, hogy a tűzfalon keresztül nem módosíthatók, visszafejtése bármely SSL-forgalmat a StorSimple eszköz és Azure között.

### <a name="url-patterns-for-firewall-rules"></a>Tűzfalszabályokat az URL-cím mintázatok

Hálózati rendszergazda gyakran adhatja meg a speciális tűzfalszabályokat alapján a URL-CÍMÉT szeretné szűrni a bejövő és kimenő forgalmat. A StorSimple és a StorSimple kezelő szolgáltatás más Microsoft-alkalmazások, például Azure Service Bus, Azure Active Directory hozzáférés-vezérlés, tároló fiókok és a Microsoft Update kiszolgálók függ. Az URL-cím mintázatok ezeket az alkalmazásokat társított tűzfalszabályokat konfigurálásához használható. Fontos, ha meg szeretné érteni, hogy az URL-cím mintázatok ezeket az alkalmazásokat társított módosíthatja. Az ehhez a hálózati rendszergazda figyelésére és a StorSimple, és szükség esetén tűzfalszabályokat frissítése.

Azt javasoljuk, hogy a tűzfalszabályokat kimenő forgalmához, IP-címek liberally rögzíteni a legtöbb esetben StorSimple alapján beállíthatja. Az alábbi információk segítségével azonban biztonságos környezetek kialakításához szükséges speciális tűzfalszabályokat beállítása.

> [AZURE.NOTE] Az eszköz (forrás) IP-címei az engedélyezett hálózati adaptereken mindig legyen. A cél IP-címei [Azure adatközpont IP-tartományokat](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653)kell állítani.

#### <a name="url-patterns-for-azure-portal"></a>Azure portál URL-cím mintázatok
| Minta URL-címe                                                      | Összetevő/funkciók                                           | Eszköz IP-címei                           |
|------------------------------------------------------------------|---------------------------------------------------------------|-----------------------------------------|
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`   | StorSimple kezelő szolgáltatás<br>Access-vezérlő szolgáltatást<br>Azure Service Bus| Felhő engedélyezni hálózati kapcsolatok        |
|`https://*.backup.windowsazure.com`|Regisztráció eszköz| Csak az adatok 0|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Tanúsítvány-visszavonási |Felhő engedélyezni hálózati kapcsolatok |
| `https://*.core.windows.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Azure tároló fiókok és figyelése | Felhő engedélyezni hálózati kapcsolatok        |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| A Microsoft Update kiszolgálók<br>                             | A vezérlő csak rögzített IP-címei               |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN |A vezérlő csak rögzített IP-címei   |
| `https://*.partners.extranet.microsoft.com/*`                    | Támogatás csomag                                                  | Felhő engedélyezni hálózati kapcsolatok        |

#### <a name="url-patterns-for-azure-government-portal"></a>Azure kormányzati portál URL-cím mintázatok
| Minta URL-címe                                                      | Összetevő/funkciók                                           | Eszköz IP-címei                           |
|------------------------------------------------------------------|---------------------------------------------------------------|-----------------------------------------|
| `https://*.storsimple.windowsazure.us/*`<br>`https://*.accesscontrol.usgovcloudapi.net/*`<br>`https://*.servicebus.usgovcloudapi.net/*`   | StorSimple kezelő szolgáltatás<br>Access-vezérlő szolgáltatást<br>Azure Service Bus| Felhő engedélyezni hálózati kapcsolatok        |
|`https://*.backup.windowsazure.us`|Regisztráció eszköz| Csak az adatok 0|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Tanúsítvány-visszavonási |Felhő engedélyezni hálózati kapcsolatok |
| `https://*.core.usgovcloudapi.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Azure tároló fiókok és figyelése | Felhő engedélyezni hálózati kapcsolatok        |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| A Microsoft Update kiszolgálók<br>                             | A vezérlő csak rögzített IP-címei               |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN |A vezérlő csak rögzített IP-címei   |
| `https://*.partners.extranet.microsoft.com/*`                    | Támogatás csomag                                                  | Felhő engedélyezni hálózati kapcsolatok        |

### <a name="routing-metric"></a>Útválasztási metrikus

Egy útválasztási mérőszám társítva a kapcsolatok és az átjáró, amely az adatokat a megadott hálózatok irányítja. Útválasztási metrikus használják az útválasztási protokoll számítja ki a legjobb elérési útját a célként megadott, ha folyamatosan tanul több út létezik a azonos célhelyre. Az alsó a útválasztási mérőszám, annál nagyobb a beállítás.

Az StorSimple környezetben Ha több hálózati kapcsolatok és átjárók vannak beállítva, hogy a forgalmat a csatornát, a útválasztási mérési módja miatt érkezzenek be a lejátszás gombra a relatív sorrend, amelyben a felületek használt első. A útválasztási mérési módja miatt nem lehet módosítani a felhasználó által. Azonban használhat a `Get-HcsRoutingTable` parancsmag kinyomtatásához StorSimple eszközén útválasztási táblával (és mértékek). További információ a Get-HcsRoutingTable parancsmag [Hibaelhárítási StorSimple](storsimple-troubleshoot-deployment.md)környezetben.

A útválasztási metrikus algoritmusok eltérő attól függően, hogy a szoftver verziószáma StorSimple eszközén futó.

**1 a frissítés előtt verziókban**

Ide tartoznak a szoftver verziók előtt Georgia, 0,1 értéket, például az Update 1 0,2 vagy 0,3 megjelenési. A sorrend útválasztási mértékek alapján van az alábbi képlettel történik:

   *Az utolsó konfigurált 10 GbE hálózati kapcsolat > egyéb 10 GbE hálózati kapcsolat > 1 GbE hálózati kapcsolat utolsó konfigurált > egyéb 1 GbE hálózati kapcsolat*


**Frissítés 1 és 2 frissítés előtti kezdési verziókban**

Ide tartoznak a szoftver verziói, például az 1, 1.1-es vagy 1.2-es. A sorrend útválasztási mértékek alapján az határozza meg az alábbi képlettel történik:

   *ADATOK 0 > 10 GbE hálózati kapcsolat utolsó konfigurált > egyéb 10 GbE hálózati kapcsolat > utolsó konfigurált 1 GbE hálózati kapcsolat > egyéb 1 GbE hálózati kapcsolat*

   A frissítés 1, a 0 adatok útválasztási mérőszám végeznek a legalacsonyabb; Ennélfogva minden a felhőben-forgalmat adatok 0 továbbít. Jegyezze fel az adott Ha egynél több felhő engedélyezni hálózati kapcsolaton StorSimple eszközén.


**Frissítés 2 kezdve verziókban**

2 frissítés még több hálózat kapcsolatos fejlesztések, és a útválasztási mérési módja miatt megváltozott. A vezérlő viselkedése a következőképpen magyarázható.

- Előre megadott értékek hálózati kapcsolatok van rendelve.   

- Fontolja meg egy példa egy táblázatra, ha cloud-engedélyezve van a különböző hálózati kapcsolatok rendelt értékek vagy a felhőben – letiltott, de konfigurált az átjárók alább látható módon. Megjegyzés: a hozzárendelt értékei példa csak az értékek.


  	| Hálózati kapcsolat | Felhőalapú engedélyezve | Felhőalapú-letiltva az átjáró |
  	|-----|---------------|---------------------------|
  	| Adatok 0  | 1            | -                        |
  	| Adatok 1  | 2            | 20                       |
  	| Adatok 2  | 3            | 30                       |
  	| Adatok 3  | 4            | 40                       |
  	| Adatok 4  | 5            | 50                       |
  	| Adatok 5  | 6            | 60                       |


- A sorrendben, amelyben a felhőben forgalom lesz irányítva, a hálózati felületeken keresztül van:

    *Adatok 0 > adatai 1 > dátum 2 > 3 adatok > adatok 4 > 5 adatok*

    Ez a következő példa magyarázható.

    Fontolja meg egy két felhő engedélyezni hálózati kapcsolatok, a 0 adatok és az adatok 5 StorSimple eszközt. Adatok 1 – 4 adatok vannak felhő letiltva, de van konfigurálva az átjárók. A sorrendben, amelyben forgalom továbbítja az eszköz a következő lesz:

    *Adatok 0 (1) > 5 (6) adatok > adatok 1 (20) > adatok 2 (30) > adatok 3 (40) > adatok 4 (50)*

    *adott számok zárójelben jelennek meg a adja meg a megfelelő útválasztási mérési módja miatt.*

    Ha nem sikerül adatok 0, a rendszer-adatok 5 a felhőben forgalom vonalra. Tekintve, hogy az átjáró más hálózaton van beállítva, mintha adatok 0 és a adatok 5 meghiúsító, a program elküldi a felhőben forgalom adatai 1 keresztül.


- Ha nem sikerül egy felhőalapú engedélyező hálózati kapcsolat, akkor csatlakozhat a felület 30 második késleltetést 3 ismétlések. Ha nem sikerül az összes újrapróbálkozások, a forgalom továbbítás a következő elérhető felhő engedélyezni felületére az útválasztási táblázatban meghatározott. Ha a felhő engedélyezni hálózat fail felületek, majd az eszköz sikertelen lesz fölé a többi vezérlő (ebben az esetben nem újraindítás).

- Virtuális hiba egy iSCSI engedélyező hálózati kapcsolat esetén, 3 2 másodpercig késleltetést ismétlések lesz. Ez a jelenség tartózkodott azonos a korábbi verziókban. Ha nem sikerül a iSCSI hálózati adaptereken, a vezérlő feladatátvétel (a számítógép újraindítása által kísért) történik.


- Értesítés is következik StorSimple eszközén be egy virtuális hiba esetén. További információért lépjen [riasztási – rövid összefoglaló](storsimple-manage-alerts.md).

- Újrapróbálkozások, értelmez iSCSI elsőbbséget élveznek felhő.

    Vegye figyelembe az alábbi példa: A StorSimple eszközzel két hálózati kapcsolatok engedélyezve van, az adatok 0 és a adatok 1. 0 adata felhő engedélyező mivel adatai 1 mindkét felhő és iSCSI engedélyezve van. Az eszköz nincs más hálózati kapcsolatok engedélyezett a felhőben vagy iSCSI.

    Ha az adatok 1 sikertelen, mivel ez a legutóbbi iSCSI hálózati kapcsolaton, ekkor egy vezérlő című témakörében adatok 1 a többi vezérlő.


### <a name="networking-best-practices"></a>Ajánlott eljárások a hálózat

A fenti hálózati követelmények, az az optimális teljesítmény eléréséhez a StorSimple megoldás kívül kérjük igazodjon az alábbi gyakorlati tanácsokat:

- Gondoskodjon arról, hogy az StorSimple eszköz a kijelölt 40 MB sávszélesség (vagy több) elérhető állandóan látszódjon. A sávszélesség nem lesznek megosztva (vagy terhelés garantálni kell való QoS házirendek) más alkalmazásokkal.

- Gondoskodjon arról, hogy mindig az internethez a hálózati kapcsolat áll rendelkezésre. Az eszközről, többek között a nem, nem internetkapcsolat kapcsolata időről-időre vagy mellékletei nem megbízható internetes kapcsolatok eredményezi beállításai nem támogatottak.

- A iSCSI és a felhő forgalom elkülönítése által dedikált hálózati kapcsolatok iSCSI és a felhő elérése az eszközön. További tudnivalókért lásd: a [hálózati kapcsolatok módosítása](storsimple-modify-device-config.md#modify-network-interfaces) StorSimple eszközén.

- Ne használja a hivatkozás összesítési Control Protocol (LACP) konfiguráció a hálózati kapcsolat esetén. Ez a beállítás egy nem támogatott.


## <a name="high-availability-requirements-for-storsimple"></a>StorSimple magas elérhetősége követelményei

A hardver platform, amely szerepel a StorSimple megoldás az elérhetőség és megbízhatóságára szolgáltatások, amelyek a alap biztosítanak egy erősen érhető el, hiba tervezett tároló infrastruktúra a adatközpontban tartalmaz. Vannak azonban olyan követelmények és meg kell felelnie a StorSimple megoldás elérhetősége érdekében gyakorlati tanácsokat. Mielőtt beállítaná StorSimple, gondosan olvassa el az alábbi követelményeket és a gyakorlati tanácsok az StorSimple eszköz és a csatlakoztatott állomások.

További információt a figyelés és fenntartása StorSimple eszköze hardver összetevői megnyitásához [használja a StorSimple kezelő szolgáltatás figyelheti a hardver-összetevő és az állapot](storsimple-monitor-hardware-status.md) és [StorSimple hardver összetevő feltölteni](storsimple-hardware-component-replacement.md).

### <a name="high-availability-requirements-and-procedures-for-your-storsimple-device"></a>Magas elérhetősége követelmények és eljárások StorSimple eszköze

Tekintse át a következő információkat gondosan rendelkezésre állásának a magas StorSimple eszközére.

#### <a name="pcms"></a>PCMs

StorSimple eszközök felesleges, melegvíz-cserélhető power és hűtési modulok (PCMs). Minden egyes PCM elegendő kapacitása ahhoz, hogy adja meg a teljes váz szolgáltatást tartalmaz. Magas rendelkezésre állás érdekében mindkét PCMs kell telepíteni.

- Csatlakozás a PCMs elérhetősége megadására, ha nem sikerül egy power forrás különböző power forrásokból.
- Ha nem sikerül egy PCM, azonnal kérése egy másikat.
- Csak akkor, ha a csere, és készen áll a telepítéshez, távolítsa el a sikertelen PCM.
- Ne távolítsa el egyidejűleg két PCMs. A PCM modul a biztonsági mentés elem modul tartalmazza. Mind a PCMs eltávolítása egy leállítás elem védelem nélkül eredményez, és nem lesznek mentve a eszköz állapotát. Az elem olvashat nyissa meg [a biztonsági mentés elem modul kezelése](storsimple-battery-replacement.md#maintain-the-backup-battery-module).

#### <a name="controller-modules"></a>Vezérlő modulok

StorSimple eszközök felesleges, melegvíz-cserélhető vezérlő modulokat. A vezérlő modulok aktív/passzív módon működnek. Az adott időpontban egy vezérlő modul aktív, és a szolgáltatást, míg a többi vezérlő modul passzív biztosít. A vezérlő passzív modul van kapcsolva, és műveleti válik, ha az aktív vezérlő modul nem sikerül vagy eltávolítja. Minden vezérlő modulban elegendő kapacitása ahhoz, hogy adja meg a teljes váz szolgáltatást tartalmaz. A két vezérlő modulok telepítenie kell a magas állásának.

- Győződjön meg arról, hogy mindkét vezérlő modulok mindig telepítve vannak.

- Ha nem sikerül egy vezérlő modulra, azonnal kérése egy másikat.

- Csak akkor, ha a csere, és készen áll a telepítéshez, távolítsa el egy hibás vezérlő modulra. A bővített időszakok hatással van a légmozgásnak eltávolítása a modul és így a rendszer hűtési.

- Győződjön meg arról, hogy a hálózati kapcsolat mindkét vezérlő modulok azonos, és a csatlakoztatott hálózati kapcsolatok van egy azonos hálózati konfigurálásról.

- Ha nem sikerül egy vezérlő modulra, vagy van szüksége a csere, ellenőrizze, hogy a vezérlő modul aktív állapotú cseréje a hibás vezérlő modul előtt. Győződjön meg róla, hogy egy vezérlő aktív, keresse fel [az aktív vezérlő az eszközön azonosítása](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).

- Ne távolítsa el egyszerre mindkét vezérlő modulokat. A vezérlő feladatátvétel folyamatban van, ha nem a készenléti vezérlő modul leállítása vagy eltávolítása abból a váz.

- Egy vezérlő feladatátvétel után várjon legalább öt perc vagy vezérlő modul eltávolítása előtt.

#### <a name="network-interfaces"></a>Hálózati kapcsolatok

StorSimple eszköz vezérlő modulok minden van négy 1 Gigabit és két 10 Gigabit Ethernet hálózati kapcsolatok.

- Győződjön meg arról, hogy a hálózati kapcsolat mindkét vezérlő modulok megegyeznek, és a hálózat felületek, hogy a vezérlő modul felületek csatlakozik egy azonos hálózati konfigurációja.

- Amikor csak lehetséges, telepítse a hálózati kapcsolatok szolgáltatás rendelkezésre állásának abban az esetben egy hálózati eszköz különböző kapcsolókat keresztül.

- Húzza ki az egyetlen, vagy az utolsó hátralévő iSCSI engedélyező felület (ha az IP-címei rendelt), amikor először letiltásához, és ezután húzza ki a kábelek. Ha a kapcsolat először nem csatlakozik, majd okoz a passzív vezérlő átadni az aktív vezérlő. A passzív vezérlő is rendelkezik a megfelelő felületek leválasztani, ha majd mindkét vezérlők újraindul többször rendezése egyetlen vezérlőn előtt.

- Legalább két adatok felületek minden vezérlő modulból csatlakozzon a hálózathoz.

- Ha engedélyezte a két 10 GbE felületek, üzembe alkalmazáson keresztül különböző kapcsolókat.

- Ha lehetséges, használatával MPIO kiszolgálókon győződjön meg arról, hogy a kiszolgálókat elviseli egy hivatkozást, hálózati vagy felület hiba.

További információt a hálózat az eszközt a magas rendelkezésre állásának és a teljesítmény, nyissa meg azt [a StorSimple 8100 eszköz telepítése](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) vagy [a StorSimple 8600 eszköz telepítése](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

#### <a name="ssds-and-hdds"></a>SSD és HDDs

StorSimple eszközök egyszínű állam lemez (SSD), és a merevlemez-meghajtók (HDDs) segítségével védett tükrözött szóközöket. Tükrözött szóközök biztosítja, hogy az eszköz képes elviselni SSD vagy HDDs a hibát.

- Győződjön meg arról, hogy telepítve vannak-e az összes SSD és a merevlemez-modulok.

- Ha nem sikerül egy olyan SSD vagy a Merevlemezen, kérése azonnal egy másikat.

- Ha egy SSD merevlemez nem sikerül vagy helyettesítő igényel, ellenőrizze, csak a SSD vagy helyettesítő igénylő merevlemez eltávolítása.

- Ne távolítsa el egynél több SSD vagy merevlemez a rendszer bármely pontján időben.
A hibát, hogy bizonyos típusú (merevlemez SSD) 2 vagy több lemezt vagy egymást követő hiba egy rövid időkereten belüli vonhat rendszer meghibásodása, és a swayből adatvesztéssel. Ha ez akkor fordulhat elő, segítségért [forduljon a Microsoft terméktámogatási](storsimple-contact-microsoft-support.md) .

- Során feltölteni figyelheti a **Karbantartás** lapot a meghajtók SSD és HDDs a **Hardver állapota** . Zöld színű pipa jel állapot azt jelzi, hogy a lemez nem megfelelő, vagy OK, mivel a piros felkiáltójel azt jelzi, egy hibás SSD vagy merevlemez.

- Azt javasoljuk, hogy úgy beállítani, hogy az összes kötet, amelyeket a rendszer hiba esetén védelme kell felhő pillanatképek.

#### <a name="ebod-enclosure"></a>EBOD ház

StorSimple eszköz modell 8600 mellett az elsődleges ház bővített Kitöltenem a lemez (EBOD) tokba tartalmazza. Egy EBOD tartalmaz EBOD vezérlők, és a merevlemez-meghajtók (HDDs) segítségével védett tükrözött szóközöket. Tükrözött szóközök biztosítja, hogy az eszköz képes egy vagy több HDDs meghibásodása elviselni. A EBOD ház az elsődleges ház felesleges Társítások kábelek keresztül csatlakozik.

- Győződjön meg arról, hogy mindkét EBOD ház vezérlő modulok Társítások kábelek, mind a merevlemez-meghajtók mindig telepített összes.

- Ha nem sikerül egy EBOD ház vezérlő modul, azonnal kérése egy másikat.

- Ha nem sikerül egy EBOD ház vezérlő modul, ellenőrizze, hogy a vezérlő modul aktív-e előtt lecseréli a sikertelen modul. Győződjön meg róla, hogy egy vezérlő aktív, keresse fel [az aktív vezérlő az eszközön azonosítása](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).

- Egy EBOD vezérlő modul helyettesítő során folyamatosan figyelheti a összetevő a StorSimple kezelő szolgáltatás állapotát **karbantartási**elérésével > **hardveres állapotát**.

- Egy Társítások kábel nem sikerült, vagy helyettesítő (Microsoft Support kell tennie, hogy ilyen meghatározás) igényel, győződjön meg arról, hogy távolítsa el a csak a csere igényel Társítások kábel.

- Ne egyidejűleg távolítsa el a két Társítások kábelek bármely pontján a rendszer időben.

### <a name="high-availability-recommendations-for-your-host-computers"></a>A host számítógépek magas elérhetősége javaslatok

Gondosan tekintse át a hosts StorSimple eszközére csatlakoztatott magas állásának alábbi gyakorlati tanácsokat.

- [Két csomópont - fájl kiszolgáló fürt konfigurációk]StorSimple konfigurálása[1]. A hiba, és a redundancia host oldalán fejlesztésére egyetlen pontok eltávolításával erősen elérhetővé válik a teljes megoldás.

- A tároló vezérlők feladatátvételkor használata folyamatosan érhető el (CA) megosztja érhető el az elérhetőség magas, a Windows Server 2012-ben (a kis-és Középvállalatok 3.0). További információt a fájl kiszolgálói fürt és folyamatosan elérhető megosztások beállítása a Windows Server 2012 olvassa el a [bemutató videó](http://channel9.msdn.com/Events/IT-Camps/IT-Camps-On-Demand-Windows-Server-2012/DEMO-Continuously-Available-File-Shares).

## <a name="next-steps"></a>Következő lépések

- [Megtudhatja, hogy StorSimple rendszer korlátai](storsimple-limits.md).
- [Megtudhatja, hogy miként a StorSimple megoldást üzembe helyez](storsimple-deployment-walkthrough-u2.md).

<!--Reference links-->
[1]: https://technet.microsoft.com/library/cc731844(v=WS.10).aspx

<properties
   pageTitle="Virtuális tömb StorSimple rendszerkövetelményei"
   description="További tudnivalók a szoftverek és a StorSimple virtuális tömb hálózati követelményei:"
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
   ms.workload="NA"
   ms.date="10/17/2016"
   ms.author="alkohli"/>

# <a name="storsimple-virtual-array-system-requirements"></a>Virtuális tömb StorSimple rendszerkövetelmények

## <a name="overview"></a>– Áttekintés

Ez a cikk a Microsoft Azure StorSimple virtuális tömbképletek (más néven StorSimple helyszíni virtuális vagy StorSimple virtuális eszköz) és a tömb tároló ügyfelei fontos rendszerkövetelményei ismerteti. Azt javasoljuk, hogy tekintse át az információkat gondosan előtt a StorSimple rendszer üzembe helyezése, és kattintson a hivatkozott vissza, ha az szükséges telepítési és a későbbi művelet során.

A rendszerkövetelmények a következők:

-   **Tárterület ügyfelek szoftverkövetelményei** – a támogatott virtualization platform, böngészők, iSCSI kezdeményezők, kis-és Középvállalatok ügyfelek, virtuális eszközön minimális követelmények és bármilyen további követelményeknek e operációs rendszerek ismerteti.

-   Kell lennie, nyissa meg a tűzfalat, hogy engedélyezze az iSCSI, felhőalapú vagy kezelési forgalmat a portokat információt a **hálózat StorSimple eszköz követelményei** - tartalmaz.

Ebben a cikkben közzétett StorSimple-követelmények Rendszerinformáció csak StorSimple virtuális tömbök vonatkozik.

- 8000 sorozat eszközök esetén lépjen [a StorSimple 8000 sorozat eszköz rendszerkövetelményei](storsimple-system-requirements.md).
 
- 7000 sorozat eszközök esetén lépjen [a StorSimple 5000-7000 sorozat eszköz rendszerkövetelményei](http://onlinehelp.storsimple.com/1_StorSimple_System_Requirements).


## <a name="software-requirements"></a>Szoftverkövetelmények

A szoftverkövetelményekkel a támogatott böngészők, kis-és Középvállalatok verziói, virtualizációs platformokon és a virtuális eszközön minimális követelmények vonatkozó információkat tartalmazza.

### <a name="supported-virtualization-platforms"></a>Virtualizációs támogatott platformok


| **Hipervizor** | **Verzió**                          |
|----------------|--------------------------------------|
| A Hyper-V        | A Windows Server 2008 R2 SP1 vagy újabb |
| VMware ESXi    | 5,5 és újabb verziók                        |

### <a name="virtual-device-requirements"></a>Virtuális eszköz vonatkozó követelmények

| **Összetevő**                                | **Követelmények**            |
|----------------------------------------------|----------------------------|
| Virtuális processzorok (magmintákat) minimális száma | 4                          |
| Minimális memória (RAM)                         | 8 GB                       |
| Szabad lemezterület<sup>1</sup>                       | Operációs rendszer merevlemez - 80 GB <br></br>Adatok szabad - és 8 TB 500 GB|
| Hálózati viteldíjjelző illesztőegységeihez minimális száma       | 1                          |
| Minimális internetes sávszélesség<sup>2</sup>       | 5 MB                     |

<sup>1</sup> - vékony kiépítése

<sup>2</sup> – hálózati követelményeknek attól is függhet, a napi adatsebesség módosítása. Például ha egy eszközt kell biztonsági másolatot 10 GB-os vagy további módosításokat nap alatt, majd a napi biztonságimásolat 5 MB-kapcsolaton keresztül óráig is eltarthat legfeljebb 4.25 (ha az adatokat nem lehet tömöríteni vagy deduplicated).

### <a name="supported-web-browsers"></a>Támogatott böngészők

| **Összetevő**     | **Verzió** | **További követelmények/megjegyzések** |
|-------------------|-----------------|-----------------------------------|
| Microsoft Edge    | Legújabb verziója  |                                   |
| Az Internet Explorer | Legújabb verziója  | Az Internet Explorer 11-es tesztelése  |
| Google Chrome     | Legújabb verziója  | A Chrome 46 tesztelése             |

### <a name="supported-storage-clients"></a>Támogatott tároló ügyfelek 

A következő szoftverkövetelményekkel valók a iSCSI kezdeményezők, melyhez hozzáféréssel a virtuális StorSimple tömb (iSCSI-kiszolgálónak beállítva).

| **Támogatott operációs rendszerek** | **Szükséges verzió** | **További követelmények/megjegyzések** |
| --------------------------- | ---------------- | ------------- |
| A Windows Server              | 2008R2 SP1 2012, 2012R2 |StorSimple hozhat létre, vékonyan kiépített és teljesen kiépített kötet. Részben kiépített kötet nem hozható létre. StorSimple iSCSI kötet támogatott csak: <ul><li>Egyszerű kötet Windows egyszerű lemezen.</li><li>Windows NTFS kötet a formázást.</li>|


A következő szoftverkövetelményekkel vannak a kis-és Középvállalatok ügyfelek, melyhez hozzáféréssel a virtuális StorSimple tömb (fájlkiszolgálóra beállítva).

| **Kis-és Középvállalatok verziója** |
|-------------|
| Kis-és Középvállalatok 2.x     |
| KIS-ÉS KÖZÉPVÁLLALATOK 3.0     |
| KIS-ÉS KÖZÉPVÁLLALATOK 3.02    |

> [AZURE.IMPORTANT] Másolja vagy nem védett szerint Windows titkosítja fájl rendszer (titkosított fájlrendszer) StorSimple virtuális tömb fájlkiszolgálóra; fájlok tárolására Ez a eredményez beállításai nem támogatottak. 

## <a name="networking-requirements"></a>Hálózati vonatkozó követelmények 

Az alábbi táblázat a portokat, amelyeket meg kell nyitni a tűzfal iSCSI, kis-és Középvállalatok, felhőalapú vagy management forgalmának engedélyezésére. Az alábbi táblázatban *, vagy *a bejövő* * utal, amely ügyfelek bejövő kéréseit hozzáférni az eszköz irányának. *Ki* vagy a *kimenő* hivatkozik, amelyben az StorSimple eszköz külső felekkel, adatokat küld a telepítést túl irányának: például kimenő az internethez.

| **Port száma<sup>1</sup>** | **Vagy kicsinyítés** | **Port hatóköre** | **Szükséges**              | **Jegyzetek**                                                                                                            |
|--------------------------|---------------|----------------|---------------------------|----------------------------------------------------------------------------------------------------------------------|
| TCP 80-AS (HTTP)            | Ki           | WAN            | nem                        | Kimenő port az Internet-hozzáférés frissítések beolvasásához használt. <br></br>A kimenő webes proxy konfigurálható felhasználói. |
| TCP 443-AS (HTTPS)          | Ki           | WAN            | igen                       | Kimenő portot használja a felhőben eléréséhez. <br></br>A kimenő webes proxy konfigurálható felhasználói. |
| UDP 53 (DNS)             | Ki           | WAN            | Bizonyos esetekben; jegyzetek témakörben talál. | A port csak egy internetes DNS-kiszolgáló esetén szükség. <br></br> **Megjegyzés**: Ha fájlkiszolgálóra telepít, akkor javasoljuk helyi DNS-kiszolgálót használ.|
| UDP 123 (NTP)            | Ki           | WAN            | Bizonyos esetekben; jegyzetek témakörben talál. | A port csak egy internetes NTP kiszolgálóval szükség.<br></br> **Megjegyzés:** Ha a fájlkiszolgálóra telepít, azt javasoljuk, idő szinkronizálása az Active Directory tartományi vezérlők.  |
| TCP 80-AS (HTTP)           | A            | LAN            | igen                       | Ez a helyi felhasználói felület helyi kezelési StorSimple az eszközre a bejövő portot. <br></br> **Megjegyzés**: a felhasználói felület helyi elérése a HTTP-úgy automatikusan irányítja át a HTTPS.|
| TCP 443-AS (HTTPS)          | A            | LAN            | igen                       | Ez a helyi felhasználói felület helyi kezelési StorSimple az eszközre a bejövő portot.|
| TCP 3260 (iSCSI)         | A            | LAN            | nem                        | A port az adatokhoz való hozzáféréshez használt iSCSI fölé.|

<sup>1</sup> nem bejövő portokat kell lehet megnyitni a nyilvános internetkapcsolat.

> [AZURE.IMPORTANT] Győződjön meg arról, hogy a tűzfalon keresztül nem módosíthatók, visszafejtése bármely SSL-forgalmat a StorSimple eszköz és Azure között.


### <a name="url-patterns-for-firewall-rules"></a>Tűzfalszabályokat az URL-cím mintázatok 

Hálózati rendszergazda gyakran adhatja meg a speciális tűzfalszabályokat alapján a URL-CÍMÉT szeretné szűrni a bejövő és kimenő forgalmat. A virtuális tömb és a StorSimple kezelő szolgáltatás más Microsoft-alkalmazások, például Azure Service Bus, Azure Active Directory hozzáférés-vezérlés, tároló fiókok és a Microsoft Update kiszolgálók függ. Az URL-cím mintázatok ezeket az alkalmazásokat társított tűzfalszabályokat konfigurálásához használható. Fontos, ha meg szeretné érteni, hogy az URL-cím mintázatok ezeket az alkalmazásokat társított módosíthatja. Az ehhez a hálózati rendszergazda figyelésére és a StorSimple, és szükség esetén tűzfalszabályokat frissítése. 

Azt javasoljuk, hogy a tűzfalszabályokat kimenő forgalmához, IP-címek liberally rögzíteni a legtöbb esetben StorSimple alapján beállíthatja. Az alábbi információk segítségével azonban biztonságos környezetek kialakításához szükséges speciális tűzfalszabályokat beállítása.

> [AZURE.NOTE] 
> 
> - Az eszköz (forrás) IP-címei mindig legyen a felhő engedélyezni hálózati adaptereken. 
> - A cél IP-címei [Azure adatközpont IP-tartományokat](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653)kell állítani.


| Minta URL-címe                                                      | Összetevő/funkciók                                           |
|------------------------------------------------------------------|---------------------------------------------------------------|
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`   | StorSimple kezelő szolgáltatás<br>Access-vezérlő szolgáltatást<br>Azure Service Bus|
|`http://*.backup.windowsazure.com`|Regisztráció eszköz|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Tanúsítvány-visszavonási |
| `https://*.core.windows.net/*`<br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Azure tároló fiókok és figyelése |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| A Microsoft Update kiszolgálók<br>                        |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN |
| `https://*.partners.extranet.microsoft.com/*`                    | Támogatás csomag                                                  |
| `http://*.data.microsoft.com `                   | Kattintson a Windows telemetriai szolgáltatás jelenik meg a [felhasználói felület és a diagnosztikai telemetriai frissítése](https://support.microsoft.com/en-us/kb/3068708)                                                  |

## <a name="next-step"></a>Következő lépés

-   [Felkészülés a portálon a StorSimple virtuális tömb terjesztése](storsimple-ova-deploy1-portal-prep.md)

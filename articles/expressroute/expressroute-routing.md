<properties
   pageTitle="Készült ExpressRoute útválasztási követelményei |} Microsoft Azure"
   description="Ez az oldal beállításáról és kezeléséről a továbbítás készült ExpressRoute áramkörök részletes követelményeket ismerteti."
   documentationCenter="na"
   services="expressroute"
   authors="osamazia"
   manager="ganesr"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="osamazia"/>


# <a name="expressroute-routing-requirements"></a>Készült ExpressRoute útválasztási vonatkozó követelmények  

A Microsoft-felhőszolgáltatások készült ExpressRoute használatával szeretne csatlakozni, kell beállítása és kezelése a továbbítás. Néhány kapcsolódási szolgáltatók beállítani és kezelni a felügyelt szolgáltatásként útválasztás. Kérdezze meg a csatlakozási szolgáltatójánál, hogy kínálnak, ha ezt a szolgáltatást. Ha nem, meg kell felelniük az alábbi követelményeket. 

Leírását is be kell állítania csatlakozási megkönnyítése érdekében a útválasztási munkameneteket a [áramkörök és útválasztási tartományok](expressroute-circuit-peerings.md) cikke nyújt útmutatást.

>[AZURE.NOTE] A Microsoft nem támogatja a bármely útválasztó redundancia protokollok (például HSRP, VRRP) magas elérhetősége konfigurációk. A felesleges két BGP a munkamenet magas elérhetőség peering telefonszámokkal.

## <a name="ip-addresses-used-for-peerings"></a>Peerings használt IP-címek

Kell az IP-címek konfigurálása között a hálózat és a Microsoft Enterprise széllel (MSEEs) útválasztó továbbítás néhány szövegblokkokat foglalni. Ez a szakasz követelmények: a listája, és ismerteti, hogyan e IP-címek kell szerezte be és használja a szabályokat.

### <a name="ip-addresses-used-for-azure-private-peering"></a>A magánjellegű Azure peering használt IP-címek

Használhatja saját IP-címek és a nyilvános IP-címek konfigurálása az peerings. A cím útvonalak beállításához használt kell nem áll átfedésben címtartományai Azure virtuális hálózatok létrehozására szolgál. 

 - Meg kell foglalni egy /29 alhálózat vagy két /30 alhálózat útválasztási felületekhez.
 - A továbbítás használt alhálózat lehet saját IP-címek és a nyilvános IP-címeket.
 - Az alhálózathoz nem kell a tartomány használata a Microsoft cloud Az ügyfél által fenntartott ütköznek.
 - Ha egy /29 alhálózat használják, a két /30 alhálózat elválasztja. 
     - Az első /30 alhálózat lesz az elsődleges hivatkozásra, és a második/30 alhálózat fogja használni a másodlagos hivatkozásra.
     - Az egyes a /30 alhálózat, az első IP-címét a /30 kell használnia az útválasztó alhálózat. A Microsoft használja a második IP-címét a /30 alhálózat állíthat be egy BGP munkamenetet.
     - Akkor be kell állítania az [Elérhetőség SLA](https://azure.microsoft.com/support/legal/sla/) legyenek érvényesek mindkét BGP munkamenetek.  

#### <a name="example-for-private-peering"></a>Példa a magánjellegű peering

Ha úgy dönt, hogy a.b.c.d/29 használatával állítsa be a peering, akkor fogja lehet osztani két /30 alhálózat. Az alábbi példában megnézi lesz az, hogy hogyan használják a a.b.c.d/29 alhálózat. 

a.b.c.d/29 a.b.c.d/30 és a.b.c.d+4/30 felosztása és a kiépítési API-k – Microsoft továbbítja. Használni kívánt a.b.c.d+1 a VRF IP-az elsődleges PE, és a Microsoft felhasználja a.b.c.d+2, mint a VRF IP-az elsődleges MSEE. Használni kívánt a.b.c.d+5 a VRF IP-a másodlagos PE, és a Microsoft használja a.b.c.d+6 a VRF IP-a másodlagos MSEE.

Fontolja meg, ahol kiválaszthatja állíthatja be a magánjellegű peering 192.168.100.128/29 eset. 192.168.100.128/29 192.168.100.135, többek között a 192.168.100.128 címeket tartalmazza:

- 192.168.100.128/30 192.168.100.129 és a Microsoftnak 192.168.100.130 szolgáltatónál link1, fog kiosztani.
- 192.168.100.132/30 192.168.100.133 és a Microsoftnak 192.168.100.134 szolgáltatónál link2, fog kiosztani.

### <a name="ip-addresses-used-for-azure-public-and-microsoft-peering"></a>Azure nyilvános és a Microsoft peering használt IP-címek

Saját nyilvános IP-címek beállítása a BGP munkamenetek kell használnia. A Microsoft Internet nyilvántartó Útválasztás és Internet útválasztás nyilvántartó keresztül az IP-címek tulajdonjogát ellenőrizheti kell lennie. 

- Egy egyedi kell használnia/29 alhálózat vagy két /30 alhálózat állíthatja be a az egyes peering készült ExpressRoute per peering BGP áramkör (Ha egynél több van). 
- Ha egy /29 alhálózat használják, a két /30 alhálózat elválasztja. 
    - Az első /30 alhálózat lesz az elsődleges hivatkozásra, és a második/30 alhálózat fogja használni a másodlagos hivatkozásra.
    - Az egyes a /30 alhálózat, az első IP-címét a /30 kell használnia az útválasztó alhálózat. A Microsoft használja a második IP-címét a /30 alhálózat állíthat be egy BGP munkamenetet.
    - Akkor be kell állítania az [Elérhetőség SLA](https://azure.microsoft.com/support/legal/sla/) legyenek érvényesek mindkét BGP munkamenetek.

## <a name="public-ip-address-requirement"></a>Nyilvános IP-cím követelmény 

### <a name="private-peering"></a>A magánjellegű Peering 

Megadhatja, hogy nyilvános vagy magánjellegű IPv4-címei használni saját peering. Kínálunk, amely a forgalom végpontok közötti elkülönítési, a többi ügyfél adataitól címeket egymást átfedő ez nem lehetséges a magánjellegű peering esetén. Ezek a címek nincs közzététel van internetkapcsolat. 

### <a name="public-peering"></a>Nyilvános Peering

Az Azure nyilvános peering elérési út lehetővé teszi, hogy csatlakozzon a nyilvános IP-címek fölé Azure-ban tárolt szolgáltatások. Ide tartoznak a szerepel a [ExpessRoute – gyakori kérdések](expressroute-faqs.md) és olyan szolgáltatásokat, a Microsoft Azure ISV által üzemeltetett. Microsoft Azure szolgáltatásaihoz nyilvános peering kapcsolódási mindig a Microsoft-hálózatba kezdeményezett a hálózatról. A Microsoft hálózati adatforgalmat nyilvános IP-címet kell használnia.

### <a name="microsoft-peering"></a>A Microsoft Peering

A Microsoft peering path lehetővé teszi a Microsoft felhőszolgáltatásokhoz által nem támogatott a Azure nyilvános peering elérési útján csatlakozást. A szolgáltatások listáját az Office 365-szolgáltatásokkal, például az Exchange online-ban, a SharePoint online-ban, a Skype vállalati verzió és a CRM Online tartalmazza. A Microsoft a a Microsoft peering támogatja a kétirányú kapcsolat. A Microsoft felhőszolgáltatásokhoz adatforgalmat érvényes nyilvános IPv4-címei kell használnia, mielőtt belépnének a Microsoft-hálózathoz.

Győződjön meg arról, hogy az IP-cím és a szám nyissa meg a az alább felsorolt nyilvántartó egyike regisztrált.

- [ARIN](https://www.arin.net/)
- [APNIC](https://www.apnic.net/)
- [AFRINIC](https://www.afrinic.net/)
- [LACNIC](http://www.lacnic.net/)
- [RIPENCC](https://www.ripe.net/)
- [RADB](http://www.radb.net/)
- [ALTDB](http://altdb.net/)

>[AZURE.IMPORTANT] A Microsoft fölé készült ExpressRoute közzététel nyilvános IP-címek nincs kell közzététel az internethez. Ez megszakíthatják a kapcsolatot egyéb Microsoft-szolgáltatásokhoz. A hálózat-kiszolgálók, amely az Office 365-ös végpontok belül a Microsoft kommunikálni használt nyilvános IP-címek közzététel előfordulhat, hogy készült ExpressRoute fölé. 

## <a name="dynamic-route-exchange"></a>Dinamikus útvonal exchange

Útválasztási exchange eBGP protokollon keresztül lesz. EBGP munkamenetek között a MSEEs és az útválasztó jönnek létre. BGP tanfolyamainkat hitelesítési nem kötelező. Ha szükséges, MD5-ujjlenyomat beállíthatók. Című [konfigurálása útválasztás](expressroute-howto-routing-classic.md) [áramkör kiépítési munkafolyamatok és áramkör állapotát](expressroute-workflows.md) BGP munkamenetek konfigurálásáról.

## <a name="autonomous-system-numbers"></a>Önálló rendszer számok

Microsoft Azure nyilvános, a magánjellegű Azure és a Microsoft peering AS 12076 fogja használni. Azt van lefoglalt ASN-ek 65515 való 65520 belső használatra. 16 és a 32 bites SZÁMKÉNT támogatott.

Nincsenek körüli adatok átadás szimmetria követelmények. A hívásátirányítás és a feladó elérési utak előfordulhat, hogy bejárása különböző útválasztó párokká. Több áramkör párokká, tartozó keresztül azonos útvonalak közzététel vagy oldalról. Útvonal mértékek nem lehet azonos szükséges.

## <a name="route-aggregation-and-prefix-limits"></a>Összesítési és előtag korlátai irányítása

Azure magánjellegű peering keresztül szeretne velünk közzététel legfeljebb 4000 prefixumokban támogatjuk. Ez növelhető legfeljebb 10 000 előtagot, ha a készült ExpressRoute prémium bővítmény engedélyezve van. Azt legfeljebb 200 prefixumokban minden BGP munkamenetben az Azure nyilvános és a Microsoft peering fogadja el. 

Ha prefixumokban száma meghaladja a program eltávolítja a BGP munkamenetet. Akkor fogad el alapértelmezett útvonalak csak a saját peering hivatkozásra. Szolgáltató alapértelmezett útvonal, és saját IP-címek (RFC 1918) az a nyilvános Azure és Microsoft peering elérési út kiszűrése kell. 

## <a name="transit-routing-and-cross-region-routing"></a>Hálózaton átvitt útválasztó és idegen-régió útválasztó

Készült ExpressRoute nem állítható be, mint a hálózaton átvitt útválasztó. Be kell hálózaton átvitt útválasztási szolgáltatások kapcsolódási szolgáltatójától támaszkodhat.

## <a name="advertising-default-routes"></a>Hirdetések alapértelmezett útvonalak

Alapértelmezett útvonalak vannak engedélyezve, a csak az Azure magánjellegű peering munkamenetek. Ebben az esetben azt a hálózat virtuális társított hálózathoz érkező minden forgalom irányítja. Alapértelmezett útvonalak hirdetési magánjellegű peering be az internetes elérési út az Azure blokkolva eredményezi. A kezdő és záró az Azure-ban tárolt szolgáltatások az internet a forgalmat a vállalati él kell használja arra. 

 Ahhoz, hogy más Azure-szolgáltatások és infrastruktúrájának szolgáltatásai kapcsolódási, meg kell győződnie arról hely szerepel az alábbi elemek egyikére:

 - Azure nyilvános peering engedélyezve van a forgalom átirányítása nyilvános végpontok
 - Használhatja a felhasználó által definiált Útválasztás engedélyezése minden internetkapcsolat igénylő alhálózat internetkapcsolat.
 
>[AZURE.NOTE] Alapértelmezett útvonalak hirdetési megszakítja a Windows és az egyéb virtuális licenc aktiválása. Kövesse az utasításokat [Itt](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx) kerülhető meg.

## <a name="support-for-bgp-communities-preview"></a>Támogatás a BGP Közösségek (előzetes verzió)


Ez a témakör áttekintést hogyan BGP Közösségek használandó készült ExpressRoute. A Microsoft a megfelelő közösségi értékű címkézett útvonalak a nyilvános és a Microsoft peering elérési útvonalak fog helyüket. Ha így indoklását közösségi értékeket tartalmaz a részletes leírása az alábbiakban olvasható. Microsoft, azonban, akkor nem figyelembe veszi a Microsoftnak közzététel útvonalak címkézett közösségi értékeket.

Ha a Microsoftnak a geopolitikai régión belüli egy peering bárhol készült ExpressRoute keresztül csatlakozik, fog minden Microsoft felhőszolgáltatásokhoz hozzáférést a minden terület belül az geopolitikai oszlopazonosító között. 

Például ha Amsterdam készült ExpressRoute keresztül a Microsoft csatlakozva, hozzáférést kap Észak-Európában és a nyugati Europe tárolt összes Microsoft felhőszolgáltatásokhoz. 

Keresse meg a [készült ExpressRoute partnerek és peering helyek](expressroute-locations.md) lap geopolitikai régiók, a társított Azure régiók és a helyek peering megfelelő készült ExpressRoute részletes listáját.

Egynél több készült ExpressRoute áramkör per geopolitikai régió vásárolhat. Több kapcsolat problémákat felajánlja a magas elérhetősége miatt a redundancia geo jelentős előnyökkel jár. Azokban az esetekben, ahol több készült ExpressRoute áramkörök van ugyanazok a prefixumokban a Microsoft nyilvános peering a és a görbék peering Microsoft Közzététel kap. Ez azt jelenti, hogy több út a hálózatról Microsoft be lesz. Esetleg okozhat az optimális útválasztási döntéseket tenni a hálózaton belül. Emiatt tapasztalhatja meg különböző szolgáltatások optimális kapcsolódási kezelőfelülete. 

Microsoft fogja nyomon követése a prefixumokban közzététel nyilvános peering keresztül, és a Microsoft értékű megfelelő BGP közösségi a területeket jelző peering a prefixumokban vannak a. A [Vevők optimális továbbítás](expressroute-optimize-routing.md)kínálatát megfelelő útválasztási döntések közösségi értékeket számíthat ki.

| **Geopolitikai terület** | **Microsoft Azure terület** | **BGP közösségi érték** |
|---|---|---|
| **Észak-Amerika** |    |  |
|    | Kelet-Amerikai Egyesült Államok | 12076:51004 |
|    | Kelet-amerikai 2 | 12076:51005 |
|    | Nyugati Amerikai Egyesült Államok | 12076:51006 |
|    | Nyugati USA-beli 2 | 12076:51026 |
|    | Nyugati központi Amerikai Egyesült Államok | 12076:51027 |
|    | A központi Észak-amerikai | 12076:51007 |
|    | A központi Dél-Amerikai Egyesült Államok | 12076:51008 |
|    | A központi Amerikai Egyesült Államok | 12076:51009 |
|    | Közép Kanadán | 12076:51020 |
|    | Kanada-keleti | 12076:51021 |
| **Dél-Amerika** |  |  |
|    | Brazília Dél | 12076:51014 |
| **Európa** |    |  |
|    | Észak-Európa | 12076:51003 |
|    | Nyugati Európa | 12076:51002 |
| **Ázsia Csendes-óceáni** |    |   |
|    | Kelet-ázsiai | 12076:51010 |
|    | Délkelet-ázsiai | 12076:51011 |
| **Japán** |     |   |
|    | Japán keleti | 12076:51012 |
|    | Japán nyugati | 12076:51013 |
| **Ausztrália** |    |   | 
|    | Ausztrália keleti | 12076:51015 |
|    | Ausztrália Könyvesbolt is. | 12076:51016 |
| **India** |    |   |
|    | India Dél | 12076:51019 |
|    | India nyugati | 12076:51018 |
|    | India központi | 12076:51017 |

Közzététel a Microsoft összes útvonalak címkét kapnak a megfelelő közösségi értéket. 

>[AZURE.IMPORTANT] Globális prefixumokban fog címkézve van egy megfelelő közösségi értéket, és csak akkor, ha engedélyezve van készült ExpressRoute prémium bővítmény meghirdetése történik.


A fenti kívül Microsoft is nyomon követése a prefixumokban a szolgáltatás tartoznak alapján. Ez csak vonatkozik a Microsoft peering. Az alábbi táblázat a megfeleltetés szolgáltatás BGP közösségi értéket nyújt.

| **Szolgáltatás** | **BGP közösségi érték** |
|---|---|
| **Exchange** | 12076:5010 |
| **A SharePoint** | 12076:5020 |
| **A Skype vállalati verzió** | 12076:5030 |
| **CRM Online** | 12076:5040 |
| **Más Office 365-szolgáltatások** | 12076:5100 |

>[AZURE.NOTE] A Microsoft a Microsoftnak közzététel útvonalak beállított BGP közösségi értékeket nem veszi figyelembe.

## <a name="next-steps"></a>Következő lépések

- Állítsa be a készült ExpressRoute kapcsolatot.

    - [A klasszikus telepítési modell egy készült ExpressRoute áramkör létrehozása](expressroute-howto-circuit-classic.md) vagy [létrehozása és módosítása az Azure-kezelővel készült ExpressRoute áramkör](expressroute-howto-circuit-arm.md)
    - [A klasszikus telepítési modell útválasztás konfigurálás](expressroute-howto-routing-classic.md) vagy [az erőforrás-kezelő telepítési modell útválasztás konfigurálása](expressroute-howto-routing-arm.md)
    - [Hivatkozás a klasszikus VNet szeretne egy készült ExpressRoute áramkör](expressroute-howto-linkvnet-classic.md) vagy a [hivatkozás egy erőforrás-kezelő VNet szeretne egy készült ExpressRoute áramkör](expressroute-howto-linkvnet-arm.md)



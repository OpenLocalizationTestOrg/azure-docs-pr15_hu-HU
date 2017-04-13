<properties
   pageTitle="Alkalmazás hibás átjárók (502) hibák elhárítása |} Microsoft Azure"
   description="Megtudhatja, hogy miként alkalmazás átjáró 502 hibák elhárítása"
   services="application-gateway"
   documentationCenter="na"
   authors="amitsriva"
   manager="rossort"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/02/2016"
   ms.author="amitsriva" />

# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a>Az alkalmazás átjáró hibás átjáró hibáinak elhárítása

## <a name="overview"></a>– Áttekintés

Miután olyan Azure átjárót, a hibák, amelyek a felhasználók előfordulhatnak egyik "kiszolgálóhiba: 502 – webkiszolgáló érvénytelen választ kapott során eljáró átjáró vagy a proxykiszolgáló kiszolgálójával". Ez akkor fordulhat elő, az alábbi főbb okok miatt:

- Azure alkalmazás átjáró háttéradatbázist készlet nincs konfigurálva vagy nem üres.
- A VMs vagy példányok virtuális skála megadása sem megfelelő.
- Háttéradatbázis VMs vagy virtuális skála megadása előfordulását nem válaszol az alapértelmezett állapot vizsgálati szeretne.
- Egyéni állapot érvénytelen vagy helytelen konfigurációja ellenőrzi.
- Kérés időkorlátja vagy csatlakozási problémákat tapasztal a felhasználói kérések.

## <a name="empty-backendaddresspool"></a>Üres BackendAddressPool

### <a name="cause"></a>OK

Ha az alkalmazás átjáró nem rendelkezik VMs vagy virtuális skála megadása a háttéradatbázist cím készletben beállítva, nem átirányíthatja az ügyfél kérésének, és a hibás átjáró hibát okoz.

### <a name="solution"></a>Megoldás

Győződjön meg arról, hogy a háttéradatbázist cím készlet nem üres. Ez történik vagy PowerShell, CLI vagy portálon keresztül.

    Get-AzureRmApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"

Az előző parancsmag kimenete tartalmaznia kell a nem üres háttér-címet. Következő képen hol két készletek visszaadott amely kódmentes VMs FQDN vagy IP-címek vannak beállítva. A BackendAddressPool kiépítési állapotának kell lennie "sikeres".
    
    BackendAddressPoolsText: 
            [{
                "BackendAddresses": [{
                    "ipAddress": "10.0.0.10",
                    "ipAddress": "10.0.0.11"
                }],
                "BackendIpConfigurations": [],
                "ProvisioningState": "Succeeded",
                "Name": "Pool01",
                "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
                "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool01"
            }, {
                "BackendAddresses": [{
                    "Fqdn": "xyx.cloudapp.net",
                    "Fqdn": "abc.cloudapp.net"
                }],
                "BackendIpConfigurations": [],
                "ProvisioningState": "Succeeded",
                "Name": "Pool02",
                "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
                "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool02"
            }]


## <a name="unhealthy-instances-in-backendaddresspool"></a>Sérült példányok BackendAddressPool

### <a name="cause"></a>OK

Ha BackendAddressPool összes előfordulását sérült, majd alkalmazás átjáró nem kellene bármely háttéradatbázist az útvonal felhasználói kérelmet. Az esetben is ennek oka lehet, amikor a háttéradatbázist példányok megfelelő azonban nem rendelkezik a szükséges alkalmazás telepítve.

### <a name="solution"></a>Megoldás

Győződjön meg arról, hogy a megfelelő elemei, és az alkalmazás helyesen van beállítva. Annak ellenőrzése, hogy válaszolhat a ping át egy másik virtuális be az azonos VNet-e a háttéradatbázist példányok. Ha be van állítva egy nyilvános végpont, győződjön meg arról, hogy a webalkalmazás böngésző kérelmének javítható.

## <a name="problems-with-default-health-probe"></a>Alapértelmezett állapot vizsgálati kapcsolatos problémák

### <a name="cause"></a>OK

502 hibák is lehet, hogy az alapértelmezett állapot vizsgálati nem tudja elérni a háttéradatbázist VMs gyakori jelölők. Ha egy alkalmazás átjárópéldány már kiépítve, automatikusan beállítja egy alapértelmezett állapot vizsgálati az egyes BackendAddressPool a BackendHttpSetting tulajdonságainak használata. Nincs felhasználó által a vizsgálati beállításához szükséges. Ha egy terheléselosztási szabály van beállítva, társítást van elvégzett BackendHttpSetting és BackendAddressPool között. Be van állítva egy alapértelmezett vizsgálati minden egyes e társítások, és az alkalmazás átjáró a BackendAddressPool a megadott BackendHttpSetting eleme port az egyes példányok periodikus állapot ellenőrzése kapcsolatot kezdeményez. Következő táblázat az alapértelmezett állapot vizsgálati társított értékek.


|Vizsgálati tulajdonság | Érték | Leírás|
|---|---|---|
| Vizsgálati URL-címe| http://127.0.0.1/ | URL-címe |
| Intervallum | 30 | A másodpercben megadott vizsgálati intervallum |
| Időtúllépési  | 30 | A másodpercben megadott vizsgálati időtúllépését |
| Sérült küszöbérték | 3 | Probe kísérletek száma. A háttéradatbázist kiszolgáló megjelölve, miután az egymás utáni vizsgálati hiba száma eléri a sérült küszöbértéket. |

### <a name="solution"></a>Megoldás

- Győződjön meg arról, hogy egy alapértelmezett hely van beállítva, és a 127.0.0.1 figyel.
- BackendHttpSetting eltérő 80-as port adja meg, ha az alapértelmezett hely kell beállítania, hogy az adott port meghallgatása.
- A hívást http://127.0.0.1:port térjen vissza egy 200 HTTP-eredmény kódot. Ez az 30 másodperc időtúllépési időszakon belül vissza kell.
- Győződjön meg arról, hogy konfigurálva port nyitva-e, és hogy nincsenek-e tűzfalszabályokat vagy letiltani a bejövő és kimenő forgalmának konfigurált portot Azure hálózati biztonsági csoportokat.
- Azure klasszikus VMs vagy Felhőszolgáltatásában FQDN és nyilvános IP használt, győződjön meg arról, hogy a megfelelő [végpont](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md) nyitja meg.
- Ha a virtuális keresztül Azure erőforrás-kezelő be van állítva, és a VNet, ha telepítve van az alkalmazás átjáró kívül esik, [Hálózati biztonsági csoport](../virtual-network/virtual-networks-nsg.md) kell beállítania, hogy a kívánt port elérésének.


## <a name="problems-with-custom-health-probe"></a>Egyéni állapot vizsgálati kapcsolatos problémák

### <a name="cause"></a>OK

Egyéni állapot szondákat további lehetősége, hogy az alapértelmezett működés számlálása teszi lehetővé. Egyéni szondákat használata esetén a felhasználók beállíthatják a vizsgálati időköz, az URL-címet, és javaslat kipróbálása és fogadja el a háttéradatbázist készlet példány megsérült megjelölése előtti hány sikertelen válaszokat. A következő további tulajdonságokat kerülnek.

|Vizsgálati tulajdonság| Leírás|
|---|---|
| név | A vizsgálati neve. Ez a név lehet hivatkozni a háttéradatbázist HTTP-beállításai a vizsgálati használják. |
| Protocol (protokoll) | Protocol (protokoll) a vizsgálati küldi. A vizsgálati fogja használni a háttéradatbázist HTTP beállításaiban megadott Protocol (protokoll) |
| A Host |  A Host name a vizsgálati küldhet. Csak akkor, ha több helyet be van állítva, a alkalmazás átjáró alkalmazható. Ez különbözik virtuális host Name mezőbe.  |
| Elérési út | Relatív elérési útját a vizsgálati. Elindítja az érvényes elérési út "/". A vizsgálati küldött \<protocol\>://\<host\>:\<port\>\<elérési út\> |
| Intervallum | Intervallum Probe a másodpercben megadott. Ez a két egymást követő szondákat között eltelt idő.|
| Időtúllépési | Időtúllépési Probe a másodpercben megadott. A vizsgálati van megjelölve sikertelen, ha nem érkezik egy érvényes válasz ezen időtúllépési időszakon belül. |
| Sérült küszöbérték | Probe kísérletek száma. A háttéradatbázist kiszolgáló megjelölve, miután az egymás utáni vizsgálati hiba száma eléri a sérült küszöbértéket. |


### <a name="solution"></a>Megoldás

Ellenőrizze, hogy az egyéni állapot Probe helyesen van beállítva az előző táblázatként. Az előző hibaelhárítási lépések mellett is győződjön meg az alábbiakról:

- Győződjön meg arról, hogy a vizsgálati megfelelően van-e megadva szerint az [Útmutató](application-gateway-create-probe-ps.md).
- Ha alkalmazás átjáró webhely van konfigurálva, a szolgáltató alapértelmezés szerint nevét kell megadni, "127.0.0.1", hacsak nem állítja be az egyéni vizsgálati másként.
- Győződjön meg arról, hogy hívást kezdeményez http://\<host\>:\<port\>\<elérési út\> 200-HTTP eredmény kódját adja eredményül.
- Bizonyosodjon meg arról, hogy időköze, időtúllépés és UnhealtyThreshold elfogadható tartományba eső.

## <a name="request-time-out"></a>Kérés időtúllépést okoz

### <a name="cause"></a>OK

Amikor egy felhasználó kérelem érkezik, alkalmazás átjáró konfigurált szabályok vonatkozik a kérést, és irányítja azt a háttéradatbázist készlet példány. Egy konfigurálható időtartományt a háttéradatbázist példányból választ vár. Alapértelmezés szerint a megadott időintervallum érték **30 másodperc**. Ha az alkalmazás átjáró nem arra választ kap a megadott időintervallum háttéradatbázist alkalmazásból, a felhasználói kérelem volna 502 hibát lát.

### <a name="solution"></a>Megoldás

Alkalmazás átjáró lehetővé teszi a felhasználóknak, hogy ez a beállítás keresztül BackendHttpSetting, amelyek különböző készletek majd alkalmazza. Másik háttéradatbázis készletek különböző BackendHttpSetting, és így különböző kérés időkorlátja konfigurálva van.

    New-AzureRmApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60

## <a name="next-steps"></a>Következő lépések

Ha a fenti lépések nem oldották meg a problémát, nyissa meg a [támogatási jegyek](https://azure.microsoft.com/support/options/).

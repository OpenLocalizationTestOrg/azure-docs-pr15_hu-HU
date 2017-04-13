

<properties
   pageTitle="Áttekintés figyelése Azure alkalmazás átjáró állapota |} Microsoft Azure"
   description="Megismerheti az Azure alkalmazás átjáró felügyeleti funkcióit"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="application-gateway-health-monitoring-overview"></a>Alkalmazás átjáró állapota – áttekintés

Azure alkalmazás átjáró alapértelmezés szerint állapotát jelző összes erőforrásában található a háttéradatbázist készlet figyeli és bármely figyelembe venni a sérült a készletből erőforrás automatikusan törli. Alkalmazás átjáró a sérült példányok Lync továbbra is fennáll, és felveszi őket vissza a megfelelő háttéradatbázist készletbe elérhetővé válnak, és állapota szondákat megválaszolása után. Alkalmazás átjáró küld az állapot ellenőrzi az ugyanazt a portot határozza meg a háttéradatbázis HTTP-beállításait. Ezzel biztosíthatja, hogy a vizsgálati ellenőrzi, hogy a vevők volna használ-e csatlakozni az kódmentes ugyanazt a portot.

![alkalmazás átjáró vizsgálati például][1]

A alapértelmezett rendszerállapot figyelése vizsgálati, mellett is testreszabhatja az állapot vizsgálati az igényeinek, az alkalmazás követelményeknek. Ebben a cikkben alapértelmezett és egyéni állapot szondákat is szerepelnek.

## <a name="default-health-probe"></a>Alapértelmezett állapot vizsgálati

Az alkalmazás átjáró automatikusan beállítja egy alapértelmezett állapot vizsgálati, beállításakor nem egyéni vizsgálati beállításokat. A felügyeleti működik HTTP felkérés, így az IP-címek a háttéradatbázist készlet be van állítva. Az alapértelmezett szondákat kódmentes http beállításai vannak HTTPS-, ha a vizsgálati használandó https, valamint a háttérkiszolgálókon állapotának ellenőrzéséhez.

Példa: beállíthatja, hogy az alkalmazás átjáró háttér-kiszolgálók A, B és C használni szeretné fogadni a 80-as port HTTP hálózati forgalmának engedélyezésére. Az alapértelmezett rendszerállapot figyelése azt vizsgálja, a három kiszolgálók 30 másodpercenként megfelelő HTTP választ. A megfelelő HTTP-válasz 200 és 399 közötti [állapotkódot](https://msdn.microsoft.com/library/aa287675.aspx) tartalmaz.

Az alapértelmezett vizsgálati ellenőrzése kiszolgáló sikertelen, ha az alkalmazás átjáró eltávolítja a háttéradatbázist készletből, és a hálózati forgalmának engedélyezésére leállítja a kiszolgáló halad. Az alapértelmezett vizsgálati továbbra is A 30 másodpercenként a kiszolgáló ellenőrzése. Kiszolgáló válaszol sikerült egy kérés egy alapértelmezett állapot vizsgálati az, ha nem adja hozzá újra a megfelelő a háttéradatbázist készlet, és a forgalom elindítja a kiszolgáló halad újra.

### <a name="default-health-probe-settings"></a>Az alapértelmezett állapot vizsgálati beállításai

|Vizsgálati tulajdonság | Érték | Leírás|
|---|---|---|
| Vizsgálati URL-címe| http://127.0.0.1:\<port\>/ | URL-címe |
| Intervallum | 30 | A másodpercben megadott vizsgálati intervallum |
| Időtúllépési  | 30 | A másodpercben megadott vizsgálati időtúllépését |
| Sérült küszöbérték | 3 | Probe kísérletek száma. A háttéradatbázist kiszolgáló megjelölve, miután az egymás utáni vizsgálati hiba száma eléri a sérült küszöbértéket. |

> [AZURE.NOTE] A port mindig ugyanazt a portot, a háttéradatbázist HTTP-beállításait.

Az alapértelmezett vizsgálati csak vizsgálja http://127.0.0.1:\<port\> állapot állapot határozza meg. Ha kell konfigurálni az állapot vizsgálati nyissa meg az egyéni URL-cím vagy bármely más beállításokat módosíthatja, az alábbi lépésekkel leírt módon használja egyéni szondákat.

## <a name="custom-health-probe"></a>Egyéni állapot vizsgálati

Egyéni szondákat lehetővé teszi a rendszerállapot figyelése finomabb hozzáférésük van. Egyéni szondákat használatakor, beállíthatja a vizsgálati időköz, az URL-cím és javaslat kipróbálása és fogadja el a háttéradatbázist készlet példány megsérült megjelölése előtti hány sikertelen válaszokat.

### <a name="custom-health-probe-settings"></a>Egyéni állapot vizsgálati beállításai

A következő táblázat tartalmaz egy egyéni állapot vizsgálati tulajdonságainak definícióinak.

|Vizsgálati tulajdonság| Leírás|
|---|---|
| név | A vizsgálati neve. Ez a név lehet hivatkozni a háttéradatbázist HTTP-beállításai a vizsgálati használják. |
| Protocol (protokoll) | Protocol (protokoll) a vizsgálati küldi. A vizsgálati fogja használni a háttéradatbázist HTTP beállításaiban megadott Protocol (protokoll) |
| A Host |  A Host name a vizsgálati küldhet. Alkalmazandó csak akkor, ha több helyet be van állítva a alkalmazás átjáró, egyéb esetben a "127.0.0.1" használja. Ez különbözik virtuális host Name mezőbe. |
| Elérési út | Relatív elérési útját a vizsgálati. Elindítja az érvényes elérési út "/". |
| Intervallum | Intervallum Probe a másodpercben megadott. Ez a két egymást követő szondákat között eltelt idő.|
| Időtúllépését | Időtúllépési Probe a másodpercben megadott. A vizsgálati van megjelölve sikertelen, ha nem érkezik egy érvényes válasz ezen időtúllépési időszakon belül. |
| Sérült küszöbérték | Probe kísérletek száma. A háttéradatbázist kiszolgáló megjelölve, miután az egymás utáni vizsgálati hiba száma eléri a sérült küszöbértéket. |

> [AZURE.IMPORTANT] Ha alkalmazás átjáró webhely van konfigurálva, a szolgáltató alapértelmezés szerint nevét kell megadni, "127.0.0.1", hacsak nem állítja be az egyéni vizsgálati másként.
Hivatkozás egy egyéni vizsgálati a szolgáltatás elküldi a \<protokoll\>://\<host\>:\<port\>\<elérési út\>. A használt port ugyanazt a portot lesz, a háttéradatbázist HTTP beállításaiban megadott.

## <a name="next-steps"></a>Következő lépések

Alkalmazás átjáró rendszerállapot figyelése megtanulása, után egy [egyéni állapot vizsgálati](application-gateway-create-probe-portal.md) az Azure-portálra vagy egy [egyéni állapot vizsgálati](application-gateway-create-probe-ps.md) PowerShell és az erőforrás-kezelő Azure telepítési modell beállítására.

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png
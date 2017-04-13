<properties
   pageTitle="Az alkalmazás átjáró létrehozása a portálon webes alkalmazás tűzfallal |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre egy alkalmazás átjáró tűzfallal weben alkalmazás, a portál használatával"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"
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

# <a name="create-an-application-gateway-with-web-application-firewall-by-using-the-portal"></a>Hozzon létre egy alkalmazás átjáró tűzfallal weben alkalmazás a portál használatával

> [AZURE.SELECTOR]
- [Azure portál](application-gateway-web-application-firewall-portal.md)
- [Azure erőforrás-kezelő PowerShell](application-gateway-web-application-firewall-powershell.md)

A webes alkalmazás tűzfal (WAF) Azure alkalmazás átjáró webalkalmazások közös webes célú támadásokkal például SQL-beszúrás, a webhelyközi parancsfájlok célú támadásokkal és a munkamenet kihasználásának védelmet nyújt. Webalkalmazás OWASP felső 10 közös webes biztonsági számos védelmet.

Azure alkalmazás az átjáró az egy réteg-7 terheléselosztó. A feladatátvétel HTTP-kérések teljesítményt továbbítása különböző kiszolgálók között, akár a helyszíni vagy felhőalapú biztosít.
Alkalmazás számos alkalmazás kézbesítési vezérlő LÉPETT funkciói, köztük a HTTP terheléselosztás, cookie-alapú munkamenet affinitás, akkor Secure Sockets Layer (SSL) kiürítése, egyéni állapot szondákat, több webhelyen támogatása és számos más.
Ha a támogatott szolgáltatások listáját, látogasson el az [Alkalmazás átjáró – áttekintés](application-gateway-introduction.md)

## <a name="scenarios"></a>Felhasználási területei

Ez a cikk a forgatókönyv két:

Az első eset hozzáadása a [webes alkalmazás tűzfalat, hogy egy meglévő alkalmazás átjáró](#add-web-application-firewall-to-an-existing-application-gateway)megismerheti.

A második helyzetben, megtudhatja, hogy miként [-alkalmazás átjáró tűzfallal weben alkalmazás létrehozása](#create-an-application-gateway-with-web-application-firewall)

![Forgatókönyv példa][scenario]

>[AZURE.NOTE] Az alkalmazás átjáró, beleértve az egyéni állapot további konfigurációs ellenőrzi, kódmentes készlet címeket és a többi szabályt az alkalmazás átjáró beállítása után, és a kezdeti telepítés során nem konfigurálja.

## <a name="before-you-begin"></a>Első lépések

Azure alkalmazás átjáró saját alhálózat szükséges. Virtuális hálózat létrehozásakor győződjön meg arról, hogy hagyja, hogy több alhálózat elég címterület használatára. Miután-alhálózat átjáró alkalmazás telepítéséhez csak további alkalmazás átjárók képesek adható hozzá a alhálózat.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Webes alkalmazás tűzfal hozzáadása egy meglévő alkalmazás átjáró

Ebben az esetben a webes alkalmazás tűzfal támogatási megelőzése módban egy meglévő alkalmazás átjáró frissítése

### <a name="step-1"></a>Lépés: 1

Nyissa meg az Azure portált, és válassza egy meglévő alkalmazás átjáró.

![Alkalmazás átjáró létrehozása][1]

### <a name="step-2"></a>Lépés: 2

Kattintson a **konfiguráció** , és az alkalmazás átjáró beállításainak frissítése. Amikor elkészült kattintson a **Mentés**

A webes alkalmazás tűzfal támogatási egy meglévő alkalmazás átjáró frissítése a beállítások az alábbiak:

- **Réteg** – a kijelölt réteg **WAF** a webes alkalmazás tűzfal támogatási kell lennie.
- Az alkalmazás átjáró tűzfallal weben alkalmazás mérete **Termékváltozat méret** - ezt a beállítást, rendelkezésre álló beállítások (**Közepes** és **nagy**).
- **Tűzfal állapota** – Ez a beállítás letiltja vagy lehetővé teszi, hogy a webes alkalmazás tűzfalon keresztül.
- **Tűzfalas üzemmódban** – ezt a beállítást, amely hogyan a webes alkalmazás tűzfal kártékony forgalom foglalkozik. **Észlelési** módot csak naplózza az eseményeket, ahol **megakadályozási** mód naplózza az eseményeket, és leállítja a kártékony forgalmat.

![a lap megjelenítő alapvető beállításai][2]

>[AZURE.NOTE] Webes alkalmazás tűzfal naplók megtekintéséhez diagnosztika engedélyezve kell lennie, és ApplicationGatewayFirewallLog jelölve. Az 1-példányok száma tesztelési célú lehet kiválasztani. Fontos tudnia, hogy, hogy minden példányok száma a két példánya nem vonatkozik a SLA, és ezért nem ajánlott. Kis átjárók nem érhetők el, amikor webes alkalmazás tűzfalat használ.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Hozzon létre egy alkalmazás átjáró tűzfallal weben alkalmazás

Ebben az esetben fognak:

- Közepes webes alkalmazás tűzfal alkalmazás az átjárók létrehozása két példánya.
- Hozzon létre egy virtuális hálózati nevű AdatumAppGatewayVNET 10.0.0.0/16 fenntartott CIDR blokkja a.
- A CIDR sávként 10.0.0.0/28 használó Appgatewaysubnet nevű alhálózat létrehozása.
- Kiürítése SSL-tanúsítvány beállítása.

### <a name="step-1"></a>Lépés: 1

Nyissa meg az Azure portált, kattintson az **Új** > **hálózati** > **Alkalmazás átjáró**

![Alkalmazás átjáró létrehozása][1-1]

### <a name="step-2"></a>Lépés: 2

Ezután adja meg az alkalmazás átjáró alapvető információkat. Győződjön meg arról, válassza ki a **WAF** , mint a réteg. Amikor elkészült **az OK** gombra.

Az alapvető beállítások szükséges információt a következő:

- **Név** – az alkalmazás átjáró neve.
- **Réteg** – az alkalmazás átjáró a réteg lehetőségek (**normál** és **WAF**). Webes alkalmazás tűzfalat a WAF réteg csak érhető el.
- Az alkalmazás átjáró mérete **Termékváltozat méret** - ezt a beállítást, rendelkezésre álló beállítások (**Közepes** és **nagy**).
- **Példányok száma** - példányok száma, ezt az értéket kell **2** és **10**közé eső szám.
- **Erőforráscsoport** – az alkalmazás átjáró tartása az erőforráscsoport Ez lehet egy meglévő erőforráscsoport vagy egy újat.
- **Hely** – az alkalmazás átjáró a régió erőforrás csoport ugyanazon a helyen. *A hely a virtuális hálózatként, és nyilvános IP kell lennie az átjáró ugyanazon a helyen*.

![a lap megjelenítő alapvető beállításai][2-2]

>[AZURE.NOTE] Az 1-példányok száma tesztelési célú lehet kiválasztani. Fontos tudnia, hogy, hogy minden példányok száma a két példánya nem vonatkozik a SLA, és ezért nem ajánlott. Kis átjárók webes alkalmazás tűzfal esetek nem támogatott.

### <a name="step-3"></a>3 lépés

Az alapvető beállítások határozzák meg, miután a következő lépésként meghatározása a virtuális hálózat használandó. A virtuális hálózat otthont ad az alkalmazást, az alkalmazás átjáró terheléselosztás számára.

Kattintson a **Válasszon egy virtuális hálózat** konfigurálása a virtuális hálózat.

![a lap beállításaival alkalmazás átjáró][3]

### <a name="step-4"></a>Lépés: 4

A **Virtuális hálózat kiválasztása** a lap kattintson az **Új létrehozása**

Ebben az esetben nem magyarázata, miközben egy meglévő virtuális hálózat ezen a ponton sikerült jelölhet ki.  Egy meglévő virtuális hálózat használata esetén fontos tudni, hogy a virtuális hálózat szüksége van egy üres alhálózat vagy csak alkalmazás átjáró erőforrások használandó alhálózat.

![Válassza ki a virtuális hálózati lap][4]

### <a name="step-5"></a>5 lépésben

Töltse ki a hálózati a **Virtuális hálózati létrehozása** lap, a megelőző [forgatókönyv](#scenario) leírás leírt módon.

![Virtuális hálózati lap létrehozása a megadott adatokkal][5]

### <a name="step-6"></a>6 lépés

Amikor létrejött a virtuális hálózat, következő lépésként az előtér-IP az alkalmazás átjáró meghatározása. Ezen a ponton a választási lehetőségek egy nyilvános vagy egy privát IP-címet az előtér-között van. A választási lehetőségek, attól függ, hogy az alkalmazás-e internetes szemben lévő vagy belső csak. Ebben az esetben azt feltételezi, hogy egy nyilvános IP-cím használatával. Válassza ki a személyes IP-címet, a **magánjellegű** gomb rákattintva a. Választja ki automatikusan hozzárendelt IP-címet, vagy választhatja a kézzel írja be a **választ egy adott magánjellegű IP-cím** jelölőnégyzet.

Kattintson a **Kiválasztás egy nyilvános IP-címet**. Ha egy meglévő nyilvános IP-cím érhető el ezen a ponton választott, és ebben az esetben hoz létre egy új nyilvános IP-címet. Kattintson az **Új létrehozása**

![Válassza a nyilvános IP-cím lap][6]

### <a name="step-7"></a>Lépés 7

Ezután a nyilvános IP-címet adjon meg egy rövid nevet, és kattintson az **OK gombra**

![Nyilvános IP-címét a lap létrehozása][7]

### <a name="step-8"></a>Lépés 8

Ezután a figyelő konfiguráció beállítása.  **Http** használata esetén konfigurálása semmi nem marad, és rákattintva **az OK gombra** . **Https** használatához további konfigurációs szükség.

**Https**használatához tanúsítvány szükség. A tanúsítvány a titkos kulcs van szükség, így kell adni a tanúsítvány igények és a jelszót, a fájl exportálása egy .pfx.

Kattintson a **HTTPS**, majd a **mappa** ikonja mellett a **Feltöltés PFX tanúsítvány** mezőben lévő értéket.
Nyissa meg a fájlrendszerben .pfx biztonságitanúsítvány-fájl. Ha be van jelölve, a tanúsítvány adjon meg egy rövid nevet, és írja be a jelszót az .pfx fájl.

Befejezése után kattintson az **OK gombra** , tekintse át a beállításokat, az alkalmazás átjáró.

![Figyelő konfigurációs szakasz a beállítások lap][8]

### <a name="step-9"></a>Lépés 9

Az adott **WAF** beállításainak konfigurálása

- Be- és kikapcsolása **tűzfal állapota** - ezt a beállítást, azzal kikapcsolja WAF.
- **Tűzfalas üzemmódban** – Ez a beállítás azt határozza meg, rosszindulatú forgalmat a műveletek WAF veszi fel. Ha **észlelési** választja, akkor forgalom csak be van jelentkezve.  Ha **megelőzése** választja, adatforgalom bejelentkezett és leállt a nem engedélyezett: 403 a.

![webalkalmazás tűzfal beállításai][9]

### <a name="step-10"></a>10 lépés

Tekintse át az Összegzés lapon, és kattintson az **OK gombra**.  Most már az alkalmazás átjáró aszinkron és létrehozott.

### <a name="step-12"></a>Lépésben 12

Az alkalmazás átjáró létrehozása után nyissa meg azt a portálon a konfigurálás az alkalmazás átjáró folytatásához.

![Alkalmazás átjáró Erőforrásnézet][10]

Ezeket a lépéseket a figyelő, kódmentes készlet, kódmentes http beállítások és szabályok alapértelmezett beállításainak egyszerű alkalmazás az átjárók létrehozása. Ezeket a beállításokat, hogy megfeleljen a telepítéssel, amikor elkészült a kiépítési sikeres módosíthatók

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogy miként bejelentkezni az események vagy miatt nem webes alkalmazás tűzfallal megtalálhatók az [Alkalmazás átjáró diagnosztika](application-gateway-diagnostics.md) a diagnosztikai naplózás beállítása

Megtudhatja, hogy miként hozhat létre egyéni állapot szondákat megtalálhatók, [Hozzon létre egy egyéni állapot vizsgálati](application-gateway-create-probe-portal.md)

Megtudhatja, hogy miként SSL mert konfigurálása és a költséges SSL visszafejtés kikapcsolása a webkiszolgálón megtalálhatók az [SSL kiürítése konfigurálása](application-gateway-ssl-portal.md)

<!--Image references-->
[1]: ./media/application-gateway-web-application-firewall-portal/figure1.png
[2]: ./media/application-gateway-web-application-firewall-portal/figure2.png
[1-1]: ./media/application-gateway-web-application-firewall-portal/figure1-1.png
[2-2]: ./media/application-gateway-web-application-firewall-portal/figure2-2.png
[3]: ./media/application-gateway-web-application-firewall-portal/figure3.png
[4]: ./media/application-gateway-web-application-firewall-portal/figure4.png
[5]: ./media/application-gateway-web-application-firewall-portal/figure5.png
[6]: ./media/application-gateway-web-application-firewall-portal/figure6.png
[7]: ./media/application-gateway-web-application-firewall-portal/figure7.png
[8]: ./media/application-gateway-web-application-firewall-portal/figure8.png
[9]: ./media/application-gateway-web-application-firewall-portal/figure9.png
[10]: ./media/application-gateway-web-application-firewall-portal/figure10.png
[scenario]: ./media/application-gateway-web-application-firewall-portal/scenario.png
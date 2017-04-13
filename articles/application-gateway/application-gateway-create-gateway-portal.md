<properties
   pageTitle="Hozzon létre egy alkalmazás átjáró a portálon |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre olyan átjárót a portál használatával"
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

# <a name="create-an-application-gateway-by-using-the-portal"></a>Az alkalmazás átjáró létrehozása a portál használatával

> [AZURE.SELECTOR]
- [Azure portál](application-gateway-create-gateway-portal.md)
- [Azure erőforrás-kezelő PowerShell](application-gateway-create-gateway-arm.md)
- [Azure klasszikus PowerShell](application-gateway-create-gateway.md)
- [Erőforrás-kezelő Azure-sablon](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure alkalmazás átjáró az egy réteg-7 terheléselosztó. A feladatátvétel HTTP-kérések teljesítmény továbbítása különböző kiszolgálók között, akár a helyszíni vagy felhőalapú biztosít. Alkalmazás átjáró sok teszi alkalmazás kézbesítési vezérlő LÉPETT funkciói, köztük a HTTP terheléselosztás cookie-alapú munkamenet affinitás, Secure Sockets Layer (SSL) kiürítése, egyéni állapot szondákat, több webhelyen támogatása és számos más. Ha a támogatott szolgáltatások listáját, látogasson el az [Alkalmazás átjáró – áttekintés](application-gateway-introduction.md)

## <a name="scenario"></a>Eset

Ebben az esetben megismerheti, hogyan hozhat létre olyan az Azure portálon átjárót.

Ebben az esetben fognak:

- Hozzon létre egy közepes alkalmazás átjáró két példánya.
- Hozzon létre egy virtuális hálózati nevű AdatumAppGatewayVNET 10.0.0.0/16 fenntartott CIDR blokkja a.
- A CIDR sávként 10.0.0.0/28 használó Appgatewaysubnet néven alhálózat létrehozása.
- Kiürítése SSL-tanúsítvány beállítása.

![Forgatókönyv példa][scenario]

>[AZURE.IMPORTANT] Az alkalmazás átjáró, beleértve az egyéni állapot további konfigurációs ellenőrzi, kódmentes készlet címeket és a többi szabályt az alkalmazás átjáró beállítása után, és a kezdeti telepítés során nem konfigurálja.

## <a name="before-you-begin"></a>Első lépések

Azure alkalmazás átjáró saját alhálózat szükséges. Virtuális hálózat létrehozásakor győződjön meg arról, hogy hagyja, hogy több alhálózat elég címterület használatára. Miután-alhálózat átjáró alkalmazás telepítéséhez csak további alkalmazás átjárók képesek adható hozzá a alhálózat.

## <a name="create-the-application-gateway"></a>Az alkalmazás átjáró létrehozása

### <a name="step-1"></a>Lépés: 1

Nyissa meg az Azure portált, kattintson az **Új** > **Networking** > **Alkalmazás átjáró**

![Alkalmazás átjáró létrehozása][1]

### <a name="step-2"></a>Lépés: 2

Ezután adja meg az alkalmazás átjáró alapvető információkat. Amikor elkészült **az OK** gombra.

Az alapvető beállítások szükséges információt a következő:

- **Név** – az alkalmazás átjáró neve.
- **Réteg** – Ez az alkalmazás átjáró a réteg. A fenti két érhetők el, **WAF** és a **normál**. WAF lehetővé teszi, hogy a webes alkalmazás tűzfala.
- Az alkalmazás átjáró mérete **Termékváltozat méret** - ezt a beállítást, rendelkezésre álló beállítások (**kicsi**, **Közepes**és **nagy**). *Kis nem érhető el, ha WAF réteg van kiválasztva*
- **Példányok száma** - példányok száma, ezt az értéket kell 2 és 10 közé eső szám.
- **Erőforráscsoport** – az alkalmazás átjáró tartása az erőforráscsoport Ez lehet egy meglévő erőforráscsoport vagy egy újat.
- **Hely** – az alkalmazás átjáró a régió erőforrás csoport ugyanazon a helyen. *A hely a virtuális hálózatként, és nyilvános IP kell lennie az átjáró ugyanazon a helyen*.

![a lap megjelenítő alapvető beállításai][2]

>[AZURE.NOTE] Az 1-példányok száma tesztelésre lehet kiválasztani. Fontos tudnia, hogy, hogy a két példánya bármely példányok száma nem vonatkozik a SZOLGÁLTATÁSISZINT, és ezért nem ajánlott. Kis átjárók vannak fejlesztők vizsgálathoz, és nem termelési célra használható.

### <a name="step-3"></a>3. lépés

Miután az alapvető beállítások vannak megadva, a következő lépés célja a virtuális hálózat használandó meghatározása. A virtuális hálózat otthont ad az alkalmazást, az alkalmazás átjáró terheléselosztás számára.

Kattintson a **Válasszon egy virtuális hálózat** konfigurálása a virtuális hálózat.

![a lap beállításaival alkalmazás átjáró][3]

### <a name="step-4"></a>Lépés: 4

A *Virtuális hálózat kiválasztása* a lap kattintson az **Új létrehozása**

Ebben az esetben nem magyarázata, miközben egy meglévő virtuális hálózat ezen a ponton sikerült jelölhet ki.  Egy meglévő virtuális hálózat használata esetén fontos tudni, hogy a virtuális hálózat szüksége van egy üres alhálózat vagy csak alkalmazás átjáró erőforrások használandó alhálózat.

![Válassza ki a virtuális hálózati lap][4]

### <a name="step-5"></a>5 lépésben

Töltse ki a hálózati a **Virtuális hálózati létrehozása** lap, a megelőző [forgatókönyv](#scenario) leírás leírt módon.

![Virtuális hálózati lap létrehozása a megadott adatokkal][5]

### <a name="step-6"></a>6 lépés

Amikor a virtuális hálózat létrejött, a következő lépés célja az előtér-IP az alkalmazás átjáró meghatározása. Ezen a ponton a választási lehetőségek egy nyilvános vagy egy privát IP-címet az előtér-között van. A választási lehetőségek, attól függ, hogy az alkalmazás-e internetes szemben lévő vagy belső csak. Ebben az esetben azt feltételezi, hogy egy nyilvános IP-cím használatával. Válassza ki a személyes IP-címet, a **magánjellegű** gomb rákattintva a. Választja ki automatikusan hozzárendelt IP-címet, vagy választhatja a kézzel írja be a **választ egy adott magánjellegű IP-cím** jelölőnégyzet.

### <a name="step-7"></a>Lépés 7

Kattintson a **Kiválasztás egy nyilvános IP-címet**. Ha egy meglévő nyilvános IP-cím érhető el ezen a ponton választott, és ebben az esetben hoz létre egy új nyilvános IP-címet. Kattintson az **Új létrehozása**

![Válassza a nyilvános IP-cím lap][6]

### <a name="step-8"></a>Lépés 8

Ezután írja be a nyilvános IP-cím egy rövid nevet, és kattintson az **OK gombra**

![Nyilvános IP-címét a lap létrehozása][7]

### <a name="step-9"></a>Lépés 9

Az utolsó beállítás esetén az alkalmazás átjáró létrehozása a figyelő konfigurációja konfigurálása.  **Http** használata esetén konfigurálása semmi nem marad, és rákattintva **az OK gombra** . **Https** használatához további konfigurációs szükség.

**Https**használatához tanúsítvány szükség. A tanúsítvány a titkos kulcs van szükség, így a tanúsítvány és a jelszó egy .pfx exportálás kell kell adni.

### <a name="step-10"></a>10 lépés

Kattintson a **HTTPS**, majd a **mappa** ikonja mellett a **Feltöltés PFX tanúsítvány** mezőben lévő értéket.
Nyissa meg a fájlrendszerben .pfx biztonságitanúsítvány-fájl. Ha be van jelölve, a tanúsítvány adjon meg egy rövid nevet, és írja be a jelszót az .pfx fájl.

Befejezése után kattintson az **OK gombra** , tekintse át a beállításokat, az alkalmazás átjáró.

![Figyelő konfigurációs szakasz a beállítások lap][9]

### <a name="step-11"></a>Lépés 11

Tekintse át az Összegzés lapon, és kattintson az **OK gombra**.  Most már az alkalmazás átjáró aszinkron és létrehozott.

### <a name="step-12"></a>Lépésben 12

Az alkalmazás átjáró létrehozása után nyissa meg azt a portálon a konfigurálás az alkalmazás átjáró folytatásához.

![Alkalmazás átjáró Erőforrásnézet][10]

Ezeket a lépéseket a figyelő, kódmentes készlet, kódmentes http beállítások és szabályok alapértelmezett beállításainak egyszerű alkalmazás az átjárók létrehozása. Ezeket a beállításokat, hogy megfeleljen a telepítéssel, amikor elkészült a kiépítési sikeres módosíthatók.

## <a name="next-steps"></a>Következő lépések

Ebben az esetben létrehoz egy alapértelmezett alkalmazás átjárót. A következő lépésekkel az alkalmazás átjáró beállítása készlet tagok hozzáadása, beállítások módosítása és beállítása, hogy megfelelően működjön az átjárót a szabályok.

Megtudhatja, hogy miként hozhat létre egyéni állapot szondákat megtalálhatók, [Hozzon létre egy egyéni állapot vizsgálati](application-gateway-create-probe-portal.md)

Megtudhatja, hogy miként SSL mert konfigurálása és a költséges SSL visszafejtés kikapcsolása a webkiszolgálón megtalálhatók az [SSL kiürítése konfigurálása](application-gateway-ssl-portal.md)

Ismerje meg, hogy az alkalmazások [Webes alkalmazás tűzfal](application-gateway-webapplicationfirewall-overview.md) funkció alkalmazás átjáró védelmét.

<!--Image references-->
[1]: ./media/application-gateway-create-gateway-portal/figure1.png
[2]: ./media/application-gateway-create-gateway-portal/figure2.png
[3]: ./media/application-gateway-create-gateway-portal/figure3.png
[4]: ./media/application-gateway-create-gateway-portal/figure4.png
[5]: ./media/application-gateway-create-gateway-portal/figure5.png
[6]: ./media/application-gateway-create-gateway-portal/figure6.png
[7]: ./media/application-gateway-create-gateway-portal/figure7.png
[8]: ./media/application-gateway-create-gateway-portal/figure8.png
[9]: ./media/application-gateway-create-gateway-portal/figure9.png
[10]: ./media/application-gateway-create-gateway-portal/figure10.png
[scenario]: ./media/application-gateway-create-gateway-portal/scenario.png

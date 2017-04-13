<properties
   pageTitle="Az alkalmazás átjáró segítségével az Azure CLI az erőforrás-kezelő létrehozása |} Microsoft Azure"
   description="Megtudhatja, hogy miként-alkalmazás átjáró létrehozása az Azure CLI az erőforrás-kezelő használatával"
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

# <a name="create-an-application-gateway-by-using-the-azure-cli"></a>Az alkalmazás átjáró létrehozása az Azure CLI használatával

> [AZURE.SELECTOR]
- [Azure portál](application-gateway-create-gateway-portal.md)
- [Azure erőforrás-kezelő PowerShell](application-gateway-create-gateway-arm.md)
- [Azure klasszikus PowerShell](application-gateway-create-gateway.md)
- [Erőforrás-kezelő Azure-sablon](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure alkalmazás az átjáró az egy réteg-7 terheléselosztó. A feladatátvétel HTTP-kérések teljesítmény továbbítása különböző kiszolgálók között, akár a helyszíni vagy felhőalapú biztosít. Alkalmazás átjáró rendelkezik az alábbi alkalmazás kézbesítési szolgáltatások: HTTP terheléselosztás, a cookie-alapú munkamenet affinitás és a kiürítés Secure Sockets Layer (SSL), egyéni állapot szondákat betöltése, és több webhelyen támogatása.

## <a name="prerequisite-install-the-azure-cli"></a>Előfeltétel: Telepítse az Azure CLI

Ez a cikk lépéseinek végrehajtása, telepítenie kell [ezeket az Azure parancssor for Mac, Linux, és a Windows (Azure CLI)](../xplat-cli-install.md) , és [Jelentkezzen be az Azure](../xplat-cli-connect.md)kell. 

> [AZURE.NOTE] Ha nem az Azure-fiók, már rá. A részletezésből bejelentkezési [Itt az ingyenes próbaverziót](../active-directory/sign-up-organization.md).

## <a name="scenario"></a>Eset

Ebben az esetben megismerheti, hogyan hozhat létre olyan az Azure portálon átjárót.

Ebben az esetben fognak:

- Hozzon létre egy közepes alkalmazás átjáró két példánya.
- Hozzon létre egy virtuális hálózati nevű AdatumAppGatewayVNET 10.0.0.0/16 fenntartott CIDR blokkja a.
- A CIDR sávként 10.0.0.0/28 használó Appgatewaysubnet nevű alhálózat létrehozása.
- Kiürítése az SSL-tanúsítvány beállítása.

![Forgatókönyv példa][scenario]

>[AZURE.NOTE] Az alkalmazás átjáró, beleértve az egyéni állapot további konfigurációs ellenőrzi, kódmentes készlet címeket és a többi szabályt az alkalmazás átjáró beállítása után, és a kezdeti telepítés során nem konfigurálja.

## <a name="before-you-begin"></a>Első lépések

Azure alkalmazás átjáró saját alhálózat szükséges. Virtuális hálózat létrehozásakor győződjön meg arról, hogy hagyja, hogy több alhálózat elég címterület használatára. Miután-alhálózat átjáró alkalmazás telepítéséhez csak további alkalmazás átjárók képesek adható hozzá a alhálózat.

## <a name="log-in-to-azure"></a>Bejelentkezés az Azure

Nyissa meg a **Microsoft Azure parancssort**, és jelentkezzen be. 

    azure login

Miután beírta az előző példában, a kód áll rendelkezésre. Nyissa meg a böngészőben, folytassa a bejelentkezési folyamatot https://aka.ms/devicelogin.

![cmd megjelenítő eszköz bejelentkezés][1]

A böngészőben írja be a kapott kódot. Megnyílik a egy bejelentkezési lapra.

![Írja be a kódot a böngészőben][2]

Ha nincs megadva a kód be van jelentkezve a, folytassa az alkalmazási példát a böngésző bezárásával.

![sikeresen bejelentkezve][3]

## <a name="switch-to-resource-manager-mode"></a>Erőforrás-kezelő módba vált

    azure config mode arm

## <a name="create-the-resource-group"></a>Az erőforráscsoport létrehozása

Az alkalmazás átjáró létrehozása, előtt erőforráscsoport, amely tartalmazhat, az alkalmazás átjáró jön létre. Az alábbi példában látható a parancsot.

    azure group create -n AdatumAppGatewayRG -l eastus

## <a name="create-a-virtual-network"></a>Hozzon létre egy virtuális hálózaton

Amikor az erőforráscsoport létrejött, az alkalmazás átjáró virtuális hálózat jön létre.  A következő példában a címterületet volt 10.0.0.0/16 az előző forgatókönyv jegyzetek definiálva.

    azure network vnet create -n AdatumAppGatewayVNET -a 10.0.0.0/16 -g AdatumAppGatewayRG -l eastus

## <a name="create-a-subnet"></a>Alhálózat létrehozása

A virtuális hálózat létrehozását követően alhálózat megjelenik az alkalmazás átjáró.  Ha alkalmazás átjáró használatára, az alkalmazás átjáró virtuális ugyanabba a hálózatba üzemeltetett webalkalmazást, ügyeljen arra, hogy elég hely egy másik alhálózat.

    azure network vnet subnet create -g AdatumAppGatewayRG -n Appgatewaysubnet -v AdatumAppGatewayVNET -a 10.0.0.0/28 

## <a name="create-the-application-gateway"></a>Az alkalmazás átjáró létrehozása

Amikor a virtuális hálózati és alhálózat létrejött, az alkalmazás átjáró a előtti követelmények fejeződtek. Ezenkívül egy korábban exportált .pfx tanúsítvány és a jelszót a tanúsítvány szükség a következő lépés: a kódmentes a használt az IP-címek a kódmentes kiszolgáló IP-címét. Ezeket az értékeket lehet személyes IP-címei a virtuális hálózaton, nyilvános IP-címei vagy teljesen minősített tartománynév a kódmentes kiszolgálókhoz.

    azure network application-gateway create -n AdatumAppGateway -l eastus -g AdatumAppGatewayRG -e AdatumAppGatewayVNET -m Appgatewaysubnet -r 134.170.185.46,134.170.188.221,134.170.185.50 -y c:\AdatumAppGateway\adatumcert.pfx -x P@ssw0rd -z 2 -a Standard_Medium -w Basic -j 443 -f Enabled -o 80 -i http -b https -u Standard

> [AZURE.NOTE] Paraméterek létrehozása a következő parancs futtatása során kapott listáját: **azure hálózati alkalmazás-átjáró létrehozása – súgó**.

Ez a példa egy egyszerű alkalmazás átjáró alapértelmezett beállításai a figyelő, kódmentes készlet, kódmentes http beállítások és szabályok hoz létre. Konfigurálja a SSL kiürítése is. Ezeket a beállításokat, hogy megfeleljen a telepítéssel, amikor elkészült a kiépítési sikeres módosíthatók.
Ha már van a webalkalmazás célokkal definiált a a kódmentes készletébe az előző lépéseket, amikor létrejött, terheléselosztás kezdődik.

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogy miként hozhat létre egyéni állapot szondákat megtalálhatók, [Hozzon létre egy egyéni állapot vizsgálati](application-gateway-create-probe-portal.md)

Megtudhatja, hogy miként SSL mert konfigurálása és a költséges SSL visszafejtés kikapcsolása a webkiszolgálón megtalálhatók az [SSL kiürítése konfigurálása](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png
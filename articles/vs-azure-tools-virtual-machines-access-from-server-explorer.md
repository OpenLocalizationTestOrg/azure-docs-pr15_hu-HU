<properties
   pageTitle="Azure virtuális gépeken futó elérése a kiszolgáló Intézőből |} Microsoft Azure"
   description="Megtudhatja, hogy miként tekinthető létrehozása és kezelése az Azure virtuális gépeken (VMs) futó a kiszolgáló-tallózó a Visual Studio GET."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="accessing-azure-virtual-machines-from-server-explorer"></a>Azure virtuális gépeken futó elérése kiszolgáló Explorerből

Kiszolgáló Intézővel a Visual Studióban, a virtuális gépeken futó Azure által üzemeltetett kapcsolatos információkat is megjeleníthet.

## <a name="accessing-virtual-machines-in-server-explorer"></a>A kiszolgáló Intézőben virtuális gépeken futó elérése

Ha üzemeltetett által Azure virtuális gépeken futó, elérheti őket Server Explorer. Kell első alkalommal jelentkezik Azure-előfizetéséhez a mobil szolgáltatások megjelenítéséhez. Jelentkezzen be, nyissa meg a helyi menü az Azure csomóponthoz a kiszolgáló Intézőben, és válassza a **Csatlakozás Microsoft Azure**.

### <a name="to-get-information-about-your-virtual-machines"></a>A virtuális gépeken futó kapcsolatos tájékozódáshoz

1. A kiszolgáló Intézőben válassza ki a virtuális géphez, és válassza a Tulajdonságok ablak megjelenítése az F4 billentyűt.

    A következő táblázat mutatja, hogy milyen tulajdonságok érhetők el, de az összes csak olvasható. Megváltoztatja őket, használja az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/?LinkID=213885).

  	|A tulajdonság|Leírás|
  	|---|---|
  	|Tartománynév|A virtuális gépen az Internet címével URL-címe.|
  	|Környezet|Virtuális géphez Ez a tulajdonság értéke mindig gyártás.|
  	|név|A virtuális gép neve.|
  	|Mérete|A virtuális számítógépre, amelyen a rendelkezésre álló memória és a lemez térközt tükrözi méretét. További tudnivalókért lásd: Útmutató: konfigurálása virtuális gép méretét.|
  	|Állapot|Értékek: indítása, lépések, leállítása, leállítva és Retrieving állapotát. Megjelenik a Retrieving állapotot, ha az aktuális állapota ismeretlen. Ez a tulajdonság értékeit az értékeket az [Azure klasszikus portál](http://go.microsoft.com/fwlink/?LinkID=213885)használt eltérnek.|
  	|SubscriptionID|Az Azure-fiók előfizetés azonosítója. Az [Azure klasszikus portál](http://go.microsoft.com/fwlink/?LinkID=213885) tulajdonságpaneljén előfizetéshez megjelenítheti ezeket az adatokat.|

1. Válassza a egy végpont csomópontra, és tekintse meg a **Tulajdonságok** ablak.

1. Az alábbi táblázat ismerteti a végpontok elérhető tulajdonságok, de ezek csak olvasható. Hozzáadása vagy szerkesztése a végpontok virtuális géphez, használja az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/?LinkID=213885). 

  	|A tulajdonság|Leírás|
  	|---|---|
  	|név|Az endpoint azonosítója.|
  	|Magánjellegű Port|Az alkalmazás belső hálózati hozzáférés portja.|
  	|Protocol (protokoll)|A Protocol (protokoll) a szállítási réteg a végpontot, amelyet használ TCP vagy UDP.|
  	|Nyilvános Port|A port, amellyel nyilvános hozzáférés az alkalmazás.|

## <a name="next-steps"></a>Következő lépések

Visual Studio Azure szerepkörök használatával kapcsolatos további információért lásd: [A távoli asztal Azure szerepkörök](vs-azure-tools-remote-desktop-roles.md).

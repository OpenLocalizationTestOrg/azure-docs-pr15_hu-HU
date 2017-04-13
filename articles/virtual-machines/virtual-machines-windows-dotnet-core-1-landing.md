<properties
   pageTitle="Azure virtuális gép DotNet Core oktatóanyag 1 |} Microsoft Azure"
   description="Azure virtuális gép DotNet Core oktatóprogram"
   services="virtual-machines-windows"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="nepeters"/>

# <a name="automating-application-deployments-to-azure-virtual-machines"></a>Azure virtuális gépeken futó alkalmazás telepítések automatizálása

E négy részből álló számozási adatait megjelenítő telepítése és Azure erőforrás és Azure erőforrás kezelése sablonokkal alkalmazások konfigurálása. A sorozat űrlapsablonok telepítve van, és a telepítési sablon vizsgálni. A sorozat célja tájékoztassa a Azure erőforrások közötti kapcsolatra, és adja meg a pálcikák élményt teljesen integrált Azure erőforrás-kezelő sablonok telepítése. Ezt a dokumentumot egyszerű szintek létrehozása az Azure erőforrás-kezelő feltételezi, az oktatóprogram elindítása előtt megismerkedhet alapfogalmak Azure erőforrás-kezelő.

## <a name="music-store-application"></a>Zenei áruház alkalmazás

A sorozatban felhasznált minta .net az alkalmazás központi élmény vásárlás zenét üzlet amelyek. Ez az alkalmazás telepítheti Windows vagy Linux virtuális rendszerhez, a minta telepítések hozták létre is. Az alkalmazás a webalkalmazás és az SQL-adatbázis tartalmazza. A sorozat a cikkek olvasása, mielőtt telepítse az alkalmazást a telepítés gombra, ezen az oldalon található. Ha teljesen telepítve van, az alkalmazás / Azure architektúra néz ki a következő diagramon. 

A zene-tár erőforrás-kezelő sablon megtalálható itt, [Zenei áruházból Windows-sablon](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows)

![Zene áruház-alkalmazás](./media/virtual-machines-windows-dotnet-core/music-store.png)

Minden összetevő, beleértve a társítása sablon JSON van megvizsgálni a következő négy cikkekben.

- [**Alkalmazás architektúra**](./virtual-machines-windows-dotnet-core-2-architecture.md) – például a webhelyek és az adatbázisok alkalmazásösszetevők kell szerepeltethetők Azure számítógéptől, például a virtuális gépeken futó és Azure SQL-adatbázisait. A dokumentum végigvezeti hozzárendelést számítási segítségre van szüksége, Azure erőforrásokat, és ezek az erőforrások egy erőforrás-kezelő Azure sablonnal telepítésével. 

- [**A hozzáférési és biztonsági**](./virtual-machines-windows-dotnet-core-3-access-security.md) – amikor szolgáltatója alkalmazások Azure-ban, szükség érdemes hogyan érhető el az alkalmazást, és hogyan másik alkalmazás összetevők elérése egymást. A dokumentum adatai kezeléséről, valamint biztonságossá tétele az alkalmazások interneten keresztüli elérését, és az access alkalmazásösszetevők között.

- [**Elérhetőség és arányának**](./virtual-machines-windows-dotnet-core-4-availability-scale.md) – elérhetősége és arányának utal, és az azt jelenti, hogy a számítási erőforrások alkalmazás igényeknek méretezni futtatása során infrastruktúra legrövidebb leállás kapcsolatának alkalmazások lehetősége. A dokumentum adatai egy terheléselosztása telepítéséhez szükséges összetevőket és könnyen hozzáférhető alkalmazásról.

- [**Alkalmazások telepítésének**](./virtual-machines-windows-dotnet-core-5-app-deployment.md) - alkalmazások alakzatot Azure virtuális gépeken futó telepítésekor a módszert, amellyel az alkalmazás bináris telepítve van a virtuális gép kell figyelembe venni. A dokumentum adatai Azure virtuális gép egyéni parancsfájl Extensions használata automatizálása alkalmazások telepítése.

Az erőforrás-kezelő Azure sablonok fejlesztésekor célja, hogy automatizálható a Azure infrastruktúra, és a telepítési és az éppen is ez az Azure infrastruktúra alkalmazások konfigurálása. Ezek a cikkek keresztül végezhető műveletek e, amikor egy példát tartalmaz.

## <a name="deploy-the-music-store-application"></a>A zenei áruház alkalmazás telepítése

A zene-tár alkalmazás telepíthető, ez a gomb segítségével.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fdotnet-core-sample-templates%2Fmaster%2Fdotnet-core-music-windows%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

Az erőforrás-kezelő Azure-sablon használatához szükséges, a következő paraméterértékeket.

|Paraméterek neve |Leírás   |
|---|---|
|ADMINUSERNAME   | A virtuális gép és az SQL Azure-adatbázis használt rendszergazdai felhasználónevével.  |
|ADMINPASSWORD | Az Azure virtuális gép és SQL-adatbázis használt jelszót.  |
|NUMBEROFINSTANCES | Virtuális gépeken futó létrehozandó száma. Minden egyes e virtuális gépeken futó tárolni a zene-tár webalkalmazás, és minden forgalom betöltés között őket. |
|PUBLICIPADDRESSDNSNAME | Globálisan egyedi DNS-neve a nyilvános IP-cím társítva. |

A sablon telepítés befejeződött, nyissa meg bármelyik internetböngésző használata nyilvános IP-címét. A .net Core zene webhely be kell mutatni.

## <a name="next-steps"></a>Következő lépések

<hr>

[Lépés: 1 – az alkalmazás architektúra Azure erőforrás-kezelő sablonokkal](./virtual-machines-windows-dotnet-core-2-architecture.md)

[Lépés: 2 - elérhetőségének és biztonságának Azure erőforrás-kezelő sablonok](./virtual-machines-windows-dotnet-core-3-access-security.md)

[3 – elérhetőségéről és az Azure erőforrás-kezelő sablonok skála lépés](./virtual-machines-windows-dotnet-core-4-availability-scale.md)

[Lépés: 4 – az alkalmazás környezet, amelyben az Azure erőforrás-kezelő sablonok](./virtual-machines-windows-dotnet-core-5-app-deployment.md)



<properties
   pageTitle="Frissítés alkalmazása: Speciális témakörök |} Microsoft Azure"
   description="Ez a cikk bemutatja a szolgáltatás háló-alkalmazások frissítés kapcsolatos speciális súgótémakörökben."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>

# <a name="service-fabric-application-upgrade-advanced-topics"></a>Szolgáltatás háló alkalmazás frissítése: Speciális témakörök

## <a name="adding-or-removing-services-during-an-application-upgrade"></a>Hozzáadásáról és eltávolításáról a szolgáltatások egy alkalmazása a frissítés során

Ha egy új szolgáltatás hozzáadása az alkalmazás már telepítve, és közzétett frissítésként bekerül az új szolgáltatás a telepített alkalmazások.  Az ilyen frissítésének nem befolyásolja a már az alkalmazás részét képező szolgáltatások közül. Azonban egy példány hozzá lett adva szolgáltatás el kell indítani az új szolgáltatás aktívnak kell lennie (használata a `New-ServiceFabricService` parancsmag).

Szolgáltatások is eltávolítható az alkalmazások frissítés részeként. Azonban a to-be törölt szolgáltatás jelenlegi előfordulását le kell állítani a frissítés előtt (használata a `Remove-ServiceFabricService` parancsmag). 

## <a name="manual-upgrade-mode"></a>Manuális frissítés mód

> [AZURE.NOTE]  Csak az a sikertelen vagy felfüggesztett frissítése figyelembe kell venni a figyelt manuális mód. A felügyelt módja az ajánlott frissítési mód szolgáltatás háló alkalmazásokhoz.

Azure Service háló biztosít a fejlesztés és a gyártási fürt támogatási több frissítési módban. Telepítési lehetőségek választott különböző környezetekben eltérőek lehetnek.

A felügyelt közbeni alkalmazás frissítés a leggyakoribb frissítés gyártási használni. Ha meg van adva a frissítési házirendet, szolgáltatás háló biztosítja, hogy az alkalmazás megfelelő előtt a frissítés során.

 Az alkalmazás rendszergazdájának a manuális közbeni alkalmazás frissítési mód használatával a frissítési folyamat teljes hozzáféréssel rendelkeznek a különböző frissítési tartományok keresztül. Ebben az üzemmódban akkor hasznos, amikor egy testre szabott vagy összetett állapot kiértékelés házirend szükség, és egy nem hagyományos frissítés történik (például az alkalmazás már szerepel az adatok elvesztését).

Végül a működés közben alkalmazás automatikus frissítés akkor hasznos, fejlesztési vagy szolgáltatás fejlesztése során verziókövetéssel egy gyors közelítését megadására tesztelés környezetekben.

## <a name="change-to-manual-upgrade-mode"></a>Kézi frissítés üzemmódjára való
**Kézi**– az alkalmazás frissítése az aktuális UD a Leállítás, és figyelt kézi frissítés módjának megváltoztatása. A rendszergazdának **MoveNextApplicationUpgradeDomainAsync** folytassa a verziófrissítéssel vagy egy visszaállítása elindítása által kezdeményezése egy új frissítése kézzel felhívására. Ha a frissítés a kézi üzemmódba lép, marad a kézi módban mindaddig, amíg az új frissítést kezdeményezni. A **GetApplicationUpgradeProgressAsync** parancs eredménye HÁLÓ\_alkalmazás\_frissítése\_állam\_működés közbeni\_előre\_váró.

## <a name="upgrade-with-a-diff-package"></a>Az élőláb csomag frissítése

A szolgáltatás háló-alkalmazások teljes, önállóan alkalmazáscsomag a kiépítési frissíthető. Az alkalmazás csak a frissített alkalmazás fájlokat, a frissített alkalmazás jegyzék és szolgáltatás jegyzékfájlok tartalmazó összehasonlítása csomagot használatával is lesznek frissítve.

Egy teljes alkalmazáscsomag indításához és a szolgáltatás háló alkalmazás futtatásához szükséges összes fájlt tartalmazza. Élőláb csomagban csak azokat a fájlokat az utolsó rendelkezés és a jelenlegi frissítés között megváltozott, valamint a teljes alkalmazás jegyzék és a szolgáltatás cikkét fájlokat. Hivatkozás a jegyzék alkalmazás vagy szolgáltatás jegyzék, amely nem található az összeállítás elrendezésben a kép tároló kell keresni.

Teljes alkalmazáscsomagok szükség a fürthöz az alkalmazás első telepítését. Későbbi frissítések lehet egy teljes alkalmazáscsomag vagy élőláb csomagot.

Alkalommal, amikor egy összehasonlítása csomag használata lenne kinek ajánljuk:

* Összehasonlítása csomag ajánlott, amikor egy nagy alkalmazáscsomag több szolgáltatás jegyzékfájlok és/vagy több kódot csomagokat, config csomagok vagy adatok csomagok hivatkozó.

* Élőláb csomag által generált az összeállítás elrendezés közvetlenül az alkalmazás build folyamat telepítési rendszer esetén előnyben részesített. Ebben az esetben annak ellenére, hogy a kód nem változott, újonnan beépített összeállítások különböző ellenőrző kaphat. Egy teljes alkalmazáscsomag használatával szeretné csak frissítés összes kód csomag verziója. Összehasonlítása csomagot használ, akkor csak adja meg a fájlokat, amelyek megváltozott és a jegyzékfájlok, ahol a verzió megváltozott.

Amikor az alkalmazás frissítését a Visual Studio segítségével, az összehasonlítás csomag automatikusan közzé. Élőláb csomag létrehozása manuálisan, az alkalmazás jegyzék és a szolgáltatás jegyzékfájlok frissíteni kell, de csak a módosított csomagok végleges alkalmazáscsomag kell foglalni. 

Ha például kezdjük a következő alkalmazást (verziószámai ismertetése Kezeléstechnikai megadva):

```text
app1        1.0.0
  service1  1.0.0
    code    1.0.0
    config  1.0.0
  service2  1.0.0
    code    1.0.0
    config  1.0.0
```

Most vásárolja meg szeretett volna, ha csak a kód csomag frissítéséhez service1 a PowerShell használatával összehasonlítása csomagot használ. Ezután a frissített alkalmazás a következő mappaszerkezet foglalja magában:

```text
app1        2.0.0      <-- new version
  service1  2.0.0      <-- new version
    code    2.0.0      <-- new version
    config  1.0.0
  service2  1.0.0
    code    1.0.0
    config  1.0.0
```

Az alkalmazás jegyzék ebben az esetben 2.0.0, majd a szolgáltatás jegyzék-a kódot csomag frissítés megfelelően service1 frissítése. A mappa a alkalmazáscsomag szeretné, hogy az alábbi szerkezet:

```text
app1/
  service1/
    code/
```

## <a name="next-steps"></a>Következő lépések

[Az alkalmazás használatáról a Visual Studio frissítése](service-fabric-application-upgrade-tutorial.md) végigvezeti az alkalmazás frissítésre Visual Studio segítségével.

[Az alkalmazás Powershell használatá frissítés](service-fabric-application-upgrade-tutorial-powershell.md) végigvezeti az alkalmazás frissítésre PowerShell használatával.

Szabályozhatja, hogy hogyan az alkalmazás [Frissítése](service-fabric-application-upgrade-parameters.md)paramétereket frissíti.

Úgy, hogy az alkalmazás frissítések kompatibilis tanulási [Adatszerializáció](service-fabric-application-upgrade-data-serialization.md)használatáról.

Gyakori problémák megoldása az alkalmazás frissítések hivatkozva [Alkalmazás frissítése – hibaelhárítás](service-fabric-application-upgrade-troubleshooting.md)című témakör lépéseit.
 

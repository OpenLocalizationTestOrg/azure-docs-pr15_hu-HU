<properties 
   pageTitle="Támogatási lehetőségek és elavulása házirend útmutató az Azure Vendég OS |} Microsoft Azure" 
   description="Mi a Microsoft támogatást tekintetében a Cloud Services által használt Azure vendég operációs rendszer információt tartalmaz." 
   services="cloud-services" 
   documentationCenter="na" 
   authors="raiye" 
   manager="timlt" 
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd" 
   ms.date="10/24/2016"
   ms.author="raiye"/>

# <a name="azure-guest-os-supportability-and-retirement-policy"></a>A vendégként való bekapcsolódáshoz OS Azure támogatási lehetőségek és elavulása házirend
Ezen a lapon az információ az Azure vendég operációs rendszer ([a vendégként való bekapcsolódáshoz-OS](cloud-services-guestos-update-matrix.md)) dolgozó Felhőszolgáltatások és a webes szerepkörök (PaaS) vonatkozik. Nem vonatkozik a virtuális gépeken futó (IaaS). 

A Microsoft tartalmaz egy közzétett [támogatja a vendégként való bekapcsolódáshoz OS házirend](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq). Az oldal olvasása most ismerteti, hogyan a házirendjét.

A házirend 

1. A Microsoft támogatást **legalább a legújabb két családok a vendégként való bekapcsolódáshoz OS**. Egy család van már inaktív, a felhasználóknak kell 12 hónapon hivatalos elavulása egy újabb támogatott vendég-OS operációs rendszere frissítéséhez.
2. Microsoft támogatni fogja a **legalább két legújabb verzió a támogatott vendég OS családok**. 
3. Microsoft támogatni fogja a **legalább két a legfrissebb verziója az Azure SDK**elemre. Amikor a SDK verziójának van már inaktív, ügyfelek lesz 12 hónapon hivatalos elavulása újabb verziójára szeretné frissíteni. 

Időnként kettőnél több családok vagy verziókban előfordulhat, hogy támogat. Hivatalos Vendég OS támogatási információt az [Azure Vendég OS kiadásairól és SDK kompatibilitási mátrix](cloud-services-guestos-update-matrix.md)fog megjelenni.


## <a name="when-a-guest-os-family-or-version-is-retired"></a>Ha a vendégként való bekapcsolódáshoz OS család vagy a verziója már inaktív 


A Windows Server operációs rendszer hivatalos új verziójának megjelenése után valamikor bevezetett új Vendég OS **család** . Amikor egy új Vendég OS tartozó bevezetett, a Microsoft fog kivonni a legrégebbi vendég operációs rendszer termékcsaládon kívüli. 

Új Vendég-OS **verzió** kapcsolatban, amelyekkel beépíthetők a legújabb Kialakításában havonta ismerkedhet meg. A normál havi frissítéseket, mert egy vendég operációs rendszer verziója saját kiadása után a szokásos módon letiltott 60 napig. Ez a továbbra is legalább két Vendég-OS verzió az egyes család számára használható. 

### <a name="process-during-a-guest-os-family-retirement"></a>A folyamat során a vendégként való bekapcsolódáshoz OS családi elavulása 


A elavulása telepítése után, ügyfelek után a régebbi család hivatalos szolgáltatásból eltávolítása előtt 12 hónapos "áttűnés" pontra. Áttűnés ezúttal meghosszabbítható Microsoft tetszése szerint. Frissítések majd a program az [Azure Vendég OS kiadásairól és SDK kompatibilitási mátrix](cloud-services-guestos-update-matrix.md).

Egy fokozatos elavulása folyamat be az áttűnés időszak 6 hónap elindul. A megadott időszakban:

1. A Microsoft arról értesíti a elavulása felhasználókat. 
2. Az Azure SDK újabb verziója nem támogatja a visszavont vendég operációs rendszer termékcsaládon kívüli.
3. Új telepítések és a Felhőszolgáltatásba redeployments nem használható a visszavont termékcsaládban

A Microsoft átmenet időtartamának, a "lejárati dátum" néven utolsó napjáig Kialakításában naprakész tartalmazó új Vendég-OS verzió bevezetésére továbbra is. Adott időpontban fut bármely Cloud Services fog kell nem támogatott, az Azure SLA csoportban. A Microsoft rendelkezik, kötelező frissítés, törlés, és azokat a szolgáltatásokat leállíthatja a dátum után tetszése szerint.



### <a name="process-during-a-guest-os-version-retirement"></a>A folyamat során a vendégként való bekapcsolódáshoz operációs rendszer verziója elavulása 
Ha ügyfelek állít be automatikusan frissíti a vendégként való bekapcsolódáshoz-OS, azok soha nem kell aggódnia amiatt Vendég-OS verziójával foglalkozó. Majd mindig a legújabb Vendég OS használ.

A vendégként való bekapcsolódáshoz-OS verzió kiadott minden hónapban. Normál kiadások mértékét miatt minden verzió rendelkezik egy rögzített élettartam.

60 napig be az élettartam egy verziója "*Letiltva*". "Letiltva" azt jelenti, hogy a verzió az Azure klasszikus portálról törlődik. Azt is már nem beállítható, hogy a CSCFG konfigurációs fájlból. Meglévő példányok futtatása marad, de új telepítések és a meglévő telepítések kódot és konfigurációs frissítéseket nem használható. 

Később a termék, a vendégként való bekapcsolódáshoz-OS verzió "*jár le*" és az összes telepítés továbbra is, hogy verzióját, hogy frissített kötelező és automatikusan frissíti a jövőben a vendégként való bekapcsolódáshoz-OS értékre van állítva. Lejárat kötegekben történik, és a lejárati idő megfelelő időtartama változhat. 

Ezeket az időszakokat tehető hosszabb a Microsoft megítélése szerint ügyfél áttűnések megkönnyítése érdekében. A [vendégként való bekapcsolódáshoz-OS kiadásairól Azure és SDK kompatibilitási mátrix](cloud-services-guestos-update-matrix.md)tájékoztatni kell el a módosításokat.



### <a name="notifications-during-retirement"></a>Értesítések elavulása során 

* **Családi elavulása** <br>Microsoft blogbejegyzéseket és Azure klasszikus portál értesítési fogja használni. Azon ügyfelek, akik még használ egy visszavont Vendég OS tartozó keresztül közvetlen kapcsolatba lépni a (e-mailek, a portál üzenetek, a telefonhívás.) hozzárendelt szolgáltatás-rendszergazdák értesítést kap. A minden módosítás fog közzétett, erre a lapra, és az RSS-hírcsatorna szerepel az ezen az oldalon elején. 


* **Verzió elavulása** <br>A minden módosítás fog közzétett, erre a lapra, és az RSS-hírcsatorna szerepel az ezen a lapon, beleértve a Megjelenés tiltható le, és a lejárati dátum elején. Szolgáltatás-rendszergazdák értesítést kap e-mailben, ha a család vagy letiltott vendég operációs rendszer verziója fut telepítések rendelkeznek. Az e-maileket időzítésének változhat. Ezek általában legalább egy hónap előtt megfelelő, bár ez időzítés nem hivatalos szolgáltatásiszint-szerződés. 


## <a name="frequently-asked-questions"></a>Gyakori kérdések

**Hogyan lehet a áttelepítési hátrányos következményeit csökkentésében?**

A felhőalapú szolgáltatások tervezése legújabb Vendég OS család kell használni. 

1. Tervezze meg az áttelepítési korán egy újabb operációs rendszere. 
2. Állítsa be ideiglenes próba-telepítések tesztelje a kattintson az új család futó felhőalapú szolgáltatáshoz. 
3. A vendégként való bekapcsolódáshoz-OS verzió beállítása **automatikusra** (osVersion = * a [.cscfg](cloud-services-model-and-package.md#cscfg) fájlban), az áttelepítés új Vendég-OS verziójára automatikusan megtörténik.

**Mi történik, ha a saját webalkalmazás mélyebb integrációja az operációs rendszer van szükség?**

Ha a webes alkalmazás architektúra mélyebb függőség az alapul szolgáló operációs rendszerre van szükség, a használati platform támogatott lehetőségek, például az [indítási feladatok](cloud-services-startup-tasks.md) vagy más bővítési mechanizmusok, amelyek lehetséges, hogy a jövőben létezik. Másik lehetőségként [Azure virtuális gépeken futó](https://azure.microsoft.com/documentation/scenarios/virtual-machines/) (IaaS – szolgáltatásként infrastruktúra), hogy a mögöttes operációs rendszer fenntartása felelős is használhatja.
 
## <a name="next-steps"></a>Következő lépések
Tekintse át a [vendégként való bekapcsolódáshoz operációs rendszer által kiadott](cloud-services-guestos-update-matrix.md)legújabb.

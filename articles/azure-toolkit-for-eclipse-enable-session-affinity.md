<properties
    pageTitle="Az Azure eszközkészlet használata Holdas munkamenet affinitás engedélyezése"
    description="Megtudhatja, hogyan engedélyezhető az Azure eszközkészlet használata Holdas munkamenet affinitás."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->

# <a name="enable-session-affinity"></a>Munkamenet affinitás engedélyezése #

Belül a Holdas Azure eszközkészletet lehetősége van engedélyezni a HTTP-munkamenet affinitás vagy a "öntapadó munkamenetek", a szerepkörök. Az alábbi képen látható, a **Terheléselosztás** tulajdonságai párbeszédpanel segítségével munkamenet affinitás szolgáltatás engedélyezése:

![][ic719492]

## <a name="to-enable-session-affinity-for-your-role"></a>Ahhoz, hogy a szerepkör a munkamenet affinitás ##

1. Kattintson a jobb gombbal a szerepkör a Holdas a Project Explorer **Azure**kattintson, és válassza a **Terheléselosztás**.
1. Az **WorkerRole1 terheléselosztás tulajdonságai** párbeszédpanelen:
    1. Jelölje be **engedélyezése HTTP munkamenet affinitás (öntapadó munkamenetek) a szerepkör.**
    1. **Beviteli végpontot kell használni**jelölje ki a bemeneti zárólap szeretne használni, például **http (nyilvános: 80, személyes: 8080)**. Az alkalmazás a végpont kell használnia, mint a HTTP-végpontot. Több végpontok engedélyezheti a szerepkör, de csak az egyik a Beragadó munkamenetek támogatási kijelölhet.
    1. Az alkalmazás újjáépítése.

Miután engedélyezte, ha egynél több szerepkör-példányt, egy adott ügyfél-ból érkező HTTP-kérések továbbra is a példányt szerepkör által kezelt.

A Holdas eszközkészlet lehetővé teszi, hogy ez egy speciális IIS modul alkalmazás kérése Routing (ARR) nevű mindegyikbe a szerepkör-példányok telepítésével. ARR reroutes HTTP-kérelmek a megfelelő szerepkört példányt. Az eszközkészlet automatikusan átállítja a kijelölt végpontot, úgy, hogy a ARR szoftver első továbbítás a bejövő HTTP-forgalmat. Az eszközkészlet is létrehoz egy új belső végpontot, amely a Java-kiszolgálót meghallgatásához. Ez a megfelelő szerepkört példány a HTTP-forgalom átirányítása ARR által használt végpontot. Ezzel a módszerrel a több elem példány üzembe szerepkör-példányban fordított proxy, minden egyéb esetben a Beragadó munkamenetek engedélyezése szolgál.

## <a name="notes-about-session-affinity"></a>Munkamenet affinitás kapcsolatos megjegyzések ##

* Munkamenet affinitás nem működik a számítási irányító. A beállítások is alkalmazható a számítási irányító az összeállítás folyamat megzavarása nélkül, illetve irányító végrehajtás számítja ki, de magát a funkció nem működik a számítási irányító belül.
* Munkamenet affinitás vett az Azure, a telepítés külön szoftver fog letölti és telepíti a szerepkör-példányok be, a szolgáltatás a Azure felhőben való indításakor lemezterületet összegének növekedését eredményezi.
* Az idő minden szerepkör inicializálni több időt vesz igénybe.
* A forgalom rerouter fent említett működhet belső zárólap added.ss lesz.

Példa a munkamenet-adatok karbantartása, ha engedélyezve van a munkamenet affinitás témakö [munkamenet adatok kezelése munkamenetet affinitás][].

## <a name="see-also"></a>Lásd még: ##

[Azure eszközkészlete Holdas][]

[A Holdas Azure egy helló világ alkalmazás létrehozása][]

[Az Azure eszközkészlet Holdas telepítése][] 

[Hogyan kezelése munkamenetet affinitás munkamenet adatok][]

Java Azure használatával kapcsolatos további tudnivalókért olvassa el a az [Azure Java Developer Center][]című témakört.

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure eszközkészlete Holdas]: http://go.microsoft.com/fwlink/?LinkID=699529
[A Holdas Azure egy helló világ alkalmazás létrehozása]: http://go.microsoft.com/fwlink/?LinkID=699533
[Hogyan kezelése munkamenetet affinitás munkamenet adatok]: http://go.microsoft.com/fwlink/?LinkID=699539
[Az Azure eszközkészlet Holdas telepítése]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png

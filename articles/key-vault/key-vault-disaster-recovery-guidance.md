<properties
    pageTitle="Mi a teendő az Azure megelőzve a szolgáltatási zavarok, amely hatással van az Azure kulcs tárolóból elemre |} Microsoft Azure"
    description="Ismerje meg, mi a teendő megelőzve az Azure szolgáltatási zavarok, amely hatással van az Azure kulcs tárolóból elemre."
    services="key-vault"
    documentationCenter=""
    authors="adamglick"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="key-vault"
    ms.workload="key-vault"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="sumedhb;aglick"/>


# <a name="azure-key-vault-availability-and-redundancy"></a>Azure elérhetősége kulcs tárolóból elemre, és a redundancia

Azure kulcs tárolóból elemre kattintva győződjön meg arról, hogy a billentyűk és titkos kulcsok használható marad az alkalmazás még akkor is, ha a szolgáltatás egyes alkatrészek sikertelen redundancia rétegeinek szolgáltatásait.

A fő tárolóra tartalmának replikált belül a régió és egy másodlagos régió legalább 150 mérföld nem vagyok a gépnél, de az azonos földrajzi belül. Ez a magas tartóssági kulcsok és titkos kulcsok kezeli.

Ha nem sikerül a fő tárolóra szolgáltatás belül az egyes összetevők, a régión belüli alternatív összetevők lépés a kérését győződjön meg arról, hogy van-e a használható funkciók körét nem csökkenés szolgáló. Nem kell tennie semmit szeretne elindítani ezt. Automatikusan megtörténik, és a átlátszóak Önnek lesznek.

A ritka esemény, hogy egy teljes Azure terület nem érhető el a kérések az Azure kulcs tárolóból elemre az adott régióban megkönnyítő is automatikusan útválasztásos (*keresztül nem sikerült*) másodlagos területére. Ha ismét érhető el az elsődleges régió, összehívások továbbítása vissza (*nem sikerült vissza*) gombra az elsődleges régió. Újra akkor nem kell tennie semmit, mivel ez automatikusan történik.

Létezik néhány telepítés által okozott problémák tudatában kell lennie:

* Régió feladatátvételnél eltarthat néhány percig, amíg átadni a szolgáltatást. A megadott időszakban létrehozott kérelmek meghiúsulhat, amíg befejeződik a feladatátvételt.
* Feladatátvevő befejeződése után csak olvasható módban van a fő tárolóból elemre. Ebben az üzemmódban támogatott kérelmek a következők:
 * Fő tárolókban lista
 * Ismerkedés a fő tárolókban tulajdonságai
 * Lista titkos kulcsok
 * Ismerkedés a titkos kulcsok
 * Lista kulcsok
 * Get (tulajdonságai) kulcsok
 * Titkosítása
 * Visszafejtés
 * Sortörés
 * Tördelésének
 * Ellenőrzése
 * Bejelentkezés
 * Biztonsági másolat
* Miután feladatátvevő vissza nem sikerült, kérelem diagramtípusokat (olvasási *és* írási kérelem is beleértve) érhetők el.

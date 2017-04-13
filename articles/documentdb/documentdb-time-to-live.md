<properties
   pageTitle="Élettartam DocumentDB az adatok lejár |} Microsoft Azure"
   description="A TTL (élettartam) a Microsoft Azure DocumentDB lehetővé teszi a dokumentumok egy idő után a rendszer automatikusan törölni kell."
   services="documentdb"
   documentationCenter=""
   keywords="élettartam"
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/28/2016"
   ms.author="kipandya"/>

# <a name="expire-data-in-documentdb-collections-automatically-with-time-to-live"></a>Adatok automatikus az élettartam DocumentDB tartozó járjon le

Alkalmazások is konzerv és tárolni a nagy mennyiségű adatot. Ezeket az adatokat egy része, például generált esemény adatok, a naplókat és a felhasználó gépekhez információk csak akkor hasznos korlátozott ideig. Miután az adatok válik az alkalmazás feladatok igényeire felesleges is biztonságos véglegesen ezeket az adatokat, és az alkalmazások tároló igényeinek csökkentésére.

Tartalmazó "élő idő" vagy a TTL (élettartam) a Microsoft Azure DocumentDB lehetővé teszi a dokumentumok egy idő után automatikusan az adatbázisból törölni kell. Az alapértelmezett élettartama beállítása a webhelycsoport szintjén, és egy dokumentum alapon felül. Amikor megadja TTL (élettartam), a webhelycsoport alapértelmezett, vagy a dokumentum szinten DocumentDB automatikusan eltávolítja a dokumentumok, amelyek adott idő, másodpercben után szerepelnek, mivel az utolsó módosítás dátuma.

A DocumentDB élettartam használja a ellen eltolás, amikor a dokumentum utolsó módosítás dátuma. Ehhez a _ts mezőt, amely létezik a minden dokumentum használ. A _ts mező értéke a unix stílusú alapidőpont időbélyeg, amely a dátum és idő. A _ts mező frissítése minden alkalommal, amikor a dokumentum módosul. 

## <a name="ttl-behavior"></a>TTL (élettartam) viselkedése

A TTL (élettartam) szolgáltatás TTL (élettartam) tulajdonságok két szinten – a webhelycsoport és a dokumentum szintjének szabályozza. Az értékeket a másodpercben megadott vannak beállítva, és az, hogy a dokumentum utolsó módosítás dátuma, a _ts egy delta számít.

 1.  A gyűjtemény DefaultTTL
  * Ha hiányzó (vagy a null értékre van állítva), dokumentumok nem automatikusan törlődnek.
  
  * Ha a jelenlegi és az érték-"1" = végtelen – dokumentumok ne járjon le, alapértelmezés szerint
  
  * Ha a jelenlegi és az érték néhány szám ("m") – dokumentumok jár le "n" másodperccel legutóbbi módosítás

 2.  A TTL (élettartam) a dokumentumok: 
  * Tulajdonsága alkalmazható, ha a DefaultTTL megtalálható a szülő vonatkozóan.
  
  * Felülírja a szülő-gyűjtemény DefaultTTL értékét.

Amint a dokumentum lejárt (TTL (élettartam) + _ts > = aktuális kiszolgáló idő), a dokumentum felirattal, mint "lejárt". Nincs művelet ezt követően ezeket a dokumentumokat a lesz jogosult, és azok bármelyik elvégzett lekérdezés eredményének fognak szerepelni. A dokumentumok fizikailag törli a rendszer, és törli a háttérben szerint később. Ez nem mobilalkalmazásokban bármely [Kérése egységek (RUs)](documentdb-request-units.md) a gyűjtemény költségvetés

A fenti logika a következő mátrixban jeleníthető meg:

|       | DefaultTTL hiányzó vagy nem a webhelycsoport beállítása | DefaultTTL = -1, a webhelycsoport | DefaultTTL = "m", a webhelycsoport|
| ------------- |:-------------|:-------------|:-------------|
| A dokumentum hiányzik a TTL (élettartam)| Semmi sem, mivel a dokumentum és a webhelycsoport amelyek a TTL (élettartam) nem ismerik a dokumentum szinten felülbírálására. | Ebben a gyűjteményben dokumentum lejár. | Ebben a gyűjteményben a dokumentumok lejár, amikor a program intervallum n. |
| TTL (élettartam) = -1, a dokumentumhoz | A dokumentum szintjén felülbírálása, mivel a webhelycsoport nem meghatározása a dokumentum felülíró DefaultTTL tulajdonság nincs. TTL (élettartam) a dokumentum a rendszer nem parancsértelmezős. | Ebben a gyűjteményben dokumentum lejár. | A TTL (élettartam) =-1 ebben a gyűjteményben dokumentumot ne járjon le. Minden más dokumentumok "m" intervallum után lejár. |
|  TTL (élettartam) = n a dokumentumhoz | Semmi sem a dokumentum szintjén felülbírálása. TTL (élettartam) a dokumentum nem parancsértelmezős, a rendszer. | A TTL (élettartam) dokumentumot = n lejár intervallum n, másodperc után. Más dokumentumok öröklik a -1 időtartományt, és a sohase járjon le. | A TTL (élettartam) dokumentumot = n lejár intervallum n, másodperc után. Más dokumentumok öröklik az "n" intervallum a gyűjteményből. |


## <a name="configuring-ttl"></a>TTL (élettartam) beállítása

Alapértelmezés szerint élettartam minden DocumentDB gyűjtemény, majd a minden dokumentum alapértelmezés szerint le van tiltva.

## <a name="enabling-ttl"></a>TTL (élettartam) engedélyezése

A webhelycsoport vagy a dokumentuma egy webhelycsoport a TTL (élettartam) engedélyezéséhez kell beállítania a DefaultTTL tulajdonság, egy webhelycsoport -1 vagy a nullától pozitív szám. Beállítás az DefaultTTL-1 azt jelenti, amely a webhelycsoport összes dokumentumok örökkévalóság fog live alapértelmezett, de a DocumentDB szolgáltatás kell figyelni a ebben a gyűjteményben, ki kell felül az alapértelmezett értéket a dokumentumokat.

## <a name="configuring-default-ttl-on-a-collection"></a>A webhelycsoport alapértelmezett TTL (élettartam) beállítása

Képesek konfigurálása a webhelycsoport szintjén élő alapértelmezett időpontot. 

A TTL (élettartam) beállítása a webhelycsoport, nullától pozitív szám, amely jelzi az időszak másodpercben lejár a gyűjteményben az utolsó módosítás időbélyeg után a dokumentum (_ts) minden dokumentum van szükség.

Másik lehetőségként a -1, ami azt jelenti, hogy a gyűjteménybe beszúrt összes dokumentumot határozatlan ideig fog live alapértelmezés szerint is beállíthatja az alapértelmezett.

## <a name="setting-ttl-on-a-document"></a>A dokumentumokon a TTL (élettartam) beállítás

A dokumentum szinten kívül egy alapértelmezett TTL (élettartam) a gyűjtemény beállíthatja adott TTL (élettartam). Ezzel felülírja a webhelycsoport alapértelmezett.

A TTL (élettartam) beállítása a dokumentumon, meg kell adnia a nullától pozitív szám, amely jelzi az időszak másodperc után az utolsó módosítás időbélyeg a dokumentum (_ts) a dokumentum elévül.

A lejárati eltolásának beállítása, kapcsolja be a TTL (élettartam) mező a dokumentumot.

Ha a dokumentum a TTL (élettartam) mező nem tartalmaz, a webhelycsoport alapértelmezett lesz érvényes.

Ha a webhelycsoport szintjén a TTL (élettartam) le van tiltva, a TTL (élettartam) mezőben kattintson a dokumentum figyelmen kívül hagyja mindaddig, amíg a TTL (élettartam) engedélyezve van-e ismét a gyűjteményben.


## <a name="extending-ttl-on-an-existing-document"></a>Létező fájlon bővítése a TTL (élettartam)

A dokumentum bármely írási művelet végrehajtásával alaphelyzetbe állíthatja a TTL (élettartam), a dokumentumon. Ezzel a _ts állítja a pontos időt, és a dokumentum lejárat, a TTL (élettartam) által beállított, a tevékenységig hátralévő időt újra meg is kezdi.

A TTL (élettartam) a dokumentum módosítani szeretné, ha a mező frissítheti, mint bármely más állítható mezőre végezhető.


## <a name="removing-ttl-from-a-document"></a>TTL (élettartam) eltávolítása a dokumentumokból

A TTL (élettartam) van beállítva egy dokumentumot, és már nem szeretné, hogy a dokumentum lejár, ha ezután beolvasni a dokumentumot, a TTL (élettartam) mező eltávolítása és cseréje a dokumentumot a kiszolgálón.

A TTL (élettartam) mező a dokumentumban való eltávolításakor a webhelycsoport alapértelmezett fog vonatkozni.

Állítsa le a dokumentumok hamarosan lejár, és nem öröklik a webhelycsoport majd kell beállítania a TTL (élettartam) értéket a -1.


## <a name="disabling-ttl"></a>TTL (élettartam) letiltása

Tiltsa le a TTL (élettartam) teljes egészében egy webhelycsoport és a lejárt dokumentumok keresése a webhelycsoport DefaultTTL tulajdonsága háttér megszakításához is törli.

Ez a tulajdonság törlése különbözik beállítása a -1. -1-eszközök a webhelycsoport felvett új dokumentumot beállítás örökkévalóság fog live, de ez felülírható a gyűjteményben adott dokumentumok.

Ez a tulajdonság teljesen eltávolítása a webhelycsoport azt jelenti, hogy a dokumentum lejár, még akkor is, ha vannak olyan dokumentumok, amelyek explicit módon van felül a korábbi alapértelmezett.


## <a name="faq"></a>GYAKORI KÉRDÉSEK

**Mit fog kerülni TTL (élettartam)?**

Nem szeretné, hogy a TTL (élettartam) beállítása a dokumentum külön költség nélkül.

**Mennyi ideig tart a dokumentum törlése, amikor elkészült a TTL (élettartam) be?**

A dokumentumok nem érhető el vannak megjelölve, amint a dokumentum lejárt (TTL (élettartam) + _ts > = aktuális kiszolgáló idő). Nincs művelet ezt követően ezeket a dokumentumokat a lesz jogosult, és fognak szerepelni minden elvégzett lekérdezés eredményéből. A dokumentumok fizikailag törli a háttérben a rendszer. Ez nem felhasználja bármely RUs a webhelycsoport költségvetés.

**Ha törli a dokumentumok időbeli vesz igénybe, azok számít felé a kvóta (és a számlázási) első törlésig?**

Nem, a dokumentum lejárt, miután nem számlázott tárolására szolgáló ezeket a dokumentumokat, és a dokumentumok méretének nem a webhelycsoport tárhelykvótájának felé érvényesülnek.

**TTL (élettartam) a dokumentum hatást bármely Licencelési költségek?**

Nem, lesznek nincs hatással a Licencelési díjai bármelyik dokumentumon belül DocumentDB végrehajtott műveletek.

**Lesz, a dokumentumok hatással lehet a gyűjtemény van kiépítve az átviteli törlésekor?**

Nem, szemben a webhelycsoport kéréseket szolgáló kap prioritás futtatása a háttérben törlése a dokumentumok fölé. TTL (élettartam) hozzáadása a dokumentum bármely, az ez nem befolyásolja.

**Dokumentum lejártakor, mennyi ideig akkor marad a gyűjteményben marad?**

Amint a dokumentum elévül többé nem lesz elérhető. A pontos idő dokumentum marad a gyűjteményben ahhoz, hogy valóban törölni nem mérvadó, és amikor a háttérben folyamat nem tudja törölni a dokumentumot a alapján.

**Lejárt dokumentumok törli a minden csomópontra, vagy inkább "ahányat consistent?"**

A dokumentum nem lesz elérhető egyszerre minden csomópontra, és minden régióban.

**Van-e egy Licencelési költségét TTL-ellenőrző háttérben futó feladatokat?**

Nem, nincs semmilyen Licencelési költsége.

**Milyen gyakran ellenőrizni a TTL (élettartam) elévülési idejének beállítása?**

TTL (élettartam) elévülési idejének beállítása ellenőrzése nem fordulhat elő, mint egy háttér folyamat. Amikor válaszol egy kérelemre a kódmentes szolgáltatás végezze el az ellenőrzések beágyazott, és kizárja a lejárt dokumentumok. A Törlés a fizikai dokumentum aszinkron a háttérben futó csak folyamata. Ez a folyamat gyakoriságának a gyűjteményben kattintson a rendelkezésre álló RUs határozza meg.

**A TTL (élettartam) szolgáltatás csak vonatkozik a teljes dokumentumok vagy is lehet személyes dokumentumainak tulajdonságértékeket lejár?**

TTL (élettartam) a teljes dokumentum vonatkozik. Ha szeretné, hogy a dokumentum egy részéhez lejár, majd ajánlott része kinyerése a törzsdokumentumban egy másik "csatolt" dokumentumra, és a majd használja a TTL (élettartam) a kibontott dokumentumon.

**A TTL (élettartam) szolgáltatás rendelkezik bármely egyedi az indexelő követelmények?**

igen. A webhelycsoport kell rendelkeznie [az indexelés házirend beállítása](documentdb-indexing-policies.md) Lusta vagy azonos. DefaultTTL beállításakor az indexelő nincs értékre van állítva egy webhelycsoport a hibát, mint fognak, kapcsolja ki a gyűjteménye, amelynek már be van állítva egy DefaultTTL indexelés próbál eredményezi.


## <a name="next-steps"></a>Következő lépések

Azure DocumentDB kapcsolatos további információért olvassa el a szolgáltatás [*dokumentációja*](https://azure.microsoft.com/documentation/services/documentdb/) lapot.





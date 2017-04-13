<properties
    pageTitle="Autoscaling és az alkalmazás-szolgáltatási környezetben |} Microsoft Azure"
    description="Autoscaling és az alkalmazás-szolgáltatási környezetben"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="byvinyal"
/>

# <a name="autoscaling-and-app-service-environment"></a>Autoscaling és az alkalmazás-szolgáltatási környezetben

Azure környezetek alkalmazás *autoscaling*támogatja. Lehetőség van Automatikus méretezéssel egyes dolgozó készletek mértékek vagy a kimutatás alapján.

![Automatikus méretezéssel lehetőségek dolgozó erőforráskészlethez tartozik.][intro]

Autoscaling az erőforrás-kihasználtság optimalizálja automatikusan növekvő, és a költségvetési igazítása és, vagy betöltése alkalmazás szolgáltatási környezetben zsugorításához.

## <a name="configure-worker-pool-autoscale"></a>Dolgozó készlet Automatikus méretezéssel konfigurálása

Az Automatikus méretezéssel funkciókat elérhet dolgozó készlet a **Beállítások** fülre.

![A dolgozó készlet beállítások fülre.][settings-scale]

Itt kell a felület meglehetősen már jól ismert, mert a azonos felület, amikor, méretezni, hogy egy App szolgáltatáscsomagja látható. 

![Kézi méretarány-beállításai.][scale-manual]

Az Automatikus méretezéssel profil is beállítható.

![Automatikus méretezéssel beállításai gombra.][scale-profile]

Automatikus méretezéssel profilok akkor hasznos lehet ahhoz, hogy a skála korlátokat. Ezzel a módszerrel is egy alsó határ skála érték (1) és a kiszámíthatóan költés vonalvég megadásával egy felső határ (2) beállításával tapasztalható egységes teljesítményét.

![A profil méretarány-beállításai.][scale-profile2]

Után egy profilt ad meg, ha át kívánja méretezni, felfelé vagy lefelé a profil által meghatározott belül dolgozó készletben példányok száma Automatikus méretezéssel szabályok adhat hozzá. Automatikus méretezéssel szabályok mértékek alapulnak.

![Méretezés szabály.][scale-rule]

 Bármely dolgozó készlet vagy az előtér-mértékek definiáljon szabályokat Automatikus méretezéssel használható. Ezek a mértékek a azonos mérési módja miatt az erőforrás lap grafikonok figyelni vagy a vonatkozó értesítések beállítása.

## <a name="autoscale-example"></a>Automatikus méretezéssel példa

Legjobb is szemlélteti a Automatikus méretezéssel-szolgáltatási alkalmazás környezet útmutató alapján a forgatókönyv szerint.

Ez a cikk ismerteti a szükséges szempontok, Automatikus méretezéssel beállításakor. A cikk bemutatja az a szolgáltatásokat foglalja össze, abban az esetben, ha Ön kiszámíthatja autoscaling alkalmazás-szolgáltatási környezetben futtatott alkalmazás szolgáltatási környezetben.

### <a name="scenario-introduction"></a>Eset bemutatása

Ferenc egy, aki rendelkezik a feladatok ő kezelő egy részét áttérni az alkalmazás-szolgáltatási környezetben a vállalati rendszergazdák.

Az alkalmazás-szolgáltatási környezetben konfigurálva kézi skála, az alábbi képlettel történik:

* **Végű Előrehozás:** 3
* **Dolgozó készlet 1**: 10
* **2 dolgozó készlet**: 5
* **Dolgozó készlet 3**: 5

1 dolgozó készlet közben dolgozó készlet 2 és 3 dolgozó készlet segítségével minőség garancia (kérdések és válaszok) és a fejlesztés munkaterhelésekből gyártási munkaterhelésekből, szolgál.

Az App milyen szolgáltatáscsomagok fejlesztők és a kérdések és válaszok manuális skála vannak beállítva. A gyártási alkalmazás szolgáltatáscsomagja Automatikus méretezéssel kell foglalkozni az változatok betöltése és forgalom értéke.

Ferenc nagyon ismerősnek az alkalmazással. Tudja, hogy a terhelést a csúcs órák 9:00 de és a 18:00:00 között is, mivel ez egy a vállalati verzió (üzleti) alkalmazás, amely az alkalmazottak használata közben az Office. Ezt követően használatát esik, amikor a felhasználók végzett az adott napra. Kívüli csúcs óra van még néhány betöltése, mert a felhasználók hozzá tudnak férni az alkalmazás távolról a mobileszközök vagy az otthoni PC-re. A gyártási alkalmazás szolgáltatáscsomagja már be van állítva az Automatikus méretezéssel processzorhasználata a következő szabályok alapján:

![ÜZLETI alkalmazás bizonyos beállításait.][asp-scale]

|   **Automatikus méretezéssel profil – hétköznapokat – az alkalmazás szolgáltatáscsomagja**     |   **Automatikus méretezéssel profil – hétvégék – az alkalmazás szolgáltatáscsomagja**     |
|   ----------------------------------------------------    |   ----------------------------------------------------    |
|   **Neve:** Weekday profil                               |   **Neve:** Hétvége-profil                               |
|   **Méretarányát:** Kimutatás és a teljesítmény szabályok            |   **Méretarányát:** Kimutatás és a teljesítmény szabályok            |
|   **Profil:** Munkanapok száma                                   |   **Profil:** Ha a hétvégi                                    |
|   **Típusa:** Ismétlődés                                    |   **Típusa:** Ismétlődés                                    |
|   **Cél tartomány:** 5 – 20-példányok                     |   **Cél tartomány:** 3 – 10-példányok                     |
|   **Nap:** Hétfő, kedd, szerda, csütörtök, péntek  |   **Nap:** Szombat, vasárnap                              |
|   **A kezdő időpont:** 9:00 de.                                 |   **A kezdő időpont:** 9:00 de.                                 |
|   **Időzóna:** UTC-08                                   |   **Időzóna:** UTC-08                                   |
|                                                           |                                                           |
|   **Automatikus méretezéssel szabály (skála felfelé)**                           |   **Automatikus méretezéssel szabály (skála felfelé)**                           |
|   **Erőforrás:** A gyártási (alkalmazás szolgáltatási környezetben)      |   **Erőforrás:** A gyártási (alkalmazás szolgáltatási környezetben)      |
|   **Metrikus:** PROCESSZOR %                                       |   **Metrikus:** PROCESSZOR %                                       |
|   **Művelet:** Nagyobb, mint 60 %-kal                         |   **Művelet:** 80 %-nál nagyobb                         |
|   **Időtartamot:** 5 perccel                                 |   **Időtartamot:** 10 perc                                |
|   **Idő összesítést:** Átlag                           |   **Idő aggregáció:** Átlag                           |
|   **Művelet:** 2 darab növelése                         |   **Művelet:** Darab növelése 1                         |
|   **Izgalmas lefelé (perc):** 15                             |   **Izgalmas lefelé (perc):** 20                             |
|                                                           |                                                           |
  	|   **Automatikus méretezéssel szabály (skála lefelé)**                     |   **Automatikus méretezéssel szabály (skála lefelé)**                         |
|   **Erőforrás:** Gyártás (alkalmazás szolgáltatási környezetben)      |   **Erőforrás:** A gyártási (alkalmazás szolgáltatási környezetben)      |
|   **Metrikus:** PROCESSZOR %                                       |   **Metrikus:** PROCESSZOR %                                       |
|   **Művelet:** 30 %-nál kevesebb                            |   **Művelet:** Kisebb, mint 20 %                            |
|   **Időtartamot:** 10 perc                                |   **Időtartam:** 15 percet                                |
|   **Idő összesítési:** Átlag                           |   **Idő összesítési:** Átlag                           |
|   **Művelet:** Darab csökkentése 1                         |   **Művelet:** Darab csökkentése 1                         |
|   **Izgalmas lefelé (perc):** 20                             |   **Izgalmas lefelé (perc):** 10                             |

### <a name="app-service-plan-inflation-rate"></a>Alkalmazás szolgáltatás terv inflációja

App milyen szolgáltatáscsomagok beállított Automatikus méretezéssel a maximális sebesség óránként műveletet. Ez a kamatláb a Automatikus méretezéssel szabály megadott értékek alapján számítható.

Ismertetése, és az *alkalmazás szolgáltatás terv inflációja* kiszámításához fontos alkalmazás szolgáltatási környezetben Automatikus méretezéssel mert dolgozó készletbe méretezési módosítások nem pillanatnyi.

Az alkalmazás szolgáltatás terv inflációja a következőképpen kiszámítása:

![Alkalmazás szolgáltatás terv inflációja számítási.][ASP-Inflation]

Az Automatikus méretezéssel – a gyártási alkalmazás szolgáltatáscsomagja hét.napja profiljának skála felfelé szabály alapján:

![Terv-inflációja alkalmazás szolgáltatás az Automatikus méretezéssel – skála be a szabály alapján munkanapok száma.][Equation1]

Az Automatikus méretezéssel – a hétvége profilt a gyártási alkalmazás szolgáltatáscsomagja skála felfelé szabály esetében a képletet úgy szeretné, hogy:

![Terv-inflációja alkalmazás szolgáltatás a hétvégék Automatikus méretezéssel – skála be a szabály alapján.][Equation2]

Ez az érték is kiszámítható skála lefelé műveletekhez.

Az Automatikus méretezéssel – az alkalmazás szolgáltatáscsomagja gyártási hét.napja profiljának skála lefelé szabály alapján ez jelenne meg az alábbi képlettel történik:

![Terv-inflációja alkalmazás szolgáltatás az Automatikus méretezéssel – skála lefelé szabály alapján munkanapok száma.][Equation3]

Az Automatikus méretezéssel – a hétvége profilt a gyártási alkalmazás szolgáltatáscsomagja skála lefelé szabály esetében a képletet úgy szeretné, hogy:  

![Terv-inflációja alkalmazás szolgáltatás a hétvégék Automatikus méretezéssel – skála lefelé szabály alapján.][Equation4]

A gyártási alkalmazás szolgáltatáscsomagja a maximális sebesség nyolc példányok/óra a héten és a négy példányok/óra során a hétvége nagyobb is. Azt is engedje fel az a héten négy példányok/óra maximális sebesség példányok hat példányok/óra hétvégék során.

Ha több App milyen szolgáltatáscsomagok egy dolgozó készletben tárolt, van-e a inflációja, amelyeket az összes alkalmazás szolgáltatás csomagok összegeként számítja ki a *teljes inflációja* szolgáltatója, az adott dolgozó készletre.

![Több, egy dolgozó készletben üzemeltetett App milyen szolgáltatáscsomagok teljes inflációja számítása.][ASP-Total-Inflation]

### <a name="use-the-app-service-plan-inflation-rate-to-define-worker-pool-autoscale-rules"></a>Az alkalmazás szolgáltatás terv inflációja segítségével dolgozó készlet Automatikus méretezéssel szabály megadása

Dolgozó készletek App milyen szolgáltatáscsomagok, vannak beállítva az Automatikus méretezéssel kell egy pufferelési kapacitás kiosztható tároló. A puffer lehetővé teszi, hogy a nagyobb, és az alkalmazás szolgáltatáscsomagja igazodjon Automatikus méretezéssel műveletekhez. A minimális puffer a számított összes alkalmazás szolgáltatás megtervezése inflációja lenne.

Alkalmazás-szolgáltatási környezetben skála műveletek alkalmazása némi időbe telik, mert bármilyen módosítást kell figyelembe további igény szerint a módosításokat, amelyeket kerül sor, míg a méretezés művelet folyamatban van. A késés rendelkezésre, azt javasoljuk használható a számított összes alkalmazás szolgáltatás megtervezése inflációja hozzáadott példányok minimális száma az egyes Automatikus méretezéssel műveletekhez.

Ezekkel az információkkal Ferenc adhatja meg a következő Automatikus méretezéssel profil és a szabályok:

![Automatikus méretezéssel profil szabályok üzleti például.][Worker-Pool-Scale]

|   **Automatikus méretezéssel profil – munkanapok száma**                        |   **Automatikus méretezéssel profil – hétvégék**                |
|   ----------------------------------------------------    |   --------------------------------------------    |
|   **Neve:** Weekday profil                               |   **Neve:** Hétvége-profil                       |
|   **Méretarányát:** Kimutatás és a teljesítmény szabályok            |   **Méretarányát:** Kimutatás és a teljesítmény szabályok    |
|   **Profil:** Munkanapok száma                                   |   **Profil:** Ha a hétvégi                            |
|   **Típusa:** Ismétlődés                                    |   **Típusa:** Ismétlődés                            |
|   **Cél tartomány:** 13-25 példányok                    |   **Cél tartomány:** 6 – 15-példányok             |
|   **Nap:** Hétfő, kedd, szerda, csütörtök, péntek  |   **Nap:** Szombat, vasárnap                      |
|   **A kezdő időpont:** 7:00 de.                                 |   **A kezdő időpont:** 9:00 de.                         |
|   **Időzóna:** UTC-08                                   |   **Időzóna:** UTC-08                           |
|                                                           |                                                   |
|   **Automatikus méretezéssel szabály (skála felfelé)**                           |   **Automatikus méretezéssel szabály (skála felfelé)**                   |
|   **Erőforrás:** Dolgozó készlet 1                             |   **Erőforrás:** Dolgozó készlet 1                     |
|   **Metrikus:** WorkersAvailable                            |   **Metrikus:** WorkersAvailable                    |
|   **Művelet:** Kisebb, mint 8                              |   **Művelet:** Kisebb, mint a 3                      |
|   **Időtartam:** 20 perc                                |   **Időtartam:** 30 percben                        |
|   **Idő összesítési:** Átlag                           |   **Idő összesítési:** Átlag                   |
|   **Művelet:** 8 darab növelése                         |   **Művelet:** 3 darab növelése                 |
|   **Izgalmas lefelé (perc):** 180                            |   **Izgalmas lefelé (perc):** 180                    |
|                                                           |                                                   |
|   **Automatikus méretezéssel szabály (skála lefelé)**                         |   **Automatikus méretezéssel szabály (skála lefelé)**                 |
|   **Erőforrás:** Dolgozó készlet 1                             |   **Erőforrás:** Dolgozó készlet 1                     |
|   **Metrikus:** WorkersAvailable                            |   **Metrikus:** WorkersAvailable                    |
|   **Művelet:** 8-nál nagyobb                           |   **Művelet:** Nagyobb, mint 3                   |
|   **Időtartam:** 20 perc                                |   **Időtartam:** 15 percet                        |
|   **Idő összesítési:** Átlag                           |   **Idő aggregáció:** Átlag                   |
|   **Művelet:** 2 darab csökkentése                         |   **Művelet:** 3 darab csökkentése                 |
|   **Izgalmas lefelé (perc):** 120-ra                            |   **Izgalmas lefelé (perc):** 120-ra                    |

A profil megadott cél tartományt a minimális példányok a profilját az alkalmazás szolgáltatáscsomagja + pufferelési meghatározott kiszámításához.

A maximális tartomány lenne összegét az maximális tartományokat a dolgozó készletben üzemeltetett App szolgáltatás csomagjai között.

A szükséges szabályokat skála növelése számának legalább 1 X skála alkalmazás szolgáltatás terv inflációja létre kell hozni.

Csökkentése darab módosítható valamire 1/2 X vagy az 1 X skála alkalmazás szolgáltatás terv inflációja közötti lefelé.

### <a name="autoscale-for-front-end-pool"></a>Az előtér-készlet Automatikus méretezéssel

Az előtér-Automatikus méretezéssel szabályokat kell egyszerűbb a készletek dolgozói számára. Elsősorban tegye a következőt:  
Győződjön meg arról, hogy a mérési és a cooldown időzítő időtartama fontolja meg, hogy az alkalmazás szolgáltatás csomagjával skála műveletek nem pillanatnyi állnak.

Az ebben az esetben a Ferenc tudja, hogy a hiba ráta első végű elérje a 80 %-os Processzor kihasználtsági megnő, és a következőképpen növeléséhez példányok Automatikus méretezéssel szabály:

![Előtér-készlet Automatikus méretezéssel beállításait.][Front-End-Scale]

|   **Automatikus méretezéssel profil – első vége**              |
|   --------------------------------------------    |
|   **Neve:** Automatikus méretezéssel – első vége                |
|   **Méretarányát:** Kimutatás és a teljesítmény szabályok    |
|   **Profil:** Hétköznapi                           |
|   **Típusa:** Ismétlődés                            |
|   **Cél tartomány:** 3 – 10-példányok             |
|   **Nap:** Hétköznapi                              |
|   **A kezdő időpont:** 9:00 de.                         |
|   **Időzóna:** UTC-08                           |
|                                                   |
|   **Automatikus méretezéssel szabály (skála felfelé)**                   |
|   **Erőforrás:** Előtér-készlet                    |
|   **Metrikus:** PROCESSZOR %                               |
|   **Művelet:** Nagyobb, mint 60 %-kal                 |
|   **Időtartam:** 20 perc                        |
|   **Idő összesítést:** Átlag                   |
|   **Művelet:** 3 darab növelése                 |
|   **Izgalmas lefelé (perc):** 120-ra                    |
|                                                   |
|   **Automatikus méretezéssel szabály (skála lefelé)**                 |
|   **Erőforrás:** Dolgozó készlet 1                     |
|   **Metrikus:** PROCESSZOR %                               |
|   **Művelet:** 30 %-nál kevesebb                    |
|   **Időtartam:** 20 perc                        |
|   **Idő összesítést:** Átlag                   |
|   **Művelet:** 3 darab csökkentése                 |
|   **Izgalmas lefelé (perc):** 120-ra                    |

<!-- IMAGES -->
[intro]: ./media/app-service-environment-auto-scale/introduction.png
[settings-scale]: ./media/app-service-environment-auto-scale/settings-scale.png
[scale-manual]: ./media/app-service-environment-auto-scale/scale-manual.png
[scale-profile]: ./media/app-service-environment-auto-scale/scale-profile.png
[scale-profile2]: ./media/app-service-environment-auto-scale/scale-profile-2.png
[scale-rule]: ./media/app-service-environment-auto-scale/scale-rule.png
[asp-scale]: ./media/app-service-environment-auto-scale/asp-scale.png
[ASP-Inflation]: ./media/app-service-environment-auto-scale/asp-inflation-rate.png
[Equation1]: ./media/app-service-environment-auto-scale/equation1.png
[Equation2]: ./media/app-service-environment-auto-scale/equation2.png
[Equation3]: ./media/app-service-environment-auto-scale/equation3.png
[Equation4]: ./media/app-service-environment-auto-scale/equation4.png
[ASP-Total-Inflation]: ./media/app-service-environment-auto-scale/asp-total-inflation-rate.png
[Worker-Pool-Scale]: ./media/app-service-environment-auto-scale/wp-scale.png
[Front-End-Scale]: ./media/app-service-environment-auto-scale/fe-scale.png

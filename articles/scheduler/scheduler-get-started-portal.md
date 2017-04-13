<properties
 pageTitle="Első lépések az Azure ütemező az Azure portál |} Microsoft Azure"
 description="Első lépések az Azure ütemező az Azure portál"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="hero-article"
 ms.date="08/10/2016"
 ms.author="deli"/>

# <a name="get-started-with-azure-scheduler-in-azure-portal"></a>Első lépések az Azure ütemező az Azure portál

Nagyon egyszerűen ütemezett feladat létrehozása az Azure ütemezőt. Ebben az oktatóanyagban meg fog megtudhatja, hogy miként hozzon létre egy projektet. A Feladatütemező figyelő és felügyeleti lehetőségeket is megismerheti.

## <a name="create-a-job"></a>Feladat létrehozása

1.  Jelentkezzen be az [Azure](https://portal.azure.com/)-portálra.  

2.  Kattintson a **+ Új** > a Keresés mezőbe írja be _a Feladatütemező_ > között válassza a **Feladatütemező** > kattintson a **Létrehozás**gombra.

     ![][marketplace-create]

3.  A feladat, amely a GET kérésekben http://www.microsoft.com/ egyszerűen találatok létrehozása. Az **Ütemezési feladat** képernyőn adja meg az alábbi adatokat:

    1.  **Neve:**`getmicrosoft`  

    2.  **Előfizetés:** Az Azure előfizetés   

    3.  **Webhelycsoport feladat:** Jelölje ki a meglévő projekt gyűjtő, vagy kattintson a **Hozzon létre új** > írjon be egy nevet.

4.  Ezután az **Akcióbeállítások**, adja meg az alábbi értékeket:

    1.  **Művelettípus:**` HTTP`  

    2.  **Módszer:**`GET`  

    3.  **URL-címe:**` http://www.microsoft.com`  

      ![][action-settings]

5.  Végezetül vegyük ütemezéséről. A feladat akkor beszélünk, ha egyszer használatos feladatként, de most válassza az ismétlődés ütemezett:

    1. **Ismétlődés**:`Recurring`

    2. **Indítsa el**: a mai dátum

    3. **Ismétlődő minden**:`12 Hours`

    4. **Befejezési dátum**: két nap múlva adatait a dátum  

      ![][recurrence-schedule]

6.  Kattintson a **Létrehozás** gombra

## <a name="manage-and-monitor-jobs"></a>Kezelheti a feladatok és figyelése

Amikor egy projekt létrejött, a fő Azure irányítópult jelenik meg. Kattintson a feladatot, és egy új ablak megnyílik, és a következő lapokon:

1.  Tulajdonságok  

2.  Az akcióbeállítások  

3.  Ütemterv  

4.  Előzmények

5.  Felhasználók

    ![][job-overview]

### <a name="properties"></a>Tulajdonságok

A csak olvasható tulajdonságok ismertetik az ütemezési feladat kezelése metaadatait.

   ![][job-properties]


### <a name="action-settings"></a>Az akcióbeállítások

A **feladatok** képernyőn feladatra kattintva lehetővé teszi, hogy a projekt beállítása. Ezzel a radírral speciális beállításainak megadása, ha nem adja meg azokat a gyors létrehozása varázsló.

Az összes művelet típusait módosíthatja a újrapróbálkozási házirendek és a művelet a hiba esetén.

A http- és HTTPS feladat tevékenységtípusok bármilyen engedélyezett HTTP igei módszer változhat. Is felveheti, törlése vagy módosítása az élőfejben és az egyszerű hitelesítési adatait.

Tárterület várólista tevékenységtípusok módosíthatja a tárterület-fiókot, a várólista neve, a biztonsági jogkivonat és az üzenet.

Szolgáltatás bus tevékenységtípusok, az témakör/várólista elérési útját, hitelesítési beállítások, átviteli típusa, üzenet tulajdonságai és szövegtörzs névtér előfordulhat, hogy módosítása.

   ![][job-action-settings]

### <a name="schedule"></a>Ütemterv

Ezzel a radírral újrakonfigurálni az ütemezést, ha azt szeretné, a létrehozott ütemezésének módosítása a gyors létrehozása varázsló.

Ez a lehetőség [összetett ütemterveket és a feladatok a speciális ismétlődés](scheduler-advanced-complexity.md) összeállítása

Előfordulhat, hogy módosítja a kezdő dátum és idő, Ismétlődés ütemezése és a záró dátum és idő (Ha a feladat ismétlődő.)

   ![][job-schedule]


### <a name="history"></a>Előzmények

Az **Előzmények** lapot a minden feladat végrehajtását kijelölt mértékek a rendszer a kiválasztott feladat jeleníti meg. Ezek a mértékek adja meg a Feladatütemező állapotának valós idejű értékeit:

1.  Állapot  

2.  Részletek  

3.  Újrapróbálkozások

4.  Előfordulás: 1st, 2nd, 3, stb.

5.  Végrehajtás elindítása  

6.  Végrehajtási befejezési időpontja

   ![][job-history]

Kattintson a Futtatás megtekinthesse a **Részletes előzményeket**, többek között a teljes válasz minden végrehajtás kattinthat. Ezen a párbeszédpanelen is lehetővé teszi, hogy másolja a vágólapra a kérdésre adott választ.

   ![][job-history-details]

### <a name="users"></a>Felhasználók

Azure szerepköralapú hozzáférés vezérlő (RBAC) lehetővé teszi, hogy az Azure ütemezője külön hozzáférés kezelése. Megtudhatja, hogy miként felhasználók lapjának használatáról, olvassa el [Azure Role-Based hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md)


## <a name="see-also"></a>Lásd még:

 [Mi az a Feladatütemező?](scheduler-intro.md)

 [A Feladatütemező fogalmak, kifejezések és entitás hierarchia](scheduler-concepts-terms.md)

 [Tervek és a számlázásra az Azure ütemező](scheduler-plans-billing.md)

 [Összetett ütemterveket és a Azure ütemezővel speciális ismétlődés létrehozásának](scheduler-advanced-complexity.md)

 [A Feladatütemező REST API-hivatkozás](https://msdn.microsoft.com/library/mt629143)

 [Referencia a Feladatütemező PowerShell-parancsmagok](scheduler-powershell-reference.md)

 [A Feladatütemező magas rendelkezésre állás és megbízhatóság](scheduler-high-availability-reliability.md)

 [A Feladatütemező korlátozások, alapértelmezett és hibakódok esetén](scheduler-limits-defaults-errors.md)

 [Kimenő hitelesítést ütemező](scheduler-outbound-authentication.md)


[marketplace-create]: ./media/scheduler-get-started-portal/scheduler-v2-portal-marketplace-create.png
[action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-action-settings.png
[recurrence-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-recurrence-schedule.png
[job-properties]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-properties.png
[job-overview]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-overview-1.png
[job-action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-action-settings.png
[job-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-schedule.png
[job-history]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history.png
[job-history-details]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history-details.png


[1]: ./media/scheduler-get-started-portal/scheduler-get-started-portal001.png
[2]: ./media/scheduler-get-started-portal/scheduler-get-started-portal002.png
[3]: ./media/scheduler-get-started-portal/scheduler-get-started-portal003.png
[4]: ./media/scheduler-get-started-portal/scheduler-get-started-portal004.png
[5]: ./media/scheduler-get-started-portal/scheduler-get-started-portal005.png
[6]: ./media/scheduler-get-started-portal/scheduler-get-started-portal006.png
[7]: ./media/scheduler-get-started-portal/scheduler-get-started-portal007.png
[8]: ./media/scheduler-get-started-portal/scheduler-get-started-portal008.png
[9]: ./media/scheduler-get-started-portal/scheduler-get-started-portal009.png
[10]: ./media/scheduler-get-started-portal/scheduler-get-started-portal010.png
[11]: ./media/scheduler-get-started-portal/scheduler-get-started-portal011.png
[12]: ./media/scheduler-get-started-portal/scheduler-get-started-portal012.png
[13]: ./media/scheduler-get-started-portal/scheduler-get-started-portal013.png
[14]: ./media/scheduler-get-started-portal/scheduler-get-started-portal014.png
[15]: ./media/scheduler-get-started-portal/scheduler-get-started-portal015.png

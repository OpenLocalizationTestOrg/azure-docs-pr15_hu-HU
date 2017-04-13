<properties
   pageTitle="Az adatok szolgáltatás ajánlat a piactér tesztelése |} Microsoft Azure"
   description="Megtudhatja, hogyan tesztelje a adatszolgáltatás ajánlatot, a Microsoft Azure piactéren."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="hascipio; avikova" />

# <a name="testing-your-data-service-offer-in-staging"></a>Az adatok szolgáltatás ajánlat átmeneti tesztelése

>[AZURE.IMPORTANT] **Adott időben azt már nem bevezetési minden új adatszolgáltatás közzétevők. Új dataservices nem első jóváhagyva listaelem.** Ha a szoftver üzleti alkalmazások AppSource a közzétenni kívánt talál további információt [Itt](https://appsource.microsoft.com/partners). Ha egy IaaS alkalmazások vagy Fejlesztőeszközök szolgáltatás szeretné közzé a Microsoft Azure piactéren, hogy több információt találhat [az alábbi](https://azure.microsoft.com/marketplace/programs/certified/).

Az első két lépést [a Microsoft Developer fiók létrehozását](marketplace-publishing-accounts-creation-registration.md) , és [a szolgáltatás-ajánlat közzétételi portál létrehozása](marketplace-publishing-data-service-creation.md) befejezése után készen áll a az ajánlat elérhetővé tétele a Microsoft Azure piactéren. Ez a témakör azt ismerteti, hogy az első, betétlap "Átmeneti" nevű lépés keresztül

Átmeneti azt jelenti, hogy üzembe helyezése a magánjellegű "védőfalas", ahol tesztelése, és ellenőrizze a szolgáltatásai, mielőtt termelési terjesztése azt a felajánlást. Az ajánlat ugyanúgy, mint egy ügyfélnek, aki rendelkezik rendszerbe tenné átmeneti fog megjelenni.

## <a name="step-1-pushing-your-offer-to-staging"></a>Lépés: 1. A fejlesztői ajánlat terjesztése
A fejlesztői ajánlat terjesztése lehetővé teszi, hogy az ajánlat tesztelhet, mielőtt a jövőbeli előfizetők számára elérhetővé válik.  Láthatja, hogy hogyan az ajánlat nyílik jelennek meg, és azok az adatok előfizetés működik.  

  ![rajz](media/marketplace-publishing-data-service-test-in-staging/step-1.1.png)

1.  Jelentkezzen be a [közzétételi portál](https://publish.windowsazure.com)
2.  A bal oldali navigációs ablakban válassza a **Data Services**
3.  Jelölje ki a kívánt átmeneti a leküldéses ajánlatot. A fenti képernyő jelenik meg.
4.  **Leküldéses a fejlesztői** gombra.  
5.  Az ajánlat szükséges hátralévő előtt a fejlesztői terjesztése kapcsolatos problémák esetén jelenik meg a lista jelenik meg.  Kattintson a minden elemet javítsa ki ezeket az elemeket. Ha az összes javítások végrehajtani, és kattintson ismét **a fejlesztői leküldéses** gombra.

Ha nem az az ajánlat problémák lépnek fel az előugró ablak, az alábbi jelenik meg.  

Ha éppen nem tervezési/nem jóvá felszíni az ajánlatot az Azure-portálon (jelenleg korlátozott kapacitás), egyszerűen zárja be az előugró ablakot.

Az adatok szolgáltatás tesztelje az Azure-portálon (mellett a DataMarket portál), szüksége lesz egy tesztelje az Azure-előfizetés azonosítója.  A előfizetés azonosítója lesz azonosítani tudja a fiókot, tesztelje a felajánlást engedélyezett.  

Kivágás, és illessze be az előfizetés azonosítója, és továbbra is a pipajeles gombra.

  ![rajz](media/marketplace-publishing-data-service-test-in-staging/step-1.2.png)

> [AZURE.NOTE] Ezek az Azure előfizetések azonosítói csak tesztelése és az [Azure Kezelőportálja](https://manage.windowsazure.com)átmeneti szükséges. Nincs rájuk szüksége kattintva tesztelje a Microsoft Azure piactéren.

A következő képernyőn megjelenő jeleníti meg, hogy a közzététel folyik jelennek meg a "folyamatban" ikon kiemelve az alábbi sárga. Az átmeneti terjesztése megnyitja 10 – 15 perccel között.  Ha tovább tart, először frissítse a böngészőben (nyomja le az F5 billentyűparancs hatására az Internet Explorer).  A ritkán, ahol az ajánlat továbbra is terjesztése után egy órával átmeneti, kattintson a megfelelő névjegyre us csatolása tudassa velünk, hogy van-e hibát.

  ![rajz](media/marketplace-publishing-data-service-test-in-staging/step-1.3.png)

A "folyamatban" ikon a leküldéses előkészítése az befejeztével fog leállítása áthelyezése és az állapot frissülnek "Szakaszos".  Most már készen áll az ajánlat ellenőrzéséhez.  

## <a name="step-2-test-your-staged-offer-in-datamarket"></a>Lépés: 2. A szakaszos ajánlat tesztelje a datamarket rendszerből

Kattintson a hivatkozásra, a **"Lásd: A szolgáltatás kínálnak teljesítmény..."** szöveget követő megjelenítéséhez a képernyő, hogy az előfizetői fogják látni, amikor az ajánlat gyártási Ugrás és DataMarket meg fog jelenni.

  ![rajz](media/marketplace-publishing-data-service-test-in-staging/step-2.2.png)

Teszt, illetve ellenőrizze a fent megjelölt összes 12 elem összes emblémák, árak/tranzakciók, szöveg, képek, dokumentáció biztosítása érdekében, és a hivatkozásokat, hogy helyes-e és működik megfelelően.  Az egy időben annak érdekében, hogy minden megadott az ajánlat létrehozásakor próba értéket a tényleges értékek helyettesíti.

1. Ajánlat embléma
2. Ajánlat neve
3. A Publisher név/ismerhetők fel a vállalat
4. Az ajánlatra kategóriák keresése
5. Az ajánlat támogatás hivatkozásával előfizetők segítésére
6. Az ajánlat környezetfüggő leírása
7. Részletes számlaadatokkal ábrázoló ajánlat terv
8. Végrehajtási kód mutató hivatkozás
9. Minta képek bemutatják, hogy az ajánlatra vonatkozó adatok használata
10. Az egyes szolgáltatásokhoz belül az ajánlatra vonatkozó bemeneti és kimeneti metaadatok
11. Ajánlat használati feltételei
12. Az ajánlatra vonatkozó adatok megtekintése


Végül, ellenőrizze a szolgáltatás "Feltárása a ADATKÉSZLET" hivatkozására kattintva a Datamarket keresztül fog működni.  Új ablak nyílik meg a eszközben hívása "Szolgáltatás Explorer", megtekintheti a lekérdezés eredményének szemben a szolgáltatásban.  Ebben az ablakban adja meg a szükséges paramétereket, és a találatokat, megjelenik egy lekérdezés szemben a szolgáltatásban.   Megjelenített is az URL-címet a lekérdezés.  

> [AZURE.NOTE] Ügyeljen arra, hogy ellenőrizze a szolgáltatás felső részén látható szöveges leírása.  És ha egynél több szolgáltatást tartalmaz az ajánlat a hívás, kattintson a Váltás a következő szolgáltatás áttekintése és tesztelése a képernyő alján a Tabulátorok gombra.



## <a name="next-step"></a>Következő lépés
Ha nehézségei vannak, és azokat segítségre van szüksége a lépjen kapcsolatba az [Azure Publisher támogatás]( http://go.microsoft.com/fwlink/?LinkId=272975).

Ha elégedett, és készen áll a közzététel az ajánlat olvassa el a [Leküldéses a termelési jóváhagyási kérelem](marketplace-publishing-push-to-production.md) dokumentációt.

## <a name="see-also"></a>Lásd még:
- [Első lépések: Útmutató felajánlás közzététele az Azure piactérről](marketplace-publishing-getting-started.md)

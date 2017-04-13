<properties
   pageTitle="Tesztelje a virtuális ajánlat a piactér |} Microsoft Azure"
   description="Megtudhatja, hogyan az Azure piactéren elérhető a virtuális tesztkép."
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
   ms.date="08/01/2016"
   ms.author="hascipio" />

# <a name="test-your-vm-offer-for-the-azure-marketplace-in-staging"></a>Tesztelje a virtuális ajánlatot, a Microsoft Azure piactéren előkészítése

Átmeneti azt jelenti, hogy üzembe helyezése a Termékváltozat a magánjellegű "védőfalas", ahol tesztelése és szolgáltatásait érvényesítése üzembe helyezése a piactér előtt. A Termékváltozat megjelenik egy ügyfélnek, aki rendelkezik rendszerbe volna átmeneti. A virtuális kép kell tanúsítvánnyal a fejlesztői kell tolódik.

## <a name="step-1-push-your-offer-to-staging"></a>Lépés: 1: Leküldéses előkészítése az ajánlat

1. A **Közzététel** lapon kattintson a **tartalomtípusban átmeneti szeretné**.

    ![rajz](media/marketplace-publishing-vm-image-test-in-staging/vm-image-push-to-staging.png)

2. Ha a közzétételi portál figyelmeztet az esetleges hibákat, javítsa ki őket.
3.  Az a **ki férhet hozzá a szakaszos ajánlat?** párbeszédpanelen adja meg a használni kívánt az ajánlatot az [Azure előzetes portál](https://portal.azure.com)előzetes Azure előfizetések listáját,.

    >[AZURE.NOTE] Virtuális gépeken futó, és megoldást sablonokat, CSP, DreamSpark vagy a Megnyitás az Azure típusú whitelist előfizetések **nem** adja meg.


    > Abban az esetben, ha virtuális gépeken futó a **LEKÜLDÉSES a fejlesztői**gombra kattintva az alábbi lépésekkel megtörténik a jelenet mögött. Fogja tudni a közzététel lapon minden egyes lépés végrehajtásának tekintheti meg a közzétételi portál. Először vegye ezen az oldalon rendszeres időközönként (mindaddig, amíg az Állapot oszlopban a szakaszos) van szüksége a végéről korrekciós hiba információkat.

    > - Először átmeneti tárolásra szolgáló kérését a hitelesítő csapatának ki a virtuális érvényesítése ugrik. Azonban ha kérését megtalálja rendelkezik, csak marketing módosítása, majd a tanúsítvány lépés kimarad.
    > - A minősítési elkészülte az ajánlat replikációs végig az Azure adatközpontokkal indítása Általában kerül 24-48hours a replikáció végrehajtása, de a virtuális méretétől függően egy hétig is eltarthat. Azonban kérését megtalálja rendelkezik, csak marketing módosítása, majd a replikáció esetén gyorsabb.
    > - Amikor befejeződött a replikáció, az ajánlatot az [Azure portál](http:/portal.azure.com)elérhető lesz. Ebben az állapotát időszakban válnak ELŐKÉSZÍTETT, kattintson a közzététel portálon. A szakaszos ajánlat az [Azure portál](http:/portal.azure.com) csak az e-mailek azonosítója, amellyel az ajánlatra vonatkozó előkészített van-előfizetéséhez társított látható.

4. Jelentkezzen be az [Azure megtekintése a portálon](https://portal.azure.com) szerepel a felsorolásban, az előző lépésben az Azure előfizetések valamelyikével.
5. Keresse meg az ajánlat, és a virtuális kép pontok ellenőrzése:
  - Győződjön meg arról, hogy marketingtartalmak jelenik meg megfelelően a piactérről.
  - A virtuális kép végpont telepítés.

      ![img-leképezés-portál](media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg)

> [AZURE.IMPORTANT] Az ajánlat maradnak mindaddig, amíg Ön értesíti a Microsoft a közzétételi portál keresztül átmeneti [**Közzététel** fülre > a gombra kattintva **"kérése jóváhagyási a leküldéses való gyártási"**], hogy készen áll a termelési leküldéses. Ez az, hogy az összes tagja a csapat jelölőnégyzet felett, amit az Áttekintés felsorolt ajánlat előkészítése a egy ideális időpontot.

> Az átmeneti tárolásra szolgáló platform a publisher az ajánlat egy kép módban teszteléshez lett tervezve. Kifejezetten nem ajánlott a platofrm használatával létrejöttét céljából.

## <a name="next-steps"></a>Következő lépések
Most, hogy az ajánlat "előkészített", és szolgáltatásait és a marketingtartalmak tesztelése, ahol folytathatja a végleges közzétételi fázis, **lépés: 4**: [üzembe helyezése a Piactérhez a felajánlást](marketplace-publishing-push-to-production.md).

## <a name="see-also"></a>Lásd még:
- [Első lépések: a Microsoft Azure piactéren felajánlás közzététele](marketplace-publishing-getting-started.md)

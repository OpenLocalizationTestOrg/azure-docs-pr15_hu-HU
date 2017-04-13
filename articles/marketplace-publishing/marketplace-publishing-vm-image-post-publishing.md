<properties
   pageTitle="A virtuális gép képe, a Microsoft Azure piactéren lévő kezelése |} Microsoft Azure"
   description="A virtuális gép képe, a Microsoft Azure piactéren lévő kezeléséről a kezdeti közzétételét követően részletes útmutatót."
   services="Azure Marketplace"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="Azure"
   ms.workload="na"
   ms.date="08/03/2016"
   ms.author="hascipio;"/>

# <a name="post-production-guide-for-virtual-machine-offers-in-the-azure-marketplace"></a>A Microsoft Azure piactéren található virtuális gép ajánlatok gyártás utáni útmutató

Ez a cikk ismerteti, hogyan frissítheti a élő virtuális gép ajánlat a az Azure piactéren. Végigvezeti a folyamaton, egy vagy több új termékváltozatok hozzáadása meglévő felajánlás a és a élő virtuális gép ajánlat vagy Termékváltozat eltávolítása a Microsoft Azure piactéren.

Miután ajánlat/Termékváltozat elő van készítve az [Azure-portálra](http://portal.azure.com), az alábbi mezők nem módosítható:

- **Azonosító kínálják:** [Közzétételi portál -> virtuális gépeken futó-az ajánlat -> Kijelölés > virtuális képek lap -> kínálnak azonosító]
- **Termékváltozat azonosító:** [Közzétételi portál -> virtuális gépeken futó-az ajánlat -> Kijelölés > lap -> termékváltozatok hozzáadása Termékváltozatot]
- **Publisher Namespace:** [Közzétételi portál -> virtuális gépeken futó-útmutató lap -> Us céges (található a "Lépés 2 regisztrálhatja a vállalati") -> Tovább > Publisher Namespace -> Namespace]

Ha az ajánlat/Termékváltozat a a [Microsoft Azure piactéren](http://azure.microsoft.com/marketplace)szerepel, nem módosítható a mezők az alábbi:

- **Azonosító kínálják:** [Közzétételi portál -> virtuális gépeken futó-az ajánlat -> Kijelölés > virtuális képek lap -> kínálnak azonosító]
- **Termékváltozat azonosító:** [Közzétételi portál -> virtuális gépeken futó-az ajánlat -> Kijelölés > lap -> termékváltozatok hozzáadása Termékváltozatot]
- **Publisher Namespace:** [Közzétételi portál -> virtuális gépeken futó lap -> forgatókönyv -> megállapítani, hogy Us kapcsolatban a vállalati (található a lépés 2 regisztrálni) Publisher Namespace -> Namespace]
- **Portok** [Közzétételi portál -> virtuális gépeken futó-az ajánlat -> Kijelölés > virtuális képek lap -> nyitott portokat]
- **Árak felsorolt SKU(s) módosítása**
- **Számlázás felsorolt SKU(s) modell módosítása**
- **Egyes területeire vonatkozó felsorolt SKU(s) számlázási eltávolítása**
- **Felsorolt SKU(s) adatok lemez számának módosítása**



## <a name="1-how-to-update-the-technical-details-of-a-sku"></a>1. hogyan Termékváltozatot technikai részleteinek módosítása

Új verzió hozzáadása a felsorolt Termékváltozat, és tegye közzé újra az ajánlatot az alábbi lépésekkel:

1. Jelentkezzen be a [közzétételi portál](https://publish.windowsazure.com).
2. Nyissa meg azt a **virtuális gépeken FUTÓ** fülre, és válassza a felajánlást.
3. A bal oldali menüben kattintson a **Virtuális képek** fülre.
4. **Virtuális képek** fülre **termékváltozatok** csoportban keresse meg a frissíteni kívánt Termékváltozat.
5. Ezt követően adja hozzá a Termékváltozat új verziószáma, és kattintson a **"+"** gombra. Az új verzió X.Y.Z formátum kell lennie, hol találhatók az X, Y és Z a egészérték részt veszi figyelembe. Verzió módosítások csak akkor egyre növekvő tendenciát mutat.
6. **Operációs rendszer virtuális URL-címe** mezőbe a virtuális operációs rendszer által létrehozott URI átengedése aláírás hozzáadása, majd mentse a módosításokat.

    >[AZURE.IMPORTANT] Azt nem növelése vagy csökkentése felsorolt Termékváltozatot adatok lemez számát. Új Termékváltozatot ebben az esetben létrehozásához szükséges. Olvassa el a szakasz [3. Hogyan lehet hozzáadni új Termékváltozatot csoportban a felsorolt ajánlat](#3-how-to-add-a-new-sku-under-a-live-offer) részletes útmutatást.

7. A módosítások elvégzése után nyissa meg a **Közzététel** fülre, majd kattintson a **LEKÜLDÉSES a fejlesztői**gombra. Az ajánlat teszteli a fejlesztői környezet részletes útmutatást a nézze meg ezt a [hivatkozást](marketplace-publishing-vm-image-test-in-staging.md)
8. Az ajánlat tesztelése a fejlesztői, után nyissa meg a **Közzététel** lapon kattintson a közzététel portál, és kattintson a **Kérelem jóváhagyási a LEKÜLDÉSES a gyártási** tegye közzé újra a Microsoft Azure piactéren található az ajánlat gombra.

    ![rajz](media/marketplace-publishing-vm-image-post-publishing/img01_07.png)

## <a name="2-how-to-update-the-non-technical-details-of-an-offer-or-a-sku"></a>2. hogyan felajánlás vagy Termékváltozatot nem technikai jellegű részleteinek módosítása

Frissítheti a nem technikai jellegű (marketing, jogi, támogatja, a kategóriák) az élő ajánlat vagy a Microsoft Azure piactéren található Termékváltozat részleteit.

### <a name="21-update-the-offer-description-and-logos"></a>2.1 ajánlat leírása és emblémák frissítése

Frissítse az ajánlat adatait, és tegye közzé újra az ajánlatot az alábbi lépéseket követve:

1. Jelentkezzen be a [közzétételi portál](https://publish.windowsazure.com).
2. Nyissa meg azt a **virtuális gépeken FUTÓ** fülre, és válassza a felajánlást.
3. A bal oldali menüben kattintson a **MARKETINGTEVÉKENYSÉG** lapon.
4. Kattintson a **Angol (Egyesült Államokbeli)** gombra.
5. A bal oldali menüben kattintson a **Részletek** lapon. A **Részletek** fülre, a *Leírás* szakaszban frissítheti az ajánlat címe, ajánlat összegzése, hosszú összefoglaló ajánlat és a módosítások mentéséhez.

    >[AZURE.NOTE] Kérjük, gondoskodik a következő közben a frissíteni kívánt Termékváltozat részleteit.
    **Az ajánlat leírást és a Termékváltozat leírást a megkettőződött szöveg nem kell megadni. Nem kell megadni a Termékváltozat címét és a hosszú összefoglaló ajánlatot megkettőződött szöveg. Nem kell megadni a Termékváltozat címét és az ajánlat összefoglaló megkettőződött szöveg.**

    ![rajz](media/marketplace-publishing-vm-image-post-publishing/img02.1_05.png)

6. A **Részletek** lapon *emblémák* csoportjában az emblémákat frissíthetők. Jó helyen jár, győződjön meg arról, hogy a emblémák kövesse az [Azure piactéren elérhető irányelvek](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content) (a szakaszban lépés 1: piactér adja meg a tartalom marketing-Részletek > -> Azure Piactér embléma útmutató).

    >[AZURE.NOTE] Fő kép ikonra a lépés nem kötelező. Megadhatja, hogy ne töltse fel a fő ikon. Miután feltöltött fő ikonra, majd van azonban nem rendelkezik törlése a közzétételi portál. Ebben az esetben követniük kell a [fő ikon irányelvek](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content) (a szakaszban lépés 1: piactér adja meg a tartalom marketing-Részletek > További útmutató a fő embléma transzparens ->).

7. A módosítások elvégzése után nyissa meg a **Közzététel** fülre, majd kattintson a **LEKÜLDÉSES a fejlesztői**gombra. Az ajánlat teszteli a fejlesztői környezet részletes útmutatást a nézze meg ezt a [hivatkozást](marketplace-publishing-vm-image-test-in-staging.md).
8. Az ajánlat tesztelése a fejlesztői, után nyissa meg a **Közzététel** lapon kattintson a közzététel portál, és kattintson a **Kérelem jóváhagyási a LEKÜLDÉSES a gyártási** tegye közzé újra a Microsoft Azure piactéren található az ajánlat gombra.

    ![rajz](media/marketplace-publishing-vm-image-post-publishing/img02.1_08.png)

### <a name="22-update-the-sku-description"></a>2.2. A Termékváltozat leírásának frissítése

A Termékváltozat a frissítésről, és tegye közzé újra az ajánlatot az alábbi lépéseket követve:

1. Jelentkezzen be a [közzétételi portál](https://publish.windowsazure.com)
2. Nyissa meg azt a **virtuális gépeken FUTÓ** fülre, és válassza a felajánlást.
3. A bal oldali menüben kattintson a **MARKETINGTEVÉKENYSÉG** lapon.
4. Kattintson a **Angol (Egyesült Államokbeli)** gombra.
5. A bal oldali menüben kattintson a **CSOMAGVÁLTÁS** lapon. A **tervek** lap *termékváltozatok* szakaszban Termékváltozat címét, Termékváltozat összefoglaló és Termékváltozat leírása részletes frissítése, majd mentse a módosításokat.

    >[AZURE.NOTE] Kérjük, gondoskodik a következő közben a frissíteni kívánt Termékváltozat részleteit. **Az ajánlat leírást és a Termékváltozat leírást a megkettőződött szöveg nem kell megadni. A cím a Termékváltozat és a hosszú összefoglaló ajánlatot megkettőződött szöveg nem kell megadni. Nem kell megadni a Termékváltozat címét és az ajánlat összefoglaló megkettőződött szöveg.**

6. A módosítások elvégzése után nyissa meg a **Közzététel** fülre, majd kattintson a **LEKÜLDÉSES a fejlesztői**gombra. Az ajánlat teszteli a fejlesztői környezet részletes útmutatást a nézze meg ezt a hivatkozást
7. Az ajánlat tesztelése a fejlesztői, után nyissa meg a **Közzététel** lapon kattintson a közzététel portál, és kattintson a **Kérelem jóváhagyási a LEKÜLDÉSES a gyártási** tegye közzé újra a Microsoft Azure piactéren található az ajánlat gombra.

    ![rajz](media/marketplace-publishing-vm-image-post-publishing/img02.2_07.png)

### <a name="23-change-the-existing-links-or-add-new-links"></a>2.3 módosíthatja a meglévő hivatkozások, vagy új hivatkozások hozzáadása

Módosíthatja a meglévő hivatkozások vagy új hivatkozások hozzáadása, és ezután tegye közzé újra az ajánlatot az alábbi lépéseket követve:

1. Jelentkezzen be a [közzétételi portál](https://publish.windowsazure.com)
2. Nyissa meg azt a **virtuális gépeken FUTÓ** fülre, és válassza a felajánlást.
3. A bal oldali menüben kattintson a **MARKETINGTEVÉKENYSÉG** lapon.
4. Kattintson a **Angol (Egyesült Államokbeli)** gombra.
5. A bal oldali menüben kattintson a **hivatkozások** lapon.
6. Ha szeretné az új hivatkozás hozzáadása, majd a *hivatkozások* szakaszban kattintson a **Hivatkozás hozzáadása** gombra. A *"hivatkozás hozzáadása"* párbeszédpanel megnyitása A párbeszédpanelen adja hozzá a hivatkozás címe és URL-címe mezők, majd mentse a módosításokat. Megadhatja, hogy minden olyan hivatkozásra, amely segíthet az ügyfelek, információkat tartalmaz.
7. Ha azt szeretné, frissítése vagy egy meglévő hivatkozás törlése, majd jelölje ki a megfelelő hivatkozást és ennek megfelelően kattintson a Szerkesztés gombra, vagy a Törlés gombra.

    >[AZURE.NOTE] Győződjön meg arról, hogy a hivatkozásokat, amely ebben a részben megadott módon működnek-megfelelően, ezeket a hivatkozásokat első érvényesíteni a gyártási kérelem folyamat során.

8. A módosítások elvégzése után nyissa meg a **Közzététel** fülre, majd kattintson a **LEKÜLDÉSES a fejlesztői**gombra. Az ajánlat teszteli a fejlesztői környezet részletes útmutatást a nézze meg ezt a [hivatkozást](marketplace-publishing-vm-image-test-in-staging.md).
9. Az ajánlat tesztelése a fejlesztői, után nyissa meg a **Közzététel** lapon kattintson a közzététel portál, és kattintson a **Kérelem jóváhagyási a LEKÜLDÉSES a gyártási** tegye közzé újra a Microsoft Azure piactéren található az ajánlat gombra.

    ![rajz](media/marketplace-publishing-vm-image-post-publishing/img02.3_09-01.png)

    ![rajz](media/marketplace-publishing-vm-image-post-publishing/img02.3-2.png)

### <a name="24-change-an-existing-sample-image-or-add-a-new-sample-image"></a>2.4 módosítása meglévő minta kép vagy az új minta beállítása

Egy meglévő minta képek módosítása vagy hozzáadása az új minta képek, és ezután tegye közzé újra az ajánlatot az alábbi lépéseket követve:

>[AZURE.NOTE] Csak egy mintán kép megjelenik a [https://portal.azure.com](https://portal.azure.com).

1. Jelentkezzen be a [közzétételi portál](https://publish.windowsazure.com)
2. Nyissa meg azt a **virtuális gépeken FUTÓ** fülre, és válassza a felajánlást.
3. A bal oldali menüben kattintson a **MARKETINGTEVÉKENYSÉG** lapon.
4. Kattintson a **Angol (Egyesült Államokbeli)** gombra.
5. A bal oldali menüben kattintson a **Minta képek** fülre.
6. Szeretné az új minta beállítása, ha a *Minta képek* csoportban kattintson az **Új kép FELTÖLTÉSE** gombra, és mentse a módosításokat.

    >[AZURE.NOTE] Egy minta kép együtt (nem kötelező).

7. Ha szeretné módosítani vagy törölni a minta meglévő képet, majd keresse meg a megfelelő mintaképe, és kattintson a **Kép cseréje** vagy a Törlés gombra, majd kattintson ennek megfelelően.

8. A módosítások elvégzése után nyissa meg a **Közzététel** fülre, majd kattintson a **LEKÜLDÉSES a fejlesztői**gombra. Az ajánlat teszteli a fejlesztői környezet részletes útmutatást a nézze meg ezt a [hivatkozást](marketplace-publishing-vm-image-test-in-staging.md).
9. Az ajánlat tesztelése a fejlesztői, után nyissa meg a **Közzététel** lapon kattintson a közzététel portál, és kattintson a **Kérelem jóváhagyási a LEKÜLDÉSES a gyártási** tegye közzé újra a Microsoft Azure piactéren található az ajánlat gombra.

    ![rajz](media/marketplace-publishing-vm-image-post-publishing/img02.4_09.png)

### <a name="25-update-the-legal-content"></a>2,5 frissíti a jogi

Frissíti a jogi, és tegye közzé újra az ajánlatot az alábbi lépéseket követve:

1. Jelentkezzen be a [közzétételi portál](https://publish.windowsazure.com)
2. Nyissa meg azt a **virtuális gépeken FUTÓ** fülre, és válassza a felajánlást.
3. A bal oldali menüben kattintson a **MARKETINGTEVÉKENYSÉG** lapon.
4. Kattintson a **Angol (Egyesült Államokbeli)** gombra.
5. A bal oldali menüben kattintson a **jogi** lapon. A *jogi* szakaszban frissítheti a szabályok és használati feltételei. Írja be vagy illessze be a szabályok feltételei a *Használati feltételek* mezőben lévő értéket, majd mentse a módosításokat.
6. A jogi feltételei karakter korlát 1,000,000 karaktereket.
7. A módosítások elvégzése után nyissa meg a **Közzététel** fülre, majd kattintson a **LEKÜLDÉSES a fejlesztői**gombra. Az ajánlat teszteli a fejlesztői környezet részletes útmutatást a nézze meg ezt a [hivatkozást](marketplace-publishing-vm-image-test-in-staging.md)
8. Az ajánlat tesztelése a fejlesztői, után nyissa meg a **Közzététel** lapon kattintson a közzététel portál, és kattintson a **Kérelem jóváhagyási a LEKÜLDÉSES a gyártási** tegye közzé újra a Microsoft Azure piactéren található az ajánlat gombra.

    ![rajz](media/marketplace-publishing-vm-image-post-publishing/img02.5_08.png)

### <a name="26-update-the-support-information"></a>2,6 a terméktámogatási információk frissítése

A terméktámogatási információk frissítése, és tegye közzé újra az ajánlatot az alábbi lépéseket követve:

1. Jelentkezzen be a [közzétételi portál](https://publish.windowsazure.com)
2. Nyissa meg azt a **virtuális gépeken FUTÓ** fülre, és válassza a felajánlást.
3. A bal oldali menüben kattintson a **támogatás** fülre.
4. A **Részletek** lapon *Mérnöki forduljon* csoportban frissítheti a kapcsolattartási adatait. Ezek az adatok csak a partnerek és a Microsoft között belső kommunikációhoz segítségével.
5. Az *Ügyfélszolgálat* területen a **támogatási** lap frissítheti a támogatási kapcsolattartási adatai, például a **nevét, az e-mailek, a telefon** és a **Támogatási URL-CÍMÉT**. Ezek az adatok csak a partnerek és a Microsoft között belső kommunikációhoz segítségével.

    >[AZURE.NOTE] Szeretne csak e-mailek támogatottak, ha adja meg a **Támogatási szolgálatát** szakaszban üres telefonszámot. Ebben az esetben a megadott e-mail lesz helyette.

6. A módosítások elvégzése után nyissa meg a **Közzététel** fülre, majd kattintson a **LEKÜLDÉSES a fejlesztői**gombra. Az ajánlat teszteli a fejlesztői környezet részletes útmutatást a nézze meg ezt a [hivatkozást](marketplace-publishing-vm-image-test-in-staging.md)
7. Az ajánlat tesztelése a fejlesztői, után nyissa meg a **Közzététel** lapon kattintson a közzététel portál, és kattintson a **Kérelem jóváhagyási a LEKÜLDÉSES a gyártási** tegye közzé újra a Microsoft Azure piactéren található az ajánlat gombra.

    ![rajz](media/marketplace-publishing-vm-image-post-publishing/img02.6_07.png)

### <a name="27-update-the-categories"></a>2.7 a kategóriák frissítése

Frissítés a Microsoft a Kategóriák szakaszban az ajánlat, és tegye közzé újra az ajánlatot az alábbi lépéseket követve:

1. Jelentkezzen be a [közzétételi portál](https://publish.windowsazure.com)
2. Nyissa meg azt a **virtuális gépeken FUTÓ** fülre, és válassza a felajánlást.
3. A bal oldali menüben kattintson a **Kategóriák** fülre.
4. A *Kategóriák* csoportban a kategóriák frissítése az ajánlatra, majd mentse a módosításokat. Microsoft Azure piactéren legfeljebb öt kategóriák is választhat.
5. A módosítások elvégzése után nyissa meg a **Közzététel** fülre, majd kattintson a **LEKÜLDÉSES a fejlesztői**gombra. Az ajánlat teszteli a fejlesztői környezet részletes útmutatást a nézze meg ezt a [hivatkozást](marketplace-publishing-vm-image-test-in-staging.md)
6. Az ajánlat tesztelése a fejlesztői, után nyissa meg a **Közzététel** lapon kattintson a közzététel portál, és kattintson a **Kérelem jóváhagyási a LEKÜLDÉSES a gyártási** tegye közzé újra a Microsoft Azure piactéren található az ajánlat gombra.

    ![rajz](media/marketplace-publishing-vm-image-post-publishing/img02.7_06.png)

## <a name="3-how-to-add-a-new-sku-under-a-listed-offer"></a>3. a következőképpen adhatja egy felsorolt ajánlatot az új Termékváltozatot

Az alábbi lépésekkel adhat hozzá új Termékváltozatot az élő ajánlat csoportban:

1. Jelentkezzen be a [közzétételi portál](https://publish.windowsazure.com).
2. Nyissa meg azt a **virtuális gépeken FUTÓ** fülre, és válassza a felajánlást.
3. A bal oldali menüben kattintson a **termékváltozatok** lapon. Miután, kattintson a **Hozzáadás A Termékváltozat**gombra.  Ekkor megnyílik egy új párbeszédpanel. Írja be a Termékváltozat azonosítóját kisbetűket. Ha az új Termékváltozat BYOL számlázási modellel közzé szeretné tenni, jelölje be a jelölőnégyzetet a számlázási model(BYOL) hozása-a-saját. Ellenkező esetben törölje a jelet a jelölőnégyzetből a BYOL. Miután, kattintson az osztásjelek hozhat létre egy új Termékváltozat a párbeszédpanelen. Ha úgy did nem dönt BYOL számlázási modell az új Termékváltozat a, majd a számlázási modell automatikusan állítja be óránkénti az új SKU. 30days a próba-előfizetésre óránkénti számlázási modell engedélyezni szeretné, ha majd kattintson az "Egy hónappal" lehetőség a "érhető el az ingyenes próbaverzióra?". Egyéb esetben a próbaidőszak "nem". [Megjegyzés: A "lehetőség a rendelkezésre álló ingyenes próbaverziót?" csak akkor jelenik meg ha nem jelölt BYOL a párbeszédpanelen az új Termékváltozat létrehozásakor.]

    >[AZURE.IMPORTANT] A beállítás "Elrejtése a Termékváltozat a piactérről, mert meg kell mindig vásárolható keresztül megoldás sablon" kell olvasottként megjelölve, "Igen" csak akkor, ha azt jóváhagyásra a megoldást sablon ajánlat közzététel a a Microsoft Azure piactéren. Egyéb esetben ezt a beállítást kell mindig lesznek olvasottként megjelölve, "Nem".

4. Most már a bal oldali menüben kattintson a **Virtuális képek** fülre, és meg, hogy az új Termékváltozat, amelyek nem hozott létre.
5. Az új Termékváltozat beállítani, olvassa el a lépés 5-útmutatás [hivatkozásra](marketplace-publishing-vm-image-creation.md#5-obtain-certification-for-your-vm-image) .
6. Az új Termékváltozat marketingtevékenység anyagot felvételéhez olvassa el a következő részt lépés 1: piactér adja meg a tartalom marketing-Részletek > pont számok 2 – 5 a [hivatkozás](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content)->.
7. Adja meg az új Termékváltozat árak adatait, olvassa el az című 2.1 parancsot. A virtuális árak, a [hivatkozás](marketplace-publishing-push-to-staging.md#step-2-set-your-prices)
8. A módosítások elvégzése után nyissa meg a **Közzététel** fülre, majd kattintson a **LEKÜLDÉSES a fejlesztői**gombra. Az ajánlat teszteli a fejlesztői környezet részletes útmutatást a nézze meg ezt a [hivatkozást](marketplace-publishing-vm-image-test-in-staging.md)
9. Az ajánlat tesztelése a fejlesztői, után nyissa meg a **Közzététel** lapon kattintson a közzététel portál, és kattintson a **Kérelem jóváhagyási a LEKÜLDÉSES a gyártási** tegye közzé újra a Microsoft Azure piactéren található az ajánlat gombra.

    ![rajz](media/marketplace-publishing-vm-image-post-publishing/img03_09-01.png)

    ![rajz](media/marketplace-publishing-vm-image-post-publishing/img03_09-02.png)

## <a name="4-how-to-change-the-data-disk-count-for-a-listed-sku"></a>4. hogyan a felsorolt Termékváltozatot lemez adatok számának módosítása

Azt nem növelése vagy csökkentése felsorolt Termékváltozatot adatok lemez számát. Ebben az esetben szüksége van egy új Termékváltozatot létrehozása. Olvassa el a szakasz [3. Hogyan lehet hozzáadni új Termékváltozatot egy élő ajánlat a](#3-how-to-add-a-new-sku-under-a-live-offer) részletes útmutatást.

## <a name="5---how-to-delete-a-listed-offer-from-the-azure-marketplace"></a>5. a felsorolt ajánlat törlése a Microsoft Azure piactéren lévő hogyan

Vannak olyan lehet venni kezeli abban az esetben, ha egy kérelmet, ha el szeretne távolítani egy élő ajánlatot kell különböző szempontok alapján. Hajtsa végre az alábbi lépésekkel úgy juthat az útmutató a támogatási csoport, ha el szeretne távolítani egy felsorolt ajánlatot az Azure piactéren elérhető:

1.  A [hivatkozás](https://support.microsoft.com/en-us/getsupport?wf=0&tenant=ClassicCommercial&oaspworkflow=start_1.0.0.0&locale=en-us&supportregion=en-us&pesid=15635&ccsid=635993707583706681) támogatási jegy előléptetése
2.  Probléma adja mint **"Kezelése ajánlatok"** , és válassza a kategória **"Ajánlat és/vagy a már a termelési Termékváltozat módosítása"**
3.  A kérelem elküldése

A támogatási csoport végigvezeti Önt a ajánlat/Termékváltozat törlési folyamat.

>[AZURE.NOTE] Mindig törölheti az ajánlat a piszkozat állapota (tehát nem a fejlesztői vagy gyártási) csoportban az **Előzmények** lap **ELVETÉSE piszkozat** gombjára kattintva.

## <a name="6-how-to-delete-a-listed-sku-from-the-azure-marketplace"></a>6. a felsorolt Termékváltozat törlése a Microsoft Azure piactéren lévő hogyan

A Microsoft Azure piactéren lévő felsorolt Termékváltozatot törölheti az alábbi lépésekkel:

1. Jelentkezzen be a [közzétételi portál](https://publish.windowsazure.com).
2. Nyissa meg azt a **virtuális gépeken FUTÓ** fülre, és válassza a felajánlást.
3. A bal oldali munkaablak kattintson a **Termékváltozatok** lapon.
4. Jelölje ki a Termékváltozat, amelyeket szeretne törölni, és kattintson a Törlés gombra, hogy Termékváltozat szemben.
5. Miután elvégezte az eltávolítást, nyissa meg a közzététel lapon kattintson a közzététel portálra, és kattintson a **Kérelem jóváhagyási a LEKÜLDÉSES a gyártási** tegye közzé újra az ajánlatot az Azure piactéren lévő gombra.
6. Miután az ajánlatot az Azure piactéren elérhető újra közzétett kap, a Termékváltozat törlődik az Azure piactéren elérhető és az Azure-portálra.

## <a name="7-how-to-delete-the-current-version-of-a-listed-sku-from-the-azure-marketplace"></a>7. az aktuális verziójának felsorolt Termékváltozatot törlése a Microsoft Azure piactéren lévő hogyan

A Microsoft Azure piactéren lévő felsorolt Termékváltozatot aktuális verziójának törölheti az alábbi lépéseket követve. A folyamat befejezése után a Termékváltozat fog állítható vissza a korábbi verzióját.

1. Jelentkezzen be a [közzétételi portál](https://publish.windowsazure.com).
2.  Nyissa meg azt a **virtuális gépeken FUTÓ** fülre, és válassza a felajánlást.
3.  A bal oldali munkaablak kattintson a **Virtuális képek** fülre.
4.  Jelölje ki a Termékváltozat, amelynek meg szeretné törölni, majd kattintson a Törlés gombon ellen verzióval korábbit.
5.  Miután elvégezte az eltávolítást, nyissa meg a **Közzététel** lapon kattintson a közzététel portálra, és kattintson a **Kérelem jóváhagyási a LEKÜLDÉSES a gyártási** tegye közzé újra az ajánlatot az Azure piactéren lévő gombra.
6.  Miután az ajánlatot az Azure piactéren elérhető újra közzétett kap, a felsorolt Termékváltozat aktuális verziójának törlődik az Azure piactéren elérhető és az Azure-portálra. A Termékváltozat fog állítható vissza a korábbi verzióját.

## <a name="8-how-to-revert-listing-price-to-production-values"></a>8. listaelem ár gyártási értékekre vissza hogyan
Lehet módosítani a felsorolt Termékváltozatot árak (vagy lehet felsorolt Termékváltozatot számlázási területe eltávolítása). Mivel az nem támogatja a Microsoft Azure piactéren található, szeretném visszaállítása: a módosítások gyártási értékekre. Hogyan elérése, hogy?

Hajtsa végre az alábbi lépéseket:

1. Jelentkezzen be a [közzétételi portál](https://publish.windowsazure.com).
2. Nyissa meg azt a **virtuális gépeken FUTÓ** fülre, és válassza a felajánlást.
3. A bal oldali menüben kattintson a **árak** fülre.
4. Az árak lapon jelölje be a régió, amelynek árak alaphelyzetbe.

    ![rajz](media/marketplace-publishing-vm-image-post-publishing/img08-04.png)

5. Abban az esetben, ha az óránkénti számlázási modellel termékváltozatban mint a a kijelölt terület gyártási alaphelyzetbe állítása az összes mag árait. Az termékváltozatok és BYOL számlázási modellt, elérhetővé teheti a Termékváltozat a tartományban lévő szemben a Termékváltozat EXTERNALLY-LICENSED (BYOL) Termékváltozat elérhetősége szakaszban a jelölőnégyzet bejelölésével (lásd a lenti képernyőképet).

    ![rajz](media/marketplace-publishing-vm-image-post-publishing/img08-05.png)

6. Kattintson a gombra **AUTOPRICE egyéb piacokon alapú tovább árak az Egyesült Államok**.

    >[AZURE.NOTE] Lehet, hogy a gombot címke eltérő attól függően, hogy a tartományban, amely kiválasztása után. Mivel az Amerikai Egyesült Államok azt van jelölve a dokumentum létrehozásakor, így a gomb felirata mint "Automatikus ár más alapján az Amerikai Egyesült Államokban árak piacokon" az alábbi képernyőképen.

    ![rajz](media/marketplace-publishing-vm-image-post-publishing/img08-06.png)

7. Az automatikus ár varázsló nyílik meg. Az első oldalra a kijelölés alap piacra szánt jeleníti meg. Ellenőrizze a szakaszt, és a **"->"** gombra kattintva helyezze át a következő lapjára.

    ![rajz](media/marketplace-publishing-vm-image-post-publishing/img08-07.png)

8. Beállítás a magmintákat és a csomagok jelennek meg a lapon a 2. Jelölje ki a kívánt tervek és a magmintákat, majd kattintson a "->" gomb.

    ![rajz](media/marketplace-publishing-vm-image-post-publishing/img08-08.png)

9. Lap 3 piacokon/régiók jeleníti meg. Az összes kapcsolót gombra kattintva jelölje ki a rekordok, amelyekben terület, vagy manuálisan jelölőnégyzetből területhez tartozik. Kattintson az Ugrás a következő oldal "->" gombra.

    ![rajz](media/marketplace-publishing-vm-image-post-publishing/img08-09.png)

10. Lap 4 árfolyam jeleníti meg. Kattintson a lépések elvégzéséhez a Befejezés gombra. A varázsló beállításától függően árak alaphelyzetbe állítása.

11. Most már nyissa meg azt a árak fülre, majd kattintson az "Összefoglalás és végrehajtott változtatások megjelenítése" gombra.
Válassza ki a "Vázlat" a "Verzió megtekintése" és "Gyártási" a "Összehasonlítása" szakaszban (lásd a lenti képernyőképet). Ha nincs árak különbség című feltételezi, árak lett visszatért a termelési értékek sikeresen.

    ![rajz](media/marketplace-publishing-vm-image-post-publishing/img08-11.png)

12. A módosítások elvégzése után nyissa meg a közzététel fülre, majd kattintson a **LEKÜLDÉSES a fejlesztői**gombra. Az ajánlat teszteli a fejlesztői környezet részletes útmutatást a nézze meg ezt a [hivatkozást](marketplace-publishing-vm-image-test-in-staging.md)
13. Az ajánlat tesztelése a fejlesztői, után nyissa meg a közzététel lapon kattintson a közzététel portál, és kattintson a **Kérelem jóváhagyási a LEKÜLDÉSES a gyártási** tegye közzé újra a Microsoft Azure piactéren található az ajánlat gombra.

## <a name="9-how-to-revert-billing-model-to-production-values"></a>9. számlázási modell gyártási értékekre vissza hogyan
E felsorolt Termékváltozatot számlázási mintája megváltoztak. Mivel az nem támogatja a Microsoft Azure piactéren található, szeretném visszaállítása: a módosítások gyártási értékekre. Hogyan elérése, hogy?

Kérjük, kövesse az alábbi lépéseket:

1. Jelentkezzen be a [közzétételi portál](https://publish.windowsazure.com).
2. Nyissa meg azt a **virtuális gépeken FUTÓ** fülre, és válassza a felajánlást.
3. A bal oldali menüben kattintson a **Termékváltozatok** fülre.
4. Kattintson a SZERKESZTÉS gombra a számlázási modell vissza. Egy ablakban nyílik meg. Jelölje be vagy törölje belőle a jelet a **"számlázás és a licencek befejeződött módjáról az Azure (más néven hozása: A saját licenc)"** jelölőnégyzet megfelelően.

    ![rajz](media/marketplace-publishing-vm-image-post-publishing/img09-04.png)

5. Egyszer olvassa el a válasz, a kérdés 8 kattintva térjen vissza az ár a dokumentumban.
6. Miután, nyissa meg a **Közzététel** lapon kattintson a közzététel-portál és -leküldéses ajánlat átmeneti tárolására kattintva tesztelje. Egyszer akkor végzett az ajánlatra vonatkozó tesztelés, majd kattintson a **Kérelem jóváhagyási a LEKÜLDÉSES a gyártási** tegye közzé újra a Microsoft Azure piactéren található az ajánlat gombra.

## <a name="10-how-to-revert-visibility-setting-of-a-listed-sku-to-the-production-value"></a>10. vissza a termelési értékre felsorolt Termékváltozatot a láthatósági beállítás hogyan

Kérjük, kövesse az alábbi lépéseket:

1. Jelentkezzen be a [közzétételi portál](https://publish.windowsazure.com).
2. Nyissa meg azt a **virtuális gépeken FUTÓ** fülre, és válassza a felajánlást.
3. A bal oldali menüben kattintson a **Termékváltozatok** fülre.
4. Jelölje ki a Termékváltozat, és visszaállíthatja a Termékváltozat a termelési értékre láthatóságának beállítása.

    ![rajz](media/marketplace-publishing-vm-image-post-publishing/img10-04.png)

5. Egyszer akkor végzett módosításokat, majd kattintson a **Kérelem jóváhagyási a LEKÜLDÉSES a gyártási** tegye közzé újra a Microsoft Azure piactéren található az ajánlat gombra.

## <a name="see-also"></a>Lásd még:
- [Első lépések: Útmutató felajánlás közzététele az Azure piactérről](marketplace-publishing-getting-started.md)
- [Jelentéskészítés eladó háttérismeretek ismertetése](marketplace-publishing-report-seller-insights.md)
- [Kifizetés jelentéskészítés ismertetése](marketplace-publishing-report-payout.md)
- [A felhőalapú megoldás szolgáltató viszonteladói arra ösztönzi módosítása](marketplace-publishing-csp-incentive.md)
- [A piactér közös közzétételi problémák elhárítása](marketplace-publishing-support-common-issues.md)
- [Technikai támogatás kérése, a Publisherben](marketplace-publishing-get-publisher-support.md)
- [A virtuális képe a helyszíni létrehozása](marketplace-publishing-vm-image-creation-on-premise.md)
- [Windows operációs rendszert futtató az Azure előzetes portálon virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md)

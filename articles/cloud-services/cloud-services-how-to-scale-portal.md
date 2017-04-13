<properties
    pageTitle="Automatikus méretezés egy felhőalapú szolgáltatásba a portálon |} Microsoft Azure"
    description="Tudnivalók a portál használatával állítsa be az automatikus méretezés szabályok egy felhőalapú szolgáltatás webes szerepkör vagy dolgozó szerepkör Azure-ban."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="how-to-auto-scale-a-cloud-service"></a>Hogyan kell egy felhőalapú szolgáltatásba automatikus méretezése

> [AZURE.SELECTOR]
- [Azure portál](cloud-services-how-to-scale-portal.md)
- [Azure klasszikus portál](cloud-services-how-to-scale.md)

Feltételek beállítható, hogy egy felhőalapú szolgáltatás dolgozó szerepkör vagy kicsinyítés művelet méretaránnyal kiváltó. A Processzor, lemezre vagy hálózati terhelést a szerepkör a szerepkör vonatkozó feltételeket is alapján. Is beállíthatja, hogy egy conditation, egy üzenet várólista vagy a mérőszám az előfizetéséhez társított néhány egyéb Azure erőforrás alapján.

>[AZURE.NOTE] Ez a cikk a Felhőbeli szolgáltatástól webes és dolgozó szerepkörök koncentrál. Virtuális gép (klasszikus) közvetlenül létrehozásakor a egy felhőalapú szolgáltatás üzemelteti. Egy szabványos virtuális gép méretezni az [elérhetőség beállítása](../virtual-machines/virtual-machines-windows-classic-configure-availability.md) társításával, és kézi őket be- és kikapcsolása.

## <a name="considerations"></a>Megfontolandó szempontok

Az alkalmazás méretezési beállítása előtt vegye figyelembe az alábbi adatokat:

- Méretezés core használatát érinti. Nagyobb szerepkör-példányok további magmintákat használja. Az alkalmazás csak magmintákat belül az előfizetéshez tartozó méretezheti. Például ha előfizetése van húsz magmintákat, és az alkalmazások még a két közepes, melynek a mérete legfeljebb cloud services (négy magmintákat összesen), csak méretezheti be más felhőalapú szolgáltatás telepítés esetén az előfizetésben 16 magminták által. Lásd: a [Felhőalapú szolgáltatás méretű](cloud-services-sizes-specs.md) méretű kapcsolatban további tudnivalókat.

- Méretezheti várólista üzenet küszöbértéket alapján. Sorok használatával kapcsolatos további tudnivalókért lásd: [a várólista tároló szolgáltatás használata](../storage/storage-dotnet-how-to-use-queues.md).

- Más erőforrások: az előfizetéséhez társított is méretezheti.

- Ahhoz, hogy az alkalmazás elérhetősége magas, győződjön meg a két vagy több szerepkör-példányok rendszerbe. További tudnivalókért olvassa el a [Szolgáltatás szint rendelkezést](https://azure.microsoft.com/support/legal/sla/)című témakört.

## <a name="where-scale-is-located"></a>Hol található az skála

Miután kiválasztotta a felhőalapú szolgáltatás, rendelkeznie kell a felhőalapú szolgáltatást a lap látható.

1. Válassza a felhőalapú szolgáltatás lap a **szerepkörök és a példányok** csempét a felhőbeli szolgáltatástól nevét.   
**Fontos**: Ügyeljen arra, hogy a felhőalapú szolgáltatás szerepkört, nem az szerepkör-példányt, amely alatt a szerepkör gombra.

    ![](./media/cloud-services-how-to-scale-portal/roles-instances.png)

2. Kattintson a **Méretezés** csempére.

    ![](./media/cloud-services-how-to-scale-portal/scale-tile.png)

## <a name="automatic-scale"></a>Automatikus méretezés

Beállíthatja a méretarány-beállításai a szerepkör kétféleképpen **kézi** vagy **automatikus**. Kézi ahogy várható, beállíthatja, hogy az abszolút példányainak száma. Automatikus azonban lehetővé teszi, hogy szabályok, amelyek meghatározzák, hogy hogyan és hogyan sokkal meg kell méretezni.

A **skála** beállítást állítsa **ütemtervet és a teljesítmény szabályok**.

![Cloud services méretarány-beállításai a profil és a szabály](./media/cloud-services-how-to-scale-portal/schedule-basics.png)

1. Egy meglévő profilhoz.
2. A szülő-profil szabály hozzáadása.
3. Vegyen fel egy másik profilt.

Válassza a **profil hozzáadása**. A profil határozza meg, mely a méretezés a használni kívánt mód: **mindig**, **Ismétlődés**, **rögzített dátum**.

Miután beállította a profil- és szabályok, válassza a **Mentés** ikonra a képernyő tetején.

#### <a name="profile"></a>Profil

A profil állítja be a méretarány minimális és maximális példányok és is ha a tartomány aktív.

* **Mindig**

    Mindig a rendelkezésre álló példányok cellatartományt.  

    ![Felhőalapú szolgáltatás, amelyek mindig méretezése](./media/cloud-services-how-to-scale-portal/select-always.png)
    
* **Ismétlődés**

    Ha át kívánja méretezni a hét napjai készletének kiválasztása.

    ![Felhőalapú szolgáltatás méretezés a Ismétlődés ütemezése](./media/cloud-services-how-to-scale-portal/select-recurrence.png)
    
* **Fix dátum**

    Rögzített dátumtartomány a szerepkör méretezni.

    ![Felhőalapú szolgáltatás méretezés a Fix dátum](./media/cloud-services-how-to-scale-portal/select-fixed.png)

Miután beállította a profillal, jelölje be a profil lap alján az **OK** gombra.

#### <a name="rule"></a>Szabály

Szabályok profilhoz kerülnek, és egy feltétel, amely elindítja a skála képviseli. 

A szabály eseményindító egy mérőszám (processzorhasználata, lemez tevékenységet vagy hálózati tevékenység) a felhőalapú szolgáltatás, amely is adhat hozzá egy feltételes értéket alapul. Emellett az eseményindító, egy üzenet várólista vagy a mérőszám az előfizetéséhez társított néhány egyéb Azure erőforrás alapján is.

![](./media/cloud-services-how-to-scale-portal/rule-settings.png)

Miután beállította a szabályt, jelölje be a szabály a lap alján az **OK** gombra.

## <a name="back-to-manual-scale"></a>Vissza a kézi méretezése

Nyissa meg a [Méretarány-beállításai](#where-scale-is-located) , majd **egy példány**darabra, amely kézzel beírja **a skála** megadása.

![Cloud services méretarány-beállításai a profil és a szabály](./media/cloud-services-how-to-scale-portal/manual-basics.png)

Ezzel eltávolítja a szerepkör az automatikus méretezés, és ezután beállíthatja, hogy a példányok száma közvetlenül. 

1. A Méretezés (Manuális vagy automatikus) lehetőséget.
2. A szerepkör példány csúszka húzásával állítsa be a példányokat, ha át kívánja méretezni, hogy.
3. Ha át kívánja méretezni, hogy a szerepkör példányát.

Miután beállította a méretarány-beállításai, válassza a **Mentés** ikonra a képernyő tetején.


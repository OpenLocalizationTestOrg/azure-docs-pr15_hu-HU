<properties
    pageTitle="Az Azure-portálon Azure keresési szolgáltatás létrehozása |} Microsoft Azure |} A felhőben tárolt keresési szolgáltatás"
    description="Megtudhatja, hogyan hozhatók létre az Azure-portálon Azure keresőszolgáltatás."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-service-using-the-azure-portal"></a>Az Azure-portálon Azure keresési szolgáltatás létrehozása

Ez az útmutató azt ismerteti, hogy azon a folyamaton, létrehozása (vagy a kiépítési) az [Azure-portálon](https://portal.azure.com/)Azure keresőszolgáltatás.

Ez az útmutató feltételezi, hogy már rendelkezik egy Azure-előfizetést, és jelentkezhet be az Azure-portálra.

## <a name="find-azure-search-in-the-azure-portal"></a>Azure keresési keresse meg az Azure-portálon
1. Az [Azure-portálra](https://portal.azure.com/) , és jelentkezzen be.
1. Kattintson a pluszjelre ("+") a bal felső sarokban.
2. Válassza az **adatok + tárhelyet**.
3. Válassza az **Azure keresése**.

![](./media/search-create-service-portal/find-search.png)

## <a name="pick-a-service-name-and-url-endpoint-for-your-service"></a>Válassza ki a szolgáltatás neve és a szolgáltatás URL-cím végpont
1. A szolgáltatás neve a Azure keresési szolgáltatás végpontjának URL-címe szemben, amely az API-hívások kezelése, és a keresési szolgáltatás használata teheti része lesz.
2. A service mezőbe írja be nevét az **URL-CÍMÉT** . A szolgáltatás neve:
  * csak tartalmaznia kell kisbetűket, számokat, illetve szaggatott vonal ("-")
  * nem használható a kötőjel ("-") az első 2 karaktereket vagy utolsó egyetlen karakter
  * nem tartalmazhat egymást követő szaggatott vonal ("–")
  * korlátozott 2 és 60 karakter hosszúságú között


## <a name="select-a-subscription-where-you-will-keep-your-service"></a>Jelöljön ki egy előfizetést, ahol megőrzi a szolgáltatásban
Ha egynél több előfizetése van, válassza a melyiket Azure keresés szolgáltatást fogja tartalmazni.

## <a name="select-a-resource-group-for-your-service"></a>Jelöljön ki egy erőforrás csoportot az Ön szolgáltatása
Hozzon létre egy új erőforráscsoport, vagy jelöljön ki egy meglévőt. Erőforráscsoport az Azure-szolgáltatások és a közösen használt erőforrások gyűjteménye. Például ha keresési Azure SQL-adatbázis index szolgáltatást használ, majd az alábbi szolgáltatások a részének kell lennie az azonos erőforráscsoport.

## <a name="select-the-location-where-your-service-will-be-hosted"></a>Jelölje ki a helyet, ahol a szolgáltatást tárolható
Azure szolgáltatásként Azure keresési adatközpont esetén a világon szerepeltethetők rendelkezésére áll. Felhívjuk a figyelmét arra, hogy [árak eltérő](https://azure.microsoft.com/pricing/details/search/) földrajzi.

## <a name="select-your-pricing-tier"></a>Jelölje ki a árak szint
[Azure keresési amely jelenleg árak többszintű](https://azure.microsoft.com/pricing/details/search/): szabad, egyszerű vagy szabványos. Minden réteg van a saját [kapacitás és korlátok](search-limits-quotas-capacity.md). [Válasszon egy árak réteg vagy Termékváltozat](search-sku-tier.md) útmutatást talál.

Ebben az esetben azt választotta, a normál réteg számára a szolgáltatás.

## <a name="select-the-create-button-to-provision-your-service"></a>Jelölje be a szolgáltatás kiépítése "Létrehozása" gomb

![](./media/search-create-service-portal/create-service.png)

## <a name="scale-your-service"></a>A szolgáltatás méretezése

A szolgáltatás már kiépítve, miután méretezni, hogy az igényeknek megfelelően. Ha úgy döntött, hogy a szabványos réteg az Azure keresési szolgáltatás, a szolgáltatás méretezheti két dimenzióban: replikák és partíciót. Ha úgy döntött, az egyszerű réteg, csak kópiák adhat hozzá.

*__A partíciók__* lehetővé teszik a szolgáltatásban mentheti és keresni az további dokumentumok.

*__Kópiák__* lehetővé teszi, hogy miként kezelje a keresési lekérdezések - [szolgáltatás 2 kópiák egy írásvédett SLA eléréséhez szükséges, és a 3 kópiák olvasási/írási szolgáltatásiszint-szerződés eléréséhez szükséges](https://azure.microsoft.com/support/legal/sla/search/v1_0/)magasabb terhelést a szolgáltatás.

1. Nyissa meg az Azure keresőszolgáltatás management lap az Azure-portálon.
2. Válassza a **Beállítások** lap **Méretezés**.
3. A szolgáltatás méretezheti kópiák vagy partíciót hozzáadásával.
  * A szolgáltatás múltbeli 36 keresési egységek nem méretezni. A keresés egységek száma a termék kópiák és partíciót (kópiák * partíciót = a teljes keresési egységek).
  * Ha úgy döntött, az egyszerű réteg, csak méretezheti 3 kópiákkal. Egyszerű szolgáltatások kötődnek egy partíciót.

![](./media/search-create-service-portal/scale-service.png)

## <a name="next"></a>Következő
-Az Azure keresési szolgáltatás kiépítése után fogja készen áll a [Azure keresési index megadása](search-what-is-an-index.md) így tölthet fel, és az adatok keresése.

Az [első lépések](search-get-started-portal.md) című az Azure-keresés a portálon rövid oktatóanyagot.

<properties
    pageTitle="Felhőbeli szolgáltatástól memóriafoglalási hiba elhárítása |} Microsoft Azure"
    description="Hibaelhárítás memóriafoglalási hiba, amikor rendszerbe állítják Cloud Services Azure-ban"
    services="azure-service-management, cloud-services"
    documentationCenter=""
    authors="simonxjx"
    manager="felixwu"
    editor=""
    tags="top-support-issue"/>

<tags
    ms.service="cloud-services"
    ms.workload="na"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="v-six"/>



# <a name="troubleshooting-allocation-failure-when-you-deploy-cloud-services-in-azure"></a>Hibaelhárítás memóriafoglalási hiba, amikor rendszerbe állítják Cloud Services Azure-ban

## <a name="summary"></a>Összefoglalás
Amikor példányok telepíteni egy felhőalapú szolgáltatásba, vagy új webhely vagy dolgozó szerepkör-példányok, a Microsoft Azure számítási erőforrások osztja ki. Hibák alkalmanként jelenhet meg ezek a műveletek végrehajtásakor, még akkor is határértékek elérése előtt már az Azure előfizetés. Ez a cikk ismerteti az egyes a gyakori hibák terhelés okait, és a javításukhoz javasolt lehetséges remediation. Az adatokat is is lehet hasznos, ha a szolgáltatások megtervezése.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

### <a name="background--how-allocation-works"></a>Háttér – terhelés működése
Azure adatközpont esetén a kiszolgálókat fürt vannak partícionálni. Új felhő terhelés szolgáltatáskérés tárcsáz, több fürtre. Ha telepítve van az első példányt felhőszolgáltatásokba (a fejlesztői vagy gyártási), amely a felhő szolgáltatás fürtre kiemelt kap. A felhőbeli szolgáltatástól bármilyen további telepítés a azonos fürt történik. Ebben a cikkben fogja hivatkozunk ez mint "fürtre a kiemelt". Az ábra 1 alatti szemlélteti a kis-és a normál terhelés, amely megpróbálkozik vele a rendszer a több fürtre; Az ábra 2 szemlélteti a kis-és-terhelés, amely magában kiemelt fürt 2, mert ez hol helyezkedik el a meglévő felhőalapú szolgáltatás CS_1.

![Felosztás Diagram](./media/cloud-services-allocation-failure/Allocation1.png)

### <a name="why-allocation-failure-happens"></a>Miért memóriafoglalási hiba történik
Amikor a kérelmet elosztási kitűzi fürt, akkor egy nagyobb valószínűséggel ingyenes információforrásokért, mivel a rendelkezésre álló erőforráskészlet korlátozódik fürt adatkapcsolat. Továbbá ha terhelés kérését kitűzi fürt, de fürtre nem támogatják a kért erőforrás típusát, a kérelem meghiúsul akkor is, ha a fürt ingyenes erőforrás. Diagram 3 alatt az esetben, ha egy rögzített terhelés nem sikerült, mivel a csak jelölt fürt nem ingyenes források mutatja be. Diagram 4 az esetben, ha egy rögzített terhelés végrehajtása meghiúsul, mert a csak jelölt fürt nem támogatja a kért virtuális méretét, akkor is, ha a fürt ingyenes források mutatja be.

![Rögzített memóriafoglalási hiba](./media/cloud-services-allocation-failure/Allocation2.png)

## <a name="troubleshooting-allocation-failure-for-cloud-services"></a>A felhőszolgáltatások memóriafoglalási hiba elhárítása
### <a name="error-message"></a>Hibaüzenet jelenik meg
Az alábbi hibaüzenet jelenhet meg:

    "Azure operation '{operation id}' failed with code Compute.ConstrainedAllocationFailed. Details: Allocation failed; unable to satisfy constraints in request. The requested new service deployment is bound to an Affinity Group, or it targets a Virtual Network, or there is an existing deployment under this hosted service. Any of these conditions constrains the new deployment to specific Azure resources. Please retry later or try reducing the VM size or number of role instances. Alternatively, if possible, remove the aforementioned constraints or try deploying to a different region."

### <a name="common-issues"></a>Gyakori kérdések
Az alábbiakban a felosztás esetei okozó terhelés felkérés egyetlen fürtre rögzíthető.

- Átmeneti tárolóhely - bevezetéshez, ha egy felhőalapú szolgáltatásba telepítés vagy tárolóhely, majd a teljes felhőalapú szolgáltatás ki van emelve egy adott fürthöz.  Ez azt jelenti, hogy ha a telepítés már létezik a termelési tárolóhely, majd egy új átmeneti tárolásra szolgáló telepítési csak kiosztható a azonos fürt, mint a termelési tárolóhely. Ha a fürtre van közelítő beosztását, a kérelem meghiúsulhat.

- Méretezés – új példányok hozzáadása egy meglévő felhőalapú szolgáltatást a azonos fürt kell foglalni.  Kis méretezés kérések általában hozzárendelhetők, de nem mindig. Ha a fürt van közelítő beosztását, a kérelem meghiúsulhat.

- Affinitás csoport – egy új telepítési, ha egy üres felhőalapú szolgáltatásba, is osztja el a háló bármely fürt az adott régióban kivéve, ha a felhőbeli szolgáltatástól rögzítve van egy affinitás csoportba. Az azonos fürt ugyanazon affinitás csoportjának telepítések történik kísérlet. Ha a fürt van közelítő beosztását, a kérelem meghiúsulhat.

- Affinitás csoport vNet - régebbi virtuális hálózatok affinitás csoportokba régiók helyett voltak tartozik, és a következő virtuális hálózatok felhőszolgáltatások volna kiemelt, a affinitás csoport fürthöz. Az ilyen típusú virtuális hálózati telepítések a lesz a rögzített fürt kísérlet történt. Ha a fürt van közelítő beosztását, a kérelem meghiúsulhat.

## <a name="solutions"></a>Megoldások

1. Új felhőszolgáltatásokba telepítsen újra – ez a megoldás valószínűleg leginkább sikeres lehetővé teszi, hogy az adott régióban minden fürt választhatja ki a platform.

    - A terhelést a ügyfélszámítógépekre való egy új, felhőalapú szolgáltatásba  

    - Frissítheti a CNAME vagy a rekord forgalom mutasson az új felhőalapú szolgáltatás

    - Miután nulla forgalmat a régi webhely fog, törölheti a régi felhőalapú szolgáltatást. Ez a megoldás nulla legrövidebb leállás kell járnak.

2. Gyártási és a helyek átmeneti törlése – Ez a megoldás megőrzi a meglévő tartománynév azonban hatására az alkalmazás legrövidebb leállás.

    - A gyártási és előkészítése a helyek egy meglévő felhőalapú szolgáltatás, hogy a felhőbeli szolgáltatástól argumentum üres, törlése és

    - Hozzon létre egy új telepítési a meglévő felhőalapú szolgáltatást. Ez újra próbálkozik felosztás és a tartományban lévő összes fürt. Gondoskodjon arról, hogy a felhőbeli szolgáltatástól nem tartozik egy affinitás csoportba.

3. Fenntartott IP - e megoldás megőrzi az meglévő IP-cím, de hatására az alkalmazás legrövidebb leállás.  

    - Hozzon létre egy fenntartott IP a meglévő telepítéshez Powershell használatával

    ```
    New-AzureReservedIP -ReservedIPName {new reserved IP name} -Location {location} -ServiceName {existing service name}
    ```

    - Kövesse a #2 feletti gondoskodhat arról, hogy a szolgáltatás CSCFG adhat meg az új fenntartott IP.

4. Távolítsa el affinitás csoport új telepítési - affinitás csoportok már nem ajánlott. Kövesse a fenti új felhőalapú szolgáltatás üzembe #1. Győződjön meg róla, felhőalapú szolgáltatás nem szerepel az egy affinitás csoportot.

5. Területi virtuális hálózati – lásd: [regionális virtuális hálózathoz (VNet) affinitás csoportok áttelepítése](../virtual-network/virtual-networks-migrate-to-regional-vnet.md)konvertálja.

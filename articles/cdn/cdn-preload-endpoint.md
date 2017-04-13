<properties
    pageTitle="Azure CDN-végpont eszközök előre betöltése |} Microsoft Azure"
    description="Megtudhatja, hogy miként előre betöltése a gyorsítótárban tárolt tartalom egy CDN-végpontot."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Azure CDN-végpont eszközök előre betöltése

[AZURE.INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Alapértelmezés szerint eszközök először gyorsítótárazott vannak kérésének. Ez azt jelenti, hogy az első kérés az egyes régiókra hosszabb ideig tarthat, mivel peremhálózat-kiszolgálói nem lesz a tartalom gyorsítótáras, és fel kell továbbítani az értekezlet-összehívást a forráskiszolgálóval. Előre betöltése a tartalom elkerülhető a első találati késés.

Mellett, mert jobb felhasználói élményt biztosít, előre betöltése a gyorsítótárban tárolt eszközöket is csökkentheti a forráskiszolgálóval a hálózati forgalmat.

> [AZURE.NOTE] Előre betöltése eszközök akkor hasznos, a nagy eseményeket, illetve a tartalom, a felhasználók, például egy új film kiadásának vagy frissítése nagyszámú egyidejű elérhetővé válik.

Ebben az oktatóanyagban végigvezeti az Azure CDN-él csomópontjait a gyorsítótárban tárolt tartalom előre betöltése.

## <a name="walkthrough"></a>Útmutató

1. Az [Azure-portálon](https://portal.azure.com)nyissa meg a CDN-profil tartalmazó az előre betölteni kívánt végpontot.  A profil lap megnyitása

2. Kattintson a végpontra a listában.  Az endpoint lap megnyitása

3. Az a CDN-végpontot lap kattintson a Betöltés gombra.

    ![CDN-végpontot lap](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)

    A betöltés lap megnyitása

    ![CDN-terhelés lap](./media/cdn-preload-endpoint/cdn-load-blade.png)

4. Írja be a teljes elérési útját minden betölteni kívánt eszköz (például `/pictures/kitten.png`) az **elérési út** rendelés_részletei.mennyiség.

    > [AZURE.TIP] További **elérési út** szövegdobozok lehetővé teszi a több eszközök listájának létrehozásához szöveg beírása után jelenik meg.  Eszközök a három pontra (…) gombra kattintva törölheti a listából.
    >
    > Elérési út kell lennie, amely megfelel az alábbi [reguláris kifejezésekkel](https://msdn.microsoft.com/library/az24scfc.aspx)relatív URL: `^(?:\/[a-zA-Z0-9-_.\u0020]+)+$`.  Egyes eszközök saját elérési utat kell rendelkeznie.  Előre betöltésekor eszközök nem helyettesítő funkció nem.

    ![Betöltés gombra](./media/cdn-preload-endpoint/cdn-load-paths.png)

5. Kattintson a **Betöltés** gombra.

    ![Betöltés gombra](./media/cdn-preload-endpoint/cdn-load-button.png)

> [AZURE.NOTE] CDN-profil percenként 10 betöltés kérések korlátozása van.

## <a name="see-also"></a>Lásd még:
- [Azure CDN zárólap törlése](cdn-purge-endpoint.md)
- [Azure CDN REST API-hivatkozás - törlése vagy zárólap előre betöltése](https://msdn.microsoft.com/library/mt634451.aspx)

<properties
    pageTitle="Azure CDN zárólap törlése |} Microsoft Azure"
    description="További információ az összes gyorsítótárazott tartalom törlése az a CDN-végpontot."
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

# <a name="purge-an-azure-cdn-endpoint"></a>Azure CDN zárólap törlése

## <a name="overview"></a>– Áttekintés

Azure CDN él csomópontok fog gyorsítótár eszközök, mindaddig, amíg az eszköz time-to-live (TTL) lejár.  Után az eszköz TTL lejár, amikor egy ügyfél az eszköz kér a szegély csomópontot, a szegély csomópont beolvassa az eszközre az ügyfél kérése szolgáló új frissített másolatának, és tár a gyorsítótár frissítése.

Időnként előfordulhat, hogy kívánt gyorsítótárban tárolt tartalom törlése a csomópontjait széle és előírhatja, hogy mindet új frissített eszközök beolvasásához.  Ez a webalkalmazás, illetve gyorsan helytelen adatokat tartalmazó frissítés eszközök frissítések miatt lehet.

> [AZURE.TIP] Megjegyzés: a végleges törlése, hogy csak törli az a CDN peremhálózat-kiszolgálói a gyorsítótárban tárolt tartalmat.  Bármely lefelé irányuló gyorsítótárát, például a proxykiszolgálók és a helyi böngésző gyorsítótárát, továbbra is tartsa lenyomva a gyorsítótárban tárolt másolatát a fájlt.  Fontos, hogy ne feledje, hogy ez egy fájlt beállításakor time-to-live.  A fájlt a legújabb kérni, így egy egyedi nevet minden alkalommal, amikor frissíti azt, vagy a [lekérdezési karakterlánc gyorsítótárazás](cdn-query-string.md)nyújtotta előnyök kihasználása lefelé irányuló ügyfél kényszerítheti.  

Ebben az oktatóanyagban végigvezeti az eszközök törlése a végpontok csomópontjait él.

## <a name="walkthrough"></a>Útmutató

1. Az [Azure-portálon](https://portal.azure.com)nyissa meg a CDN-profil tartalmazó véglegesen törölni szeretné a végpontot.

2. Az a CDN a profil lap a végleges törlése gombra.

    ![CDN-profil lap](./media/cdn-purge-endpoint/cdn-profile-blade.png)

    Ekkor megnyílik a kiürítés lap.

    ![CDN-végleges törlése lap](./media/cdn-purge-endpoint/cdn-purge-blade.png)

3. A végleges törlése a lap válassza ki a szolgáltatás címet szeretné üríteni a URL-cím legördülő listából.

    ![Űrlap törlése](./media/cdn-purge-endpoint/cdn-purge-form.png)

    > [AZURE.NOTE] Is elérheti a kiürítés lap a CDN-végpontot lap **törlése** gombjára kattintva.  Ebben az esetben az **URL-címe** mezőbe előre megadott adott végpontra szolgáltatás címe lesz.

4. Jelölje ki, milyen eszközöket, hogy a szegély csomópontok véglegesen kíván.  Ha törölni szeretné az összes eszköz, kattintson az **összes törlése** jelölőnégyzetet.  Egyéb esetben írja be a teljes elérési útját minden véglegesen törölni kívánt eszközt (például `/pictures/kitten.png`) az **elérési út** rendelés_részletei.mennyiség.

    > [AZURE.TIP] További **elérési út** szövegdobozok lehetővé teszi a több eszközök listájának létrehozásához szöveg beírása után jelenik meg.  Eszközök a három pontra (…) gombra kattintva törölheti a listából.
    >
    > Elérési út kell lennie a relatív URL-cím, az alábbi [reguláris kifejezésekkel](https://msdn.microsoft.com/library/az24scfc.aspx)illeszkedő: `^\/(?:[a-zA-Z0-9-_.\u0020]+\/)*\*$";`.  **Azure CDN a Verizon** (normál és Premium), a csillag (\*) használható helyettesítő karakterként (például `/music/*`).  Helyettesítő karakterek és az **összes törlése** az **Azure CDN Akamai a**nem engedélyezett.
    
5. Kattintson a **törlése** gombra.

    ![Törlése gomb](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [AZURE.IMPORTANT] Végleges törlése kérelmek a folyamat **Verizon az Azure CDN** (normál és Premium) és az **Azure CDN a Akamai**körülbelül 7 perc körülbelül 2-3 percet igénybe vehet.  Azure CDN legfeljebb 50 egyidejű kérelmek véglegesen az adott időpontban tartalmaz. 

## <a name="see-also"></a>Lásd még:
- [Azure CDN-végpont eszközök előre betöltése](cdn-preload-endpoint.md)
- [Azure CDN REST API-hivatkozás - törlése vagy zárólap előre betöltése](https://msdn.microsoft.com/library/mt634451.aspx)

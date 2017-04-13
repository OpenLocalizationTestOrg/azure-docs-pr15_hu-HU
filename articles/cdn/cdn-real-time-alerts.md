<properties
    pageTitle="Valós idejű Azure CDN-értesítések |} Microsoft Azure"
    description="A Microsoft Azure CDN valós idejű értesítések. Valós idejű értesítések adja meg a végpontok a CDN-profilhoz teljesítményét kapcsolatos értesítéseket."
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
    ms.date="07/12/2016"
    ms.author="casoper"/>

# <a name="real-time-alerts-in-microsoft-azure-cdn"></a>Microsoft Azure CDN valós idejű riasztásai

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]


## <a name="overview"></a>– Áttekintés

Ez a dokumentum a Microsoft Azure CDN valós idejű értesítések ismerteti. Ez a funkció valós idejű értesítésekkel a teljesítmény, a végpontok a CDN-profilhoz.  E-mailben vagy a HTTP-riasztások alapján beállíthatja:

* A sávszélesség
* Állapot kódok
* Gyorsítótár állapotok
* Kapcsolatok

## <a name="creating-a-real-time-alert"></a>A valós idejű értesítés létrehozása

1. Az [Azure-portálon](https://portal.azure.com)nyissa meg a CDN-profil.

    ![CDN-profil lap](./media/cdn-real-time-alerts/cdn-profile-blade.png)

2. Az a CDN a profil lap a **kezelés** gombra.

    ![CDN-profil lap kezelése gomb](./media/cdn-real-time-alerts/cdn-manage-btn.png)

    Ekkor megnyílik a CDN kezelőportálja segítségével.

3. Mutasson rá az **analitikai** fülre, majd mutasson a **Valós idejű stat** előugró.  Kattintson a **riasztások valós időben**.

    ![CDN-adatkezelési portál](./media/cdn-real-time-alerts/cdn-premium-portal.png)

    A meglévő riasztási beállítások (ha van ilyen) a listában látható lesz.

4. A **Riasztási hozzáadása** gombra.

    ![Riasztási gomb felvétele](./media/cdn-real-time-alerts/cdn-add-alert.png)

    Űrlap létrehozása az új értesítés jelenik meg.

    ![Az új értesítés űrlap](./media/cdn-real-time-alerts/cdn-new-alert.png)

5. Ha azt szeretné, hogy ez legyen aktív, amikor a **Mentés**gombra kattintva az értesítés, jelölje be a **Értesítés engedélyezve** jelölőnégyzet.

6. A **név** mezőben írja be egy jól értelmezhető nevet az értesítésre.

7. A **Médiatípus** tartalmazó legördülő listára jelölje ki **A HTTP nagy objektumot**.

    ![A HTTP nagyobb kijelölt objektum médiatípus](./media/cdn-real-time-alerts/cdn-http-large.png)

    > [AZURE.IMPORTANT] **HTTP nagy objektum** ki kell választania a **Médiatípus**szerint.  **Azure CDN a Verizon**nem használják a többi beállítást.  Sikertelen a **HTTP nagy objektum** kijelöléséhez okoz a riasztást soha nem indítható el.

8. Lync-jelöljön ki egy **mérőszám**, **operátorok**és **eseményindító érték** **kifejezés** létrehozása.

    - **Metrikus**válassza ki a kívánt felügyelt feltétel.  A **Sávszélesség MB** MB másodpercenként a sávszélesség-használat érték.  **Kapcsolatok száma** az egyidejű HTTP-kapcsolatot a peremhálózat-kiszolgálói számát jelenti.  A különböző gyorsítótár állapotok és állapotkód definíciókat lásd: [Azure CDN gyorsítótár állapot kódok](https://msdn.microsoft.com/library/mt759237.aspx) és [Azure CDN HTTP állapot](https://msdn.microsoft.com/library/mt759238.aspx)
    - **Operátor** a matematikai műveleti, amely megteremti a kapcsolatot a metrikus és a kiváltó ok mező értéke között.
    - **Eseményindító** a határérték mielőtt értesítést küld mezőfeltételeket, amelyek értéke.

    Az az alábbi példában a kifejezés én hoztam létre azt jelzi, hogy szeretném, ha értesítést szeretne kapni, amikor 404-es állapot kódok értéke nagyobb, mint a 25.

    ![Valós idejű riasztási példakifejezés](./media/cdn-real-time-alerts/cdn-expression.png)

9. **Intervallum**adja meg, milyen gyakran szeretné kiértékelt kifejezés.

10. A **szóló értesítés** tartalmazó legördülő listára jelölje ki, ha a kifejezés értéke igaz, ha értesítést szeretne.
    
    - **Feltétel indítása** azt jelzi, hogy az egy értesítést kapnak, amikor először észlelésekor a megadott feltételnek.
    - **Feltétel vége** azt jelzi, hogy értesítést küld észlelésekor a megadott feltétel már nem. Ez az értesítés csak is elindítja, miután a rendszer figyelése hálózatba észleli, hogy történt-e a megadott feltételnek.
    - **Folytonos** , az azt jelzi, hogy értesítést küld minden alkalommal, amikor, hogy a rendszer figyelése hálózat észlel a megadott feltételnek. Ne feledje, hogy a rendszer figyelése hálózat csak ellenőrzi egyszer / időköz a megadott feltételnek.
    - **A feltétel kezdete és vége** azt jelzi, hogy értesítést küld az első alkalommal a megadott feltétel lép fel, és ismét mikor a feltétel már nem észlelhető.

11. Ha mailben értesítéseket szeretne, jelölje be az **értesítési mailben** jelölőnégyzet.  

    ![Ha értesítést E-mail űrlap szerint](./media/cdn-real-time-alerts/cdn-notify-email.png)
    
    A **címzett** mezőben adja meg az e-mail címet, amelyre értesítés küldte-e. A **Tárgy** és a **szövegtörzs**hagyhatja, az alapértelmezett, vagy szabhatja testre az üzenetet a **rendelkezésre álló kulcsszavak** lista használatával dinamikusan beszúrása riasztási adatok, amikor a program elküldi az üzenetet.

    > [AZURE.NOTE] **Értesítés tesztelése** gombra kattintva, de csak azt követően a riasztási konfiguráció mentését tesztelheti az értesítő e-mailt.

12. Ha azt szeretné, hogy a webkiszolgálóra történő közzéteendő értesítések, jelölőnégyzetet a **HTTP postán értesítés** .

    ![HTTP-bejegyzés űrlap tagját értesítheti](./media/cdn-real-time-alerts/cdn-notify-http.png)

    Az **URL-cím** mezőbe írja be az URL-címet, amelyre a HTTP-üzenet Ön tett közzé. Az **élőfejek** mezőbe írja be a HTTP-fejlécekben az összehívást küldeni.  A **szervezet** testre előfordulhat, hogy az üzenetet a **rendelkezésre álló kulcsszavak** lista használatával dinamikusan beszúrása riasztási adatok, amikor a program elküldi az üzenetet.  **Élőfejek** és az **üzenet** az alapértelmezett hasonlóan, mint egy XML-tartalom az alábbi példában.

    ```
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
        <![CDATA[Expression=Status Code : 404 per second > 25&Metric=Status Code : 404 per second&CurrentValue=[CurrentValue]&NotificationCondition=Condition Start]]>
    </string>
    ```

    > [AZURE.NOTE] **Értesítés tesztelése** gombra kattintva, de csak az értesítés konfiguráció mentését követően a HTTP-bejegyzés értesítés tesztelheti.

13. A **Mentés** gombra kattintva mentse a riasztási konfigurációt.  Ha bejelölte a **Értesítés engedélyezve van** az 5, a értesítés ettől kezdve az aktív.

## <a name="next-steps"></a>Következő lépések

- [Valós idejű stat az Azure CDN](cdn-real-time-stats.md) elemzése
- Alaposabban az [speciális HTTP-jelentések](cdn-advanced-http-reports.md)
- [Szokásai](cdn-analyze-usage-patterns.md) elemzése


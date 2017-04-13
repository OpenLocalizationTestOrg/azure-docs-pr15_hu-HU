<properties
    pageTitle="Jelentkezzen Analytics Nézettervezőt |} Microsoft Azure"
    description="A napló Analytics Nézettervezőt lehetővé teszi a MOBILE konzolban különböző képi megjelenítésekről a MOBILE adattárban adatokat tartalmazó egyéni nézeteket hozhat létre. Ez a cikk Nézettervezőt és létrehozásának és szerkesztése az egyéni nézetek áttekintése tartalmazza."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer"></a>Log Analytics Nézettervezőt
A nézet Tervező napló Analytics lehetővé teszi a MOBILE konzolban különböző képi megjelenítésekről a MOBILE adattárban adatokat tartalmazó egyéni nézeteket hozhat létre. Ez a cikk Nézettervezőt és létrehozásának és szerkesztése az egyéni nézetek áttekintése tartalmazza.

További cikkek a Tervező nézetben érhető el a következők:

- [Mozaik hivatkozás](log-analytics-view-designer-tiles.md) - hivatkozás az egyes a mozaikokat érhető el az egyéni nézetek használata beállítást. 
- [Képi megjelenítés rész hivatkozás](log-analytics-view-designer-parts.md) - hivatkozás az egyes a mozaikokat érhető el az egyéni nézetek használata beállítást. 


## <a name="concepts"></a>Fogalmak
Nézetek a nézet Tervező készült az elemeket az alábbi táblázat tartalmazza.

| Kijelző | Leírás |
|:--|:--|
| Csempe | Megjelenik a fő napló Analytics áttekintése irányítópulton.  Tartalmaz, az egyéni nézet tárolt adatok vizuális engedélyeiről.  Különböző csempe nyújtanak különböző képi megjelenítésekről a MOBILE tárat a rekordok.  Kattintson a Csempére kattintva nyissa meg az egyéni nézet. |
| Egyéni nézet | Jelenik meg, amikor a felhasználó a hivatkozásra kattint.  Egy vagy több megjelenítést kijelzők tartalmazza. |
| Képi megjelenítés részei | Képi megjelenítés-adatok a MOBILE tárat egy vagy több [napló keresések](log-analytics-log-searches.md)alapján.  A legtöbb kijelzők fogja tartalmazni, élőfej, ahol a magas szintű képi megjelenítés és a legjobb eredményt listáját.  Másik területre típusok nyújtanak különböző képi megjelenítésekről a MOBILE tárat a rekordok.  Kattintson a elemei részletes rekordok nyújtó napló keresést végez a kijelzőt. |

![Nézet tervező – áttekintés](media/log-analytics-view-designer/overview.png)

## <a name="add-view-designer-to-your-workspace"></a>A munkaterület Nézettervezőt hozzáadása
Amikor Nézettervezőt előzetes, hozzá kell adnia azt a munkaterület a **Beállítások** csoport a MOBILE portál **Előzetes verzió szolgáltatások** kiválasztásával.

![Minta engedélyezése](media/log-analytics-view-designer/preview.png)

## <a name="creating-and-editing-views"></a>Létrehozásáról és szerkesztéséről nézetek

### <a name="create-a-new-view"></a>Új nézet létrehozása
A fő MOBILE irányítópulton Nézettervezőt csempével kattintva nyissa meg a új nézet a **Nézettervezőt használhat** .

![Nézet Tervező csempe](media/log-analytics-view-designer/view-designer-tile.png)

### <a name="edit-an-existing-view"></a>Meglévő nézet szerkesztése
A nézet Tervező a meglévő nézet szerkesztéséhez nyissa meg a nézetben a fő MOBILE irányítópulton a hivatkozásra kattintva.  Kattintson a **Szerkesztés** gombra a nézet megnyitásához kattintson a nézet Tervező.

![Nézet szerkesztése](media/log-analytics-view-designer/menu-edit.png)

### <a name="clone-an-existing-view"></a>Meglévő nézet másolása
Nézet, klónozhatja, amikor létrehoz egy új nézetet, és megnyitja a nézet Tervező az.  Az új nézet lesz ugyanazt a nevet, mint az eredeti a "másolása" hozzáfűzött a végéig.  Nézet klónozhatja, a fő MOBILE irányítópulton a hivatkozásra kattintva nyissa meg a létező nézetet.  Kattintson a nézet megnyitásához kattintson a nézet Tervező a **Adatfeliratsor** gombra.

![Nézet másolása](media/log-analytics-view-designer/edit-menu-clone.png)

### <a name="delete-an-existing-view"></a>Meglévő nézet törlése
Ha törölni szeretne egy meglévő nézet, a fő MOBILE irányítópulton a hivatkozásra kattintva nyissa meg a nézet.  Kattintson a **Szerkesztés** gombra kattintva nyissa meg a nézetben a nézet Tervező, és kattintson a **Nézet törlése**gombra.

![Nézet törlése](media/log-analytics-view-designer/edit-menu-delete.png)

### <a name="export-an-existing-view"></a>Meglévő nézet exportálása
Nézet, amely egy másik munkaterületi importálása, vagy egy [erőforrás-kezelő Azure-sablon](../resource-group-authoring-templates.md)használata JSON-fájlba exportálhatja.  Meglévő nézet exportál, a fő MOBILE irányítópulton a hivatkozásra kattintva nyissa meg a nézet.  Kattintson az **Exportálás** gombra kattintva hozzon létre egy fájlt a böngésző letöltése mappában.  A fájl nevét a bővítmény *omsview*a nézet neve lesz.

![A nézet exportálása](media/log-analytics-view-designer/edit-menu-export.png)

### <a name="import-an-existing-view"></a>Meglévő nézet importálása
Importálhat egy másik kezelése csoportban az exportált *omsview* fájlt.  Meglévő nézet importálni, először létre kell hoznia egy új nézetet.  Kattintson az **Importálás** gombra, majd jelölje ki a *omsview* fájlt.  A fájlban található konfiguráció bemásolja a létező nézetet.

![A nézet exportálása](media/log-analytics-view-designer/edit-menu-import.png)

## <a name="working-with-view-designer"></a>A Tervező nézet használata
A nézet Tervező három ablaktáblát tartalmaz.  A **Tervezés** munkaablak felel meg az egyéni nézet.  Amikor felvesz csempék és kijelzők a **vezérlő** ablakból a **Tervezés** munkaablak kerülnek a nézetet.  A **Tulajdonságok** ablaktáblában jelennek meg a mozaik elrendezés vagy a kijelölt rész tulajdonságait.

![Nézettervezőt használhat.](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-view-tile"></a>Állítsa be a view mozaik
Egyéni nézet beállíthatja, hogy csak egyetlen csempére.  A **vezérlő** ablak az aktuális kártyára tekinthet meg és jelölje ki az alternatív jelölje ki a **Mozaik elrendezés** fülre.  A **Tulajdonságok** ablaktáblában jelennek meg az aktuális kártyára tulajdonságait.  Tulajdonságait csempe megfelelően a részletes adatokat a [Mozaik hivatkozást](log-analytics-view-designer-tiles.md) , és kattintson az **Alkalmaz** a módosítások mentéséhez.

### <a name="configure-visualization-parts"></a>Képi megjelenítés kijelzők konfigurálása
Nézet is elhelyezhet a kijelzők megjelenítési tetszőleges számú.  Jelölje be a **Nézet** fülre, majd a nézet hozzáadása egy képi megjelenítés kijelzőt.  A **Tulajdonságok** ablaktáblában jelennek meg a kijelölt rész tulajdonságait.  Tulajdonságait nézet megfelelően a részletes adatokat a [képi megjelenítések rész hivatkozást](log-analytics-view-designer-parts.md) , és kattintson az **Alkalmaz** a módosítások mentéséhez.

### <a name="delete-a-visualization-part"></a>Töröl egy képi megjelenítés
A nézet egy képi megjelenítés kijelző eltávolítható a kijelző jobb felső sarkában lévő **X** gombra kattintva.

### <a name="rearrange-visualization-parts"></a>Képi megjelenítés kijelzők átrendezése
Nézetek csak a Megjelenítés részeinek egy sor van.  Meglévő kijelzők nézetben átrendezése kattintva, majd húzza őket az új helyre.


## <a name="next-steps"></a>Következő lépések

- [Csempék](log-analytics-view-designer-tiles.md) felvétele az egyéni nézetben.
- [Képi megjelenítés kijelzők](log-analytics-view-designer-parts.md) hozzáadása az egyéni nézetben.

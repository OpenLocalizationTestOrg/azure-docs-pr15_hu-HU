<properties
    pageTitle="Jelentkezzen be a bővítmények több hiba után"
    description="A jelentés, amely jelzi a felhasználók sikeresen a után több egymást követő nem sikerült bejelentkezési kísérleteket."
    services="active-directory"
    documentationCenter=""
    authors="SSalahAhmed"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/04/2016"
    ms.author="saah;kenhoff"/>

# <a name="sign-ins-after-multiple-failures"></a>Bejelentkezési bővítmények több hiba után
Ez a jelentés azt jelzi, hogy felhasználók sikeresen a után több egymást követő nem sikerült bejelentkezési kísérleteket. Lehetséges okai lehetnek:

- Felhasználói volna elfelejtette a jelszavát</li><li>Felhasználó, a sikeres jelszótalálgatásokkal biztonságosabb áldozatává homonimaszerű webcímmel kikényszerítése

Ez a jelentés származó találatok jelenjenek meg kell követnie az egymást követő sikertelen bejelentkezés kísérletek sikeres bejelentkezés előtt és az első sikeres bejelentkezés társított időbélyeg számát.

**Jelentés futtatása**: beállíthatja a lehető legkevesebb egymást követő sikertelen bejelentkezés kísérletek kell ahhoz a jelentés megjeleníteni. Amikor módosítja Ez a beállítás fontos tudni, hogy a módosítások nem történik bármelyik meglévő sikertelen bejelentkezés modulok jelenleg jelenjenek meg a meglévő jelentés. Azonban ezek fog vonatkozni minden jövőbeni bejelentkezési bővítmények. Ez a jelentés módosításai is csak licenccel rendelkező rendszergazdák által végezhető.


![Jelentkezzen be a bővítmények több hiba után](./media/active-directory-reporting-sign-ins-after-multiple-failures/signInsAfterMultipleFailures.PNG)

<properties
    pageTitle="Azure CDN prémium a gyorsítótárazás viselkedése a lekérdezési karakterlánc kérelmek Verizon szabályozása |} Microsoft Azure"
    description="Azure CDN lekérdezési karakterláncban gyorsítótár-vezérlők, hogyan fájlok nem tartalmazzák a lekérdezési karakterlánc gyorsítótárba helyezhető."
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

#<a name="controlling-caching-behavior-of-cdn-requests-with-query-strings---premium"></a>CDN-kérelmek a lekérdezési karakterlánc - támogatás gyorsítótárazási viselkedésének szabályozása

> [AZURE.SELECTOR]
- [Normál](cdn-query-string.md)
- [A Verizon Azure CDN-támogatás](cdn-query-string-premium.md)

##<a name="overview"></a>– Áttekintés

Hogyan fájlok nem tartalmazzák a lekérdezési karakterlánc gyorsítótárba helyezhető vezérlők gyorsítótárazás a lekérdezési karakterlánc.

> [AZURE.IMPORTANT] A szokásos és prémium CDN termékeket adja meg a gyorsítótár-funkciók egy lekérdezési karakterláncban, de a felhasználói felület működik.  Ehhez a dokumentumhoz a felület **Azure CDN-támogatás az Verizon**ismerteti.  Lekérdezési karakterlánc gyorsítótárazás **Azure CDN szabványos a Akamai** és **Azure CDN szabványos a Verizon**, című témakör tartalmaz [CDN-kérelmek a lekérdezési karakterlánc szabályozása gyorsítótárazási működését](cdn-query-string.md).

Három módok állnak rendelkezésre:

- **normál-gyorsítótár**: Ez az alapértelmezett módját.  A CDN él csomópont fogja át, a lekérdezési karakterlánc a lekérdező a az origin az első kérelem és a gyorsítótár az eszköz.  Minden későbbi kérelmek eszköz a szegély csomópont kombinációja figyelmen kívül hagyja a lekérdezési karakterlánc mindaddig, amíg a gyorsítótárban tárolt eszköz lejár.
- **nincs-gyorsítótár**: Ebben az üzemmódban kérelmek a lekérdezési karakterlánc nem gyorsítótárban az a CDN él csomópontot.  A szegély csomópont az eszköz közvetlenül az origin olvassa be, és átadja a lekérdező minden kérésével.
- **egyedi gyorsítótár**: Ebben az üzemmódban minden kérés egy lekérdezési karakterlánccal kezeli a saját gyorsítótár egyedi eszközként.  Például *foo.ashx?q=bar* kér kiindulópontja válasza a szegély csomópontot a gyorsítótáras szeretne, és esetén a visszaadott későbbi gyorsítótárát, hogy ugyanazon lekérdezési karakterlánccal.  *Foo.ashx?q=somethingelse* kér a külön eszközként saját idővel élő volna gyorsítótárba.

##<a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>Lekérdezési karakterláncban gyorsítótárazás prémium CDN profilok beállításainak módosítása

1. Az a CDN a profil lap a **kezelés** gombra.

    ![CDN-profil lap kezelése gomb](./media/cdn-query-string-premium/cdn-manage-btn.png)

    Ekkor megnyílik a CDN kezelőportálja segítségével.

2. Mutasson a **HTTP nagy** fülre, majd mutasson a **Gyorsítótár beállításainak** előugró.  Kattintson a **lekérdezés-karakterlánc gyorsítótárazás**.

    Lekérdezési karakterlánc gyorsítótárazási beállítások is megjelennek.

    ![CDN lekérdezési karakterláncban gyorsítótár-beállításai](./media/cdn-query-string-premium/cdn-query-string.png)

3. A kijelölés elvégzése után kattintson a **frissítés** gombra.


> [AZURE.IMPORTANT] A beállításokon végzett módosítások nem feltétlenül láthatók azonnal, mint a szülőtől keresztül a CDN regisztráció időt vesz igénybe.  <b>Azure CDN Verizon a</b> profilok propagálása 90 percen belül általában befejeződik, de bizonyos esetekben több időt vesz igénybe.

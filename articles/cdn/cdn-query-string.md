<properties
    pageTitle="Azure CDN-gyorsítótárazás viselkedése a lekérdezési karakterlánc kérelmek szabályozása |} Microsoft Azure"
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

#<a name="controlling-caching-behavior-of-cdn-requests-with-query-strings"></a>A lekérdezési karakterlánc CDN-kérelmek gyorsítótárazási viselkedését szabályozása

> [AZURE.SELECTOR]
- [Normál](cdn-query-string.md)
- [A Verizon Azure CDN-támogatás](cdn-query-string-premium.md)

##<a name="overview"></a>– Áttekintés

Hogyan fájlok nem tartalmazzák a lekérdezési karakterlánc gyorsítótárba helyezhető vezérlők gyorsítótárazás a lekérdezési karakterlánc.

> [AZURE.IMPORTANT] A szokásos és prémium CDN termékeket adja meg a gyorsítótár-funkciók egy lekérdezési karakterláncban, de a felhasználói felület működik.  Ehhez a dokumentumhoz a felület **Azure CDN szabványos a Akamai** és **Azure CDN szabványos a Verizon**ismerteti.  Lekérdezés az **Azure CDN prémium verzióval Verizon a**gyorsítótár karakterlánc, című témakör tartalmaz [szabályozása CDN kérelmek a lekérdezési karakterlánc - prémium gyorsítótárazási viselkedését](cdn-query-string-premium.md).

Három módok állnak rendelkezésre:

- **Figyelmen kívül hagyása lekérdezési karakterláncok**: Ez az alapértelmezett módját.  A CDN él csomópont fogja át, a lekérdezési karakterlánc a lekérdező a az origin az első kérelem és a gyorsítótár az eszköz.  Minden későbbi kérelmek eszköz a szegély csomópont kombinációja figyelmen kívül hagyja a lekérdezési karakterlánc mindaddig, amíg a gyorsítótárban tárolt eszköz lejár.
- **A lekérdezési karakterlánc URL-gyorsítótárazás figyelmen kívül hagyása**: Ebben az üzemmódban kérelmek a lekérdezési karakterlánc nem gyorsítótárban az a CDN él csomópontot.  A szegély csomópont az eszköz közvetlenül az origin olvassa be, és átadja a lekérdező minden kérésével.
- **Gyorsítótár minden egyedi URL-címe**: Ebben az üzemmódban minden kérés egy lekérdezési karakterlánccal kezeli a saját gyorsítótár egyedi eszközként.  Például *foo.ashx?q=bar* kér kiindulópontja válasza a szegély csomópontot a gyorsítótáras szeretne, és esetén a visszaadott későbbi gyorsítótárát, hogy ugyanazon lekérdezési karakterlánccal.  *Foo.ashx?q=somethingelse* kér a külön eszközként saját idővel élő volna gyorsítótárba.

##<a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Lekérdezési karakterláncban gyorsítótárazás szabványos CDN-profilok beállításainak módosítása

1. Kattintson a CDN-profil lap a CDN-végpont kezelni szeretné.

    ![CDN-profil lap végpontok](./media/cdn-query-string/cdn-endpoints.png)

    Ekkor megnyílik a CDN-végpontot lap.

2. Kattintson a **Konfigurálás** gombra.

    ![CDN-profil lap kezelése gomb](./media/cdn-query-string/cdn-config-btn.png)

    Az a CDN-konfigurációs lap megnyitása

3. Engedélyezze a **lekérdezési karakterlánc gyorsítótárazás viselkedése** legördülő listából.

    ![CDN lekérdezési karakterláncban gyorsítótár-beállításai](./media/cdn-query-string/cdn-query-string.png)

4. A kijelölés elvégzése után kattintson a **Mentés** gombra.

> [AZURE.IMPORTANT] A beállításokon végzett módosítások nem feltétlenül láthatók azonnal, mint a szülőtől keresztül a CDN regisztráció időt vesz igénybe.  <b>Azure CDN Akamai a</b> profilok propagálása általában befejezi egy percen belül.  <b>Azure CDN Verizon a</b> profilok propagálása 90 percen belül általában befejeződik, de bizonyos esetekben több időt vesz igénybe.

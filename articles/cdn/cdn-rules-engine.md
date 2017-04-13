<properties
    pageTitle="A szabályok motor használata Azure CDN HTTP alapértelmezett működés felülbírálása |} Microsoft Azure"
    description="A szabályok motor lehetővé teszi, hogy hogyan HTTP-kérések kezelnek Azure CDN, például, hogy bizonyos típusú tartalmakat kézbesítve blokkolása testreszabása, gyorsítótár szabályzat kialakítása és módosíthatja a HTTP-fejlécek."
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

# <a name="override-default-http-behavior-using-the-rules-engine"></a>A szabályok motor használata HTTP alapértelmezés felülbírálása

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>– Áttekintés

A szabályok motor testreszabása, hogy miként HTTP-kérések kezelése, például bizonyos típusú tartalmakat kézbesítve blokkolása, a gyorsítótár házirend megadása és módosítása a HTTP-fejlécek teszi lehetővé.  Ebben az oktatóanyagban fog bemutatják, a szabály létrehozása módosítása CDN-eszközök gyorsítótárazási viselkedését.  Van még videotartalmak érhető el a "[Lásd még](#see-also)" szakasz.

## <a name="tutorial"></a>Oktatóprogram

1. Az a CDN a profil lap a **kezelés** gombra.

    ![CDN-profil lap kezelése gomb](./media/cdn-rules-engine/cdn-manage-btn.png)

    Ekkor megnyílik a CDN kezelőportálja segítségével.

2. Kattintson a **Szabályok motor**által követett **HTTP nagy** lap.

    Az új szabály beállítások is megjelennek.

    ![CDN-új szabály beállításai](./media/cdn-rules-engine/cdn-new-rule.png)

    >[AZURE.IMPORTANT] A sorrendben, amelyben a több szabállyal szerepelnek, az hatással van kezelésének módját. A következő szabályok felülbírálhatják az előző szabály által megadott műveletek.
    
3. Írjon be egy nevet a **név és leírás** szövegdoboz.

4. Azonosítják az kérelmek a szabály érvényesek lesznek.  A **mindig** egyezés feltétel alapértelmezés szerint be van jelölve.  Ebben az oktatóanyagban **mindig** használni fog, így hagyja, hogy a kijelölt.

    ![CDN-egyezés feltétel](./media/cdn-rules-engine/cdn-request-type.png)

    >[AZURE.TIP] Nincsenek egyezés számos különböző típusú feltételt a legördülő menü érhető el.  A bal oldalán a hol.van feltétel kék információs ikonra kattintva a kijelölt feltételnek részletesen magyarázza el.
    >
    >Hol.van feltételek részletesen teljes listáját olvassa el a [szabályok motor megfelelően feltétel és a szolgáltatás részletei](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_0)című témakört.

5.  Kattintson a **+** gomb mellett egy új szolgáltatást hozzáadható **szolgáltatások** .  Jelölje ki a legördülő menü a bal oldalon, **Kötelező belső Max életkorát**.  A megjelenő mezőbe írja be a **300**.  A hátralévő alapértékek hagyja.

    ![CDN-funkció](./media/cdn-rules-engine/cdn-new-feature.png)

    >[AZURE.NOTE] Hol.van feltételekkel, bal oldalán található az új funkció kék információs ikonra jelenik meg ez a funkció olvashat.  **Kötelező belső Max kor**, amíg azt vannak felülbírálása az eszköz **Gyorsítótár-vezérlők** és **elévülési** fejlécek vezérlőelemre, amikor a CDN él csomópont az eszköz a eredetű frissíthetők.  300 másodperc példáinkban azt jelenti, hogy a CDN él csomópont az eszköz fog gyorsítótár-5 percig az eszközre az eredetihez képest frissítése előtt.
    >
    >Részletesen szolgáltatások teljes listáját olvassa el a [szabályok motor egyezés feltétel és a szolgáltatás részletei](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1)című témakört.

6.  A **Hozzáadás** gombra kattintva mentse az új szabályt.  Az új szabály most jóváhagyásra. Miután engedélyezte, az állapot változik **Függőben XML** **Aktív**XML-fájlba.

    >[AZURE.IMPORTANT] Szabályok módosításokat – a CDN propagálása legfeljebb 90 órába is telhet.

## <a name="see-also"></a>Lásd még:
* [Azure pénteken: Azure CDN hatékony új prémium szolgáltatásaihoz](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (videó)
* [Szabályok motor egyezés feltétel és a szolgáltatás részletei](https://msdn.microsoft.com/library/mt757336.aspx)

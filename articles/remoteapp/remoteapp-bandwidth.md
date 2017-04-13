
<properties 
    pageTitle="Azure RemoteApp hálózati sávszélesség-használat becslése |} Microsoft Azure"
    description="Tudjon meg többet a hálózati sávszélességre vonatkozó követelmények a Azure RemoteApp webhelycsoportok és-alkalmazások."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />

# <a name="estimate-azure-remoteapp-network-bandwidth-usage"></a>Azure RemoteApp hálózati sávszélesség-használat becslése 

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp a távoli asztali Protocol (RDP) használ az Azure felhő és a felhasználók az alkalmazások közötti kommunikációt. Ebben a cikkben néhány alapvető útmutatók, hogy a hálózat-használat becslése, illetve esetleg a hálózati sávszélesség-használat Azure RemoteApp felhasználónként értékeléséhez is használhatja.

Felhasználónként sávszélesség-használat becslése nagyon összetett, és csak több alkalmazást futtató egyidejű többfeladatos helyzetekben hol is hatással lehet alkalmazások egymás – a hálózat sávszélessége igény alapján teljesítményét. Akkor is (például Mac ügyfél és a HTML5-ös ügyfél) távoli asztali ügyfél típusú különböző sávszélesség eredmények vezethet. Segít a munka – ezek komplikációk, azt fogja szétválasztani az használatát jelenik meg több való replikáció a való életben esetek általános kategóriát. (Ha a való életben forgatókönyv, természetesen kombinálja a kategóriák és felhasználó által eltér.)

További - megyünk előtt látható, hogy feltételezzük RDP nyújt a kiváló rendszerének kiváló találatokkal használatát a hálózatokon alatti 120 ms és sávszélesség válaszidejű több mint 5 MB - alapul RDP-féle dinamikusan segítségével módosíthatja a rendelkezésre álló hálózati sávszélesség és az alkalmazás becsült sávszélesség használatának lehetősége van szüksége. Ez a cikk Ugrás túl, amelyek "használatát találatokkal" nézzen át a szegélyt, ahol esetek lecseréléshez megkezdi és csökkentheti a felhasználói felület kezdődik.

Most már nézze meg az adatait, beleértve fontolja meg, hogy tényezők az alábbi cikkekből eredeti javaslatok, és mi azt nem vette fel a ad becslést.

- [Hogyan hálózat sávszélessége és minőségét tapasztalja munka együtt?](remoteapp-bandwidthexperience.md)
- [Tesztelje a hálózati sávszélesség-használat a néhány gyakori alkalmazási területek](remoteapp-bandwidthtests.md)
- [Ha nem rendelkezik az idő vagy az azt jelenti, hogy tesztelje rövid útmutató](remoteapp-bandwidthguidelines.md)


## <a name="what-are-we-not-including"></a>Mi a Microsoft nem tartalmazzák az?

Amikor, olvassa el a javasolt vizsgálatok és a teljes (és admittedly általános) javaslatok, ügyeljen arra, hogy nincsenek-e számos tényező, hogy nem figyelembe venni. Ha például a felhasználói élmény komplikációk összehasonlítása feltöltés aszimmetrikus jellegét által biztosított töltse le a sávszélesség. A legtöbb Wi-Fi hálózatokon aszimmetrikus jellegét továbbá lesz hatással lehet a teljesítmény és a felhasználói élmény felismerésének. Interaktív felhasználási területei kisebb, mint az előző, amelyek elveszett video- vagy keretek számának növelése és így hatással lehet a felhasználó a továbbított felület felismerésének előfordulhat, hogy az lefelé irányuló forgalom prioritása. A saját kísérletek megtudhatja, Mire jó a adott használati eset és a hálózati futtathatja.

Bár eszközök átirányítása témákat ölelik fel, azt nem vette figyelembe a hatása a sávszélességre csatlakoztatott eszközök, például a tárhely, nyomtatók, víruskeresők, webes fényképezőgépek és más USB-eszközök okozta hálózati forgalmat. Az effektus ezen eszközök általában a sávszélesség igényeinek ideiglenes napra, és eltűnik a feladat befejezésekor. De ha gyakran sávszélesség igény szerinti igazán észrevehető lehetnek.

Még nem témákat ölelik fel egy felhasználó hatással lehet a más felhasználók ugyanabba a hálózatba belül. Egyetlen felhasználó 100 MB/s hálózaton 4K videó használata más például jelentősen is hatással más felhasználók próbál ugyanazt a feladat ugyanazon a hálózaton. Sajnos kap fokozatosan nehezebb megállapíthatja, hogyan hajtja végre a rendszer az összesítés kapcsolatos gyakori és az összes felölelő ajánlást adjon egyidejű használatát hatását. Azt is fel, hogy, hogy protokoll alaptechnológiát láthatóvá válik a rendelkezésre álló hálózati sávszélesség felhasználási, de korlátokkal lesz.
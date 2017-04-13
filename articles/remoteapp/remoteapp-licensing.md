<properties
    pageTitle="Azure RemoteApp licencelési |} Microsoft Azure"
    description="Megtudhatja, hogyan működik a licencelés az Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />


# <a name="how-does-licensing-work-in-azure-remoteapp"></a>Licencelési működése az Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Úgy állítja be a Azure RemoteApp szolgáltatásban, a sablonok, létrehozott, és készen áll a felhasználók alkalmazások közzététele. De van még egy utolsó lépésként megállapítása: licencelési. Csak elve munkát licencelési, RemoteApp és RemoteApp megosztott alkalmazások

Távoli asztali CAL vagy bármely Windows licenceket RemoteApp nincs szükség. Az előfizetés magát RemoteApp oldalának gondoskodik. (Nézze meg a [tervek árak](https://azure.microsoft.com/pricing/details/remoteapp)részleteit.)

A képek az előfizetés a egyikét használja, ha a telepített összes alkalmazás adott képhez erre szolgáló külön licenc nélkül megoszthat. Például ha a webhelycsoport létrehozása a Windows Server 2012 R2 sablonként képe használ, megoszthatja System Center Endpoint Protection a felhasználókkal. A csak a szabály alól kivételt az Office 365 ProPlus, amelyek külön előfizetést igényel, és az Office 2013, mely egy gyártási gyűjteményben nem lehet megosztani.

Ha szeretné használni az Office 365 részét képező RemoteApp sablonként képe, egy *meglévő* Office 365 ProPlus csomagot kell rendelkeznie. Ugyanez igaz bármely közzétenni kívánt egyéni sablon használatával az Office 365-alkalmazásokban. A saját verzióra szóló előfizetés az alkalmazások aktiválása szüksége. Ez a próba- és a kifizetett előfizetésekhez igaz. Ha azt szeretné, hogy közben szeretné használni az Office 365-sablonként képe a próba-előfizetést, *és előfizetést még nincs*, az Office 365- [előfizetés](https://go.microsoft.com/fwlink/p/?LinkID=403802) próba-előfizetés lap megnyitásához. Lásd: [hogyan RemoteApp és az Office működnek együtt?](remoteapp-o365.md) további információt.

Ha a próbaidőszak során nem kívánt beszerzése az Office 365 próba-előfizetés, akkor használja az Office 2013 Professional Plus sablon kép, amely megtalálható RemoteApp. Ez a sablon kép csak akkor használható 30 napig, és nem alakítható fizetett gyűjteménybe.

Egyéb alkalmazások frissítenie kell győződjön meg arról, hogy az alkalmazás megosztása a licenc.

Amely értelemszerű, jobbra? Bármely alkalmazásra, amelyek jogilag jogosult megosztása közzéteheti. És győződjön meg arról, hogy valójában jogosultak a programok megosztása kell.

Felhívjuk a figyelmét arra, hogy nem használhatók a CAL vagy mennyiségi licenc szerződés felhő gyűjteménye. A hibrid webhelycsoport (kivéve az Office) az alkalmazások aktiválása *is* használja a mennyiségi licencszerződést. Csak kell telepíteni őket a sablon képe, a mennyiségi licenc adathordozóról. Kövesse a licencek telepítéséhez távoli asztali környezetben alkalmazás szállítótól.

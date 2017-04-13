<properties
    pageTitle="Első lépések az Azure tároló öt perc az |} Microsoft Azure"
    description="Gyorsan emelkedő fel a Microsoft Azure BLOB, táblázat és Azure tároló rövid elkezdi, a Visual Studio és az Azure tároló irányító sorok. Az öt perc az első Azure tároló az alkalmazásnak a futtatására."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="get-started-with-azure-storage-in-five-minutes"></a>Első lépések az Azure tároló öt perc az

## <a name="overview"></a>– Áttekintés

Nagyon egyszerűen induláshoz fejlesztéséhez Azure adathordozós. Ebből az oktatóanyagból megtudhatja, hogyan szerezhető be az Azure tároló alkalmazások és a gyors. A rövid útmutató a Microsoft és az Azure SDK .NET kell megadnia. Ezeket a rövid elindul kattintásra kész kódot tartalmaznak, amely bemutat néhány egyszerű programozási forgatókönyvet Azure adathordozós.

Ha többet szeretne tudni Azure tárhely, mielőtt a kódot be van, nézze meg a [Következő lépések](#next-steps).

## <a name="prerequisites"></a>Előfeltételek

Az alábbi előfeltételek kell, mielőtt elkezdené:

1. Állíthat össze, és az alkalmazás összeállítása a [Visual Studio](https://www.visualstudio.com/) telepítve van a számítógépén a verziójára van szükség.

2. Telepítse a legújabb [Azure SDK a .NET rendszerhez](https://azure.microsoft.com/downloads/). A SDK tartalmaz, az Azure quickstart útmutató minta projektek, az Azure tároló irányító és az [Azure tároló ügyfél tár a .NET rendszerhez](https://msdn.microsoft.com/library/azure/dn261237.aspx).

3. Győződjön meg arról, hogy [a .NET-keretrendszer 4.5](http://www.microsoft.com/download/details.aspx?id=30653) telepítve van a számítógépén, ahogyan azt az Azure quickstart útmutató minta projektek, ebben az oktatóanyagban fogjuk használni.

    Ha nem biztos abban, hogy melyik .NET-keretrendszer verziója telepítve van a számítógépre, olvassa el a [hogyan: meghatározása amely .NET keretrendszer verzió telepített](https://msdn.microsoft.com/vstudio/hh925568.aspx). Vagy nyomja le az ENTER a **Start** gombra, vagy a Windows-billentyűt, írja be a **Vezérlőpult parancsra**. Kattintson a **programok** > **Programok és szolgáltatások**, és határozza meg, hogy a .NET-keretrendszer 4.5 a telepített programok között szerepel a listában.

4. Az Azure-előfizetéssel és Azure tárterület-fiókkal kell.

    - Az Azure előfizetéssel című témakörben kaphat [Ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/), a [Vásárlás beállításokat](https://azure.microsoft.com/pricing/purchase-options/)és a [Tag kínál](https://azure.microsoft.com/pricing/member-offers/) (az MSDN webhelyen, a Microsoft Partner Network és BizSpark és más Microsoft-szoftverekhez tagjai).
    - Tárterület fiók létrehozása az Azure-ban, megtudhatja, [hogy miként tárterület-fiók létrehozása](storage-create-storage-account.md#create-a-storage-account).

## <a name="run-your-first-azure-storage-application-against-azure-storage-in-the-cloud"></a>Az első Azure tároló alkalmazás futtatásához a felhőben Azure tárolás ellen

Ha befejezte a fiók, létrehozhat egy egyszerű Azure tároló alkalmazást az Azure gyors indítása minta projektek közül a Visual Studióban. Ebben az oktatóanyagban Azure tárolására koncentrál a minta projektek: **Azure tároló: BLOB**, **Azure tároló: fájlok**, **Azure tároló: sorban várakozó**, és **Azure tároló: táblázatok**:

1. Indítsa el a Visual Studio.
2. Kattintson a **fájl** menü **Új projektet**.
3. **Új projekt** párbeszédpanelen kattintson a **telepített** > **sablonok** > **Visual C#** > **felhő** > **QuickStarts csomagban** > **Data Services**.
    egy. Válasszon az alábbi sablonok közül: **Azure tároló: BLOB**, **Azure tároló: fájlok**, **Azure tároló: sorban várakozó**, vagy **Azure tároló: táblázatok**.
    b. Győződjön meg arról, hogy a célhely keretrendszer **.NET-keretrendszer 4.5** van kiválasztva.
    - 3. Adja meg a projekt nevét, és az új Visual Studio megoldás létrehozása, ahogy azt:

    ![Azure gyors indítása][Image1]

Előfordulhat, hogy ellenőrizni kívánt forráskódot az alkalmazás futtatása előtt. Tekintse át a kódot, jelölje be a **Nézet** menüben a Visual Studióban **Solution Explorer** . Ezután kattintson duplán a Program.cs fájlt.

Ezután a minta alkalmazásnak a futtatására:

1.  A Visual Studióban jelölje be a **Megoldást jelölőnégyzetet** a **Nézet** menüben. Nyissa meg a App.config fájl- és a kapcsolati karakterláncot, a Megjegyzés Azure tároló irányító:

    `<!--<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>-->`

2.  Az Azure tároló szolgáltatás vegye ki a megjegyzésjeleket a kapcsolati karakterláncot, és adja meg a a tárhely nevét és az access fiókkulcs a App.config fájl:`<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]"`

    A tároló fiókkulcs access beolvasásához, olvassa el a [a tárhely hívóbetűk kezelése](storage-create-storage-account.md#manage-your-storage-access-keys)című témakört.

3.  Miután, adja meg a tárhely fiók nevét, és a hívóbetű App.config a fájlban, kattintson a **fájl** menü, válassza az **Összes mentése** a projektfájlok mentésére.
4.  A **Szerkesztés** menüben kattintson a **Szerkesztés megoldás**.
5.  A **hibakeresési** menüben nyomja le az **F11** futtatását a megoldást lépésről lépésre, és nyomja le az **F5** futtatásához a megoldást.


## <a name="run-your-first-azure-storage-application-locally-against-the-azure-storage-emulator"></a>Az első Azure tárterület-alkalmazásokat az Azure tároló irányító helyileg futtassanak

Az [Azure tároló irányító](storage-use-emulator.md) , amelynek azonos fejlesztési célokra helyezése Azure Blob, a várakozási sora és a táblázat helyi környezetet nyújt. A tároló irányító helyi meghajtóra, tesztelje a a tárterület-alkalmazás létrehozása az Azure előfizetést vagy a tárterület-fiók nélkül, és bármilyen költség nélkül is használhatja.

Próbálja ki, hozzunk létre egy egyszerű Azure tároló alkalmazást az Azure gyors indítása minta projektek közül a Visual Studióban. Ebben az oktatóprogramban az **Azure Blob-tárolóhoz** **Azure Táblatárolóhoz**és **Azure várólista-tároló** minta projektek koncentrál:

1. Indítsa el a Visual Studio.
2. Kattintson a **fájl** menü **Új projektet**.
3. **Új projekt** párbeszédpanelen kattintson a **telepített** > **sablonok** > **Visual C#** > **felhő** > **QuickStarts csomagban** > **Data Services**.
    egy. Válasszon az alábbi sablonok közül: **Azure tároló: BLOB**, **Azure tároló: fájlok**, **Azure tároló: sorban várakozó**, vagy **Azure tároló: táblázatok**.
    b. Győződjön meg arról, hogy a célhely keretrendszer **.NET-keretrendszer 4.5** van kiválasztva.
    c billentyűkombinációt. Adja meg a projekt nevét, és az új Visual Studio megoldás létrehozása, ahogy azt:

    ![Azure gyors indítása][Image1]

4.  A Visual Studióban jelölje be a **Megoldást jelölőnégyzetet** a **Nézet** menüben. Nyissa meg a App.config fájl- és megjegyzés a kapcsolati karakterláncot az Azure tároló fiók meg, ha már felvett egy. Ezután vegye ki a megjegyzésjeleket a kapcsolati karakterláncot az Azure tároló irányító:

    `<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>`

Előfordulhat, hogy ellenőrizni kívánt forráskódot az alkalmazás futtatása előtt. Tekintse át a kódot, jelölje be a **Nézet** menüben a Visual Studióban **Solution Explorer** . Ezután kattintson duplán a Program.cs fájlt.

A minta alkalmazás futtatásához a Azure tároló irányító a Tovább gombra:

1.  Kattintson a **Start** gombra, vagy a Windows billentyűt, *a Microsoft Azure tároló irányító*keres, és indítsa el az alkalmazást. A irányító indításakor megjelenik egy ikon, és a Windows tevékenységnézethez terület jelenít meg értesítést.
2.  A Visual Studióban kattintson a **Megoldás összeállítása** a **Szerkesztés** menüben.
3.  A **hibakeresési** menüben nyomja le az **F11** futtatásához a megoldást lépésenkénti, vagy nyomja le az **F5** lépései a megoldást futtatásához.

## <a name="next-steps"></a>Következő lépések

Ha többet szeretne tudni az Azure tároló következő forrásokban talál:

* [Bevezetés a Microsoft Azure-tárolóhoz](storage-introduction.md)
* [Azure tároló Explorer – első lépések](../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Első lépések a .NET használatával Azure Blob-tárolóhoz](storage-dotnet-how-to-use-blobs.md)
* [Azure táblatárolóhoz .NET használata – első lépések](storage-dotnet-how-to-use-tables.md)
* [Azure várólista-tároló .NET használata – első lépések](storage-dotnet-how-to-use-queues.md)
* [A Windows Azure fájltároló – első lépések](storage-dotnet-how-to-use-files.md)
* [A AzCopy parancssori segédprogram az adatok átvitele](storage-use-azcopy.md)
* [Azure tároló dokumentáció](https://azure.microsoft.com/documentation/services/storage/)
* [Microsoft Azure tároló ügyfél tár a .NET rendszerhez](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Azure tárolása REST API-val](https://msdn.microsoft.com/library/azure/dd179355.aspx)

[Image1]: ./media/storage-getting-started-guide/QuickStart.png

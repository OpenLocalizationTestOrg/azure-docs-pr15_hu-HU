<properties 
    pageTitle="Hogyan Media Services fejlesztési .NET a számítógép beállítása" 
    description="Tudjon meg többet a Media Services .NET a Media Services SDK használatának előfeltételei. Is megtudhatja, hogyan hozhat létre a Visual Studio-at." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/24/2016"  
    ms.author="juliako"/>

#<a name="media-services-development-with-net"></a>A .NET Media Services szolgáltatás fejlesztését

[AZURE.INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

Ez a témakör azt ismerteti, hogy a .NET használatával Media Services-alkalmazások fejlesztésével indítása.

Az **Azure Media Services.NET SDK** tár lehetővé teszi a Media Services .NET használata szemben a program. Az **Azure Media Services .NET SDK-bővítmények** tár, hogy még jobban megkönnyíti a .NET fejlesztése, megadva. A tár bővítmény módszerek és leegyszerűsíti a .NET-kód segítő függvényeket tartalmazza. Mindkét tárak **NuGet** és **GitHub**keresztül érhetők el.


##<a name="prerequisites"></a>Előfeltételek

-   Egy új vagy meglévő Azure-előfizetésben Media Services fiókot. Olvassa el a [Media Services-fiók létrehozása](media-services-portal-create-account.md).
-   Operációs rendszer: A Windows 10, Windows 7, Windows 2008 R2 vagy Windows 8-ban.
-   .NET-keretrendszer 4.5.
-    Visual Studio 2015, a Visual Studio 2013, a Visual Studio 2012 vagy a Visual Studio 2010 SP1 (Professional, prémium, Ultimate vagy Express).


##<a name="create-and-configure-a-visual-studio-project"></a>Létrehozása és konfigurálása a Visual Studio projekt

Ez a szakasz bemutatja, hogyan hozhat létre a projekt a Visual Studióban, és Media Services fejlesztési tagjának beállítja.  A projekt ebben az esetben C# Windows konzol alkalmazás, de más típusú projektek Media Services alkalmazás (például a Windows-űrlapok alkalmazások vagy egy olyan ASP.NET webalkalmazást) készíthet alkalmazása ábrán beállítási lépéseket.

Ez a szakasz megtudhatja, hogy hogyan **NuGet** Media Services .NET SDK és más függő tárak hozzáadása.

Azt is megteheti a legújabb Media Services .NET SDK bittel beolvasása GitHub ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) és [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), a megoldás, és adja hozzá az ügyfél projekt mutató hivatkozásokat. Ne feledje, hogy a szükséges függőségeket első letöltött automatikusan kibontása.

1. Új C# Console-alkalmazás létrehozása a Visual Studio 2010 SP1 vagy újabb VIEWBEN verziók. **Megoldás neve**, **helyét**és **nevét**, és kattintson az OK gombra.

2. A megoldás létrehozható.

2. **NuGet** segítségével telepítése, és adja hozzá az **Azure Media Services .NET SDK-bővítmények**. A csomag telepítésekor is **Media Services.NET SDK** telepíti, és hozzáadja szükséges függőségek.

    Győződjön meg arról, hogy a legújabb verziója telepítve NuGet. További információk és telepítési című cikkben olvashat [NuGet](http://nuget.codeplex.com/).

2. A megoldás Intézőben kattintson a jobb gombbal a projekt nevét, és válassza a NuGet kezelése csomagok...

    A NuGet csomagok kezelése párbeszédpanel jelenik meg.

3. Az Online gyűjtemény Azure MediaServices bővítmények keresése, válassza az Azure Media Services .NET SDK-bővítmények, és kattintson a telepítés gombra.

    A projekt módosul, és hivatkozásokat a Media Services .NET SDK-bővítmények, Media Services .NET SDK és más függő összeállítások kerülnek.

4. A napló is tisztább lesz fejlesztői környezet előléptetése, fontolja meg NuGet csomag visszaállítása. További tudnivalókért lásd: [NuGet csomag visszaállítása "](http://docs.nuget.org/consume/package-restore).

3. **System.Configuration** összeállítás mutató hivatkozás hozzáadása. Ez az összeállítás a System.Configuration tartalmazza. Konfigurációs fájl (például App.config) eléréséhez használt **ConfigurationManager** osztály.

    A hivatkozások kezelése párbeszédpanelen hivatkozás hozzáadásához kattintson a jobb gombbal a projekt nevét a megoldást Intéző. Válassza a Hozzáadás gombra, és a hivatkozások.

    A hivatkozások kezelése párbeszédpanel jelenik meg.

4. A .NET-keretrendszer szerelvények keresse meg és jelölje ki a System.Configuration összeállítás, és kattintson az OK gombra.
5. Nyissa meg a App.config fájlt (a fájl hozzáadása a projekthez Ha alapértelmezés szerint nem került), és egy *appSettings* szakasz hozzáadása a fájlhoz.     
Az Azure Media Services nevét és a fiók fiókkulcs, értékeinek beállítása, az alábbi példában látható módon.

    A név és a kulcs értékek megkeresése az Azure portált, és jelölje ki a fiókját. Beállítások ablakban a jobb oldalon megjelenik. A beállítások ablakban jelölje ki a billentyűk. A minden szövegmező melletti ikonra kattintva másolja át az értéket, a rendszer a vágólapra.


        <configuration>
        ...
            <appSettings>
              <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
              <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
            </appSettings>

        </configuration>

6. A meglévő **használata** kimutatásokban elején Program.cs fájl felülírja a következő kódot.

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;

Ezen a ponton készen áll a Media Services-alkalmazások fejlesztése indításához.    


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

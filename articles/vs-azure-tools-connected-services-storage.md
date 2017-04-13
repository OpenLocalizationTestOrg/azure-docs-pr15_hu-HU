<properties 
   pageTitle="Azure-tár hozzáadása a Visual Studio segítségével a csatlakoztatott szolgáltatások |} Microsoft Azure"
   description="Azure tárhely hozzáadása az alkalmazásban a Visual Studio csatlakoztatott-szolgáltatás hozzáadása párbeszédpanel használatával"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="storage"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="adding-azure-storage-by-using-visual-studio-connected-services"></a>Azure tárhely hozzáadása a Visual Studio csatlakoztatott szolgáltatások

## <a name="overview"></a>– Áttekintés

A Visual Studio Skype 2015 lehet csatlakozni bármely C# felhőalapú szolgáltatás, .NET kódmentes mobilszolgáltatás, ASP.NET-webhely vagy szolgáltatás, ASP.NET 5 szolgáltatás vagy Azure WebJob szolgáltatás Azure tárolóhoz a **Csatlakoztatott szolgáltatások hozzáadása** párbeszédpanel segítségével. A csatlakoztatott szolgáltatást összeadja a szükséges hivatkozásokat és a kapcsolat kódot, és a konfigurációs fájlok megfelelően módosítja. A párbeszédpanelen is megjeleníti, amely elárulja, a következő lépésekkel blob-tárolóhoz, a sorok és a táblázatokat indítása dokumentáció.

## <a name="supported-project-types"></a>Támogatott projekttípus létrehozása

A csatlakoztatott szolgáltatások párbeszédpanel, amelyhez csatlakozni szeretne Azure tárterület az alábbi projekttípusok is használhatja.

- ASP.NET webes projektek

- ASP.NET 5 projektek

- .NET felhőalapú szolgáltatás webes szerepkör és dolgozó szerepkör projektek

- .NET mobil szolgáltatások projektek

- Azure WebJob projektek


## <a name="connect-to-azure-storage-using-the-connected-services-dialog"></a>Csatlakozás a csatlakoztatott szolgáltatások párbeszédpanelen Azure-tárolóhoz

1. Győződjön meg arról, hogy fiókja van, az Azure. Ha nincs Azure-fiók, jelentkezzen az [ingyenes próbaverziót](http://go.microsoft.com/fwlink/?LinkId=518146). Ha befejezte az Azure-fiók, tárterület-fiókok létrehozása, mobil szolgáltatások létrehozása és konfigurálása az Azure Active Directory.

1. Nyissa meg a projekt a Visual Studióban, nyissa meg a helyi menü **hivatkozás** csomópont Solution Explorer elemre, és válassza a **Csatlakoztatott szolgáltatás hozzáadása**lehetőséget.

    ![A csatlakoztatott szolgáltatás hozzáadása](./media/vs-azure-tools-connected-services-storage/IC796702.png)

1. **Csatlakoztatott szolgáltatás hozzáadása** párbeszédpanelen válassza az **Azure tárhely**, és kattintson a **Konfigurálás** gombra. Jelentkezzen be az Azure, ha még nem tette kérheti.

    ![Csatlakoztatott szolgáltatás szolgáló párbeszédpanel - tárhely](./media/vs-azure-tools-connected-services-storage/IC796703.png)

1. **Azure tároló** párbeszédpanelen jelöljön ki egy meglévő tárterület-fiókot, és válassza a **Hozzáadás**gombra.

    Ha hozzon létre egy új tárterület-fiókot kell lépjen a következő lépéssel. Egyéb esetben ugorjon a 6.

    ![Azure tárolására szolgáló párbeszédpanel](./media/vs-azure-tools-connected-services-storage/IC796704.png)

1. Tárterület új fiók létrehozása: 

    1. Válassza az Azure tárolási párbeszédpanel alján a **tárhely új fiók létrehozása** gombra.

    1. Töltse ki a **Tárterület-fiók létrehozása** párbeszédpanelen, és kattintson a **Létrehozás** gombra.
    
        ![Azure tárolására szolgáló párbeszédpanel](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)

        Ha vissza az **Azure tárolási** párbeszédpanel, az új tároló megjelent a listában.

    1. Jelölje ki az új tároló a listában, és a **Hozzáadás**gombra.

1. A csatlakoztatott tárhelyszolgáltatáshoz a szolgáltatás hivatkozások csomópontot a WebJob projekt alatt jelenik meg.

    ![A feladatok a project web Azure tárhely](./media/vs-azure-tools-connected-services-storage/IC796705.png)

1. Tekintse át az első lépések lapja jelenik meg, és megtudhatja, hogy a projekt módosításának. Első lépések lap jelenik meg a böngészőben, amikor a csatlakoztatott szolgáltatás hozzáadása. Nézze meg az javasolt a következő lépések és kód példákat, vagy lépjen a Mi történt a lapra, mely hivatkozások lettek hozzáadva a projekthez, és hogyan kódot és konfigurációs fájlok módosították.

## <a name="how-your-project-is-modified"></a>Hogyan a projekt módosul.

Amikor elkészült a párbeszédpanelen, a Visual Studio hivatkozások hozzáadása, és bizonyos konfigurációs fájl módosítja. Az adott módosításokat a projekt típusától függ. 

 - ASP.NET-projektekhez megtudhatja, [Mi történt – ASP.NET projektek](http://go.microsoft.com/fwlink/p/?LinkId=513126). 
 - ASP.NET-5 projektekhez megtudhatja, [Mi történt – ASP.NET 5 projektek](http://go.microsoft.com/fwlink/p/?LinkId=513124). 
 - A felhőalapú szolgáltatás projektek (webes és dolgozó szerepkörhöz), megtudhatja, [Mi történt – Felhőszolgáltatásba projektek](http://go.microsoft.com/fwlink/p/?LinkId=516965). 
 - WebJob projektekhez megtudhatja, [Mi történt - WebJob projektek](./storage/vs-storage-webjobs-what-happened.md).

## <a name="next-steps"></a>Következő lépések

1. Útmutató az első lépések mintakódok használ, hoz létre, amelyhez tároló típusú, és kezdje a tárterület-fiókja elérésére kódírás!

1. Kérdések és segítség kérése
     - [Fórum az MSDN webhelyen: Azure tároló](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)

     - [Azure tároló csapatának blogja](http://blogs.msdn.com/b/windowsazurestorage/)

     - [A azure.microsoft.com tárhely](https://azure.microsoft.com/services/storage/)

     - [Tárterület-dokumentációt a azure.microsoft.com](https://azure.microsoft.com/documentation/services/storage/)


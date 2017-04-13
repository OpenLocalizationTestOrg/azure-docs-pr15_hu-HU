<properties 
    pageTitle="Az Azure RemoteApp QuickBooks telepítése |} Microsoft Azure" 
    description="Megtudhatja, hogyan oszthat meg a QuickBooks Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-do-you-deploy-quickbooks-in-azure-remoteapp"></a>Hogyan telepíthető az Azure RemoteApp QuickBooks?

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Az alábbi információk segítségével QuickBooks-alkalmazás Azure RemoteApp megosztása.


Azure RemoteApp QuickBooks Skype 2015 vállalati megoszthatja egy hibrid, illetve a felhőbe a webhelycsoportban. A vállalat fájlt egy virtuális futtató a QuickBooks adatbázis-kiszolgáló, amely az Azure RemoteApp kiszolgálókról külön kell lennie. Soha nem tárolja a vállalat fájlt az Azure RemoteApp képe – Ha ezek az adatok elvesztését várható. Csak a QuickBooks vállalati támogatja a QuickBooks-fájl a QuickBooks-adatbázis kiszolgálója szabványos Windows hálózati keresztül érhető el a külső megosztás szolgáltatója.   

> [AZURE.IMPORTANT] A QuickBooks adatbázis-kiszolgáló, amelyen a vállalat fájlt egy külön virtuális belül az azonos VNET a Azure RemoteApp gyűjteménnyel kell lennie.  

## <a name="steps-to-deploy-quickbooks"></a>A QuickBooks üzembe lépések

1. Hozzon létre egy Azure virtuális, QuickBooks, QuickBooks adatbázis-kiszolgáló, telepítése, és helyezze a vállalat fájlt egy Azure virtuális.  Ellenőrizze, hogy megfelelően konfigurálni tűzfalszabályokat.
2. Telepítse a QuickBooks [egyéni képet](remoteapp-imageoptions.md) , és létrehozása az [Azure RemoteApp webhelycsoport](remoteapp-collections.md), vagy a felhőben, vagy a hibrid belül a pontos azonos VNET a virtuális szolgáltatója a QuickBooks-adatbázis kiszolgálója a munkahelyi fájlok helye. 
3.  [Közzététel](remoteapp-publish.md) Felhasználók QuickBooks-alkalmazás
4.  Indítsa el az Azure RemoteApp által üzemeltetett QuickBooks-ügyfél, nyissa meg a szabványos Windows hálózati a virtuális gép szolgáltatója a QuickBooks adatbázis-kiszolgáló használatával, és nyissa meg a vállalat fájlt. 

## <a name="documentation-references"></a>Dokumentáció hivatkozások

- QuickBooks [konfigurációk támogatott](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)
- QuickBooks [telepítési lehetőségek](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)

Is érdemes a Ignite bemutató, [a Microsoft Azure RemoteApp felügyelet kapcsolatos Alapismeretekről és kezelés](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) – 1:02:45 értéket a QuickBooks rész eléréséhez az előretekerés.

## <a name="deployment-architecture"></a>Telepítési architektúra

![QuickBooks + Azure RemoteApp telepítési](./media/remoteapp-quickbooks/ra-quickbooks.png)
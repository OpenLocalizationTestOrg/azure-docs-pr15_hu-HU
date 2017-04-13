<properties
    pageTitle="Gépi tanulási PowerShell-modult |} Microsoft Azure"
    description="A PowerShell-modult az Azure gépi tanulási nyilvános kép módban érhető el. Munkaterületek, kísérletek, web services és további létrehozása és kezelése a PowerShell használatával."
    keywords="kísérletezés a lineáris regressziós, gépi tanulási algoritmusok, gépi tanulási oktatóprogram, prediktív modellezési technikák, adatok tudományos kísérlet"
    services="machine-learning"
    documentationCenter=""
    authors="hning86"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/05/2016"
    ms.author="garye;haining"/>

# <a name="powershell-module-for-microsoft-azure-machine-learning"></a>Microsoft Azure gépi tanulási PowerShell-modult

A PowerShell-modult az Azure gépi tanulási, amely lehetővé teszi, hogy a Windows PowerShell-lel való kezelése a munkaterületek, kísérletek, adatkészleteket, webes serivces és az egyéb hatékony eszköz.

Nézze át a dokumentációt, és töltse le a modul teljes forráskód a [https://aka.ms/amlps](https://aka.ms/amlps)együtt. 

## <a name="what-is-the-machine-learning-powershell-module"></a>Mi az a gépi tanulási PowerShell-modult?

A gépi tanulási PowerShell-modult van egy. NETTÓ-alapú DLL modul, amely lehetővé teszi, hogy teljesen Azure gépi tanulási munkaterületek, kísérletek, adatkészleteket, web services és az web service végpontok kezelése a Windows PowerShell. Együtt a modul töltse le a teljes forráskód tisztán tabulátorral tagolt [API-C# réteg](https://github.com/hning86/azuremlps/blob/master/code/AzureMLSDK.cs)tartalmazó. Ez azt jelenti, hogy DLL hivatkozást a saját .NET projektből, és Azure gépi tanulási kezelése a .NET-kódot. Ezeken kívül DLL-szolgáltatásfájlját az alapul szolgáló REST API-hoz, amely közvetlenül a kedvenc ügyfélprogramból is élvezheti függ.

## <a name="what-can-i-do-with-the-powershell-module"></a>Mire van lehetőségem a PowerShell-modult tartalmazó?

Az alábbiakban a PowerShell-modult elvégezhető feladatokat. Nézze meg a [teljes dokumentáció](https://aka.ms/amlps) az ezek és sok új funkciót.

- Új munkaterület management tanúsítvánnyal ([Új-AmlWorkspace](https://github.com/hning86/azuremlps#new-amlworkspace)) kiépítése
- Exportálása és importálása, amely egy kísérlet graph ([Export-AmlExperimentGraph](https://github.com/hning86/azuremlps#export-amlexperimentgraph) és [Importálás-AmlExperimentGraph](https://github.com/hning86/azuremlps#import-amlexperimentgraph)) JSON fájl
- Egy kísérlet ([Start-AmlExperiment](https://github.com/hning86/azuremlps#start-amlexperiment)) futtatása
- Hozzon létre egy webszolgáltatásból ki a cserélendő kísérletet ([Új-AmlWebService](https://github.com/hning86/azuremlps#new-amlwebservice))
- Hozzon létre egy új végpontot közzétett webes szolgáltatás ([Hozzáadás-AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint))
- Meghívása egy Erőforrásrekordok és/vagy BES webes szolgáltatás végpontjának ([Invoke-AmlWebServiceRRSEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) és [Invoke-AmlWebServicBESEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint))

Íme egy gyors példa egy meglévő kísérlet futtatásához a PowerShell használatával:

        #Find the first Experiment named “xyz”
        $exp = (Get-AmlExperiment | where Description -eq ‘xyz’)[0]
        #Run the Experiment
        Start-AmlExperiment -ExperimentId $exp.ExperimentId 

Részletesebb használatieset-ismertető témakört is be a PowerShell-modulról automatizálhatja a nagyon gyakran kért tevékenység: [sok gépi tanulási modellek és webes szolgáltatás végpontok egy kísérlet PowerShell használatával hozhat létre](machine-learning-create-models-and-endpoints-with-powershell.md).

## <a name="how-do-i-get-started"></a>Hogyan kezdjek hozzá?

Első lépések a gépi tanulási PowerShell, töltse le a [package felszabadítása](https://github.com/hning86/azuremlps/releases) GitHub, és kövesse a [telepítési utasításokat](https://github.com/hning86/azuremlps/blob/master/README.md). A letöltött/kibontott DLL tiltásának feloldása, és importálja a PowerShell környezetet kell. A parancsmagok a legtöbb kell adnia a munkaterület-Azonosítóját, a munkaterület-jóváhagyási jogkivonat és az Azure terület, amely a munkaterület a. A megadását ezek legegyszerűbben keresztül alapértelmezett config.json fájlt, amelyre részletesen a telepítési utasításokat. Természetesen is a mely számjegy fa klónozhatja, és a kód helyileg a Visual Studio segítségével módosíthatja, fordítási.

## <a name="next-steps"></a>Következő lépések

A PowerShell-modult továbbfejlesztett és a kibontott a betekintő időszak alatt továbbra is. Figyelemmel a [Cortana üzletiintelligencia- és gépi tanulási Blog](https://blogs.technet.microsoft.com/machinelearning/) további híreket és információkat.

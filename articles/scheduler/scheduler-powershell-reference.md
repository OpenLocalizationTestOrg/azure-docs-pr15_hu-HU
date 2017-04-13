<properties
 pageTitle="A Feladatütemező PowerShell-parancsmagok hivatkozás"
 description="A Feladatütemező PowerShell-parancsmagok hivatkozás"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-powershell-cmdlets-reference"></a>A Feladatütemező PowerShell-parancsmagok hivatkozás

Az alábbi táblázat ismerteti, valamint a Azure ütemező a fő parancsmagokkal hivatkozás lapján mutató hivatkozásokat.

Azure PowerShell telepítése, és azt társítása Azure-előfizetése, megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md). 

[Azure erőforrás-kezelő parancsmagok](https://msdn.microsoft.com/library/mt125356\(v=azure.200\).aspx)kapcsolatos további tudnivalókért lásd: [Azure PowerShell használatá Azure erőforrás-kezelővel](../powershell-azure-resource-manager.md).

|Parancsmag|Parancsmag leírásának|
|---|---|
[Tiltsa le-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490133\(v=azure.200\).aspx) |Egy feladat webhelycsoport letiltja. 
[Enable-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490135\(v=azure.200\).aspx) |Lehetővé teszi, hogy a feladat gyűjteménye.
[Get-AzureRmSchedulerJob](https://msdn.microsoft.com/library/mt490125\(v=azure.200\).aspx) |A Feladatütemező feladatok kap.
[Get-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490132\(v=azure.200\).aspx) |Lekérdezi gyűjtemények feladat.
[Get-AzureRmSchedulerJobHistory](https://msdn.microsoft.com/library/mt490126\(v=azure.200\).aspx) |Lekérdezi előzmények feladat.
[Új AzureRmSchedulerHttpJob](https://msdn.microsoft.com/library/mt490136\(v=azure.200\).aspx) |Létrehoz egy HTTP feladatot.
[Új AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490141\(v=azure.200\).aspx) |Létrehoz egy feladatot webhelycsoport.
[Új AzureRmSchedulerServiceBusQueueJob](https://msdn.microsoft.com/library/mt490134\(v=azure.200\).aspx) |A szolgáltatás bus várólistás feladat létrehozása
[Új AzureRmSchedulerServiceBusTopicJob](https://msdn.microsoft.com/library/mt490142\(v=azure.200\).aspx) |Létrehoz egy szolgáltatás bus tematikus feladatot.
[Új AzureRmSchedulerStorageQueueJob](https://msdn.microsoft.com/library/mt490127\(v=azure.200\).aspx) |Létrehoz egy tároló várólista feladatot. 
[Eltávolítás-AzureRmSchedulerJob](https://msdn.microsoft.com/library/mt490140\(v=azure.200\).aspx) |Eltávolítja a Feladatütemező feladatot.  
[Eltávolítás-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490131\(v=azure.200\).aspx) |Egy feladat webhelycsoport eltávolítja. 
[Set-AzureRmSchedulerHttpJob](https://msdn.microsoft.com/library/mt490130\(v=azure.200\).aspx) |A Feladatütemező HTTP feladat módosítja.
[Set-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490129\(v=azure.200\).aspx) |Egy feladat webhelycsoport módosítja. 
[Set-AzureRmSchedulerServiceBusQueueJob](https://msdn.microsoft.com/library/mt490143\(v=azure.200\).aspx) |A szolgáltatás bus várólistás feladat módosítja.  
[Set-AzureRmSchedulerServiceBusTopicJob](https://msdn.microsoft.com/library/mt490137\(v=azure.200\).aspx) |Módosítja a szolgáltatás bus tematikus feladatot. 
[Set-AzureRmSchedulerStorageQueueJob](https://msdn.microsoft.com/library/mt490128\(v=azure.200\).aspx) |A tároló várakozási sorban található feladatok módosítása.   

További információ a következő parancsmagok bármelyikét futtathatja: 

```
Get-Help <cmdlet name> -Detailed
```
```
Get-Help <cmdlet name> -Examples
```
```
Get-Help <cmdlet name> -Full
```

## <a name="see-also"></a>Lásd még:


 [Mi az a Feladatütemező?](scheduler-intro.md)

 [Azure ütemező fogalmak, kifejezések és entitás hierarchia](scheduler-concepts-terms.md)

 [Az Azure-portálon a Feladatütemező használatának első lépései](scheduler-get-started-portal.md)

 [Tervek és a számlázásra az Azure ütemező](scheduler-plans-billing.md)

 [Azure ütemező REST API-hivatkozás](https://msdn.microsoft.com/library/mt629143)

 [Azure ütemező magas rendelkezésre állás és megbízhatóság](scheduler-high-availability-reliability.md)

 [Azure ütemező korlátozások, alapértelmezett és hibakódok esetén](scheduler-limits-defaults-errors.md)

 [Azure ütemező kimenő hitelesítést](scheduler-outbound-authentication.md)

<properties 
    pageTitle="Hogyan lehet hozzáadni a kódolási mennyiség" 
    description="Megtudhatja, hogy miként vehet fel a .NET-kódolási egységek hogyan"  
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2016"
    ms.author="juliako;milangada;gtrifonov"/>


#<a name="how-to-scale-encoding-with-net-sdk"></a>Ha át kívánja méretezni, .NET SDK kódolást hogyan

> [AZURE.SELECTOR]
- [Portál](media-services-portal-scale-media-processing.md )
- [.NET](media-services-dotnet-encoding-units.md)
- [TÖBBI](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="overview"></a>– Áttekintés

>[AZURE.IMPORTANT] Győződjön meg arról, tekintse át a témakör a [áttekintése](media-services-scale-media-processing-overview.md) témakör feldolgozása media méretezés további információkra.
 
Ha módosítani szeretné a fenntartott egység, és a kódolás .NET SDK használatával fenntartott egységek számát, tegye a következőket:

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds to S1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);
    
    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();
    
    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

##<a name="opening-a-support-ticket"></a>A támogatási jegyek megnyitása

Alapértelmezés szerint minden Media Services fiók legfeljebb 25 kódolási és méretezheti 5 igény szerinti, a folyamatos átvitelű fenntartott egységek. Kérheti magasabb korlátozott támogatás jegy megnyitásával.

###<a name="open-a-support-ticket"></a>Nyissa meg a támogatási jegyek

Nyissa meg a támogatási jegyek tegye a következőket:

1. Kattintson a [technikai támogatás kérése](https://manage.windowsazure.com/?getsupport=true)gombra. Ha nem jelentkezett be, a rendszer kéri, a hitelesítő adatok.

1. Jelölje ki azt az előfizetést.

1. A támogatás típusa csoportban jelölje be a "Műszaki".

1. Kattintson a "Jegy létrehozása".

1. Jelölje be a "Azure Media Services" a terméklistában mutatják be a következő lapon.

1. Jelölje ki a "hiba"típusát, amely megfelel a problémát.

1. Kattintson a Tovább gombra.

1. A következő lapon kövesse az utasításokat, és írja be a probléma részleteit.

1. Kattintással nyissa meg a jegy nyújt.



##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

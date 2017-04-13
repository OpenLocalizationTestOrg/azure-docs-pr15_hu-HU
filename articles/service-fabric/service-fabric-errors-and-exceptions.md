<properties
   pageTitle="Gyakori FabricClient kivételek |} Microsoft Azure"
   description="A közös kivételek és hibák, amelyek az alkalmazások és a fürt adatkezelési műveletek végrehajtása során az FabricClient API-k által is elő mutatja be."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>

# <a name="common-exceptions-and-errors-when-working-with-the-fabricclient-apis"></a>Gyakori kivételek és hibákat, amikor a FabricClient API-k használata
A [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) API-k engedélyezése felügyeleti feladatok szolgáltatás háló alkalmazás, szolgáltatás, vagy fürt fürt és az alkalmazás számára. Például alkalmazás telepítési, frissítés, és az Eltávolítás a fürt állapot-ellenőrzés vagy a tesztelés szolgáltatás. Alkalmazásfejlesztő és fürt rendszergazdák segítségével a FabricClient API-k eszközök kezeléséhez, a szolgáltatás háló fürt és alkalmazások fejlesztése.

Vannak olyan műveleteket, amelyek végezhető el FabricClient számos különböző típusú.  Mindegyik módszernek is throw a Kivételek hatóköre hibák miatt nem a megfelelő beviteli, futási idejű hiba vagy infrastruktúra átmeneti problémák.  A dokumentációjában API hivatkozás található, mely a kivételek okozott egy adott módszerrel. Néhány kivételek vannak, azonban, amely számos különböző [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) API-khoz is lehet vissza. Az alábbi táblázat a kivételeket, amelyek megegyeznek a végig a FabricClient API-k.

|Kivétel| Ha váltott|
|---------|:-----------|
|[System.Fabric.FabricObjectClosedException](https://msdn.microsoft.com/library/system.fabric.fabricobjectclosedexception.aspx)|A [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) objektum zárt állapotban van. A [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) objektum használ, és hozható létre egy új [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) objektum megszabadulni. |
|[System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx)|A művelet időtúllépés. [OperationTimedOut](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) ad vissza, ha a művelet elvégzéséhez MaxOperationTimeout több tart.|
|[System.UnauthorizedAccessException](https://msdn.microsoft.com/en-us/library/system.unauthorizedaccessexception.aspx)|A hozzáférés-ellenőrzés a művelet sikertelen volt. E_ACCESSDENIED adja vissza.|
|[System.Fabric.FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx)|Futási idejű hiba történt a művelet végrehajtása. Az FabricClient módszerekkel is esetleg throw [FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx), a [Hibakód](https://msdn.microsoft.com/library/system.fabric.fabricexception.errorcode.aspx) tulajdonság adja meg pontosan a probléma okát, a kivétel. A [FabricErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) felsorolás hibakódok definiálva.|
|[System.Fabric.FabricTransientException](https://msdn.microsoft.com/library/system.fabric.fabrictransientexception.aspx)|A művelet valamilyen feltétel tranziens hiba miatt sikertelen volt. Művelet meghiúsulhat például mert kópiák határozatképességének átmenetileg nem érhető. Ideiglenes (tranziens) kivételeket is meg kell ismételni sikertelen műveletek felelnek meg.|

Néhány gyakori [FabricErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) hibák egy [FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx)visszaküldött:

|Hibaüzenet| Feltétel|
|---------|:-----------|
|CommunicationError|A kapcsolati hiba miatt a művelet sikertelen, ismételje meg a műveletet.|
|InvalidCredentialType|Érvénytelen hitelesítő adatok típusát.|
|InvalidX509FindType|A X509FindType érvénytelen.|
|InvalidX509StoreLocation|A X509 tárolóhely argumentum valamelyike érvénytelen.|
|InvalidX509StoreName|A X509 tár neve nem érvényes.|
|InvalidX509Thumbprint|A X509 tanúsítvány ujjlenyomat karakterlánc érvénytelen.|
|InvalidProtectionLevel|A védelem szint érvénytelen.|
|InvalidX509Store|A tanúsítvány tároló nem nyitható meg X509.|
|InvalidSubjectName|A tárgy érvénytelen.|
|InvalidAllowedCommonNameList|Közös neve listában karakterlánc formátuma nem érvényes. Vesszővel tagolt listáját kell tenni.|

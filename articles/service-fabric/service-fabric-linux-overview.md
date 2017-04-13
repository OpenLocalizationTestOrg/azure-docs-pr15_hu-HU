<properties
   pageTitle="Azure Service háló Linux |} Microsoft Azure"
   description="Szolgáltatás háló fürt Linux és Java, ami azt jelenti is üzembe és host szolgáltatás háló alkalmazásai Java a és C# Linux támogatja."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="Java"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="SubramaR"/>

# <a name="service-fabric-on-linux"></a>Szolgáltatás háló Linux

A szolgáltatás háló Linux mintája lehetővé teszi, hogy a létrehozásához, telepítéséhez és alkalmazáskezelési könnyen hozzáférhető, nagyon méretezhető Linux ugyanúgy, mint a Windows. A szolgáltatás háló keretek (megbízható szolgáltatások és megbízható szereplők) mellett C# (.NET Core) Linux Java érhetők el.  [A vendégként való bekapcsolódáshoz végrehajtható szolgáltatások](service-fabric-deploy-existing-app.md) nyelvi vagy keretrendszer is készíthet. Ezeken kívül az előnézet támogatja orchestrating Docker tárolók is. A vendégként való bekapcsolódáshoz végrehajtható, illetve natív szolgáltatás háló szolgáltatások, használja a szolgáltatás háló keretek docker tárolók futtatását is lehetővé teszi.

Szolgáltatás háló Linux fogalmilag szolgáltatás háló Windows (kivéve a OS adatai és a nyelv támogatását programozási). A [meglévő dokumentáció](http://aka.ms/servicefabricdocs) a legtöbb ily módon segít a technológia megismerkedhet a vonatkozik.

> [AZURE.VIDEO service-fabric-linux-preview]

## <a name="supported-operating-systems-and-programming-languages"></a>Támogatott operációs rendszerek és programozási nyelven

A korlátozott előzetes verzió többszörös gép fürt mellett egy beépített fejlesztési fürt létrehozását támogatja a rendszert futtató kiszolgáló-16.04 Ubuntu Azure-ban. Az előnézet támogatja a megbízható szereplők és a megbízható állapot nélküli szolgáltatások keretek Java a és C# Vendég végrehajtható és Docker tárolók orchestrating mellett.  

>[AZURE.NOTE] Megbízható gyűjtemények nem támogatottak a Linux még. Nem támogatja a készenléti egyedül fürt – akár csak egy mezőt, és Azure Linux több gép fürt a nyomtatási képen támogatottak.

## <a name="supported-tooling"></a>Támogatott szerszámok

Az előnézet támogatja a fürt Azure CLI keresztül folytatott kommunikációt. Java-fejlesztők számára integráció a Holdas és Yeoman ellátni Holdas Linux és OSX támogatott. OSX integrációja használja egy Linux virtuális vagrant keresztül a motorháztető alatt fülre. A C# fejlesztők számára integráció a Yeoman kapja meg alkalmazássablonok készítése.

## <a name="next-steps"></a>Következő lépések


1. Megismerkedhet a [Megbízható szereplők](service-fabric-reliable-actors-introduction.md) és [Megbízható szolgáltatások](service-fabric-reliable-services-introduction.md) programozási keretek.

2. [A fejlesztői környezet Linux előkészítése](service-fabric-get-started-linux.md)

3. [A fejlesztői környezet a OSX előkészítése](service-fabric-get-started-mac.md)

4. [Az első szolgáltatás háló Java-alkalmazás létrehozása Linux](service-fabric-create-your-first-linux-application-with-java.md)

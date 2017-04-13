<properties
    pageTitle="Web App erőforrások áthelyezése egy másik erőforráscsoport"
    description="Ez a témakör a hol meg lehet váltani Web Apps alkalmazások és szolgáltatások alkalmazás egy erőforráscsoport."
    services="app-service"
    documentationCenter=""
    authors="ZainRizvi"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/04/2016"
    ms.author="zarizvi"/>
    
# <a name="supported-move-configurations"></a>Támogatott áthelyezés konfigurációk

Áthelyezheti a [ARM áthelyezése erőforrások Api](../resource-group-move-resources.md)Azure Web App erőforrásokat.

Azure Web Apps alkalmazások jelenleg támogatja az alábbi áthelyezés jelenik meg:

* Erőforráscsoport (a web Apps alkalmazások, a app milyen szolgáltatáscsomagok és a tanúsítványok) teljes tartalmának áthelyezése egy másik erőforráscsoport 
    * Megjegyzés: A cél erőforráscsoport nem tartalmazhat ebben az esetben Microsoft.Web erőforrások
* Egyes webalkalmazások áthelyezése egy másik erőforráscsoport, miközben továbbra is a jelenlegi alkalmazás (a régi erőforráscsoport megmarad az alkalmazás szolgáltatáscsomagja) szolgáltatás csomagban szolgáltatója őket

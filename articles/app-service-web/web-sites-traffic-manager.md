<properties
    pageTitle="Azure web app forgalom Azure forgalom Manager szabályozása"
    description="Ebben a cikkben összegzést Azure forgalom Manager Azure web Apps alkalmazások vonatkozik."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    writer="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/25/2016"
    ms.author="cephalin"/>

# <a name="controlling-azure-web-app-traffic-with-azure-traffic-manager"></a>Azure web app forgalom Azure forgalom Manager szabályozása

> [AZURE.NOTE] Ebben a cikkben összegző adatai a Microsoft Azure forgalom Manager Azure alkalmazás Service Web Apps alkalmazások vonatkozik. További információ: Azure forgalom kezelő névjegye magát megtalálhatók a hivatkozásokat, ez a cikk végén található.

## <a name="introduction"></a>– Bevezetés
Azure forgalom Manager használatával szabályozhatja, hogy hogyan kerülnek terjesztésre a webes ügyfelek kérelmeket web Apps alkalmazások Azure App szolgáltatásban. Azure forgalom Manager profilhoz web app végpontok felvétele után Azure forgalom Manager nyomon követi az állapotát (fut, leállítva vagy törölt), a web Apps alkalmazások, így eldöntheti, amely ezeket a végpontok kell kapniuk a forgalmat.

## <a name="load-balancing-methods"></a>Terheléselosztó módszerek betöltése
Azure forgalom Manager három különböző betöltés terheléselosztó módszert használja. Ezek megkötésekről a következő lista szerint Azure web Apps alkalmazások eseménykódok.

* **Feladatátvevő**: esetén web app klónok különböző régiókban is használja ezt a módszert egy webalkalmazás az összes webes ügyfelek forgalom szolgáltatás konfigurálása, és egy másik web app beállítása egy másik régióbeli karbantartására, hogy a forgalmat, abban az esetben, ha az első web App alkalmazásban nem érhető el.

* **Ciklikus**: esetén web app klónok különböző régiókban is használhatja ezt a módszert egyaránt elosztása forgalmat a web Apps alkalmazások különböző régiókban.

* **Teljesítményét**: A teljesítményét módszer elosztja a forgalom az ügyfeleknek legrövidebb üzenetváltási időt alapján. A teljesítmény módszerrel a web Apps alkalmazások ugyanabban a régióban belül, illetve különböző régiókban használható.

##<a name="web-apps-and-traffic-manager-profiles"></a>Web Apps alkalmazások és a forgalom Manager profilok
A vezérlő web app forgalom konfigurálása, hozzon létre egy profilt az Azure forgalom-kezelőben, használja a három közül betöltése terheléselosztó módszerekkel korábban, és adja hozzá az a végpontok (ebben az esetben az web Apps alkalmazások), amelynek meg szeretné adni a profil-alapú forgalmat. A web app állapot (fut, leállítása vagy törölt) rendszeresen tájékoztatni a profilhoz, úgy, hogy az Azure forgalom Manager is ennek megfelelően közvetlen forgalmat.

Azure forgalom Manager az Azure használatakor tartsa szem előtt az alábbiakat:

* Web app csak a azonos régión belüli telepítések esetén az alábbiakban Web Apps alkalmazások már és biztosít a feladatátvevő ciklikus tekintet nélkül a web app módban.

* Telepítési ugyanabban a régióban, amely a Web Apps alkalmazások használata az Azure valamelyik másik felhőszolgáltatásával együtt mindkét fajta ahhoz, hogy hibrid környezetek kialakításához végpontok is összevonhatja.

* Csak egy web app végpont / régió megadhatja egy profilt. Egy adott területre zárólap webalkalmazást választja, a hátralévő web Apps alkalmazások, az adott régióban válnak nem érhető el, hogy a profil kiválasztását.

* A web app végpontok Azure forgalom Manager profil megadott jelenik meg a **Domain Names** szakaszban a profil webalkalmazások számára a lap, de nem konfigurálható van.

* Miután webalkalmazást profilhoz, a **Webhely URL-CÍMÉT** az irányítópulton a web app portál lapja a egyéni tartományi URL-CÍMÉT a web app megjeleníti, ha egy be van állítva. Egyéb esetben megjelenítése a forgalom Manager profil URL-CÍMÉT (például `contoso.trafficmgr.com`). Mindkét közvetlen tartomány nevét a web app és a forgalom kezelőjének URL-címe lesz látható a web app beállítása lapon a **Domain Names** szakaszban.

* Az egyéni tartománynevet a várt módon működik, de beszúrásán őket a web Apps alkalmazások, be kell állítania is a DNS-térképre, mutasson a forgalom Manager URL-címét. Azure webalkalmazást egyéni tartomány beállítása olvashat tanulmányozza a [egy Azure webhelyet egyéni tartománynév beállítása](web-sites-custom-domain-name.md)című témakört.

* Csak a web Apps alkalmazások, amelyek Azure forgalom Manager profilhoz szabványos üzemmódban adhat hozzá.

## <a name="next-steps"></a>Következő lépések

Egy elvi és technikai áttekintés Azure forgalom Manager a [Forgalom Manager áttekintése](../traffic-manager/traffic-manager-overview.md)című témakörben találhat.

Forgalom segítségével az Web Apps alkalmazásokkal kapcsolatos további tudnivalókért olvassa el a blogbejegyzéseket [Azure forgalom Manager használata az Azure-webhelyek](http://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx) és [Azure forgalom Manager most integrálhatja a Azure webhelyek](https://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/)című témakört.

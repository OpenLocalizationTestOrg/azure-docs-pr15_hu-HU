<properties 
    pageTitle="Windows Phone Silverlight SDK – áttekintés" 
    description="A Windows Phone Silverlight SDK Azure mobil tetszés szerint elmélyedhet áttekintése"                     
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede"
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-sdk-overview-for-azure-mobile-engagement"></a>Windows Phone az Azure mobil tetszés szerint elmélyedhet a Silverlight SDK – áttekintés

Kezdje itt a részletek hogyan integrálása az Azure Mobile tetszés szerint elmélyedhet a Windows Phone-Silverlight alkalmazásban. Ha a szeretné tegyen egy próbát első, győződjön meg arról, hogy el kell végeznie a [15 perces oktatóprogram](mobile-engagement-windows-phone-get-started.md).

Ide kattintva megtekintheti a [Tartalom SDK](mobile-engagement-windows-phone-sdk-content.md)

##<a name="integration-procedures"></a>Integráció eljárások

1. Kezdje itt: [hogyan integrálja a Mobile tetszés szerint elmélyedhet a Windows Phone Silverlight alkalmazásban](mobile-engagement-windows-phone-integrate-engagement.md)

2. [Történő vannak (értesítések) a Windows Phone Silverlight alkalmazásban integrálásáról](mobile-engagement-windows-phone-integrate-engagement-reach.md) értesítések:

3. Terv végrehajtás nyomon követése: [a speciális Mobile tetszés szerint elmélyedhet címkézés API-ja a Windows Phone-Silverlight-alkalmazás használata](mobile-engagement-windows-phone-use-engagement-api.md)

##<a name="release-notes"></a>Kibocsátási megjegyzések

###<a name="330-04192016"></a>3.3.0 (04/19 vagy 2016)
A *MicrosoftAzure.MobileEngagement* nuget részét **v3.4.0** csomag

-   A felvett "TestLogLevel" API-val való engedélyezése vagy letiltása és szűrés konzol naplók a SDK által kibocsátott.

Korábbi verzió talál a [teljes kibocsátási megjegyzések](mobile-engagement-windows-phone-release-notes.md)

##<a name="upgrade-procedures"></a>Frissítési eljárások

Ha akkor már van beépül a SDK régebbi verzióját az alkalmazást, akkor a SDK frissítéskor, vegye figyelembe az alábbiakat.

Előfordulhat, hogy végre kell hajtani több eljárás, ha Ön kihagyott a SDK különböző verzióiban. A teljes [Frissítése eljárások](mobile-engagement-windows-phone-upgrade-procedure.md)találhat. Ha például telepít át 0.10.1 0.11.0 először menete a "feladó 0.9.0 való 0.10.1" kell majd a "from 0.10.1 való 0.11.0" eljárást.

###<a name="from-200-to-330"></a>A 2.0.0 való 3.3.0

####<a name="test-logs"></a>Naplók tesztelése

A SDK készített konzol naplók lehet most engedélyezett/letiltott/szűrt. Testreszabásához ez a tulajdonság frissítéséhez `EngagementAgent.Instance.TestLogEnabled` az egyik elérhető értékét a `EngagementTestLogLevel` számbavétel, például:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="upgrade-from-older-versions"></a>Régebbi verziójával frissítése

Lásd: a [eljárások frissítése](mobile-engagement-windows-phone-upgrade-procedure.md)
 

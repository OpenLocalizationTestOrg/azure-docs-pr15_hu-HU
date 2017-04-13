<properties 
    pageTitle="Azure WebJobs dokumentáció erőforrások" 
    description="Források az használatának elsajátításához Azure WebJobs és az Azure WebJobs SDK használata ajánlott." 
    services="app-service" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="tdykstra"/>

# <a name="azure-webjobs-documentation-resources"></a>Azure WebJobs dokumentáció erőforrások

## <a name="overview"></a>– Áttekintés

Ez a témakör a Azure WebJobs és az Azure WebJobs SDK használatával kapcsolatos dokumentációt erőforrásokra mutató hivatkozásokat tartalmaz. Azure WebJobs adja meg az [alkalmazás szolgáltatás web App alkalmazásban, API-alkalmazást, vagy mobilalkalmazásban](../app-service/app-service-value-prop-what-is.md)környezetben háttérfolyamatok parancsfájlok programok telepítése és futtatása ugyanígy. Töltse fel és a cmd karaktereket, például egy végrehajtható fájl futtatása Bát, exe (.NET), ps1, sh, php, másolása, js és üveg. Ezeket a programokat futtatása WebJobs időközönként (cron) vagy folyamatosan.

A [WebJobs SDK](websites-webjobs-resources.md) célja, a kód írhat a gyakori feladatok, hogy egy WebJob is végezhet, például kép feldolgozás, várólista feldolgozása, RSS összesítési, fájl karbantartási és e-mailek küldése egyszerűsítése érdekében. A WebJobs SDK csomagjában talál Azure-tárhely és a szolgáltatás Bus kezelése, a feladatok ütemezése és kezelése a hibák és sok más tipikus esetei beépített funkciókat tartalmaz. Ezeken kívül kell bővíthető célja, és egy [Nyissa meg a bővítmények forrás tárháza](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview). [Azure-funkciók](../azure-functions/functions-overview.md) (jelenleg az előzetes verzió) a WebJobs SDK csomagjában talál, hogy működik-e a C# parancsfájl, Node.js és más nyelvű változatában függően. 

Létrehozás, üzembe helyezése, WebJobs kezelése az a Visual Studio integrált szerszámok zökkenőmentesen elvégezhető. WebJobs létrehozása sablonokból, közzététele és kezelése (Futtatás/leállítása/monitor/hibakeresési) őket. 

Az WebJobs irányítópultot az Azure-portálon biztosít hatékony szolgáltatásairól, amelyek segítségével a teljes hozzáféréssel WebJobs, beleértve az azt jelenti, hogy egyes függvények belül WebJobs meghívása végrehajtását. Az irányítópult is szükséges függvény és a naplózás kimeneti jeleníti meg. 

##<a name="getstarted"></a>WebJobs és a WebJobs SDK első lépések

* [Azure WebJobs – bevezetés](http://www.hanselman.com/blog/IntroducingWindowsAzureWebJobs.aspx)
* [Azure WebJobs Soft és használatuk pillanatban kezdetének!](http://www.troyhunt.com/2015/01/azure-webjobs-are-awesome-and-you.html) (Troy Hunt blogbejegyzés.)
* [Azure WebJobs funkciók](/blog/2014/10/22/webjobs-goes-into-full-production/)
* [Mi az a WebJobs SDK](websites-dotnet-webjobs-sdk.md)
* [Háttér feladatok útmutatást Microsoft mintázatok és eljárások](/documentation/articles/best-practices-background-jobs/)
* [A 1.1.0 kihirdetése Microsoft Azure WebJobs SDK RTM](/blog/azure-webjobs-sdk-1-1-0-rtm/)
* [Az Azure WebJobs SDK – első lépések](websites-dotnet-webjobs-sdk-get-started.md)
* [A WebJobs SDK Azure várólista-tároló használata](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)
* [A WebJobs SDK Azure blob-tárolóhoz használata](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
* [Azure táblatárolója használata a WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)
* [A WebJobs SDK Azure Service Bus használata](websites-dotnet-webjobs-sdk-service-bus.md)
* [A WebJobs SDK GitHub, IFTTT és a HTTP példákkal WebHooks használata](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/WebHooks-Walkthrough)
* [Azure WebJobs SDK – rövid összefoglaló (PDF-fájl letöltése)](http://go.microsoft.com/fwlink/?LinkID=524028&clcid=0x409)
* [WebJobs beállítások dokumentáció GitHub](https://github.com/projectkudu/kudu/wiki/Web-jobs).
* Videók
    * [WebJobs és a WebJobs SDK](http://channel9.msdn.com/Shows/Cloud+Cover/Episode-153-WebJobs-with-Pranav-Rastogi?utm_source=dlvr.it&utm_medium=twitter)
    * [Azure WebJobs videósorozat a csatorna 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)
    * [Visual Studio szerszámok WebJobs bemutatása](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
    * [WebJobs szerszámok és távoli hibakeresés](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster)
    * [Azure WebJobs frissítés Pranav Rastogi - bővítési 1.1-es verzióban](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-183-Azure-WebJobs-Update-with-Pranav-Rastogi)

Lásd még: az alábbi szakaszok [WebJobs üzembe helyezése](#deploy) és [tesztelése és hibakeresés WebJobs](#debug).

##<a name="deploy"></a>WebJobs üzembe helyezése

* [Hogyan kell üzembe Azure WebJobs Visual Studio segítségével](websites-dotnet-deploy-webjobs.md)
* [Az Azure-portálon WebJobs telepítése](web-sites-create-web-jobs.md)
* [Azure WebJobs parancssori és folyamatos kézbesítve engedélyezése](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/)
* [Mely számjegy Azure WebJobs használata a .NET-konzol alkalmazás telepítése](http://blog.amitapple.com/post/73574681678/git-deploy-console-app/)
* [Azure F # WebJob telepítése](http://blogs.msdn.com/b/dave_crooks_dev_blog/archive/2015/02/18/deploying-f-web-job-to-azure.aspx)
* [Egyéni szolgáltatások mint Azure Webjobs üzembe helyezése](http://withouttheloop.com/articles/2015-06-23-deploying-custom-services-as-azure-webjobs/)
* Videók
    * [Visual Studio szerszámok WebJobs bemutatása](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
    * [WebJobs szerszámok és távoli hibakeresés](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

##<a name="schedule"></a>Ütemezési WebJobs

* [Az Azure WebJob párbeszédpanel hozzáadása](websites-dotnet-deploy-webjobs.md#configure)
* [Hozzon létre egy ütemezett WebJob az Azure-portálon](web-sites-create-web-jobs.md#CreateScheduled)
* [Egy WebJob ütemező feladat csatlakoztatása](http://blog.davidebbo.com/2015/05/scheduled-webjob.html)
* [Azure WebJobs ütemezési cron kifejezésekkel együtt](http://blog.amitapple.com/post/2015/06/scheduling-azure-webjobs/)
* [Egyes WebJob függvények használata a WebJobs SDK TimerTrigger ütemezése](websites-dotnet-webjobs-sdk.md#schedule)

##<a name="debug"></a>Tesztelés és hibakeresés WebJobs

* [Új fejlesztői és a Visual Studióban Azure WebJobs funkcióinak hibakeresése során](http://blogs.msdn.com/b/webdev/archive/2014/11/12/new-developer-and-debugging-features-for-azure-webjobs-in-visual-studio.aspx)
* [A WebJobs irányítópult megtekintése](websites-dotnet-webjobs-sdk-get-started.md#view-the-webjobs-sdk-dashboard)
* [Írja be a WebJobs SDK használatával naplók, és megtekintheti azokat az irányítópulton](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs)
* [Távoli hibakeresési WebJobs](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebugwj)
* [Ki a program, hogy a blob?](http://blogs.msdn.com/b/jmstall/archive/2014/02/19/who-wrote-that-blob.aspx) 
* [A felhőben interaktív kód szolgáltatónál](http://blogs.msdn.com/b/jmstall/archive/2014/04/26/hosting-interactive-code-in-the-cloud.aspx)
* [Azure WebJobs nyomkövetési hozzáadása](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx)
* [Figyelése, diagnosztizálása és a Microsoft Azure tároló – problémamegoldás](../storage/storage-monitoring-diagnosing-troubleshooting.md)
* Videók
    * [WebJobs szerszámok és távoli hibakeresés](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

##<a name="scale"></a>Méretezési WebJobs

* [A webes alkalmazás Azure-webhely méretezése](http://msdn.microsoft.com/magazine/dn786914.aspx)
* [Azure alkalmazás szolgáltatás: tömeges méreteket kész vállalati Web Apps alkalmazások Architecting](https://channel9.msdn.com/Events/Build/2014/3-626). Fedőlap WebJobs, beleértve a WebJobs SDK csomagjában talál a webalkalmazások méretezését.
* Videók
    * [Méretezés kifelé WebJobs](http://channel9.msdn.com/Shows/Azure-Friday/Azure-WebJobs-105-Scaling-out-Web-Jobs)

##<a name="additional"></a>További WebJobs források

* [Azure WebJobs Georgia blogbejegyzés Magnus Mårtensson](http://magnusmartensson.com/azure-webjobs-ga)
* [Powershell webes feladatok futó Azure alkalmazás szolgáltatás](http://blogs.msdn.com/b/nicktrog/archive/2014/01/22/running-powershell-web-jobs-on-azure-websites.aspx)
* [Az első értesítést kap az Azure WebJobs indított befejeződik.](http://blog.amitapple.com/post/2014/03/webjobs-notification/)
* [Egyszerű Web App biztonsági másolat WebJobs az adatmegőrzési szabály](https://azure.microsoft.com/blog/2014/04/28/simple-web-site-backup-retention-policy-with-webjobs/)
* [Azure Web Apps alkalmazások és az első kérésre lassú Cloud Services](http://wp.sjkp.dk/windows-azure-websites-and-cloud-services-slow-on-first-request/). Megtudhatja, hogy hogyan WebJobs hasonlóan a AlwaysOn szolgáltatás, amely csak a szokásos árak réteg érhető el.
* A [sikeres-e leállás WebJobs](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.U72Il_5OWUl). WebJobs SDK sikeres-e leállás, lásd: a [sikeres-e leállás](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).)
* [Azure WebJobs & AzCopy biztonsági mentés automatizálása](http://markjbrown.com/azure-webjobs-azcopy/)
* Videók
    * [Azure WebJobs videók Magnus Mårtensson szerint](https://www.youtube.com/playlist?list=PLqp1ZOYYUSd81yEzMYLTw8cz91wx_LU9r)
    * [Azure WebJobs videósorozat a csatorna 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

##<a name="additionalsdk"></a>További WebJobs SDK források

* [WebJobs SDK kibocsátási megjegyzések](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes)
* [WebJobs SDK forráskód](https://github.com/Azure/azure-webjobs-sdk)
* [WebJobs SDK bővítmények forráskód](https://github.com/Azure/azure-webjobs-sdk-extensions), a [részletes útmutató bővítési adatmodellhez](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).  
* [WebJobs SDK parancsprogram-forráskód](https://github.com/Azure/azure-webjobs-sdk-script/) ( [Azure függvények](../azure-functions/functions-overview.md)használt)
* [WebJob FREB fájlok feltöltése a WebJobs SDK használatával Azure-tárolóhoz](http://thenextdoorgeek.com/post/WAWS-WebJob-to-upload-FREB-files-to-Azure-Storage-using-the-WebJobs-SDK)
* [Azure kívüli Azure webjobs szolgáltatója, az az Azure naplózás előnyeivel üzemeltetett webjob](http://bypassion.dk/?p=510)
* [Azure WebJobs az adatok importálása eszköz létrehozása](http://www.freshconsulting.com/building-data-import-tool-azure-webjobs/)
* [Azure függvények áttekintése](../azure-functions/functions-overview.md)
* Videók
    * [Azure WebJobs videósorozat a csatorna 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

##<a name="samples"></a>Minta WebJob alkalmazások

* [A GitHub a WebJobs csoport által megadott minta alkalmazások](https://github.com/azure/azure-webjobs-sdk-samples)
* [Egyszerű Azure Web App használatával a WebJobs SDK WebJobs-fájlok](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb)
* [SiteMonitR](http://code.msdn.microsoft.com/SiteMonitR-dd4fcf77). Az ütemezett és eseményvezérelt WebJobs használati mutatja be. Tekintse meg az [újbóli létrehozása az Azure WebJobs SDK használatával SiteMonitR](http://www.bradygaster.com/post/rebuilding-the-sitemonitr-using-windows-azure-webjobs)közzé.

##<a name="blogs"></a>Blogok

* [Azure-blog](/blog)
* [Amit Apple blog](http://blog.amitapple.com/). A fókusz a WebJobs (nem a SDK).
* [Magnus Mårtensson blog](http://magnusmartensson.com/)

##<a name="gethelp"></a>Segítség WebJobs

* [A WebJobs StackOverflow](http://stackoverflow.com/questions/tagged/azure-webjobs)
* [Az a WebJobs SDK StackOverflow](http://stackoverflow.com/questions/tagged/azure-webjobssdk)
* [Azure függvények StackOverflow](http://stackoverflow.com/questions/tagged/azure-functions)
* [Azure és ASP.NET-fórum](http://forums.asp.net/1247.aspx)
* [Azure alkalmazás szolgáltatás Web Apps-fórum](http://social.msdn.microsoft.com/Forums/azure/home?forum=windowsazurewebsitespreview)
* [Azure Web Apps alkalmazások felhasználói hangposta-webhely](https://feedback.azure.com/forums/169385-websites/)
* A [twitter](http://twitter.com/). A kettős keresztes címke #AzureWebJobs használja.
* [Jelentés WebJobs programhibák vagy a kibocsátás](https://github.com/projectkudu/kudu/wiki/Reporting-WebJobs-issues)


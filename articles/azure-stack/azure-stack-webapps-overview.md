<properties
    pageTitle="Azure Papírhalom Web Apps alkalmazások áttekintése |} Microsoft Azure"
    description="Azure egymást fedő Web Apps alkalmazások áttekintése"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="azure-stack-web-apps-overview"></a>Azure Papírhalom Web Apps alkalmazások áttekintése
    
> [AZURE.NOTE] Az alábbi információk csak az Azure Papírhalom TP1 telepítések vonatkozik.

Azure Papírhalom Web Apps alkalmazások az Azure alkalmazás szolgáltatás felületek Azure jegyzettömbhöz vett fel, az első elemet. Az Azure Papírhalom Web Apps alkalmazások telepítőt öt szükséges szerepkör különböző egy példányának hoz létre, és is hoz létre a fájlkiszolgálóra. Szerepkör különböző felvehet további előfordulásait, de ne feledje, hogy nem áll a Technical Preview 1 VMs a szabad terület. Az aktuális funkciók Azure Papírhalom Web Apps alkalmazások elsősorban a rendszer és host web Apps alkalmazások kezeléséhez szükséges foundation lehetőségei.

![Azure Papírhalom alkalmazás szolgáltatás Web Apps az Azure-ban portál egymásra halmozni.][1]

## <a name="limitations-of-the-technical-preview"></a>A Technical Preview korlátai

Nem támogatott az Azure Papírhalom alkalmazás szolgáltatás preview verziója nem. Gyártási munkaterhelésekből nem helyezhet el a előzetes verzióval. Még nem nincs frissítés Azure Papírhalom alkalmazás szolgáltatás preview verziója között. A betekintő kiadásokkal elsődleges alkalmazásában, hogy milyen azt nyújtó megjelenítéséhez és visszajelzés juthat. 

## <a name="what-is-an-app-service-plan"></a>Mi az, hogy egy App szolgáltatás megtervezése?

Az Azure Papírhalom Web Apps alkalmazások erőforrás szolgáltató használja az Azure-App szolgáltatásban a Azure Web Apps alkalmazások szolgáltatást használó ugyanazt a kódot. Néhány gyakori fogalmak eredményt adja, érdemes leíró vannak. A Web Apps alkalmazások a web Apps alkalmazások árak tároló alkalmazás szolgáltatáscsomagja neve. A dedikált virtuális gépeken futó alkalmazás fájlformátumát csoportját képviseli. Egy adott előfizetés belül több App milyen szolgáltatáscsomagok is. Ez akkor is érvényes, az Azure Papírhalom Web Apps alkalmazásokban. 

Azure, a megosztott és dedikált dolgozók vannak. Egy megosztott dolgozó támogatja nagysűrűségű multitenant web app-szolgáltatója, és csak egy csoportja közös dolgozók. Csak egy bérlői által használt és három méretben származnak dedikált kiszolgálók: kicsi, közepes és nagyméretű. Az egyéni igényeknek a helyszíni ügyfelek nem mindig kell leírni e kifejezések használatával. Az Azure Papírhalom Web Apps az erőforrás-szolgáltató rendszergazdák az általuk elérhetővé szeretne tenni, hogy megosztott dolgozók több feltételcsoportnak vagy saját igényeinek szolgáltatója egyedi webes alapján dedikált dolgozók különböző csoportjaihoz dolgozó rétegek adhatja meg. Réteg dolgozó definíciók használ, azok is majd határozza meg saját termékváltozatok árak.

## <a name="portal-features"></a>A portál jellemzői

Szintén igaz és a háttér, mint az Azure Papírhalom Web Apps alkalmazások ugyanazt a felhasználói felület Azure Web Apps alkalmazások használó használja. Egyes szolgáltatások le vannak tiltva, és még nem működési Azure egymást fedő. Ennek oka az Azure-specifikus várakozásoknak vagy a szolgáltatások, ezek a szolgáltatások használatához még nem érhetők el az Azure Papírhalom. 

## <a name="next-steps"></a>Következő lépések

- [Az Azure Papírhalom Web Apps alkalmazások megkezdése előtt](azure-stack-webapps-before-you-get-started.md)
- [Telepítse a Web Apps erőforrás-szolgáltató](azure-stack-webapps-deploy.md)

Más [platform szolgáltatást (PaaS) szerint](azure-stack-tools-paas-services.md), például az [erőforrás-szolgáltató SQL Server](azure-stack-sql-rp-deploy-short.md) és a [MySQL-erőforrás szolgáltató](azure-stack-mysql-rp-deploy-short.md)is próbálkozhat.

<!--Image references-->
[1]: ./media/azure-stack-webapps-overview/AppService_Portal.png

<properties
    pageTitle="Linux és Azure a számítások Megnyitás forrás |} Microsoft Azure"
    description="Linux és Megnyitás-forrás számítások cikkek az Azure-egyszerű Linux használatát, ideértve fut, vagy Linux Azure, és a más tartalmak adott technológiák és optimalizálásokat képek feltöltése kapcsolatos néhány alapvető fogalmak listája."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="06/27/2016"
    ms.author="rasquill"/>



# <a name="linux-and-open-source-computing-on-azure"></a>Linux és Azure a számítások Megnyitás forrás

Keresse meg kell létrehozni és kezelni a virtuális gépeken futó Linux-alapú a klasszikus telepítési modell összes dokumentációt.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="get-started"></a>Első lépések
- [Az Azure a Linux – bevezetés](virtual-machines-linux-intro-on-azure.md)
- [Gyakori kérdés kapcsolatos Azure virtuális gépeken futó a klasszikus telepítési modell készült](virtual-machines-linux-classic-faq.md)
- [Tudnivalók a virtuális gépeken futó képe](virtual-machines-linux-classic-about-images.md)
- [A saját Distro kép feltöltése](virtual-machines-linux-classic-create-upload-vhd.md) (, és útmutatást egy [Azure-Endorsed terjesztési](virtual-machines-linux-endorsed-distros.md))
- [Jelentkezzen be a Linux virtuális használatával az Azure klasszikus portálra](virtual-machines-linux-mac-create-ssh-keys.md)

## <a name="set-up"></a>Beállítása

- [Telepítse az Azure parancssori kezelőfelületről Azure](../xplat-cli-install.md)


## <a name="tutorials"></a>Oktatóanyagok

- [Telepítse a LÁMPA Papírhalom Linux virtuális gépre Azure-ban](virtual-machines-linux-create-lamp-stack.md)
- [Fonetikus az Azure virtuális a sínek webalkalmazáshoz](linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)
- [Útmutató: telepítés Apache Qpid Proton-C AMQP és szolgáltatás Bus](../service-bus-messaging/service-bus-amqp-apache.md)

### <a name="databases"></a>Adatbázisok
- [A Azure MySQL teljesítményének optimalizálása](virtual-machines-linux-classic-optimize-mysql.md)
- [MySQL fürt](virtual-machines-linux-classic-mysql-cluster.md)
- [Cassandra Linux Azure fut, és használja a Node.js](virtual-machines-linux-classic-cassandra-nodejs.md)
- [Több diaminta fürt a MariaDbs létrehozása](virtual-machines-linux-classic-mariadb-mysql-cluster.md)

### <a name="hpc"></a>HPC
- [Első lépések a Linux számítási csomópontok HPC csomag fürt Azure-ban](virtual-machines-linux-classic-hpcpack-cluster.md)
- [A Microsoft HPC Pack NAMD futtatni Linux számítási csomópontok Azure-ban](virtual-machines-linux-classic-hpcpack-cluster-namd.md)
- [MPI alkalmazások futtatásához a Linux RDMA fürtre beállítása](virtual-machines-linux-classic-rdma-cluster.md)

### <a name="docker"></a>Docker
- [Az Azure parancssori Docker virtuális kiterjesztésre használatával felület (Azure CLI)](virtual-machines-linux-classic-cli-use-docker.md)
- [Az Docker virtuális kiterjesztés az Azure portál használatával](virtual-machines-linux-classic-portal-use-docker.md)
- [Azure docker-gép használatáról](virtual-machines-linux-docker-machine.md)

### <a name="ubuntu"></a>Ubuntu
- [Útmutató: MySQL fürt](virtual-machines-linux-classic-mysql-cluster.md)
- [Útmutató: Node.js és Cassandra](virtual-machines-linux-classic-cassandra-nodejs.md)

### <a name="opensuse"></a>OpenSUSE
- [Útmutató: telepítése és futtatása a MySQL](virtual-machines-linux-classic-mysql-on-opensuse.md)

### <a name="coreos"></a>CoreOS
- [Útmutató: Azure CoreOS használata](https://coreos.com/os/docs/latest/booting-on-azure.html)


## <a name="planning"></a>Megtervezése
- [Azure infrastruktúra szolgáltatások végrehajtása útmutató](virtual-machines-linux-infrastructure-subscription-accounts-guidelines.md)
- [Linux felhasználónevét kijelölése](virtual-machines-linux-usernames.md)
- [Az elérhetőség beállítása a klasszikus telepítési modell virtuális gépeken futó konfigurálása](virtual-machines-linux-classic-configure-availability.md)
- [Azure VMs tervezett karbantartásokat ütemezéséről](virtual-machines-linux-planned-maintenance-schedule.md)
- [Virtuális gépeken futó elérhetőségének kezelése](virtual-machines-linux-manage-availability.md)
- [Tervezett karbantartás Linux virtuális gépeken futó Azure-ban](virtual-machines-linux-planned-maintenance.md)


## <a name="deployment"></a>Telepítési
- [Egyéni virtuális gép Linux operációs rendszert futtató létrehozása](virtual-machines-linux-classic-createportal.md)
- [Alapismeretek: a sablon egy Linux virtuális rögzítése](virtual-machines-linux-classic-capture-image.md)
- [Nem záradékkal terjesztését adatait](virtual-machines-linux-create-upload-generic.md)


## <a name="management"></a>Kezelése

- [SSH](virtual-machines-linux-mac-create-ssh-keys.md)
- [Hogy hogyan állíthatja alaphelyzetbe a jelszó vagy a SSH tulajdonságok Linux](virtual-machines-linux-classic-reset-access.md)
- [Legfelső szintű használata](virtual-machines-linux-use-root-privileges.md)


## <a name="azure-resources"></a>Azure erőforrások

- [Az Azure Linux ügynök](virtual-machines-linux-agent-user-guide.md)
- [Azure virtuális Extensions és szolgáltatások](virtual-machines-windows-extensions-features.md)
- [Egyéni adatok hogy egy virtuális felhő nicializálás használata](virtual-machines-windows-classic-inject-custom-data.md)


## <a name="storage"></a>Tárhely

- [Adatok lemezen csatol egy Linux virtuális](virtual-machines-linux-classic-attach-disk.md)
- [Egy Linux virtuális származó adatok lemezen leválasztása](virtual-machines-linux-classic-detach-disk.md)
- [RAID](virtual-machines-linux-configure-raid.md)


## <a name="networking"></a>Hálózati
- [Hogy miként állíthatja be a végpontok klasszikus virtuális gépen Azure-ban](virtual-machines-linux-classic-setup-endpoints.md)


## <a name="troubleshooting"></a>Hibaelhárítás
- [Azure virtuális Linux-alapú géphez biztonságos rendszerhéj (SSH) kapcsolatok hibaelhárítása](virtual-machines-linux-troubleshoot-ssh-connection.md)
- [Az új Linux virtuális gép létrehozása az Azure klasszikus telepítési problémák elhárítása](virtual-machines-linux-classic-troubleshoot-deployment-new-vm.md)  
- [Újraindítását vagy egy meglévő Azure-ban Linux virtuális gép átméretezés klasszikus telepítési problémák elhárítása](virtual-machines-linux-classic-restart-resize-error-troubleshooting.md) 


## <a name="reference"></a>Hivatkozás

- [Azure CLI parancsok az Azure Szolgáltatáskezelés (asm) mód](../virtual-machines-command-line-tools.md)
- [Azure Szolgáltatáskezelés REST API-val](https://msdn.microsoft.com/library/azure/ee460799.aspx)




## <a name="general-links"></a>Általános hivatkozások
Az alábbi hivatkozások van Microsoft-blogok, Technet lapok, és a külső webhelyek: helyett Azure.com dokumentáció, mint feljebb. Azure és a Megnyitás-forrás számítógépes világ is vannak a gyorsan változó célok, célszerű majdnem biztos, hogy az alábbi hivatkozásokat is ki a dátum, *ellenére* arra, hogy azt kell végezze el a legjobb folyamatosan újabb bővítéséhez meg és távolíthat el elavult is. Hogy amit kifelejtettem végre, ha kérjük, tudassa velünk, hogy a megjegyzések, vagy ki a saját [GitHub repó](https://github.com/Azure/azure-content/)kérelem.

- [ASP.NET-5 futó Linux Docker tárolók használatával](http://blogs.msdn.com/b/webdev/archive/2015/01/14/running-asp-net-5-applications-in-linux-containers-with-docker.aspx)
- [Egy CentOS virtuális képet OpenLogic telepítése](https://azure.microsoft.com/blog/2013/01/11/deploying-openlogic-centos-images-on-windows-azure-virtual-machines/)
- [SUSE frissítési infrastruktúra](https://forums.suse.com/showthread.php?5622-New-Update-Infrastructure)
- [SAP felhő készülék tár SUSE Linux vállalati kiszolgáló](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver11sp3forsapcloudappliance/)
- [Könnyen hozzáférhető Linux támaszkodva Azure lépésben 12](http://blogs.technet.com/b/keithmayer/archive/2014/10/03/quick-start-guide-building-highly-available-linux-servers-in-the-cloud-on-microsoft-azure.aspx)
- [Azure Azure CLI, node.js, jhawk a kiépítési Linux automatizálása](http://blogs.technet.com/b/keithmayer/archive/2014/11/24/step-by-step-automated-provisioning-for-linux-in-the-cloud-with-microsoft-azure-xplat-cli-json-and-node-js-part-1.aspx)
- [A Azure GlusterFS](http://dastouri.azurewebsites.net/gluster-on-azure-part-1/)

### <a name="freebsd"></a>FreeBSD
- [Az Azure FreeBSD fut.](https://azure.microsoft.com/blog/2014/05/22/running-freebsd-in-azure/)
- [Egyszerű üzembe FreeBSD](http://msopentech.com/blog/2014/10/24/easy-deploy-freebsd-microsoft-azure-vm-depot/)
- [A testre szabott FreeBSD kép üzembe helyezése](http://msopentech.com/blog/2014/05/14/deploy-customize-freebsd-virtual-machine-image-microsoft-azure/)
- [Kaspersky AV Linux fájl kiszolgáló](https://azure.microsoft.com/marketplace/partners/kaspersky-lab/kav-for-lfs-kav-for-lfs/)

### <a name="nosql"></a>NoSQL

- [Azure 8 Megnyitás-forrás NoSql adatbázisok](http://openness.microsoft.com/blog/2014/11/03/open-source-nosql-databases-microsoft-azure/)
- [Slideshare (MSOpenTech): Az Azure a CouchDb szolgáltatások](http://www.slideshare.net/brianbenz/experiences-using-couchdb-inside-microsofts-azure-team)
- [CouchDB-mint-a-szolgáltatást futtató node.js, CORS és Grunt](http://msopentech.com/blog/2013/12/19/tutorial-building-multi-tier-windows-azure-web-application-use-cloudants-couchdb-service-node-js-cors-grunt-2/)

- [A Windows Azure vgx.dll gyorsítótár-szolgáltatás a vgx.dll](http://msopentech.com/blog/2014/05/12/redis-on-windows/)
- [ASP.NET-munkamenet állapot-szolgáltató az előnézeti Release vgx.dll kihirdetése](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)

- [Blog: RavenHQ most érhető el a az Azure piactérről](https://azure.microsoft.com/blog/2014/08/12/ravenhq-now-available-in-the-azure-store/)

### <a name="big-data"></a>Nagy adatok
- [Azure Linux VMs Hadoop telepítése](http://blogs.msdn.com/b/benjguin/archive/2013/04/05/how-to-install-hadoop-on-windows-azure-linux-virtual-machines.aspx)
- [Azure hdinsight szolgáltatáshoz](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/)

### <a name="relational-database"></a>Relációs adatbázisok
- [MySQL magas elérhetősége architektúra Microsoft Azure-ban](http://download.microsoft.com/download/6/1/C/61C0E37C-F252-4B33-9557-42B90BA3E472/MySQL_HADR_solution_in_Azure.pdf)
- [Postgres telepítése az corosync, pg_bouncer ILB használatával](https://github.com/chgeuer/postgres-azure)

### <a name="linux-high-performance-computing-hpc"></a>Linux nagy teljesítményű számítások (HPC)

- [Sablon quickstart Útmutató: SLURM fürt léptetőnyíl vezérlőelem](https://github.com/Azure/azure-quickstart-templates/tree/master/slurm) (és [kérdések feltevése: blog](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx))
- [Quickstart útmutató sablon: hozzon létre egy HPC fürthöz Linux számítási csomópontokkal](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/)

### <a name="devops-management-and-optimization"></a>Devops, kezelése és optimalizálási

Devops földrajzi, mint kezelési és optimalizálási igazán kiterjedtnek és módosítani szeretné a nagyon gyorsan, vegye figyelembe az alábbi listán kiindulási pont.

- [Videó: Azure virtuális gépeken futó: segítségével tölthetők le, Puppet és Docker Linux VMs kezelésére szolgáló](https://azure.microsoft.com/blog/2014/12/15/azure-virtual-machines-using-chef-puppet-and-docker-for-managing-linux-vms/)

- [Automatikus Kubernetes fürt telepítési CoreOS és nagy teljes útmutató](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
- [Kubernetes Visualizer](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)

- [Jenkins kisegítő Azure beépülő modul](http://msopentech.com/blog/2014/09/23/announcing-jenkins-slave-plugin-azure/)
- [GitHub repó: Jenkins tároló Azure beépülő modul](https://github.com/jenkinsci/windows-azure-storage-plugin)

- [Harmadik fél: Hudson kisegítő Azure beépülő modul](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
- [Harmadik fél: Hudson tároló Azure beépülő modul](https://github.com/hudson3-plugins/windows-azure-storage-plugin)

- [Videó: Mi az tölthetők le, és hogyan működik?](https://msopentech.com/blog/2014/03/31/using-chef-to-manage-azure-resources/)

- [Videó: Hogyan Linux VMs Azure automatizálási használata](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)

- [Blog: Hogyan lehet Powershell DSC Linux számára](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
- [GitHub: Docker ügyfél DSC](https://github.com/anweiss/DockerClientDSC)

- [Azure csomagoló beépülő modul](https://github.com/msopentech/packer-azure)

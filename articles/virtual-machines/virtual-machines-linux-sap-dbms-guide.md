<properties
   pageTitle="SAP – NetWeaver Linux virtuális gépeken (VMs) – az adatbázis-kezelő telepítési útmutató |} Microsoft Azure"
   description="SAP – NetWeaver Linux virtuális gépeken (VMs) – az adatbázis-kezelő telepítési útmutató"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="MSSedusch"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/18/2016"
   ms.author="sedusch"/>

# <a name="sap-netweaver-on-azure-virtual-machines-vms--dbms-deployment-guide"></a>SAP – NetWeaver Azure virtuális gépeken (VMs) – az adatbázis-kezelő telepítési útmutató

[767598]:https://service.sap.com/sap/support/notes/767598
[773830]:https://service.sap.com/sap/support/notes/773830
[826037]:https://service.sap.com/sap/support/notes/826037
[965908]:https://service.sap.com/sap/support/notes/965908
[1031096]:https://service.sap.com/sap/support/notes/1031096
[1139904]:https://service.sap.com/sap/support/notes/1139904
[1173395]:https://service.sap.com/sap/support/notes/1173395
[1245200]:https://service.sap.com/sap/support/notes/1245200
[1409604]:https://service.sap.com/sap/support/notes/1409604
[1558958]:https://service.sap.com/sap/support/notes/1558958
[1585981]:https://service.sap.com/sap/support/notes/1585981
[1588316]:https://service.sap.com/sap/support/notes/1588316
[1590719]:https://service.sap.com/sap/support/notes/1590719
[1597355]:https://service.sap.com/sap/support/notes/1597355
[1605680]:https://service.sap.com/sap/support/notes/1605680
[1619720]:https://service.sap.com/sap/support/notes/1619720
[1619726]:https://service.sap.com/sap/support/notes/1619726
[1619967]:https://service.sap.com/sap/support/notes/1619967
[1750510]:https://service.sap.com/sap/support/notes/1750510
[1752266]:https://service.sap.com/sap/support/notes/1752266
[1757924]:https://service.sap.com/sap/support/notes/1757924
[1757928]:https://service.sap.com/sap/support/notes/1757928
[1758182]:https://service.sap.com/sap/support/notes/1758182
[1758496]:https://service.sap.com/sap/support/notes/1758496
[1772688]:https://service.sap.com/sap/support/notes/1772688
[1814258]:https://service.sap.com/sap/support/notes/1814258
[1882376]:https://service.sap.com/sap/support/notes/1882376
[1909114]:https://service.sap.com/sap/support/notes/1909114
[1922555]:https://service.sap.com/sap/support/notes/1922555
[1928533]:https://service.sap.com/sap/support/notes/1928533
[1941500]:https://service.sap.com/sap/support/notes/1941500
[1956005]:https://service.sap.com/sap/support/notes/1956005
[1973241]:https://service.sap.com/sap/support/notes/1973241
[1984787]:https://service.sap.com/sap/support/notes/1984787
[1999351]:https://service.sap.com/sap/support/notes/1999351
[2002167]:https://service.sap.com/sap/support/notes/2002167
[2015553]:https://service.sap.com/sap/support/notes/2015553
[2039619]:https://service.sap.com/sap/support/notes/2039619
[2121797]:https://service.sap.com/sap/support/notes/2121797
[2134316]:https://service.sap.com/sap/support/notes/2134316
[2178632]:https://service.sap.com/sap/support/notes/2178632
[2191498]:https://service.sap.com/sap/support/notes/2191498
[2233094]:https://service.sap.com/sap/support/notes/2233094
[2243692]:https://service.sap.com/sap/support/notes/2243692

[azure-cli]:../xplat-cli-install.md
[azure-portal]:https://portal.azure.com
[azure-ps]:../powershell-install-configure.md
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../azure-subscription-service-limits.md#subscription

[dbms-guide]:virtual-machines-linux-sap-dbms-guide.md (SAP NetWeaver Linux virtuális gépeken (VMs) – az adatbázis-kezelő telepítési útmutató) [dbms-guide-2.1]:virtual-machines-linux-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (gyorsítótárazás VMs és VHD) [dbms-guide-2.2]:virtual-machines-linux-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (szoftver RAID) [dbms-guide-2.3]:virtual-machines-linux-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (a Microsoft Azure tároló) [dbms-guide-2]:virtual-machines-linux-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (RDBMS telepítés szerkezetének) [dbms-guide-3]:virtual-machines-linux-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (magas rendelkezésre állásának és a helyreállítás az Azure VMs) [dbms-guide-5.5.1]:virtual-machines-linux-sap-dbms-guide.md# 0fef0e79-d3fe-4ae2-85af-73666a6f7268 (SQL Server 2012 SP1 CU4 és újabb verzióiban) [dbms-guide-5.5.2]:virtual-machines-linux-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (SQL Server 2012 SP1 CU3 és korábbi verziókban) [dbms-guide-5.6]:virtual-machines-linux-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (a egy SQL Server képek használatáról a a Microsoft Azure piactérről kívül) [dbms-guide-5.8]:virtual-machines-linux-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (Azure összefoglaló SAP általános SQL Server) [dbms-guide-5]:virtual-machines-linux-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (az SQL Server RDBMS adatai) [dbms-guide-8.4.1]:virtual-machines-linux-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (tárhely konfigurációja) [dbms-guide-8.4.2]:virtual-machines-linux-sap-dbms-guide.md# 23c78d3b-ca5a-4e72-8a24-645d141a3f5d (biztonsági mentési és visszaállítási) [dbms-guide-8.4.3]:virtual-machines-linux-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (a biztonsági mentés és visszaállítás teljesítménnyel kapcsolatos szempontok) [dbms-guide-8.4.4]:virtual-machines-linux-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 (egyéb) [dbms-guide-900-sap-cache-server-on-premises]:virtual-machines-linux-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:./media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:./media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:./media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:./media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:./media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:./media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:./media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:./media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:./media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:virtual-machines-linux-sap-deployment-guide.md (SAP NetWeaver Linux virtuális gépeken (VMs) – telepítési útmutató) [deployment-guide-2.2]:virtual-machines-linux-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 (SAP erőforrások) [deployment-guide-3.1.2]:virtual-machines-linux-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab (üzembe helyezése a virtuális egy egyéni képpel) [deployment-guide-3.2]:virtual-machines-linux-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 (forgatókönyv 1: üzembe helyezése a virtuális ki az Azure piactéren elérhető az SAP) [deployment-guide-3.3]:virtual-machines-linux-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (forgatókönyv 2: üzembe helyezése a virtuális egy egyéni képpel SAP) [deployment-guide-3.4]:virtual-machines-linux-sap-deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 () Forgatókönyv 3: Egy virtuális áthelyezése a helyszíni egy nem általános Azure virtuális használata SAP) [deployment-guide-3]:virtual-machines-linux-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e (telepítési esetek a VMs az SAP, a Microsoft Azure) [deployment-guide-4.1]:virtual-machines-linux-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (telepíti az Azure PowerShell-parancsmagok) [deployment-guide-4.2]:virtual-machines-linux-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (töltse le és importálás SAP vonatkozó PowerShell-parancsmagok) [deployment-guide-4.3]:virtual-machines-linux-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (Join virtuális a helyszíni tartomány - csak Windows) [deployment-guide-4.4.2]:virtual-machines-linux-sap-deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux) [telepítési útmutató, 4.4]: Virtual-machines-Linux-SAP-Deployment-Guide.md#c7cbb0dc-52a4-49DB-8e03-83e7edc2927d (letöltési, telepítési és Azure virtuális ügynök engedélyezése) [deployment-guide-4.5.1]:virtual-machines-linux-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 (Azure PowerShell) [deployment-guide-4.5.2]:virtual-machines-linux-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f (Azure CLI) [deployment-guide-4.5]:virtual-machines-linux-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (konfigurálása Azure fokozott figyelése kiterjesztés az SAP) [deployment-guide-5.1]:virtual-machines-linux-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (készenléti ellenőrzi, hogy Azure az SAP fokozott figyelése) [deployment-guide-5.2]:virtual-machines-linux-sap-deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (állapot-ellenőrzés az Azure Megfigyelés infrastruktúra konfigurációja) [deployment-guide-5.3]:virtual-machines-linux-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (további hibaelhárítási Azure figyelése infrastruktúra az SAP)

[deployment-guide-configure-monitoring-scenario-1]:virtual-machines-linux-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b (Configure Monitoring)
[deployment-guide-configure-proxy]:virtual-machines-linux-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d (Configure Proxy)
[deployment-guide-figure-100]:./media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:./media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:virtual-machines-linux-sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]:./media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:./media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:./media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:virtual-machines-linux-sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]:./media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:./media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:./media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:virtual-machines-linux-sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]:./media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:./media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:virtual-machines-linux-sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]:./media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:virtual-machines-linux-sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]:./media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:./media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:./media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:virtual-machines-linux-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:virtual-machines-linux-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:virtual-machines-linux-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:virtual-machines-linux-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b (Checks and Troubleshooting for End-to-End Monitoring Setup for SAP on Azure)

[deploy-template-cli]:../resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]:../resource-group-template-deploy.md#deploy-with-the-preview-portal
[deploy-template-powershell]:../resource-group-template-deploy.md#deploy-with-powershell

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:virtual-machines-linux-sap-get-started.md
[getting-started-dbms]:virtual-machines-linux-sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:virtual-machines-linux-sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:virtual-machines-linux-sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:./media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:./media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:virtual-machines-linux-sap-planning-guide.md (SAP NetWeaver Linux virtuális gépeken (VMs) – tervezési és implementálási útmutatójában) [planning-guide-1.2]:virtual-machines-linux-sap-planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (erőforrások) [planning-guide-11]:virtual-machines-linux-sap-planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (nagy elérhetőség (HA) és katasztrófa helyreállítási (DR) az SAP NetWeaver fut a Azure virtuális gépeken futó) [planning-guide-11.4.1]:virtual-machines-linux-sap-planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (SAP alkalmazáskiszolgálók magas elérhetőség) [planning-guide-11.5]:virtual-machines-linux-sap-planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (használatával beállításával automatikusan elindíthat SAP-példányok) [planning-guide-2.1]:virtual-machines-linux-sap-planning-guide.md# 1625df66-4CC6-4d60-9202-de8a0b77f803 (csak Cloud - anélkül, hogy a helyszíni ügyfélhálózat függőségét az Azure virtuális gép telepítések) [planning-guide-2.2]:virtual-machines-linux-sap-planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (határokon helyszíni – telepítése egy vagy több SAP VMs követelménye készül teljes mértékben integrálva a helyszíni hálózaton az Azure be) [planning-guide-3.1]:virtual-machines-linux-sap-planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a (Azure régió) [planning-guide-3.2.1]:virtual-machines-linux-sap-planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 (hibafa tartományok) [planning-guide-3.2.2]:virtual-machines-linux-sap-planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (frissítése tartományok) [planning-guide-3.2.3]:virtual-machines-linux-sap-planning-guide.md#18810088- f9be - 4c 97-958a - 27996255c 665 (Azure elérhetőségének beállítása) [planning-guide-3.2]:virtual-machines-linux-sap-planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (a Microsoft Azure virtuális gép fogalmat) [planning-guide-3.3.2]:virtual-machines-linux-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure prémium tároló) [planning-guide-5.1.1]:virtual-machines-linux-sap-planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (áthelyezése egy virtuális a helyszíni az Azure-általános lemezen) [planning-guide-5.1.2]:virtual-machines-linux-sap-planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (telepíti az ügyfélnek adott képpel egy virtuális) [planning-guide-5.2.1]:virtual-machines-linux-sap-planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (áthelyezése egy virtuális a helyszíni az Azure-általános lemezen előkészítése) [ Planning-Guide-5.2.2]:Virtual-machines-Linux-SAP-Planning-Guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (üzembe helyezése a virtuális ügyfélnek adott képpel SAP előkészítése) [planning-guide-5.2]:virtual-machines-linux-sap-planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (előkészítése VMs az SAP az Azure) [planning-guide-5.3.1]:virtual-machines-linux-sap-planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (különbség között az Azure lemez és Azure kép) [planning-guide-5.3.2]:virtual-machines-linux-sap-planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (egy virtuális a helyszíni Azure való feltöltés) [planning-guide-5.4.2]:virtual-machines-linux-sap-planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (lemezre másolása a Azure tároló fiókok között) [planning-guide-5.5.1]:virtual-machines-linux-sap-planning-guide.md# 4efec401-91e0-40c0-8e64-f2dceadff646 (virtuális/virtuális szerkezet SAP telepítésekhez) [planning-guide-5.5.3]:virtual-machines-linux-sap-planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (beállítás automount a csatlakoztatott) [planning-guide-7.1]:virtual-machines-linux-sap-planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (az SAP NetWeaver bemutató/oktatás forgatókönyv egyetlen virtuális) [planning-guide-7]:virtual-machines-linux-sap-planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (SAP-példányok fogalmak a Cloud-Only telepítés) [planning-guide-9.1]:virtual-machines-linux-sap-planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (Azure figyelése megoldás az SAP) [planning-guide-azure-premium-storage]:virtual-machines-linux-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure prémium tároló)

[planning-guide-figure-100]:./media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:./media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:./media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:./media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:./media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:./media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:./media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:./media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:./media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:./media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:./media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:./media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:./media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:./media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:./media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:./media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:./media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:./media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:./media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:./media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:./media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:./media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:./media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:virtual-machines-linux-sap-planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd (Microsoft Azure Networking)
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:virtual-machines-linux-sap-planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f (Storage: Microsoft Azure Storage and Data Disks)

[powershell-install-configure]:../powershell-install-configure.md
[resource-group-authoring-templates]:../resource-group-authoring-templates.md
[resource-group-overview]:../resource-group-overview.md
[resource-groups-networking]:../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam (SAP Product Availability Matrix)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[storage-azure-cli]:../storage/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../storage/storage-azure-cli.md#copy-blobs
[storage-introduction]:../storage/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../storage/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../storage/storage-premium-storage.md
[storage-redundancy]:../storage/storage-redundancy.md
[storage-scalability-targets]:../storage/storage-scalability-targets.md
[storage-use-azcopy]:../storage/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:virtual-machines-linux-attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-linux-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:virtual-machines-linux-cli-deploy-templates.md (Deploy and manage virtual machines by using Azure Resource Manager templates and the Azure CLI)
[virtual-machines-deploy-rmtemplates-powershell]:virtual-machines-windows-ps-manage.md (Manage virtual machines using Azure Resource Manager and PowerShell)
[virtual-machines-linux-agent-user-guide]:virtual-machines-linux-agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:virtual-machines-linux-agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:virtual-machines-linux-capture-image.md#capture-the-vm
[virtual-machines-linux-configure-raid]:virtual-machines-linux-configure-raid.md
[virtual-machines-linux-configure-lvm]:virtual-machines-linux-configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:virtual-machines-linux-suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:virtual-machines-linux-redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:virtual-machines-linux-add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:virtual-machines-linux-add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:virtual-machines-linux-quick-create-cli.md
[virtual-machines-linux-update-agent]:virtual-machines-linux-update-agent.md
[virtual-machines-manage-availability]:virtual-machines-linux-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-create-powershell.md
[virtual-machines-sizes]:virtual-machines-linux-sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:virtual-machines-windows-classic-ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]:../virtual-network/virtual-network-deploy-multinic-arm-cli.md
[virtual-network-deploy-multinic-arm-ps]:../virtual-network/virtual-network-deploy-multinic-arm-ps.md
[virtual-network-deploy-multinic-arm-template]:../virtual-network/virtual-network-deploy-multinic-arm-template.md
[virtual-networks-configure-vnet-to-vnet-connection]:../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../virtual-network/virtual-networks-create-vnet-arm-pportal.md
[virtual-networks-manage-dns-in-vnet]:../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../virtual-network/virtual-networks-multiple-nics.md
[virtual-networks-nsg]:../virtual-network/virtual-networks-nsg.md
[virtual-networks-reserved-private-ip]:../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]:../vpn-gateway/vpn-gateway-site-to-site-create.md
[vpn-gateway-vpn-faq]:../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../xplat-cli-install.md
[xplat-cli-azure-resource-manager]:../xplat-cli-azure-resource-manager.md

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klasszikus telepítési modell.

Ez az útmutató a dokumentáció végrehajtása és a Microsoft Azure az SAP-szoftver telepítése része. Ez az útmutató olvasása, előtt olvassa el a [tervezés és implementálási útmutatójában] [tervezési – útmutató]. A dokumentum különböző relációs adatbázisok kezelése rendszerek (RDBMS) és a kapcsolódó termékekhez a a Microsoft Azure virtuális gépeken futó (VMs) SAP együtt az Azure infrastruktúra használata egy szolgáltatási (IaaS) lehetőségeket a központi foglalkozik.

A papír kiegészíti az SAP dokumentáció és az SAP megjegyzéseket, amelyek az elsődleges erőforrások telepítések és az SAP-szoftver telepítések jelenítik meg a megadott platformokon

[AZURE.INCLUDE [windows-warning](../../includes/virtual-machines-linux-sap-warning.md)]

## <a name="general-considerations"></a>Általános szempontok
Ez a fejezet SAP futtatása előtt megfontolandó kérdések kapcsolódó Azure VMs az adatbázis-kezelő rendszerek ismerkedhet. A fejezet adott adatbázis-kezelő rendszerek néhány hivatkozások találhatók. Helyette az adott adatbázis-kezelő rendszerek kezelése a papírt, a fejezet után.

### <a name="definitions-upfront"></a>Definíciók upfront
A dokumentum egészében fogjuk használni az alábbi kifejezések:

* IaaS: Infrastruktúra szolgáltatásként.
* PaaS: Platform szolgáltatásként.
* A szoftver: A szoftver szolgáltatásként.
* SAP-összetevő: az egyes SAP alkalmazások például ECC, FF, megoldás Manager vagy EP  SAP-összetevők hagyományos ABAP vagy Java technológiák vagy egy nem NetWeaver alapú alkalmazást, például a vállalati objektumok alapulhatnak.
* SAP-környezet: egy vagy több SAP-összetevők logikailag csoportosított való fejlesztése, QAS, oktatás, DR vagy gyártási például egy üzleti funkció ellátásának.
* SAP fekvő: A teljes SAP eszközök ügyfél utal informatikai fekvő. Az SAP fekvő tartalmazza a gyártási és a nem üzemi környezetben.
* SAP-rendszer: Az adatbázis-kezelő réteg és kombinációja alkalmazási réteg, például egy SAP ERP fejlesztői rendszer SAP FF vizsgálati rendszer, SAP CRM munkakörnyezetben, stb. A Azure környezetekben nem támogatott ezeket a helyszíni és Azure között két rétegeket osztáshoz. Ez azt jelenti az SAP rendszer vagy rendszerbe a helyszíni, vagy az Azure telepítve van. A különböző rendszerek, az SAP fekvő Azure vagy a helyszíni azonban telepítheti. Például sikerült az SAP CRM fejlesztési telepítése, és Azure, de az SAP CRM gyártási rendszer helyszíni rendszerek tesztelheti.
* Csak a felhőalapú környezetben: A telepítés, ahol az Azure előfizetés nem csatlakozik egy webhely, illetve a helyszíni hálózati infrastruktúrát készült ExpressRoute kapcsolat. Közös Azure dokumentáció telepítések az ilyen típusú is "Csak felhő" telepítések leírása. Ezzel a módszerrel rendszerbe virtuális gépeken futó érhetők el az Internet és Azure-ban a VMs rendelt nyilvános Internet végpontok. A helyszíni Active Directory (AD) és a DNS nem terjeszteni az ilyen típusú telepítések Azure. Ezért a VMs nem vesznek részt a helyszíni Active Directory. Megjegyzés: Ehhez a dokumentumhoz a felhőben telepítések meghatározásuk szerint a teljes SAP tájak, amely futtatja kizárólag az Azure Active Directory vagy a névfeloldás kiterjesztés nélkül a helyszíni nyilvános felhő be. Gyártási SAP rendszereihez vagy a beállításait, ahol SAP STM vagy más erőforrások: a helyszíni kell közötti SAP rendszereihez Azure és erőforrások: a helyszíni tartózkodó is használható a felhőben konfigurációk nem támogatottak.
* Idegen helyszíni: Ahol VMs van telepítve, amelyen a webhely több helyre vagy a helyszíni datacenter(s) és Azure készült ExpressRoute összekapcsolását Azure-előfizetésbe példa mutatja be. Közös Azure dokumentációt, az ilyen típusú telepítések is határokon helyszíni esetek leírása. A kapcsolat az az oka, hogy a helyszíni tartomány kiterjesztése, a helyszíni Active Directory és a helyszíni DNS az Azure. A helyszíni fekvő kiterjed az előfizetés az Azure eszközöket. A bővítmény problémákat, a VMs lehet a helyszíni tartomány részét. A helyszíni tartomány felhasználói tartomány férhet hozzá a kiszolgálókat, és szolgáltatások futtatható e VMs (például az adatbázis-kezelő szolgáltatások). VMs közötti kommunikáció és a névfeloldás rendszerbe a helyszíni és telepítését az Azure VMs lehetőség. Ez legyen az SAP-eszközök a Azure találnak a leggyakoribb helyzet megakadályozási. Lásd: [a] [ vpn-gateway-cross-premises-options] cikk és [a] [ vpn-gateway-site-to-site-create] további információt.

> [AZURE.NOTE] Hol találhatók az Azure virtuális gépeken futó operációs rendszert futtató az SAP rendszereihez a helyszíni tartomány tagjai SAP rendszereihez határokon helyszínen telepített gyártási SAP rendszereihez támogatott. Idegen helyszíni konfigurációk alkatrészek telepíti támogatott, és töltse ki az SAP tájak az Azure. A teljes SAP fekvő Azure-ban futó szükséges ezeket a helyszíni tartomány és a HIRDETÉSEK részét VMs problémákat. A dokumentáció korábbi verzióiban azt beszélgetett az informatikai hibrid környezetek kialakításához, ahol a kifejezést a hibrid"arra, hogy van egy helyszíni és Azure határokon helyszíni összekapcsolását gyökerezik is. Ebben az esetben a hibrid"is azt jelenti, hogy a VMs Azure-ban a helyszíni Active Directory részét.

Néhány Microsoft dokumentáció határokon helyszíni esetek különösen az adatbázis-kezelő HA konfigurációk esetén egy kicsit máshogyan kell ismerteti. Amíg az SAP kapcsolódó dokumentumok, az idegen helyszíni forgatókönyv csak forrni kezd le egy webhely vagy magánjellegű (készült ExpressRoute) kapcsolódási problémákat, és arra, hogy az SAP fekvő között a helyszíni és Azure kanadai.

### <a name="resources"></a>Erőforrások
Az alábbi útmutatókat témájának Azure SAP-telepítéshez érhetők el:

* [SAP NetWeaver Azure virtuális gépeken (VMs) – tervezése és üzembe helyezési útmutatóban] [tervezési – útmutató]
* [SAP NetWeaver Azure virtuális gépeken (VMs) – telepítési útmutató] [telepítési útmutató]
* [SAP NetWeaver Azure virtuális gépeken (VMs) – az adatbázis-kezelő telepítési útmutató (a dokumentum)] [adatbázis-kezelő – útmutató]

A következő SAP megjegyzések témájának SAP Azure kapcsolódnak:

| Megjegyzés száma   | Cím
|------------|--------
| [1928533] | SAP-alkalmazások a Azure: támogatott termékek és Azure virtuális típusai
| [2015553] | A Microsoft Azure SAP: támogatja a vonatkozó követelmények
| [1999351] | Fokozott Azure ellenőrzés az SAP – hibaelhárítás
| [2178632] | A Microsoft Azure SAP mértékek figyelése billentyűt
| [1409604] | A Windows virtualizációs: fokozott figyelése
| [2191498] | SAP – Linux az Azure: fokozott figyelése
| [2039619] | SAP-alkalmazások a Microsoft Azure az Oracle-adatbázis használata: támogatja a termékek és a verziókkal
| [2233094] | DB6: SAP-alkalmazások Azure IBM DB2 használata Linux rendszerhez, a UNIX és a Windows - további információt a
| [2243692] | A Microsoft Azure (IaaS) virtuális Linux: SAP licenccel kapcsolatos problémák
| [1984787] | SUSE LINUX vállalati kiszolgáló 12: A telepítéssel kapcsolatos megjegyzések
| [2002167] | Piros kalap vállalati Linux 7.x: telepítési és frissítése

Olvassa el is, amely tartalmazza az összes SAP feljegyzés Linux [Állapotváltozás Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) .

Rendelkeznie kell a Microsoft Azure-architektúra és a Microsoft Azure virtuális gépeken futó rendszerbe és üzemeltetett hogyan végezhető műveletek ismereteket. További információt találhat itt <https://azure.microsoft.com/documentation/>
 
> [AZURE.NOTE] Azt egy (PaaS) szolgáltatásajánlatok a Microsoft Azure platform, a Microsoft Azure Platform **nem** beszél meg. Ez a papír az adatbázis-kezelő rendszer (adatbázis-kezelő) a Microsoft Azure virtuális gépeken futó (IaaS) operációs rendszert futtató ugyanúgy, mint az adatbázis-kezelő futtatni a helyszíni környezetben. Adatbázis funkciók és funkciók között a következő két ajánlatok nagyon eltérőek, és nem kell összekeverni egymást. Lásd még: <https://azure.microsoft.com/services/sql-database/>

Azt IaaS beszél meg, mivel az általános a Windows, a Linux és az adatbázis-kezelő telepítése és konfigurálása a következők ugyanaz, mint bármely virtuális gép vagy bA fémes gépen szeretné telepíteni a helyszíni. Vannak azonban olyan néhány architektúrája és a rendszer management végrehajtása döntéseket amely különböző lesz, amikor IaaS felhasználásával. A dokumentum célja elmagyarázni az adott építészeti és a rendszer különbségeket meg kell készíteni IaaS használata esetén.

Az általános a különbséggel, hogy a papír bemutatja, hogyan lehet általános területek vannak:

* SAP rendszereihez, hogy biztosan a megfelelő adatokat a megfelelő virtuális/virtuális elrendezését tervezési elrendezés fájlt, és érhet el a terhelést a elég IOPS.
* Hálózati szempontok IaaS használata esetén.
* Adott adatbázis-szolgáltatásokra annak érdekében, hogy az adatbázis Elrendezés optimalizálása használni.
* Biztonsági mentési és visszaállítási szempontok IaaS.
* Különböző típusú képek telepítéshez felhasználásával.
* Magas elérhetőség az Azure IaaS.

## <a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>Egy RDBMS telepítési felépítése
Sorrendben hajtsa végre a fejezet, és mi lett foglalt [telepítési útmutató] [telepítési útmutató], [a] [telepítési útmutató, 3] fejezet megértéséhez szükséges. A különböző virtuális számozási és azok és különbségek az Azure Standard és a prémium tárolás ismereteket kell értelmezni, és a fejezet elolvasása előtt ismert.

Március 2015, amíg az operációs rendszert tartalmazó Azure VHD korlátozott 127 GB méretű volt. A korlátozás a Skype 2015 vállalati március használ szüntetni (a további információkat jelölőnégyzet <https://azure.microsoft.com/blog/2015/03/25/azure-vm-os-drive-limit-octupled/> ). A VHD innen az operációs rendszer tartalmazó beállíthatja, hogy bármely más virtuális méretével azonos méretben. Mindazonáltal azt is előnyben részesíti az operációs rendszer, az adatbázis-kezelő és az esetleges SAP bináris esetén az adatbázis-fájlokból külön telepítési szerkezetét. Ezért megakadályozási fut az Azure virtuális gépeken futó SAP rendszereihez fog rendelkezni az alap virtuális (vagy virtuális) telepítve van az operációs rendszerrel, adatbázis kezelése rendszer végrehajtható és az SAP végrehajtható. Az adatbázis-kezelő adatokat, és jelentkezzen be a fájlok fog (normál vagy prémium tároló) Azure-tárolóban lévő külön virtuális-fájlokban tárolt, és az eredeti kép Azure operációs rendszer virtuális logikai tartalmazó csatolva. 

A feljebb helyezése Azure normál vagy prémium tároló (például a Tartományi sorozathoz vagy GS-sorozat VMs) segítségével nincs függő más kvóták Azure-ban, amelyeket [az alábbi][virtual-machines-sizes]. Az Azure VHD tervezésekor kell keresse meg a legjobb egyenlege a kvótájának a következő:

* Adatfájlok száma.
* A fájlokat tartalmazó VHD száma.
* Egy egyetlen virtuális IOPS kvótáinak.
* Egy virtuális fájlmegosztásra.
* További VHD lehetséges egy virtuális méret száma.
* Az általános tárolási átviteli egy virtuális lehet nyújtani.
 
Azure-IOPS kvóta egy virtuális merevlemez-meghajtó kényszeríti. Ezek a kvóták eltérőek VHD Azure szabványos tároló és támogatási szolgáltatások is. I/O késések nagyon különböző tényezők jobb I/O késések előadásához prémium adathordozós tárolási két típusa közötti is. Különböző virtuális különböző van, amely lehet a csatolása VHD korlátozott számú. Egy másik Ez a korlátozástípus akkor, hogy csak bizonyos virtuális típusú is élvezheti Azure prémium tárhelyet. Ez azt jelenti, hogy bizonyos típusú virtuális döntés előfordulhat, hogy nem csak alapján vezeti a Processzor- és követelményeket, hanem a IOPS, általában méretezése VHD számát vagy a prémium tároló lemez típusú időtartama és a lemez átviteli követelmények. Különösen prémium adathordozós egy virtuális méretét is előfordulhat, hogy szabja IOPS és átviteli valósítható meg minden virtuális kell, hogy a számát.

Előfordulhat, hogy az, hogy a teljes IOPS ráta; csatlakoztatott VHD számát, és a virtuális méretének összes tartozik együtt, egy Azure konfigurálása az SAP rendszer lehetnek, mint a helyszíni példányban. A IOPS egy logikai határértékeket általában a helyszínen telepített konfigurálását. Mivel az Azure adathordozós ezeket a korlátokat rögzített vagy ahogy prémium tárolási függő a lemez adja. Így tartalmazó helyszíni telepítések láthatja ügyfél konfigurációk számos különböző kötet használja az SAP és az adatbázis-kezelő vagy speciális kötet ideiglenes adatbázisok vagy táblázat szóközöket például különleges végrehajtható adatbázis-kiszolgálók. Ha például egy helyszíni rendszer Azure helyez át azt vezethet egy potenciális IOPS sávszélesség hulladéka próbálgatás egy virtuális végrehajtható vagy az adatbázisok, amelyek nem hajtja végre nem IOPS sok vagy tetszés szerint. Emiatt az Azure VMs azt javasoljuk, hogy az adatbázis-kezelő és az SAP végrehajtható telepíthető az operációs rendszer lemezen, ha lehetséges.

Az adatbázis fájlok és a naplófájlokat és Azure tárterületet használ, milyen típusú helyzetének IOPS, időtartam és átviteli követelmények kell meghatározni. Annak érdekében, hogy van elegendő a tranzakciónaplója az IOPS, előfordulhat, hogy ki kell kihasználhatja a tranzakció naplófájl több VHD, vagy használhatja a prémium tárterület (GB) nagyobb lemezen. A szoftver a a VHD, amely tartalmazza a tranzakciónaplója az ebben az esetben egy egyszerűen volna összeállítása RAID (például Windows tároló készlet for Windows vagy MDADM és LVM (logikai mennyiségi kezelő) Linux az).

___

> ![A Windows][Logo_Windows] A Windows
>
> Az Azure virtuális gép D:\ meghajtó nem tárolt meghajtóra, amely egyes helyi lemezen az Azure számítási csomóponton mögött. Mivel nem tárolt, ez azt jelenti, hogy a tartalom a D:\ meghajtón végzett módosítások esetén elvesznek a virtuális újraindítása után. "Módosítások" "azt jelenti mentett fájlokat, a létrehozott könyvtárak, a telepített alkalmazások, a stb.
>
> ![Linux][Logo_Linux] Linux
>
> Linux Azure VMs automatikusan /mnt/resource, vagyis helyi lemezen az Azure számítási csomóponton mögött nem tárolt meghajtó meghajtót csatlakoztatni. Mivel nem tárolt, ez azt jelenti, hogy /mnt/resource tartalmának végzett módosítások esetén elvesznek a virtuális újraindítása után. Módosítások azt jelenti, mentett fájlokat, a létrehozott könyvtárak, a telepített alkalmazások, a stb.

___

Az Azure virtuális adatsor függ, a helyi lemezt a számítási csomópontra megjelenítése különböző teljesítmény sorolható, például:

* A0 – A7: Nagyon kevés teljesítményét. Az összes windows lap fájl túl nem használható
* A8-A11: Igen jó teljesítményt jellemzőiket befolyásolják az egyes tíz ezer IOPS és > 1GB/sec átviteli
* D-sorozat: Igen jó teljesítményt jellemzőiket befolyásolják az egyes tíz arányosítva IOPS és > 1GB/sec kapacitása.
* Tartományi-sorozat: Igen jó teljesítményt jellemzőiket befolyásolják az egyes tíz ezer IOPS és > 1GB/sec átviteli
* G-sorozat: Igen jó teljesítményt jellemzőiket befolyásolják az egyes tíz ezer IOPS és > 1GB/sec átviteli
* GS-sorozat: Igen jó teljesítményt jellemzőiket befolyásolják az egyes tíz ezer IOPS és > 1GB/sec átviteli

A fenti kimutatások vannak alkalmazása biztosítására tanúsítvánnyal rendelkező SAP virtuális adattípusa. A virtuális-sorozat kiváló IOPS és átviteli bizonyos az adatbázis-kezelő szolgáltatások, például tempdb vagy a ideiglenes tábla terület szerint emelés jogosultak.

### <a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>Gyorsítótár-VMs és VHD
E lemezt/VHD a portálon keresztül, vagy azt a feltöltött VHD VMs szeretne csatlakoztatni létrehozásakor azt adhatja meg a virtuális és ezeket a Azure tárban található VHD I/O forgalmát gyorsítótárban e. Azure normál és prémium tárolási használata két különböző technológiák gyorsítótár ilyen típusú. Mindkét esetben a gyorsítótár maga szeretné lemez készül a virtuális ideiglenes lemez (Windows D:\) vagy Linux /mnt/resource által használt azonos meghajtókon.
 
Azure szabványos tárhely a lehetséges gyorsítótár-típusok a következők:

* Nincs gyorsítótárazás
* Olvassa el a gyorsítótár
* Olvasási és írási gyorsítótárazás

Ahhoz, hogy következetes és mérvadó teljesítmény, beállíthatja a gyorsítótárazás Azure szabványos tároló összes rendszerindításának tartalmazó **az adatbázis-kezelő kapcsolódó adatfájlok, a naplófájlokat és a hely a táblázat "Nincs"**. A virtuális gyorsítótárazása maradhat az alapértelmezett.

Azure prémium tárhelyként használható a következő gyorsítótárazási lehetőségek állnak rendelkezésre:

* Nincs gyorsítótárazás
* Olvassa el a gyorsítótár

Azure prémium tároló való megjelöléséről értekezletállapota megjelenjen, **olvassa el a gyorsítótár-adatfájlok** az SAP-adatbázis, és **nem a napló fájlokat a VHD(s) gyorsítótárazás**választotta.

### <a name="c8e566f9-21b7-4457-9f7f-126036971a91"></a>A szoftver RAID
A korábban már említettük, szüksége lesz az adatbázis-fájlok között adhatja VHD számát szükséges IOPS számát egyenleg, és a maximális IOPS az Azure virtuális ad egy virtuális vagy prémium tároló lemezen írja be. A IOPS betöltés foglalkozik fölé VHD legegyszerűbben a szoftver RAID össze a különböző VHD fölé. A logikai ki a szoftver RAID faragottnak majd helyezze az SAP az adatbázis-kezelő az adatfájlok számos. Függő, érdemes lehet érdemes prémium tárolási kettő három óta, valamint a használatát az igényeknek megfelelően alakítható különböző prémium tárolási lemezt adja meg szabványos tárolási alapján VHD-nál nagyobb IOPS kvóta. Azure prémium tároló által megadott értékes jobb I/O késés mellett. 

Ugyanaz a tranzakciónaplója a másik adatbázis-kezelő rendszerek vonatkozik. Sok őket közvetlenül a további Tlog fájlok hozzáadása a nem segít óta egyszerre csak egyet a fájlok írja be az adatbázis-kezelő rendszerek. Ha nagyobb IOPS sebesség van szükség, mint a tárterület-alapú egységes virtuális tartana, akkor is paritásos több szabványos tároló VHD fölé, vagy nagyobb prémium tároló lemez típusú, amely nagyobb IOPS sebesség túl is biztosítja tényezők alsó késés műveletekkel együtt be a tranzakciónaplója írásra is használhatja.
 
Esetek, amikor a Azure környezetekben, amely esetben alkalmazást, a szoftverrel tapasztalt RAID a következők:

* Log/mégis Tranzakciónaplója, mint az Azure virtuális egyetlen merevlemez nyújt további IOPS szükség. Fent említett Ez megoldható egy logikai épület szoftverrel RAID több VHD fölé.
* Páratlan I/O terhelést eloszlás fölé a különböző adatfájlok az SAP-adatbázis. Ebben az esetben egy sorolunk meglehetősen gyakran szerezze meg a kvóta egy adatfájlt. Mivel a más adatfájlok még nem jutnak közelébe az egy egyetlen virtuális IOPS kvótáját. Ebben az esetben a legegyszerűbb megoldás, egy logikai össze több VHD szoftverrel RAID fölé. 
* Nem tudja, hogy milyen adatok fájlonként pontos I/O terhelését, és csak nagyjából tudható, hogy milyen általános IOPS terhelését az adatbázis-kezelő szemben. Legegyszerűbben a teendő, ha a szoftver RAID segítségével egy logikai összeállítása. A logikai mögött több VHD kvótáinak összege kell majd teljesítése az ismert IOPS ráta.

___

> ![A Windows][Logo_Windows] A Windows
>
> A Windows Server 2012 vagy magasabb tárhelyek használatát célszerű, mivel ez éppen hatékonyabb, mint a Windows korábbi verzióinak Windows csíkozást. Ügyeljen arra, hogy szükség lehet létrehozása a Windows tároló készletek és tárhelyek PowerShell-parancsok használata a Windows Server 2012 operációs rendszer esetén. A PowerShell-parancsait megtalálható itt <https://technet.microsoft.com/library/jj851254.aspx>

> 
> ![Linux][Logo_Linux] Linux
>
> Csak a MDADM és a LVM (logikai mennyiségi kezelő) létrehozásához a szoftver Linux RAID támogatott. További tudnivalókért olvassa el az alábbi cikkekben:
>
> * [Állítsa be a szoftver Linux RAID] [ virtual-machines-linux-configure-raid] (a MDADM)
> * [Azure-ban egy Linux virtuális LVM konfigurálása][virtual-machines-linux-configure-lvm]


___

Megfontolandó szempontok feljebb helyezése virtuális-sorozat, amelyek képesek általában Azure prémium tárhely kezelése a következők:

* I/O késések, amelyek közelébe SAN/hálózati hitelesítési szolgáltatás eszközök előadása igénylésének.
* Igény szerint az tényezők jobb I/O késés, mint az Azure szabványos tároló tartana.
* Írja be a Mit érhető el a több szabványos tárolási VHD egy bizonyos virtuális szemben, mint egy virtuális magasabb IOPS.

Mivel az alapul szolgáló Azure tároló replikálja minden virtuális csomópontok tároló legalább három egyszerű RAID 0 csíkozást is használható. Nincs szükség az RAID5 vagy RAID1 végrehajtásához.

### <a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>Microsoft Azure-tárolóhoz
Microsoft Azure tároló legalább 3 külön tároló csomópontok tárolja az Alap (ha az operációs rendszerrel) virtuális és VHD vagy BLOB. Tárterület fiók létrehozásakor választási lehetőség van védelem itt ismertetett módon:

![Azure tárolási fiókhoz engedélyezett GEO replikációs][dbms-guide-figure-100]

Azure tároló helyi replikációs (helyben felesleges) tartalmaz, amely néhány ügyfelek sikerült engedheti meg magának üzembe infrastruktúra hiba miatt adatvesztés elleni védelem szintek. Amint a fentiekből lehetőség 4 különböző az ötödik éppen változata, az első három közül. Megjeleníti őket elemre közelebbi azt különböztethetjük meg:

* **Prémium helyileg felesleges tároló (LRS)**: Azure prémium tároló biztosítja e/O-igényes munkaterhelésekből futó virtuális gépeken futó nagy teljesítményű, alacsony-késés lemezt támogatása. Vannak olyan 3 replikái az adatokat az azonos Azure adatközponthoz egy Azure terület belül. A másolatok lesznek a különböző hibafa és tartományok frissítése (fogalmak lásd:, [a]-[tervezési – útmutató a-3,2] fejezet a [tervezési Guide][planning-guide]). Abban az esetben, ha az adatokat egy tárolási csomópont hibája vagy lemez meghibásodása miatt szolgáltatás ki fogja replikáját új kópiát automatikusan létrejön.
* **Helyi meghajtóra felesleges tároló (LRS)**: Ebben az esetben is meg vannak az adatok belül egy Azure terület azonos Azure adatközponthoz 3 kópiák. A másolatok lesznek a különböző hibafa és tartományok frissítése (fogalmak lásd:, [a]-[tervezési – útmutató a-3,2] fejezet a [tervezési Guide][planning-guide]). Abban az esetben, ha az adatokat egy tárolási csomópont hibája vagy lemez meghibásodása miatt szolgáltatás ki fogja replikáját új kópiát automatikusan létrejön. 
* **GEO felesleges tároló (GRS)**: Ebben az esetben nem fog hírcsatorna-további 3 kópiák az adatok, egy másik, amely a legtöbb esetben ugyanabban a földrajzi régióban (például Észak-Európa és a nyugati Europe) Azure régióban egy aszinkron replikációs. Ekkor 3 további replikák, hogy 6 kópiák összefoglalva. Ez változata kiegészítése hol az adatokat a geo replikált Azure régióban (olvasásra Geo-felesleges) olvasási célokra használható.
* **A kijelzőzónák felesleges tároló (ZRS)**: Ebben az esetben az adatok a 3 replikák marad, ugyanabban a Azure régióban. [A] című cikkben ismertetett módon [tervezési, útmutató, 3.1] fejezet [tervezési útmutató] [tervezési – útmutató] egy Azure terület lehet adatközpontokkal számos közelében. Az LRS esetében a replikák volna kell elosztva a különböző adatközpont esetén, amelyek egy adott Azure területre.

További információt talál [az alábbi][storage-redundancy].
 
> [AZURE.NOTE] Az adatbázis-kezelő telepítésekhez a felesleges tároló Geo használatát nem ajánlott
>
> Azure tároló Geo replikációs aszinkron. Az egyes VHD egy egyetlen virtuális gép csatlakoztatott replikációs nem szinkronizálódnak a zárolás lépésben. Emiatt nem alkalmas való replikáció az adatbázis-kezelő fájlokat, amelyek különböző VHD elosztva vagy egy több VHD alapján RAID szoftverek ellen rendszerbe. Az adatbázis-kezelő szoftver használatához szükséges, hogy az állandó lemezterület pontosan szinkronizált különböző logikai és az alapul szolgáló lemez/VHD/tengelyek keresztül. Az adatbázis-kezelő szoftvert használ különböző mechanizmusok a sorozat IO írási tevékenységekre, és egy adatbázis-kezelő jelenti, hogy a lemezterülettel, a replikáció célként sérült, ha ezek típusától függően változnak akár néhány ezredmásodperc. Ezért egyik valójában nem felel meg egy adatbázis konfigurációs kiterjesztett keresztül több VHD geo replikált adatbázist tartalmazó, ha egy ilyen replikációs kell adatbázis eszközök és funkciók kell elvégezni. Egy nem használja a Azure tároló Geo-való replikáció a feladat végrehajtásához arra. 
>
> A probléma nem legegyszerűbben ahhoz, hogy elmagyarázhasson példa használhat. Tegyük fel, az SAP rendszer feltölteni az Azure, amelynek adatfájlok az adatbázis-kezelő, valamint a tranzakció naplófájlt tartalmazó egy virtuális tartalmazó 8 VHD van. A következő 9 VHD mindegyik fog rendelkezni az adatbázis-kezelő, megfelelően következetes módszert a írt adatok, hogy az adatok vagy tranzakció naplófájljának ír, a az adatok.
>
> Az adatok való replikáció megfelelően geo érdekében és egységes adatbázis képként karbantartása összes kilenc VHD tartalmát a vágólapra kell geo replikált szemben a kilenc különböző VHD végrehajtása történt meg a bemeneti és kimeneti műveletet meghatározott sorrendben kell. Azure tároló geo replikációs azonban nem engedi VHD közötti függőségek deklarálása. Ez azt jelenti, hogy a Microsoft Azure tároló geo replikációs arra, hogy a tartalom, az alábbi kilenc különböző VHD egymáshoz kapcsolódó és az adatok módosítását egységes csak akkor, ha a bemeneti és kimeneti műveletet abban a sorrendben replikálása történt, a 9-es VHD keresztül nem tud.
>
> Amellett, hogy magas, hogy az alkalmazási példát geo replikált képek nem adja meg egy következetes adatbázis képet, és van is rendszer teljesítményét geo felesleges tároló szigorúan is jelenik meg, hogy a teljesítményt. Az összefoglaló ne használjon tároló redundancia ilyen típusú az adatbázis-kezelő típus munkaterhelésekből.
 
#### <a name="mapping-vhds-into-azure-virtual-machine-service-storage-accounts"></a>Hozzárendelés VHD az Azure virtuális szolgáltatás tárolási számítógépfiókok
Azure tároló fiók, nem csak egy felügyeleti szerkezetet, hanem egy korlátozások tárgya. Mivel a korlátozások változnak meg, hogy megvitatjuk-Azure szabványos tárolási vagy egy Azure prémium tárolási fiókkal. A pontos jellemzői és korlátai szerepel [Itt][storage-scalability-targets]
 
Ezért Azure szabványos tárolására fontos megjegyzés, a használati tárterület-fiók a IOPS korlátozva van (sort tartalmazó "Teljes kérése árfolyam" [című témakörben][storage-scalability-targets]). Ezenkívül van egy kezdeti korlát 100 tároló fiókok Azure előfizetésenként (kezdve 2015 július). Ezért ajánlott IOPS a VMs egyenleg Azure szabványos tárolási használata esetén több tárterületet fiók között. Mivel a egyetlen virtuális ideális egy tárolási fiókot használ, ha lehetséges. Így megvitatjuk, ahol minden virtuális Azure szabványos tároló tárolt elérhetik a saját kvótakorlátja az adatbázis-kezelő telepítések, ha kell csak rendszerbe 30-40 VHD per Azure szabványos tároló használó Azure tárterület-fiókkal. Azonban ha kihasználhatja az Azure prémium tárolási és nagy adatbázis kötet tárolja, akkor is értelmez IOPS aggódnia. De tároló fiók Azure prémium módja szigorúbb adatok mennyiségi az Azure-szabványos tárterület-fiókjába. Szerezze meg az adatok mennyiségi határérték mielőtt eredményeként belül Azure prémium tároló fiók VHD korlátozott számú csak telepítheti. A a befejezési tudományos, mint "Virtuális SAN" korlátozott funkciók IOPS és/vagy kapacitás az Azure tároló fiók. Emiatt a feladat megőrződik, ahogy a helyszínen telepített meghatározása a különböző SAP rendszerek a VHD elrendezését a különböző "képzetes SAN eszközök" vagy az Azure tároló fiókok fölé.
 
Azure szabványos tárolására nem ajánlott, ha lehetséges egyetlen virtuális eltérő tárolási fiókokból tároló bemutatására.

Mivel a Tartományi vagy Azure VMs GS sorozata ajánlatos csatlakoztatási VHD Azure szabványos tárhely és a prémium tároló fiókok ki. Használati eset például biztonsági másolatok írása a szabványos tároló biztonsági VHD, mivel az adatbázis-kezelő adatok, és a naplófájlok prémium tárolón állnak rendelkezésre hol ilyen eltérő tárolás kapcsolatos sikerült károkozásra. 

Ügyfél-telepítések és vizsgálata körülbelül 30-40 VHD adatbázisfájlok adatok és a naplófájlok tartalmazó kiépítése egyetlen Azure szabványos tárterület-fiókjában elfogadható teljesítmény alapján. A korábban említett Azure prémium tároló fiók korlátai valószínűleg az adatok kapacitás tárolható és nem IOPS.

Mint SAN eszközök a helyszíni megosztása igényel néhány figyelése annak érdekében, hogy végül észleli a Azure tároló fiókra szűk. Az SAP- és az Azure-portálra a Azure figyelése bővítmény elfoglalt, lehet, kézbesítése optimálisnál IO teljesítmény Azure tárolási fiókok feltárása használható eszközök.  Ha ezt a helyzetet észlelésekor ajánlott elfoglalt VMs áthelyezése egy másik Azure tárterület-fiókjába. Olvassa el a [telepítési útmutató] [telepítési útmutató] aktiválása az SAP állomás figyelése lehetőségeket olvashat.

Ajánlott eljárások megalkotása Azure szabványos tároló és Azure szabványos tároló fiókok engedélyeiről cikk megtalálható itt <https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx>
 
#### <a name="moving-deployed-dbms-vms-from-azure-standard-storage-to-azure-premium-storage"></a>Az adatbázis-kezelő VMs Azure szabványos tárhelyről Azure prémium tárolóhoz áthelyezése rendszerbe.
Azt, bizonyos igazán bizonyos esetekben, ahol vevőt áthelyezni kívánt egy telepített virtuális Azure szabványos tárhelyről Azure prémium tárolóba. Ez nem lehetséges az adatok fizikailag helyezés nélkül. Többféle módon elérése a cél:

* Egyszerűen átmásolhatja összes VHD, alap virtuális, valamint adatokat VHD új Azure prémium tárterület-fiókba. Gyakran kiválasztott VHD számát szabványos Azure-tárolóban lévő arra, hogy van szüksége az adatok mennyiségi miatt nem. De ennyi VHD van szüksége a IOPS miatt. Most, hogy az Azure prémium tároló áthelyez, sikerült nyissa módszer kevésbé VHD az egyes IOPS teljesítmény elérése érdekében. Tekintve, hogy az Azure szabványos tárolási fizet a felhasznált adatok és a nem a névleges lemez méretét VHD számát did nem igazán számít költségek pedig. Azonban Azure prémium adathordozós volna fizet a névleges lemez méretét. Ezért a legtöbb az ügyfél próbálja meg a őrizni Azure VHD számát a prémium tárhely a számot a IOPS átviteli eléréséhez szükséges szükséges. Igen a legtöbb ügyfelek döntse el, szemben a módon, egy egyszerű 1:1 másolása.
* Ha még nem csatlakoztatva, egy egyetlen virtuális, amely tartalmazhat adatbázis biztonsági másolatának az SAP-adatbázis csatlakoztatása. A biztonsági mentés után válassza az összes VHD, beleértve a biztonsági másolatot tartalmazó virtuális le, és másolja a vágólapra az alap virtuális és a virtuális a mentéssel Azure prémium tárterület-fiókba. Azt, akinek majd a virtuális az alap virtuális alapján telepítése és csatlakoztatása a virtuális a biztonsági mentést. Most hoz létre üres prémium tároló további lemezt a virtuális be az adatbázis visszaállítása felhasznált. Ez azt feltételezi, hogy az adatbázis-kezelő lehetővé teszi, hogy az adatokat, és jelentkezzen be a fájl elérési út módosítása a helyreállítási folyamat részeként.
* Egy másik lehetőség, a korábbi folyamat, ahol csak másolja be a biztonsági mentés virtuális Azure prémium tárolási és csatolja a virtuális újonnan rendszerbe és telepített ellen változata.
* A negyedik lehetőséget akkor válassza a közben rászoruló módosítani szeretné a számát az adatbázis adatfájlok. Ebben az esetben elvégeznie SAP homogén rendszer másolatának exportálási/importálási használatával. Fájlok azok exportálása egy virtuális Azure prémium tárolási fiók másolja a program, és csatolja a virtuális az importálási folyamat futtatásához használt helyezése. Ügyfelek használja ezt a lehetőséget, főleg, ha a kívánt adatok fájlok számának csökkentése.

### <a name="deployment-of-vms-for-sap-in-azure"></a>Az SAP Azure-ban a VMs telepítésének
Microsoft Azure VMs és a kapcsolódó lemez telepítése több lehetőséget is kínál. Egyúttal nagyon fontos, hogy megérteni a különbségeket, mivel előkészületet, a VMs függő telepítési módja eltérő lehet. Általánosságban elmondható hogy vizsgálja meg a az jelenik meg a következő fejezetekben ismertetett.

#### <a name="deploying-a-vm-from-the-azure-marketplace"></a>Az Azure piactéren lévő egy virtuális üzembe helyezése
Szeretné, hogy a virtuális bevezetését tervezi a Microsoft vagy 3 résztvevős, feltéve, a Microsoft Azure piactéren lévő kép. Azure-ban a virtuális telepítette, akkor kövesse az azonos útmutatások és eszközök belül a virtuális SAP szoftver telepítése, mint a helyszíni környezetben. Az a Azure virtuális belül az SAP szoftvert telepít, SAP és a Microsoft javasoljuk feltöltéséhez és tárolni az SAP telepítési adathordozóját Azure VHD vagy létrehozása az Azure virtuális működik, mint "fájlkiszolgálóra" az összes a szükséges SAP telepítési adathordozóját tartalmazó.

#### <a name="deploying-a-vm-with-a-customer-specific-generalized-image"></a>A virtuális az ügyfélnek adott általános képével üzembe helyezése
Adott javítás szükségletek szerint az operációs rendszer és az adatbázis-kezelő verziója a Microsoft Azure piactéren megadott képek előfordulhat, hogy nem felel meg az igényeinek. Ezért előfordulhat, hogy létrehozásához szükséges egy saját "private" OS/adatbázis-kezelő virtuális kép, amely ezután többször telepíthető segítségével virtuális. Ismétlődések ilyen egy "private" kép elkészítéséhez az operációs rendszer kell általános, kattintson a helyszíni virtuális. Kérjük, olvassa el a [telepítési útmutató] [telepítési útmutató] egy virtuális generalize olvashat.

Ha már telepítette SAP-tartalmat a helyszíni virtuális gép (különösen a 2-réteg System), módosíthatja az SAP rendszerbeállításoktól, után a Azure virtuális keresztül a példány a központi nevezze át az SAP szoftver kiépítési Manager (SAP Megjegyzés [1619720]) által támogatott eljárást. Egyéb esetben később a Azure virtuális telepítését követően az SAP-szoftver is telepítheti.
 
Kezdve az SAP-alkalmazások által használt adatbázis tartalom vagy létrehozhat a tartalom frissen alapján egy SAP-telepítést, vagy importálhatja a tartalom Azure a Microsoft Azure tároló közvetlenül a biztonsági másolatot készít az adatbázis-kezelő eszköz képességeit feljebb helyezése vagy egy virtuális az adatbázis-kezelő adatbázis biztonsági másolatának használatával. Ebben az esetben is VHD készítenie az adatbázis-kezelő adatokat, és jelentkezzen be fájlokat a helyszíni az is, majd importálnia azokat tartalmazó Azure. De a továbbított az adatbázis-kezelő adatok első betöltése a helyszíni az Azure virtuális lemezt kell a helyszíni elkészíteni fölé működő.

#### <a name="moving-a-vm-from-on-premises-to-azure-with-a-non-generalized-disk"></a>Áttérés a virtuális a helyszíni Azure-általános lemezen
Azt tervezi, hogy a helyszíni áthelyezése egy adott SAP rendszer Azure (felvonó és a SHIFT billentyűt). Ez történik a virtuális, amely tartalmazza az operációs rendszer, az SAP bináris és az esetleges az adatbázis-kezelő bináris plusz az adatokat, és jelentkezzen be az adatbázis-kezelő az Azure fájljai merevlemezekkel feltöltésével. A #2. a fenti forgatókönyv leváló, ne a hostname (állomásnév), az SAP biztonsági AZONOSÍTÓK és az SAP-fiókokkal a Azure virtuális, azok a helyszíni környezetben fiókoknál. Emiatt a kép generalizing már nem szükséges. Ebben az esetben főként ha az SAP fekvő része fut a helyszíni és az alakzatok Azure határokon helyszíni felhasználási területei lesz érvényes.

## <a name="871dfc27-e509-4222-9370-ab1de77021c3"></a>Magas rendelkezésre állásának és a az Azure VMs helyreállítás
Azure kínál a következő magas elérhetőség (HA) és katasztrófa helyreállítási (DR) funkciók, amely azt SAP és az adatbázis-kezelő telepítésekhez használna különböző összetevőket alkalmazása

### <a name="vms-deployed-on-azure-nodes"></a>Azure csomóponton rendszerbe VMs
Az Azure Platform telepített VMs szolgáltatások, például Live áttelepítési nem ajánlja. Ez azt jelenti, hogy van karbantartási szükséges a kiszolgáló fürt, amelyen telepítve van egy virtuális, ha a virtuális első leáll, és újraindul kell. Azure-ban karbantartási használatával történik úgy nevű fürt-kiszolgálók frissítése a tartományok. Egyszerre csak egy frissítése tartomány követettek. A számítógép újraindítása során fennakadás-szolgáltatás billentyűt a virtuális leállítása, a végrehajtott karbantartási virtuális indítani. A legtöbb az adatbázis-kezelő szállítók azonban funkcióit magas rendelkezésre állásának és a helyreállítás gyorsan indítani egy másik csomópont az adatbázis-kezelő szolgáltatást, ha az elsődleges csomópontok nem érhető el. Az Azure Platform terjesztheti VMs, tárolása és egyéb Azure szolgáltatás tartományok frissítése annak érdekében, hogy tervezett karbantartás vagy infrastruktúra hibák csak hatással VMs vagy szolgáltatások kis részhalmazát funkciót kínál.  Ügyeljen tervezéssel az elérhetőség szintek helyszíni infrastruktúra anyagokéhoz eléréséhez.

Microsoft Azure elérhetőségének beállítása VMs logikai csoportosítása vagy szolgáltatások, amely biztosítja VMs és más szolgáltatások elosztott különböző hibafa és frissítése a tartományok fürt belül úgy, hogy csak lenne egy csomópont leállítás, amely bármely időben ( [Ez] olvasható[ virtual-machines-manage-availability] további információt a cikk).

Meg kell úgy kell konfigurálni célját közbeni VMs, ahogy itt látható, ha:

![Az adatbázis-kezelő HA konfigurációk elérhetőségének beállítása meghatározását][dbms-guide-figure-200]

Ha azt szeretné, létrehozása az adatbázis-kezelő telepítések erősen elérhető konfigurációjának (az egyes az adatbázis-kezelő HA funkciókat használt független), az adatbázis-kezelő VMs kimutatásadatokat:

* Adja hozzá a VMs Azure virtuális hálózatához (<https://azure.microsoft.com/documentation/services/virtual-network/>)
* HA konfiguráció a VMs kell ugyanahhoz az alhálózathoz is. A különböző alhálózat közötti névfeloldás nem lehetséges a felhőben környezetekben, csak az IP-felbontás működni fog. Webhely vagy készült ExpressRoute kapcsolódási határokon helyszínen telepített használata esetén a hálózaton: legalább egy alhálózat már létrejön. Névfeloldás akkor történik, aszerint, hogy a helyszíni Active Directory házirendek és a hálózati infrastruktúrát. 
[Megjegyzés]: <>  (MSSedusch teendő próba a ARM továbbra is igaz)

#### <a name="ip-addresses"></a>IP-címek
Ajánlott a VMs, HA konfiguráció beállítása rugalmassá útján. IP-címek és a HA közös belül a HA konfiguráció cím használna nem megbízható Azure-ban csak statikus IP-címek használnak. Azure két "Leállítás" fogalmak van:

* Azure portálja vagy az Azure PowerShell-parancsmag leállítása-AzureRmVM keresztül leállítása: Ebben az esetben a virtuális gép leállítás kap, és vonja kiosztva. Az Azure-fiók már nem megterheljük a virtuális, így az egyetlen költségek merülnek fel, amelyekre használt tárolására szolgáló. Ha a hálózati kapcsolaton magánjellegű IP-címe nem volt statikus, megjelent az IP-címet, és nem biztos, hogy a hálózati kapcsolaton lekéri a virtuális újraindítása után újra a régi IP-cím. A Leállítás elvégzéséhez a le vagy leállítása-AzureRmVM hívja fel az Azure-portálon keresztül automatikusan okozó megszüntetése terhelés. Ha nem szeretne deallocat a gép leállítása-AzureRmVM - StayProvisioned használata 
* Ha kikapcsolja a virtuális-OS szintű, a virtuális kap állítsa le és nem vonja kiosztott. Jó helyen jár ebben az esetben az Azure-fiók továbbra is megterheljük a virtuális annak ellenére, hogy az leállás. Ebben az esetben a egy leállítva virtuális IP-cím a hozzárendelés változatlanok maradnak. A virtuális belül a Leállítás fog nem kényszeríti automatikusan megszüntetése terhelés.

Még ha határokon helyszíni esetben alapértelmezés szerint egy leállás és megszüntetése terhelés azt jelenti, IP-címek megszüntetése hozzárendelés a virtuális, az akkor is, ha a helyszíni házirendek DHCP beállításai különböznek. 

* Az kivétel, ha egy hozzárendel egy statikus IP-cím leírt [Itt], hálózati kártya[virtual-networks-reserved-private-ip].
* Ebben az esetben az IP-cím rögzített marad, amíg a program nem törli a hálózati kapcsolaton.

> [AZURE.IMPORTANT] Annak érdekében, hogy az egyszerű és kezelhető, hogy a teljes példányban, a világos ajánlási beállítása a egy adatbázis-kezelő HA vagy DR konfigurációs belül Azure oly módon, hogy van egy működő névfeloldás között az érintett különböző VMs kapcsolattal VMs.
 
## <a name="deployment-of-host-monitoring"></a>A Host figyelése telepítésének
SAP hatékonyabban az Azure virtuális gépeken futó SAP-alkalmazások használatát, a sharepointban az Azure virtuális gépeken futó futó fizikai állomástól-adatok figyelése host van szükség. Egy adott SAP HostAgent javítás szinten van szükség, amely lehetővé teszi, hogy az SAPOSCOL és az SAP HostAgent ezt a lehetőséget. A pontos javítás szint SAP Megjegyzés [1409604]ismertetését.

A host adatok SAPOSCOL és SAPHostAgent előadása összetevők telepítése és az adott összetevők életciklus-kezelése kapcsolatos részletekért olvassa el a [telepítési útmutató] [telepítési útmutató]

## <a name="3264829e-075e-4d25-966e-a49dad878737"></a>A Microsoft SQL Server adatai

### <a name="sql-server-iaas"></a>Az SQL Server IaaS
Microsoft Azure kezdve, egyszerűen áttelepítheti a meglévő épül a Windows Server platform Azure virtuális gépeken futó SQL Server-alkalmazások. Az SQL Server egy virtuális gépen lehetővé teszi a teljes költség telepítés, kezelése és a vállalati szélessége alkalmazások karbantartás tulajdonjogának csökkentése ezeket az alkalmazásokat, hogy a Microsoft Azure egyszerűen áttelepítésével. Az SQL Server-Azure virtuális gép Rendszergazdák és a felhasználók továbbra is használhatók lesznek az ugyanazon fejlesztés és felügyeleti eszközök, amelyek a rendelkezésre álló helyszíni. 

> [AZURE.IMPORTANT] Ne feledje, hogy nem beszél meg a Microsoft Azure SQL-adatbázissal, amely egy Platform megegyezik egy szolgáltatás ajánlatot, a Microsoft Azure platform. A vitafórum ebben a cikkben az SQL Server termék futó helyszíni telepítésekhez az Azure virtuális gépeken futó, a infrastruktúra vizsgál, egy Azure Service videofunkcióinak ismert. Adatbázis-funkciók és funkciók között a következő két ajánlatok különböző, és nem kell összekeverni egymást. Lásd még: <https://azure.microsoft.com/services/sql-database/>
 
Erősen ajánlott, tekintse át [a] [ virtual-machines-sql-server-infrastructure-services] dokumentáció a továbblépés előtt.

Az alábbi szakaszok a fenti hivatkozást a dokumentáció részeinek hardvereszközöket fogja összesíteni és említett. SAP körül adatai, valamint megemlítik, és néhány fogalmak témakörben olvashat részletesebben. Azonban ajánlott a dokumentáció feletti első az SQL Server adott dokumentációjában olvasása előtt szeretné.

Van néhány SQL Server a IaaS adott adatot ismernie kell a továbblépés előtt:

* **Virtuális gép SLA**: van egy SLA virtuális gépeken futó, amely itt Azure-ban futó: <https://azure.microsoft.com/support/legal/sla/>  
* **SQL-verzió támogatja**: SQL Server 2008 R2 támogatjuk az SAP-ügyfeleknek, és a Microsoft Azure virtuális gép magasabb. Korábbi kiadásai nem támogatottak. Tekintse át az általános [Támogatási utasítás](https://support.microsoft.com/kb/956893) további információt. Felhívjuk a figyelmét arra, hogy az általános SQL Server 2008 által támogatott Microsoft is. Azonban miatt jelentős funkciók SAP – jelent meg, amelyek az SQL Server 2008 R2, SQL Server 2008 R2 az SAP a minimális megjelenése. Ne feledje, hogy az SQL Server 2012 és 2014-es gépe van bővítve mélyebb integrálása a IaaS forgatókönyv (például biztonsági mentése közvetlenül Azure tárolás ellen). Emiatt azt korlátozása a papír SQL Server 2012 és 2014-es legújabb javítás szintekre az Azure.
* **SQL által támogatott funkciók**: leginkább SQL Server-funkcióit támogatja a Microsoft Azure virtuális gépeken futó néhány kivétellel. Az **SQL Server feladatátvételét megosztott lemez használata nem támogatott**.  Az elosztott technológiák, például az adatbázis-tükrözéshez, AlwaysOn rendelkezésre állási csoportok, replikációs, napló szállítási és a szolgáltatás ügynök támogatott egyetlen Azure terület belül. Az SQL Server AlwaysOn is támogatott különböző Azure területek között itt ismertetett: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.  Olvassa el a [Támogatási utasítás](https://support.microsoft.com/kb/956893) további információt. Egy AlwaysOn konfigurációs telepítéséről példa [ezt] a[ virtual-machines-workload-template-sql-alwayson] cikk. Is olvassa el a gyakorlati tanácsok dokumentált [Itt][virtual-machines-sql-server-infrastructure-services] 
* **SQL-teljesítmény**: vagyunk benne, hogy a Microsoft Azure szolgáltatott virtuális gépeken futó más nyilvános felhő virtualizációs szeretne rendelni, de egyes eredményekre alkalmazott összehasonlítva nagyon jól végrehajtása változhat. Nézze meg [a] [ virtual-machines-sql-server-performance-best-practices] cikk.
* **Képek használata a Microsoft Azure piactéren lévő**: A leggyorsabban, egy új Microsoft Azure virtuális üzembe, hogy a Microsoft Azure piactéren lévő kép használata. A Microsoft Azure piactéren lévő képek, SQL Server tartalmazó vannak. A képek, ahol az SQL Server már telepítve van az SAP NetWeaver alkalmazások azonnal nem használhatók. A hiba oka az SQL Server alapértelmezetten telepítve van a ezekkel a képekkel és nem a rendezési sorrend SAP NetWeaver rendszerekhez által igényelt belül. Például a képek használatához, ellenőrizze a fejezet [képekkel egy SQL Server ki a Microsoft Azure piactérről] leírt lépéseket [adatbázis-kezelő – útmutató-5.6]. 
* Nézze meg a [Részleteket árak](https://azure.microsoft.com/pricing/) további információt. Az [SQL Server 2012 licencelési útmutatójának](https://download.microsoft.com/download/7/3/C/73CAD4E0-D0B5-4BE5-AB49-D5B886A5AE00/SQL_Server_2012_Licensing_Reference_Guide.pdf) és az [SQL Server 2014-es licencelési útmutatójának](https://download.microsoft.com/download/B/4/E/B4E604D9-9D38-4BBA-A927-56E4C872E41C/SQL_Server_2014_Licensing_Guide.pdf) is egy fontos erőforrás.
 
### <a name="sql-server-configuration-guidelines-for-sap-related-sql-server-installations-in-azure-vms"></a>Az SQL Server konfigurációs útmutató SAP kapcsolódó SQL Server Azure VMs telepítések

#### <a name="recommendations-on-vmvhd-structure-for-sap-related-sql-server-deployments"></a>Az SAP virtuális/virtuális struktúra arra vonatkozó javaslatokat kapcsolódó SQL Server-telepítések
Megfelelően az általános leírása, SQL Server végrehajtható található, vagy el kell telepíteni a virtuális alap virtuális rendszermeghajtóra (c meghajtó\).  Általában az SQL Server rendszer adatbázisokhoz a legtöbb nem használhatók magas szintű SAP NetWeaver terhelést szerint. Ezért a rendszer adatbázisok az SQL Server (fő msdb és modell), valamint a C:\ meghajtón marad. Kivételt sikerült tempdb, amely az egyes SAP ERP és az összes Sávszélesség-munkaterhelésekből, előfordulhat, hogy vagy magasabb szintű adatok vagy I/O műveletek kötet, amelyek nem illeszkednek a eredeti virtuális. Az ilyen rendszerek az alábbi lépéseket kell elvégezni:

* Az elsődleges tempdb adatok fájl áthelyezése ugyanarra a logikai meghajtóra, az elsődleges adatok fájlokat az SAP-adatbázis.
* Adja hozzá bármilyen további tempdb adatfájlok minden más logikai az SAP-felhasználó adatbázis adatok fájlt tartalmazó meghajtót.
* A logikai meghajtót, amely tartalmazza a felhasználói adatbázis naplófájl adja hozzá a tempdb naplófájlban.
* A fájlokon számítási csomópont tempdb adatokat, és jelentkezzen be a **kizárólag a virtuális típusú helyi SSD használó** D:\ meghajtón lehet helyezni. Ennek ellenére ajánlatos lenne több tempdb adatfájlok használatát. A felhasználónevek D:\ meghajtó kötet eltérőek virtuális függően.
 
Ezek a konfigurációk engedélyezése tempdb nagyobb területet, mint rendszermeghajtóra képes használhatnak. Határozza meg, hogy a megfelelő tempdb méretét, hogy egy ellenőrizheti a meglévő rendszert futtató futtassa a helyszíni tempdb méretű. Ezeken kívül ilyen konfiguráció lehetővé teszik IOPS számok tempdb, amelyek nem tudja ellátni rendszermeghajtóra szemben. Ismét a rendszert futtató a helyszíni Lync-I/O terhelést tempdb ellen, hogy akkor is származhat a IOPS számokat, ha szeretné látni a tempdb várt használható.

A virtuális konfiguráció SAP adatbázis SQL Server fut, amely, és hol tempdb adatok és a tempdb naplófájl kerülnek a D:\ meghajtón jelenne meg:
 
![Hivatkozás az SAP Azure IaaS virtuális konfigurálása][dbms-guide-figure-300]

Ne feledje, hogy rendelkezik-e a D:\ meghajtó függő virtuális tekinti meg különböző méretű. Függő tempdb méret követelménye, előfordulhat, hogy mindenképpen pár tempdb adatokat, és jelentkezzen be és fájlok az SAP adatbázis adatokat, és jelentkezzen be abban az esetben, ha túl kicsi D:\ meghajtóra.

#### <a name="formatting-the-vhds"></a>A VHD formázás
Az SQL Server NTFS méretű legyen rendszerindításának SQL Serverből származó adatot tartalmazó, és a naplófájlok 64 ezer kell lennie. Nincs szükség a D:\ meghajtó formázása nem. Ez a meghajtó előre formázott megtalálható.

Érdekében győződjön meg arról, hogy a visszaállítás vagy az adatbázisok létrehozásának nem előkészíti a adatfájlok által a fájlok tartalma nulla, egy győződjön meg arról, hogy a felhasználói környezet az SQL Server szolgáltatást fut egy bizonyos engedély. A Windows-rendszergazda csoportban lévő felhasználók általában a következő engedélyeket rendelkeznek. Az SQL Server szolgáltatást Windows rendszergazdai jogokkal nem rendelkező felhasználók környezetében fut, ha szükséges, hogy a felhasználó "Feladatok" felhasználó jobbra hozzárendelése.  Részletesebben a Microsoft Tudásbázis-cikk: <https://support.microsoft.com/kb/2574695>
 
#### <a name="impact-of-database-compression"></a>Adatbázis tömörítése hatását
Hol I/O sávszélesség előfordulhat, hogy a korlátozó tényező konfigurációk esetén minden mértéke, amely csökkenti a IOPS segíthet a terhelést a egyik futtatását is lehetővé teszi például az Azure-IaaS helyzetben egymástól. Ezért ha még nem kész, SQL Server-lap tömörítés alkalmazása erősen ajánlott SAP és a Microsoft Azure az adatbázisok egy meglévő SAP feltöltése előtt.
 
Az ajánlási adatbázis tömörítése végrehajtásához Azure feltöltése előtt két oka lehet ki számítható ki:

* A feltölteni kívánt adatok mennyiségét alsó.
* A tömörítési végrehajtás időtartama rövidebb, feltételezve, hogy több processzorok vagy magasabb I/O sávszélesség- vagy kisebb I/O késés a helyszíni használni tudja erősebb hardver.
* Kisebb adatbázis mérete kisebb költségének lemez terhelés vezethet

Adatbázis tömörítés során, valamint az Azure virtuális gépeken futó, ahogyan ezt a helyszíni. Ha további információra kíváncsi, hogyan, akkor egy már meglévő SAP SQL Server adatbázis olvassa el az alábbi: <https://blogs.msdn.com/b/saponsqlserver/archive/2010/10/08/compressing-an-sap-database-using-report-msscompress.aspx>
  
### <a name="sql-server-2014--storing-database-files-directly-on-azure-blog-storage"></a>Az SQL Server 2014-es – tárolás adatbázis fájlok közvetlenül az Azure Blog tároló
Az SQL Server 2014-es adatbázis tárolásához közvetlenül az Azure Blob-tárolóban nélkül jelenik meg körülöttük egy virtuális "csomagolópapír" lehetősége nyílik meg. Különösen az Azure-tárolóhoz vagy kisebb virtuális típusok használatával lehetővé teszi esetek hol is az egyes kisebb virtuális típusú csatlakoztatott VHD korlátozott számú szerint szeretné érvényesíthetők IOPS határain megoldása. Ez számos felhasználói adatbázisok rendszer adatbázisok az SQL Server-azonban nem. Is működik az adatokat, és jelentkezzen be a fájlokat az SQL Server. Ha szeretné, hogy az SAP SQL Server-adatbázis terjesztése "körbefuttatási" helyett így azt be VHD, törő a következő szem előtt:

* A tárhely fiók igényeinek használt el szeretné helyezni az ugyanazon Azure régió, az egyet, amely a virtuális SQL Server telepítése fut-e.
* Korábban szereplő VHD terjesztheti fölé különböző Azure tároló fiókokat illetően érvényesek az adott telepítéshez, valamint mód. Azt jelenti, hogy az Azure-tároló fiók határértékek műveletek I/O számának.
[Megjegyzés]: <>  (MSSedusch Teendők, de használata hálózati sávszélesség és nem tároló sávszélességre, akkor nem?)

Részleteinek üzembe ilyen típusú listája megtalálható: <https://msdn.microsoft.com/library/dn385720.aspx>
 
Annak érdekében, hogy tárolja az SQL Server-adatfájlok közvetlenül Azure prémium tároló, amely itt ismertetett minimális SQL Server 2014-es javítás kiadások van szükség: <https://support.microsoft.com/kb/3063054>. Az SQL Server Azure szabványos tároló adatok fájlok tárolására működik az SQL Server 2014-es kiadását. Azonban nagyon ugyanazokat a javításokat tartalmaz a javítások, amelyek az Azure Blob-tárolóhoz közvetlen használatát az SQL Server-adatfájlok és a biztonsági másolatok megbízhatóbb másik adatsort. Ezért azt ajánljuk az általános használata az alábbi javításokat.

### <a name="sql-server-2014-buffer-pool-extension"></a>Az SQL Server 2014-es pufferelési készlet bővítmény
Az SQL Server 2014-es jelent meg egy új funkciót, ami pufferelési készlet bővítmény nevezik. Ez a funkció az SQL Server egy második szintű gyorsítótár mögött a kiszolgáló helyi SSD vagy a virtuális memória tartott pufferelési készlete kiterjed. Lehetővé teszi az adatok egy nagyobb munkakészlet megtartása "a memóriában". Azure szabványos tárolási elérése és összehasonlítása az Azure virtuális gép helyi SSD a tárolt pufferelési készlet a bővítmény be-e számos tényező gyorsabb.  Ezért feljebb helyezése a helyi D:\ meghajtó kiváló IOPS és átviteli virtuális típusú lehet nagyon lehetővé teszi csökkentheti a IOPS betöltés Azure tárolás ellen, és a lekérdezések válaszidő jelentősen javítása lehetőséget. Ez a cikk különösen akkor, ha nem a prémium tároló vonatkozik. Prémium tárhely és a számítási csomópont prémium Azure olvasható gyorsítótára a használatát az adatfájlok, javasolt nagy különbségek várható. Oka az, hogy mindkét gyorsítótárát (SQL Server pufferelési készlet bővítmény és prémium tárolására szolgáló olvasható gyorsítótár) használja a számítási csomópontok helyi lemezt.
Ha többet szeretne tudni a funkció, ellenőrizze a dokumentáció: <https://msdn.microsoft.com/library/dn133176.aspx> 

### <a name="backuprecovery-considerations-for-sql-server"></a>Az SQL Server biztonsági mentése és helyreállítási megfontolandó szempontok
Ha telepíti az SQL Server Azure be a biztonsági másolat módszertan kell vizsgálni. Még ha a rendszer nem a hatékony munkát rendszert, az SAP-adatbázisban, az SQL Server által üzemeltetett kell lennie rendszeres biztonsági másolat készül. Azure tároló három képek továbbra is, mivel a biztonsági másolatot már kevésbé fontos e az összeomlást tároló végtermék kapcsolatban. Prioritás megfelelő biztonsági mentése és helyreállítása terv karbantartásának oka az, többet is egyenlíti logikai/kézi hibákat, mert a pont, az idő helyreállítási lehetőségeket. Így a cél, vagy használata biztonsági mentés visszaállítása az adatbázis vissza egy bizonyos ponton idő vagy a biztonsági másolatok Azure-ban egy másik rendszer rendezi a meglévő adatbázis átmásolásával. Például meg továbbíthat a 2-réteg SAP-konfiguráció és a 3 irányítási rendszer beállítást az azonos rendszer által visszaállítása biztonsági másolatból.

Háromféleképpen különböző biztonsági SQL Server Azure-tárolóhoz:
 
1. Az SQL Server 2012 CU4 és újabb lehet natív módon biztonsági adatbázisoknál URL-címre. Ez a blogban [az SQL Server 2014-es – rész 5 – biztonsági mentési és visszaállítási kapcsolatos fejlesztések, új funkciók](https://blogs.msdn.com/b/saponsqlserver/archive/2014/02/15/new-functionality-in-sql-server-2014-part-5-backup-restore-enhancements.aspx)részletes. Című [az SQL Server 2012 SP1 CU4 és újabb verzióiban] [adatbázis-kezelő – útmutató-5.5.1].
1. Az SQL Server-verziókban SQL 2012 CU4 előtt egy átirányítási funkció segítségével egy virtuális biztonsági mentést, és alapvetően áthelyezése az írási adatfolyam-Azure tárolóhelyen konfigurált felé. Című [az SQL Server 2012 SP1 CU3 és korábbi verziókban] [adatbázis-kezelő – útmutató-5.5.2].
1. A végleges módja biztonsági másolat készítése a hagyományos SQL Server lemez parancs, amely egy virtuális lemez eszköz alakzatot.  A helyszíni telepítésének minta azonos, és nem tárgyalt részletesen a dokumentumban.

#### <a name="0fef0e79-d3fe-4ae2-85af-73666a6f7268"></a>Az SQL Server 2012 SP1 CU4 és újabb verziók
Ez a funkció lehetővé teszi közvetlenül az Azure BLOB-tárolóhoz biztonsági másolatot. Ez a módszer nélkül más Azure VHD, amely a virtuális és IOPS kapacitás szeretne felhasználni a kell biztonsági. A arról alapjában meg:
 
 ![Windows Azure BLOB-tárolóhoz SQL Server 2012 biztonsági másolat használata][dbms-guide-figure-400]

Az ebben az esetben előnye, hogy egy nem kell televíziózással töltenek VHD tárolja az SQL Server biztonsági mentést. Úgy van kiosztva kevesebb VHD, és az adatok és a naplófájlok virtuális IOPS teljes sávszélességét is használható. Ne feledje, hogy biztonsági maximális mérete legfeljebb 1 TB-e a "Korlátozások" szakasz a jelen cikkben ismertetett módon: <https://msdn.microsoft.com/library/dn435916.aspx#limitations>. Ha a biztonsági másolat mérete ellentétben SQL Server biztonsági másolat tömörítést használja az 1 TB meghaladná mérete, a fejezet [az SQL Server 2012 SP1 CU3 és korábbi verziókban] ismertetett funkciók [adatbázis-kezelő – útmutató-5.5.2] a dokumentum kell használni.

Az adatbázisok Azure Blob-tárolóban biztonsági mentés visszaállítása leíró [kapcsolódó dokumentáció](https://msdn.microsoft.com/library/dn449492.aspx) ajánlani nem közvetlenül az Azure BLOB-tárolóban vissza, ha a biztonsági mentés > 25 GB. Ebben a cikkben az ajánlási teljesítménnyel kapcsolatos szempontok és funkcionális korlátozások miatt nem egyszerűen alapul. Ezért különböző feltételek alkalmazhat egy esetenként.

[Ebben](https://msdn.microsoft.com/library/dn466438.aspx) az oktatóanyagban található a biztonsági másolat ilyen típusú beállítása és kapcsolatos károkozásra hogyan dokumentáció
 
Példa a néhány lépést is olvashatók [Itt](https://msdn.microsoft.com/library/dn435916.aspx).

Biztonsági mentés automatizálása, célszerű a legmagasabb fontos, hogy az egyes biztonsági másolatának BLOB másképp elnevezése. Egyéb esetben írható felül, és a visszaállítás lánc megszakad.
 
Annak érdekében, hogy a biztonsági másolatok 3 különböző típusú közötti dolgot be keverjen ki tanácsos különböző tárolók alatt a biztonsági másolatok használt tárterület-fiók létrehozásához. A tárolók lehet által virtuális csak vagy virtuális és biztonsági másolat típus szerint. A séma ábrája a táblagéptől:
 
 ![Az SQL Server 2012 biztonsági mentése Microsoft Azure tároló BLOB – külön tárterület-fiókon különböző tárolók használatával][dbms-guide-figure-500]

A fenti példában a biztonsági másolatok volna nem lehet elvégezni a azonos tárterület-fiókba, ahol a VMs van telepítve. Új tároló fiókot kifejezetten a biztonsági másolatok lenne. A tároló fiókok belül biztonsági mentése és a virtuális neve típusú mátrixok készült különböző tárolók következő lesz. Az ilyen szegmens fog könnyebben felügyelheti a különböző VMs másolatait.

A BLOB egyik közvetlenül beírja a biztonsági másolatok, nem vesz fel egy virtuális gép a VHD számát. Ezért egyik sikerült a maximális számát, az adott virtuális Termékváltozat adatokhoz csatlakoztatott VHD és maximalizálása tranzakció jelentkezzen be a fájlt, és továbbra is a tárhely a tároló elleni biztonsági végrehajtása. 

#### <a name="f9071eff-9d72-4f47-9da4-1852d782087b"></a>Az SQL Server 2012 SP1 CU3 és a korábbi verziókban
Az első lépést kell végrehajtania annak érdekében, hogy közvetlenül a Azure tárolás ellen biztonsági elérése az msi csatolt [KBA tartalom](https://www.microsoft.com/download/details.aspx?id=40740) letöltése lenne.
 
A x64 letöltési telepítőfájl és a dokumentációt. A fájl telepíti az programot: "A Microsoft SQL Server biztonsági másolat a Microsoft Azure eszköz". A termék dokumentációjában alapos olvasható.  Az eszköz alapvetően működését az alábbi módon:

* Az SQL Server oldalról egy helyet a lemezen az SQL Server biztonsági másolat definiált (nem használható a D:\ meghajtó ez).
* Az eszköz definiáljon szabályokat, irányítsa át a biztonsági mentés más Azure tároló különböző típusú felhasználható teszi lehetővé.
* A szabályok vannak, ha az eszköz úgy irányítja át az írási adatfolyam a biztonsági másolat a egyik VHD/lemezen az Azure tároló helyre, amely a korábban meghatározott.
* Az eszköz hagyja néhány KB méretű kis helyettes fájlok a lemezen virtuális /, amely meg van adva az SQL Server biztonsági. **Ez a fájl a tárolóhelyen kell hagyni, mivel szükség van az Azure-tárhelyről újra visszaállítása.**
    * Ha elvesztette a helyettes fájlt (például keresztül veszteség a adathordozóra, amely a helyettes fájl tartalmaz), és a Microsoft Azure tárterület-fiókhoz biztonsági mentése lehetőséget választotta, ha letölti a tárhely tárolóból azt helyezni a Microsoft Azure tároló keresztül helyettes fájl előfordulhat, hogy helyreállítása. Helyettes fájl egy mappába a helyi számítógépen, ahová az eszköz van konfigurálva észleli, és töltse fel a ugyanabban a tárolóban ugyanazzal a titkosítás jelszóval, ha a titkosítási használta az eredeti szabály a majd helyezze. 

Ez azt jelenti, hogy a séma az SQL Server újabb verzióiban fentebb ismertetett elhelyezhető, valamint az SQL Server verziókban, amelyek nem teszik lehetővé közvetlen címet az Azure tárolási helye helyen.
 
Ez a módszer nem alkalmazható, amely támogatja a biztonsági mentését natív módon Azure tárolás ellen újabb SQL Server-verziókban. Kivételek, ahol a natív biztonsági másolatot az Azure korlátai amelyek gátolják natív biztonsági mentés végrehajtása az Azure.

#### <a name="other-possibilities-to-backup-sql-server-databases"></a>Egyéb lehetőségek biztonsági SQL Server-adatbázishoz
Egyéb lehetőségek biztonsági adatbázisokhoz további VHD csatolhat egy virtuális használt biztonsági másolatok-on történő tárolására. Ebben az esetben meg kellene győződjön meg arról, hogy a VHD nem futnak teljes. Ez a helyzet, ha meg kellene válassza le a virtuális úgy, hogy speak "archiválhatja" és cserélhet le egy új üres virtuális. Az elérési út lefelé lép, ha meg szeretné őrizni ezek VHD az Azure-tároló külön fiókok közül, amelyek az adatbázisfájlokat merevlemezekkel.

A második lehetőséget egy nagy virtuális, beállíthatja, hogy hány VHD csatolt környezetbe. Pl. A 32VHDs D14. Használatával tárhelyek egy rugalmas környezettel hol használják majd biztonsági célok a másik adatbázis-kezelő kiszolgáló megosztások sikerült összeállítása.
 
Ajánlott eljárásokat használ dokumentált [Itt](https://blogs.msdn.com/b/sqlcat/archive/2015/02/26/large-sql-server-database-backup-on-an-azure-vm-and-archiving.aspx) is látható. 

#### <a name="performance-considerations-for-backupsrestores"></a>Biztonsági mentés és visszaállítás teljesítménybeli szempontok
Gépi telepítésekről, ahogy a biztonsági mentés és visszaállítás teljesítmény érték hány kötet párhuzamosan is olvashatók, és a mennyiségek átviteli lehet függ. Ezenkívül a biztonságimásolat-tömörítéssel által használt Processzor felhasználás előfordulhat, hogy jelentős szerepet VMs a az imént legfeljebb 8 Processzor beszélgetésekben. Ezért egyik felveheti:

* A kevesebb számát az adatok tárolására szolgáló VHD fájlok, minél kisebb az általános átviteli az olvasási.
* Minél kisebb Processzor számát a virtuális, a szigorúbb milyen következményekkel járnak a biztonságimásolat-tömörítéssel szálak.
* A kevesebb célok (BLOB vagy VHD) írni a biztonsági mentés, a kisebb a kapacitása.
* Minél kisebb a virtuális mérete, minél kisebb az átviteli tárhelykvótát írása és olvasása Azure-tárhelyről. Független, hogy a biztonsági mentés közvetlenül tárolódnak Azure Blob vagy e mappába kerülnek, amelyeket ismét Azure BLOB VHD.

Microsoft Azure BLOB tárhely és a későbbi kiadásokban a biztonsági másolat cél használatakor áll az egyes adott biztonsági mentése csak egy URL-cím cél kijelölő korlátozni.
 
De amikor a "Microsoft SQL Server biztonsági másolat a Microsoft Azure eszköz" a régebbi verziókban, egynél több fájl cél adhatja meg. Több cél, az a biztonsági mentés méretezheti és a biztonsági másolat átviteli nagyobb. Eredményezne majd, valamint az Azure tárterület-fiókot a fájlokat. A tesztelés, az egyik növekedni biztosan érhet el az átviteli, amelyek egyik sikerült elérése a biztonsági másolat kiterjesztésű több fájl célhelyre használatával szerepelni fog az SQL Server 2012 SP1 CU4 a. Akkor is nem blokkolja az 1TB korlát, ahogy a natív biztonsági másolatot az Azure.

Ne feledje azonban, a átviteli is hol található a biztonságimásolat-et, a Azure tároló fiók típusától függően változnak. Keresse meg a tárhely fiókot egy másik régióbeli, mint a VMs futnak arról lehet. Pl. Futtassa a virtuális konfiguráció nyugati Európában tenné, de helyezze be ellen Észak-Európában biztonsági használt tárterület-fiókkal. Hogy biztosan lesz hatással a biztonsági másolat átviteli, és valószínűleg nem lehetséges azokban az esetekben, ahol a cél tárhely és a VMs fut a azonos regionális adatközponthoz tűnik, egy átviteli 150MB/s létrehozásához.

#### <a name="managing-backup-blobs"></a>Biztonsági másolat BLOB kezelése
Nincs a követelmény, hogy a biztonsági másolatok önálló kezelése. Mivel az elvárásoknak, hogy hány BLOB hozza létre a gyakori tranzakció napló biztonsági mentés végrehajtása, az adott BLOB felügyeleti egyszerűen is használatában az Azure-portálra. Ezért érdemes recommendable egy Azure tároló Explorer használhatja. Vannak olyan több jó is elérhető, amely segítséget nyújt az Azure tárterület-fiók kezelése

* Microsoft Visual Studio az Azure SDK telepítve (<https://azure.microsoft.com/downloads/>)
* Microsoft Azure tároló Explorer (<https://azure.microsoft.com/downloads/>)
* 3. a gyártók által fejlesztett eszközök

[Megjegyzés]: <>  (Még nem támogatott a felhő!)
[Megjegyzés]: <>  (### Azure virtuális biztonsági másolat)
[Megjegyzés]: <>  (VMs az SAP rendszerből készíthető Azure virtuális gép biztonsági másolat funkcióval. Azure virtuális gép biztonsági korai az év 2015 használ bevezetett és eközben szabványos módszer biztonsági mentése a kész virtuális Azure-ban. Azure biztonsági mentése a biztonsági másolatok tárolja az Azure-ban, és lehetővé teszi, hogy újra egy virtuális visszaállítás.) 
[Megjegyzés]: <> (futtatása adatbázisok is biztonsági másolatot következetesen, valamint ha az adatbázis-kezelő rendszerek támogatja a Windows VSS (kötet árnyék szolgáltatás – <https://msdn.microsoft.com/library/windows/desktop/bb968832.aspx>), ahogyan például SQL Server VMs. Így Azure virtuális biztonsági mentéssel visszaállítható biztonsági másolatot az SAP-adatbázis elérése oly módon lehet. Vigyázzon Azure virtuális biztonsági mentés pont az idő a visszaállítja az adatbázisok alapján ez nem lehetséges. Az ajánlási ezért végezze el az adatbázisok biztonsági másolatok helyett használna Azure virtuális biztonsági mentése az adatbázis-kezelő funkciót.) [Megjegyzés]: <> (megismerkedhet Azure virtuális gép biztonsági másolat indítsa el az alábbi <https://azure.microsoft.com/documentation/services/backup/>)

### <a name="1b353e38-21b3-4310-aeb6-a77e7c8e81c8"></a>Az SQL Server képekkel ki a Microsoft Azure piactérről
A Microsoft SQL Server változata tartalmazó VMs a Microsoft Azure piactéren lévő kínál. Olyan SAP-ügyfeleknek, akik a SQL Server és a Windows-licencek szükségesek Ez lehet licenceket kell alapvetően szabályozza be az SQL Server már telepítve van programmal VMs frissítésjelző lehetőséget. Az ilyen a képek használatához az SAP, a következő szempontokat mérlegelve kell elvégezni:

* Az SQL Server nem kiértékelés verziók szerezheti be a csak a "Csak a Windows" virtuális rendszerbe Microsoft Azure piactéren lévő-nál magasabb költségeket. Ezek a cikkek összehasonlítását árának lásd: <https://azure.microsoft.com/pricing/details/virtual-machines/> és <https://azure.microsoft.com/pricing/details/virtual-machines/#Sql>. 
* SAP –, például SQL Server 2012 által támogatott SQL Server-verziókban csak használhatja.
* Az illesztés az SQL Server-példányt, amelyen telepítve van a VMs ajánlja fel a Microsoft Azure piactéren lévő, nem a rendezési sorrend SAP NetWeaver az SQL Server-példányt futtatásához szükséges. Az illesztés módosíthatja, hogy az a következő szakasz útmutatásait.

#### <a name="changing-the-sql-server-collation-of-a-microsoft-windowssql-server-vm"></a>A Microsoft Windows/SQL Server virtuális SQL Server rendezésének módosítása
Mivel az Azure piactéren elérhető SQL Server képek nem állíthatók be a rendezési sorrend, amely szükséges SAP NetWeaver-alkalmazások használata, kell a telepítés után azonnal módosítható. Az SQL Server 2012 Ez lehet tenni a az alábbi lépéseket, amint a virtuális lett telepítve, és rendszergazdaként jelentkezzen be a telepített virtuális képes:

* "Rendszergazdaként" Windows parancs ablak megnyitása
* A könyvtár C:\Program Files\Microsoft SQL Server\110\Setup Bootstrap\SQLServer2012 módosítása
* Hajtsa végre a következő parancsot: Setup.exe/test/művelet REBUILDDATABASE InstanceName = MSSQLSERVER /SQLSYSADMINACCOUNTS = =`<local_admin_account_name`> /SQLCOLLATION = SQL_Latin1_General_Cp850_BIN2   
    * `<local_admin_account_name`> a fiók, amely adták meg a rendszergazda fiókot a keresztül a gyűjteményben az első alkalommal a virtuális telepítésekor.

A folyamat csak kell néhány percet igénybe. Győződjön meg arról, hogy a lépés befejeződött a helyes eredmény az, hogy kérjük, hajtsa végre az alábbi lépéseket:

* Nyissa meg az SQL Server Management Studio eszközben.
* Lekérdezés ablak megnyitása.
* Hajtsa végre a parancs sp_helpsort fő SQL Server-adatbázisban.

A kívánt eredményt hasonlóan kell kinéznie:

    Latin1-General, binary code point comparison sort for Unicode Data, SQL Server Sort Order 40 on Code Page 850 for non-Unicode Data

Ha nem az eredményt, LEÁLLÍTÁSA SAP üzembe helyezése, és vizsgálja meg, a telepítés parancs miért nem működött is. A különböző SQL Server kódlapokat, mint az SQL Server-példányt az alakzatot SAP NetWeaver-alkalmazások telepítésének említett feletti **nem** támogatott.

### <a name="sql-server-high-availability-for-sap-in-azure"></a>Az SQL Server magas rendelkezésre állás az SAP Azure-ban
Korábbi ebben a cikkben említett, nincs hozhat létre, ez a funkció használata a a legrégebbi SQL Server magas elérhetőség megosztott tárolási lehetőség. Ezt a funkciót az a Windows Server feladatátvevő fürt (WSFC) megosztott lemezen használata a felhasználó-adatbázisok (és végül az tempdb) szeretné telepíteni a két vagy több SQL Server-példány. Ez a a túl hosszú ideig szabványos magas elérhetősége módszer, amelyet SAP is támogat. Azure nem támogatja a megosztott tárhely, mert az SQL Server magas elérhetősége konfigurációk egy megosztott lemez fürt beállításokkal nem kell megvalósítani. Magas elérhetősége számos más módszerekkel azonban továbbra is lehetséges, és az alábbi szakaszok ismertetik.

[Megjegyzés]: <>  (A témakör az továbbra is a ASM refering)
[Megjegyzés]: <>  (Az SQL Server Azure-ban használható különböző adott magas elérhetőség technológiák olvasása előtt van egy nagyon hasznos dokumentumot, amely nyújt további részletek és a mutatók [here][virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions])

#### <a name="sql-server-log-shipping"></a>Az SQL Server szállítási naplózása
Magas elérhetőség (HA) módszereinek egyik SQL Server napló szállítási. Ha a HA konfiguráció részt a VMs névfeloldás dolgozik, nem jelent problémát, és az Azure-ban a telepítő nem különböznek bármely beállítása, hogy a helyszíni befejeződött. Csak az IP-felbontás támaszkodhat nem javasolt. Ellenőrizze a dokumentáció illetően napló szállítási és a napló szállítási körül elvek beállítása:

<https://technet.microsoft.com/library/ms187103.aspx>
 
Annak érdekében, hogy valóban elérése minden olyan magas elérhetőségét, egy kell telepíteni a VMs alá tartozó ilyen napló szállítási konfiguráció belül az azonos Azure elérhetőségének beállítása.

#### <a name="database-mirroring"></a>Adatbázis tükrözése
Adatbázis tükrözés SAP által támogatott, az SAP kapcsolati karakterláncban feladatátvevő partner létrehozása (lásd az SAP Megjegyzés [965908]) támaszkodik. Idegen helyszíni esetben feltételezzük, hogy a két VMs ugyanabban a tartományban vannak, hogy a felhasználói környezet a két SQL Server példánya fut a tartomány, valamint a felhasználók és jogosult a két érintett SQL Server-példányok. Emiatt az adatbázis-tükrözéshez Azure-ban a telepítő nem változat egymástól eltérő a tipikus helyszíni beállítás és konfiguráció.

Felhőben telepítésekről, kezdve a legegyszerűbb módszer, hogy van-e egy másik tartomány beállítása, hogy az adott adatbázis-kezelő VMs (és ideális dedikált SAP VMs) Azure egy tartományon belül.

Ha a tartomány nem lehetséges, egy is használhat a tanúsítványok az adatbázis-végpontok tükrözési az itt leírt módon: <https://technet.microsoft.com/library/ms191477.aspx>

Beállítási adatbázis-tükrözéshez Azure-ban való oktatóanyagot megtalálható itt: <https://technet.microsoft.com/library/ms189852.aspx> 

#### <a name="alwayson"></a>AlwaysOn
Támogatott AlwaysOn az SAP helyszíni (lásd az SAP Megjegyzés [1772688]), az SAP Azure-ban használható együtt támogatja. A FAKT, hogy Ön nem megosztott lemez létrehozhatnak Azure-ban nem jelenti azt, hogy egy nem hozható létre egy AlwaysOn Windows Server feladatátvevő fürt (WSFC) konfigurációs különböző VMs között. Csak azt jelenti, hogy nincs használni egy megosztott lemezen a fürt konfigurációs határozatképesség lehetőséget. Így Azure-ban egy AlwaysOn WSFC konfigurációs összeállítása és egyszerűen nem jelölje ki a kvórum típusa, amelyik a megosztott lemez. Az Azure környezetben e VMs telepítik az feloldása a VMs neve és a VMs kell kell ugyanabban a tartományban. Ez csak Azure és idegen helyszínen telepített igaz. Néhány szempontot körül telepíti az SQL Server elérhetőség csoport figyelő (szabad összekeverni a Azure elérhetőségének beállítása nem), mivel az Azure ezen a ponton a egyszerre nem teszi lehetővé a egyszerűen hozzon létre egy Active Directory és a DNS-objektumot, mivel az esetleges helyszíni. Ezért néhány más telepítési lépéseket szükségesek, amelyek az adott Azure működését.

Az elérhetőség csoport figyelő használatával megfontolandó a következők:

* Az elérhetőség csoport figyelő használata csak Windows Server 2012-ben vagy Windows Server 2012 R2 lehetséges a virtuális az operációs rendszer vendégként. A Windows Server 2012 frissítenie kell győződjön meg arról, hogy van-e hozzárendelve a javítást: <https://support.microsoft.com/kb/2854082> 
* A Windows Server 2008 R2 nem létezik a javítást és AlwaysOn kimutatásadatokat a kapcsolatok karakterláncban feladatátvevő partner megadásával használható az adatbázis-tükrözéshez megegyező módon (lásd az SAP default.pfl paraméter adatbázisok/mss/kiszolgálón keresztül – kész az SAP Megjegyzés [965908]).
* Az elérhetőség csoport figyelő használatakor az adatbázis VMs kell csatlakoznia kell egy dedikált terheléselosztó. Az SAP rendszer (alkalmazáskiszolgálók, az adatbázis-kezelő kiszolgáló, valamint (A) SCS) minden VMs virtuális ugyanabba a hálózatba, vagy lenne szükség egy SAP alkalmazási réteg a a karbantartás etc\host fájl ahhoz, hogy a rendszer az SQL Server VMs virtuális azoknak a névfeloldás a felhőben környezetekben vagy kell rendelkeznie. Elkerülése érdekében, hogy Azure van-e olyan esetekben, ahol mindkét VMs mellékesen leállítás új IP-címek hozzárendelése, egy kell hozzárendelése statikus IP-címek a hálózati kapcsolatok lévők VMs az AlwaysOn beállítás (a statikus IP-cím megadása ismertetett [a] [ virtual-networks-reserved-private-ip] cikk) [Megjegyzés]: <> (régi blogok) [Megjegyzés]: (<https://blogs.msdn.com/b/alwaysonpro/archive/2014/08/29/recommendations-and-best-practices-when-deploying-sql-server-alwayson-availability-groups-in-windows-azure-iaas.aspx> <> <https://blogs.technet.com/b/rmilne/archive/2015/07/27/how-to-set-static-ip-on-azure-vm.aspx>) 
* Vannak olyan speciális szükséges lépésekről amikor felépíteni a WSFC fürt konfigurálása, ahol van szüksége a fürt az egy speciális IP-címet, mivel a jelenlegi működést az Azure volna rendeljen a fürt nevének ugyanazt a címet a csomópontra a fürt jön létre. Ez azt jelenti, hogy egy kézi lépést végre kell hajtani hozzárendelése egy másik IP-címet a fürthöz.
* Az elérhetőség csoport figyelő fog Azure-ban vannak rendelve a VMs fut az elérhetőség csoport elsődleges és másodlagos kópiák TCP/IP végpontok jönnek létre.
* Előfordulhat, hogy ezeket a végpontokat, a hozzáférés-vezérlési listák biztonságos szükséges.

[Megjegyzés]: <>  (Teendők a régi blog)
[Megjegyzés]: <>  (A lépések részletes leírását és Azure-AlwaysOn konfigurációjának telepítésének szükségleti cikkek tapasztalt legjobban a Ha útmutató alapján az oktatóprogram elérhető [here][virtual-machines-windows-classic-ps-sql-alwayson-availability-groups])
[Megjegyzés]: <>  (Előre AlwaysOn beállítási keresztül az Azure gyűjtemény < https://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx>)
[Megjegyzés]: <>  (Az elérhetőség csoport figyelő létrehozása, akkor legjobb leírtak [this][virtual-machines-windows-classic-ps-sql-int-listener] oktatóprogram)
[Megjegyzés]: <>  (Biztonságossá tétele hálózati végpontokat a hozzáférés-vezérlési listák vannak magyarázata legjobb itt:)
[Megjegyzés]: <>  (* < https://michaelwasham.com/windows-azure-powershell-reference-guide/network-access-control-list-capability-in-windows-azure-powershell/>)
[Megjegyzés]: <>  (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/08/31/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-1-of-2.aspx>)
[Megjegyzés]: <>  (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/01/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-2-of-2.aspx>)  
[Megjegyzés]: <>  (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/18/creating-acls-for-windows-azure-endpoints.aspx>) 

Egy SQL Server AlwaysOn rendelkezésre állási csoport üzembe fölé, valamint a különböző Azure régiókban lehetőség. Ez a funkció fog kihasználhatja az Azure VNet Vnet kapcsolatot ([További részleteket][virtual-networks-configure-vnet-to-vnet-connection]).
[Megjegyzés]: [Megjegyzés]<> (teendők régi blogjába): <> (SQL Server AlwaysOn rendelkezésre állási csoportok beállításainak egy ilyen esetben az alábbiakban ismertetjük: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.) 

#### <a name="summary-on-sql-server-high-availability-in-azure"></a>Az SQL Server magas elérhetőség az Azure összefoglalása
Tekintve, hogy az Azure tároló védi az a tartalmat, egy kisebb oka van egy készenléti kép ragaszkodjanak. Ez azt jelenti, hogy magas elérhetősége igényektől kell csak vírusvédelmet az alábbi esetekben:

* A virtuális elérhetetlenségét miatt a kiszolgáló fürt Azure vagy más oka lehet karbantartásokat egészének mérete
* Az SQL Server-példányt szoftver problémák
* Ha adatok törlése és helyreállítási pont és az idő van szükség kézi hiba szembeni védelmével kapcsolatban:

Egy is is, hogy az első két esetben is hatálya alá adatbázis-tükrözéshez vagy AlwaysOn, mivel a harmadik helyzet csak is hatálya alá napló-szállítási egyező technológiák megjeleníti.

Szüksége lesz a bonyolultabb AlwaysOn, az adatbázis-tükrözéshez összehasonlítva AlwaysOn előnyei egyenleg. E előnyei is szerepelnek, például:

*   Olvasható másodlagos kópiák.
*   Másodlagos kópiák mentést.
*   Jobb méretezhetősége.
*   Több, mint egy másodlagos kópiák.

### <a name="9053f720-6f3b-4483-904d-15dc54141e30"></a>Általános SQL Server Azure összefoglaló SAP
Ebből az útmutatóból sok javaslatok, és azt javasoljuk, elolvasni többször az Azure bevezetését tervezi előtt. Általánosságban elmondható bár, ne felejtse el a felső tíz általános az adatbázis-kezelő Azure bizonyos pontjait kövesse:

[Megjegyzés]: <> Mi mint átviteli  (magasabb 2.3? Egy virtuális-nál?)
1. Használja a legújabb az adatbázis-kezelő verzió, például SQL Server 2014-es, hogy előnyökkel jár a legtöbb Azure-ban. Az SQL Server Ez az SQL Server 2012 SP1 CU4, amely tartalmazza a mögöttes Azure tárterület az alábbi funkciót. Azonban SAP együtt szeretné ajánljuk legalább az SQL Server 2014-es SP1 CU1 vagy SQL Server 2012 SP2, és a legújabb Kivágása.
1. Gondosan megtervezése az SAP rendszer fekvő egyenleg adatok fájl elrendezésének és Azure korlátozások az Azure-ban:
    * Nem túl sok VHD van, de van-e elegendő érheti el a szükséges IOPS biztosítása érdekében.
    * Ne feledje, hogy IOPS is korlátozódik per Azure tárterület-fiókjába, és a tárterület-fiókok belül minden Azure előfizetés korlátozódik ([További részleteket][azure-subscription-service-limits]). 
    * Csak a különböző VHD, ha módosítani szeretné egy nagyobb teljesítmény elérése csíkok.
1. Soha ne az olyan szoftverek telepítése, vagy bármely fájlokat nem állandó és a meghajtó semmit sem a Windows újra kell indítani a elvesznek a D:\ meghajtón adatmegőrzési igénylő helyezni.
1. Ne használja a szabványos tároló Azure gyorsítótárazás Azure virtuális.
1. Ne használjon Azure geo replikált tároló fiókok.  Az adatbázis-kezelő munkaterhelésekből használata a helyi meghajtóra felesleges.
1. Az adatbázis-kezelő szállító HA/DR megoldás való replikáció adatbázis-adatok használata
1. Mindig névfeloldás használja, és nem IP-címek támaszkodhat.
1. A lehetséges legnagyobb adatbázis tömörítést használja. Az SQL Server Ez az oldal tömörítést.
1. Ügyeljen arra, hogy az SQL Server képek használatáról a Microsoft Azure piactéren lévő. Ha az SQL Server egyik, módosítania kell a példány rendezési sorrend olyan SAP NetWeaver rendszerekhez telepítése előtt.
1. Telepítése és konfigurálása az SAP Host nyomon követése az Azure [telepítési útmutató] [telepítési útmutató] leírt módon.

## <a name="specifics-to-sap-ase-on-windows"></a>Az SAP ASE Windows adatai
Microsoft Azure kezdve, egyszerűen áttelepítheti a meglévő SAP ASE-alkalmazások Azure virtuális gépeken futó. SAP ASE virtuális gépen lehetővé teszi, hogy a teljes költség telepítés, kezelése és a vállalati közösségének alkalmazások karbantartás tulajdonjogának csökkentése ezeket az alkalmazásokat, hogy a Microsoft Azure egyszerűen áttelepítésével. Az SAP ASE az Azure virtuális gép Rendszergazdák és a felhasználók továbbra is használhatók lesznek az ugyanazon fejlesztés és felügyeleti eszközök, amelyek a rendelkezésre álló helyszíni.

Van egy SZOLGÁLTATÁSISZINT-az Azure virtuális gépeken futó itt találja: <https://azure.microsoft.com/support/legal/sla>

Jelenleg benne, hogy a Microsoft Azure szolgáltatott virtuális gépeken futó más nyilvános felhő virtualizációs szeretne rendelni, de egyes eredményekre alkalmazott összehasonlítva nagyon jól végrehajtása változhat. A különböző SAP SAP átméretezésre SAP számait tanúsítvánnyal virtuális termékváltozatok biztosítja az egy külön SAP Megjegyzés [1928533].

Kimutatások és javaslatok a használatát tekintetében Azure tároló, az SAP-telepítés VMs vagy SAP figyelése alkalmazása SAP ASE telepítéseknél SAP-alkalmazások együtt az első négy a dokumentum fejezeteit egész jelzett módon.

### <a name="sap-ase-version-support"></a>SAP – ASE verzió támogatása 
SAP jelenleg támogatja az SAP ASE verzió 16,0 SAP Business csomagja termékekkel való használatra. SAP ASE kiszolgálón vagy JDBC és az ODBC-illesztőprogramok SAP Business csomagja-termékekkel használt összes frissítésének kizárólag az SAP szolgáltatás piactér címen keresztül is közöljük: <https://support.sap.com/swdc>.

Mint a berendezések a helyszíni nem töltse le a frissítések az SAP ASE server, illetve a JDBC és ODBC-illesztőprogramok közvetlenül Sybase-webhelyeket. Javítások, amely SAP Business csomagja termékeken történő helyszíni és az Azure virtuális gépeken futó használata támogatott részletes információkat talál a következő SAP megjegyzéseket:

* [1590719]
* [1973241]
 
SAP Business csomagja futó SAP ASE általános információkat az [Állapotváltozás](https://scn.sap.com/community/ase) találhatók.

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>SAP ASE konfigurációs útmutató SAP kapcsolódó Azure VMs SAP ASE telepítések

#### <a name="structure-of-the-sap-ase-deployment"></a>Az SAP ASE telepítési felépítése
Megfelelően az általános leírása, SAP ASE végrehajtható található, vagy el kell telepíteni a virtuális alap virtuális rendszermeghajtóra (c meghajtó\). Általában az SAP ASE rendszer és eszközök adatbázisok a legtöbb vannak nem igazán kapcsolatos károkozásra gyárilag SAP NetWeaver terhelést szerint. Ezért a rendszer és eszközök adatbázisok (fő, modell, saptools, sybmgmtdb, sybsystemdb), valamint a C:\drive is marad. 

Kivételt sikerült az ideiglenes adatbázis tartalmazó összes munka táblázatok és ideiglenes SAP ASE, amely bizonyos SAP ERP és az összes Sávszélesség-munkaterhelésekből előfordulhat, hogy vagy magasabb szintű adatok vagy I/O műveletek kötet, amelyek nem illeszkednek a eredeti virtuális alap virtuális által létrehozott (c meghajtó\).
 
Attól függően, hogy a SAPInst/SWPM használta a rendszer verziója az adatbázis tartalmazhatja:

* Egy egyetlen SAP ASE tempdb, amely SAP ASE telepítésekor jön létre
* Az SAP ASE tempdb SAP ASE és az SAP-telepítés gyakorlatának által létrehozott egy további saptempdb telepítésével létrehozott
* Az SAP ASE tempdb SAP ASE és egy további tempdb már létrehozott kézzel (pl. következő SAP Megjegyzés [1752266]) ERP/FF adott tempdb követelményeknek telepítésével létrehozott

Adott ERP vagy az összes Sávszélesség-munkaterhelésekből esetén célszerű, tekintetében teljesítmény elérése érdekében a továbbá létrehozott tempdb (SWPM vagy kézzel) a tempdb eszközök megtartása eltérő C: meghajtón\. Ha nincs további tempdb létezik, azt javasoljuk hozzon létre egy újat (SAP Megjegyzés [1752266]).

Az ilyen rendszerek az alábbi lépéseket kell elvégezni a továbbá létrehozott TempDB:

* Az első tempdb eszköz áthelyezése az első eszközön az SAP-adatbázis
* Minden egyes a VHD az SAP-adatbázis egy eszközöket tartalmazó tempdb eszközök hozzáadása

Ez a beállítás lehetővé teszi, hogy a tempdb vagy használhatnak nagyobb területet, mint rendszermeghajtóra rendelkezésére áll. Hivatkozásként egy meglévő rendszert futtató futtassa a helyszíni tempdb eszköz méretű ellenőrizheti. Vagy ilyen konfiguráció lehetővé teszik IOPS számok tempdb, amelyek nem tudja ellátni rendszermeghajtóra szemben. Ismét rendszert futtató a helyszíni Lync-I/O terhelést elleni tempdb használható.

Soha ne helyezze SAP ASE eszközöket alakzatot a D:\ meghajtóra, a virtuális. Ez is érinti a tempdb, akkor is, ha az objektumok tartani a tempdb csak ideiglenes.

#### <a name="impact-of-database-compression"></a>Adatbázis tömörítése hatását
Hol I/O sávszélesség előfordulhat, hogy a korlátozó tényező konfigurációk esetén minden mértéke, amely csökkenti a IOPS segíthet a terhelést a egyik futtatását is lehetővé teszi például az Azure-IaaS helyzetben egymástól. Így erősen ajánlott annak ellenőrzéséhez, hogy SAP ASE tömörítés használta SAP meglévő adatbázis feltöltése Azure.

Az ajánlási előtt Azure feltöltését, ha nem használható a tömörítési végrehajtásához kívül számos oka lehet számítható ki:

* Azure fel kell tölteni adatok mennyiségét alsó.
* A tömörítési végrehajtás időtartama rövidebb feltételezve, hogy több processzorok vagy magasabb I/O sávszélesség- vagy kisebb I/O késés a helyszíni használni tudja erősebb hardver
* Kisebb adatbázis mérete kisebb költségének lemez terhelés vezethet

A virtuális üzemeltetett az Azure virtuális gépeken futó, ahogyan ezt a helyszíni adatok és üzleti tömörítés munka. További információt a ellenőrzése, ha a tömörítési már használatban van egy meglévő SAP ASE adatbázis ellenőrizze SAP Megjegyzés [1750510].

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>DBACockpit használata a Lync-adatbázis-példányok
SAP rendszerekhez, amelyek adatbázis platform SAP ASE használ a DBACockpit érhető el a tranzakciók DBACockpit beágyazott böngészőablakot, vagy Webdynpro. Azonban a teljes figyelemmel kísérésére és az adatbázis felügyelete funkció használható csak a DBACockpit Webdynpro végrehajtásában.

A helyszíni rendszerek néhány lépést szükséges ahhoz, hogy a DBACockpit Webdynpro végrehajtásának által használt összes SAP NetWeaver funkció. Kövesse az SAP Megjegyzés [1245200] webdynpros a használatát engedélyezni, és szükség szerint a lehetőségekből létrehozásához. Ha a fenti jegyzetek utasításait követve is fog konfigurálni az internetes kommunikációs kezelője (icm) a http- és https-kapcsolatokat használható portok együtt. Az alapértelmezett HTTP néz ki:

> ICM/server_port_0 = PROT = HTTP, PORT 8000, PROCTIMEOUT = 600, IDŐTÚLLÉPÉS = = 600
>
> ICM/server_port_1 = PROT = HTTPS, PORT = 443$ $, PROCTIMEOUT 600, IDŐTÚLLÉPÉS = = 600

és a hivatkozások tranzakció generált DBACockpit fog kinézni alábbihoz hasonló:

> https://`<fullyqualifiedhostname`>: 44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: 8000/sap/bc/webdynpro/sap/dba_cockpit

Függően Ha, és hogyan keresztül webhelyek, csatlakozik az SAP rendszer szolgáltatója Azure virtuális gépen több webhelyen vagy készült ExpressRoute (határokon helyszíni telepítés) kell győződjön meg arról, hogy ICM használ egy teljesen minősített hostname (állomásnév) megoldható, hogy a számítógépen Ha akkor próbál megnyitni a DBACockpit a. Lásd: az SAP Megjegyzés [773830] megértéséhez, hogy hogyan ICM határozza meg, hogy a teljesen minősített állomásnév attól függően, hogy a profil paraméterek, és paraméter icm/host_name_full explicit módon beállítása, ha szükséges.

Ha a felhőben helyzetben nélkül határokon helyszíni összekapcsolását a helyszíni és Azure virtuális telepítette, adja meg a nyilvános IP-címet és egy domainlabel szeretne. A virtuális nyilvános DNS-neve formátumának majd így jelenik meg:

> `<custom domainlabel`>. `<azure region`>. cloudapp.azure.com

A tartománynév kapcsolódó további részletek találhatók [Itt][virtual-machines-azurerm-versus-azuresm].

Az SAP profil paraméter icm/host_name_full beállítást a Azure virtuális a hivatkozást a DNS-neve néz:

> https://mydomainlabel.westeurope.cloudapp.NET:44300/sap/bc/webdynpro/sap/dba_cockpit

> http://mydomainlabel.westeurope.cloudapp.NET:8000/sap/bc/webdynpro/sap/dba_cockpit

Ebben az esetben akkor győződjön meg róla, hogy:

* A bejövő szabályok hozzáadása az Azure-portálon ICM kommunikálhat portok számára a hálózati biztonsági csoport
* A bejövő szabályok hozzáadása a Windows tűzfal beállításait használatával kommunikál a ICM portok számára

Az automatikus az összes rendelkezésre álló javítások importált ajánlott rendszeresen alkalmazni a javítási gyűjtemény SAP az SAP-verziójára vonatkozó Megjegyzés::

* [1558958]
* [1619967]
* [1882376]

További információ a kereskedelmi vezérlőpultja SAP ASE a következő SAP jegyzetek találhatók:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496] 
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>SAP – ASE biztonsági mentése és helyreállítási megfontolandó szempontok
SAP ASE az Azure telepítésekor ellenőrizni kell a biztonsági másolat módszertan. Még ha a rendszer nem a hatékony munkát rendszert, az SAP-adatbázis SAP ASE által üzemeltetett kell lennie rendszeres biztonsági másolat készül. Azure tároló három képek továbbra is, mivel a biztonsági másolatot már kevésbé fontos e az összeomlást tároló végtermék kapcsolatban. Elsődleges megfelelő biztonsági mentési és visszaállítási terv karbantartásának oka az, többet is egyenlíti logikai/kézi hibákat, mert a pont, az idő helyreállítási lehetőségeket. Így a cél, vagy használata biztonsági mentés visszaállítása az adatbázis vissza egy bizonyos ponton idő vagy a biztonsági másolatok Azure-ban egy másik rendszer rendezi a meglévő adatbázis átmásolásával. Például meg továbbíthat a 2-réteg SAP-konfiguráció és a 3 irányítási rendszer beállítást az azonos rendszer által visszaállítása biztonsági másolatból.

Mentésével és visszaállításával Azure-ban adatbázis ugyanígy működik, ahogyan ezt a helyszíni. SAP – jegyzetek témakörökben találhat:

* [1588316]
* [1585981]

További információ: létrehozása kiírása konfigurációk és ütemezési biztonsági mentést. Attól függően, hogy a stratégia és beállíthatja igényeinek adatbázist, és jelentkezzen be kiírása, a meglévő VHD egyik alakzatot lemezre jelölőnégyzetből, vagy felveheti egy további virtuális a biztonsági másolat.  Csökkentse a hiba esetén az adatok elvesztését egy virtuális, ahol található nincs adatbázis-eszköz használata ajánlott.

Adatok és üzleti mellett tömörítés SAP ASE is kínál a biztonságimásolat-tömörítéssel. Az adatbázist, és jelentkezzen be kiírása a kisebb helyet foglalnak akkor célszerű használni a biztonságimásolat-tömörítéssel. Olvassa el az SAP Megjegyzés [1588316] további információt. A biztonsági másolat tömörítése elengedhetetlen is adatok átvitele, ha azt tervezi, töltse le az biztonsági mentés és biztonsági másolat kiírása a Azure virtuális gép a helyszíni tartalmazó VHD mennyiségének csökkentésére.

Ne használjon meghajtó D:\ adatbázis vagy napló kiírása célként.

#### <a name="performance-considerations-for-backupsrestores"></a>Biztonsági mentés és visszaállítás teljesítménybeli szempontok
Gépi telepítésekről, ahogy a biztonsági mentés és visszaállítás teljesítmény érték hány kötet párhuzamosan is olvashatók, és a mennyiségek átviteli lehet függ. Ezenkívül a biztonságimásolat-tömörítéssel által használt Processzor felhasználás előfordulhat, hogy jelentős szerepet VMs a az imént legfeljebb 8 Processzor beszélgetésekben. Ezért egyik felveheti:

* Az adatbázis eszközök tárolására szolgáló kevesebb a számát VHD olvasási teljes minél kisebb adatátvitel
* Minél kisebb Processzor számát a virtuális, a szigorúbb milyen következményekkel járnak a biztonságimásolat-tömörítéssel szálak
* Kevesebb célok (csíkok könyvtárak, VHD) írni a biztonsági mentés, a kisebb a teljesítmény

Két írni célok számát növelni Ez lehet attól függően, hogy igényeinek megfelelően használt/kombinált beállításait:

* A biztonsági másolat cél mennyiségi közti csíkozás több csatlakoztatott VHD fölé a IOPS kapacitása a sávos kötet javítása érdekében
* Írja be a határozza meg, hogy egynél több cél címtár használó SAP ASE szintjén kiírása konfiguráció létrehozása

Közti csíkozás kötet felett több csatlakoztatott VHD van már korábbi részében bemutatott tartalom. Az SAP ASE kiírása konfiguráció több könyvtárak használatáról további információt találhat a tárolt eljárás sp_config_dump a konfiguráció kiírása a [Sybase az Adatközpont](http://infocenter.sybase.com/help/index.jsp)létrehozásához használt dokumentációja.

### <a name="disaster-recovery-with-azure-vms"></a>Az Azure VMs vészhelyreállítás

#### <a name="data-replication-with-sap-sybase-replication-server"></a>Adatok replikációs SAP Sybase replikációs kiszolgálóval
Az SAP Sybase replikációs Server (SRS) SAP ASE biztosít aszinkron távoli helyre adhatja át az adatbázis-tranzakciók készenléti meleg megoldást. 

A telepítési és SRS működésének működik, valamint funkcionális egy virtuális az Azure virtuális gép szolgáltatásokban tárolt, ahogyan ezt a helyszíni.

SAP – replikációs kiszolgálón keresztül ASE HADR tervezett az elkövetkező kiadásokban. Fog azt vizsgálni, és a Microsoft Azure platformokon megjelenésével, amint érhető el.

## <a name="specifics-to-sap-ase-on-linux"></a>Az SAP ASE Linux adatai

Microsoft Azure kezdve, egyszerűen áttelepítheti a meglévő SAP ASE-alkalmazások Azure virtuális gépeken futó. SAP ASE virtuális gépen lehetővé teszi, hogy a teljes költség telepítés, kezelése és a vállalati szélessége alkalmazások karbantartás tulajdonjogának csökkentése ezeket az alkalmazásokat, hogy a Microsoft Azure könnyen áttelepítésével. Az SAP ASE az Azure virtuális gép Rendszergazdák és a felhasználók továbbra is használhatók lesznek az ugyanazon fejlesztés és felügyeleti eszközök, amelyek a rendelkezésre álló helyszíni.

Azure VMs üzembe helyezése fontos tudnivalók a hivatalos SLA, amely itt: <https://azure.microsoft.com/support/legal/sla>

SAP méretezése információk és SAP virtuális termékváltozatok tanúsítvánnyal listájának SAP Megjegyzés [1928533]nyújtanak. Azure virtuális gépeken futó dokumentumok méretezése további SAP megtalálható itt <http://blogs.msdn.com/b/saponsqlserver/archive/2015/06/19/how-to-size-sap-systems-running-on-azure-vms.aspx> , és itt <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

Kimutatások és javaslatok a használatát tekintetében Azure tároló, az SAP-telepítés VMs vagy SAP figyelése alkalmazása SAP ASE telepítéseknél SAP-alkalmazások együtt az első négy a dokumentum fejezeteit egész jelzett módon.

A következő két SAP megjegyzések Linux és ASE ASE általános információt a felhőben tartalmazza:

* [2134316]
* [1941500]

### <a name="sap-ase-version-support"></a>SAP – ASE verzió támogatása 
SAP jelenleg támogatja az SAP ASE verzió 16,0 SAP Business csomagja termékekkel való használatra. SAP ASE kiszolgálón vagy JDBC és az ODBC-illesztőprogramok SAP Business csomagja-termékekkel használt összes frissítésének kizárólag az SAP szolgáltatás piactér címen keresztül is közöljük: <https://support.sap.com/swdc>.

Mint a berendezések a helyszíni nem töltse le a frissítések az SAP ASE server, illetve a JDBC és ODBC-illesztőprogramok közvetlenül Sybase-webhelyeket. Javítások, amely SAP Business csomagja termékeken történő helyszíni és az Azure virtuális gépeken futó használata támogatott részletes információkat talál a következő SAP megjegyzéseket:

* [1590719]
* [1973241]
 
SAP Business csomagja futó SAP ASE általános információkat az [Állapotváltozás](https://scn.sap.com/community/ase) találhatók.

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>SAP ASE konfigurációs útmutató SAP kapcsolódó Azure VMs SAP ASE telepítések

#### <a name="structure-of-the-sap-ase-deployment"></a>Az SAP ASE telepítési felépítése
Az általános leírása, megfelelően SAP ASE végrehajtható kell található vagy telepítve van a legfelső szintű fájl rendszerbe a virtuális gép (/sybase). Általában az SAP ASE rendszer és eszközök adatbázisok a legtöbb vannak nem igazán kapcsolatos károkozásra gyárilag SAP NetWeaver terhelést szerint. Ezért a rendszer és eszközök adatbázisok (fő, modell, saptools, sybmgmtdb, sybsystemdb), valamint a legfelső szintű fájlrendszerben marad. 

Kivételt sikerült tartalmazó összes munka táblázatok és ideiglenes SAP ASE, amely bizonyos SAP ERP és az összes Sávszélesség-munkaterhelésekből előfordulhat, hogy vagy magasabb szintű adatok vagy I/O műveletek kötet, amelyek nem illeszkednek a eredeti virtuális OS lemez által létrehozott ideiglenes adatbázis.
 
Attól függően, hogy a SAPInst/SWPM használta a rendszer verziója az adatbázis tartalmazhatja:

* Egy egyetlen SAP ASE tempdb, amely SAP ASE telepítésekor jön létre
* Az SAP ASE tempdb SAP ASE és az SAP-telepítés gyakorlatának által létrehozott egy további saptempdb telepítésével létrehozott
* Az SAP ASE tempdb SAP ASE és egy további tempdb már létrehozott kézzel (pl. következő SAP Megjegyzés [1752266]) ERP/FF adott tempdb követelményeknek telepítésével létrehozott

Adott ERP vagy az összes Sávszélesség-munkaterhelésekből esetén célszerű, tekintetében teljesítmény elérése érdekében a továbbá létrehozott tempdb (SWPM vagy kézzel) a tempdb eszközök tartani egy külön fájlba rendszerben, mely lemezen egyetlen Azure adatok vagy több Azure adatok lemezről Linux RAID jelöli meg. Ha nincs további tempdb létezik, akkor célszerű hozzon létre egy újat (SAP Megjegyzés [1752266]).

Az ilyen rendszerek az alábbi lépéseket kell elvégezni a továbbá létrehozott TempDB:

* Az első tempdb könyvtár áthelyezése az első fájlrendszer az SAP-adatbázis
* A fájlrendszer az SAP-adatbázis tartalmazó VHD mindegyike tempdb könyvtárak hozzáadása

Ez a beállítás lehetővé teszi, hogy a tempdb vagy használhatnak nagyobb területet, mint rendszermeghajtóra rendelkezésére áll. Hivatkozásként egy meglévő rendszert futtató futtassa a helyszíni tempdb címtár méretű ellenőrizheti. Vagy ilyen konfiguráció lehetővé teszik IOPS számok tempdb, amelyek nem tudja ellátni rendszermeghajtóra szemben. Ismét rendszert futtató a helyszíni Lync-I/O terhelést ellen tempdb használható.

Soha ne helyezze SAP ASE könyvtárak /mnt vagy /mnt/resource a virtuális az alakzatot. Ez is érinti a tempdb, akkor is, ha az objektumok tartani a tempdb csak ideiglenes mivel /mnt vagy /mnt/resource alapértelmezett Azure virtuális temp szóközt amely nem állandó. [Ebben] a cikkben található többet szeretne tudni az Azure virtuális temp térköz[virtual-machines-linux-how-to-attach-disk]

#### <a name="impact-of-database-compression"></a>Adatbázis tömörítése hatását
Hol I/O sávszélesség előfordulhat, hogy a korlátozó tényező konfigurációk esetén minden mértéke, amely csökkenti a IOPS segíthet a terhelést a egyik futtatását is lehetővé teszi például az Azure-IaaS helyzetben egymástól. Így erősen ajánlott annak ellenőrzéséhez, hogy SAP ASE tömörítés használta SAP meglévő adatbázis feltöltése Azure.

Az ajánlási előtt Azure feltöltését, ha nem használható a tömörítési végrehajtásához kívül számos oka lehet számítható ki:

* Azure fel kell tölteni adatok mennyiségét alsó.
* A tömörítési végrehajtás időtartama rövidebb feltételezve, hogy több processzorok vagy magasabb I/O sávszélesség- vagy kisebb I/O késés a helyszíni használni tudja erősebb hardver
* Kisebb adatbázis mérete kisebb költségének lemez terhelés vezethet

A virtuális üzemeltetett az Azure virtuális gépeken futó, ahogyan ezt a helyszíni adatok és üzleti tömörítés munka. További információt a ellenőrzése, ha a tömörítési már használatban van egy meglévő SAP ASE adatbázis ellenőrizze SAP Megjegyzés [1750510]. SAP – Megjegyzés [2121797] adatbázis tömörítése kapcsolatban további információt is megjelenik.

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>DBACockpit használata a Lync-adatbázis-példányok
SAP rendszerekhez, amelyek adatbázis platform SAP ASE használ a DBACockpit érhető el a tranzakciók DBACockpit beágyazott böngészőablakot, vagy Webdynpro. Azonban a teljes figyelemmel kísérésére és az adatbázis felügyelete funkció használható csak a DBACockpit Webdynpro végrehajtásában.

A helyszíni rendszerek néhány lépést szükséges ahhoz, hogy a DBACockpit Webdynpro végrehajtásának által használt összes SAP NetWeaver funkció. Kövesse az SAP Megjegyzés [1245200] webdynpros a használatát engedélyezni, és szükség szerint a lehetőségekből létrehozásához. Ha a fenti jegyzetek utasításait követve is fog konfigurálni az internetes kommunikációs kezelője (icm) a http- és https-kapcsolatokat használható portok együtt. Az alapértelmezett HTTP néz ki:

> ICM/server_port_0 = PROT = HTTP, PORT 8000, PROCTIMEOUT = 600, IDŐTÚLLÉPÉS = = 600
>
> ICM/server_port_1 = PROT = HTTPS, PORT = 443$ $, PROCTIMEOUT 600, IDŐTÚLLÉPÉS = = 600

és a hivatkozások tranzakció generált DBACockpit fog kinézni alábbihoz hasonló:

> https://`<fullyqualifiedhostname`>: 44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: 8000/sap/bc/webdynpro/sap/dba_cockpit

Függően Ha, és hogyan keresztül webhelyek, csatlakozik az SAP rendszer szolgáltatója Azure virtuális gépen több webhelyen vagy készült ExpressRoute (határokon helyszíni telepítés) kell győződjön meg arról, hogy ICM használ egy teljesen minősített hostname (állomásnév) megoldható, hogy a számítógépen Ha akkor próbál megnyitni a DBACockpit a. Lásd: az SAP Megjegyzés [773830] megértéséhez, hogy hogyan ICM határozza meg, hogy a teljesen minősített állomásnév attól függően, hogy a profil paraméterek, és paraméter icm/host_name_full explicit módon beállítása, ha szükséges.

Ha a felhőben helyzetben nélkül határokon helyszíni összekapcsolását a helyszíni és Azure virtuális telepítette, adja meg a nyilvános IP-címet és egy domainlabel szeretne. A virtuális nyilvános DNS-neve formátumának majd így jelenik meg:

> `<custom domainlabel`>. `<azure region`>. cloudapp.azure.com

A tartománynév kapcsolódó további részletek találhatók [Itt][virtual-machines-azurerm-versus-azuresm].

Az SAP profil paraméter icm/host_name_full beállítást a Azure virtuális a hivatkozást a DNS-neve néz:

> https://mydomainlabel.westeurope.cloudapp.NET:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://mydomainlabel.westeurope.cloudapp.NET:8000/sap/bc/webdynpro/sap/dba_cockpit

Ebben az esetben akkor győződjön meg róla, hogy:

* A bejövő szabályok hozzáadása az Azure-portálon ICM kommunikálhat portok számára a hálózati biztonsági csoport
* A bejövő szabályok hozzáadása a Windows tűzfal beállításait használatával kommunikál a ICM portok számára

Az automatikus az összes rendelkezésre álló javítások importált ajánlott rendszeresen alkalmazni a javítási gyűjtemény SAP az SAP-verziójára vonatkozó Megjegyzés::

* [1558958]
* [1619967]
* [1882376]

További információ a kereskedelmi vezérlőpultja SAP ASE a következő SAP jegyzetek találhatók:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496] 
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>SAP – ASE biztonsági mentése és helyreállítási megfontolandó szempontok
SAP ASE az Azure telepítésekor ellenőrizni kell a biztonsági másolat módszertan. Még ha a rendszer nem a hatékony munkát rendszert, az SAP-adatbázis SAP ASE által üzemeltetett kell lennie rendszeres biztonsági másolat készül. Azure tároló három képek továbbra is, mivel a biztonsági másolatot már kevésbé fontos e az összeomlást tároló végtermék kapcsolatban. Elsődleges megfelelő biztonsági mentési és visszaállítási terv karbantartásának oka az, többet is egyenlíti logikai/kézi hibákat, mert a pont, az idő helyreállítási lehetőségeket. Így a cél, vagy használata biztonsági mentés visszaállítása az adatbázis vissza egy bizonyos ponton idő vagy a biztonsági másolatok Azure-ban egy másik rendszer rendezi a meglévő adatbázis átmásolásával. Például meg továbbíthat a 2-réteg SAP-konfiguráció és a 3 irányítási rendszer beállítást az azonos rendszer által visszaállítása biztonsági másolatból.

Mentésével és visszaállításával Azure-ban adatbázis ugyanígy működik, ahogyan ezt a helyszíni. SAP – jegyzetek témakörökben találhat:

* [1588316]
* [1585981]

További információ: létrehozása kiírása konfigurációk és ütemezési biztonsági mentést. Attól függően, hogy a stratégia és beállíthatja igényeinek adatbázist, és jelentkezzen be kiírása, a meglévő VHD egyik alakzatot lemezre jelölőnégyzetből, vagy felveheti egy további virtuális a biztonsági másolat.  Csökkentse a hiba esetén az adatok elvesztését akkor célszerű használni egy virtuális, ahol nincs adatbázis címtár/fájl található.

Adatok és üzleti mellett tömörítés SAP ASE is kínál a biztonságimásolat-tömörítéssel. Az adatbázist, és jelentkezzen be kiírása a kisebb helyet foglalnak akkor célszerű használni a biztonságimásolat-tömörítéssel. Olvassa el az SAP Megjegyzés [1588316] további információt. A biztonsági másolat tömörítése elengedhetetlen is adatok átvitele, ha azt tervezi, töltse le az biztonsági mentés és biztonsági másolat kiírása a Azure virtuális gép a helyszíni tartalmazó VHD mennyiségének csökkentésére.

Ne használja az Azure virtuális temp terület /mnt vagy /mnt/resource adatbázis vagy napló kiírása célként.

#### <a name="performance-considerations-for-backupsrestores"></a>Biztonsági mentés és visszaállítás teljesítménybeli szempontok
Gépi telepítésekről, ahogy a biztonsági mentés és visszaállítás teljesítmény érték hány kötet párhuzamosan is olvashatók, és a mennyiségek átviteli lehet függ. Ezenkívül a biztonságimásolat-tömörítéssel által használt Processzor felhasználás előfordulhat, hogy jelentős szerepet VMs a az imént legfeljebb 8 Processzor beszélgetésekben. Ezért egyik felveheti:

* Az adatbázis eszközök tárolására szolgáló kevesebb a számát VHD olvasási teljes minél kisebb adatátvitel
* Minél kisebb Processzor számát a virtuális, a szigorúbb milyen következményekkel járnak a biztonságimásolat-tömörítéssel szálak
* Írni a biztonsági mentés, a kisebb az átviteli kevesebb célok (Linux szoftveres RAID VHD)

Két írni célok számát növelni Ez lehet attól függően, hogy igényeinek megfelelően használt/kombinált beállításait:

* A biztonsági másolat cél mennyiségi közti csíkozás több csatlakoztatott VHD fölé a IOPS kapacitása a sávos kötet javítása érdekében
* Írja be a határozza meg, hogy egynél több cél címtár használó SAP ASE szintjén kiírása konfiguráció létrehozása

Közti csíkozás kötet felett több csatlakoztatott VHD van már korábbi részében bemutatott tartalom. Az SAP ASE kiírása konfiguráció több könyvtárak használatáról további információt találhat a tárolt eljárás sp_config_dump a konfiguráció kiírása a [Sybase az Adatközpont](http://infocenter.sybase.com/help/index.jsp)létrehozásához használt dokumentációja.

### <a name="disaster-recovery-with-azure-vms"></a>Az Azure VMs vészhelyreállítás

#### <a name="data-replication-with-sap-sybase-replication-server"></a>Adatok replikációs SAP Sybase replikációs kiszolgálóval
Az SAP Sybase replikációs Server (SRS) SAP ASE biztosít aszinkron távoli helyre adhatja át az adatbázis-tranzakciók készenléti meleg megoldást. 

A telepítési és SRS működésének működik, valamint funkcionális egy virtuális az Azure virtuális gép szolgáltatásokban tárolt, ahogyan ezt a helyszíni.

Ezen a ponton a egyszerre ASE HADR SAP replikációs kiszolgálón keresztül nem támogatott. Előfordulhat, hogy azt vizsgálni, és a jövőben jelent meg a Microsoft Azure platformokon.

## <a name="specifics-to-oracle-database-on-windows"></a>Az Oracle-adatbázishoz a Windows adatai
Oracle szoftver óta midyear 2013, a Microsoft Windows Hyper-V és Azure futtatásához Oracle által támogatott. Ez a cikk további részleteket az általános támogatja a Windows Hyper-V és a Azure olvassa el az Oracle: <https://blogs.oracle.com/cloud/entry/oracle_and_microsoft_join_forces> 

Az általános támogatás az adott forgatókönyvet az SAP-alkalmazások feljebb helyezése az Oracle-adatbázisok van támogatja a következő is. Részletek az ebben a dokumentum egy részéhez neve.

### <a name="oracle-version-support"></a>Az Oracle-verzió támogatása
A következő SAP Megjegyzés [2039619] található összes és részletes tudnivalókat az Oracle-verziók megfelelő, amelynek támogatnak SAP futó Oracle meg Azure virtuális gépeken futó operációs rendszer verziója

Általános információkat az SAP Business csomagja futó Oracle Állapotváltozás találhatók: <https://scn.sap.com/community/oracle>

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Az Oracle konfigurálási útmutatója Azure VMs az SAP-telepítések

#### <a name="storage-configuration"></a>Tárterület konfigurálása
Csak egyetlen példányt az Oracle NTFS formátumú lemez használata támogatott. A virtuális lemezt alapján NTFS fájlrendszerben kell tárolni, az adatbázisfájlokat. Ezek a VHD az Azure virtuális gép csatlakoztatva van, vagy Azure lap BLOB-tárolóhoz (<https://msdn.microsoft.com/library/azure/ee691964.aspx>) alapján. Bármilyen típusú hálózati meghajtók vagy távoli megosztások Azure fájl szolgáltatások hasonlóan:
 
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx> 
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-Files.aspx>
 
az Oracle-adatbázis-fájlok **nem** támogatottak!

Azure VHD, oldal Azure BLOB-tárolóhoz alapján használ, a nyilatkozatok ehhez a dokumentumhoz a fejezet [gyorsítótárazás VMs és VHD] [adatbázis-kezelő – útmutató-2.1] és [Microsoft Azure tároló] [adatbázis-kezelő – útmutató-2.3] alkalmazhat, valamint az Oracle-adatbázishoz való telepítések.

Leírtak korábbi verziójában a dokumentum az általános rész, IOPS átviteli Azure rendszerindításának vonatkozó kvóták létezik. A pontos kvótákat virtuális típusától függően használják. A kvótákat tartalmazó listát virtuális megtalálható [Itt][virtual-machines-sizes]

A támogatott Azure virtuális típusok azonosítása, olvassa el SAP Megjegyzés [1928533]

Mindaddig, amíg az aktuális IOPS kvóta egy lemezen megfelel, lehetőség a egy egyetlen csatlakoztatott Azure virtuális DB tárolásához. 

Ha további IOPS szükség, erősen ajánlott ablak tárterület-készletek (csak a Windows Server 2012-ben elérhető vagy újabb) vagy a Windows csíkozást Windows 2008 R2 használatával hozhat létre egy nagy logikai eszköz több csatlakoztatott virtuális lemezre. Lásd még: fejezet [szoftver RAID] [adatbázis-kezelő – útmutató-2.2], a dokumentum. Ezt a megközelítést egyszerűbbé teszi a felügyeleti terhelést a lemezterület kezelése és elkerülhetők a manuális elosztása fájlok több csatlakoztatott VHD munkamennyiség.

#### <a name="backup--restore"></a>Biztonsági mentése és visszaállítása
Biztonsági másolatának visszaállítása funkciót, az SAP BR / * eszközök az Oracle ugyanúgy használhatók a szabványos Windows kiszolgálói operációs rendszerek és a Hyper-V. Az Oracle-helyreállítás-kezelő (RMAN) is támogatott lemezre jelölőnégyzetből, és lemezről visszaállítása biztonsági másolatok.

#### <a name="high-availability"></a>Magas elérhetősége
[Megjegyzés]: <>  (a csatolás ASM hivatkozik.)
Az Oracle-adatok Guard magas rendelkezésre állásának és vészhelyreállításra támogatott. Részletek találhatók [a] [ virtual-machines-windows-classic-configure-oracle-data-guard] dokumentációt.

#### <a name="other"></a>Más
Minden más általános témakörök hivatkozásait Azure elérhetőségének beállítása vagy SAP felügyeletet igényel, például az első három telepítéseknél VMs az Oracle-adatbázishoz, valamint a dokumentum fejezeteit ismertetett módon vonatkoznak.

## <a name="specifics-for-the-sap-maxdb-database-on-windows"></a>Az SAP MaxDB adatbázis Windows adatai

### <a name="sap-maxdb-version-support"></a>SAP – MaxDB verzió támogatása
SAP jelenleg támogatja az SAP MaxDB verzió 7.9 SAP NetWeaver alapú termékek Azure-ban való használatra. SAP MaxDB kiszolgálón vagy JDBC és az ODBC-illesztőprogramok SAP NetWeaver-alapú termékekkel használt összes frissítésének kizárólag az SAP szolgáltatás piactér <https://support.sap.com/swdc>címen keresztül is közöljük.
Általános tudnivalók a SAP NetWeaver futó SAP MaxDB <https://scn.sap.com/community/maxdb>találhatók.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-maxdb-dbms"></a>Microsoft Windows-verziók és Azure virtuális támogatott SAP MaxDB az adatbázis-kezelő
A támogatott Microsoft Windows-verzió az SAP MaxDB az adatbázis-kezelő a Azure című szakaszban talál segítséget:

* [SAP – termék elérhetősége mátrix (PAM)][sap-pam]
* SAP – Megjegyzés [1928533]

Ajánlott az operációs rendszer Microsoft Windows, amely a Microsoft Windows 2012 R2 legújabb verzióját használja.

### <a name="available-sap-maxdb-documentation"></a>Rendelkezésre álló SAP MaxDB dokumentáció
SAP MaxDB dokumentáció frissített listája megtalálható a következő SAP Megjegyzés [767598]
    
### <a name="sap-maxdb-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP – MaxDB konfigurálási útmutatója Azure VMs az SAP-telepítések

#### <a name="b48cfe3b-48e9-4f5b-a783-1d29155bd573"></a>Tárterület konfigurálása
Azure tároló gyakorlati tanácsok a SAP MaxDB, hajtsa végre az általános ajánlások említett fejezet [RDBMS telepítés szerkezetének] [adatbázis-kezelő – útmutató a-2].

> [AZURE.IMPORTANT] Más adatbázisok, például SAP MaxDB szintén adatok és a naplófájlok megtekintése. Az SAP MaxDB terminológia a helyes kifejezés azonban "mennyiségi" (nem "fájl"). SAP MaxDB vannak például adaton és napló kötet. Ne tévessze OS lemez kötet együtt. 

Rövid kell:

* Az SAP MaxDB adatokat, és jelentkezzen be kötet (azaz fájlok) tároló a **Helyi felesleges tároló (LRS)** szerint megadott a fejezet [Microsoft Azure tároló] [adatbázis-kezelő – útmutató-2.3] Azure tároló fiókot beállítani.
* SAP MaxDB adaton (azaz fájlok) IO elérési útját elválasztja a napló kötet (azaz fájlok) IO elérési útját. Ez azt jelenti, hogy egy logikai meghajtón telepítésére van SAP MaxDB adaton (azaz fájlok), és SAP MaxDB napló kötet (azaz fájlok) van telepítve van a logikai egy másik meghajtóra.
* A megfelelő fájl gyorsítótárazás megadása minden Azure blob, attól függően, hogy használni SAP MaxDB adat- vagy kötet (azaz fájlokat), és hogy használja-e Azure normál vagy Azure prémium tárolására, [VMs gyorsítótárba helyezése] fejezet ismertetett módon [adatbázis-kezelő – útmutató-2.1].
* Mindaddig, amíg az aktuális IOPS kvóta egy lemezen megfelel, lehetőség a adaton tárolhatja a egyetlen csatlakoztatott Azure virtuális, és egy másik egyetlen csatlakoztatott Azure virtuális összes adatbázis napló kötet is tárolhatja.
* Ha további IOPS és/vagy a szóköz szükség, erősen ajánlott Microsoft ablak tárterület-készletek (csak a Microsoft Windows Server 2012-ben elérhető vagy újabb) vagy Microsoft Windows csíkozást a Microsoft Windows 2008 R2 használatával hozhat létre egy nagy logikai eszköz több csatlakoztatott virtuális lemezre. Lásd még: fejezet [szoftver RAID] [adatbázis-kezelő – útmutató-2.2], a dokumentum. Ezt a megközelítést egyszerűbbé teszi a felügyeleti terhelést a lemezterület kezelése és elkerülhetők a kézi terjesztésével fájlok több csatlakoztatott VHD végig a munkamennyiség.
* A legmagasabb IOPS követelményeket Azure prémium tároló követheti, amely és áll rendelkezésre DS-sorozat GS-sorozat VMs.

![Hivatkozás az SAP MaxDB az adatbázis-kezelő Azure IaaS virtuális konfigurálása][dbms-guide-figure-600]

#### <a name="23c78d3b-ca5a-4e72-8a24-645d141a3f5d"></a>Biztonsági mentési és visszaállítási
SAP – MaxDB az Azure telepítésekor esetén olvassa el a a biztonsági másolat módszertan. Még ha a rendszer nem a hatékony munkát rendszert, az SAP-adatbázis SAP MaxDB által üzemeltetett kell lennie rendszeres biztonsági másolat készül. Azure tárolási három képek továbbra is, mivel biztonsági ettől kezdve a rendszer tárolási hibája és a fontosabb működési vagy felügyeleti hibák elleni védelem értelmez kevésbé fontos. Az elsődleges egy megfelelő biztonsági mentés és visszaállítás terv karbantartásának oka az, hogy is egyenlíti logikai vagy manuális hibákat, mert a pont és az idő-helyreállítási lehetőségeket. Így a cél, vagy használja az biztonsági mentés visszaállítása az adatbázis egy bizonyos ponton idő vagy a biztonsági másolatok Azure-ban egy másik rendszer rendezi a meglévő adatbázis másolásával. Például meg továbbíthat a 2-réteg SAP-konfiguráció és a 3 irányítási rendszer beállítást az azonos rendszer által visszaállítása biztonsági másolatból.

Mentésével és visszaállításával Azure-ban adatbázis ugyanígy működik, ahogyan ezt a helyszíni rendszerekhez, hogy használni tudja a szabványos SAP MaxDB biztonsági mentési és visszaállítási eszközök SAP Megjegyzés [767598]szerepel, az SAP MaxDB dokumentáció dokumentumok egyikét bemutatott. 

#### <a name="77cd2fbb-307e-4cbf-a65f-745553f72d2c"></a>Biztonsági mentési és visszaállítási teljesítménybeli szempontok
Gépi telepítésekről, ahogy a biztonsági mentési és visszaállítási teljesítmény a párhuzamos és a mennyiségek kapacitásának hány kötet olvasható függ. Ezenkívül a biztonságimásolat-tömörítéssel által használt Processzor felhasználás játszhatók le jelentős szerepet a VMs, akár 8 Processzor szál. Ezért egyik felveheti:

* A kevesebb számát az adatbázis-eszközök, az alsó a teljes olvasási adatátvitel tárolására szolgáló VHD
* Minél kisebb Processzor számát a virtuális, a szigorúbb milyen következményekkel járnak a biztonságimásolat-tömörítéssel szálak
* A kevesebb célok (csíkok könyvtárak, VHD) írni a biztonsági mentés, az alsó a teljesítmény

Ha növelni szeretné írni a cél számát, létezik a is használhatja, esetleg kombináció igényeitől függően két lehetőség közül választhat:

* Biztonsági másolat külön kötet hogy
* A biztonsági másolat cél mennyiségi felett több csatlakoztatott VHD annak érdekében, hogy a kötet csíkos a IOPS átviteli közti csíkozás
* Ha problémákat külön dedikált logikai lemez eszközöket:
    * SAP – MaxDB biztonsági kötet (azaz fájlok)
    * SAP – MaxDB adaton (azaz fájlok)
    * SAP – MaxDB napló kötet (azaz fájlok)

Közti csíkozás kötet felett több csatlakoztatott VHD van már korábbi részében bemutatott fejezet [szoftver RAID] [adatbázis-kezelő – útmutató-2.2], a dokumentum. 

#### <a name="f77c1436-9ad8-44fb-a331-8671342de818"></a>Más
Minden más általános témakörök hivatkozásait Azure elérhetőségének beállítása vagy SAP felügyeletet igényel, például a az első három SAP MaxDB adatbázis-kezelő VMs telepítéseknél a dokumentum fejezeteit ismertetett módon is vonatkoznak.
Egyéb SAP MaxDB nyelvspecifikus beállítások átlátszóak Azure VMs, és az SAP Megjegyzés [767598] és a következő SAP-jegyzetek felsorolt különböző dokumentumok ismertetett:

* [826037] 
* [1139904]
* [1173395]

## <a name="specifics-for-sap-livecache-on-windows"></a>Az SAP liveCache Windows adatai

### <a name="sap-livecache-version-support"></a>SAP – liveCache verzió támogatása
Támogatott az Azure virtuális gépeken futó SAP liveCache minimális verziója **SAP LC/LCAPPS 10.0 SP 25** , beleértve a **liveCache 7.9.08.31** és **LCA-összeállítás 25**, megjelenik az **EhP** 2 az SAP SCM 7.0-s vagy újabb.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-livecache-dbms"></a>Microsoft Windows-verziók és Azure virtuális támogatott SAP liveCache az adatbázis-kezelő
A támogatott Microsoft Windows-verzió az SAP liveCache a Azure című szakaszban talál segítséget:

* [SAP – termék elérhetősége mátrix (PAM)][sap-pam]
* SAP – Megjegyzés [1928533]

Ajánlott az operációs rendszer Microsoft Windows, amely a Microsoft Windows 2012 R2 legújabb verzióját használja. 

### <a name="sap-livecache-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP – liveCache konfigurálási útmutatója Azure VMs az SAP-telepítések

#### <a name="recommended-azure-vm-types"></a>Ajánlott Azure virtuális típusai
SAP – liveCache van olyan alkalmazás, amely a nagyon nagy számításokat, az összeg és a RAM és a Processzor sebességét egy SAP liveCache teljesítmény fő hatással van. 

Az SAP (SAP Megjegyzés [1928533]) által támogatott Azure virtuális felsorolt összes virtuális Processzor feladatoknak a virtuális is szeretne biztonsági másolatot dedikált a hipervizor fizikai Processzor erőforrások által. Nincs overprovisioning (és ezért nincs versenyhelyzetből Processzor erőforrások) történik.

Minden Azure virtuális példány típust támogatják az SAP, hasonlóan a virtuális memória is 100 %-os megfeleltetve a fizikai memória – például overprovisioning (túlzott lekötési), nem használatos.

Ebből a perspektívából ajánlott használni az új adatsort D vagy DS-számozási (Azure prémium tároló együtt) Azure virtuális típus, mint 60 %-kal gyorsabb processzorok, mint A-sorozat rendelkeznek. A legnagyobb RAM és Processzor betöltés használhatja G-sorozat és GS-számozási (Azure prémium tárolási együtt) VMs legújabb®-Xeon® Intel processzor E5 v3 termékcsaládban kétszer a memória és négyszer egyszínű állam meghajtó tárolására (SSD) a D/DS-sorozat.

#### <a name="storage-configuration"></a>Tárterület konfigurálása
SAP liveCache SAP MaxDB technológia alapul, mint az Azure tároló ajánlott gyakorlat javaslatok említett az SAP MaxDB fejezet [tárolási konfigurációt] [adatbázis-kezelő – útmutató-8.4.1] SAP liveCache is érvényesek. 

#### <a name="dedicated-azure-vm-for-livecache"></a>A liveCache dedikált Azure virtuális
SAP – liveCache a számítási power intenzíven használ, hatékonyabban használatát az ajánlott egy dedikált Azure virtuális számítógépre telepítése. 
 
![Azure virtuális dedikált hatékonyabban használatieset-liveCache számára][dbms-guide-figure-700]

#### <a name="backup-and-restore"></a>Biztonsági mentési és visszaállítási
Biztonsági mentési és visszaállítási, beleértve a teljesítménnyel kapcsolatos szempontok, már a a megfelelő SAP MaxDB fejezetekben [biztonsági mentési és visszaállítási] [adatbázis-kezelő – útmutató-8.4.2] és [biztonsági mentése és visszaállítása a teljesítménnyel kapcsolatos szempontok] [adatbázis-kezelő – útmutató-8.4.3]. 

#### <a name="other"></a>Más
A megfelelő SAP MaxDB [a]-[adatbázis-kezelő – útmutató-8.4.4] fejezet már megkötésekről más általános témakörök hivatkozásait. 

## <a name="specifics-for-the-sap-content-server-on-windows"></a>A tartalom SAP-kiszolgálóhoz, a Windows adatai
Az SAP-tartalmat kiszolgáló összetevő külön, a kiszolgáló-alapú elektronikus dokumentumok például tartalmak tárolását különböző formátumokban. Az SAP-tartalom kiszolgálóval technológia fejlesztésének által biztosított, és bármely SAP-alkalmazások használt határokon alkalmazása legyen. Telepítés külön rendszeren. Tipikus tartalom pedig tananyag Tudásbázis raktári vagy a mySAP PLM dokumentumkezelő rendszer származó műszaki rajzokat dokumentációt. 

### <a name="sap-content-server-version-support"></a>SAP – tartalom kiszolgálói verzió támogatása
SAP – jelenleg támogatja:

* **SAP-kiszolgálóval** verzió **6.50 (vagy újabb)**
* **SAP – MaxDB 7.9 verziója**
* **A Microsoft IIS (Internet Information Server) verzió 8.0 (vagy újabb)**

Ajánlott az SAP tartalom kiszolgáló, amely a dokumentum írása idején **6.50-es verzió SP4**, legújabb verziója és a **Microsoft IIS 8.5**legújabb verzióját használja. 

Jelölje be a legújabb verziók SAP-kiszolgálóval, és az [SAP termék elérhetőség mátrix (PAM)]Microsoft IIS[sap-pam].

### <a name="supported-microsoft-windows-and-azure-vm-types-for-sap-content-server"></a>SAP-kiszolgálóval támogatott Microsoft Windowshoz és Azure virtuális típusai
Verzióját a Windows Azure SAP tartalom kiszolgáló, olvassa el:

* [SAP – termék elérhetősége mátrix (PAM)][sap-pam]
* SAP – Megjegyzés [1928533]

Ajánlott a Microsoft Windows, amely írás ezt a dokumentumot egyszerre **A Windows Server 2012 R2**legújabb verzióját szeretné használni.

### <a name="sap-content-server-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP – Content Server konfigurálási útmutatója Azure VMs az SAP-telepítések

#### <a name="storage-configuration"></a>Tárterület konfigurálása 
Konfigurálja az SAP tartalom kiszolgáló tárolja a fájlokat az SAP MaxDB adatbázisban, ha minden Azure tároló ajánlott eljárások a ajánlási említett az SAP MaxDB fejezet [tárolási konfigurációt] [adatbázis-kezelő – útmutató-8.4.1] is érvényes, az SAP-kiszolgálóval forgatókönyvet. 

Tartalom SAP-kiszolgáló tárolja a fájlokat a fájlrendszerben az adja meg, ha egy dedikált logikai meghajtó használata ajánlott. Tárhelyek biztosítanak is növelheti a logikai lemez méretét és a IOPS átviteli, [szoftver RAID] [adatbázis-kezelő – útmutató-2.2] fejezet ismertetett módon. 

#### <a name="sap-content-server-location"></a>SAP-tartalom kiszolgáló helyére
SAP-kiszolgálóval rendelkezik a ugyanazon Azure régió és Azure VNET, ha telepíti az SAP rendszer telepíthető. Szabadon döntse el, hogy szeretné-e egy dedikált Azure virtuális vagy a azonos virtuális a SAP rendszer fut, ahol az SAP-kiszolgálóval-összetevők telepítése. 
 
![Az SAP-kiszolgálóval dedikált Azure virtuális][dbms-guide-figure-800]

#### <a name="sap-cache-server-location"></a>SAP – gyorsítótár-kiszolgáló helyét
Az SAP-gyorsítótár kiszolgálóval összetevő-további kiszolgálóalapú helyileg (gyorsítótárazott) dokumentumokhoz való hozzáférést biztosít. Az SAP-gyorsítótár kiszolgálóval gyorsítótárát tartalom SAP-kiszolgáló a dokumentumokat. Ez a hálózati forgalmának engedélyezésére optimalizálása, ha többször beolvasása más helyekre kell dokumentumokat. Az általános szabály, hogy az SAP-gyorsítótár kiszolgálóval közelébe az ügyfél, az SAP-gyorsítótár kiszolgálóval hozzáférő fizikailag lesz. 

Az alábbi két lehetőség áll rendelkezésére:

1. **Ügyfél rendszer kódmentes SAP** Ha egy kódmentes SAP rendszer elérése az SAP-kiszolgálóval van konfigurálva, a SAP rendszer ügyfél. Azonos Azure terület – ugyanabban a Azure adatközpontban – SAP rendszer és az SAP-kiszolgálóval egyaránt meg van telepítve, azok fizikailag közelébe egymást. Emiatt nem szeretné, hogy egy dedikált SAP gyorsítótár-kiszolgáló nincs szükség. SAP felhasználói felület ügyfelek (SAP grafikus vagy böngészőt) az SAP rendszer közvetlenül elérhető, és az SAP rendszer dokumentumok beolvassa az SAP-tartalom kiszolgálóval.
1. **Ügyfél egy helyszíni webböngészőben** Az SAP-tartalom kiszolgálóval beállítható úgy, hogy közvetlenül a böngészőben elérhető. Ebben az esetben a webböngészőben fut, a helyszíni – az SAP-tartalom kiszolgáló ügyfél. A helyszíni adatközponthoz és Azure adatközponthoz (ideális esetben közelébe egymást) más fizikai helyre kerülnek. A helyszíni adatközponthoz Azure Azure-webhely VPN vagy készült ExpressRoute keresztül csatlakozik. Habár mindkét kérdésre biztonságos VPN hálózati kapcsolat Azure ajánlja fel, webhelyre a hálózati kapcsolat nem ajánlja fel a hálózati sávszélesség és a késés SLA a helyszíni adatközponthoz és az Azure adatközponthoz között. Dokumentumok hozzáférés gyorsítása végezheti el az alábbiak egyikét:
    1. SAP-gyorsítótár-kiszolgáló helyszíni telepítésének jelenik meg, zárja be, hogy a helyszíni böngészőben ([this][dbms-guide-900-sap-cache-server-on-premises] ábra lehetőség)
    1. Állítsa be a készült Azure ExpressRoute, amely a helyszíni adatközponthoz és Azure adatközponthoz között nagy sebességű és a kis-késés dedikált hálózati kapcsolaton.
 
![SAP-gyorsítótár-kiszolgáló helyszíni telepítéséhez beállítás][dbms-guide-figure-900]
<a name="642f746c-e4d4-489d-bf63-73e80177a0a8"></a>

#### <a name="backup--restore"></a>Biztonsági mentése és visszaállítása
Ha úgy állítja be az SAP-tartalom kiszolgálóval az SAP MaxDB adatbázis fájlok tárolására, a biztonsági mentés és visszaállítás eljárás és teljesítménybeli szempontok már az SAP MaxDB fejezet [biztonsági mentési és visszaállítási] [adatbázis-kezelő – útmutató-8.4.2] és fejezet [biztonsági mentése és visszaállítása a teljesítménnyel kapcsolatos szempontok] [adatbázis-kezelő – útmutató-8.4.3]. 

Ha úgy állítja be az SAP-tartalom kiszolgálóval a fájlrendszerben található fájlok tárolására, egy javasoljuk, hogy hajtsa végre a kézi biztonsági mentése és visszaállítása a teljes fájlok struktúra hol találhatók a dokumentumokat. SAP MaxDB biztonsági mentési és visszaállítási hasonló, javasoljuk, hogy egy dedikált lemez mennyiségi biztonsági célra. 

#### <a name="other"></a>Más
Más SAP-kiszolgálóval speciális beállításokkal átlátszóak Azure VMs, és különböző dokumentumok és az SAP-jegyzetek ismertetjük:

* <https://Service.SAP.com/contentserver> 
* SAP – Megjegyzés [1619726]  

## <a name="specifics-to-ibm-db2-for-luw-on-windows"></a>A Windows LUW az IBM DB2 adatai
A Microsoft Azure egyszerűen áttelepítheti a meglévő SAP alkalmazást IBM DB2 Linux rendszerhez, a UNIX és a Windows (LUW) az Azure virtuális gépeken futó. Az IBM DB2-LUW SAP a rendszergazdák és a felhasználók továbbra is használhatók lesznek az ugyanazon fejlesztés és felügyeleti eszközök, amelyek érhető el a helyszíni.
SAP Business csomagja futó IBM DB2-LUW általános információt a az SAP közösségi hálózati (Állapotváltozás) a <https://scn.sap.com/community/db2-for-linux-unix-windows>találhatók.

További információk és az Azure a LUW DB2 SAP frissítéseket című témakörben talál SAP Megjegyzés [2233094]. 

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 Linux rendszerhez, a UNIX és a Windows-verzió támogatása
A Microsoft Azure virtuális gép Services LUW az IBM DB2 SAP kezdve DB2 verzió 10.5 használata támogatott.

Támogatott SAP-termékek és Azure virtuális típusú információt olvassa el SAP Megjegyzés [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 Linux, UNIX és konfigurációs útmutató a Windows Azure VMs az SAP-telepítések

#### <a name="storage-configuration"></a>Tárterület konfigurálása
A virtuális lemezt alapján NTFS fájlrendszerben kell tárolni, az adatbázisfájlokat. Ezeket a VHD az Azure virtuális gép csatlakoztatva van, és az Azure lap BLOB-tárolóhoz (<https://msdn.microsoft.com/library/azure/ee691964.aspx>) alapul. Hálózati meghajtók vagy távoli megosztások, például a következő Azure fájl szolgáltatások **nem** támogatja az adatbázisfájlok, bármilyen típusú: 

* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-Files.aspx>
 
Azure VHD, oldal Azure BLOB-tárolóhoz alapján használatakor nyilatkozatok dokumentumtartalom fejezet [RDBMS telepítés szerkezetének] [adatbázis-kezelő – útmutató a-2] is az adatbázis LUW az IBM DB2 telepítések vonatkoznak. 

Leírtak korábbi verziójában a dokumentum az általános rész, IOPS átviteli Azure rendszerindításának vonatkozó kvóták létezik. A pontos kvóták attól függenek, hogy a használt virtuális típust. A kvótákat tartalmazó listát virtuális megtalálható [Itt][virtual-machines-sizes].

Mindaddig, amíg az aktuális IOPS kvóta egy lemezen elegendő, lehetőség a egy egyetlen csatlakoztatott Azure virtuális adatbázis tárolásához. 

A teljesítmény elérése érdekében szempontokat is érdemes fejezet "Adatok biztonsági és teljesítménybeli szempontok az adatbázis könyvtárak" az SAP telepítési útmutatók.

Azt is megteheti segítségével Windows tárterület-készletek (csak a Windows Server 2012-ben elérhető vagy újabb) vagy a Windows csíkozást Windows 2008 R2 fejezet [szoftver RAID] [adatbázis-kezelő – útmutató-2.2], a dokumentum ismertetett módon hozhat létre egy nagy logikai eszköz több csatlakoztatott virtuális lemezre.
A lemezt a sapdata és saptmp könyvtárak DB2-tároló elérési útvonalát tartalmazó meg kell adnia egy fizikai lemez szektor méretének 512 KB. Windows tároló készletek használata esetén létre kell hoznia a tárterület-készletek manuálisan a paraméter használatával parancssor keresztül "-LogicalSectorSizeDefault". <Https://technet.microsoft.com/library/hh848689.aspx>témakörben talál.

#### <a name="backuprestore"></a>Biztonsági mentési és visszaállítási
Az IBM DB2-LUW biztonsági mentési és visszaállítási funkciókat támogatja a ugyanúgy, mint a szokásos Windows kiszolgálói operációs rendszerek és a Hyper-V.

Győződjön meg arról, hogy egy érvényes adatbázis biztonsági stratégia már a helyükön. 

Gépi telepítésekről, ahogy biztonsági mentési és visszaállítási teljesítmény függ, hogy hány kötet párhuzamosan is olvashatók, és a mennyiségek átviteli lehet. Ezenkívül a biztonságimásolat-tömörítéssel által használt Processzor felhasználás előfordulhat, hogy jelentős szerepet VMs a az imént legfeljebb 8 Processzor beszélgetésekben. Ezért egyik felveheti:

* Az adatbázis eszközök tárolására szolgáló kevesebb a számát VHD olvasási teljes minél kisebb adatátvitel
* Minél kisebb Processzor számát a virtuális, a szigorúbb milyen következményekkel járnak a biztonságimásolat-tömörítéssel szálak
* A kevesebb célok (csíkok könyvtárak, VHD) írni a biztonsági mentés, az alsó a teljesítmény

Ha növelni szeretné írni a cél számát, két lehetőség közül választhat lehet attól függően, hogy igényeinek megfelelően használt/kombinált:

* A biztonsági másolat cél mennyiségi közti csíkozás több csatlakoztatott VHD fölé a IOPS kapacitása a sávos kötet javítása érdekében
* Írja be a biztonsági mentés egynél több cél címtár használatával

#### <a name="high-availability-and-disaster-recovery"></a>Magas rendelkezésre állásának és a helyreállítás
Microsoft fürt Server (MSCS) nem támogatott.

Támogatott DB2 magas elérhetősége vészhelyreállítás (HADR). Ha a HA konfiguráció virtuális gépeken futó névfeloldás dolgozik, a telepítő Azure-ban nem különböznek bármely beállítása, hogy a helyszíni befejeződött. Csak az IP-felbontás támaszkodhat nem javasolt.

Ne használjon Azure áruházból Geo-replikáció. További információért olvassa el az fejezet [Microsoft Azure tárolási] [adatbázis-kezelő – útmutató-2.3] és [magas rendelkezésre állásának és a helyreállítás az Azure VMs] fejezet [adatbázis-kezelő-útmutató-3].

#### <a name="other"></a>Más
Minden más általános témakörök hivatkozásait Azure elérhetőségének beállítása vagy SAP felügyeletet igényel, például az első három, valamint az IBM DB2-LUW VMs telepítéseknél a dokumentum fejezeteit ismertetett módon vonatkoznak. 

Is érdemes fejezet [általános SQL Server Azure összefoglaló SAP-] [adatbázis-kezelő – útmutató-5.8].

## <a name="specifics-to-ibm-db2-for-luw-on-linux"></a>Az IBM DB2-LUW Linux használatát részletek
A Microsoft Azure egyszerűen áttelepítheti a meglévő SAP alkalmazást IBM DB2 Linux rendszerhez, a UNIX és a Windows (LUW) az Azure virtuális gépeken futó. Az IBM DB2-LUW SAP a rendszergazdák és a felhasználók továbbra is használhatók lesznek az ugyanazon fejlesztés és felügyeleti eszközök, amelyek érhető el a helyszíni. SAP Business csomagja futó IBM DB2-LUW általános információt a az SAP közösségi hálózati (Állapotváltozás) a <https://scn.sap.com/community/db2-for-linux-unix-windows>találhatók.

További információk és az Azure a LUW DB2 SAP frissítéseket című témakörben talál SAP Megjegyzés [2233094].

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 Linux rendszerhez, a UNIX és a Windows-verzió támogatása
A Microsoft Azure virtuális gép Services LUW az IBM DB2 SAP kezdve DB2 verzió 10.5 használata támogatott.

Támogatott SAP-termékek és Azure virtuális típusú információt olvassa el SAP Megjegyzés [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 Linux, UNIX és konfigurációs útmutató a Windows Azure VMs az SAP-telepítések

#### <a name="storage-configuration"></a>Tárterület konfigurálása
Az összes adatbázisfájlok virtuális lemezt alapján fájlrendszerben kell tárolni. Ezeket a VHD az Azure virtuális gép csatlakoztatva van, és az Azure lap BLOB-tárolóhoz (<https://msdn.microsoft.com/library/azure/ee691964.aspx>) alapul.
Hálózati meghajtók vagy távoli megosztások, például a következő Azure fájl szolgáltatások **nem** támogatja az adatbázisfájlok, bármilyen típusú:

* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-Files.aspx>

Azure VHD, oldal Azure BLOB-tárolóhoz alapján használatakor nyilatkozatok ehhez a dokumentumhoz a fejezet [RDBMS telepítés szerkezetének] [adatbázis-kezelő – útmutató a-2] is alkalmazása és az IBM DB2 LUW adatbázis telepítések.

Leírtak korábbi verziójában a dokumentum az általános rész, IOPS átviteli Azure rendszerindításának vonatkozó kvóták létezik. A pontos kvóták attól függenek, hogy a használt virtuális típust. A kvótákat tartalmazó listát virtuális megtalálható [Itt][virtual-machines-sizes].

Mindaddig, amíg az aktuális IOPS kvóta egy lemezen elegendő, lehetőség a egy egyetlen csatlakoztatott Azure virtuális adatbázis tárolásához.

A teljesítmény elérése érdekében szempontokat is érdemes fejezet "Adatok biztonsági és teljesítménybeli szempontok az adatbázis könyvtárak" az SAP telepítési útmutatók.

Azt is megteheti segítségével LVM (logikai mennyiségi kezelő) vagy MDADM fejezet [szoftver RAID] [adatbázis-kezelő – útmutató-2.2], a dokumentum ismertetett módon hozhat létre egy nagy logikai eszköz több csatlakoztatott virtuális lemezre.
A lemezt a sapdata és saptmp könyvtárak DB2-tároló elérési útvonalát tartalmazó meg kell adnia egy fizikai lemez szektor méretének 512 KB.

#### <a name="backuprestore"></a>Biztonsági mentési és visszaállítási
Az IBM DB2-LUW biztonsági mentési és visszaállítási funkciókat támogatja a ugyanúgy, mint a szokásos Linux telepítésével helyszíni.

Győződjön meg arról, hogy egy érvényes adatbázis biztonsági stratégia már a helyükön.

Gépi telepítésekről, ahogy biztonsági mentési és visszaállítási teljesítmény függ, hogy hány kötet párhuzamosan is olvashatók, és a mennyiségek átviteli lehet. Ezenkívül a biztonságimásolat-tömörítéssel által használt Processzor felhasználás előfordulhat, hogy jelentős szerepet VMs a az imént legfeljebb 8 Processzor beszélgetésekben. Ezért egyik felveheti:

* Az adatbázis eszközök tárolására szolgáló kevesebb a számát VHD olvasási teljes minél kisebb adatátvitel
* Minél kisebb Processzor számát a virtuális, a szigorúbb milyen következményekkel járnak a biztonságimásolat-tömörítéssel szálak
* A kevesebb célok (csíkok könyvtárak, VHD) írni a biztonsági mentés, az alsó a teljesítmény

Ha növelni szeretné írni a cél számát, két lehetőség közül választhat lehet attól függően, hogy igényeinek megfelelően használt/kombinált:

* A biztonsági másolat cél mennyiségi közti csíkozás több csatlakoztatott VHD fölé a IOPS kapacitása a sávos kötet javítása érdekében
* Írja be a biztonsági mentés egynél több cél címtár használatával

#### <a name="high-availability-and-disaster-recovery"></a>Magas rendelkezésre állásának és a helyreállítás
Támogatott DB2 magas elérhetőség vészhelyreállítás (HADR). Ha a HA konfiguráció virtuális gépeken futó névfeloldás dolgozik, a telepítő Azure-ban nem különböznek bármely beállítása, hogy a helyszíni befejeződött. Csak az IP-felbontás támaszkodhat nem javasolt.

Ne használjon Azure áruházból Geo-replikáció. További információért olvassa el az fejezet [Microsoft Azure tároló] [adatbázis-kezelő – útmutató-2.3] és [magas rendelkezésre állásának és a helyreállítás az Azure VMs] fejezet [adatbázis-kezelő-útmutató-3].

#### <a name="other"></a>Más
Minden más általános témakörök hivatkozásait Azure elérhetőségének beállítása vagy SAP felügyeletet igényel, például az első három, valamint az IBM DB2-LUW VMs telepítéseknél a dokumentum fejezeteit ismertetett módon vonatkoznak.

Is érdemes fejezet [általános SQL Server Azure összefoglaló SAP-] [adatbázis-kezelő – útmutató-5.8].
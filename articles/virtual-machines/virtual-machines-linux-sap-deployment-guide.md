<properties
   pageTitle="SAP – NetWeaver Linux virtuális gépeken (VMs) – telepítési útmutató |} Microsoft Azure"
   description="SAP – NetWeaver Linux virtuális gépeken (VMs) – telepítési útmutató"
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

# <a name="sap-netweaver-on-azure-virtual-machines-vms--deployment-guide"></a>SAP – NetWeaver Azure virtuális gépeken (VMs) – telepítési útmutató

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
[resource-group-overview]:../azure-resource-manager/resource-group-overview.md
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
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-ps-create.md
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

Microsoft Azure lehetővé teszi a vállalatok szerezheti be a számítási és tárolási erőforrásokat minimális időben hosszadalmas beszerzési ciklus nélkül. Azure virtuális gépeken futó lehetővé teszi, hogy a vállalatok klasszikus alkalmazások, telepítése, mint SAP NetWeaver-alapú alkalmazások az Azure és bővítése a megbízhatóság és elérhetőség további erőforrások érhető el a helyszíni nélkül. Microsoft Azure támogatja az idegen helyszíni kapcsolat, amely lehetővé teszi a vállalatok, akik tevékenyen Azure virtuális gépeken futó integrálása a helyszíni tartományok a saját felhőket és az SAP rendszer fekvő is.

Ez a tanulmány lépésről lépésre ismerteti, hogyan készült Azure virtuális gép az SAP NetWeaver alapú alkalmazások telepítését. Azt feltételezi, hogy minden információ megtalálható a [tervezés és implementálási útmutatójában] [tervezési – útmutató] ismert. Ha nem a megfelelő dokumentumot először kell olvasni.

A papír kiegészíti az SAP-dokumentáció és az SAP platformokon adni az elsődleges erőforrások telepítések és az SAP-szoftver telepítések képviselik.

[AZURE.INCLUDE [windows-warning](../../includes/virtual-machines-linux-sap-warning.md)]

## <a name="introduction"></a>– Bevezetés
Cégek világszerte sok használata – leginkább kiemelten az SAP Business csomagot – SAP NetWeaver alapú alkalmazások futtatásához a feladatát kritikus üzleti folyamatok. A rendszerállapot ezért a kulcsfontosságú eszköz, és az azt jelenti, hogy a vállalati támogatása abban az esetben, ha a meghibásodása teljesítmény események, például olyan elengedhetetlen követelmény válik.
Microsoft Azure itt kiváló platform műszerezettségi az összes üzleti kritikus alkalmazások támogatási lehetőségek követelményeinek igazodik. Ez az útmutató azt ellenőrzi, célzott telepítési SAP szoftver van konfigurálva, hogy az enterprise-támogatás is ajánlja fel, függetlenül attól, amely a virtuális gép módon jön létre, a Microsoft Azure virtuális gép kell venni a Microsoft Azure piactéren ki azt, vagy egy ügyfél adott kép felhasználásával.
A következő, minden szükséges beállítás a lépések részletes témakörben olvashat.

## <a name="prerequisites-and-resources"></a>Előfeltételek és erőforrások:
### <a name="prerequisites"></a>Előfeltételek
Mielőtt nekikezdene, ellenőrizze, hogy teljesülnek-e a Előfeltételek, amelyek a következő fejezetekben leírtak.

#### <a name="local-personal-computer"></a>Helyi személyi számítógépen
A beállítás egy Azure virtuális gép az SAP szoftvertelepítést több lépésből magába foglalja. Windows VMs vagy Linux VMs felügyeletéhez kell egy PowerShell-parancsprogramot, és a Microsoft Azure-portálon. Az, hogy a Windows 7-es vagy újabb operációs rendszert futtató helyi személyi számítógépen szükség. Ha csak szeretne Linux VMs kezeléséhez, és ezt a feladatot egy Linux számítógépre használni szeretne, az Azure parancs sor kezelőfelületről Azure is használhatja.

#### <a name="internet-connection"></a>Internetkapcsolat
Töltse le, és hajtsa végre a szükséges eszközök és parancsfájlok, internetkapcsolatra szükség. A Microsoft Azure virtuális gépen fut az Azure fokozott figyelése bővítmény ezenkívül van szüksége az Internet-hozzáféréssel. Abban az esetben, ha a Azure virtuális az Azure virtuális hálózati vagy a helyszíni tartomány része, győződjön meg arról, hogy a megfelelő proxybeállítások fejezet [Proxy beállítása] ismertetett módon[ deployment-guide-configure-proxy] a dokumentumban.

#### <a name="microsoft-azure-subscription"></a>Microsoft Azure-előfizetés
Azure-fiók már létezik, és ismert függően a bejelentkezési adatokat.

#### <a name="topology-consideration-and-networking"></a>Szempontokat topológiája és a hálózati
A topológiája és az SAP-telepítés Azure-ban architektúra kell definiálhatók. Architektúra szerint:

* Microsoft Azure tároló fiókok használandó
* Virtuális hálózat be az SAP rendszer üzembe helyezése
* Erőforráscsoport be az SAP rendszer üzembe helyezése
* Azure-terület az SAP rendszer üzembe helyezése
* SAP-konfiguráció (2-réteg vagy 3 szintű)
* Virtuális size(s) és a VM(s) csatlakoztatása további VHD száma
* SAP – átviteli és javítása a Rendszerkonfiguráció

Azure tárolási fiókok és Azure virtuális hálózatok ilyenként kell létrehozott és beállított már. Hogyan hozhat létre, és állítsa be őket a [tervezés és implementálási útmutatójában] foglalt [tervezési – útmutató].

#### <a name="sap-sizing"></a>SAP – átméretezése
* A tervezett SAP-terhelés meghatározása, például az SAP Quicksizer használatával, és az SAP függően szám ismert 
* Ismerni kell az SAP rendszer szükséges Processzor erőforrás- és fogyasztása
* Ismerni kell a szükséges perifériaműveletek / szekundum
* Ismert a szükséges sávszélességet az esetleges kommunikációját különböző VMs Azure-ban
* A szükséges sávszélességet között a helyszíni eszközök és az Azure rendszerbe az SAP rendszerekhez ismert

#### <a name="resource-groups"></a>Erőforrás-csoportok
Erőforrás-csoportok egy új fogalom, amelyeknél az azonos életciklus pl. őket létre, és törölni egyszerre rendelkező erőforrások tartoznak. [Ebből] a cikkből[ resource-group-overview] erőforráscsoport további információt. 

### <a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>SAP – források
A konfiguráció munka közben az alábbi forrásokban van szükség:

* SAP – Megjegyzés [1928533]
    * a lista Azure virtuális gép különböző méretű támogatottak az SAP-szoftver telepítése 
    * fontos kapacitás információ / Azure virtuális gép mérete
    * támogatott SAP-szoftverek és -OS és DB kombinációja
* SAP – Megjegyzés [2015553] bejegyzésére előfeltételei SAP által támogatott Microsoft Azure az SAP-szoftver telepítésekor.
* SAP – Megjegyzés [1999351] fokozott Azure nyomon követése az SAP további hibaelhárítási információkat tartalmazó.
* SAP – Megjegyzés [2178632] részletes információkat az összes rendelkezésre álló felügyeleti mérési módja miatt az SAP, a Microsoft Azure tartalmazó. 
* SAP – Megjegyzés [1409604] tartalmazó a szükséges SAP-Host ügynök verzió Windows, a Microsoft Azure, kattintson az új Azure erőforrás-kezelő telepítésekor.
* SAP – Megjegyzés [2191498] tartalmazó a szükséges SAP-Host ügynök verziót a Microsoft Azure Linux meg az új Azure erőforrás-kezelő telepítésekor.
* SAP – Megjegyzés [2243692] az SAP Linux Azure a licencelésre vonatkozó adatokat tartalmazó
* Megjegyzés: [1984787] SUSE LINUX vállalati kiszolgáló 12 kapcsolatos általános tudnivalókat tartalmazó SAP
* Megjegyzés: [2002167] Red Hat Enterprise Linux általános információkat tartalmazó SAP 7.x
* [Állapotváltozás](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) , amely tartalmazza az összes szükséges SAP jegyzetek Linux
* SAP – adott PowerShell-parancsmagok az [Azure PowerShell] részét képező[azure-ps]
* SAP – adott Azure CLI az [Azure CLI] részét képező[azure-cli]
* [Microsoft Azure-portál][azure-portal]

[comment]: <> (SAP-Host ügynök az SAP Megjegyzés 1409604 MSSedusch teendő hozzáadása ARM javítás szintje)
 
Az alábbi útmutatókat a témát, amely a Microsoft Azure SAP, valamint terjed ki:

* [SAP NetWeaver Azure virtuális gépeken (VMs) – tervezése és üzembe helyezési útmutatóban] [tervezési – útmutató]
* [SAP NetWeaver Azure virtuális gépeken (VMs) – telepítési útmutató (a dokumentum)] [telepítési útmutató]
* [SAP NetWeaver Azure virtuális gépeken (VMs) – az adatbázis-kezelő telepítési útmutató] [adatbázis-kezelő – útmutató]

## <a name="b3253ee3-d63b-4d74-a49b-185e76c4088e"></a>Az SAP, a Microsoft Azure VMs telepítési forgatókönyve
Ez a fejezet, különböző módjairól az telepítési és az egyes telepítési típus egyetlen lépéseit.

### <a name="deployment-of-vms-for-sap"></a>Az SAP VMs telepítése
Microsoft Azure VMs és a kapcsolódó lemez telepítése több lehetőséget is kínál. Egyúttal nagyon fontos, hogy megérteni a különbségeket, mivel előkészületet, a VMs függő telepítési módja eltérő lehet. Általánosságban elmondható hogy vizsgálja meg a az alábbi esetekben:

#### <a name="deploying-a-vm-out-of-the-azure-marketplace"></a>A Microsoft Azure piactéren ki a virtuális üzembe helyezése
Szeretné, hogy a virtuális üzembe Microsoft vagy 3 résztvevős, feltéve, kép ki a Microsoft Azure piactéren. A Microsoft Azure virtuális gép telepítette, akkor kövesse az azonos útmutatások és eszközök belül a virtuális az SAP olyan szoftverek telepítése, mint a helyszíni környezetben. Az a Azure virtuális belül az SAP szoftvert telepít, SAP és a Microsoft javasoljuk feltöltéséhez és tárolni az SAP telepítési adathordozóját Azure VHD vagy létrehozása az Azure virtuális működik, mint "fájlkiszolgálóra" az összes a szükséges SAP telepítési adathordozóját tartalmazó.

[comment]: <> (Miért szükség egy fájlkezelés, például a fájlkiszolgálóra vagy a virtuális javasoljuk MSSedusch teendő? Eltér, így a helyszíni?)

További részletek: fejezet [forgatókönyv 1: üzembe helyezése a virtuális ki az Azure piactéren elérhető az SAP] [telepítési – útmutató a-3,2].

#### <a name="3688666f-281f-425b-a312-a77e7db2dfab"></a>Az egyéni képével egy virtuális üzembe helyezése
Adott javítás szükségletek szerint az operációs rendszer és az adatbázis-kezelő verziója a megadott képek ki a Microsoft Azure piactéren előfordulhat, hogy nem felel meg az igényeinek. Ezért előfordulhat, hogy létrehozásához szükséges egy saját "private" OS/DB virtuális kép, amelyeket később többször telepíthető segítségével virtuális.
A saját képét lépéseket eltérnek a Windows- és Linux képként.

___

> ![A Windows][Logo_Windows] A Windows
>
> Felkészülés a több virtuális gépeken futó üzembe használható Windows képként, a Windows beállításai (például a Windows biztonsági AZONOSÍTÓK és hostname (állomásnév)) kell a helyszíni virtuális a kivett/általános. Ezt megteheti sysprep használata a <https://technet.microsoft.com/library/cc721940.aspx>leírt módon.
>
> ![Linux][Logo_Linux] Linux
>
> Felkészülés a több virtuális gépeken futó üzembe használható Linux képként, bizonyos Linux beállítások kell a helyszíni virtuális a kivett/általános. Ezt megteheti waagent használatával – a [jelen cikkben] leírt módon deprovision[ virtual-machines-linux-capture-image] vagy a [jelen cikkben][virtual-machines-linux-agent-user-guide-command-line-options].

___

Az adatbázis tartalmát beállítása vagy-kezelővel az SAP szoftver rendelkezést új SAP rendszert telepít, adatbázis biztonsági másolatának visszaállítása a virtuális géphez csatlakoztatott merevlemezről vagy közvetlenül visszaállítása adatbázis biztonsági másolatának Azure-tárhelyről, ha az adatbázis-kezelő támogatja. (lásd: az [adatbázis-kezelő telepítési Guide][dbms-guide]). Ha már telepítette az SAP rendszer a helyszíni virtuális gép (különösen a 2-réteg System), módosíthatja az SAP rendszerbeállításoktól a Azure virtuális eljárással a rendszer nevezze át az SAP szoftver kiépítési Manager (SAP Megjegyzés [1619720]) által támogatott, a telepítés után. Egyéb esetben később a Azure virtuális telepítését követően az SAP-szoftver is telepítheti.

További részletek: fejezet [forgatókönyv 2: üzembe helyezése a virtuális egy egyéni képpel SAP] [telepítési útmutató, 3.3].

#### <a name="moving-a-vm-from-on-premises-to-microsoft-azure-with-a-non-generalized-disk"></a>Áttérés a virtuális a helyszíni Microsoft Azure nem általános lemezen
Azt tervezi, hogy a helyszíni áthelyezése egy adott SAP rendszer Microsoft Azure. Ehhez fel kell tölteni a virtuális, amely tartalmazza az operációs rendszer, az SAP bináris és esetleges az adatbázis-kezelő bináris, valamint a VHD adatokat, és jelentkezzen be az adatbázis-kezelő a Microsoft Azure fájljaival. Az alkalmazási példát, [üzembe helyezése a virtuális egy egyéni képpel] fejezet ismertetett való az ellentéte [telepítési – útmutató-3.1.2] fölött, ne a hostname (állomásnév), az SAP biztonsági AZONOSÍTÓK és az SAP felhasználói fiókokat az Azure virtuális, mint ahogyan azok a helyszíni környezetben fiókoknál. Emiatt az operációs rendszer generalizing már nem szükséges. Ebben az esetben határokon helyszíni felhasználási területei hol az SAP fekvő része fut a helyszíni és az alakzatok a Microsoft Azure főleg érvényesek lesznek.

További részletek: fejezet [forgatókönyv 3: egy virtuális áthelyezése a helyszíni egy nem általános Azure virtuális használata SAP] [telepítési útmutató, 3.4].

### <a name="db477013-9060-4602-9ad4-b0316f8bb281"></a>1 alkalmazási helyzetek: Az Azure piactéren elérhető az SAP ki a virtuális üzembe helyezése
Microsoft Azure ki az Azure piactéren elérhető, amely a Windows Server és a különböző Linux terjesztését néhány szokásos OS képet egy virtuális példány üzembe lehetőséget biztosít. Akkor is, amelyek tartalmazza az adatbázis-kezelő termékváltozatok például SQL Server telepítése. Ezekkel a képekkel használata az adatbázis-kezelő termékváltozatok részletekért olvassa el a [adatbázis-kezelő telepítési útmutató] [adatbázis-kezelő – útmutató]

Az SAP adott néhány lépést üzembe helyezése a Microsoft Azure piactéren ki a virtuális jelenne meg:

![Az SAP rendszerekhez egy virtuális kép használata a Microsoft Azure piactéren lévő virtuális telepítési folyamatábra][deployment-guide-figure-100]

A folyamatábra követően az alábbi lépéseket kell futtatható:

#### <a name="create-virtual-machine-using-the-azure-portal"></a>Az Azure-portálon virtuális gép létrehozása
A hozzon létre egy új virtuális számítógépre a Microsoft Azure piactéren lévő egy kép felhasználásával legegyszerűbben az Azure-portálon keresztül. Nyissa meg azt a <https://portal.azure.com/#create>. Írja be a keresőmezőjébe, például a Windows, SLES vagy RHEL telepítése, és válassza ki a verziót operációs rendszer. Győződjön meg arról, jelölje be a telepítési modell "Azure erőforrás-kezelő", és kattintson a Létrehozás gombra.

A varázsló végigvezeti Önt a szükséges paramétereket a virtuális gép együtt az összes szükséges erőforrások, mint a hálózati kapcsolatok és a tárterület-fiókok létrehozása. Ezeket a paramétereket a következők:

1. Alapjai
    1. : A név az erőforrás, azaz a virtuális számítógép neve
    1. Felhasználónév és jelszó/SSH nyilvános kulcs: írja be a felhasználónevét és jelszavát, a felhasználót, hogy a kiépítési során létre. Linux virtuális géphez, adja meg a használni kívánt nyilvános SSH kulcs történő bejelentkezéshez a gép SSH használatával.
    1. Előfizetés: Jelölje be az előfizetést, az új virtuális gép kiépítése használni kívánt.
    1. Erőforráscsoport: Az erőforrás csoport neve. Új erőforráscsoport vagy egy már létező erőforráscsoport a nevét vagy beszúrhat
    1. Hely: Válassza ki a helyet, ahol új virtuális gépen kell telepíthető. Ha szeretne a virtuális gép csatlakoztatása a helyszíni hálózaton, feltétlenül jelölje ki a helyet a helyszíni hálózaton kapcsolódó Azure virtuális hálózat. További részletekért olvassa el [A Microsoft Azure hálózati] fejezet[ planning-guide-microsoft-azure-networking] a [tervezési útmutató] [tervezési – útmutató].
1. Méret: Olvassa el az SAP Megjegyzés [1928533] listáját a támogatott virtuális típusok. Is győződjön meg róla, hogy a megfelelő típusú Ha prémium tárolási használni kívánt. Nem virtuális diagramtípusokat prémium tároló támogatja. Című [tároló: Microsoft Azure-tárhely és a adatok lemez] [ planning-guide-storage-microsoft-azure-storage-and-data-disks] , [Azure prémium tárterület] [tervezési-útmutató-azure-prémium-tárolás] a [tervezési útmutató] [tervezési – útmutató] további információt.
1. Beállítások
    1. Tárterület fiók: Akkor is jelöljön ki egy meglévő tárterület-fiókot vagy hozzon létre egy újat. Olvassa el a fejezet [Microsoft Azure tároló] [adatbázis-kezelő – útmutató-2.3] a [adatbázis-kezelő útmutató] [adatbázis-kezelő – útmutató] további információt a különböző tároló típusa. Ne feledje, hogy nem az összes tárolási típusok SAP-alkalmazások is használható.
    1. Virtuális hálózati és alhálózat: válassza ki a virtuális hálózat, amely a virtuális gép integrálása a vállalati intranethez tetszés csatlakozik a helyszíni hálózaton.
    1. Nyilvános IP-cím: jelölje be a nyilvános IP-cím, amelyet használni, vagy írja be a paramétereket hozhat létre egy új nyilvános IP-címet. Egy nyilvános IP-címét is használhatja az interneten keresztül a virtuális gép eléréséhez. Ellenőrizze, hogy az access a virtuális géphez szűréséhez hálózati biztonsági csoport is létrehozhat.
    1. Hálózati biztonsági csoport: lásd: [Mi az, hogy egy hálózati biztonsági csoport (NSG)] [ virtual-networks-nsg] további információt.
    1. Figyelemmel kísérése: Letilthatja a diagnosztika beállítást. Azt a rendszer automatikusan engedélyezi a parancsok ahhoz, hogy az Azure fokozott figyelése [Konfigurálása figyelése]fejezet ismertetett módon futtatásakor[deployment-guide-configure-monitoring-scenario-1].
    1. Elérhetőség: Válasszon egy Availablility, vagy adja meg a paraméterek hozzon létre egy új Availablility. További információt talál az fejezet [Azure elérhetőségének beállítása] [tervezési, útmutató, 3.2.3].
1. Összefoglalás: Az Összefoglalás oldalon megadott adatokkal érvényesítése, és kattintson az OK gombra.

Varázsló befejezése után a virtuális gép az erőforráscsoport kijelölt telepíthető.

#### <a name="create-virtual-machine-using-a-template"></a>Hozzon létre virtuális gép sablon használatával
Is létrehozhat az SAP-sablonok az [azure-quickstart útmutató-sablonok github tárházba]közzétett egyikével telepítés[azure-quickstart-templates-github]. Vagy hozhat létre virtuális gép az [Azure-portálon][virtual-machines-windows-tutorial], [PowerShell] [ virtual-machines-ps-create-preconfigure-windows-resource-manager-vms] vagy [Azure CLI] [ virtual-machines-linux-tutorial] manuálisan.

* [2 – réteg konfigurációs (csak egy virtuális gép) sablon] [ sap-templates-2-tier-marketplace-image] ezt a sablont használja, ha létre szeretne hozni egy 2 irányítási rendszer csak egy virtuális gépet használ.
* [3 – réteg konfigurációs (több virtuális gépeken futó) sablon] [ sap-templates-3-tier-marketplace-image] használja ezt a sablont, ha azt szeretné, a 3-as szintű rendszerbe több virtuális gépeken futó létrehozásához.

Miután megnyitotta a fenti sablonok közül, a szerkesztése paramétereinek panel megnyitja az Azure-portálra. Írja be az alábbi adatokat:

* **sapSystemId**: SAP azonosító
* **osType**: szeretné telepíteni, például a Windows Server 2012 R2, SLES 12 vagy RHEL 7.2 operációs rendszer
    * A lista csak tartalmazza a Microsoft Azure SAP által támogatott verziók
* **sapSystemSize**: az SAP rendszer méretének
    * Az új rendszer fog szolgálni SAP összege. Ha nem biztos abban, hogy a rendszer esetén kell, hogy hány SAP, kérje meg az SAP-technológia Partner vagy rendszerintegrátortól
* **systemAvailability**: (csak a 3-as szintű sablon) rendszer elérhetősége 
    * HA a HA példányát alkalmas konfiguráció kijelölése Az ASC két kiszolgálóinak létrejön, és két adatbázis-kiszolgáló.
* storageType: (csak a sablon egységes 2) használandó tároló típusú 
    * Nagyobb rendszerhez a prémium tárolási használatát ajánljuk. A különböző tároló típusú kapcsolatos további tudnivalókért olvassa el a 
        * [Microsoft Azure tároló] [adatbázis-kezelő – útmutató-2.3] [adatbázis-kezelő útmutatójának] [adatbázis-kezelő – útmutató]
        * [Prémium tárterület: Azure virtuális gép Munkaterhelésekből nagy teljesítményű tárterület][storage-premium-storage-preview-portal]
        * [Bevezetés a Microsoft Azure-tárolóhoz][storage-introduction]
* **adminUsername** és **adminPassword**: felhasználónév és jelszó
    * Új felhasználót hoz létre, amelyek segítségével jelentkezzen be a számítógépre.
* **newOrExistingSubnet**: azt határozza meg, hogy egy új virtuális hálózati és alhálózat kell létrehozni, vagy egy meglévő alhálózat kell használni. Ha már van egy virtuális hálózat, amelyhez csatlakozik a helyszíni hálózaton, jelölje be a meglévő.
* **subnetId**: az alhálózathoz, amely a virtuális gépeken futó kell csatlakoznia kell a azonosítója. Jelölje ki a alhálózat a virtuális Magánhálózati vagy Express útvonal virtuális hálózat a virtuális gép csatlakozni a helyszíni hálózaton. Az azonosító általában a /subscriptions/`<subscription id`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`>

Miután az összes paramétert ad meg, jelölje be az előfizetést, és a használni kívánt erőforráscsoport. Jelölje ki a meglévő erőforráscsoport vagy hozzon létre egy újat kiválasztásával "+ új" legördülő menüből. Ha létrehoz egy új erőforráscsoport, is jelölje ki a régió, ahol az erőforráscsoport és a virtuális gép jön létre.

Olvassa el a jogi feltételeket, fogadja el őket, és kattintson a Létrehozás gombra.

Felhívjuk a figyelmét arra, hogy az Azure virtuális ügynök rendszerbe alapértelmezés szerint, ha egy kép felhasználásával az Azure piactérről.

#### <a name="configure-proxy-settings"></a>A proxykiszolgáló beállításainak konfigurálása
Attól függően, hogy a helyszíni hálózati konfigurálásról akkor lehet szükség a proxy konfigurálása a virtuális gépen, ha a helyszíni hálózaton keresztül VPN vagy Express útvonal csatlakozik. Egyéb esetben a virtuális gép előfordulhat, hogy nem tud az internetre és ezért nem tudja a szükséges bővítmények letöltése vagy felügyeleti adatgyűjtés. Tanulmányozza a [Proxy beállítása] fejezet[ deployment-guide-configure-proxy] a dokumentum.

#### <a name="join-domain-windows-only"></a>Tartományhoz (csak Windows)
Abban az esetben, hogy a telepítés Azure-ban csatlakoztatva van-e a helyszíni Active Directory/DNS Azure-webhelyek, illetve Express útvonal (is szerepelhet határokon helyszíni, mint a [tervezése és végrehajtása Guide][planning-guide]), várhatóan, hogy a virtuális csatlakozás-e egy helyszíni tartomány. Ebben a lépésben kapcsolatos megfontolások fejezet [Bekapcsolódás virtuális helyszíni tartományba (csak Windows)] [telepítési – útmutató a-4,3], a dokumentum témakörben olvashat.

#### <a name="ec323ac3-1de9-4c3a-b770-4ff701def65b"></a>Figyelés konfigurálása
Konfigurálja az Azure fokozott figyelése bővítmény az SAP, a fejezet [konfigurálása Azure fokozott figyelése bővítményének SAP] leírtak szerint [telepítési – útmutató-4.5], a dokumentum.

Jelölje be a szükséges minimális változatának SAP Kernel és az SAP-Host ügynök fejezet [SAP erőforrások] [telepítési útmutató, 2.2], a dokumentum szerepel a források előfeltételei SAP figyelemmel kísérésére.

#### <a name="monitoring-check"></a>Jelölőnégyzet figyelése
Ellenőrizze, ha figyelemmel kísérése működik [ellenőrzi, és a végpontok közötti figyelés beállításához az SAP Azure a hibaelhárítás]fejezet ismertetett módon[deployment-guide-troubleshooting-chapter].

#### <a name="post-deployment-steps"></a>Bejegyzés telepítési lépéseket
Miután létrehozta a virtuális, fog szolgálni, és ezután meg, hogy a virtuális be minden szükséges összetevők telepítését. Így virtuális telepítési ilyen típusú vagy a szoftver már, néhány egyéb virtuális vagy lemezen, amely kapcsolható Microsoft Azure-ban rendelkezésre álló telepített beeing kell rendelkeznie. Azt keresi az idegen helyszíni esetek a helyszíni eszközökhöz (megosztások telepítés) kapcsolat esetén, vagy egy adott.

### <a name="54a1fc6d-24fd-4feb-9c57-ac588a55dff2"></a>2 alkalmazási helyzetek: SAP egy virtuális egy egyéni képpel üzembe helyezése
A [tervezés és implementálási útmutatójában] [tervezési – útmutató] már a részletes lépéseket ismertetett módon van egy módszer készítheti elő és egyéni kép készítése és több új VMs létrehozásához. A folyamatábra lépéseit sorozata jelenne meg:
 
![Virtuális telepítésének használata egy virtuális kép a magánjellegű piactéren SAP rendszereihez folyamatábrája][deployment-guide-figure-300]

A folyamatábra követően az alábbi lépéseket kell futtatható:

#### <a name="create-virtual-machine"></a>Virtuális gép létrehozása
Hozzon létre egy privát OS képe az Azure-portálon keresztül alkalmazó-telepítéshez, kövesse az SAP-sablonok az [azure-quickstart útmutató-sablonok github tárházba]közzétett egyikét[azure-quickstart-templates-github].
Szükség esetén létrehozhatja a [PowerShell] használatával virtuális gép[ virtual-machines-upload-image-windows-resource-manager] manuálisan. 

* [2 – réteg konfigurációs (csak egy virtuális gép) sablon] [ sap-templates-2-tier-user-image] használja ezt a sablont, ha azt szeretné, a csak egy virtuális gép és a saját OS kép egységes 2 rendszert szeretne létrehozni.
* [3 – réteg konfigurációs (több virtuális gépeken futó) sablon] [ sap-templates-3-tier-user-image] használja ezt a sablont, ha azt szeretné, egy több virtuális gépeken futó és a saját OS kép 3 szintű rendszert szeretne létrehozni.

Miután megnyitotta a fenti sablonok közül, a szerkesztése paramétereinek panel megnyitja az Azure-portálra. Írja be az alábbi adatokat:

* **sapSystemId**: SAP azonosító
* **osType**: operációs rendszer típusát szeretné telepíteni, a Windows vagy Linux rendszerhez
* **sapSystemSize**: az SAP rendszer méretének
    * Az új rendszer fog szolgálni SAP összege. Ha nem biztos abban, hogy a rendszer esetén kell, hogy hány SAP, kérje meg az SAP-technológia Partner vagy rendszerintegrátortól
* **systemAvailability**: (csak a 3-as szintű sablon) rendszer elérhetősége 
    * HA a HA példányát alkalmas konfiguráció kijelölése Az ASC két kiszolgálóinak létrejön, és két adatbázis-kiszolgáló.
* **storageType**: (csak a sablon egységes 2) használandó tároló típusú 
    * Nagyobb rendszerhez a prémium tárolási használatát ajánljuk. A különböző tároló típusú kapcsolatos további tudnivalókért olvassa el a 
        * [Microsoft Azure tároló] [adatbázis-kezelő – útmutató-2.3] [adatbázis-kezelő útmutatójának] [adatbázis-kezelő – útmutató]
        * [Prémium tárterület: Azure virtuális gép Munkaterhelésekből nagy teljesítményű tárterület][storage-premium-storage-preview-portal]
        * [Bevezetés a Microsoft Azure-tárolóhoz][storage-introduction]
* **adminUsername** és **adminPassword**: felhasználónév és jelszó
    * Új felhasználót hoz létre, amelyek segítségével jelentkezzen be a számítógépre.
* **userImageVhdUri**: URI, valamint a személyes OS kép virtuális, például a https://`<accountname`>.blob.core.windows.net/vhds/userimage.vhd
* **userImageStorageAccount**: a személyes OS kép pl. tároló tárterület-fiók neve `<accountname`> URI a fenti példában
* **newOrExistingSubnet**: azt határozza meg, hogy egy új virtuális hálózati és alhálózat kell létrehozni, vagy egy meglévő alhálózat kell használni. Ha már van egy virtuális hálózat, amelyhez csatlakozik a helyszíni hálózaton, jelölje be a meglévő.
* **subnetId**: az alhálózathoz, amely a virtuális gépeken futó kell csatlakoznia kell a azonosítója. Jelölje ki a alhálózat a virtuális Magánhálózati vagy Express útvonal virtuális hálózat a virtuális gép csatlakozni a helyszíni hálózaton. Az azonosító általában a /subscriptions/`<subscription id`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`>

Miután az összes paramétert ad meg, jelölje be az előfizetést, és a használni kívánt erőforráscsoport. Jelölje ki a meglévő erőforráscsoport vagy hozzon létre egy újat kiválasztásával "+ új" legördülő menüből. Ha létrehoz egy új erőforráscsoport, is jelölje ki a régió, ahol az erőforráscsoport és a virtuális gép jön létre.

Olvassa el a jogi feltételeket, fogadja el őket, és kattintson a Létrehozás gombra.

#### <a name="install-vm-agent-linux-only"></a>Virtuális ügynököt (csak Linux)
A Linux Agent már telepítve kell lennie a felhasználó kép, ha azt szeretné, hogy a fenti sablonok. Egyéb esetben a telepítés meghiúsul. Töltse le és telepítse a virtuális Agent felhasználói képe fejezet ismertetett módon [letöltése, telepítése, és Azure virtuális ügynök engedélyezése] [telepítési – útmutató-4.4], a dokumentum.
Ha Ön nem használja a fenti sablonokat, később is telepítheti a virtuális Agent.

#### <a name="join-domain-windows-only"></a>Tartományhoz (csak Windows)
Abban az esetben, hogy a telepítés Azure-ban csatlakoztatva van-e a helyszíni Active Directory/DNS Azure-webhelyek, illetve Express útvonal (is szerepelhet határokon helyszíni, mint a [tervezése és végrehajtása Guide][planning-guide]), várhatóan, hogy a virtuális csatlakozás-e egy helyszíni tartomány. Ebben a lépésben kapcsolatos megfontolások fejezet [Bekapcsolódás virtuális helyszíni tartományba (csak Windows)] [telepítési – útmutató a-4,3], a dokumentum témakörben olvashat.

#### <a name="configure-proxy-settings"></a>A proxykiszolgáló beállításainak konfigurálása
Attól függően, hogy a helyszíni hálózati konfigurálásról akkor lehet szükség a proxy konfigurálása a virtuális gépen, ha a helyszíni hálózaton keresztül VPN vagy Express útvonal csatlakozik. Egyéb esetben a virtuális gép előfordulhat, hogy nem tud az internetre és ezért nem tudja a szükséges bővítmények letöltése vagy felügyeleti adatgyűjtés. Tanulmányozza a [Proxy beállítása] fejezet[ deployment-guide-configure-proxy] a dokumentum.

#### <a name="configure-monitoring"></a>Figyelés konfigurálása
Konfigurálja az Azure figyelése bővítmény az SAP, a fejezet [konfigurálása Azure fokozott figyelése bővítményének SAP] leírtak szerint [telepítési – útmutató-4.5], a dokumentum.
Jelölje be a szükséges minimális változatának SAP Kernel és az SAP-Host ügynök fejezet [SAP erőforrások] [telepítési útmutató, 2.2], a dokumentum szerepel a források előfeltételei SAP figyelemmel kísérésére.

#### <a name="monitoring-check"></a>Jelölőnégyzet figyelése
Ellenőrizze, ha figyelemmel kísérése működik [ellenőrzi, és a végpontok közötti figyelés beállításához az SAP Azure a hibaelhárítás]fejezet ismertetett módon[deployment-guide-troubleshooting-chapter].

### <a name="a9a60133-a763-4de8-8986-ac0fa33aa8c1"></a>Forgatókönyv 3: Egy virtuális áthelyezése a helyszíni SAP egy nem általános Azure virtuális használata
Ebben az esetben a kis-és az SAP-rendszer egyszerűen áthelyezése az aktuális űrlap és az alakzat a helyszíni az Azure van megcímezheti. Eszközökkel neve nincs módosítása a Windows vagy Linux rendszerhez hostname (állomásnév) és az SAP biztonsági AZONOSÍTÓK vagy valami hasonlóan történik. Ebben az esetben a virtuális képként a telepítés során nem hivatkozik, de az operációs rendszer lemez közvetlenül szolgál. Esetében a telepítéshez ebben az esetben különböznek egymástól a két korábbi esetek a tény, hogy a virtuális Agent nem tudja automatikusan települ, a telepítés során. Az Azure virtuális ügynök ezért kell tölthető le a Microsoft, és telepítve és engedélyezve van a virtuális belül manuálisan később kell. Követően, hogy a feladat sikeresen befejeződött, továbbra is az SAP Host figyelése Azure bővítmény és konfigurációját kezdeményezhet. A függvény az Azure virtuális ügynök kapcsolatos részletekért ellenőrizze a Ez a cikk:

[comment]: <> (Teendők MSSedusch frissítés Windows hivatkozásra) 

___

> ![A Windows][Logo_Windows] A Windows
>
> <http://blogs.msdn.com/b/wats/Archive/2014/02/17/bginfo-Guest-Agent-Extension-for-Azure-VMS.aspx>
>
> ![Linux][Logo_Linux] Linux
>
> [Azure Linux ügynök útmutatója][virtual-machines-linux-agent-user-guide]

___

A munkafolyamat különböző lépés néz ki:
 
![Az SAP rendszerekhez virtuális lemezzel virtuális telepítési folyamatábra][deployment-guide-figure-400]

Feltételezve, hogy a lemez már feltöltés és az Azure (lásd: [tervezése és végrehajtása Guide][planning-guide]), tegye a következőket definiált

#### <a name="create-virtual-machine"></a>Virtuális gép létrehozása
Egy privát OS lemezzel az Azure-portálon keresztül telepítési létrehozásához használja az SAP-sablon az [azure-quickstart útmutató-sablonok github tárházba]közzétett[azure-quickstart-templates-github]. A virtuális gép használatával is létrehozhat a [PowerShell vagy Azure CLI manuálisan.

* [2 – réteg konfigurációs (csak egy virtuális gép) sablon][sap-templates-2-tier-os-disk]
    * E sablonnal Ha létre szeretne hozni egy 2 irányítási rendszer csak egy virtuális gépet használ.

A fenti sablon kibontása után megnyitja a szerkesztése paramétereinek panelen az Azure-portálra. Írja be az alábbi adatokat:

* **sapSystemId**: SAP azonosító
* **osType**: operációs rendszer típusát szeretné telepíteni, a Windows vagy Linux rendszerhez
* **sapSystemSize**: az SAP rendszer méretének
    * Az új rendszer fog szolgálni SAP összege. Ha nem biztos abban, hogy a rendszer esetén kell, hogy hány SAP, kérje meg az SAP-technológia Partner vagy rendszerintegrátortól
* **storageType**: (csak a sablon egységes 2) használandó tároló típusú 
    * Nagyobb rendszerhez a prémium tárolási használatát ajánljuk. A különböző tároló típusú kapcsolatos további tudnivalókért olvassa el a 
        * [Microsoft Azure tároló] [adatbázis-kezelő – útmutató-2.3] [adatbázis-kezelő útmutatójának] [adatbázis-kezelő – útmutató]
        * [Prémium tárterület: Azure virtuális gép Munkaterhelésekből nagy teljesítményű tárterület][storage-premium-storage-preview-portal]
        * [Bevezetés a Microsoft Azure-tárolóhoz][storage-introduction]
* **osDiskVhdUri**: a személyes OS URI-lemezre, például a https://`<accountname`>.blob.core.windows.net/vhds/osdisk.vhd
* **newOrExistingSubnet**: azt határozza meg, hogy egy új virtuális hálózati és alhálózat kell létrehozni, vagy egy meglévő alhálózat kell használni. Ha már van egy virtuális hálózat, amelyhez csatlakozik a helyszíni hálózaton, jelölje be a meglévő.
* **subnetId**: az alhálózathoz, amely a virtuális gépeken futó kell csatlakoznia kell a azonosítója. Jelölje ki a alhálózat a virtuális Magánhálózati vagy Express útvonal virtuális hálózat a virtuális gép csatlakozni a helyszíni hálózaton. Az azonosító általában a /subscriptions/`<subscription id`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`>

Miután az összes paramétert ad meg, jelölje be az előfizetést, és a használni kívánt erőforráscsoport. Jelölje ki a meglévő erőforráscsoport vagy hozzon létre egy újat kiválasztásával "+ új" legördülő menüből. Ha létrehoz egy új erőforráscsoport, is jelölje ki a régió, ahol az erőforráscsoport és a virtuális gép jön létre.

Olvassa el a jogi feltételeket, fogadja el őket, és kattintson a Létrehozás gombra.

#### <a name="install-vm-agent"></a>Virtuális ügynököt
A Linux Agent már telepítve kell lennie az operációs rendszer lemezt, ha azt szeretné, hogy a fenti sablonok. Egyéb esetben a telepítés meghiúsul. Töltse le és telepítse a virtuális Agent a virtuális fejezet ismertetett módon [letöltése, telepítése, és Azure virtuális ügynök engedélyezése] [telepítési – útmutató-4.4], a dokumentum.

Ha Ön nem használja a fenti sablonokat, később is telepítheti a virtuális Agent.

#### <a name="join-domain-windows-only"></a>Tartományhoz (csak Windows)
Abban az esetben, hogy a telepítés Azure-ban csatlakoztatva van-e a helyszíni Active Directory/DNS Azure-webhelyek, illetve Express útvonal (is szerepelhet határokon helyszíni, mint a [tervezése és végrehajtása Guide][planning-guide]), várhatóan, hogy a virtuális csatlakozás-e egy helyszíni tartomány. Ebben a lépésben kapcsolatos megfontolások fejezet [Bekapcsolódás virtuális helyszíni tartományba (csak Windows)] [telepítési – útmutató a-4,3], a dokumentum témakörben olvashat.

#### <a name="configure-proxy-settings"></a>A proxykiszolgáló beállításainak konfigurálása
Attól függően, hogy a helyszíni hálózati konfigurálásról akkor lehet szükség a proxy konfigurálása a virtuális gépen, ha a helyszíni hálózaton keresztül VPN vagy Express útvonal csatlakozik. Egyéb esetben a virtuális gép előfordulhat, hogy nem tud az internetre és ezért nem tudja a szükséges bővítmények letöltése vagy felügyeleti adatgyűjtés. Tanulmányozza a [Proxy beállítása] fejezet[ deployment-guide-configure-proxy] a dokumentum.

#### <a name="configure-monitoring"></a>Figyelés konfigurálása
Konfigurálja az Azure fokozott figyelése bővítmény az SAP, a fejezet [konfigurálása Azure fokozott figyelése bővítményének SAP] leírtak szerint [telepítési – útmutató-4.5], a dokumentum.

Jelölje be a szükséges minimális változatának SAP kernel és az SAP-Host ügynök fejezet [SAP erőforrások] [telepítési útmutató, 2.2], a dokumentum szerepel a források előfeltételei SAP figyelemmel kísérésére.

#### <a name="monitoring-check"></a>Jelölőnégyzet figyelése
Ellenőrizze, ha figyelemmel kísérése működik [ellenőrzi, és a végpontok közötti figyelés beállításához az SAP Azure a hibaelhárítás]fejezet ismertetett módon[deployment-guide-troubleshooting-chapter].

### <a name="scenario-4-updating-the-monitoring-configuration-for-sap"></a>Forgatókönyv 4: Frissítése az SAP felügyeleti beállításai
Előfordulhatnak olyan esetek, ahol szeretné frissíteni kell a megfigyeléssel konfiguráció:

* A közös MS/SAP csoport a felügyeleti lehetőségeket terjeszteni, és úgy döntött, hogy több Számláló hozzáadása vagy törlése a néhány számláló. 
* A Microsoft bemutatja az alapul szolgáló Azure infrastruktúra ellenőrző adatok előadásához új verziója, és az Azure fokozott figyelése bővítményének SAP alkalmazkodik ezekhez a változtatásokhoz.
* Az Azure virtuális gép csatlakoztatott további VHD hozzáadása vagy eltávolítása egy virtuális. Ebben az esetben módosítania kell a webhelycsoport tárolási kapcsolódó adatok. Ha módosítja a konfigurációban hozzáadása vagy törlése a végpontok vagy egy virtuális IP-címek hozzárendelése, ez az ellenőrzési konfiguráció nincs hatással.
* Megváltoztatja az Azure virtuális méretének pl. A5 virtuális bármilyen más méretet.
* Új hálózati kapcsolatok hozzáadása az Azure virtuális

Annak érdekében, hogy a felügyeleti konfiguráció frissíteni, folytassa az alábbi képlettel történik:

* A felügyeleti infrastruktúra frissítése a fejezet [konfigurálása Azure fokozott figyelése bővítményének SAP] című cikkben ismertetett lépéseket követve [telepítési – útmutató-4.5], a dokumentum. A fejezet ismertetett parancsfájl újra kell futtatni észleli, hogy a felügyeleti konfiguráció telepítve van, és végrehajtja a szükséges módosításokat a felügyeleti beállításait. 

___

> ![A Windows][Logo_Windows] A Windows
>
> A frissítést az Azure virtuális ügynök felhasználói beavatkozás nélkül szükség. Virtuális ügynök automatikusan frissíti magát, és nem kell indítani a virtuális.
>
> ![Linux][Logo_Linux] Linux
>
> Kövesse a [jelen cikkben] ismertetett lépések[ virtual-machines-linux-update-agent] az Azure Linux ügynök frissítéséhez. 

___

## <a name="detailed-single-deployment-steps"></a>Részletes egyetlen telepítési lépéseket.

### <a name="604bcec2-8b6e-48d2-a944-61b0f5dee2f7"></a>Azure PowerShell-parancsmagok üzembe helyezése
* Nyissa meg a <https://azure.microsoft.com/downloads/>
* A szakaszban "Parancssori eszközök" nincs egy "Windows PowerShell" című szakaszt. A "Telepítés" hivatkozásra.
* Microsoft letöltése elemre fog előugró ablak az a .exe végződő vonal elemre. Válassza a "Futtatás" lehetőséget.
* Előugró ablak kéri, hogy a Microsoft webes Platform telepítő futtatása-e be érkezzenek. Nyomja le az Igen gombra
* Egy, az alábbihoz hasonló képernyő jelenik meg:
 
![Azure PowerShell-parancsmagok telepítési képernyője][deployment-guide-figure-500]
<a name="figure-5"></a>

* Nyomja le a telepítést, és fogadja el a LICENCSZERZŐDÉST.

A gyakran ellenőrizze, hogy módosultak-e a PowerShell-parancsmagok. Általában frissítéseket a havi időtartamra vannak. Ehhez a legegyszerűbb módja felfelé a telepítés képernyőn látható [a] fentebb ismertetett kövesse a telepítési[ deployment-guide-figure-5] ábrán. A képernyőn a megjelenés dátumát a parancsmagok és a tényleges megjelenési száma látható. SAP jegyzetek [1928533] vagy [2015553]jelzett másképp, kivéve a legújabb Azure PowerShell-parancsmagok használata ajánlott.

Az Azure parancsmag asztali vagy hordozható a jelenlegi verzióját lehet ellenőrizni a PS paranccsal:

```powershell
Import-Module Azure
(Get-Module Azure).Version
```

Az eredmény be kell mutatni [ezt] az alábbiak szerint[ deployment-guide-figure-6] ábrán.

![Eredmény az Azure-PS parancsmag verziójának ellenőrzése][deployment-guide-figure-600]
<a name="figure-6"></a>

Az aktuális sora telepítve van az asztali vagy hordozható a Azure parancsmag verzió esetén a Microsoft webes Platform Installer indítása után első képernyőjén fog megjelenése némileg eltérő látható [Ez] egy képest[ deployment-guide-figure-5] ábrán.

Kérjük, figyelje meg a piros kör alatt az [ábra] [ deployment-guide-figure-7] alatt.
 
![Telepítési képernyője for jelezve, hogy telepítve vannak-e a legújabb kiadásának Azure PS parancsmagok Azure PowerShell-parancsmagok][deployment-guide-figure-700]
<a name="figure-7"></a>

Ha a képernyő néz ki, mint [feletti][deployment-guide-figure-7], jelezve, hogy a legutóbbi Azure parancsmag verziója már telepítve van, nem kell a telepítés folytatásához. Ebben az esetben "kiléphet" Ebben a szakaszban a telepítést.

### <a name="1ded9453-1330-442a-86ea-e0fd8ae8cab3"></a>Azure CLI üzembe helyezése
* Nyissa meg a <https://azure.microsoft.com/downloads/>
* A szakaszban a "Parancssori eszközök" nincs Azure parancssor nevű szakasz. Az operációs rendszerének telepítése hivatkozásra.

A gyakran ellenőrizze, hogy módosultak-e az Azure CLI. Általában frissítéseket a havi időtartamra vannak. A művelet legegyszerűbben a lépések telepítési fent leírt módon.

Aktuális verzióját Azure CLI meg az asztali vagy hordozható lehet ellenőrizni a paranccsal:

```
azure --version
```

Az eredmény be kell mutatni [ezt] az alábbiak szerint[ deployment-guide-figure-azure-cli-version] ábrán.

![Eredmény az Azure CLI verziójának ellenőrzése][deployment-guide-figure-760]
<a name="0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda"></a>

### <a name="31d9ecd6-b136-4c73-b61e-da4a29bbc9cc"></a>Virtuális csatlakozás helyszíni tartományba (csak Windows)
Azokban az esetekben, ahol határokon helyszíni példa az SAP VMs üzembe hol a helyszíni Active Directory és az Azure meghosszabbította a DNS, várhatóan, hogy a VMs adatbázis egy helyszíni tartományban. A virtuális csatlakozással a helyszíni tartomány és külön szoftver a részletes lépéseket kell a helyszíni tartomány felhasználói függő szükséges. Általában kapcsolódok be egy virtuális egy helyszíni tartomány azt jelenti, például a kártevők elleni védelem szoftver külön szoftver vagy a biztonsági másolat vagy felügyeleti szoftver különböző ügynökök telepítése.

Ezenkívül kell ellenőrizze, hogy hol Internet proxybeállításokat vannak ki a tartományt, csatlakozáskor, hogy a Windows helyi rendszer Account(S-1-5-18) a vendégként való bekapcsolódáshoz virtuális van-e ezek a beállítások megfelelőek. A tartomány csoportházirend alkalmazza a rendszer a tartományban, amely a proxy kényszerítése easiest.

### <a name="c7cbb0dc-52a4-49db-8e03-83e7edc2927d"></a>Letöltése, telepítése, és Azure virtuális ügynök engedélyezése
Az alábbi lépésekkel szükség, ha egy virtuális az SAP telepítve van a-OS képek nem általános például nem a Sysprep segédprogrammal előkészített Windows. Még nem a virtuális gépeken futó az Azure piactéren lévő rendszerbe Agent telepítéséhez szükséges. Ezeket a képeket a az Azure ügynök már tartalmaz.

#### <a name="b2db5c9a-a076-42c6-9835-16945868e866"></a>A Windows

* Töltse le az Azure virtuális ügynök:
    * Az Azure virtuális ügynök installer-csomag letöltéséhez: <https://go.microsoft.com/fwlink/?LinkId=394789>
    * A virtuális ügynök MSI-csomag helyben tárolni a hordozható vagy egy kiszolgálón
* Telepítse az Azure virtuális ügynök:
    * Csatlakozás a telepített Azure virtuális a terminált szolgáltatások (RDP)
    * Nyissa meg a virtuális a Windows Intézőt, és nyissa meg a cél könyvtár a virtuális Agent MSI-fájl
    * Húzással helyezze a helyi laptopon Server az Azure virtuális ügynök Installer MSI-fájlt a virtuális Agent a virtuális az a cél könyvtár
    * Kattintson duplán a virtuális az MSI-fájl
    * A virtuális csatlakozik a helyszíni tartomány, győződjön meg arról, hogy esetleges Internet proxybeállításokat alkalmazni a Windows helyi rendszer fiók (S-1-5-18) a virtuális, valamint a fejezet [Proxykiszolgáló beállítása]a[deployment-guide-configure-proxy]. A virtuális ügynök futtatható az ebben a környezetben, és tud csatlakozni az Azure lennie.

#### <a name="6889ff12-eaaf-4f3c-97e1-7c9edc7f7542"></a>Linux
Telepítse a virtuális Agent Linux rendszerhez, a következő parancsot a

- **SLES**

```
sudo zypper install WALinuxAgent
```
- **RHEL**

```
sudo yum install WALinuxAgent
```

### <a name="baccae00-6f79-4307-ade4-40292ce4e02d"></a>A proxykiszolgáló beállítása
Windows és Linux között bekapcsolásának lépései a proxy beállítása.

#### <a name="windows"></a>A Windows
Ezeket a beállításokat is érvényes, a rendszerfiók csatlakozik az internetre kell lenniük. Ha a proxybeállításokat nincsenek beállítva csoportházirend, a helyi rendszerfiók kövesse ezeket a lépéseket követve állítsa be a beállíthatja őket.

1.  Nyissa meg a gpedit.msc
1.  Nyissa meg azt a számítógép konfigurációja –> Felügyeleti sablonok-Windows-összetevők > az Internet Explorer -> és engedélyezése: "a proxykiszolgáló beállításainak gépi (helyett felhasználónkénti)
1.  Nyissa meg a Vezérlőpultot, és kattintson az-hálózat és Internet > Internetbeállítások
1.  Nyissa meg a kapcsolatok fülre, és kattintson a helyi hálózati beállítások
1.  "A beállítások automatikus észlelése" letiltása
1.  "A proxykiszolgáló használata a helyi hálózaton" engedélyezése, és írja be a proxy host és a port

#### <a name="linux"></a>Linux
A helyes proxy konfigurálása a Microsoft Azure Vendég Agent a konfigurációs fájl amely a felső /etc/waagent.conf található. A következő paraméterek be kell állítania:

```
HttpProxy.Host=<proxy host e.g. proxy.corp.local>
HttpProxy.Port=<port of the proxy host e.g. 80>
```

Miután módosította a konfigurációban, indítsa újra a agent

```
sudo service waagent restart
```

A proxybeállítások a /etc/waagent.conf is alkalmazni a szükséges virtuális Extensions. Ha meg szeretné használni az Azure tárházakban, győződjön meg arról, hogy a következő tárházakban forgalmat nem jutnak a helyi intranet elemet. Ha felhasználó által definiált útvonalak engedélyezése a kényszerített Tunneling, ügyeljen arra, hogy út hozzáadása hozta létre, amelyek a forgalom átirányítása a tárházakban közvetlenül az interneten, és a webhely kapcsolaton keresztül nem.

- **SLES** Meg kell útvonalak /etc/regionserverclnt.cfg felsorolt IP-címek hozzáadása. Példa az alábbi képernyőképen látható. 

- **RHEL** Meg kell útvonalak /etc/yum.repos.d/rhui-load-balancers felsorolt állomások IP-címek hozzáadása. Példa az alábbi képernyőképen látható.

Ha többet szeretne tudni a felhasználó által definiált utakat, lásd: [Ez a cikk][virtual-networks-udr-overview].

![Kényszerített Tunneling][deployment-guide-figure-50]

### <a name="d98edcd3-f2a1-49f7-b26a-07448ceb60ca"></a>SAP Azure továbbfejlesztett felügyeleti bővítmény konfigurálása
Miután a virtuális készen áll arra, [telepítési esetek a VMs az SAP, a Microsoft Azure] fejezet ismertetett módon [telepítési-útmutató-3], az Azure virtuális ügynök telepítve van a gépen. A következő fontos lépésként üzembe az Azure fokozott figyelése bővítmény az SAP, amely érhető el az Azure-bővítmény adattárban a globális adatközpont esetén a Microsoft Azure. További részletekért ellenőrizze a [tervezés és implementálási útmutatójában] [tervezési, útmutató, 9.1]. 

Telepítse és állítsa be az Azure fokozott figyelése bővítményének SAP Azure PowerShell vagy Azure CLI használható. Olvassa el a fejezet [Azure PowerShell] [telepítési – útmutató-4.5.1], ha azt szeretné, hogy a bővítmény telepítése Windows vagy Linux virtuális egy Windows gépet használ. A bővítmény telepítése egy használata egy Linux Linux virtuális asztali olvassa el a fejezet [Azure CLI] [telepítési – útmutató-4.5.2.].

#### <a name="987cf279-d713-4b4c-8143-6b11589bb9d4"></a>Azure PowerShell Linux és a Windows VMs
Az Azure fokozott figyelése bővítményének SAP telepítésének a feladat végrehajtásához hajtsa végre az alábbi lépéseket:

* Győződjön meg arról, hogy telepítette a Microsoft Azure PowerShell-parancsmag a legújabb verzióját. Című [telepíti az Azure PowerShell-parancsmagok] [telepítési – útmutató-4.1-es], a dokumentum.  
* Az alábbi PowerShell-parancsmag futtatásával. A rendelkezésre álló környezetek listáját futtassa a Get-AzureRmEnvironment parancsmag. Ha nyilvános Azure használni kívánt, a környezete AzureCloud. Azure Kínában jelölje ki a AzureChinaCloud.

```powershell
    $env = Get-AzureRmEnvironment -Name <name of the environment>
    Login-AzureRmAccount -Environment $env
    Set-AzureRmContext -SubscriptionName <subscription name>
    
    Set-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
```

Megadott fiók adatait, és az Azure virtuális számítógépen, miután a parancsfájl a szükséges bővítmények telepítése, és a szükséges szolgáltatásainak engedélyezése. Néhány percig, amíg ez vehet igénybe.
Olvassa el [a MSDN-cikk] [ msdn-set-azurermvmaemextension] további információt a Set-AzureRmVMAEMExtension.
  
![A sikeres végrehajtás SAP adott Azure parancsmag Set-AzureRmVMAEMExtension eredmény képernyő][deployment-guide-figure-900]

A Set-AzureRmVMAEMExtension sikeres futtatása minden, a funkció figyelése az SAP állomás beállításához szükséges lépések hajt végre. 

A kimenet a parancsfájl továbbítsa néz ki:

* Az ellenőrző adatokat az alap virtuális (az operációs rendszer tartalmazó), valamint az összes további VHD csatlakoztatott a virtuális visszaigazoló lett beállítva.
* A következő két üzenetek vannak arról, hogy a tárolási mértékek adott tárterület-fiók beállításait. 
* A kimenet egy sorral állapotban ad meg a tényleges frissítés az ellenőrzési beállításainak.
* Egy másik megjelenik, hogy a konfigurációs telepített vagy frissített tájékoztat.
* A kimenet utolsó sora tájékoztató megjelenítő az ellenőrzési beállítások tesztelése lehetőséget.
* Azure fokozott figyelemmel kísérés összes lépések végrehajtása sikeres és ellenőrzéséhez, hogy az Azure-infrastruktúrát biztosít a szükséges adatokat folytatásához Azure fokozott figyelése bővítmény az SAP, készenléti ellenőrzése [készenléti keresésének Azure fokozott figyelése az SAP] fejezet ismertetett módon [telepítési – útmutató-5.1] a dokumentumban. 
* Ezzel a folytatáshoz várja meg a 15-30 percig, amíg az Azure diagnosztika lesz a megfelelő adatokat gyűjtött.

#### <a name="408f3779-f422-4413-82f8-c57a23b4fc2f"></a>Azure CLI Linux VMs számára

Telepíthető az Azure fokozott figyelése kiterjesztése SAP Azure CLI használata a feladat végrehajtásához hajtsa végre az alábbi lépéseket:

1. Telepítse az Azure CLI ismertetett módon [a] [ azure-cli] cikk
1. Jelentkezzen be az Azure-fiók

    ```
    azure login
    ```
1. Erőforrás-kezelő Azure módba vált

    ```
    azure config mode arm
    ```
1. Azure fokozott ellenőrzés engedélyezése

    ```
    azure vm enable-aem <resource-group-name> <vm-name>
    ```  
1. Ellenőrizze, hogy az Azure fokozott figyelése a Azure Linux virtuális aktív. A fájl /var/lib/AzureEnhancedMonitor/PerfCounters létezésének ellenőrzése. Ha létezik, az AEM által gyűjtött adatok megjelenítése:

    ```
    cat /var/lib/AzureEnhancedMonitor/PerfCounters
    ```
    Majd a kimenet például jelenik meg:
    
    ```
    2;cpu;Current Hw Frequency;;0;2194.659;MHz;60;1444036656;saplnxmon;
    2;cpu;Max Hw Frequency;;0;2194.659;MHz;0;1444036656;saplnxmon;
    …
    …
    ```

## <a name="564adb4f-5c95-4041-9616-6635e83a810b"></a>Ellenőrzések és SAP Azure-végpont figyelése beállítása kapcsolatos problémák megoldása
Után az Azure virtuális rendszerbe, és állítsa be a megfelelő Azure felügyeleti infrastruktúra, ellenőrizze, hogy az adott Azure fokozott nyomon követése összetevői működik-e megfelelő módon. 

Ezért, hajtsa végre az Azure fokozott figyelése bővítményének SAP készenléti ellenőrzése [készenléti keresésének Azure fokozott figyelése az SAP] fejezet ismertetett módon [telepítési útmutató, 5.1]. Ha ez az ellenőrzés eredménye pozitív, és minden vonatkozó teljesítmény számláló kap, az Azure figyelése a telepítő sikeresen megtörtént. Ebben az esetben az SAP-Host ügynök fejezet [SAP erőforrások] [telepítési útmutató, 2.2], a dokumentum felsorolt SAP jegyzeteket ismertetett módon a telepítés folytatásához. Ha az eredmény a felkészülés ellenőrzés hiányzó számláló azt jelzi, folytassa az állapot-ellenőrzés a Azure figyelése infrastruktúrájának végrehajtása a fejezet [állapot-ellenőrzés az Azure figyelése infrastruktúra konfigurációs] ismertetett módon [telepítési útmutató, 5,2]. Abban az esetben, ha bármilyen problémát az Azure figyelése beállításokkal, jelölje be a fejezet [további hibaelhárítási Azure figyelése infrastruktúra az SAP] [telepítési – útmutató-5.3] további segítséget a hibaelhárítás.

### <a name="bb61ce92-8c5c-461f-8c53-39f5e5ed91f2"></a>Azure fokozott ellenőrzés az SAP készenléti ellenőrzése
Ez az ellenőrzés az gondoskodnia arról, hogy a mértékek, amelyek kezdése az SAP-alkalmazás meg fog jelenni érhetők el teljesen az alapul szolgáló Azure figyelése infrastruktúra. 

#### <a name="execute-the-readiness-check-on-a-windows-vm"></a>Hajtsa végre a Windows virtuális készenléti ellenőrzése
Hajtsa végre a készenléti jelölőnégyzetet, a bejelentkezési az Azure virtuális gépen (rendszergazdai fiók mező nem szükséges), és hajtsa végre az alábbi lépéseket:

* Nyissa meg a Windows parancssort, és módosítsa az Azure figyelése bővítmény telepítési mappájában az SAP C:\Packages\Plugins\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\`<version`> \drop

A megadott elérési útját a fenti felügyeleti bővítmény verzió rész eltérőek lehetnek. Több mappa a megfigyeléssel bővítmény verzió, a telepítési mappájában jelenik meg, ha a Windows-szolgáltatás "AzureEnhancedMonitoring" konfigurációjának ellenőrzése, és a "Futtatható fájl elérési útja" jelöléssel mappára vált.
 
![Az Azure fokozott figyelése bővítményének SAP futó szolgáltatás tulajdonságai][deployment-guide-figure-1000]

* Hajtsa végre a azperflib.exe paraméter nélkül a parancssorablakban.

> [AZURE.NOTE] A azperflib.exe ismétlődve fut, és frissíti a összegyűjtött számláló 60 másodpercenként. Annak érdekében, hogy a leállításig befejezéséhez zárja be a parancssorablakban.

Ha nincs telepítve az Azure fokozott figyelése bővítmény, vagy nem fut a szolgáltatás "AzureEnhancedMonitoring", a bővítmény nincs beállítva megfelelően. Ebben az esetben, hajtsa végre a fejezet [további hibaelhárítási Azure figyelése infrastruktúra az SAP] [telepítési – útmutató-5.3] létrehozásának lépéseit hogyan telepítsen újra a bővítmény.

##### <a name="check-the-output-of-azperflibexe"></a>Jelölje be a azperflib.exe kimenete
Azperflib.exe eredményét jeleníti meg, az SAP az Azure teljesítmény számláló összes töltve. Összegyűjtött számláló listájának alján található a összegzését és Azure nyomon követése állapotát jelző állapotát mutató. 
 
![Állapot-ellenőrzés hajtja végre, azperflib.exe, amely jelzi, hogy nincsenek problémák léteznek kimenete][deployment-guide-figure-1100]
<a name="figure-11"></a>

Jelölje be a "Számláló összesen", az összegének a Kimenet művelet eredménye, amely jelentett üresnek, és az állapot-ellenőrzése mint az ábrán [felett]a fent látható[deployment-guide-figure-11].

Az eredmény értékek is értelmezi az alábbi képlettel történik:

| Azperflib.exe eredményértékeinek | Azure készenléti állapot ellenőrzése |
| ------------------------------|----------------------------------- |
| **Számláló összeg: üres** | Lehet, hogy a következő 2 Azure tároló számláló üres: <ul><li>Tároló további m késés kiszolgáló ezredmásodperc</li><li>Tároló további m késés E2E ezredmásodperc</li></ul>Az összes többi számláló értékeket kell tartalmaznia. |
| **Állapot-ellenőrzése** | Csak az OK gombra, ha feladó állapot mutatja az OK gombra |

Nem mindkét eredményül azperflib.exe mutatják, hogy az összes feltöltött számláló helyes eredmény, kövesse az utasításokat, az állapot-ellenőrzés az Azure figyelése infrastruktúra konfiguráció, fejezet [állapot-ellenőrzés az Azure figyelése infrastruktúra konfigurációs] ismertetett módon [telepítési – útmutató-5,2] alatt.

#### <a name="execute-the-readiness-check-on-a-linux-vm"></a>Hajtsa végre a Linux virtuális készenléti ellenőrzése
Annak érdekében, hogy a felkészülés jelölőnégyzetet, a Azure virtuális gép SSH kapcsolatba, és hajtsa végre az alábbi lépéseket:

* Jelölje be a kimenet az Azure fokozott figyelése bővítmény
    * További /var/lib/AzureEnhancedMonitor/PerfCounters
        * Meg kell adnia teljesítmény számláló listáját. A fájl nem lehet üres.
    * /var/lib/AzureEnhancedMonitor/PerfCounters macskaeledel |} GREP hiba
        * Térjen vissza a hiba esetén nincs pl. 3 egysoros; beállítások; Hiba; 0; 0; **nincs**; 0; 1456416792; teszt – servercs;
    * További /var/lib/AzureEnhancedMonitor/LatestErrorRecord
        * Érdemes lehet üres vagy kell nem létezik
* Ha a fenti első jelölőnégyzet nem sikerült, végezze el az alábbi további teszteket:
    * Győződjön meg róla, hogy a waagent telepítve és lépések
        * sudo ls-al/var/lib/waagent /
            * a tartalom a waagent könyvtár szerepelnie kell
        * PS-ax |} GREP waagent
            * meg kell jelennie egy bejegyzésnek hasonló "python /usr/sbin/waagent-démon"
    * Győződjön meg arról, hogy a Linux diagnosztikai bővítmény telepítése és elindítása
        * sudo sh - c "ls-al /var/lib/waagent/Microsoft.OSTCExtensions.LinuxDiagnostic-*"
            * a tartalom a diagnosztikai bővítmény Linux könyvtár szerepelnie kell
        * PS-ax |} diagnosztikai GREP
            * meg kell jelennie egy bejegyzésnek hasonló "python /var/lib/waagent/Microsoft.OSTCExtensions.LinuxDiagnostic-2.0.92/diagnostic.py-démon"
    * Győződjön meg arról, hogy az Azure fokozott figyelése bővítmény telepítése és elindítása
        * sudo sh - c "ls-al /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-*/"
            * a tartalom az Azure fokozott figyelése bővítmény könyvtár szerepelnie kell
        * PS-ax |} GREP AzureEnhanced
            * meg kell jelennie egy bejegyzésnek hasonló "python /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-2.0.0.2/handler.py démon"
* Az SAP-Host ügynök telepítse az SAP Megjegyzés [1031096] ismertetett módon, és jelölje be a saposcol kimenete
    * Hajtsa végre a /usr/sap/hostctrl/exe/saposcol -d
    * Kiírása ÉKM végrehajtása
    * Ellenőrizze, hogy a mérőszám "Virtualization_Configuration\Enhanced figyelése Access" igaz
* Ha már van egy SAP NetWeaver ABAP alkalmazáskiszolgáló telepítve van, nyissa meg tranzakció ST06 és jelölőnégyzetet, ha a bővített funkciójú figyelés engedélyezve van.

Ha minden fölött fail, ellenőrzést hajtsa végre a fejezet [további hibaelhárítási Azure figyelése infrastruktúra az SAP] [telepítési – útmutató-5.3] létrehozásának lépéseit hogyan telepítsen újra a bővítmény.

### <a name="e2d592ff-b4ea-4a53-a91a-e5521edb6cd1"></a>Állapot-ellenőrzés Azure-infrastruktúra konfigurációjának ellenőrzése
Ha egyes nyomon követése adatok nem kézbesítve megfelelően a vizsgálat [készenléti keresésének Azure fokozott figyelése az SAP] fejezet ismertetett jelölt [telepítési útmutató, 5.1] fent, hajtsa végre a próba-AzureRmVMAEMExtension parancsmag ellenőrzéséhez, ha az aktuális az Azure figyelése infrastruktúra és konfigurálása a figyelés bővítmény az SAP helyes.

Annak érdekében, hogy az ellenőrzési beállítások tesztelése, hajtsa végre a következő sorrendben:

* Győződjön meg arról, hogy telepítve van a Microsoft Azure PowerShell-parancsmag a legújabb fejezet [telepíti az Azure PowerShell-parancsmagok] ismertetett módon [telepítési – útmutató-4.1-es], a dokumentum.
* Az alábbi PowerShell-parancsmag futtatásával. A rendelkezésre álló környezetek listáját futtassa a Get-AzureRmEnvironment parancsmag. Ha nyilvános Azure használni kívánt, a környezete AzureCloud. Azure Kínában jelölje ki a AzureChinaCloud.

```powershell
$env = Get-AzureRmEnvironment -Name <name of the environment>
Login-AzureRmAccount -Environment $env
Set-AzureRmContext -SubscriptionName <subscription name>
Test-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
```

* Megadott fiók adatait, és az Azure virtuális számítógépen, miután a a parancsfájl fog tesztelje a konfigurációt, kiválaszthatja, hogy virtuális gép.

 
![SAP – adott Azure parancsmag próba-VMConfigForSAP_GUI beviteli képernyő][deployment-guide-figure-1200]

Után a fiók és az Azure virtuális gép adta meg az adatokat, a parancsfájl fog tesztelje a konfigurációt, kiválaszthatja, hogy virtuális gép.
 
![Az SAP Azure figyelése infrastruktúra sikeres próba eredménye][deployment-guide-figure-1300]

Győződjön meg arról, hogy minden be van-e jelölve az OK gombra. Ha az ellenőrzések részét nem ok, hajtsa végre a frissítés parancsmag fejezet [konfigurálása Azure fokozott figyelése bővítményének SAP] ismertetett módon [telepítési – útmutató-4.5], a dokumentum. Várjon egy másik 15 percet, és az ellenőrzéshez ismertetett fejezet [készenléti keresésének Azure fokozott figyelése az SAP] [telepítési útmutató, 5.1] és [állapot-ellenőrzés az Azure figyelése infrastruktúra konfigurációs] [telepítési – útmutató-5,2] újra. Az ellenőrzés továbbra is néhány vagy összes számláló hibáját jelzi, ha csak a fejezet [további hibaelhárítási Azure figyelése infrastruktúra az SAP] [telepítési útmutató, 5.3].

### <a name="fe25a7da-4e4e-4388-8907-8abc2d33cfd8"></a>További hibaelhárítási Azure figyelése infrastruktúra az SAP

#### <a name="windowslogowindows-azure-performance-counters-do-not-show-up-at-all"></a>![A Windows][Logo_Windows] Azure teljesítmény számláló nem jelenik meg minden
Az Azure a teljesítménymutatók gyűjteménye befejeződött, a Windows-szolgáltatás "AzureEnhancedMonitoring". Ha a szolgáltatás nincs telepítve megfelelően, vagy ha nem fut a virtuális, nincs teljesítménymutatók egyáltalán gyűjthetők össze.

##### <a name="the-installation-directory-of-the-azure-enhanced-monitoring-extension-is-empty"></a>A telepítési könyvtárban az Azure fokozott figyelése bővítmény ürül. 

###### <a name="issue"></a>A probléma
A telepítési könyvtár C:\Packages\Plugins\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\`<version`> \drop ürül.

###### <a name="solution"></a>Megoldás
A bővítmény nincs telepítve. Ellenőrizze a proxy probléma (leírtak szerint előtt) esetén. Indítsa újra a számítógépet, illetve futtassa újra a konfigurációs parancsfájl parancsfájl Set-AzureRmVMAEMExtension szükséges

##### <a name="service-for-azure-enhanced-monitoring-does-not-exist"></a>Azure fokozott ellenőrzésére szolgáló szolgáltatás nem érhető el 

###### <a name="issue"></a>A probléma
Windows-alapú szolgáltatást "AzureEnhancedMonitoring" nem létezik. Azperflib.exe: A azperlib.exe kimeneti okoz hiba az [alábbi ábrán]látható módon[deployment-guide-figure-14].
 
![Azperflib.exe végrehajtását azt jelzi, hogy nem fut a szolgáltatás az Azure fokozott figyelése bővítmény az SAP][deployment-guide-figure-1400]
<a name="figure-14"></a>

###### <a name="solution"></a>Megoldás
Ha a szolgáltatás nem érhető el a [fenti ábrán]látható módon[deployment-guide-figure-14], az SAP Azure figyelése bővítmény nincs telepítve megfelelően. A bővítmény telepítési igényektől fejezet [telepítési esetek a VMs az SAP, a Microsoft Azure] című témakörben leírt lépésekkel szerint telepítsen újra [telepítési útmutató, 3]. 

A bővítmény telepítő, miután újbóli ellenőrzése gombra, hogy az Azure teljesítmény számláló is szerepelnek a Azure virtuális után 1 óra értéket.

##### <a name="service-for-azure-enhanced-monitoring-exists-but-fails-to-start"></a>Azure fokozott ellenőrzésére szolgáló szolgáltatás létezik, de nem indul el 

###### <a name="issue"></a>A probléma
Windows-alapú szolgáltatást "AzureEnhancedMonitoring" van engedélyezve van, de nem indul el. Jelölje be az alkalmazás eseménynaplójában talál további információt.

###### <a name="solution"></a>Megoldás
Helytelen konfigurációja. A felügyeleti bővítmény ismételt engedélyezése a virtuális fejezet [konfigurálása Azure fokozott figyelése bővítményének SAP] ismertetett módon [telepítési útmutató, 4.5].

#### <a name="windowslogowindows-some-azure-performance-counters-are-missing"></a>![A Windows][Logo_Windows] Néhány Azure teljesítmény számláló hiányoznak.
"AzureEnhancedMonitoring", amely számos különböző forrásból adatok lekérése a Windows-szolgáltatás által végzett az Azure a teljesítménymutatók gyűjteménye. Néhány konfigurációs adatgyűjtés helyi meghajtóra, teljesítménymutatók olvasnak az Azure diagnosztika, és tároló számláló használt tárterület előfizetés szintjén a naplózás.

Ha a hibaelhárítás SAP Megjegyzés [1999351] nem segítettek, futtassa újra a konfigurációs parancsfájl Set-AzureRmVMAEMExtension. Előfordulhat, ha megvárja, mert tárolási analytics vagy diagnosztika számláló nem hozható létre, közvetlenül az után engedélyezve van egy óra. Ha a probléma továbbra is fennáll, nyissa meg a az összetevő BC-m-NT-AZR egy SAP ügyfél támogatási üzenetet.

#### <a name="linuxlogolinux-azure-performance-counters-do-not-show-up-at-all"></a>![Linux][Logo_Linux] Azure teljesítmény számláló nem jelenik meg minden

Az Azure a teljesítménymutatók gyűjteménye egy deamon végzi. Ha nem fut a deamon, nincs teljesítménymutatók egyáltalán gyűjthetők össze.

##### <a name="the-installation-directory-of-the-azure-enhanced-monitoring-extension-is-empty"></a>A telepítési könyvtárban az Azure fokozott figyelése bővítmény ürül. 

###### <a name="issue"></a>A probléma
A címtár/var/tár vagy waagent/nem tartalmaz az Azure fokozott figyelése bővítmény alkönyvtárat.

###### <a name="solution"></a>Megoldás
A bővítmény nincs telepítve. Ellenőrizze a proxy probléma (leírtak szerint előtt) esetén. Indítsa újra a számítógépet, illetve futtassa újra a konfigurációs parancsfájl Set-AzureRmVMAEMExtension szükséges

#### <a name="linuxlogolinux-some-azure-performance-counters-are-missing"></a>![Linux][Logo_Linux] Néhány Azure teljesítmény számláló hiányoznak.

Az Azure a teljesítménymutatók gyűjteménye adatok lekérése számos különböző forrásból démon végzi. Néhány konfigurációs adatgyűjtés helyi meghajtóra, teljesítménymutatók olvasnak az Azure diagnosztika, és tároló számláló használt tárterület előfizetés szintjén a naplózás.

Egy olyan és naprakész ismert problémák listáját című SAP Megjegyzés [1999351] fokozott Azure nyomon követése az SAP további hibaelhárítási információkat tartalmazó.

Ha a hibaelhárítás SAP Megjegyzés [1999351] nem segítettek, futtassa újra a konfigurációs parancsfájl Set-AzureRmVMAEMExtension fejezet [konfigurálása Azure fokozott figyelése bővítményének SAP] ismertetett módon [telepítési útmutató, 4.5]. Előfordulhat, ha megvárja, mert tároló analytics vagy diagnosztika számláló nem hozható létre, közvetlenül az után engedélyezve van egy óra. Ha a probléma továbbra is fennáll, nyissa meg a SAP ügyfél támogatási üzenetben összetevő BC-m-NT-AZR for Windows vagy BC-m-LNX-AZR Linux virtuális géphez.

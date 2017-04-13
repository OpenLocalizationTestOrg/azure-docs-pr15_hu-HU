<properties
    pageTitle="Azure webhely helyreállítási támogatási mátrix |} Microsoft Azure"
    description="A támogatott operációs rendszerek és -összetevők áttekintheti az Azure-webhely helyreállítás"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="azure-site-recovery-support-matrix"></a>Azure webhely helyreállítási támogatási mátrix

Ez a cikk a támogatott operációs rendszerek és -összetevők Azure webhely helyreállítási telepítésekhez foglalja össze. Támogatott összetevők és a vonatkozó követelmények érhető el az egyes telepítési forgatókönyv minden megfelelő telepítési olvashatók, és a dokumentum összefoglalja őket.

## <a name="supported-operating-systems-for-virtualization-servers"></a>Támogatott operációs rendszerek virtualizációs kiszolgálók


**Forrás** | **Cél** | **A Host operációs rendszer**
---|---|---|--- 
**A Hyper-V hosts (nélkül VMM)** | Azure | A legújabb frissítéseit a Windows Server 2012 R2
**A Hyper-V hosts (a VMM)** | Azure | A legújabb frissítéseit a Windows Server 2012 R2
**A Hyper-V hosts (a VMM)** | Másodlagos VMM webhely | Legalább Windows Server 2012 legújabb frissítésekben
**VMware hosts/vCenter** | Azure | vCenter 5.5 vagy 6.0 (csak a 5,5 szolgáltatások támogatása) <br/><br/> vSphere 6.0, a 5.5 vagy a legújabb frissítésekben 5.1
**VMware hosts/vCenter** | Másodlagos VMware webhely | vCenter 5.5 vagy 6.0 (csak a 5,5 szolgáltatások támogatása) <br/><br/> vSphere 6.0, a 5.5 vagy a legújabb frissítésekben 5.1

## <a name="supported-requirements-for-replicated-machines"></a>Replikált gépek támogatott követelményei

**Forrás** | **Mire van replikált** | **Cél** | **A Host operációs rendszer**
---|---|---|--- 
**A Hyper-V VMs** | Bármely terhelést | Azure | Bármely Vendég OS [Azure által támogatott](https://technet.microsoft.com/library/cc794868.aspx)<br/><br/> VMs meg kell felelnie a [Azure követelményeknek](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**A Hyper-V VMs (a VMM)** | Bármely terhelést | Azure | Bármely Vendég OS [Azure által támogatott](https://technet.microsoft.com/library/cc794868.aspx)<br/><br/> VMs meg kell felelnie a [Azure követelményeknek](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**A Hyper-V VMs (a VMM)** | Bármely terhelést | Másodlagos VMM webhely | Bármely vendég operációs rendszer [támogatja a Hyper-V](https://technet.microsoft.com/library/mt126277.aspx)
**VMware VMs** | Minden futó Windows virtuális terhelést | Azure | 64 bites Windows Server 2012 R2, a Windows Server 2012-ben, a Windows Server 2008 R2 a legalacsonyabb SP1<br/><br/> VMs meg kell felelnie a [Azure követelményeknek](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**VMware VMs** | Minden futó Linux virtuális terhelést | Azure | Piros kalap vállalati Linux 6,7, 7.1, 7.2. <br/><br/> Centos 6.5 6.6 6,7, 7.0-s, 7.1, 7.2. <br/><br/> Az Oracle vállalati Linux 6.4 6.5 rendszeren futó piros kalap kompatibilis kernel vagy szoros vállalati megjelenés 3.kernelfrissítés (UEK3) <br/><br/> SUSE Linux vállalati kiszolgáló 11 SP3 <br/><br/> Tárhely szükséges: File system (EXT3, ETX4, ReiserFS, XFS); Többutas szoftver-eszköz hozzárendelést (többutas)); Mennyiségi manager: (LVM2). Fizikai kiszolgálók HP CCISS vezérlő adathordozós nem támogatottak. A ReiserFS formázáshoz csak a SUSE Linux vállalati kiszolgáló 11 SP3 rendszeren támogatott.<br/><br/> VMs meg kell felelnie a [Azure követelményeknek](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**VMware VMs** | Minden futó Windows virtuális terhelést | Másodlagos VMware webhely | 64 bites Windows Server 2012 R2, a Windows Server 2012-ben, a Windows Server 2008 R2 a legalacsonyabb SP1
**VMware VMs** | Minden futó Linux virtuális terhelést | Másodlagos VMware webhely | Piros kalap vállalati Linux 6,7, 7.1, 7.2. <br/><br/> Centos 6.5 6.6 6,7, 7.0-s, 7.1, 7.2. <br/><br/> Az Oracle vállalati Linux 6.4 6.5 rendszeren futó piros kalap kompatibilis kernel vagy szoros vállalati megjelenés 3.kernelfrissítés (UEK3) <br/><br/> SUSE Linux vállalati kiszolgáló 11 SP3 <br/><br/> Tárhely szükséges: File system (EXT3, ETX4, ReiserFS, XFS); Többutas szoftver-eszköz hozzárendelést (többutas)); Mennyiségi manager: (LVM2). Fizikai kiszolgálók HP CCISS vezérlő adathordozós nem támogatottak. A ReiserFS formázáshoz csak a SUSE Linux vállalati kiszolgáló 11 SP3 rendszeren támogatott.
**Fizikai kiszolgálók** | Bármely, a Windows rendszeren futó terhelést | Azure | 64 bites Windows Server 2012 R2, a Windows Server 2012-ben, a Windows Server 2008 és a legalacsonyabb SP1
**Fizikai kiszolgálók** | Minden futó Linux terhelést | Azure | Piros kalap vállalati Linux 6.7,7.1,7.2 <br/><br/> Centos 6.5, 6.6,6.7,7.0,7.1,7.2 <br/><br/> Az Oracle vállalati Linux 6.4 6.5 rendszeren futó piros kalap kompatibilis kernel vagy szoros vállalati megjelenés 3.kernelfrissítés (UEK3) <br/><br/> SUSE Linux vállalati kiszolgáló 11 SP3 <br/><br/> Tárhely szükséges: File system (EXT3, ETX4, ReiserFS, XFS); Többutas szoftver-eszköz hozzárendelést (többutas)); Mennyiségi manager: (LVM2). Fizikai kiszolgálók HP CCISS vezérlő adathordozós nem támogatottak. A ReiserFS formázáshoz csak a SUSE Linux vállalati kiszolgáló 11 SP3 rendszeren támogatott.
**Fizikai kiszolgálók** | Bármely, a Windows rendszeren futó terhelést | Másodlagos webhely | 64 bites Windows Server 2012 R2, a Windows Server 2012-ben, a Windows Server 2008 és a legalacsonyabb SP1
**Fizikai kiszolgálók** | Minden futó Linux terhelést | Másodlagos webhely | Piros kalap vállalati Linux 6.7,7.1,7.2 <br/><br/> Centos 6.5, 6.6,6.7,7.0,7.1,7.2 <br/><br/> Az Oracle vállalati Linux 6.4 6.5 rendszeren futó piros kalap kompatibilis kernel vagy szoros vállalati megjelenés 3.kernelfrissítés (UEK3) <br/><br/> SUSE Linux vállalati kiszolgáló 11 SP3 <br/><br/> Tárhely szükséges: File system (EXT3, ETX4, ReiserFS, XFS); Többutas szoftver-eszköz hozzárendelést (többutas)); Mennyiségi manager: (LVM2). Fizikai kiszolgálók HP CCISS vezérlő adathordozós nem támogatottak. A ReiserFS formázáshoz csak a SUSE Linux vállalati kiszolgáló 11 SP3 rendszeren támogatott.


## <a name="provider-versions"></a>Szolgáltató verziók

**név** | **Leírás** | **Legújabb verziója** | **Támogatás** | **Részletek**
---|---|---|---| ---
**Azure webhely helyreállítási szolgáltató** | A helyszíni kiszolgálók és Azure/másodlagos webhely közötti kommunikáció koordináták <br/><br/> Ha nincs VMM kiszolgáló helyszíni VMM kiszolgálót vagy a Hyper-V telepítve | 5.1.1700 (portálról érhető el) | [Legújabb funkciókat és megoldásai](https://support.microsoft.com/kb/3155002)
**Azure webhely helyreállítási egyesített beállítása (az Azure VMware)** | A helyszíni VMware kiszolgálók és Azure közötti kommunikáció koordináták <br/><br/> A helyszíni VMware kiszolgálókon telepítve | 9.3.4246.1 (portálról érhető el) | [Legújabb funkciókat és megoldásai](https://support.microsoft.com/kb/3155002)
**Mobilitás szolgáltatás** | Koordinátákat replikációs helyszíni VMware kiszolgálók/fizikai kiszolgálók és Azure/másodlagos webhely között | HIÁNYZIK (portálról érhető el) | Telepítve van egyes VMware virtuális vagy a fizikai kiszolgálóhoz való replikáció, amelyet a. **Microsoft Azure helyreállítási szolgáltatások (MARS) agent** | Koordinátákat replikációs a Hyper-V VMs és Azure között<br/><br/> Telepítve van a helyszíni a Hyper-V servers (vagy anélkül VMM kiszolgáló) | 2.0.8689.0 (portálról érhető el) | Ez az ügynök által használt Azure webhely helyreállítási és Azure biztonsági másolat). [Kapcsolatos további] (https://support.microsoft.com/en-us/kb/2997692)

## <a name="next-steps"></a>Következő lépések

[Felkészülés a telepítéshez](site-recovery-best-practices.md)


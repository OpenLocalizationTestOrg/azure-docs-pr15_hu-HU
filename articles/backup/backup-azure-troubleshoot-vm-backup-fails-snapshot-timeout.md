<properties
   pageTitle="Sikertelen az Azure virtuális biztonsági másolat: nem lehet kommunikálni a virtuális agent pillanatkép állapot - pillanatfelvétel virtuális sub tevékenység időtúllépési |} Microsoft Azure"
   description="Jelenségeket okok és megoldások az Azure virtuális kapcsolatos biztonsági hibák nem tudott kommunikálni a virtuális agent pillanatkép állapotát. Pillanatkép virtuális sub tevékenység időtúllépési hiba"
   services="backup"
   documentationCenter=""
   authors="genlin"
   manager="cfreeman"
   editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jimpark; markgal;genli"/>

# <a name="azure-vm-backup-fails-could-not-communicate-with-the-vm-agent-for-snapshot-status---snapshot-vm-sub-task-timed-out"></a>Sikertelen az Azure virtuális biztonsági másolat: nem sikerült kommunikálni a virtuális agent pillanatkép állapot - pillanatfelvétel virtuális sub tevékenység időtúllépési

## <a name="summary"></a>Összefoglalás

Után regisztrálása, és az ütemezés biztonsági Azure virtuális gép (virtuális), a biztonsági másolat Azure szolgáltatás kezdeményez a biztonsági mentési feladat az ütemezett időpontban által a virtuális pont és az idő pillanatkép állapotba biztonsági bővítmény kommunikáció. Vannak olyan feltételek szerint is megakadályozhatja a pillanatkép indított, amely biztonsági hiba vezet. Ez a cikk hibaelhárítási lépéseit Azure virtuális pillanatkép időtúllépése hiba kapcsolatos biztonsági hibák kapcsolatos problémák esetében.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a>A jelenség

Microsoft Azure biztonsági másolat szolgáltatásként (IaaS) virtuális infrastruktúra meghiúsul, adja eredményül a következő hibaüzenet a feladat részletek az [Azure portálon](https://portal.azure.com/):

**Nem tudott kommunikálni a virtuális agent pillanatkép állapot - pillanatfelvétel virtuális sub tevékenység időtúllépési.**

## <a name="cause"></a>OK
Ehhez a hibához négy gyakori oka is van:

- A virtuális nincs internetkapcsolata.
- A Microsoft Azure virtuális agent a virtuális az elavult (a Linux VMs).
- A biztonsági másolat bővítmény nem sikerül frissíteni vagy betöltése.
- Nem lehet beolvasni a pillanatképek állapotát, vagy a pillanatképek nem használhatók.

## <a name="cause-1-the-vm-does-not-have-internet-access"></a>OK 1: A virtuális nincs internetkapcsolata
A telepítési követelmény a virtuális nem Internet fér hozzá, vagy, amelyek megakadályozzák a hozzáférést az Azure infrastruktúrára vonatkozó helyen.

A biztonsági másolat meghosszabbításához connectivity az Azure nyilvános IP-címek megfelelően működnek. Ennek az oka parancsokat küld Azure tároló zárólap (HTTP URL-CÍMÉT) az elérhető információk, amelyek a virtuális kezelése. Ha a bővítmény nincs nyilvános Internet-hozzáféréssel, a biztonsági mentés végül sikertelen lesz.

### <a name="solution"></a>Megoldás
Ebben az esetben használja az alábbi lehetőségek közül a probléma megoldásához:

- Whitelist az Azure adatközpont IP-tartományok
- HTTP forgalmat mozgásvonal létrehozása

### <a name="to-whitelist-the-azure-datacenter-ip-ranges"></a>Az Azure whitelist az Adatközpont IP-tartományok

1. Szerezze be az [Azure adatközpont IP-címei listája](https://www.microsoft.com/download/details.aspx?id=41653) whitelisted.
2. Tiltásának feloldása az IP-címei az új-NetRoute parancsmag használatával. Ezzel a parancsmaggal futtassa a Azure virtuális, emelt PowerShell ablakban (Futtatás rendszergazdaként).
3. Szabályok felvétele a hálózati biztonsági csoport (NSG) Ha több az IP-címei való hozzáférés engedélyezése.

### <a name="to-create-a-path-for-http-traffic-to-flow"></a>Létrehoz egy mozgásvonalat HTTP forgalmat a

1. Ha a hálózati korlátozások már a helyükön (például NSG), üzembe a forgalmat a HTTP-proxy kiszolgálón.
2. Ha hálózati biztonsági csoport (NSG), a hozzáférés engedélyezése az Internet a HTTP-proxy szabályok felvétele.

Megtudhatja, hogy miként [állíthat be egy HTTP-proxy virtuális biztonsági másolatok](backup-azure-vms-prepare.md#using-an-http-proxy-for-vm-backups).

## <a name="cause-2-the-microsoft-azure-vm-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>2 okai lehetnek: A Microsoft Azure virtuális agent a virtuális az elavult (a Linux VMs)

### <a name="solution"></a>Megoldás
Leggyakrabban ügynök vagy kiterjesztés kapcsolódó hibák Linux VMs a régi virtuális ügynökszoftvert befolyásoló problémákat okozza. Az általános útmutató, mint az első lépések a probléma megoldásához a következők:

1. [Telepítse az Azure virtuális agent legújabb verzióját](https://github.com/Azure/WALinuxAgent).
2. Győződjön meg arról, hogy az Azure ügynök fut-e meg a virtuális. Ehhez a következő parancsot:```ps -e```

    Ha ez a folyamat nem fut, indítsa újra az alábbi parancsok használatával.

    A Ubuntu:```service walinuxagent start```

    Az egyéb terjesztését: "" waagent start szolgáltatás
```

3. [Configure the auto restart agent](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).

4. Run a new test backup. If the failures persist, please collect logs from the following folders for further debugging.

    We require the following logs from the customer’s VM:

    - /var/lib/waagent/*.xml
    - /var/log/waagent.log
    - /var/log/azure/*

If we require verbose logging for waagent, follow these steps to enable this:

1. In the /etc/waagent.conf file, locate the following line:

    Enable verbose logging (y|n)

2. Change the **Logs.Verbose** value from n to y.
3. Save the change, and then restart waagent by following the previous steps in this section.

## Cause 3: The backup extension fails to update or load
If extensions cannot be loaded, then Backup fails because a snapshot cannot be taken.

### Solution
For Windows guests:

1. Verify that the iaasvmprovider service is enabled and has a startup type of automatic.
2. If this is not the configuration, enable the service to determine whether the next backup succeeds.

For Linux guests:

The latest version of VMSnapshot Linux (extension used by backup) is 1.0.91.0

If the backup extension still fails to update or load, you can force the VMSnapshot extension to be reloaded by uninstalling the extension. The next backup attempt will reload the extension.

### To uninstall the extension

1. Go to the [Azure portal](https://portal.azure.com/).
2. Locate the particular VM that has backup problems.
3. Click **Settings**.
4. Click **Extensions**.
5. Click **Vmsnapshot Extension**.
6. Click **uninstall**.

This will cause the extension to be reinstalled during the next backup.

## Cause 4: The snapshots status cannot be retrieved or the snapshots cannot be taken
VM backup relies on issuing snapshot command to underlying storage. The backup can fail because it has no access to storage or because of a delay in snapshot task execution.

### Solution
The following conditions can cause snapshot task failure:

| Cause | Solution |
| ----- | ----- |
| VMs that have Microsoft SQL Server Backup configured. By default, VM Backup runs a VSS Full backup on Windows VMs. On VMs that are running SQL Server-based servers and on which SQL Server Backup is configured, snapshot execution delays may occur. | Set following registry key if you are experiencing backup failures because of snapshot issues.<br><br>[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT] "USEVSSCOPYBACKUP"="TRUE" |
| VM status is reported incorrectly because the VM is shut down in RDP. If you shut down the virtual machine in RDP, check the portal to determine whether that VM status is reflected correctly. | If it’s not, shut down the VM in the portal by using the ”Shutdown” option in the VM dashboard. |
| Many VMs from the same cloud service are configured to back up at the same time. | It’s a best practice to spread out the VMs from the same cloud service to have different backup schedules. |
| The VM is running at high CPU or memory usage. | If the VM is running at high CPU usage (more than 90 percent) or high memory usage, the snapshot task is queued and delayed and eventually times out. In this situation, try on-demand backup. |
|The VM cannot get host/fabric address from DHCP.|DHCP must be enabled inside the guest for IaaS VM Backup to work.  If the VM cannot get host/fabric address from DHCP response 245, then it cannot download ir run any extensions. If you need a static private IP, you should configure it through the platform. The DHCP option inside the VM should be left enabled. View more information about [Setting a Static Internal Private IP](../virtual-network/virtual-networks-reserved-private-ip.md).|

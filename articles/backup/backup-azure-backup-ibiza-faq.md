<properties
   pageTitle="Helyreállítási szolgáltatások tárolóra gyakran ismételt kérdések |} Microsoft Azure"
   description="Ezzel az alkalmazással kapcsolatos Gyakori verzióval az Azure biztonsági másolat szolgáltatás a nyilvános előzetes verzióját. Gyakori kérdések a biztonsági másolat agent, biztonsági mentése és adatmegőrzési, helyreállítási, biztonság és a biztonsági másolat Azure megoldást egyéb gyakori kérdésekre adott válaszok."
   services="backup"
   documentationCenter=""
   authors="markgalioto"
   manager="jwhit"
   editor=""
   keywords="biztonsági másolat megoldást. biztonsági másolat szolgáltatás"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="10/21/2016"
     ms.author="trinadhk; markgal; jimpark;"/>

# <a name="recovery-services-vault---faq"></a>Helyreállítási szolgáltatások tárolóra – gyakori kérdések


Ebben a cikkben információkat helyreállítási szolgáltatások tárolóból elemre, és akkor egészíti ki az [Azure biztonsági másolat – gyakori kérdések](backup-azure-backup-faq.md). Az Azure biztonsági másolatot a témakör a kérdések és válaszok az Azure biztonsági másolat szolgáltatásról a skype_for_businesshoz.  

A kérdéseket Azure névjegy Ez a cikk vagy egy kapcsolódó témakör Disqus szakaszában. Az Azure biztonsági másolat szolgáltatás kapcsolatos kérdések [vitafórum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup)is küldhet.

## <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>Helyreállítási szolgáltatások tárolókban alapú erőforrás-kezelő. Biztonsági másolat tárolókban (klasszikus mód) továbbra is használhatók? <br/>
Igen, biztonsági mentése tárolókban továbbra is használhatók. Biztonsági másolat tárolókban létrehozása a [Klasszikus portálon](https://manage.windowsazure.com). Helyreállítási szolgáltatások tárolókban létrehozása az [Azure-portálon](https://portal.azure.com). Azonban azt ajánljuk, hogy létrehozása a helyreállítási szolgáltatások tárolóból elemre, minden jövőbeni kiemelés érhetők el csak helyreállítási szolgáltatások tárolóból elemre.

## <a name="can-i-migrate-a-backup-vault-to-a-recovery-services-vault-br"></a>Telepítheti a biztonsági másolat tárolóból elemre is át a helyreállítási szolgáltatások tárolóból elemre az? <br/>
Sajnos nem, egyelőre nem egy biztonsági másolat tárolóból elemre a tartalmát való áttelepítése a helyreállítási szolgáltatások tárolóból elemre. Dolgozunk a Hozzáadás, ezt a funkciót, de nem érhető el a nyilvános előzetes verzió részeként.

## <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Támogatják helyreállítási szolgáltatások tárolókban klasszikus VMs, vagy az erőforrás-kezelő alapú VMs? <br/>
Helyreállítási szolgáltatások tárolókban mindkét modellek támogatja.  Készíthet biztonsági másolatot a klasszikus portálon (amelyek Klasszikus módú VMs) létrehozott egy virtuális, vagy egy virtuális létrehozott az Azure-portálon (amelyek alapú erőforrás-kezelő) a helyreállítási szolgáltatások tárolóból elemre.

## <a name="i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-to-migrate-my-vms-from-classic-mode-to-resource-manager-mode--how-can-i-backup-them-in-recovery-services-vault"></a>E biztonsági másolatban a klasszikus VMs a biztonsági másolat tárolóból elemre. Most már lehet át szeretne térni a VMs a Klasszikus módú erőforrás-kezelő módra.  Hogyan lehet biztonsági a őket a helyreállítási szolgáltatások tárolóra?
Biztonsági másolat tárolóból elemre a klasszikus VMs biztonsági másolatait nem áttelepítése az automatikus helyreállítási szolgáltatások tárolóból elemre frissítéskor a VMs klasszikus az erőforrás-kezelő módra. Hajtsa végre ezeket a lépéseket a virtuális biztonsági másolatok áttelepítését:

1. A biztonsági másolat tárolóból elemre nyissa meg a **Védett elemek** fülre, és válassza ki a virtuális. Kattintson a [Dokumentumvédelem kikapcsolása](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). Kilépés a *társított biztonsági adatok törlése* beállítást **bejelölve**.
2. A virtuális gép áttelepítése Klasszikus módú erőforrás-kezelő módra. Győződjön meg arról, hogy a tárhely és a megfelelő virtuális gép hálózati is áttérni erőforrás-kezelő módban.
3. A helyreállítási szolgáltatások tárolóból elemre létrehozása és konfigurálása a biztonsági másolat az áttelepített virtuális gépen **biztonsági** művelettel fölött irányítópult tárolóból elemre. További információ a [helyreállítási szolgáltatások tárolóból elemre a biztonsági másolat](backup-azure-vms-first-look-arm.md) engedélyezése

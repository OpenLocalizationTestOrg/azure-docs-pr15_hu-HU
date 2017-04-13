<properties
   pageTitle="Az Azure biztonság otthon lemez titkosítási alkalmazása |} Microsoft Azure"
   description="A dokumentum bemutatja, hogyan az Azure biztonság otthon ajánlást **Alkalmaz lemez titkosítási**végrehajtásához."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="apply-disk-encryption-in-azure-security-center"></a>Az Azure biztonság otthon lemez titkosítási alkalmazása

Azure biztonság otthon javasol lemez titkosítási alkalmazása esetén nem védett Azure lemez titkosítással Windows vagy Linux virtuális lemezt. A Windows és Linux IaaS virtuális merevlemezeken titkosítása lemez titkosítás segítségével.  Az operációs rendszer és az adatok kötet a virtuális a titkosítási ajánlott.


Lemezen titkosítási a iparági szabvány szerinti szabványos [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) funkció a Windows és a Linux operációs rendszer és az adatok védelme és az adatok védelme és a szervezeti biztonság és megfelelőség kötelezettségek felel meg titkosítást megadására [A titkosítási DM](https://en.wikipedia.org/wiki/Dm-crypt) funkciót használja. Lemez titkosítási integrálva van szabályozni, és a lemez titkosítási kulcs és titkos kulcsok kulcs tárolóból elemre az előfizetésben, biztosítva, hogy az összes adat a virtuális lemez titkosított a többi az [Azure-tároló](https://azure.microsoft.com/documentation/services/storage/)kezelése [Azure kulcs tárolóból elemre](https://azure.microsoft.com/documentation/services/key-vault/) .

> [AZURE.NOTE] Az alábbi Windows kiszolgálói operációs rendszereken – a Windows Server 2008 R2, a Windows Server 2012-ben és a Windows Server 2012 R2 Azure lemez titkosítási támogatott. A következő Linux kiszolgálói operációs rendszerek - Ubuntu, CentOS, SUSE és SUSE Linux vállalati kiszolgáló (SLES) lemez titkosítás esetén támogatott.

## <a name="implement-the-recommendation"></a>Az ajánlási megvalósítása

1. Jelölje ki a **javaslatok** lap **Alkalmaz lemez titkosítást**.
2. Az **alkalmazás lemez titkosítás** lap, amelynek a lemez titkosítási ajánlott VMs listáját fogja látni.
3. Kövesse az utasításokat, ezek VMs titkosítási alkalmazni.

![][1]

Biztonság otthon, erre szolgáló titkosító azonosított Azure virtuális gépeken futó titkosítására, azt javasoljuk az alábbi lépéseket:

- Telepítse és állítsa be a Azure PowerShell. Ez lehetővé teszi, hogy az Azure virtuális gépeken futó titkosítása előfeltételei beállításához szükséges PowerShell-parancsokat futtathat.
- Szerezze be, és futtassa az Azure lemez titkosítási Előfeltételek Azure PowerShell parancsprogramot.
- Titkosítsa a virtuális gépeken futó.

[Az Azure virtuális gép titkosítása](security-center-disk-encryption.md) azt ismerteti, hogy a fenti lépéseket.  Ez a témakör tartalma feltételezi, hogy az ügyfélgép, amelyből lemez titkosítási fog konfigurálni, használja a Windows 10.

Vannak olyan sok az alábbi módszerek beállítása a Előfeltételek és titkosítás az Azure virtuális gépeken futó konfigurálása használható. Ha már jól versed Azure PowerShell vagy Azure CLI, majd, előfordulhat, hogy előnyben részesíti az alternatív módszer. Ha többet szeretne tudni, hogy ezek a más módszer [Azure lemez titkosítás](../security/azure-security-disk-encryption.md)témakörben talál.



## <a name="see-also"></a>Lásd még:

A dokumentum mutatott megvalósításáról a biztonság otthon ajánlási "Alkalmaz lemez titkosítás." Többet szeretne tudni a lemez titkosítás, olvassa el az alábbiakat:

- [Titkosítás és Azure kulcs tárolóból elemre a kulcskezelő](https://azure.microsoft.com/documentation/videos/azurecon-2015-encryption-and-key-management-with-azure-key-vault/) (videó-és 36 min 39 mp) – védelme és az adatok védelme érdekében használata a lemez titkosítási management IaaS VMs és Azure kulcs tárolóból elemre.
- [Az Azure virtuális gép titkosítása](security-center-disk-encryption.md) (dokumentum) – megtudhatja, hogy miként titkosíthatja az Azure virtuális gépeken futó.
- [Azure lemez titkosítás:](../security/azure-security-disk-encryption.md) (dokumentum) – útmutató ahhoz, hogy a Windows és Linux VMs lemez titkosítás.

Többet szeretne tudni a biztonság otthon, olvassa el az alábbiakat:

- [Az Azure biztonság otthon biztonsági házirendek beállítása](security-center-policies.md) – megtudhatja, hogy miként biztonsági házirendek beállítása.
- [Biztonsági állapot ellenőrzése Azure biztonság otthon](security-center-monitoring.md) – útmutató a Lync-állapota az Azure erőforrások.
- [Kezelése és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md) – megtudhatja, hogy miként kezelheti, és a biztonsági figyelmeztetések válaszolni.
- [Kezelése biztonsági ajánlások az Azure biztonság otthon](security-center-recommendations.md) névre hallgat – ismerje meg, hogyan javaslatok segítenek az Azure erőforrások védelme.
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md) – keresés gyakran ismételt kérdések a szolgáltatással.
- [Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – keresés blog bejegyzések Azure biztonság és megfelelőség kapcsolatban.



<!--Image references-->
[1]: ./media/security-center-apply-disk-encryption/apply-disk-encryption.png

<properties
   pageTitle="Azure biztonság otthon OS biztonsági ismételt |} Microsoft Azure"
   description="A dokumentum megtekintheti az Azure biztonság otthon ajánlást **ismételt OS biztonsági**megvalósításáról."
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
   ms.date="10/17/2016"
   ms.author="terrylan"/>

# <a name="remediate-os-vulnerabilities-in-azure-security-center"></a>Azure biztonság otthon OS biztonsági ismételt

Azure biztonság otthon elemzi naponta a virtuális gép (virtuális) operációs rendszeren konfigurációk, amely a virtuális támadásokkal szemben teheti, és ezek biztonsági cím konfiguráció módosításainak javasolja. Az adott konfigurációk figyelni a további tudnivalókért lásd: az [ajánlott konfiguráció a szabályok listáját](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Biztonság otthon javasolja biztonsági megoldani, ha a virtuális operációs rendszer konfigurációja nem egyezik meg az ajánlott konfiguráció a szabályokat.

> [AZURE.NOTE] A dokumentum egy példa telepítési használatával vezet be a szolgáltatást.  Ez a cikk részletesen ismerteti nem.

## <a name="implement-the-recommendation"></a>Az ajánlási megvalósítása

1. Jelölje ki a **javaslatok** lap **ismételt OS biztonsági**.
![Operációs rendszer biztonsági ismételt][1]

    Az **ismételt OS biztonsági** lap megnyílik, és megjeleníti a VMs OS konfigurációjának, amelyek nem felelnek meg a ajánlott konfiguráció a szabályokat.  Minden egyes virtuális a lap sorolja fel:

   - **Sikertelen szabályok** –, amely a virtuális OS konfigurálása sikertelen szabályok számát.
   - **Utolsó beolvasása idő** – a dátum és idő, hogy a biztonság otthon utolsó beolvasott a virtuális operációs rendszer konfigurációja.
   - **Állapot** – a biztonsági aktuális állapotát:

        - Nyitott: A biztonsági által nem javított még
        - Folyamatban: A biztonsági az aktuálisan alkalmazott, nincs további teendő van szüksége
        - A rendszer: A biztonsági volt, amelyekkel már foglalkozott (Ha a probléma megoldódott, a bejegyzés szürkén jelenik meg)
  - **SÚLYOSSÁGÁT** – minden biztonsági vannak beállítva, hogy egy súlyosságát alacsony, ami azt jelenti, egy biztonsági kell foglalkozni, de nem azonnali figyelmet igénylő.

Jelölje ki a virtuális. A lap adott virtuális megnyílik, és megjeleníti a szabályokat, amelyek nem sikerült.
   ![Konfigurációs szabályok, amelyek nem sikerült][2]

Jelöljön ki egy szabályt. Ebben a példában lehetővé teszi, jelölje be a **jelszót kell a összetettsége követelményeknek**. Egy lap nyílik meg, hogy a sikertelen szabály, és milyen következményekkel leíró. Tekintse át a részleteket, és fontolja meg, hogyan operációs rendszer konfigurációk fog vonatkozni.
  ![A sikertelen szabály leírása][3]

  Biztonság otthon használja a közös konfigurációs számbavétel (CCE) egyedi azonosítókat konfigurációs szabályok hozzárendelése. Ez a lap a elérhetők az alábbi adatokat:

  - NEVE – Szabály neve
  - SÚLYOSSÁGÁT – Kritikus, fontos vagy figyelmeztető CCE súlyosságát értéke
  - CCIED – CCE egyedi azonosító a szabály
  - LEÍRÁSÁNAK – A szabály leírása
  - BIZTONSÁGI – Biztonsági vagy kockázat, ha a program nem állítja a szabály magyarázata
  - HATÁSA – Szabály lép érvénybe üzleti gyakorolt hatás
  - VÁRT érték – Ha a biztonság otthon elemzi a virtuális operációs rendszer konfigurációja, a szabály alapján várható érték
  - SZABÁLYOK művelet – A virtuális operációs rendszer konfigurációja, a szabály alapján-elemzés során biztonság otthon által használt szabály művelet
  - TÉNYLEGES érték--Után a virtuális operációs rendszer konfigurációja, a szabály alapján elemzésének visszaadott érték
  - ÉRTÉKELÉS eredménye –-elemzés eredménye: fázis, Fail:


## <a name="see-also"></a>Lásd még:

Ez a cikk mutatott megvalósításáról a biztonság otthon ajánlási "Remediate OS biztonsági." Konfigurációs szabálykészlet áttekintheti [az alábbi](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Biztonság otthon konfigurációs szabályok egyedi azonosítókat hozzárendeléséhez CCE (közös konfigurációs számbavétel) használ. [CCE](http://cce.mitre.org) webhelyén tájékozódhat.

Többet szeretne tudni a biztonság otthon, olvassa el az alábbiakat:

- [Az Azure biztonság otthon biztonsági házirendek beállítása](security-center-policies.md) – a Azure előfizetések és erőforrás-csoportok biztonsági házirendek konfigurálásának ismertetése.
- [Kezelése biztonsági ajánlások az Azure biztonság otthon](security-center-recommendations.md) névre hallgat – ismerje meg, hogyan javaslatok segítenek az Azure erőforrások védelme.
- [Biztonsági állapot ellenőrzése Azure biztonság otthon](security-center-monitoring.md) – útmutató a Lync-állapota az Azure erőforrások.
- [Kezelése és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md) – megtudhatja, hogy miként kezelheti, és a biztonsági figyelmeztetések válaszolni.
- [Figyelés partnermegoldások Azure biztonsági központ](security-center-partner-solutions.md) – megtudhatja, hogy miként figyelheti a partnerek megoldásaival állapotának.
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md) – keresés gyakran ismételt kérdések a szolgáltatással.
- [Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – keresés blog bejegyzések Azure biztonság és megfelelőség kapcsolatban.

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/recommendation.png
[2]:./media/security-center-remediate-os-vulnerabilities/vm-remediate-os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png

<properties
   pageTitle="Adja meg a biztonsági kapcsolattartási adatai a Azure biztonság otthon |} Microsoft Azure"
   description="A dokumentum megtudhatja, hogy hogyan adja meg a biztonsági kapcsolattartási adatai a Azure biztonsági központban."
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

# <a name="provide-security-contact-details-in-azure-security-center"></a>Adja meg a biztonsági kapcsolattartási adatai a Azure biztonság otthon

Azure biztonság otthon fog ajánljuk, hogy kapcsolattartási adatai biztonsági Azure-előfizetéséhez, ha még nem tette meg. Ez az információ használandó Microsoft Önnel, ha a Microsoft biztonsági válasz központ (Kialakításában) találja, hogy az ügyfél adatait egy jogellenes vagy jogosulatlan fél által elérhető-e. Kialakításában elvégzi a választó biztonsági Azure hálózati és infrastruktúra figyelemmel kísérését, és veszélyt intelligencia és visszaélés panaszok kap külső féltől származó.

E-mailben értesítést a az első napi előfordulást, egy figyelmeztetés, és csak magas súlyosságát riasztások küldi. E-mail beállítások csak az előfizetés házirendek állítható be. Az erőforrás-csoportok belül előfizetés örökli ezeket a beállításokat.

> [AZURE.NOTE] A dokumentum egy példa telepítési használatával vezet be a szolgáltatást.  Ez a cikk részletesen ismerteti nem.

## <a name="implement-the-recommendation"></a>Az ajánlási megvalósítása

1. Jelölje ki a **javaslatok** lap **Provide biztonsági felelős adatai**.
![Adja meg a biztonsági partner][1]

2. Ekkor megnyílik a **kapcsolattartási adatok Provide biztonsági**lap. Válassza ki a kapcsolattartási adatok megadására a Azure előfizetést.
![Adja meg a biztonsági kapcsolattartási adatai][2]

3. A második **Adja meg a biztonsági kapcsolattartási adatai** a lap megnyitása

  - Adja meg a biztonsági kapcsolatfelvételi e-mail cím vagy a vesszővel elválasztott címét. Nem szerepel a adhatja meg az e-mail címében száma korlátozott.
  - Írjon be egy biztonsági nemzetközi telefonszám számot.
  - Szeretne kapni a magas súlyosságát folyamatának követése e-mailek, a beállítás **küldése e-mailek kapcsolatos értesítések**bekapcsolása.
  - A jövőben a vezérlőt, amellyel az értesítő e-mailek küldése az előfizetés-tulajdonosok lesz. Ez a beállítás jelenleg szürkén jelenik meg.
  - Válassza az **OK** gombra a biztonsági partneradatok alkalmazása az előfizetésre.

## <a name="see-also"></a>Lásd még:

Többet szeretne tudni a biztonság otthon, olvassa el az alábbiakat:

- [Az Azure biztonság otthon biztonsági házirendek beállítása](security-center-policies.md) – a Azure előfizetések és erőforrás-csoportok biztonsági házirendek konfigurálásának ismertetése.
- [Kezelése biztonsági ajánlások az Azure biztonság otthon](security-center-recommendations.md) névre hallgat – ismerje meg, hogyan javaslatok segítenek az Azure erőforrások védelme.
- [Biztonsági állapot ellenőrzése Azure biztonság otthon](security-center-monitoring.md) – útmutató a Lync-állapota az Azure erőforrások.
- [Kezelése és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md) – megtudhatja, hogy miként kezelheti, és a biztonsági figyelmeztetések válaszolni.
- [Figyelés partnermegoldások Azure biztonsági központ](security-center-partner-solutions.md) – megtudhatja, hogy miként figyelheti a partnerek megoldásaival állapotának.
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md) – keresés gyakran ismételt kérdések a szolgáltatással.
- [Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – legújabb Azure biztonsági híreket és információkat.

<!--Image references-->
[1]: ./media/security-center-provide-security-contacts/provide-contacts.png
[2]:./media/security-center-provide-security-contacts/provide-contact-details.png

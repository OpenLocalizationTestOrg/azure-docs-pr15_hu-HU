<properties
   pageTitle="A webes alkalmazás tűzfal felvétele az Azure biztonság otthon |} Microsoft Azure"
   description="A dokumentum megtekintheti az Azure biztonság otthon ajánlások **hozzáadása a webes alkalmazás tűzfal** és **Véglegesítés alkalmazás védelmének**megvalósításáról."
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

# <a name="add-a-web-application-firewall-in-azure-security-center"></a>A webes alkalmazás tűzfal felvétele az Azure biztonság otthon

Azure biztonság otthon előfordulhat, hogy ajánlott felvenni a webes alkalmazás tűzfal (WAF) Microsoft partnertől annak érdekében, hogy a webalkalmazások biztonságos. A dokumentum megismerkedhet ehhez példa keresztül.

> [AZURE.NOTE] A dokumentum egy példa telepítési használatával vezet be a szolgáltatást.  Ez a cikk részletesen ismerteti nem.

## <a name="implement-the-recommendation"></a>Az ajánlási megvalósítása

1. A **javaslatok** a lap válassza a **webes alkalmazás tűzfalat használ a webes alkalmazás biztonságos**.
![Biztonságos webhely alkalmazás][1]

2. Válassza **a webes alkalmazás tűzfalat használ webalkalmazások biztonságos** lap webalkalmazás. A **webes alkalmazás tűzfalat hozzáadása** lap megnyitása
![A webes alkalmazás tűzfal hozzáadása][2]
3. Választhatja a meglévő webes alkalmazás tűzfal használata, ha elérhető, vagy hozhat létre egy újat. Ebben a példában érhetők el nem létező WAFs, azt egy új WAF létre fogja hozni.

4. Hozzon létre egy új WAF, jelölje be a megoldást az integrált partnerek listájából. Ebben a példában a **Barracuda webes alkalmazás tűzfal**kijelöli azt.
5. A **Barracuda webes alkalmazás tűzfal** lap megnyitása és, akkor a partner megoldást információt. Jelölje ki az adatok lap **létrehozása** .
![Tűzfal információ lap][3]

6. Az **Új webes alkalmazás tűzfal** lap jelenik meg, ahol végezze el a **Virtuális konfigurálási** lépéseket, és adja meg a **WAF információkat**. Válassza a **virtuális beállítást**.

7. A **Virtuális konfigurációs** lap meg kell adni a virtuális gép fog futni a WAF felfelé léptetőnyíl vezérlőelem szükséges információkat.
![Virtuális konfigurálása][4]
8. Térjen vissza a lap az **Új webes alkalmazás tűzfalat** , és válassza az **Információ WAF**. A **WAF információ** lap az állítsa be a WAF magát. Lépés 7 beállítása a virtuális gépen a WAF fog futni és 8 képessé válik, hogy magát a WAF kiépítése teszi lehetővé.

## <a name="finalize-application-protection"></a>Alkalmazás védelmének véglegesítése

1. Vissza a **javaslatok** lap. Új bejegyzés létrehozása után a WAF **Véglegesítés alkalmazás védelmének**nevű hozták létre. Ez a bejegyzés jelzi, hogy a kell, hogy azt megvédheti az alkalmazás mentése a WAF az Azure virtuális hálózaton belül ténylegesen huzalozási utolsó lépéseként.
![Alkalmazás védelmének véglegesítése][5]

2. Jelölje ki a **Véglegesítés alkalmazás védelmének**. Egy új lap nyílik meg. Látható, hogy van-e webalkalmazás átirányítva forgalmának kell szerepelnie.
3. Jelölje ki a webalkalmazást. Egy lap nyílik meg, amely lehetővé teszi a lépéseket az véglegesítése a webalkalmazás tűzfal beállítása. Hajtsa végre a lépéseket, és válassza a **korlátozott forgalmat**. Biztonság otthon a, majd a huzalozási rajzokhoz fel hajtsa végre.
![Adatforgalom korlátozása][6]

> [AZURE.NOTE] A biztonság otthon több webalkalmazások megvédheti ezeket az alkalmazásokat, a meglévő WAF környezetekben való hozzáadásával. (Az erőforrás-kezelő telepítési modell használatával létrehozott) WAF készülékek kell telepíthetők külön virtuális hálózathoz. (A klasszikus telepítési modell használatával létrehozott) WAF készülékek korlátozva hálózati biztonsági csoport használata. Ez a támogatás a jövőben kiterjesztik egy teljesen testre szabott példányhoz egy WAF készülék (klasszikus). További tudnivalók a [hagyományos és erőforrás-kezelő telepítési modellek](../azure-classic-rm.md) Azure erőforrások.

A naplók az adott WAF most teljes mértékben integrálva. Biztonság otthon elkezdheti automatikusan összegyűjti és elemzése a naplókat, úgy, hogy az Önnek fontos biztonsági figyelmeztetések elhelyezhetők benne.

## <a name="see-also"></a>Lásd még:

A dokumentum mutatott megvalósításáról a biztonság otthon ajánlási "Hozzáadása egy webalkalmazáshoz." Webes alkalmazás tűzfalat beállításával kapcsolatos további információért olvassa el az alábbiakat:

- [Az alkalmazás-szolgáltatási környezetben webes alkalmazás tűzfal (WAF) konfigurálása](../app-service-web/app-service-app-service-environment-web-application-firewall.md)

Többet szeretne tudni a biztonság otthon, olvassa el az alábbiakat:

- [Az Azure biztonság otthon biztonsági házirendek beállítása](security-center-policies.md) – a Azure előfizetések és erőforrás-csoportok biztonsági házirendek konfigurálásának ismertetése.
- [Biztonsági állapot ellenőrzése Azure biztonság otthon](security-center-monitoring.md) – útmutató a Lync-állapota az Azure erőforrások.
- [Kezelése és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md) – megtudhatja, hogy miként kezelheti, és a biztonsági figyelmeztetések válaszolni.
- [Kezelése biztonsági ajánlások az Azure biztonság otthon](security-center-recommendations.md) névre hallgat – ismerje meg, hogyan javaslatok segítenek az Azure erőforrások védelme.
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md) – keresés gyakran ismételt kérdések a szolgáltatással.
- [Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – keresés blog bejegyzések Azure biztonság és megfelelőség kapcsolatban.

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png

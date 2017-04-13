<properties
    pageTitle="Azure kulcs tárolóra Azure automatizálási használatával kezelése |} Microsoft Azure"
    description="További tudnivalók: hogyan az Azure automatizálási szolgáltatásai számára használható, kezelheti az Azure kulcs tárolóból elemre."
    services="Key-Vault, automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="magoedte;csand"/>

#<a name="managing-azure-key-vault-using-azure-automation"></a>Azure kulcs tárolóra Azure automatizálási használatával kezelése

Ez az útmutató fog megismerteti a Azure automatizálási szolgáltatásai számára, és hogyan használható billentyűparancsok és titkos kulcsok Azure kulcs tárolóból elemre az irányítás egyszerűsítése érdekében.

## <a name="what-is-azure-automation"></a>Mi az Azure automatizálási?

[Azure automatizálási](../automation/automation-intro.md) az Azure szolgáltatásainak egyszerűsítése a felhőbeli adatkezelési folyamat-automatizálása és a kívánt állapotot konfigurációs. Azure automatizálási kézi, használatával ismételni, hosszabb ideig futó és hiba jobban tevékenységek automatizálható növeléséhez megbízhatóság, a hatékonyság és a time to érték a szervezet számára.

Azure automatizálási tartalmaz egy nagymértékben megbízható, könnyen hozzáférhető munkafolyamat végrehajtása motor, amely az igényeknek méretezze át. Az Azure automatizálás folyamatok is lehet megrúgni manuálisan, 3rd külső rendszerek, illetve rendszeres időközönként, hogy a tevékenységek fordulhat elő, szükség esetén pontosan.

Csökkentse a terhelést műveleti és szabadítson fel informatikai és DevOps oktatói szűkítheti az üzleti értéknövelő áthelyezésével, a felhőbeli adatkezelési feladatok Azure automatizálási automatikusan futtatható munkát.


## <a name="how-can-azure-automation-help-manage-azure-key-vault"></a>Hogyan segíthet az Azure automatizálási Azure kulcs tárolóra kezelése?

Kulcs tárolóból elemre az Azure automatizálás kezelhető parancsmagokkal a [AzureRM kulcs tárolóra] (https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) és [parancsmagok Azure klasszikus kulcs tárolóból elemre](https://msdn.microsoft.com/library/azure/dn868052.aspx). A klasszikus kulcs tárolóra kezelésére szolgáló Azure modul automatikusan az Azure automatizálási érhető el, és importálhatja a [AzureRM-KeyVault modul](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) Azure automatizálás, hogy a kulcs tárolóból elemre felügyeleti feladatok belül a szolgáltatás számos végezhet. A parancsmagok az Azure automatizálás többi Azure szolgáltatás, összetett Azure szolgáltatásokat és a 3 külső rendszerek automatizálhatja a parancsmagokat párosítás is, is.

Az Azure kulcs tárolóra parancsmagokat többek között az alábbi műveleteket végezheti el: 

- Létrehozása és konfigurálása a fő tárolóból elemre
- Indítson vagy importáljon egy kulcsot
- Hozzon létre, vagy a titkos módosítása
- Egy kulcsot tulajdonságainak módosítása
- Billentyű vagy titkos kulcs
- Titkos kulcs vagy törlése

Íme néhány példa a kulcs tárolóból elemre kezelése a PowerShell használatá:  

* [Azure kulcs tárolóra - Step by Step](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step)
* [Beállításáról és konfigurálásáról az Azure kulcs tárolóból elemre](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault)


## <a name="next-steps"></a>Következő lépések

Most, hogy már megtanulta Azure automatizálási, és hogyan használható kezelése Azure kulcs tárolóból elemre a alapjait, kövesse ezeket a hivatkozásokat, ha többet szeretne tudni az Azure automatizálási.

* Lásd: az [első lépések oktatóanyag](../automation/automation-first-runbook-graphical.md)Azure automatizálási.
* Lásd: az [Azure kulcs tárolóra PowerShell-parancsfájlokat](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).

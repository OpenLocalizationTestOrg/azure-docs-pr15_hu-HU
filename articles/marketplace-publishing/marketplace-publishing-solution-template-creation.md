<properties
   pageTitle="Útmutató a megoldást sablon létrehozása a piactér |} Microsoft Azure"
   description="Részletes útmutatást, hogy miként hozhat létre, tanúsítása és üzembe többszörös-virtuális kép megoldás sablon vásárolja meg az Azure piactéren."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="07/27/2016"
      ms.author="hascipio; v-divte" />

# <a name="guide-to-create-a-solution-template-for-azure-marketplace"></a>Útmutató a megoldást sablon létrehozása a Microsoft Azure piactéren
Lépés: 1, [fiók létrehozása és a regisztrációs]befejezése után[link-acct-creation], azt folyamatát, az Azure-kompatibilis megoldás sablon [technikai Előfeltételek megoldás sablon létrehozása](marketplace-publishing-solution-template-creation-prerequisites.md)a létrehozásakor. Most fog végigvezetjük, megoldás sablon létrehozása a [Közzétételi portál] több VMs lépései[ link-pubportal] az Azure piactér.

## <a name="create-your-solution-template-offer-in-the-publishing-portal"></a>A megoldás sablon ajánlat létrehozása a közzétételi portál
Nyissa meg a [https://publish.windowsazure.com](http://publish.windowsazure.com). Bejelentkezés az első alkalommal a [Közzétételi portál](https://publish.windowsazure.com/)használatakor ugyanazt a fiókot, amellyel a vállalatprofil eladó regisztrált. Később bármelyik alkalmazottak a vállalat is hozzáadhat a közzétételi portál további-rendszergazdaként.

### <a name="1-select-solution-templates"></a>1. Válassza a "Megoldás sablonok"

  ![rajz][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a>2. a megoldás új sablon létrehozása

  ![rajz][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a>3. Kiindulás topológiák
Megoldás sablon "parent" az összes annak topológiák. Több topológiák egy ajánlat/megoldás sablonba határozhatja meg. Felajánlás átmeneti tolódik, amikor azt az összes annak topológiák tolódik. Hajtsa végre az alábbi lépéseket követve az ajánlat meghatározása:     

- Hozzon létre egy topológia: "Topológia azonosító" általában a topológia a megoldást sablon nevét. A topológia azonosító az URL-címet használja a alább látható módon:

  Azure piactér: http://azure.microsoft.com/marketplace/partners/ {PublisherNamespace} / {OfferIdentifier} {TopologyIdentifier}

  Azure portálon: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {TopologyIdentifier}

- Adja hozzá az új verziót.

### <a name="4-get-your-topology-versions-certified"></a>4. a tanúsítvánnyal topológia verzió beszerzése
Hozhatók létre, hogy a topológia adott verziója szükséges összes fájlt tartalmazó zip-fájl feltöltése. A zip-fájl kell tartalmaznia az alábbiak:

- a legfelső szintű címtár *mainTemplate.json* és *createUiDefinition.json* fájlt.
- Bármely csatolt sablonok és az összes szükséges parancsfájlokat.

  > [AZURE.TIP] A fejlesztők a megoldás létrehozása a sablon topológiák és az első őket tanúsítvánnyal, a vállalkozás és a munka közben is dolgozhat a vállalat marketingtevékenység és/vagy jogi részlegek a marketing- és jogi tartalmak.

## <a name="next-steps"></a>Következő lépések
Most, hogy a megoldás sablon létrehozása és a zip-fájl feltöltése, kérjük, kövesse a [piactér marketingtevékenység tartalmat végigvezetik](marketplace-publishing-push-to-staging.md) átmeneti ajánlat terjesztése előtt. Szeretné látni a piactér cikkek közzététele teljes készlete, látogasson el a [első lépések: a Microsoft Azure piactéren felajánlás közzététele](marketplace-publishing-getting-started.md).

Is érdekelheti e kapcsolódó cikkekben:

- Virtuális képek: [A virtuális gép képek Azure-ban](https://msdn.microsoft.com/library/azure/dn790290.aspx)
- Virtuális bővítmények: [virtuális ügynök virtuális bővítmények – áttekintés](https://msdn.microsoft.com/library/azure/dn832621.aspx) és [Azure virtuális Extensions és szolgáltatások](https://msdn.microsoft.com/library/azure/dn606311.aspx)
- Erőforrás-kezelő Azure: [Azure ARM sablonok létrehozása](../resource-group-authoring-templates.md) és [egyszerű ARM sablon példák](https://github.com/rjmax/ArmExamples)
- Tárterület-fiókja lehetővé: [az tároló fiók szabályozásának monitort hogyan](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) és [prémium tárhely](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage)

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com

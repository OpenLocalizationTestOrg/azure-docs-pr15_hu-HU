<properties
    pageTitle="Azure alkalmazás szolgáltatás: Alkalmazás szolgáltatásalkalmazások méretezése"
    description="Ismerje meg, hogy a másolat a méretezés alkalmazás App szolgáltatásban."
    keywords="alkalmazás, azure alkalmazás szolgáltatás, a méretarány méretezhető, alkalmazás szolgáltatáscsomagja, alkalmazás szolgáltatás költség"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="byvinyal"/>

# <a name="azure-app-service-scaling-app-service-applications"></a>Azure alkalmazás szolgáltatás: Alkalmazás szolgáltatásalkalmazások méretezése

Azure App szolgáltatásban tárolt alkalmazások is [tömeges méreteket elérése](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/).
Azonban alkalmazás méretezése nincs telepítve "egy mérete pontosan elférjen az összes" megoldást összetett probléma. Helyesen méretarányra van az alkalmazás hozzájárul ahhoz, hogy az alkalmazások sikeres 3 fő területen:

1. Az alkalmazás architektúra és annak gyengeségek ismertetése.
    * Az alkalmazás megbízható, szűrőként működő van? Állapot nélküli?
    * Melyek az alkalmazás minden összetevő?
        * Hol találhatók a szűk alkalmazásban?
    * Amikor a terhelést a alkalmazásba, mi oldaltörés első?
2. A várt betöltése és a teljesítmény követelményeinek ismertetése.
    * Az alkalmazás-nek ezresenként felhasználók szolgáló? vagy egymillió?
    * Egyetlen földrajzi helytől vagy globálisan érkezzenek forgalom?
    * Évszakos van? adatforgalom csúcsok?
    * Milyen gyorsan kell válaszoljon az alkalmazás? 1 másodperc? 1 ezredmásodperc?
3. Ismertetése, és a platform, az alkalmazás-annak megfelelően kihasználhatja.
    * Milyen szolgáltatások kell kihasználhatja saját méretarányra célok eléréséhez?

Ez a szakasz segít megérteni a tényezők és a Súgó, mivel ezek a stratégia, amely megnyitja az méretezhetőség célok eléréséhez szükséges alkalmazás szolgáltatás funkciói előnyt.

[AZURE.INCLUDE [app-service-blueprint-scaling-app-service-applications](../../includes/app-service-blueprint-scaling-app-service-applications.md)]

<properties
    pageTitle="Hogy miként telepítheti át a logika alkalmazások séma verzió 2015-08-01-előzetes |} Microsoft Azure alkalmazás szolgáltatás"
    description="Egyszerűen áttelepítheti a logikájának alkalmazások séma legújabb verziójára. Kövesse ezeket a lépéseket."
    services="logic-apps"
    documentationCenter=""
    authors="MSFTMAN"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="deonhe"/>

# <a name="how-to-migrate-logic-apps-to-schema-version-2015-08-01-preview"></a>Hogy miként telepítheti át a logika alkalmazások séma verzió 2015-08-01-előzetes

Ugrás a meglévő összefüggés-alkalmazásokat az új séma, tegye a következőket:  
1. Nyissa meg a logika alkalmazás az Azure-portálon  
2. Kattintson a frissítés séma:

 ![API ikon][step1]   
A frissítés séma lapot jeleníti meg, és egy dokumentumot, amely a részletek adja meg az új séma fejlődött hivatkozást tartalmaz: ![API ikon][step2]

>[AZURE.NOTE] **Frissítés séma**kiválasztásakor azt automatikusan futtassa az áttelepítési lépéseket, és adja meg a kód eredménye meg. Definíciójának, azonban frissítéséhez követni kell a helyes kódolás gyakorlatát, például az alábbi **tanácsokat** részben leírt használható.

## <a name="best-practices-when-migrating-your-logic-apps-to-the-latest-schema-version"></a>Ajánlott eljárások a logika alkalmazások áttelepítéséhez séma legújabb verziójára:  

- Másolja az áttelepített parancsfájlt új logika alkalmazás – nem írja felül a régi egyik mindaddig, amíg a sikeresen befejeződött, a tesztelése és az áttelepített alkalmazás megerősítést a várt módon működik.
- A logika alkalmazásban tesztelje **előtt** gyártási illusztráló
- Áttelepítés befejeződése után indítsa el a [felügyelt API-k](./apis-list.md) használata, ha lehetséges, hogy a logika alkalmazások frissítése. Ha például is használatba Dropbox v2, használja a DropBox v1 whereever.


## <a name="whats-next"></a>Következő lépések
-  [Megtudhatja, hogy miként manuális áttelepítése az összefüggés-alkalmazások](../app-service-logic/app-service-logic-schema-2015-08-01.md)


<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png







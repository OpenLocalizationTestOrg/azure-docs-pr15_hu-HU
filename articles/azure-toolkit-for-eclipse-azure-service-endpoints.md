<properties
    pageTitle="Azure Service végpontok"
    description="Az Azure eszközkészlet Holdas az Azure szolgáltatás végpontjának beállításait ismerteti."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->

# <a name="azure-service-endpoints"></a>Azure Service végpontok #

Azure service végpontok határozza meg, hogy az alkalmazás telepítve, és a globális Azure platform által felügyelt, Azure a 21Vianet által üzemeltetett Kína vagy magánjellegű Azure platform. A **Szolgáltatás végpontok** párbeszédpanel lehetővé teszi, hogy milyen használni kívánt szolgáltatás végpontokat adja meg. Nyissa meg a **Szolgáltatási végpontok** párbeszédpanel Holdas belül, kattintson az **ablak**, kattintson a **Beállítások**gombra, bontsa ki az **Azure**és válassza a **Szolgáltatás végpontok**. Az **Aktív Set** mező beállítása azt határozza meg, milyen Azure service végpontokat fogja használni a Azure projektek az aktuális munkaterületen.

A következő **Szolgáltatási végpontok** párbeszédpanelje látható.

![][ic719493]

## <a name="to-set-the-service-endpoints"></a>A szolgáltatás végpontokat ##

**Szolgáltatási végpontok** párbeszédpanelen hajtsa végre az alábbi műveletek egyikét:

* Ha szeretné használni a globális Azure platform **Aktív megadása** legördülő listából **windowsazure.com** jelölje ki, és kattintson az **OK gombra**.
* Ha szeretné használni az Azure **Active megadása** legördülő listából, Kínában a 21Vianet által üzemeltetett **windowsazure.cn (China)** jelölje ki, és kattintson az **OK gombra**.
* Ha szeretne egy magánjellegű Azure platform használata:
    1. Kattintson a **Szerkesztés**gombra.
    2. Ekkor megjelenik egy párbeszédpanel, amely közli, hogy a **Szolgáltatási végpontok** párbeszédpanel bezárul, és megnyílik a beállításfájl beállítása. Kattintson az **OK gombra**.
    3. A preferencesets.xml fájlban hozzon létre egy új `preferenceset` elemet. Az új elem létrehozása `name`, `blob`, `management`, `portalURL` és `publishsettings` attribútumok, és adjon értéket azokat, amelyek megfelelnek a magánjellegű Azure platform. Használhatja a meglévő megadott értékek `preferenceset` elemek sablonként. **Megjegyzés**: A értékét a `blob` attribútum tartalmaznia kell a szöveget az URL-cím a "blob".
    4. Mentse és zárja be a preferencesets.xml.
    5. Nyissa meg a **Szolgáltatási végpontok** párbeszédpanel.
    6. Az **Aktív megadása** legördülő listából jelölje ki a létrehozott aktív megadása, és kattintson az **OK gombra**.
    7. Miután létrehozta a magánjellegű Azure platform `preferenceset` elemet, módosíthatja az értékeket rendelt **Szolgáltatások végpont** párbeszédpanelen a **Szerkesztés** gombra kattintva. Létrehozhat több magánjellegű Azure platform `preferenceset` , ha a kívánt elemeket.

## <a name="see-also"></a>Lásd még: ##

[Azure eszközkészlete Holdas][]

[Az Azure eszközkészlet Holdas telepítése][] 

[A Holdas Azure egy helló világ alkalmazás létrehozása][]

Java Azure használatával kapcsolatos további tudnivalókért olvassa el a az [Azure Java Developer Center][]című témakört.

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure eszközkészlete Holdas]: http://go.microsoft.com/fwlink/?LinkID=699529
[A Holdas Azure egy helló világ alkalmazás létrehozása]: http://go.microsoft.com/fwlink/?LinkID=699533
[Az Azure eszközkészlet Holdas telepítése]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png

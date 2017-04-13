<properties
   pageTitle="Adatok beolvasása az Azure Analysis Services |} Microsoft Azure"
   description="Megtudhatja, hogy miként csatlakozhat, és az adatok beolvasása az Analysis Services-kiszolgáló Azure-ban."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="get-data-from-azure-analysis-services"></a>Adatok beolvasása az Azure Analysis Services rendszerből
Miután létrehozott egy server Azure-ban, illetve rendszerbe állított táblázatos modellből, a szervezet felhasználói készen áll a csatlakozás és adatok vizsgálatának megkezdéséhez.

Azure Analysis Services támogatja a [frissített objektum modellek](#client-libraries); client hibák TOM, AMO, Adomd.Net vagy MSOLAP, csatlakozás a kiszolgálóhoz xmla keresztül. Ha például a Power BI, a Power BI-asztal, az Excelben vagy a minden olyan külső ügyfélalkalmazás, amely támogatja az objektum modelljei.

## <a name="server-name"></a>Kiszolgálónév
Azure-ban létrehozott egy Analysis Services-kiszolgáló, beállításakor megad egy egyedi nevet és a terület, ahol a kiszolgáló hozható létre. A kiszolgálónév megadása egy kapcsolatot, a kiszolgáló elnevezési rendszer esetén:
```
<protocol>://<region>/<servername>
```
 Ahol protokoll **asazure**karakterláncot, a terület eléri a régió, a kiszolgáló létrehozásának helyét a Uri (például, ha a nyugati USA-beli westus.asazure.windows.net) a régión belüli az egyedi kiszolgáló neve pedig a kiszolgálónév.

## <a name="get-the-server-name"></a>A kiszolgáló nevét
Mielőtt csatlakoztatná, kell a kiszolgáló nevét. Az **Azure-portálon** > kiszolgáló > **Áttekintés** > **kiszolgáló nevét**, másolja a teljes kiszolgáló nevét. Ha a szervezet más felhasználók is csatlakozni a kiszolgáló, érdemes kell velük osztani a kiszolgáló nevét. A kiszolgáló nevét megadásakor a teljes elérési utat kell használni.

![Kiszolgálónév Ismerkedés az Azure-ban](./media/analysis-services-deploy/aas-deploy-get-server-name.png)


## <a name="connect-in-power-bi-desktop"></a>A Power BI Desktop csatlakoztatása

> [AZURE.NOTE] Ez a funkció előzetes.

1. A [Power BI Desktopot](https://powerbi.microsoft.com/desktop/), kattintson az **Adatok átvétele** > **adatbázisok** > **Azure Analysis Services**.

2. A **kiszolgáló**illessze be a kiszolgáló nevét, a vágólapról.

3. Az **adatbázis**Ha tudja a nevét a táblázatosmodell-adatbázishoz, vagy a perspektívát, amelyhez csatlakozni szeretne, illessze be az alábbi. Egyéb esetben akkor is hagyja üresen a mezőt. Adatbázis vagy a következő képernyőn perspektíva kijelölhet.

4. Kilépés az alapértelmezett **Csatlakozás live** beállítás kijelölve, majd nyomja le a **Csatlakozás**. Ha a rendszer kéri, hogy adja meg a fiók, adja meg a szervezeti fiókkal.

5. A **kezelő**bontsa ki a kiszolgálót, jelölje ki a modell vagy a perspektívát, amelyhez csatlakozni szeretne, majd kattintson a **Csatlakozás**gombra. A modell vagy perspektíva egyetlen kattintással jeleníti meg, ha a nézet összes objektumot.


## <a name="connect-in-power-bi"></a>Csatlakozás a Power BI szolgáltatásban
1. Hozzon létre egy Power BI Desktop fájlt, amelynek a modell élő kapcsolatot a kiszolgálón.

2. A [Power BI](https://powerbi.microsoft.com), kattintson az **Adatok átvétele** > **fájlokat**. Keresse meg és jelölje ki a fájlt.


## <a name="connect-in-excel"></a>Csatlakozás az Excelben
Adatok beolvasása az Excel 2016-ban vagy a Power Query korábbi verzióiban támogatott csatlakoztatása az Azure Analysis Services-kiszolgáló az Excelben. [MSOLAP.7 szolgáltató](https://aka.ms/msolap) szükség. Kapcsolódás a Power Pivot programban a tábla importálása varázsló használatával nem támogatott.

1. Az Excel 2016-ban, az **adatok** menüszalagon kattintson a **Külső adatok átvétele** > **A más forrásokból** > **Az Analysis Services szolgáltatásból**.

2. Az Adatkapcsolat varázsló a **kiszolgáló neve**mezőbe illessze be a kiszolgáló nevét, a vágólapról. **A bejelentkezési adatokat**, válassza a **használja az alábbi felhasználónevet és jelszót**, és írja be például a szervezeti felhasználónév nancy@adventureworks.com, és a jelszavát.

    ![Az Excel bejelentkezési csatlakoztatása](./media/analysis-services-connect/aas-connect-excel-logon.png)

4. Az **adatbázis és tábla kijelölése**jelölje be az adatbázis és a modell vagy a perspektívát, és kattintson a **Befejezés gombra**.

    ![Excel-választó modell csatlakoztatása](./media/analysis-services-connect/aas-connect-excel-select.png)

## <a name="connection-string"></a>Kapcsolati karakterlánc
Azure Analysis Services táblázatos a objektummodell segítségével való csatlakozáskor használja az alábbi kapcsolat-karakterlánc formátumokat:

###### <a name="integrated-azure-active-directory-authentication"></a>Integrált Azure Active Directory-hitelesítés
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```
Integrált hitelesítés felveszi az Azure Active Directory hitelesítő adatok gyorsítótár, ha elérhető lesz. Ha nem, akkor az Azure bejelentkezési ablaka jelenik meg.

###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Azure Active Directory authentication felhasználónévvel és jelszóval
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

## <a name="client-libraries"></a>Ügyfél-tárak
Amikor csatlakozik az Excel vagy más, például TOM, AsCmd, ADOMD.NET felületek Azure Analysis Services, előfordulhat, telepítse a legújabb szolgáltató ügyfél tárak. A legújabb:  

[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)</br>
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</br>
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</br>
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</br>



## <a name="next-steps"></a>Következő lépések
[A kiszolgáló kezelése](analysis-services-manage.md)

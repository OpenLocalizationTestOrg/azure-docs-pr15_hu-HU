<properties
   pageTitle="Visual Studio és SSDT telepíteni az SQL adatraktár |} Microsoft Azure"
   description="Az SQL Azure-adatraktár Visual Studio és az SQL Server Fejlesztőeszközök (SSDT) telepítése"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="sonyama;barbkess"/>

# <a name="install-visual-studio-2015-and-ssdt-for-sql-data-warehouse"></a>Visual Studio 2015 és SSDT SQL adatraktár telepítése

Adatraktár SQL-alkalmazásokat fejleszt, ajánlott a legújabb verzió az SQL Server Data Tools (SSDT) a Visual Studio 2015 használja.  Kompatibilitás a korábbi verziókkal is támogatott SSDT Visual Studio 2013 frissítés 5.  

Visual Studio segítségével SSDT teszi lehetővé az SQL Server-objektum Explorer segítségével vizuálisan táblák, nézetek, tárolt eljárások és az SQL adatraktár számos további objektumainak feltárása, valamint lekérdezések futtatása.

> [AZURE.NOTE] SQL-adatraktár egyelőre nem támogatja a Visual Studio-adatbázis projektek.  Ez a funkció megjelenik egy későbbi verziójában.

## <a name="step-1-install-visual-studio-2015"></a>Lépés: 1: Telepítse az Visual Studio 2015

Az alábbi hivatkozásokra kattintva töltse le és telepítse az Visual Studio 2015. Már Visual Studio 2013 vagy a Skype 2015 telepítve van, Ugrás a következő lépés 2, SSDT telepítése.

1. [Töltse le a Visual Studio 2015][].
2. A [Visual Studio telepítése][] útmutató kövesse az MSDN webhelyen, és válassza az alapértelmezett beállításokat.

## <a name="step-2-install-ssdt"></a>Lépés: 2: SSDT telepítése

Telepítéséhez SSDT for Visual Studio egyszerűen jelölje be a Visual Studio belül egy SSDT frissítési ezeket a lépéseket követve.

1. A Visual Studio alkalmazásban kattintson az **eszközök** / **Extensions és frissítések...**  /  **Frissítések**
2. Jelölje ki a **Frissítéseket** , és keresse meg **a Microsoft frissítés az SQL Server-adatbázis szerszámok**

Ha nem található meg a frissítést, majd kell rendelkeznie a legújabb verziója.  Telepítve van a SSDT megerősítéséhez kattintson a **Súgó** / **Kapcsolatban a Microsoft Visual Studio** , és keresse meg az SQL Server Data Tools a listában.  A legújabb SSDT 14.0.60525.0.  Ha a beállítás nem érhető el a Visual Studióban, azt is megteheti ellátogathat a [SSDT letöltési][] lapra, töltse le és telepítse kézzel a SSDT.

## <a name="next-steps"></a>Következő lépések

Most, hogy a legújabb SSDT, készen áll [csatlakoztatni][] szeretné az SQL adatraktár.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[Csatlakozás]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Visual Studio 2015 letöltése]: https://www.visualstudio.com/downloads/
[Visual Studio telepítése]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT letöltése]: https://msdn.microsoft.com/library/mt204009.aspx

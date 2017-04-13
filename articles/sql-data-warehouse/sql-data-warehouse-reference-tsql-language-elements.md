<properties
   pageTitle="SQL-raktári Transact-SQL nyelv elemek |} Microsoft Azure"
   description="Az SQL adatraktár a Transact-SQL nyelv elemeket segédletekre mutató hivatkozások listája."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/08/2016"
   ms.author="barbkess;sonyama;kevin"/>

# <a name="language-elements"></a>Nyelvi elemek

## <a name="core-elements"></a>Alapvető elemek

- [szabályok szintaxisa](https://msdn.microsoft.com/library/ms177563.aspx)
- [objektumok elnevezési szabályai](https://msdn.microsoft.com/library/ms175874.aspx)
- [fenntartott kulcsszavak](https://msdn.microsoft.com/library/ms189822.aspx)
- [rendezési sorrendek](https://msdn.microsoft.com/library/ff848763.aspx)
- [Megjegyzések](https://msdn.microsoft.com/library/ms181627.aspx)
- [állandók](https://msdn.microsoft.com/library/ms179899.aspx)
- [adattípusok](https://msdn.microsoft.com/library/ms187752.aspx)
- [VÉGREHAJTÁSA](https://msdn.microsoft.com/library/ms188332.aspx)
- [a kifejezések](https://msdn.microsoft.com/library/ms190286.aspx)
- [TÖRLÉSE](https://msdn.microsoft.com/library/ms173730.aspx)
- [IDENTITÁS tulajdonság megoldás:](https://msdn.microsoft.com/library/ms186775.aspx)
- [NYOMTATÁS](https://msdn.microsoft.com/library/ms176047.aspx)
- [HASZNÁLATA](https://msdn.microsoft.com/library/ms188366.aspx)

## <a name="batches-control-of-flow-and-variables"></a>Tételek, a vezérlő, a munkafolyamat és a változók

- [KEZDJE EL... VÉGE](https://msdn.microsoft.com/library/ms190487.aspx)
- [OLDALTÖRÉS](https://msdn.microsoft.com/library/ms181271.aspx)
- [DEKLARÁLÁSA@local_variable](https://msdn.microsoft.com/library/ms188927.aspx)
- [HA... EGYÉB](https://msdn.microsoft.com/library/ms182717.aspx)
- [RAISERROR](https://msdn.microsoft.com/library/ms178592.aspx)
- [SET@local_variable](https://msdn.microsoft.com/library/ms189484.aspx)
- [THROW](https://msdn.microsoft.com/library/ee677615.aspx)
- [PRÓBÁLJA MEG... A TÉNYLEGES](https://msdn.microsoft.com/library/ms175976.aspx)
- [KÖZBEN](https://msdn.microsoft.com/library/ms178642.aspx)

## <a name="operators"></a>Operátorok
- [+ (Hozzáadás)](https://msdn.microsoft.com/library/ms178565.aspx)
- [+ (Karakterlánc összefűzés)](https://msdn.microsoft.com/library/ms177561.aspx)
- [-(Negatív)](https://msdn.microsoft.com/library/ms189480.aspx)
- [-(Kivonása)](https://msdn.microsoft.com/library/ms189518.aspx)
- [* (Szorzása)](https://msdn.microsoft.com/library/ms176019.aspx)
- [/ (Osztása)](https://msdn.microsoft.com/library/ms175009.aspx)
- [Moduló](https://msdn.microsoft.com/library/ms190279.aspx)

## <a name="wildcard-characters-to-match"></a>A megfelelő helyettesítő karaktereket
- [= (Egyenlő)](https://msdn.microsoft.com/library/ms175118.aspx)
- [> (Nagyobb, mint)](https://msdn.microsoft.com/library/ms178590.aspx)
- [< (Legalább)](https://msdn.microsoft.com/library/ms179873.aspx)
- [> = (nagyszerű vagy azzal egyenlő)](https://msdn.microsoft.com/library/ms181567.aspx)
- [< = (kisebb vagy egyenlő)](https://msdn.microsoft.com/library/ms174978.aspx)
- [<> (Nem egyenlő)](https://msdn.microsoft.com/library/ms176020.aspx)
- [! = (Nem egyenlő)](https://msdn.microsoft.com/library/ms190296.aspx)
- [ÉS](https://msdn.microsoft.com/library/ms188372.aspx)
- [KÖZÖTT](https://msdn.microsoft.com/library/ms187922.aspx)
- [LÉTEZIK](https://msdn.microsoft.com/library/ms188336.aspx)
- [A](https://msdn.microsoft.com/library/ms177682.aspx)
- [NEM [] NULL](https://msdn.microsoft.com/library/ms188795.aspx)
- [PÉLDÁUL](https://msdn.microsoft.com/library/ms179859.aspx)
- [NEM](https://msdn.microsoft.com/library/ms189455.aspx)
- [VAGY](https://msdn.microsoft.com/library/ms188361.aspx)

### <a name="bitwise-operators"></a>Bitenkénti operátorok

- [& (Bitenkénti és)](https://msdn.microsoft.com/library/ms174965.aspx)
- [| (Bitenkénti vagy)](https://msdn.microsoft.com/library/ms186714.aspx)
- [^ (Bitenkénti kizáró vagy)](https://msdn.microsoft.com/library/ms190277.aspx)
- [~ (Bitenkénti NOT)](https://msdn.microsoft.com/library/ms173468.aspx)
- [^ = (Bitenkénti kizárólagos vagy egyenlő)](https://msdn.microsoft.com/library/cc627413.aspx)
- [|} = (Bitenkénti vagy egyenlő)](https://msdn.microsoft.com/library/cc627409.aspx)
- [& = (bitenkénti és egyenlő)](https://msdn.microsoft.com/library/cc627427.aspx)

## <a name="functions"></a>Függvények

- [@@DATEFIRST](https://msdn.microsoft.com/library/ms187766.aspx)
- [@@ERROR](https://msdn.microsoft.com/library/ms188790.aspx)
- [@@LANGUAGE](https://msdn.microsoft.com/library/ms177557.aspx)
- [@@SPID](https://msdn.microsoft.com/library/ms189535.aspx)
- [@@TRANCOUNT](https://msdn.microsoft.com/library/ms187967.aspx)
- [@@VERSION](https://msdn.microsoft.com/library/ms177512.aspx)
- [ABS](https://msdn.microsoft.com/library/ms189800.aspx)
- [ARCCOS](https://msdn.microsoft.com/library/ms178627.aspx)
- [ASCII](https://msdn.microsoft.com/library/ms177545.aspx)
- [ARCSIN](https://msdn.microsoft.com/library/ms181581.aspx)
- [ARCTAN](https://msdn.microsoft.com/library/ms181746.aspx)
- [ATN2](https://msdn.microsoft.com/library/ms173854.aspx)
- [BINARY_CHECKSUM](https://msdn.microsoft.com/library/ms173784.aspx)
- [ESET](https://msdn.microsoft.com/library/ms181765.aspx)
- [CAST és konvertálása](https://msdn.microsoft.com/library/ms187928.aspx)
- [FELSŐ HATÁR](https://msdn.microsoft.com/library/ms189818.aspx)
- [KARAKTER](https://msdn.microsoft.com/library/ms187323.aspx)
- [CHARINDEX](https://msdn.microsoft.com/library/ms186323.aspx)
- [ELLENŐRZŐ](https://msdn.microsoft.com/library/ms189788.aspx)
- [EGYESÍTÉS](https://msdn.microsoft.com/library/ms190349.aspx)
- [COL_NAME](https://msdn.microsoft.com/library/ms174974.aspx)
- [COLLATIONPROPERTY](https://msdn.microsoft.com/library/ms190305.aspx)
- [ÖSSZEFŰZÉS](https://msdn.microsoft.com/library/hh231515.aspx)
- [COS](https://msdn.microsoft.com/library/ms188919.aspx)
- [COT](https://msdn.microsoft.com/library/ms188921.aspx)
- [DARABSZÁM](https://msdn.microsoft.com/library/ms175997.aspx)
- [COUNT_BIG](https://msdn.microsoft.com/library/ms190317.aspx)
- [CUME_DIST](https://msdn.microsoft.com/library/hh231078.aspx)
- [CURRENT_TIMESTAMP](https://msdn.microsoft.com/library/ms188751.aspx)
- [CURRENT_USER](https://msdn.microsoft.com/library/ms176050.aspx)
- [DATABASEPROPERTYEX](https://msdn.microsoft.com/library/ms186823.aspx)
- [ADATHOSSZ](https://msdn.microsoft.com/library/ms173486.aspx)
- [DATEADD](https://msdn.microsoft.com/library/ms186819.aspx)
- [DATEDIFF](https://msdn.microsoft.com/library/ms189794.aspx)
- [DATEFROMPARTS](https://msdn.microsoft.com/library/hh213228.aspx)
- [DATENAME](https://msdn.microsoft.com/library/ms174395.aspx)
- [DATEPART](https://msdn.microsoft.com/library/ms174420.aspx)
- [DATETIME2FROMPARTS](https://msdn.microsoft.com/library/hh213312.aspx)
- [DATETIMEFROMPARTS](https://msdn.microsoft.com/library/hh213233.aspx)
- [DATETIMEOFFSETFROMPARTS](https://msdn.microsoft.com/library/hh231077.aspx)
- [NAP](https://msdn.microsoft.com/library/ms176052.aspx)
- [DB_ID](https://msdn.microsoft.com/library/ms186274.aspx)
- [DB_NAME](https://msdn.microsoft.com/library/ms189753.aspx)
- [FOK](https://msdn.microsoft.com/library/ms178566.aspx)
- [DENSE_RANK](https://msdn.microsoft.com/library/ms173825.aspx)
- [ELTÉRÉS](https://msdn.microsoft.com/library/ms188753.aspx)
- [HÓNAP.UTOLSÓ.NAP](https://msdn.microsoft.com/library/hh213020.aspx)
- [ERROR_MESSAGE](https://msdn.microsoft.com/library/ms190358.aspx)
- [ERROR_NUMBER](https://msdn.microsoft.com/library/ms175069.aspx)
- [ERROR_PROCEDURE](https://msdn.microsoft.com/library/ms188398.aspx)
- [ERROR_SEVERITY](https://msdn.microsoft.com/library/ms178567.aspx)
- [ERROR_STATE](https://msdn.microsoft.com/library/ms180031.aspx)
- [EXP](https://msdn.microsoft.com/library/ms179857.aspx)
- [FIRST_VALUE](https://msdn.microsoft.com/library/hh213018.aspx)
- [PADLÓ](https://msdn.microsoft.com/library/ms178531.aspx)
- [GETDATE](https://msdn.microsoft.com/library/ms188383.aspx)
- [GETUTCDATE](https://msdn.microsoft.com/library/ms178635.aspx)
- [HAS_DBACCESS](https://msdn.microsoft.com/library/ms187718.aspx)
- [HASHBYTES](https://msdn.microsoft.com/library/ms174415.aspx)
- [INDEXPROPERTY](https://msdn.microsoft.com/library/ms187729.aspx)
- [ISDATE](https://msdn.microsoft.com/library/ms187347.aspx)
- [ISNULL](https://msdn.microsoft.com/library/ms184325.aspx)
- [ISNUMERIC](https://msdn.microsoft.com/library/ms186272.aspx)
- [ELTÉRÉS](https://msdn.microsoft.com/library/hh231256.aspx)
- [LAST_VALUE](https://msdn.microsoft.com/library/hh231517.aspx)
- [ÁTVEZETÉSI](https://msdn.microsoft.com/library/hh213125.aspx)
- [BALRA](https://msdn.microsoft.com/library/ms177601.aspx)
- [HOSSZ](https://msdn.microsoft.com/library/ms190329.aspx)
- [NAPLÓ](https://msdn.microsoft.com/library/ms190319.aspx)
- [LOG10](https://msdn.microsoft.com/library/ms175121.aspx)
- [ALSÓ](https://msdn.microsoft.com/library/ms174400.aspx)
- [AZ LTRIM](https://msdn.microsoft.com/library/ms177827.aspx)
- [MAX](https://msdn.microsoft.com/library/ms187751.aspx)
- [MIN](https://msdn.microsoft.com/library/ms179916.aspx)
- [HÓNAP](https://msdn.microsoft.com/library/ms187813.aspx)
- [NCHAR](https://msdn.microsoft.com/library/ms182673.aspx)
- [NTILE](https://msdn.microsoft.com/library/ms175126.aspx)
- [NULLIF](https://msdn.microsoft.com/library/ms177562.aspx)
- [OBJECT_ID](https://msdn.microsoft.com/library/ms190328.aspx)
- [OBJEKTUMNÉV](https://msdn.microsoft.com/library/ms186301.aspx)
- [OBJECTPROPERTY](https://msdn.microsoft.com/library/ms176105.aspx)
- [OIBJECTPROPERTYEX](https://msdn.microsoft.com/library/ms188390.aspx)
- [ODBCS skaláris függvények](https://msdn.microsoft.com/library/bb630290.aspx)
- [FÖLÉ záradék](https://msdn.microsoft.com/library/ms189461.aspx)
- [PARSENAME](https://msdn.microsoft.com/library/ms188006.aspx)
- [PATINDEX](https://msdn.microsoft.com/library/ms188395.aspx)
- [PERCENTILE_CONT](https://msdn.microsoft.com/library/hh231473.aspx)
- [PERCENTILE_DISC](https://msdn.microsoft.com/library/hh231327.aspx)
- [PERCENT_RANK](https://msdn.microsoft.com/library/hh213573.aspx)
- [A PI](https://msdn.microsoft.com/library/ms189512.aspx)
- [A POWER](https://msdn.microsoft.com/library/ms174276.aspx)
- [QUOTENAME](https://msdn.microsoft.com/library/ms176114.aspx)
- [KOMPLEX SZÁM RADIÁNBAN](https://msdn.microsoft.com/library/ms189742.aspx)
- [VÉL FÜGGVÉNY](https://msdn.microsoft.com/library/ms177610.aspx)
- [RANGSOR](https://msdn.microsoft.com/library/ms176102.aspx)
- [CSERE](https://msdn.microsoft.com/library/ms186862.aspx)
- [BIZONYOS](https://msdn.microsoft.com/library/ms174383.aspx)
- [FORDÍTOTT SORREND](https://msdn.microsoft.com/library/ms180040.aspx)
- [JOBBRA](https://msdn.microsoft.com/library/ms177532.aspx)
- [KEREKÍTÉS](https://msdn.microsoft.com/library/ms175003.aspx)
- [OSZLOPSZÁM ALAPJÁN](https://msdn.microsoft.com/library/ms186734.aspx)
- [RTRIM](https://msdn.microsoft.com/library/ms178660.aspx)
- [SCHEMA_ID](https://msdn.microsoft.com/library/ms188797.aspx)
- [SCHEMA_NAME](https://msdn.microsoft.com/library/ms175068.aspx)
- [SERVERPROPERTY](https://msdn.microsoft.com/library/ms174396.aspx)
- [SESSION_USER](https://msdn.microsoft.com/library/ms177587.aspx)
- [BEJELENTKEZÉS](https://msdn.microsoft.com/library/ms188420.aspx)
- [SIN](https://msdn.microsoft.com/library/ms188377.aspx)
- [SMALLDATETIMEFROMPARTS](https://msdn.microsoft.com/library/hh213396.aspx)
- [SOUNDEX](https://msdn.microsoft.com/library/ms187384.aspx)
- [HELY](https://msdn.microsoft.com/library/ms187950.aspx)
- [SQL_VARIANT_PROPERTY](https://msdn.microsoft.com/library/ms178550.aspx)
- [GYÖK](https://msdn.microsoft.com/library/ms176108.aspx)
- [NÉGYZETES](https://msdn.microsoft.com/library/ms173569.aspx)
- [STATS_DATE](https://msdn.microsoft.com/library/ms190330.aspx)
- [SZÓRÁS](https://msdn.microsoft.com/library/ms190474.aspx)
- [SZÓRÁSP](https://msdn.microsoft.com/library/ms176080.aspx)
- [STR](https://msdn.microsoft.com/library/ms189527.aspx)
- [ELEMEK](https://msdn.microsoft.com/library/ms188043.aspx)
- [KARAKTERLÁNC](https://msdn.microsoft.com/library/ms187748.aspx)
- [ÖSSZEG](https://msdn.microsoft.com/library/ms187810.aspx)
- [SUSER_SNAME](https://msdn.microsoft.com/library/ms174427.aspx)
- [SWITCHOFFSET](https://msdn.microsoft.com/library/bb677244.aspx)
- [SYSDATETIME](https://msdn.microsoft.com/library/bb630353.aspx)
- [SYSDATETIMEOFFSET](https://msdn.microsoft.com/library/bb677334.aspx)
- [SYSTEM_USER](https://msdn.microsoft.com/library/ms179930.aspx)
- [SYSUTCDATETIME](https://msdn.microsoft.com/library/bb630387.aspx)
- [TAN](https://msdn.microsoft.com/library/ms190338.aspx)
- [TERTIARY_WEIGHTS](https://msdn.microsoft.com/library/ms186881.aspx)
- [TIMEFROMPARTS](https://msdn.microsoft.com/library/hh213398.aspx)
- [TODATETIMEOFFSET](https://msdn.microsoft.com/library/bb630335.aspx)
- [TYPE_ID](https://msdn.microsoft.com/library/ms181628.aspx)
- [TYPE_NAME](https://msdn.microsoft.com/library/ms189750.aspx)
- [TYPEPROPERTY](https://msdn.microsoft.com/library/ms188419.aspx)
- [UNICODE](https://msdn.microsoft.com/library/ms180059.aspx)
- [FELSŐ](https://msdn.microsoft.com/library/ms180055.aspx)
- [FELHASZNÁLÓI](https://msdn.microsoft.com/library/ms186738.aspx)
- [FELHASZNÁLÓNÉV](https://msdn.microsoft.com/library/ms188014.aspx)
- [VAR](https://msdn.microsoft.com/library/ms186290.aspx)
- [VARP](https://msdn.microsoft.com/library/ms188735.aspx)
- [ÉV](https://msdn.microsoft.com/library/ms186313.aspx)
- [XACT_STATE](https://msdn.microsoft.com/library/ms189797.aspx)

## <a name="transactions"></a>Tranzakciók

- [tranzakciók](https://msdn.microsoft.com/library/mt204031.aspx)

## <a name="diagnostic-sessions"></a>Diagnosztikai munkamenetek

- [DIAGNOSZTIKAI MUNKAMENET LÉTREHOZÁSA](https://msdn.microsoft.com/library/mt204029.aspx)

## <a name="procedures"></a>Eljárások

- [sp_addrolemember](https://msdn.microsoft.com/library/ms187750.aspx)
- [sp_columns](https://msdn.microsoft.com/library/ms176077.aspx)
- [sp_configure](https://msdn.microsoft.com/library/ms188787.aspx)
- [sp_datatype_info_90](https://msdn.microsoft.com/library/mt204014.aspx)
- [sp_droprolemember](https://msdn.microsoft.com/library/ms188369.aspx)
- [sp_execute](https://msdn.microsoft.com/library/ff848746.aspx)
- [Sp_executesql](https://msdn.microsoft.com/library/ms188001.aspx)
- [sp_fkeys](https://msdn.microsoft.com/library/ms175090.aspx)
- [sp_pdw_add_network_credentials](https://msdn.microsoft.com/library/mt204011.aspx)
- [sp_pdw_database_encryption](https://msdn.microsoft.com/library/mt219360.aspx)
- [sp_pdw_database_encryption_regenerate_system_keys](https://msdn.microsoft.com/library/mt204033.aspx)
- [sp_pdw_log_user_data_masking](https://msdn.microsoft.com/library/mt204023.aspx)
- [sp_pdw_remove_network_credentials](https://msdn.microsoft.com/library/mt204038.aspx)
- [sp_pkeys](https://msdn.microsoft.com/library/ms189813.aspx)
- [sp_prepare](https://msdn.microsoft.com/library/ff848808.aspx)
- [sp_spaceused](https://msdn.microsoft.com/library/ms188776.aspx)
- [sp_special_columns_100](https://msdn.microsoft.com/library/mt204025.aspx)
- [sp_sproc_columns](https://msdn.microsoft.com/library/ms182705.aspx)
- [sp_statistics](https://msdn.microsoft.com/library/ms173842.aspx)
- [sp_tables](https://msdn.microsoft.com/library/ms186250.aspx)
- [sp_unprepare](https://msdn.microsoft.com/library/ff848735.aspx)



## <a name="set-statements"></a>Kimutatások beállítása

- [ANSI_DEFAULTS BEÁLLÍTÁSA](https://msdn.microsoft.com/library/ms188340.aspx)
- [ANSI_NULL_DFLT_OFF BEÁLLÍTÁSA](https://msdn.microsoft.com/library/ms187356.aspx)
- [ANSI_NULL_DFLT_ON BEÁLLÍTÁSA](https://msdn.microsoft.com/library/ms187375.aspx)
- [ANSI_NULLS BEÁLLÍTÁSA](https://msdn.microsoft.com/library/ms188048.aspx)
- [ANSI_PADDING BEÁLLÍTÁSA](https://msdn.microsoft.com/library/ms187403.aspx)
- [ANSI_WARNINGS BEÁLLÍTÁSA](https://msdn.microsoft.com/library/ms190368.aspx)
- [ARITHABORT BEÁLLÍTÁSA](https://msdn.microsoft.com/library/ms190306.aspx)
- [ARITHIGNORE BEÁLLÍTÁSA](https://msdn.microsoft.com/library/ms184341.aspx)
- [CONCAT_NULL_YIELDS_NULL BEÁLLÍTÁSA](https://msdn.microsoft.com/library/ms176056.aspx)
- [DATEFIRST BEÁLLÍTÁSA](https://msdn.microsoft.com/library/ms181598.aspx)
- [DÁTUMFORMÁTUM BEÁLLÍTÁSA](https://msdn.microsoft.com/library/ms189491.aspx)
- [SET FMTONLY METÓDUS](https://msdn.microsoft.com/library/ms173839.aspx)
- [IMPLICIT_TRANSACITONS BEÁLLÍTÁSA](https://msdn.microsoft.com/library/ms187807.aspx)
- [LOCK_TIMEOUT BEÁLLÍTÁSA](https://msdn.microsoft.com/library/ms189470.aspx)
- [NUMBERIC_ROUNDABORT BEÁLLÍTÁSA](https://msdn.microsoft.com/library/ms188791.aspx)
- [QUOTED_IDENTIFIER BEÁLLÍTÁSA](https://msdn.microsoft.com/library/ms174393.aspx)
- [ROWCOUNT BEÁLLÍTÁSA](https://msdn.microsoft.com/library/ms188774.aspx)
- [TEXTSIZE BEÁLLÍTÁSA](https://msdn.microsoft.com/library/ms186238.aspx)
- [SET TRANZAKCIÓ ELKÜLÖNÍTÉSI FOKOT](https://msdn.microsoft.com/library/ms173763.aspx)
- [XACT_ABORT BEÁLLÍTÁSA](https://msdn.microsoft.com/library/ms188792.aspx)


## <a name="next-steps"></a>Következő lépések
Hivatkozás kapcsolatos további tudnivalókért [adatraktár SQL referencia áttekintése][]című témakörben találhat.

<!--Image references-->

<!--Article references-->
[Adatraktár SQL referencia – áttekintés]: sql-data-warehouse-overview-reference.md

<!--MSDN references-->

Most, hogy hozzáadott egy eseményindító, az időt szeretne elhelyezni, amely az eseményindító által generált adatokat tartalmazó érdekes eredményeket. Hajtsa végre az alábbi lépéseket követve hozzáadhat egy **SFTP - kivonat mappa** művelet. Ez a művelet fog bontsa ki a fájl tartalmát a megadott feltételek teljesülése esetén. 

Állítsa be az Ez a művelet, meg kell a következő adatokat. Megfigyelheti, hogy az egyes az új fájl tulajdonságainak bemeneteként az eseményindító által generált könnyen használható adatokat:

|SFTP - kivonat mappa tulajdonság|Leírás|
|---|---|
|Forrás archív fájl elérési útja|Ez a kibontása a fájl elérési útját. Válassza ki azt a tokenek korábbi művelet, vagy keresse meg a SFTP-kiszolgálót, keresse meg a fájl elérési útját.|
|Cél mappa elérési útja|Ez egy, az elérési utat, ahol kell elhelyezni a kibontott fájlokat. Válassza a tokenek egyikét a rendeltetési hely elérési útjaként korábbi művelet vagy keresse meg a SFTP kiszolgáló, és jelöljön ki egy görbét.|
|Felülírja?|Azt jelzi, ha a kibontott fájllal azonos nevű fájl megtalálható a a cél mappa elérési útja, ha a létező fájl vagy nem írható felül.|

Első lépések bontsa ki a fájlokat, ha a korábban megadott feltétel kiértékelésének eredménye *Igaz*, a művelet hozzáadása. 

1. Válassza a **Hozzáadás művelet**.        
![SFTP művelet feltétel kép 6](./media/connectors-create-api-sftp/condition-6.png)   
- Jelölje ki a **SFTP - kivonat mappa** műveletet      
![SFTP művelet feltétel kép 7](./media/connectors-create-api-sftp/condition-7.png)   
- Jelölje be a **forrás archív fájl elérési útja**              
![SFTP művelet feltétel kép 9](./media/connectors-create-api-sftp/condition-9.png)   
- Jelölje be a **fájl elérési útja** jogkivonat. Ez azt jelzi, hogy használni kívánt fájl elérési útját a fájl, amely az eseményindító található a forrás archív fájl elérési útja.           
![SFTP művelet feltétel kép 10](./media/connectors-create-api-sftp/condition-10.png)   
- Jelölje be a **cél mappa elérési útja**           
![SFTP művelet feltétel képe 11](./media/connectors-create-api-sftp/condition-11.png)   
- Jelölje be a **fájl elérési útja** jogkivonat. Ez azt jelzi, hogy használni kívánt fájl elérési útját a fájl, amely az eseményindító található a rendeltetési hely elérési útját a kibontott fájlok.   
- Írja be a **cél mappa elérési útja** vezérlő *\ExtractedFile* . Ehhez a csak a fájl elérési útja jogkivonat, a cél mappa elérési útját vezérlő után.         
![SFTP művelet feltétel kép 12](./media/connectors-create-api-sftp/condition-12.png)   
- Adja meg a *True* a a **felülírása?* vezérlő jelzi, hogy meglévő fájlok felül-e, ha ugyanazt a nevet a kibontott fájlként is.      
![SFTP művelet feltétel kép 13-mal](./media/connectors-create-api-sftp/condition-13.png)   
- A munkafolyamat a módosítások mentése  

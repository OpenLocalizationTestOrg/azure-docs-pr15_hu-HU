Helyezzen el egy eseményindító.

1. *Sftp* a logika alkalmazások Tervező a Keresés mezőbe írja be, majd válassza a **SFTP – Ha a fájl hozzáadása vagy módosítása** az eseményindító   
![SFTP eseményindító kép 1](./media/connectors-create-api-sftp/trigger-1.png)  
- Megnyitja a **fájl hozzáadása vagy módosítása során** vezérlő  
![SFTP eseményindító kép 2](./media/connectors-create-api-sftp/trigger-2.png)  
- Jelölje ki a **…** a vezérlő jobb oldalán található. Ekkor megnyílik a mappa Dátumválasztó gomb  
![SFTP eseményindító kép 3](./media/connectors-create-api-sftp/action-1.png)  
- Jelölje ki a **SFTP** jelölje ki a legfelső szintű mappa az új vagy módosított fájlok Lync mappával. Figyelje meg a legfelső szintű mappa, megjelennek a **mappa** vezérlő.  
![SFTP eseményindító kép 4](./media/connectors-create-api-sftp/action-2.png)   

Ezen a ponton a logika app van beállítva, hogy a fájl módosított vagy az adott mappában SFTP létrehozott elindul a Futtatás eseményindítók és a munkafolyamat műveletei az eseményindító. 

>[AZURE.NOTE]Egy logikai alkalmazását fog működni legalább egy eseményindító és egy műveletet kell tartalmaznia. Kövesse a művelet hozzáadása a következő szakaszban.  
Ebben a példában lehet megtudhatja, hogy miként kezdeményezhet logika alkalmazás munkafolyamat, amikor a SharePoint Online-lista jön létre új elem a **SharePoint online-ban – új elem létrehozásakor** eseményindító használatával.

>[AZURE.NOTE]Jelentkezzen be a SharePoint-fiókjába, ha még nem hozott létre *kapcsolatot* a SharePoint Online fogja kérni.  

1. Adja meg *a sharepoint* a logika alkalmazások Tervező a Keresés mezőbe, majd jelölje be a **SharePoint online-ban – új elem létrehozásakor** eseményindító.  
![A SharePoint online eseményindító képe](./media/connectors-create-api-sharepointonline/trigger-1.png)  
- Az **Új elem létrehozásakor** vezérlőelem jelenik meg.  
![A SharePoint online eseményindító kép 2](./media/connectors-create-api-sharepointonline/trigger-2.png)   
- Válassza a **webhely URL-CÍMÉT**. Az új elemek a munkafolyamat elindításának figyelni kívánt SharePoint online webhelyre.  
![A SharePoint online eseményindító kép 3](./media/connectors-create-api-sharepointonline/trigger-3.png)   
- Jelölje ki a **lista nevére**. Az új elemek, amely elindítja a munkafolyamatot figyelni kívánt SharePoint Online-webhelyen a listában.  
![A SharePoint online eseményindító kép 4](./media/connectors-create-api-sharepointonline/trigger-4.png)   

Ezen a ponton a logika alkalmazás van beállítva az eseményindító eseményindítók és a munkafolyamat műveletei a Futtatás elindul. Ez történik minden alkalommal, amikor új elemet a kijelölt SharePoint Online-lista jön létre.  
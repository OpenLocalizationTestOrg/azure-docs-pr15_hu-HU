Az alábbiakban a **Szolgáltatás Bus - várólista üzenet érkezésekor** eseményindító használata logika alkalmazás munkafolyamat indítására új elem szolgáltatás Bus várólista elküldésekor.  

>[AZURE.NOTE]Jelentkezzen be a szolgáltatás Bus kapcsolati karakterláncot, ha még nem hozott létre kapcsolatot szolgáltatás Bus kéri.  

1. A logikai alkalmazások Tervező a Keresés mezőbe írja be a **szolgáltatás bus**. Ezután jelölje ki a **Szolgáltatás Bus - várólista üzenet érkezésekor** eseményindító.  
![Szolgáltatás Bus eseményindító kép 1](./media/connectors-create-api-servicebus/trigger-1.png)   
- Az **üzenet érkezésekor várólista** párbeszédpanel jelenik meg.  
![Szolgáltatás Bus eseményindító kép 2](./media/connectors-create-api-servicebus/trigger-2.png)   
- Adja meg a szolgáltatás Bus várólista szeretne Lync-az eseményindító nevét.   
![Szolgáltatás Bus eseményindító kép 3](./media/connectors-create-api-servicebus/trigger-3.png)   

Ezen a ponton a logika app van beállítva az eseményindító. Új elem a kijelölt várakozási sorban található érkezése esetén az eseményindító elindul a Futtatás eseményindítók és a munkafolyamat műveletei.    

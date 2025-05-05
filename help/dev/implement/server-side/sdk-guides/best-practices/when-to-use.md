---
source-git-commit: 5c66ee5c8dd5fe60eaeed10fdb9bb6dcee000c89
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 0%

---
# Quando utilizzare le decisioni su dispositivo rispetto a quelle edge

## Considera i casi di utilizzo quando decidi se utilizzare le decisioni sul dispositivo

![Alt immagine](assets/comparison.jpeg)

La differenza principale tra il decisioning *sul dispositivo* e il decisioning edge è che il decisioning sul dispositivo esegue le decisioni localmente sui server, mentre le decisioni edge vengono prese sulla rete Edge di Adobe Target. Le decisioni sul dispositivo devono essere utilizzate per qualsiasi attività A/B o XT che deve essere consegnata su pagine con traffico elevato, in cui le prestazioni influiscono notevolmente sui KPI aziendali come conversione, ricavi e conservazione. Ad esempio, supponiamo che il team marketing esegua campagne pubblicitarie per attrarre potenziali clienti nella tua pagina principale. L’esecuzione di campagne pubblicitarie sulle reti degli editori richiede un pagamento, pertanto qualsiasi potenziale cliente che arrivi sulla pagina Home si traduce in un importo in dollari. Allo stesso tempo, supponiamo che tu stia eseguendo esperimenti A/B per vedere quale immagine protagonista cattura al meglio l&#39;attenzione del consumatore. Se la consegna di questi esperimenti A/B richiede altri 2 secondi, c’è un’alta probabilità che il consumatore diventi impaziente e rimbalzi. Ecco i vostri dollari di marketing ed esperimenti A/B! Perdere questa prospettiva guadagnata è difficile, dal momento che qualsiasi opportunità di conversione di questo potenziale ad un cliente fedele o ripetuto è ora perduto. Pertanto, l’esecuzione di un’attività di decisione su dispositivo per questo caso d’uso può evitare qualsiasi impatto negativo che la latenza può introdurre.

D’altra parte, le decisioni edge richiedono una chiamata di blocco della rete per recuperare un’esperienza, ma possono essere molto utili, in quanto i dati in tempo reale e l’apprendimento automatico possono essere utilizzati per rendere l’esperienza dell’utente finale altamente coinvolgente. Una chiamata di blocco della rete introdurrà una latenza aggiuntiva durante la distribuzione dell’esperienza; tuttavia, in alcuni scenari, questo compromesso potrebbe avere senso. Ad esempio, considera uno scenario in cui un cliente naviga all’interno del catalogo dei prodotti e suppone che si sposti verso una pagina dei dettagli dei prodotti. Se in quella pagina viene visualizzato un elenco consigliato di prodotti, insieme al prodotto attualmente visualizzato dal cliente, ciò può aumentare il coinvolgimento, e in un secondo momento la conversione e i ricavi. Se mostrare in questo modo l’elenco consigliato di prodotti richiederebbe una decisione Edge influenzata dall’algoritmo ML di Adobe Target (che comporterebbe un’ulteriore latenza), tale latenza aggiuntiva non sarebbe sufficientemente significativa da consentire all’utente finale di eseguire un mancato recapito. Inoltre, un elenco raccomandato di prodotti si traduce in un tasso di conversione più elevato. Pertanto, in questo caso, una decisione Edge offre il massimo valore alla tua azienda.

## Funzioni supportate

Oltre a valutare i tuoi casi d&#39;uso e gli obiettivi aziendali, controlla quali funzionalità supporta il decisioning sul dispositivo [1&rbrace; prima di decidere se utilizzare il decisioning sul dispositivo rispetto al decisioning Edge. ](../on-device-decisioning/supported-features.md) Attualmente, Edge Decisioning supporta tutti i tipi di attività, il targeting del pubblico e i metodi di allocazione.
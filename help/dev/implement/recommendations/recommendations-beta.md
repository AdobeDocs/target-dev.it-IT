---
keywords: Recommendations, impostazioni, preferenze, settore verticale, filtro criteri incompatibili, gruppo host predefinito, url base miniature, token api consigli,
description: Scopri come implementare [!UICONTROL Recommendations] attività in [!DNL Adobe Target].
title: Come posso implementare [!UICONTROL Recommendations] Attività?
feature: Recommendations
hidefromtoc: true
hide: true
source-git-commit: 8d04fe249aafb5ee064dc614ae72f68b8f8459f2
workflow-type: tm+mt
source-wordcount: '1550'
ht-degree: 20%

---

# Pianificare e implementare [!UICONTROL Recommendations]

Informazioni utili per pianificare e implementare [!DNL Adobe Target Recommendations].

>[!NOTE]
>
>Oltre a questo articolo, il [Guida di Adobe Target per professionisti aziendali](https://experienceleague.adobe.com/en/docs/target/using/target-home){target=_blank} contiene informazioni approfondite su [Recommendations di destinazione](https://experienceleague.adobe.com/en/docs/target/using/recommendations/recommendations){target=_blank}.

Prima di configurare il primo [!UICONTROL Recommendations] attività in [!DNL Adobe Target], completa i passaggi seguenti:

1. [Implementare [!UICONTROL Target]](#implement-target) sulle superfici dell’app web e mobile che desideri utilizzare per acquisire il comportamento degli utenti e distribuire i consigli.
1. [Configurare [!UICONTROL Recommendations] catalogo](#set-up-your-recommendations-catalog) di prodotti o contenuti che desideri consigliare ai tuoi utenti.
1. [Trasmettere informazioni comportamentali e contesto](#pass-behavioral-information-and-context) a [!DNL Target Recommendations] per consentirgli di fornire consigli personalizzati.
1. [Configurare le esclusioni globali](#configure-global-exclusions).
1. [Configura [!UICONTROL Recommendations] impostazioni](#configure-recommendations-settings).
1. (Facoltativo) [Amministrare [!UICONTROL Recommendations] utilizzo delle API di amministrazione](#administer-recommendations-using-admin-apis).

## 1. Implementare [!UICONTROL Target]

[!DNL Target Recommendations] richiede l&#39;implementazione di [!DNL Adobe Experience Platform Web SDK] o at.js 0.9.2 (o versione successiva). Consulta la [[!UICONTROL Target] guide all’implementazione lato client](../client-side/overview.md) per ulteriori informazioni.

## 2. Configurare [!UICONTROL Recommendations] catalogo

Per fornire consigli di alta qualità, [!UICONTROL Target] devono conoscere i prodotti o i contenuti che desideri consigliare. I cataloghi in genere includono tre tipi di informazioni sugli elementi consigliati. Supponiamo che tu stia consigliando dei film. Includi quanto segue:

1. Dati da presentare all’utente che riceve il consiglio. Ad esempio, è possibile visualizzare il nome del filmato e un URL per una miniatura del poster.
1. Dati utili per applicare controlli di marketing e merchandising. Ad esempio, è possibile visualizzare la classificazione del filmato in modo da non consigliare i film NC-17.
1. Dati utili per determinare la somiglianza degli elementi con altri elementi. Ad esempio, è possibile visualizzare il genere del filmato e il regista del filmato.

[!UICONTROL Target] offre più opzioni di integrazione per compilare il catalogo. Queste opzioni possono essere utilizzate in combinazione per aggiornare articoli diversi nel catalogo o per aggiornare attributi di articoli diversi su frequenze diverse.

| Metodo | Che cos’è | Quando utilizzarlo | Informazioni aggiuntive |
| --- | --- | --- | --- |
| Feed catalogo | Pianificare un feed (CSV, [!DNL Google] XML prodotto, oppure [!UICONTROL Analytics Product Classifications]) da caricare e acquisire su base giornaliera. | Per inviare informazioni su più elementi alla volta. Per l’invio di informazioni che non cambiano frequentemente. | Consulta [Feed](https://experienceleague.adobe.com/en/docs/target/using/recommendations/entities/feeds). |
| API entità | Chiama un’API per inviare aggiornamenti immediati per un singolo elemento. | Per l’invio di aggiornamenti man mano che si verificano su un elemento alla volta. Per l’invio di informazioni che cambiano frequentemente (ad esempio prezzo, livello di magazzino/scorte). | Consulta la [Documentazione per gli sviluppatori API per le entità](https://developer.adobe.com/target/administer/recommendations-api/#tag/Entities). |
| Trasmettere gli aggiornamenti sulla pagina | Invia aggiornamenti al minuto per un singolo elemento utilizzando JavaScript nella pagina o utilizzando l’API di consegna. | Per l’invio di aggiornamenti man mano che si verificano su un elemento alla volta. Per l’invio di informazioni che cambiano frequentemente (ad esempio prezzo, livello di magazzino/scorte). | Consulta [Visualizzazioni elemento/pagine prodotto](#item-views-or-product-pages) di seguito. |

La maggior parte dei clienti deve implementare almeno un feed. Puoi quindi scegliere di integrare il feed con aggiornamenti per gli attributi o gli elementi modificati di frequente utilizzando l’API delle entità o il metodo on-the-page.

## 3. Trasmettere informazioni comportamentali e contesto

Informazioni e contesto comportamentali da trasmettere a [!UICONTROL Target] dipende dall’azione intrapresa dal visitatore, che è spesso associata al tipo di pagina con cui il visitatore interagisce.

### Visualizzazioni di elementi o pagine di prodotti

Nelle pagine in cui un visitatore visualizza un singolo elemento, ad esempio la pagina dei dettagli di un prodotto, devi trasmettere l’identità dell’elemento che il visitatore sta visualizzando. Inoltre, passa la categoria più granulare dell’elemento che il visitatore sta visualizzando, per consentire di filtrare i consigli per la categoria corrente.

Puoi anche trasmettere alcuni attributi che si modificano rapidamente sulla pagina stessa del prodotto. Ad esempio, puoi trasmettere il prezzo (`value`) e a livello di inventario/scorte.

#### Trasmetti prezzo e inventario

```js {line-numbers="true"}
<script type="text/javascript">
function targetPageParams() { 
   return { 
      "entity": { 
         "id": "32323", 
         "categoryId": "running-shoes", 
         "value": 119.99, 
         "inventory": 329 
      } 
   } 
}
</script>
```

### Visualizzazioni per categoria/pagine per categoria

In una pagina di categoria, è probabile che si desideri limitare i consigli ai prodotti o contenuti all’interno di tale categoria. A questo scopo, assicurati di trasmettere l’identità della categoria attualmente visualizzata.

#### Trasmetti categoria corrente

```js {line-numbers="true"}
function targetPageParams() { 
   return { 
      "entity": { 
         "categoryId": "running-shoes" 
      } 
   } 
}
```

### Aggiunte al carrello/Visualizzazioni carrello/Pagine di pagamento

In una pagina del carrello, puoi consigliare gli articoli in base al contenuto del carrello corrente del visitatore. A questo scopo, passa gli ID di tutti gli elementi nel carrello corrente del visitatore utilizzando il parametro speciale `cartIds`.

#### Passa elementi attualmente nel carrello

```js {line-numbers="true"}
function targetPageParams() {
   return {
      "cartIds": "352,223,23432,432,553"
      }
}
```

Per ulteriori informazioni sui consigli basati sul carrello, consulta [Basato su carrello](https://experienceleague.adobe.com/en/docs/target/using/recommendations/criteria/base-the-recommendation-on-a-recommendation-key#cart-based) nel *[!DNL Adobe Target]Guida per professionisti aziendali*.

### Escludere gli elementi già presenti nel carrello del visitatore

Sulle pagine di tutto il sito, puoi escludere alcuni elementi dai consigli. Ad esempio, potresti non voler consigliare articoli già presenti nel carrello del visitatore. A questo scopo, passa gli ID di tutti gli elementi che desideri escludere utilizzando il parametro speciale `excludedIds`.

#### Passa elementi da escludere

```js {line-numbers="true"}
function targetPageParams() {
   return {
      "excludedIds": "352,223,23432,432,553"
      }
}
```

### Pagine di conferma acquisti/ordini

Quando si verifica un evento di acquisto, passa l’identità dell’articolo o degli articoli acquistati. Consulta [Tracciare le conversioni](../client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md#track-conversions) nel [Come distribuire at.js > Implementare [!UICONTROL Target] senza un sistema per la gestione dei tag](../client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md) articolo.

## 4. Configurare le esclusioni globali

Escludi gli elementi a livello globale che non desideri consigliare a un visitatore. Consulta [Esclusioni](https://experienceleague.adobe.com/en/docs/target/using/recommendations/entities/exclusions) nel *[!DNL Adobe Target]Guida per professionisti aziendali*.

## 5. Configurare [!UICONTROL Recommendations] impostazioni

Utilizza le impostazioni per gestire l’implementazione di [!UICONTROL Recommendations].

Per accedere al **[!UICONTROL Recommendations Settings]** opzioni, apri [!DNL Target] nel [!DNL Adobe Experience Cloud], quindi fai clic su **[!UICONTROL Administration]** > **[!UICONTROL Recommendations]**.

![Pagina Impostazioni Recommendations](/help/dev/implement/recommendations/assets/recs-settings-new.png)

Configura le seguenti opzioni:

### [!UICONTROL Recommendations API Token]

Le seguenti opzioni sono disponibili nel [!UICONTROL Recommendations API Token] sezione:

#### [!UICONTROL Client code]

Il [!DNL Target] [!UICONTROL client code].

Se non conosci il tuo [!UICONTROL client code], nella [!DNL Target] interfaccia utente, fai clic su **[!UICONTROL Administration]** > **[!UICONTROL Implementation]**. Il [!UICONTROL client code] viene visualizzato nella [!UICONTROL Account Details] sezione.

#### Token di autenticazione

Il [!DNL Adobe Target] API di amministrazione, tra cui [!DNL Recommendations Admin] Le API sono protette dall&#39;autenticazione per garantire che solo gli utenti autorizzati possano accedervi [!DNL Adobe Target]. Utilizza il [Console Adobe Developer](https://developer.adobe.com/console/home) per gestire questa autenticazione per tutti [!DNL Adobe Experience Cloud solutions], tra cui [!DNL Adobe Target].

Per ulteriori informazioni, consulta [Configurare l’autenticazione per le API di Adobe Target](/help/dev/before-administer/configure-authentication.md).

>[!WARNING]
>
>Presta attenzione durante la generazione di un nuovo token di autenticazione. La generazione di un nuovo token causa un errore nelle chiamate API che utilizzano il token corrente. Aggiorna eventuali script o applicazioni con il nuovo token di autenticazione generato.

### Criteri

Conoscere il settore verticale del tuo sito aiuta Target a scegliere i criteri per i consigli.

Criteri in [!DNL Recommendations] sono regole che determinano quali prodotti o contenuti consigliare in base a un set predeterminato di comportamenti dei visitatori. I criteri possono essere basati sulle tendenze popolari, sui comportamenti attuali e passati di un visitatore o su prodotti e contenuti simili. È possibile sottoporre e test più tipi di consigli tra loro aggiungendo più criteri.

Per ulteriori informazioni, consulta [Criteri](https://experienceleague.adobe.com/en/docs/target/using/recommendations/criteria/algorithms){target=_blank} nel *Guida di Adobe Target per professionisti aziendali.*

Le seguenti impostazioni sono disponibili nel [!UICONTROL Criteria] sezione:

#### [!UICONTROL Industry/Vertical]

Il settore verticale è utilizzato per aiutare a categorizzare i criteri per i consigli. Queste informazioni aiutano i membri del team a trovare i criteri più appropriati per una pagina specifica, ad esempio quelli più adatti per la pagina del carrello o per una pagina multimediale.

Le seguenti categorie sono disponibili dall’elenco a discesa:

* Nessuna personalizzazione
* Retail/E-commerce
* Generazione di lead/B2B/servizi finanziari
* Media/Editoria

#### [!UICONTROL Filter Incompatible Criteria]

Abilita questa opzione per mostrare solo i criteri per i quali la pagina selezionata trasmette i dati richiesti. Non tutti i criteri vengono eseguiti correttamente su ogni pagina. La pagina o mbox deve passare `entity.id` o `entity.categoryId` per rendere compatibili i consigli per l&#39;elemento o la categoria corrente.

In generale, è consigliabile mostrare solo i criteri compatibili. Tuttavia, se desideri che i criteri non compatibili siano disponibili per l’attività, non abilitare questa opzione.

L’Adobe consiglia di disabilitare questa opzione se utilizzi una soluzione di gestione tag.

Per ulteriori informazioni su questa opzione, consulta [[!UICONTROL Recommendations] Domande frequenti](https://experienceleague.adobe.com/en/docs/target/using/recommendations/recommendations-faq/recommendations-faq){target=_blank} nel *[!DNL Adobe Target]Guida per professionisti aziendali*.

### [!UICONTROL Product Catalog]

Le seguenti opzioni sono disponibili nel [!UICONTROL Product Catalog] sezione:

#### [!UICONTROL Default Host Group]

Seleziona il gruppo di host predefinito.

Il gruppo di host può essere utilizzato per separare gli elementi disponibili nel catalogo per usi diversi. Ad esempio, puoi utilizzare i gruppi di host per ambienti di sviluppo e produzione, marchi diversi o diverse aree geografiche. Per impostazione predefinita, i risultati dell&#39;anteprima in Ricerca nel catalogo, Raccolte ed Esclusioni si basano sul gruppo di host predefinito. Puoi anche selezionare un gruppo di host diverso per visualizzare in anteprima i risultati, utilizzando il filtro Ambiente. Per impostazione predefinita, gli elementi appena aggiunti sono disponibili in tutti i gruppi di host, a meno che non sia specificato un ID ambiente al momento della creazione o dell&#39;aggiornamento dell&#39;elemento. I consigli distribuiti dipendono dal gruppo di host specificato nella richiesta.

Se i prodotti non vengono visualizzati, assicurati di utilizzare il gruppo host corretto. Ad esempio, se imposti che il consiglio usi un ambiente di gestione temporanea e imposti il gruppo host su Gestione temporanea, potrebbe essere necessario ricreare le raccolte nell&#39;ambiente di gestione temporanea perché si visualizzino i prodotti. Per visualizzare i prodotti disponibili in ogni ambiente, utilizza Ricerca catalogo con ogni ambiente. Puoi anche visualizzare in anteprima il contenuto di [!UICONTROL Recommendations] raccolte ed esclusioni per un ambiente selezionato (gruppo di host).

>[!NOTE]
>
>Dopo aver modificato l’ambiente selezionato, fai clic su **[!UICONTROL Search]** per aggiornare i risultati restituiti.

Il **[!UICONTROL Environment]** Il filtro è disponibile nelle seguenti posizioni nell’interfaccia utente di Target:

* Ricerca nel catalogo (**[!UICONTROL Recommendations]** > **[!UICONTROL Catalog Search]**)
* Raccolte (**[!UICONTROL Recommendations]** > **[!UICONTROL Collections]**)
* Finestra di dialogo Crea raccolta (**[!UICONTROL Recommendations]** > **[!UICONTROL Collections]** > **[!UICONTROL Create collection]**)
* Finestra di dialogo Aggiorna raccolta (**[!UICONTROL Recommendations]** > **[!UICONTROL Collections]** > **[!UICONTROL Edit]**)
* Finestra di dialogo Crea esclusione (**[!UICONTROL Recommendations]** > **[!UICONTROL Exclusions]** > **[!UICONTROL Create exclusion]**)
* Finestra di dialogo Aggiorna esclusione (**[!UICONTROL Recommendations]** > **[!UICONTROL Exclusions]** > **[!UICONTROL Edit]**)

Per ulteriori informazioni, consulta [Host](https://experienceleague.adobe.com/en/docs/target/using/administer/hosts){target=_blank} nel *[!DNL Adobe Target]Guida per professionisti aziendali*.

#### [!UICONTROL Thumbnail Base]

L&#39;impostazione di un URL di base per il catalogo dei prodotti rende possibile l’utilizzo di URL relativi quando specifichi le miniature dei prodotti fornendo l’URL della miniatura.

Ad esempio:

`"entity.thumbnailURL=/Images/Homepage/product1.jpg"`

imposta un URL relativo all’URL di base della miniatura.

### [!UICONTROL Custom Attribute Key Configuration]

Basa i consigli su un elemento memorizzato nel profilo del visitatore. Ad esempio, &quot;ultimo elemento aggiunto al carrello&quot; o &quot;ultimo video visualizzato al 90% o più&quot;.

Clic **[!UICONTROL Add]** per creare una nuova configurazione, specifica un nome per la configurazione, seleziona l’attributo profilo desiderato, quindi fai clic su **[!UICONTROL Save]**.

## 6. (Facoltativo) Amministrare [!UICONTROL Recommendations] utilizzo delle API di amministrazione

Consulta la [Utilizzare [!UICONTROL Recommendations] API](../../before-administer/recs-api/overview.md) guida pratica per scoprire come configurare e utilizzare il [!UICONTROL Target] API di amministrazione e consegna per [!UICONTROL Recommendations].
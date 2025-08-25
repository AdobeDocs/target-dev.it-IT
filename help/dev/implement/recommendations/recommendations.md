---
keywords: Consigli, impostazioni, preferenze, settore verticale, filtro criteri incompatibili, gruppo host predefinito, URL base miniature, token API consigli, $ 9
description: Scopri come implementare [!UICONTROL Recommendations] attività in [!DNL Adobe Target].
title: Come Si Implementano [!UICONTROL Recommendations] Attività?
feature: Recommendations
exl-id: af1e8b60-6dbb-451b-aa4f-e167d1800d1c
source-git-commit: 94a4122244065384f487ca9a29dfa1b414168cb8
workflow-type: tm+mt
source-wordcount: '1462'
ht-degree: 21%

---

# Pianifica e implementa [!UICONTROL Recommendations]

Informazioni utili per pianificare e implementare [!DNL Adobe Target Recommendations].

>[!NOTE]
>
>Oltre a questo articolo, la [Guida di Adobe Target Business Practitioner](https://experienceleague.adobe.com/docs/target/using/target-home.html?lang=it){target=_blank} contiene informazioni approfondite su [Target Recommendations](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html){target=_blank}.

Prima di configurare la prima attività [!UICONTROL Recommendations] in [!DNL Adobe Target], completare i passaggi seguenti:

1. [Implementa [!UICONTROL Target]](#implement-target) sulle superfici dell&#39;app Web e mobile che desideri utilizzare per acquisire il comportamento dell&#39;utente e distribuire i consigli.
1. [Configura il [!UICONTROL Recommendations] catalogo](#set-up-your-recommendations-catalog) di prodotti o contenuti da consigliare agli utenti.
1. [Passa informazioni comportamentali e contesto](#pass-behavioral-information-and-context) a [!DNL Target Recommendations] per consentirgli di distribuire consigli personalizzati.
1. [Configura esclusioni globali](#configure-global-exclusions).
1. [Configura [!UICONTROL Recommendations] impostazioni](#configure-recommendations-settings).
1. (Facoltativo) [Amministrare [!UICONTROL Recommendations] tramite le API amministratore](#administer-recommendations-using-admin-apis).

## &#x200B;1. Implementare [!UICONTROL Target]

[!DNL Target Recommendations] richiede l&#39;implementazione di Adobe Experience Platform Web SDK o at.js 0.9.2 (o versione successiva). Per ulteriori informazioni, consulta le [[!UICONTROL Target] guide all&#39;implementazione lato client](../client-side/overview.md).

## &#x200B;2. Configurare il catalogo [!UICONTROL Recommendations]

Per fornire consigli di alta qualità, [!UICONTROL Target] deve conoscere i prodotti o i contenuti che desideri consigliare. I cataloghi in genere includono tre tipi di informazioni sugli elementi consigliati. Supponiamo che tu stia consigliando dei film. Includi quanto segue:

1. Dati da presentare all’utente che riceve il consiglio. Ad esempio, è possibile visualizzare il nome del filmato e un URL per una miniatura del poster.
1. Dati utili per applicare controlli di marketing e merchandising. Ad esempio, è possibile visualizzare la classificazione del filmato in modo da non consigliare i film NC-17.
1. Dati utili per determinare la somiglianza degli elementi con altri elementi. Ad esempio, è possibile visualizzare il genere del filmato e il regista del filmato.

[!UICONTROL Target] offre più opzioni di integrazione per compilare il catalogo. Queste opzioni possono essere utilizzate in combinazione per aggiornare articoli diversi nel catalogo o per aggiornare attributi di articoli diversi su frequenze diverse.

| Metodo | Che cos’è | Quando utilizzarlo | Informazioni aggiuntive |
| --- | --- | --- | --- |
| Feed catalogo | Pianifica il caricamento e l’acquisizione di un feed (CSV, Google Product XML o Classificazioni di prodotto Analytics) su base giornaliera. | Per inviare informazioni su più elementi alla volta. Per l’invio di informazioni che non cambiano frequentemente. | Consulta [Feed](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/feeds.html). |
| API entità | Chiama un’API per inviare aggiornamenti immediati per un singolo elemento. | Per l’invio di aggiornamenti man mano che si verificano su un elemento alla volta. Per l’invio di informazioni che cambiano frequentemente (ad esempio prezzo, livello di magazzino/scorte). | Consulta la documentazione per gli sviluppatori API [Entities](https://developer.adobe.com/target/administer/recommendations-api/#tag/Entities). |
| Trasmettere gli aggiornamenti sulla pagina | Invia aggiornamenti al minuto per un singolo elemento tramite JavaScript sulla pagina o utilizzando l’API di consegna. | Per l’invio di aggiornamenti man mano che si verificano su un elemento alla volta. Per l’invio di informazioni che cambiano frequentemente (ad esempio prezzo, livello di magazzino/scorte). | Consulta [Visualizzazioni elemento/pagine prodotto](#item-views-or-product-pages) di seguito. |

>[!IMPORTANT]
>
>Presta attenzione quando aggiorni [!DNL Recommendations] [!UICONTROL Catalog] tramite [!DNL Delivery API]. [!DNL Delivery API] è pubblico, quindi evita di utilizzarlo per popolare gli elementi cliccabili nel catalogo dei consigli. In questo modo si possono introdurre contenuti invalidati e inquinare il catalogo.
>
>**Best practice**: utilizza [!DNL Delivery API] solo per aggiornare gli attributi del catalogo che:
>
>* Cambia frequentemente (ad esempio, prezzo, livello di azioni).
>
>* Segui un formato predefinito che può essere facilmente convalidato sul tuo sito web.
>
>* Non utilizzarlo per aggiungere o modificare elementi cliccabili o altri contenuti non verificati.
>
>* Se necessario, puoi richiedere all’assistenza clienti di disabilitare gli aggiornamenti del catalogo tramite l’API di consegna.
>
>Per ulteriori informazioni, vedere la documentazione di [[!UICONTROL Adobe Target Delivery API]](https://developer.adobe.com/target/implement/delivery-api/){target=_blank}.

La maggior parte dei clienti deve implementare almeno un feed. Puoi quindi scegliere di integrare il feed con aggiornamenti per gli attributi o gli elementi modificati di frequente utilizzando l’API delle entità o il metodo on-the-page.

## &#x200B;3. Trasmettere informazioni comportamentali e contesto

Le informazioni comportamentali e il contesto da trasmettere a [!UICONTROL Target] dipendono dall&#39;azione intrapresa dal visitatore, che è spesso associata al tipo di pagina con cui il visitatore interagisce.

### Visualizzazioni di elementi o pagine di prodotti

Nelle pagine in cui un visitatore visualizza un singolo elemento, ad esempio la pagina dei dettagli di un prodotto, devi trasmettere l’identità dell’elemento che il visitatore sta visualizzando. Per consentire al visitatore di filtrare i consigli per la categoria corrente, devi passare anche la categoria più granulare dell’elemento che sta visualizzando.

Puoi anche trasmettere alcuni attributi che si modificano rapidamente sulla pagina stessa del prodotto. Ad esempio, è possibile trasmettere il prezzo (`value`) e il livello scorte/scorte.

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

Per ulteriori informazioni sui consigli basati su carrello, consulta [In base al carrello](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/base-the-recommendation-on-a-recommendation-key.html?lang=en#cart-based) nella *[!DNL Adobe Target]Guida per professionisti aziendali*.

### Escludere gli elementi già presenti nel carrello del visitatore

Sulle pagine di tutto il sito, puoi escludere alcuni elementi dai consigli. Ad esempio, potresti non voler consigliare articoli già presenti nel carrello del visitatore. A tale scopo, passare gli ID di tutti gli elementi da escludere utilizzando il parametro speciale `excludedIds`.

#### Passa elementi da escludere

```js {line-numbers="true"}
function targetPageParams() {
   return {
      "excludedIds": "352,223,23432,432,553"
      }
}
```

### Pagine di conferma acquisti/ordini

Quando si verifica un evento di acquisto, passa l’identità dell’articolo o degli articoli acquistati. Vedi [Tracciare le conversioni](../client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md#track-conversions) nell&#39;articolo [Come distribuire at.js > Implementare [!UICONTROL Target] senza un sistema per la gestione dei tag](../client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md).

## &#x200B;4. Configurare le esclusioni globali

Escludi gli elementi a livello globale che non desideri consigliare a un visitatore. Consulta [Esclusioni](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/exclusions.html) nella Guida di *[!DNL Adobe Target]professionisti aziendali*.

## &#x200B;5. Configurare le impostazioni di [!UICONTROL Recommendations]

Utilizza le impostazioni per gestire l’implementazione di [!UICONTROL Recommendations].

Per accedere alle opzioni **[!UICONTROL Recommendations Settings]**, apri Target in [!DNL Adobe Experience Cloud], quindi fai clic su **[!UICONTROL Recommendations]** > **[!UICONTROL Settings]**.

![Pagina Impostazioni consigli](/help/dev/implement/recommendations/assets/recs_settings.png)

Sono disponibili le seguenti opzioni:

| Impostazione | Descrizione |
|--- |--- |
| Mbox globale personalizzata | (Facoltativo) Specifica la mbox globale personalizzata utilizzata per le attività di [!UICONTROL Target]. Per impostazione predefinita, la mbox globale utilizzata da [!UICONTROL Target] viene utilizzata per [!UICONTROL Recommendations].<P>Nota: questa opzione è impostata sulla pagina [!UICONTROL Target] **[!UICONTROL Administration]**. Apri [!UICONTROL Target], quindi fai clic su **[!UICONTROL Administration]** > **[!UICONTROL Visual Experience Composer]**. |
| Settore verticale | Il settore verticale è utilizzato per aiutare a categorizzare i criteri per i consigli. Queste informazioni aiutano i membri del team a trovare i criteri più appropriati per una pagina specifica, ad esempio quelli più adatti per la pagina del carrello o per una pagina multimediale. |
| Filtra criteri incompatibili | Abilita questa opzione per mostrare solo i criteri per i quali la pagina selezionata trasmette i dati richiesti. Non tutti i criteri vengono eseguiti correttamente su ogni pagina. La pagina o mbox deve passare `entity.id` o `entity.categoryId` per rendere compatibili i consigli per l&#39;elemento o la categoria corrente. In generale, è consigliabile mostrare solo i criteri compatibili. Per fare in modo che i criteri incompatibili siano disponibili per l’attività, deseleziona questa opzione.<P>È consigliabile disattivare l’opzione se utilizzi una soluzione di gestione tag.<P>Per ulteriori informazioni su questa opzione, vedere [[!UICONTROL Recommendations] Domande frequenti](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/recommendations-faq.html) nella *[!DNL Adobe Target]Guida per professionisti aziendali*. |
| Gruppo host predefinito | Seleziona il gruppo di host predefinito.<P>Il gruppo di host può essere utilizzato per separare gli elementi disponibili nel catalogo per usi diversi. Ad esempio, puoi utilizzare i gruppi di host per ambienti di sviluppo e produzione, marchi diversi o diverse aree geografiche. Per impostazione predefinita, i risultati dell&#39;anteprima in Ricerca nel catalogo, Raccolte ed Esclusioni si basano sul gruppo di host predefinito. Puoi anche selezionare un gruppo di host diverso per visualizzare in anteprima i risultati, utilizzando il filtro Ambiente. Per impostazione predefinita, gli elementi appena aggiunti sono disponibili in tutti i gruppi di host, a meno che non venga specificato un ID ambiente al momento della creazione o dell’aggiornamento dell’elemento. I consigli distribuiti dipendono dal gruppo di host specificato nella richiesta.<P>Se i prodotti non vengono visualizzati, assicurati di utilizzare il gruppo host corretto. Ad esempio, se imposti che il consiglio usi un ambiente di gestione temporanea e imposti il gruppo host su Gestione temporanea, potrebbe essere necessario ricreare le raccolte nell&#39;ambiente di gestione temporanea perché si visualizzino i prodotti. Per visualizzare i prodotti disponibili in ogni ambiente, utilizza Ricerca catalogo con ogni ambiente. È inoltre possibile visualizzare in anteprima il contenuto di [!UICONTROL Recommendations] raccolte ed esclusioni per un ambiente selezionato (gruppo di host).<P>**Nota:** dopo aver modificato l&#39;ambiente selezionato, fare clic su Cerca per aggiornare i risultati restituiti.<P> Il filtro **[!UICONTROL The Environment]** è disponibile nelle seguenti posizioni nell&#39;interfaccia utente di Target:<ul><li>Ricerca nel catalogo (**[!UICONTROL Recommendations]** > **[!UICONTROL Catalog Search]**)</li><li>Finestra di dialogo Crea raccolta (**[!UICONTROL Recommendations]** > **[!UICONTROL Collections]** > **[!UICONTROL Create New]**)</li><li>Finestra di dialogo Aggiorna raccolta (**[!UICONTROL Recommendations]** > **[!UICONTROL Collections]** > **[!UICONTROL Edit]**)</li><li>Finestra di dialogo Crea esclusione (**[!UICONTROL Recommendations]** > **[!UICONTROL Exclusions]** > **[!UICONTROL Create New]**)</li><li>Finestra di dialogo Aggiorna esclusione (**[!UICONTROL Recommendations]** > **[!UICONTROL Exclusions]** > **[!UICONTROL Edit]**)</li></ul>Per ulteriori informazioni, vedere [Host](https://experienceleague.adobe.com/docs/target/using/administer/hosts.html) nella Guida di *[!DNL Adobe Target]professionisti aziendali*. |
| URL di base per le miniature | L&#39;impostazione di un URL di base per il catalogo dei prodotti rende possibile l’utilizzo di URL relativi quando specifichi le miniature dei prodotti fornendo l’URL della miniatura.<P>Ad esempio:<P>`"entity.thumbnailURL=/Images/Homepage/product1.jpg"`<P>imposta un URL relativo all’URL di base della miniatura. |
| Token API [!UICONTROL Recommendations] | Usa questo token nelle chiamate API [!UICONTROL Recommendations], ad esempio l&#39;API di download. |

## &#x200B;6. (Facoltativo) Amministrare [!UICONTROL Recommendations] tramite le API amministratore

Consulta la guida pratica [Utilizzare le API [!UICONTROL Recommendations]](../../before-administer/recs-api/overview.md) per scoprire come configurare e utilizzare le API di amministrazione e consegna [!UICONTROL Target] per [!UICONTROL Recommendations].

---
title: Aggiornamento a Adobe Audience Manager DIL versione 8.0 (o successiva)
description: Il presente articolo illustra i passaggi e le raccomandazioni per aggiornare il codice Data Integration Library (DIL) di Adobe Audience Manager (AAM) alla versione 8.0 o successiva. Questo si riferisce all’implementazione DIL "lato client", non all’inoltro lato server dei dati Adobe Analytics, e riguarderà DTM, Launch by Adobe e le implementazioni senza alcuna soluzione Adobe di gestione tag.
feature: DIL Implementation
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
role: Developer, Data Engineer
level: Intermediate
exl-id: 8c1e6ed5-0f21-427b-a681-0ecb020a0e60
source-git-commit: 62b43b5627dabf754cf821f974a56c60989ef7ef
workflow-type: tm+mt
source-wordcount: '1074'
ht-degree: 0%

---

# Aggiornamento a Adobe Audience Manager’s DIL versione 8.0 (o successiva) {#updating-to-adobe-audience-manager-s-dil-version-or-greater}

Questo articolo fornisce passaggi e raccomandazioni per l&#39;aggiornamento del codice Adobe Audience Manager (AAM) [!DNL Data Integration Library] (DIL) alla versione 8.0 o successiva. Questo si riferisce all’implementazione DIL &quot;lato client&quot;, non all’inoltro lato server dei dati Adobe Analytics, e riguarderà DTM, Launch by Adobe e le implementazioni senza alcuna soluzione Adobe di gestione tag.

## Panoramica {#overview}

Il codice di Audience Manager [!DNL Data Integration Library] (DIL) ti consente di implementare AAM sul tuo sito Web*. Durante l’implementazione delle versioni precedenti di DIL, non era necessario implementare anche il servizio ID Experience Cloud (ECID) di Adobe (anche se era un’idea molto buona). A partire da DIL versione 8.0, esiste una forte dipendenza dalla versione 3.3 o successiva di ECID. Se implementi DIL 8.0 o versione successiva senza ECID 3.3 o con una versione precedente, verrà visualizzato un errore e non funzionerà. Poiché esistono diversi modi per implementare l’AAM, abbiamo creato questa pagina per fornire alcuni passaggi da seguire e alcuni consigli. Di seguito sono riportati i passaggi e le raccomandazioni suddivisi per piattaforma/metodo di implementazione. Ulteriori informazioni su DIL sono disponibili nella [documentazione](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=it).

* Come indicato nella descrizione di questa pagina, questo riguarderà solo le implementazioni DIL &quot;lato client&quot;, utilizzate dai clienti AAM che non dispongono di Adobe Analytics. Se disponi di Adobe Analytics, utilizza il metodo di inoltro lato server per implementare l’AAM. Questo metodo è descritto nella [documentazione](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html?lang=it).

## Metodi ed elementi duplicati e obsoleti {#duplicate-and-deprecated-elements-and-methods}

Nelle versioni precedenti di DIL ed ECID, esistevano metodi duplicativi (metodi che svolgono la stessa funzione sia in DIL che in ECID), che causavano confusione riguardo a quale utilizzare. In genere, era necessario utilizzarli entrambi e abbinarli, e quel messaggio non era ben comunicato ai nostri clienti. A partire da DIL 8.0, questi metodi ed elementi duplicati sono stati dichiarati obsoleti in DIL ed è consigliabile utilizzare la versione ECID.

Ad esempio:

* Durante l&#39;utilizzo di [!DNL DIL.create], alcuni elementi sono stati dichiarati obsoleti ed è consigliabile utilizzare gli elementi ECID. Questi elementi vengono richiamati nella [[!DNL DIL.create] documentazione](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html?lang=it).
* Anche il metodo a livello di istanza [!DNL idSync] è stato dichiarato obsoleto, come indicato nella [documentazione](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-instance-methods.html?lang=it) del metodo.

## Sincronizzazione ID con un ID cliente {#id-syncing-with-a-customer-id}

In AAM, puoi sincronizzare il tuo UUID (ID utente univoco anonimo) sul computer con un ID cliente, in modo da poter caricare dati offline su quel cliente e collegarli al suo comportamento online, per comprendere meglio i tuoi clienti. In passato, ciò è stato fatto in uno dei due modi seguenti:

* Il metodo a livello di istanza [!DNL idSync]
* Elemento [!DNL declaredId] in [!DNL DIL.create]

Se utilizzi uno di questi metodi meno recenti per la sincronizzazione con un ID cliente, ti consigliamo di eseguire l&#39;aggiornamento a utilizzando il metodo [!DNL setCustomerIDs], che fa parte del servizio ECID. Ulteriori informazioni su [!DNL setCustomerIDs] sono disponibili nella [documentazione](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html?lang=it) del metodo.

**Suggerimento rapido:** quando in precedenza si utilizzava uno dei metodi di cui sopra, si faceva riferimento all&#39;AAM [!UICONTROL Data Source] con l&#39;ID [!UICONTROL Data Source] (ovvero &quot;DPID&quot;). Durante l&#39;aggiornamento a [!DNL setCustomerIDs], sarà necessario utilizzare il &quot;[!UICONTROL Integration Code]&quot; dell&#39;AAM [!UICONTROL Data Source]. Punta ancora allo stesso [!UICONTROL Data Source] ma è solo un identificatore diverso. Questo è mostrato nel video seguente.

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

Nelle sezioni seguenti sono elencati i passaggi e le raccomandazioni per l’aggiornamento a DIL 8.0 in base al metodo di implementazione:

## Aggiornamento a DIL 8.0 nei tag Adobe Experience Platform {#updating-to-dil-in-experience-platform-launch}

Passaggi di base per l’aggiornamento a DIL 8.0

1. Se utilizzi una versione precedente alla 8.0 di DIL, prima di eseguire l’aggiornamento passa alla configurazione DIL nell’estensione AAM e prendi nota di tutte le opzioni avanzate in uso (da utilizzare in un passaggio successivo).
1. Aggiorna l’estensione AAM alla versione 8.0 o successiva.
1. Verifica che la versione dell&#39;estensione del servizio ID Experience Cloud sia 3.3.0 o successiva.
1. Per tutti i metodi/elementi obsoleti (come `disableIDSyncs`) presenti nell&#39;estensione AAM precedente alla versione 8.0 o nel codice personalizzato per DIL, abilita i metodi ECID nell&#39;estensione ECID.

   1. (DIL) `disableDestinationPublishingIframe` -> (ECID) `disableIdSyncs`
   1. (DIL) `disableIDSyncs` -> (ECID) `disableIdSyncs`
   1. (DIL) `iframeAkamaiHTTPS` -> (ECID) `dSyncSSLUseAkamai`
   1. (DIL) `declaredId` -> (ECID) `setCustomerIDs`

1. Publish le modifiche.

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## Aggiornamento a DIL 8.0 in Adobe DTM {#updating-to-dil-in-adobe-dtm}

1. Aggiorna lo strumento AAM alla versione 8.0 o successiva. Questa impostazione della versione si trova nella sezione &quot;Generale&quot; dello strumento AAM.
1. Per eventuali metodi/elementi obsoleti (come `disableIDSyncs`) presenti nel codice personalizzato per DIL dello strumento AAM precedente alla versione 8.0, annotali (in modo da poterli aggiungere allo strumento ECID) e rimuoverli dalla [!DNL DIL code] personalizzata nello strumento AAM.
1. Aggiornare l’estensione del servizio ID Experience Cloud alla versione 3.3.0 o successiva
1. Aggiungi le opzioni avanzate allo strumento ECID che hai rimosso dal codice personalizzato dello strumento AAM.
1. Publish le modifiche

## Aggiornamento a DIL 8.0 senza alcuna soluzione Tag Management Adobe {#additional-resources}

Se aggiorni il codice direttamente sulla pagina, puoi semplicemente sostituire gli elementi meno recenti con quelli più recenti, tranne quando devi spostare metodi/elementi da DIL a ECID, come descritto in precedenza. In tal caso, il metodo/elemento precedente nella posizione DIL verrà semplicemente sostituito con il metodo/elemento ECID nella posizione ECID.

Lo stesso vale per i gestori di tag non Adobi. Ovunque siano presenti le versioni precedenti di tale soluzione di gestione tag, sostituiscila con il nuovo codice come descritto nei passaggi seguenti.

1. Aggiorna la libreria DIL alla versione più recente (8.0 o successiva). Sarà necessario ottenere il codice DIL più recente da Adobe Consulting o Adobe Customer Care, in quanto non è attualmente disponibile in una posizione pubblica. Quindi sostituisci semplicemente il vecchio codice della libreria DIL con il nuovo codice della libreria DIL e passa al passaggio successivo (non fermarti ora o riscontrerai problemi, ha).
1. Installa [!DNL ECID Service] o aggiorna la versione esistente alla versione 3.3.0 o successiva. Puoi scaricare l&#39;ultima versione del servizio ID Experience Cloud [dalla nostra pagina GitHub](https://github.com/Adobe-Marketing-Cloud/id-service/releases). Se hai bisogno di assistenza, consulta la [documentazione](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=it) o rivolgiti a un consulente Adobe.

1. Verifica che tutti i metodi o gli elementi obsoleti presenti nel codice personalizzato per DIL vengano spostati nei metodi ECID:

   1. (DIL) `disableDestinationPublishingIframe` -> (ECID) `disableIdSyncs`

      [Documentazione](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html?lang=it)

   1. (DIL) `disableIDSyncs` -> (ECID) `disableIdSyncs`

      [Documentazione](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html?lang=it)

   1. (DIL) `iframeAkamaiHTTPS` -> (ECID) `idSyncSSLUseAkamai`

      [Documentazione](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html?lang=it)

   1. (DIL) `declaredId` -> (ECID) `setCustomerIDs`

      [Documentazione](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html?lang=it)

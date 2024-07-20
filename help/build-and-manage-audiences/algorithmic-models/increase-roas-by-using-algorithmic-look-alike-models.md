---
title: Aumentare il ROAS utilizzando modelli algoritmici (lookalike)
description: La vera potenza della modellazione lookalike di Audience Manager arriva quando cerchi di espandere il pubblico di base rispetto a un set di utenti nuovo di qualità da origini dati di seconde e terze parti. In questa esercitazione, scopri i passaggi per creare un modello da questi dati.
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 25188.jpg
kt: 1849
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6626ae11-8709-4302-9e03-0d55878d2409
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '896'
ht-degree: 0%

---

# Aumentare il ROAS utilizzando i modelli algoritmici (lookalike) in Audience Manager {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

La vera potenza del look-alike [!UICONTROL Modeling] di Audience Manager viene quando si cerca di espandere il pubblico di base rispetto a un set di utenti nuovo di qualità da origini dati di seconde e terze parti. In questa esercitazione, scopri i passaggi necessari per creare un modello da questi dati.

## Abilitare i flussi di dati di seconde o terze parti dall’Audience Marketplace {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

Per utilizzare dati di seconde e terze parti in un modello lookalike, dobbiamo prima abilitare tali dati nell’interfaccia di Audience Manager. Adobe dispone di un numero elevato di provider di dati di seconde e terze parti tra cui puoi scegliere. Questi sono disponibili in un’interfaccia self-service in AAM, tramite l’Audience Marketplace. Passa all’Audience Marketplace e sfoglia le possibilità. Il video seguente illustra come eseguire questa operazione, tra cui come abilitare i flussi gratuiti &quot;prova prima di acquistare&quot;, in modo da poter bloccare i dati che saranno più utili per la tua organizzazione prima di confermare i prezzi del provider di dati.

Inoltre, per aiutarti a ricercare e decidere quale provider di dati utilizzare, un&#39;ottima risorsa è [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/).

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## Identificare o creare una caratteristica o un segmento utente (conversione) ideale {#identify-create-an-ideal-user-conversion-trait-or-segment}

Cosa stai cercando di convincere le persone a fare sul tuo sito? Qual è il tuo evento di conversione? Naturalmente, ci sono molte risposte diverse a questa domanda, a seconda del tipo di sito/verticale e degli obiettivi organizzativi. In ogni caso, è comune in AAM creare una caratteristica per i visitatori che hanno soddisfatto tali criteri.

Nel video seguente, ti mostrerò come creare una caratteristica di conversione, che vorrai avere in posizione mentre prosegui attraverso questa esercitazione e crei un modello lookalike.

Inoltre, quando si utilizzano gli eventi di Adobe Analytics per creare le caratteristiche, è necessario tenere presente un aspetto importante, in modo da non raccogliere più utenti di quanto si dovrebbe per la caratteristica. Guarda il video seguente per la grande rivelazione. :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**NOTA:** Nel video precedente, l&#39;esempio che mostro presuppone che tu disponga di Adobe Analytics. Ovviamente, questo potrebbe non essere il caso. Se hai Google Analytics (GA), abbiamo un modulo che puoi utilizzare per inviare dati all&#39;AAM (consulta la [documentazione](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html)) e se l&#39;attività di conversione sul tuo sito viene inviata all&#39;AAM da GA, puoi creare la caratteristica di conversione da tale modulo. Se disponi di una soluzione di analisi diversa (o nessuna soluzione di analisi), puoi comunque inviare dati all’AAM tramite il nostro codice DIL e la funzione `submit`, ecc. (consulta la [documentazione](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html)). Quindi crea la caratteristica di conversione in base ai dati inviati quando l’attività di conversione viene eseguita sul sito.

## Creare un modello lookalike da dati di seconde o terze parti {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

Dopo aver completato i passaggi precedenti, siamo pronti per creare un modello algoritmico (simile). Durante la configurazione del modello, utilizzeremo la caratteristica di conversione come caratteristica di base (visitatori chiave che vogliamo duplicare) e il flusso di dati di terze parti abilitato come gruppo di persone da cui attingere.

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## Una best practice importante {#an-important-best-practice}

Quando creiamo il modello algoritmico in Audience Manager, ovviamente vogliamo che il modello sia il più efficace possibile. Poiché il modello considera tutte le caratteristiche di cui fanno parte i membri della caratteristica/segmento di base, non aiuta il modello se TUTTE le persone sono in una caratteristica/segmento. Pertanto, se disponi di caratteristiche super generiche (come tutti coloro che sono andati sul tuo sito o tutti coloro che hanno ricevuto annunci da te, ecc.), assicurati che l’origine dati a cui appartengono NON sia inclusa nelle origini dati del modello. Nel caso di utilizzo di questo articolo, è improbabile che tu possa farlo, perché ci stiamo concentrando sull’analisi di dati di terze parti per i nostri nuovi lookalike, ma vale la pena menzionare comunque, e si applica a TUTTI i tuoi modelli algoritmici.

## Crea un [!UICONTROL Algorithmic Trait] {#creating-an-algorithmic-trait}

Successivamente, sarà necessario creare un [!UICONTROL Algorithmic Trait], in modo da poter utilizzare i risultati del modello. Senza creare una caratteristica, il modello è inutile. Quindi, dopo l&#39;esecuzione del modello, assicurarsi di passare alla finestra di dialogo delle caratteristiche e creare un [!UICONTROL Algorithmic Trait]. Il video seguente illustra e fornisce alcuni suggerimenti.

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## Creare un segmento dai dati del modello e inviarlo all’DSP {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

Dopo aver creato un [!UICONTROL Algorithmic Trait], puoi creare un nuovo segmento in cui inserirlo, in modo da poter attivare i dati (non puoi attivare una caratteristica, ma piuttosto creare un nuovo segmento con una caratteristica in esso [!UICONTROL Algorithmic Trait], in modo da poter attivare (utilizzare) il segmento).

Dopo aver creato un segmento da questa caratteristica algoritmica, avrai un pubblico di potenziali clienti che assomigliano a persone già convertite sul tuo sito. Ora puoi mappare questo segmento a una qualsiasi delle destinazioni DSP in Audience Manager. Potrai indirizzare il marketing a quei lookalike che hanno più probabilità di conversione sul tuo sito rispetto al solo pubblico normale, aumentando il Return on Ad Spend.

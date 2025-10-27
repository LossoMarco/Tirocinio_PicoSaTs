**IEEE Std 1720‑2012 =>** Devo avere un XPD di 10/15 dB migliore rispetto alla AUT, il che implica un axial ratio molto vicino a 0 dB

**IEEE Std 149-2021 =>** La griglia deve soddisfare il criterio di misura per cui devo captare segnali di -45 dB rispetto alla misura massima sulla mia AUT

**Tutti =>** Che antenna usare => Tromba o guida d'onda non direttiva, lobo principale che prende tutto il FOV della AUT

**IEEE Std 149-2021 =>** L'antenna caratterizzante va puntata verso la AUT? NO, vengono usati i metodi di compensazione della sonda con il teorema di reciprocità di Lorentz applicato al campo vicino. Il campo misurato è la convoluzione dei due campi, quindi vanno de-convoluti. Per mantenere un sistema di riferimento coerente e gli algoritmi FFT per NF/FF assumono che la sonda sia fissa (oltre alla AUT), inoltre aggiungere gradi di liberà rende tutto più difficile e costoso.

**IEEE Std 1720‑2012 / An\_overview\_of\_near-field\_antenna\_measurements =>** Si preferisce la sonda non direttiva, pattern ampio. Sonda piccola rispetto a λ, altrimenti perdo risoluzione.

**IEEE Std 1720‑2012 =>** Buona pratica vuole che si facciano piu misure con punti di acquisizione spostati di λ/8 e anche la distanza tra le antenne per mediare le riflessioni tra le stesse

**IEEE Std 1720‑2012 =>** Test sui cavi flessibili (system phase error) => utilizzo un cavo di loop back al posto della AUT e vedo come varia il segnale muovendo la sonda sulla griglia

**IEEE 149-2021 =>** Distanza dalla AUT almeno 3λ per NF, chiaramente più piccola del FF 2D^2/λ

**electronics-07-00257** **=>** MRSS porta al limite teorico dei gradi di libertà, quindi conn interpolazione ricostruisco griglia uniforme e FFT veloce

**electronics-07-00257 =>** I gradi di liberà corrispondono a quante info indip contiene il campo. Fortemente influenzati dalle dimensioni elettriche dell'antenna (+ grande => + gradi), dalla distanza dall'antenna (+ lontano => - gradi (solo componenti propaganti)).

Quindi per antenne lineari NDF = 2 \* (D/λ); mentre per antenne con una superficie si ha NDF = 4 \* A/(λ^2)

I valori singolari di A corrispondono alla radice quadrata degli autovalori di A^+ \* A e sono numeri reali non negativi decrescenti. Dove A è la funzione che mappa J (densità di corrente) in E (campo elettrico).

**Probes correction for planar near field antennas measurements =>** Nel metodo della deconvoluzione si ha che **E** è il campo elettrico da trovare. **V** è la tensione misurata e **H** è la risposta spaziale della sonda, quindi è cosa nota. Si ha che **V = E conv H**. quindi per trovare **E** devo dividere (nel dominio della frequenza)  **V/H** e fare la trasformata inversa.
Per ottenere **H** si fanno delle simualzioni i CST come se fosse la misura, da stare attenti a mantenere la distanza della simualzione uguale a quella della misura reale

**Measurements_of_Antenna_Radiation_Pattern_Laboratory_Manual =>** A pagina 4 si parla di relative radiation pattern, quindi si fa la misura del pattern relativo alla AUT, quindi la calibrazione non è necessaria.

**How to Design and Test a Phased Array Antenna =>** Il pitch è la distanza tra i centri di due elementi radianti, troppo grande si sa che si hanno i grating lobes e troppo  piccola ho accoppiamento, si misura osservando la presenza di eventuali grating lobes anche lo steering del fascio. Il fill factor è il rapporto tra l'area effettivamente radiativa e quella totale dell'array, si misura tramite efficienza radiante e uniformità del pattern. Il mutual coupling è l'interazione tra elementi vicini, porta a errori nel pattern del singolo e nella sua impedenza (diversa da singolo o in array), si analizza la matrice S, si vede come varia il pattern da quando è isolato a quando è nell'array, sempre eccitando solo lui la sistemo con calibrazione attiva.

**Near-Field_Scanning_Measurements =>** Sonda piccola rispetto a lambda perchè essendo così vicini alla AUT si ha ch enon miusra un punto del campo ma un superficie. Si vuole Guadagno della sonda piccolo rispetto alla AUT(10-20dB più bassa) perchè essendo vicine le antenne, più sono simili i loro guadagni più ci sono disturbi da riflessioni multiple.
La matrice P è quella che lega il campo vero della AUT a quello misurato. In poche parole è al matrice che ci permette di togliere l'interazione della sonda dall misura efettuata e quindi ottenere il vero campo irradiato dalla AUT.

**How to Design and Test a Phased Array Antenna =>** G/T è quanto bene riesce a ricevere segnali piccoli e distinguerli dal rumore, si fa con Y-factor con rumore HOT (6000K) e COLD (290K). 
Il time gating è il periodo di tempo durante il quale si misura il segnale, si fa per evitare di misurare rumore, quindi voglio misurare solo il primo impulso e non l eriflessioni succesive.
Il beam codebook è un insieme di beam pattern che vengono usati per steerare il fascio, descrive un sistema discreto di fasci che riduce la complessità del sistema.


-------------------------------TO ADD-------------------------------------
- nf2ff_transformation — Algoritmi NF→FF (Matlab): codice analizzato per soluzioni pratiche di trasformazione NF→FF e compensazione della sonda; usato come spunto per gli script Python della pipeline. Link: https://github.com/hbartle/nf2ff_transformation
- Documentazione R&S per misure di antenne AESA: https://www.rohde-schwarz.com/it/applicazioni/misura-di-un-antenna-con-beamforming-in-modalita-di-trasmissione-nota-di-applicazione_56280-408715.html

Caratterizzazzione array con beamforming, che tipo di misure si fanno 
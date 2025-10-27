# <span style="color: #6bc06bff;">Report settimanale –Tirocinio PicoSaTs</span>

Questo documento raccoglie, settimana per settimana, i punti chiave dai paper analizzati, con focus su misure Near-Field (NF) e trasformazione Far-Field (FF), e sulla loro integrazione con il beamforming di antenne AESA.

---

## <span style="color: #e69a44ff;">Settimana 1</span>

Focus: sistemi di misura in camera anecoica e metodi NF→FF, con attenzione al planar scanning.

- Obiettivo
  - Comprendere il flusso di misura NF planare e la trasformazione al FF.
  - Identificare requisiti di campionamento, correzione di sonda e principali fonti di errore.

- Setup di misura in camera anecoica (planar scanning)
  - AUT su supporto fisso; piano di scansione X–Y a distanza costante `z0`.
  - Sonda ricevente a polarizzazione nota, montata su traslatore X–Y con controllo fine.

- Trasformazione NF→FF planare (spettro di onde piane)
  - Si misura il campo tangenziale su griglia rettangolare: `E(x,y,z0)`.
  - Si calcola lo spettro spaziale via 2D FFT per ottenere componenti `k_x, k_y` e propagazione lungo `k_z`.
  - Si effettua la correzione di polarizzazione/sonda e la ricombinazione in campo lontano.

- Campionamento e area di scansione
  - Passo di campionamento tipico ≤ `λ/2` (spesso più fine per dinamica elevata).
  - Area di scansione: estendere oltre il contorno dell’AUT (margine) per ridurre troncamento.
  - Finestre (es. Hann) utili per mitigare ripple e lobi laterali dovuti a bordi finiti.

- Sonda e correzione (probe correction)
  - Il campo misurato è la convoluzione con la risposta della sonda; serve deconvoluzione/correzione.
  - È necessaria la caratterizzazione della sonda (fattore di correzione) e l’allineamento di polarizzazione.
  - <span style="color: #f8df00ff;">**Tutti =>** Che antenna usare => Tromba o guida d'onda non direttiva, lobo principale che prende tutto il FOV della AUT</span>
  
  - MMS 2015 — Probe correction (riassunto)
    - Problema: la tensione misurata `V` è la convoluzione tra il campo AUT `E` e la risposta spaziale della sonda `H` (`V = E conv H`). Obiettivo: recuperare `E` per una trasformazione NF→FF corretta.
    - Modello/procedura: stimare/ottenere `H` (ampiezza+fase) via misura o simulazione elettromagnetica alla stessa distanza `z0`; eseguire la deconvoluzione nel dominio spaziale o, preferibilmente, nel dominio spettrale planare (`k_x, k_y`) con regolarizzazione per evitare amplificazione del rumore.
    - Sonda OERWG (open-ended rectangular waveguide): pattern e polarizzazione noti; considerare cross‑pol e banda utile della sonda nella correzione.
    - Passaggi operativi: calibrare ampiezza/fase della sonda con antenne standard; acquisire NF con riferimento di fase stabile; applicare correzione sonda (de‑embedding) prima della 2D FFT; verificare il FF risultante rispetto a pattern attesi.
    - Accortezze: aliasing/oversampling, dinamica VNA/receiver, finestre (apodizzazione) per troncamento, coerenza del sistema di riferimento (orientamento sonda e assi), controllo drift e rumore.
  
  - <span style="color: #f8df00ff;">**Probes correction for planar near field antennas measurements =>** Nel metodo della deconvoluzione si ha che **E** è il campo elettrico da trovare. **V** è la tensione misurata e **H** è la risposta spaziale della sonda, quindi è cosa nota. Si ha che **V = E conv H**. quindi per trovare **E** devo dividere (nel dominio della frequenza)  **V/H** e fare la trasformata inversa.</span>
  - <span style="color: #f8df00ff;">Per ottenere **H** si fanno delle simualzioni i CST come se fosse la misura, da stare attenti a mantenere la distanza della simualzione uguale a quella della misura reale</span>

- Principali fonti di errore
  - Troncamento del piano, aliasing (passo troppo grande), drift di fase/ampiezza.
  - Disallineamenti meccanici/angolari, multi-path residuo, non idealità della sonda.

- Note operative e prossimi passi
  - <span style="color: #f8df00ff;">**Measurements_of_Antenna_Radiation_Pattern_Laboratory_Manual =>** A pagina 4 si parla di relative radiation pattern, quindi si fa la misura del pattern relativo alla AUT, quindi la calibrazione non è necessaria.</span>

- <span style="color: #4FC3F7;">Linee guida IEEE 149 (1979, 2021)</span>
  - Scopo: pratiche raccomandate per misure di pattern, guadagno, polarizzazione e parametri associati in gamme di prova (anechoic, outdoor, compact, near-field).
  - 2021: aggiornamenti su metodi NF (planare/sferico/cilindrico), analisi dell'incertezza, requisiti software/strumentazione, sicurezza e high-power, verifiche e terminologia aggiornata (quiet zone, target support, tracking).
  - Quiet zone e range: definizione dell’area utile, controllo riflessioni (assorbitori, ground bounce), materiali del supporto AUT a basso scattering, gestione delle diffrazioni dei bordi.
  - Scanner planare: planarità/ortogonalità assi, accuratezza di passo e ripetibilità, calibrazione del piano di misura, riferimenti di posizione e controllo dell’errore di posizionamento.
  - Incertezze: contributi da strumentazione, sonda, allineamento, ambiente e post-elaborazione; costruzione del budget e propagazione all’FF dopo la trasformazione.
  - <span style="color: #f8df00ff;">**IEEE Std 149-2021 =>** La griglia deve soddisfare il criterio di misura per cui devo captare segnali di -45 dB rispetto alla misura massima sulla mia AUT</span>
  - <span style="color: #f8df00ff;">**IEEE Std 149-2021 =>** L'antenna caratterizzante va puntata verso la AUT? NO, vengono usati i metodi di compensazione della sonda con il teorema di reciprocità di Lorentz applicato al campo vicino. Il campo misurato è la convoluzione dei due campi, quindi vanno de-convoluti. Per mantenere un sistema di riferimento coerente e gli algoritmi FFT per NF/FF assumono che la sonda sia fissa (oltre alla AUT), inoltre aggiungere gradi di liberà rende tutto più difficile e costoso.</span>
  - <span style="color: #f8df00ff;">**IEEE 149-2021 =>** Distanza dalla AUT almeno 3λ per NF, chiaramente più piccola del FF 2D^2/λ</span>

- <span style="color: #4FC3F7;">IEEE 1720-2012 — Pratiche di misura NF</span>
  - Ambito e geometrie: planare, sferica, cilindrica; criteri di scelta in base a dimensioni AUT, direzioni richieste e vincoli meccanici/ambientali.
  - Scanner e posizionamento: planarità e ortogonalità degli assi, accuratezza del passo, controllo della distanza `z0`, tilt e bow; tracciabilità dei riferimenti di posizione.
  - Campionamento e area di scansione: passi tipici ≤ `λ/2` (con oversampling se necessario per dinamica); estensione dell’area oltre i bordi AUT; apodizzazione (Hann/Hamming) per ridurre ripple da troncamento.
  - Sonda e correzione: caratterizzazione complessa della sonda (ampiezza/fase), de-embedding e correzione di polarizzazione/cross-pol; verifica periodica e certificazione dei fattori di correzione.
  - Trasformazioni NF→FF:
    - Planare: spettro di onde piane con 2D FFT; gestione componenti evanescenti; normalizzazione e conversione in coordinate angolari (`θ, φ`).
    - Sferica: espansione in armoniche sferiche; requisiti di copertura angolare e numero di punti; ricostruzione completa del pattern.
    - Cilindrica: espansione in armoniche cilindriche per antenne allungate; criteri di campionamento lungo asse e perimetro.
  - Verifica e validazione: uso di antenne standard (SGH), ripetibilità, inter-range comparison, controlli periodici del range; criteri di accettazione e report dei parametri.
  - Output e metriche: pattern 2D/3D, guadagno/directivity, cross-pol.
  - <span style="color: #f8df00ff;">**IEEE Std 1720‑2012 =>** Devo avere un XPD di 10/15 dB migliore rispetto alla AUT, il che implica un axial ratio molto vicino a 0 dB</span>
  - <span style="color: #f8df00ff;">**IEEE Std 1720‑2012 / An_overview_of_near-field_antenna_measurements =>** Si preferisce la sonda non direttiva, pattern ampio. Sonda piccola rispetto a λ, altrimenti perdo risoluzione.</span>
  - <span style="color: #f8df00ff;">**IEEE Std 1720‑2012 =>** Buona pratica vuole che si facciano piu misure con punti di acquisizione spostati di λ/8 e anche la distanza tra le antenne per mediare le riflessioni tra le stesse</span>
  - <span style="color: #f8df00ff;">**IEEE Std 1720‑2012 =>** Test sui cavi flessibili (system phase error) => utilizzo un cavo di loop back al posto della AUT e vedo come varia il segnale muovendo la sonda sulla griglia</span>

- <span style="color: #4FC3F7;">Electronics 2018, 7, 257 — Strategie di campionamento NF </span>
  - Scopo: rassegna di strategie di campionamento per NF→FF, con obiettivo di ridurre i punti rispetto al passo `λ/2` mantenendo l’accuratezza del FF.
  - Limite informativo inferiore: gradi di libertà del campo/AUT.
  - Campionamento standard: passo `λ/2`; spesso sovracampionamento rispetto al limite; effetti su tempo di misura e troncamento.
  - Linee pratiche: usare `λ/2` come baseline; per ridurre punti applicare MRSS con margine di sicurezza sui DOF previsti; apodizzazione; riferimento di fase; validare l’FF con antenne di riferimento.
  - Implicazioni per il nostro setup: riduzione di punti e tempo; definire set MRSS per AESA; aggiornare pipeline NF→FF e diagnostica; pianificare verifiche e budget d’incertezza.
  - Procedura MRSS
    1) Input: dimensioni AUT (`D`), frequenza (`f`/`λ`), distanza piano `z0`, dinamica desiderata (dB) e banda/polarizzazione della sonda.
    2) Stima DOF: usa `NDF ≈ 2·(D/λ)` per array lineari o `NDF ≈ 4·(A/λ^2)` per aperture planari; aggiungere margine per dinamica (tipicamente +10–20%).
    3) Densità MRSS: costruire una mappa di densità più fitta dove il campo varia (vicino all’asse principale, bordi/autocorr); impostare distribuzioni tipo Chebyshev o profili radiali/lineari non uniformi.
    4) Generazione posizioni: calcolare coordinate non uniformi su X,Y con controllo di errore di posizionamento; assicurarsi di poter ricampionare su griglia uniforme (regridding) mantenendo fase.
    5) Acquisizione: impostare riferimento di fase stabile, controlla drift, registrare ampiezza/fase in ciascun punto MRSS; conservare metadati (IFBW, media, potenza).
    6) Regridding: ricostruire griglia uniforme tramite kernel bandlimitati (sinc finestrata) o interpolatori min‑errore; applicare correzione sonda e apodizzazione.
    7) Verifica FF: confrontare con misura `λ/2` su antenna standard; metriche: errore RMS dB, sidelobes, beam pointing/squint, cross‑pol.
  - <span style="color: #f8df00ff;">**electronics-07-00257** **=>** MRSS porta al limite teorico dei gradi di libertà, quindi con interpolazione ricostruisco griglia uniforme e FFT veloce</span>
  - <span style="color: #f8df00ff;">**electronics-07-00257 =>** I gradi di liberà corrispondono a quante info indip contiene il campo. Fortemente influenzati dalle dimensioni elettriche dell'antenna (+ grande => + gradi), dalla distanza dall'antenna (+ lontano => - gradi (solo componenti propaganti)).</span>
  - <span style="color: #f8df00ff;">Quindi per antenne lineari NDF = 2 * (D/λ); mentre per antenne con una superficie si ha NDF = 4 * A/(λ^2)</span>
  - <span style="color: #f8df00ff;">I valori singolari di A corrispondono alla radice quadrata degli autovalori di A^+ * A e sono numeri reali non negativi decrescenti. Dove A è la funzione che mappa J (densità di corrente) in E (campo elettrico).</span>


- <span style="color: #4FC3F7;">How to Design and Test a Phased Array Antenna </span>
  
  - Obiettivo e contesto
    - Definire requisiti di sistema e di test per array a fasi in microonde/mmWave.
    - Descrivere principi di beamforming e implicazioni di misura OTA (Over‑The‑Air).
  
  - Architetture di beamforming
    - Analogico (controllo di fase/ampiezza per elemento), Digitale (per canale con ADC/DAC), Ibrido (subarray analogici con precoding digitale).
    - Moduli TRx: catene RF per elemento/subarray; controlli di fase `ϕ` e ampiezza `A`; quantizzazione (bit) e step minimi.
    - Distribuzione LO e sincronizzazione: riferimento di fase comune, drift e jitter; clock e trigger.
  
  - Geometrie e spaziatura degli elementi
    - `d ≤ λ/2` per evitare grating lobes entro lo scan massimo; progetto del volume di scansione (`θ_max`), scan loss e degradazioni di guadagno.
    - Pitch, fill factor e interazione con radome/superstrati; mutual coupling e pattern del singolo elemento.
  
  - Pattern e sidelobes
    - Composizione `Array Factor × Element Pattern`; taper (Taylor/Chebyshev/DPSS) per controllo dei sidelobes e bilanciamento guadagno–SLL.
    - Beam squint in banda larga; mitigazioni con true time delay, sub‑band e equalizzazione.
  
  - Prestazioni chiave e figure di merito
    - `EIRP`, `EIS`, `G/T`, `NF`, efficienza, linearità (`IP3`, `P1dB`, `ACPR`), leakage e spuri.
    - Coerenza di fase tra canali, equalizzazione di ampiezza, calibrazione vs temperatura e invecchiamento.
  
  - Calibrazione e allineamento
    - Metodi: built‑in couplers/loopback, near‑field probe, antenna di riferimento; correzioni di ampiezza/fase per boresight e scan.
    - Modelli di errore: offset di fase, quantizzazione, compressione; aggiornamenti periodici via BIST e tabelle di compensazione.
  
  - Strategie di test (OTA/cablato)
    - OTA in camera anecoica: pattern, `EIRP/EIS/TRP`, cross‑pol; set‑up meccanico e sincronizzazione; time gating e gestione della dinamica.
    - Cablato: caratterizzazione canali (S‑parametri), linearità e rumore; verifica dei controlli di fase/ampiezza; interfacce di controllo e logging.
    - Metriche per array: pointing accuracy, beam squint, sidelobes, scan‑range, switching speed; calibrazione su codebook.
  
  - Automazione e controllo
    - Codebook di beam e sequenze di misura; sweeping di fase/ampiezza; tracciabilità e ripetibilità.
    - Integrazione con generatori, VNA, analizzatori, posizionatori; sincronizzazione tra strumenti (trigger/clock).
  
  - Criticità e mitigazioni
    - Grating lobes per `d > λ/2`, scan loss con `θ` elevati, quantizzazione dei controlli.
    - Drift termico, phase noise LO, mismatch inter‑canale, radome che altera l’element pattern; verifiche ambientali.
  
  - Linee guida operative
    - Definire obiettivi di `SLL`, `EIRP/EIS`, scan e banda; scegliere `d` e taper; pianificare la catena di calibrazione.
    - Progettare il test OTA: range/quiete zone, sonda/ricevitore, dinamica e incertezze; definire KPI e criteri di accettazione.
  
  - Pagine successive (sintesi in 2 righe)
    - Coprono esempi di test di produzione/automazione, conformità 5G FR2 e strategie di scalabilità del collaudo per array di grandi dimensioni.
    - Evidenziano processi di validazione, KPI pratici ed integrazione BIST per manutenzione e ricalibra automatica.

- Riferimenti
  - Balanis, “Antenna Theory”, Chapter 17 (NF measurements, planar NF→FF). File: `NF-FF/Balanis_Chapter_17.pdf`.
  - IEEE Std 149-1979: `NF-FF/IEEE 149-1979.pdf`.
  - IEEE Std 149-2021: `NF-FF/IEEE 149-2021.pdf`.
  - Linee guida complementari: `NF-FF/IEEE 1720-2012.pdf`.
  - Electronics 2018, 7, 257 (doi:10.3390/electronics7100257): `NF-FF/electronics-07-00257.pdf`.
  - Probes correction for planar near field antennas measurements (MMS 2015): `NF-FF/Probes_correction_for_planar_near_field_antennas_measurements.pdf`.
  - How to Design and Test a Phased Array Antenna (AESA): `AESA/How to Design and Test a Phased Array Antenna.pdf`.
---


## <span style="color: #e69a44ff;">Settimana 2</span>

- <span style="color: #4FC3F7;">Near-Field Scanning Measurements — Scan planare</span>
  - Obiettivo e ambito
    - Focus esclusivo sulla scansione planare (X–Y) a distanza fissa `z0`.
    - Ricostruzione del FF tramite spettro di onde piane (2D FFT) delle componenti di campo tangenziali misurate sul piano.
  - Setup e strumentazione
    - AUT fissata su supporto a basso scattering; piano di scansione ortogonale e planare; posizionatore X–Y con accuratezza di passo e ripetibilità elevate.
    - Controllo della distanza `z0` stabile nel tempo; tilt/bow del piano minimizzati; riferimenti di posizione e allineamento (boresight) tracciabili.
    - Sonda ricevente a polarizzazione nota; preferibilmente non direttiva (pattern ampio) e fisicamente contenuta rispetto a `λ` per preservare risoluzione spaziale; gestione della banda utile e della cross‑pol.
  - Misura delle componenti di campo
    - Acquisire il campo tangenziale `E_t` (due componenti) o una tensione proporzionale `V` con VNA/receiver; registrare ampiezza e fase per ciascun punto della griglia.
    - Stabilizzare il riferimento di fase (clock/trigger condivisi, cavi coerenti); scegliere IFBW e medie per SNR/dinamica; controllare drift e jitter.
  - Griglia di scansione e campionamento
    - Passi `Δx, Δy ≤ λ/2` per evitare aliasing; oversampling (`λ/3`–`λ/4`) utile quando si richiede FF ad alta dinamica (>40–50 dB) e sidelobes bassi.
    - Area di scansione: estendere oltre l’apertura effettiva della AUT con margine (≥ 0.5–1 `λ` per lato) per ridurre troncamento e ripple da bordi.
    - Applicare finestre di apodizzazione (Hann/Hamming/Taylor) per mitigare artefatti da troncamento; usare zero‑padding per migliorare densità angolare nel dominio `θ, φ`.
  - Probe correction (de‑embedding)
    - Il segnale `V(x,y)` è la convoluzione tra il campo della AUT e la risposta spaziale della sonda `H`; correggere in dominio `(k_x, k_y)` dividendo per `H(k_x, k_y)` con regolarizzazione per limitare l’amplificazione del rumore.
    - Allineare correttamente la polarizzazione; caratterizzare `H` via misura o simulazioni EM alla stessa distanza `z0`; considerare cross‑pol e banda operativa.
    <details class="md-box">
      <summary class="md-box-title">Procedura per la costruzione della matrice P (Probe Correction)</summary>
      <div class="md-box-body">
        <h4>Dati necessari</h4>
        <ul>
          <li>Pattern di radiazione della sonda in campo lontano (FF), in due polarizzazioni ortogonali.</li>
          <li>Per ogni direzione angolare (theta, phi): ampiezza e fase delle componenti co‑polar e cross‑polar.</li>
        </ul>

        <ol>
          <li>
            <strong>Conversione dei dati</strong>
            <ul>
              <li>Trasforma ampiezza + fase in valori complessi:</li>
              <li><code>H_co = ampiezza_co × exp(i × fase_co)</code></li>
              <li><code>H_cross = ampiezza_cross × exp(i × fase_cross)</code></li>
            </ul>
          </li>
          <li>
            <strong>Allineamento</strong>
            <ul>
              <li>Porta i dati della sonda sulla stessa griglia angolare della AUT.</li>
              <li>Assicurati che la fase sia riferita a un unico riferimento comune.</li>
            </ul>
          </li>
          <li>
            <strong>Ottenimento della seconda colonna</strong>
            <ul>
              <li><strong>Sonda lineare ruotata di 90°</strong>: misura nuovamente co/cross → ottieni la seconda colonna.</li>
              <li><strong>Sonda dual‑port</strong>: usa i pattern dei due porti, con ampiezza e fase relative note.</li>
              <li><strong>Caso ideale (sonda quasi ideale)</strong>: assumi P diagonale (solo co e cross, senza termini di accoppiamento).</li>
            </ul>
          </li>
          <li>
            <strong>Costruzione della matrice P</strong>
            <ul>
              <li>Per ogni angolo (theta, phi):</li>
            </ul>
            <p>P = [[col1_co, col2_co], [col1_cross, col2_cross]]</p>
            <ul>
              <li>Dove col1 e col2 sono i valori complessi ottenuti dalle due configurazioni ortogonali.</li>
            </ul>
          </li>
          <li>
            <strong>Verifica della matrice</strong>
            <ul>
              <li>Controlla la condizione numerica di P (determinante o rapporto tra singolari).</li>
              <li>Se P è mal condizionata, applica regolarizzazione o limita l’analisi alle regioni angolari affidabili.</li>
            </ul>
          </li>
          <li>
            <strong>Applicazione della correzione</strong>
            <ul>
              <li>Per ogni direzione: <code>E_aut = P⁻¹ × E_measured</code></li>
              <li>Dove:
                <ul>
                  <li><code>E_measured</code> = campo ottenuto dalla trasformazione NF→FF della AUT.</li>
                  <li><code>E_aut</code> = campo corretto della AUT, libero dall’influenza della sonda.</li>
                </ul>
              </li>
            </ul>
          </li>
        </ol>

        <h4>Note pratiche</h4>
        <ul>
          <li>Co/cross sono definiti rispetto all’orientazione della sonda: mantieni coerenza con la base scelta.</li>
          <li>La rotazione di 90° della sonda può introdurre cambi di segno/fase: verifica con attenzione.</li>
          <li>Nei sistemi multi‑porta, la fase relativa tra porti deve essere misurata in FF commutando tra le porte.</li>
          <li>Se la sonda ha cross‑pol elevato, i termini fuori diagonale di P non sono trascurabili.</li>
        </ul>
      </div>
    </details>
  - Trasformazione NF→FF planare
    - Convertire il campo corretto `E_t(x,y,z0)` in spettro planare `E_t(k_x,k_y,z0)` tramite 2D FFT (spettro di onde piane).
    - Separare componenti propaganti (`k_x^2 + k_y^2 ≤ k^2`) da quelle evanescenti; calcolare `k_z = √(k^2 − k_x^2 − k_y^2)` per propagazione.
    - Propagare a distanza `R` e trasformare in coordinate angolari (`θ, φ`); ricombinare le componenti per ottenere `E_θ, E_φ`, intensità e polarizzazioni copol/cross‑pol.
  - Normalizzazione e calibrazione
    - Calibrare ampiezza/fase della catena di misura e della sonda (antenne standard o loopback); normalizzare il FF rispetto a potenza/impedenza e convertire a `dBi/dB`.
    - Applicare correzioni sistematiche (cavi, attenuatori, drift) e verifiche periodiche con antenne di riferimento.
  - Errori e incertezze
    - Principali contributi: troncamento dell’area, aliasing (passo troppo grande), misregistrazione (errori di posizionamento), drift di fase/ampiezza, multi‑path residuo.
    - Geometria: non planarità/tilt del piano, errore sulla distanza `z0`, non idealità e banda limitata della sonda.
    - Costruire budget d’incertezza separando i contributi di strumentazione, geometria, sonda e post‑processing; propagare l’incertezza all’FF.
  - Linee pratiche operative
    - Usare `Δx=Δy ≈ λ/2` come baseline; impiegare oversampling quando la dinamica richiesta è elevata e si vuole contenere `SLL`.
    - Impostare un margine dell’area di misura di almeno `1 λ` per lato; testare sensibilità rispetto a margine e finestra applicata.
    - Validare la pipeline con AUT note e confronti con FF diretto quando disponibile; controllare errori RMS (dB) sul pattern, sidelobes e pointing.
    - Loggare metadati (frequenza, `z0`, IFBW, medie, potenza TX, temperatura) e controllare periodicamente drift e coerenza di fase.
  - Output e KPI
    - Pattern 2D/3D (`E_θ, E_φ`), `HPBW`, `SLL`, pointing accuracy, copol/cross‑pol, dinamica; errori RMS rispetto a riferimento; ripetibilità tra sessioni.
  - Integrazione con pipeline NF→FF
    - Sequenza tipica: acquisizione su griglia → (eventuale) regridding MRSS → correzione sonda → 2D FFT → filtraggio evanescente → conversione angolare → normalizzazione → esportazione.
    - Configurare finestra e zero‑padding per ottimizzare risoluzione e ridurre artefatti angolari; attenzione all’aliasing dei picchi.



- Riferimenti
  - nf2ff_transformation — Algoritmi NF→FF (Matlab): codice analizzato per soluzioni pratiche di trasformazione NF→FF e compensazione della sonda; usato come spunto per gli script Python della pipeline. Link: https://github.com/hbartle/nf2ff_transformation
  - Near-Field Scanning Measurements — Scan planare (setup, campionamento, correzioni, incertezze). File: `NF-FF/Near-Field_Scanning_Measurements.pdf`.
  - Documentazione R&S per misure di antenne AESA: https://www.rohde-schwarz.com/it/applicazioni/misura-di-un-antenna-con-beamforming-in-modalita-di-trasmissione-nota-di-applicazione_56280-408715.html

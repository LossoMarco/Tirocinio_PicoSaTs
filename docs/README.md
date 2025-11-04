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
  - Si calcola lo spettro spaziale via 2D FFT.
  - Si effettua la correzione di polarizzazione/sonda e la ricombinazione in campo lontano.

- Campionamento e area di scansione
  - Passo di campionamento tipico ≤ `λ/2` (spesso più fine per dinamica elevata).
  - Area di scansione: estendere oltre il contorno dell’AUT (margine) per ridurre troncamento.
  - Finestre (es. Hann) utili per mitigare ripple e lobi laterali dovuti a bordi finiti.

- Sonda e correzione (probe correction)
  - Il campo misurato è la convoluzione con la risposta della sonda; serve deconvoluzione/correzione.
  - È necessaria la caratterizzazione della sonda (fattore di correzione) e l’allineamento di polarizzazione.
- <span class="md-note">**Tutti =>** Che antenna usare => Tromba o guida d'onda non direttiva, lobo principale che prende tutto il FOV della AUT</span>
- <span class="md-note">**Probes correction for planar near field antennas measurements =>** Nel metodo della deconvoluzione si ha che **E** è il campo elettrico da trovare. **V** è la tensione misurata e **H** è la risposta spaziale della sonda, quindi è cosa nota. Si ha che **V = E conv H**. quindi per trovare **E** devo dividere (nel dominio della frequenza)  **V/H** e fare la trasformata inversa.</span>
- <span class="md-note">Per ottenere **H** si fanno delle simualzioni i CST come se fosse la misura, da stare attenti a mantenere la distanza della simualzione uguale a quella della misura reale</span>

- Principali fonti di errore
  - Troncamento del piano, aliasing (passo troppo grande), drift di fase/ampiezza.
  - Disallineamenti meccanici/angolari, multi-path residuo, non idealità della sonda.

- Note operative
- <span class="md-note">**Measurements_of_Antenna_Radiation_Pattern_Laboratory_Manual =>** A pagina 4 si parla di relative radiation pattern, quindi si fa la misura del pattern relativo alla AUT, quindi la calibrazione non è necessaria.</span>

- <span class="md-cite">Linee guida IEEE 149 (1979, 2021)</span>
  - Scopo: pratiche raccomandate per misure di pattern, guadagno, polarizzazione e parametri associati in gamme di prova (anechoic, outdoor, compact, near-field).
  - 2021: aggiornamenti su metodi NF (planare/sferico/cilindrico), analisi dell'incertezza, requisiti software/strumentazione, sicurezza e high-power, verifiche e terminologia aggiornata (quiet zone, target support, tracking).
  - Quiet zone e range: definizione dell’area utile, controllo riflessioni (assorbitori, ground bounce), materiali del supporto AUT a basso scattering, gestione delle diffrazioni dei bordi.
  - Scanner planare: planarità/ortogonalità assi, accuratezza di passo e ripetibilità, calibrazione del piano di misura, riferimenti di posizione e controllo dell’errore di posizionamento.
  - Incertezze: contributi da strumentazione, sonda, allineamento, ambiente e post-elaborazione; costruzione del budget e propagazione all’FF dopo la trasformazione.
- <span class="md-note">**IEEE Std 149-2021 =>** La griglia deve soddisfare il criterio di misura per cui devo captare segnali di -45 dB rispetto alla misura massima sulla mia AUT</span>
- <span class="md-note">**IEEE Std 149-2021 =>** L'antenna caratterizzante va puntata verso la AUT? NO, vengono usati i metodi di compensazione della sonda con il teorema di reciprocità di Lorentz applicato al campo vicino. Il campo misurato è la convoluzione dei due campi, quindi vanno de-convoluti. Per mantenere un sistema di riferimento coerente e gli algoritmi FFT per NF/FF assumono che la sonda sia fissa (oltre alla AUT), inoltre aggiungere gradi di liberà rende tutto più difficile e costoso.</span>
- <span class="md-note">**IEEE 149-2021 =>** Distanza dalla AUT almeno 3λ per NF, chiaramente più piccola del FF 2D^2/λ</span>

- <span class="md-cite">IEEE 1720-2012 — Pratiche di misura NF</span>
  - Ambito e geometrie: planare, sferica, cilindrica; criteri di scelta in base a dimensioni AUT, direzioni richieste e vincoli meccanici/ambientali.
  - Scanner e posizionamento: planarità e ortogonalità degli assi, accuratezza del passo, controllo della distanza `z0`, tilt e bow.
  - Campionamento e area di scansione: passi tipici ≤ `λ/2` (con oversampling se necessario per dinamica); estensione dell’area oltre i bordi AUT; apodizzazione (Hann/Hamming) per ridurre ripple da troncamento.
  - Sonda e correzione: caratterizzazione complessa della sonda (ampiezza/fase).
  - Trasformazioni NF→FF:
    - Planare: spettro di onde piane con 2D FFT.
    - Sferica: espansione in armoniche sferiche; requisiti di copertura angolare e numero di punti; ricostruzione completa del pattern.
    - Cilindrica: espansione in armoniche cilindriche per antenne allungate.
  - Verifica e validazione: uso di antenne standard (SGH).
  - Output e metriche: pattern 2D/3D, guadagno/directivity, cross-pol.
- <span class="md-note">**IEEE Std 1720‑2012 =>** Devo avere un XPD di 10/15 dB migliore rispetto alla AUT, il che implica un axial ratio molto vicino a 0 dB</span>
- <span class="md-note">**IEEE Std 1720‑2012 / An_overview_of_near-field_antenna_measurements =>** Si preferisce la sonda non direttiva, pattern ampio. Sonda piccola rispetto a λ, altrimenti perdo risoluzione.</span>
- <span class="md-note">**IEEE Std 1720‑2012 =>** Buona pratica vuole che si facciano piu misure con punti di acquisizione spostati di λ/8 e anche la distanza tra le antenne per mediare le riflessioni tra le stesse</span>
- <span class="md-note">**IEEE Std 1720‑2012 =>** Test sui cavi flessibili (system phase error) => utilizzo un cavo di loop back al posto della AUT e vedo come varia il segnale muovendo la sonda sulla griglia</span>

- <span class="md-cite">Electronics 2018, 7, 257 — Strategie di campionamento NF </span>
  - Scopo: rassegna di strategie di campionamento per NF→FF, con obiettivo di ridurre i punti rispetto al passo `λ/2` mantenendo l’accuratezza del FF.
  - Limite informativo inferiore: gradi di libertà del campo/AUT.
  - Campionamento standard: passo `λ/2`; spesso sovracampionamento rispetto al limite; effetti su tempo di misura e troncamento.
  - Linee pratiche: usare `λ/2` come baseline; per ridurre punti applicare MRSS con margine di sicurezza sui DOF (Degree Of Freedom) previsti.
  <details class="md-box">
    <summary class="md-box-title">Procedura MRSS</summary>
    <div class="md-box-body">
      <ol>
        <li>Input: dimensioni AUT (<code>D</code>), frequenza (<code>f</code>/<code>λ</code>), distanza piano <code>z0</code>, dinamica desiderata (dB) e banda/polarizzazione della sonda.</li>
        <li>Stima DOF: usa <code>NDF ≈ 2·(D/λ)</code> per array lineari o <code>NDF ≈ 4·(A/λ^2)</code> per aperture planari; aggiungere margine per dinamica (tipicamente +10–20%).</li>
        <li>Densità MRSS: costruire una mappa di densità più fitta dove il campo varia (vicino all’asse principale, bordi/autocorr); impostare distribuzioni tipo Chebyshev o profili radiali/lineari non uniformi. E' necessario parametrizzare il campo in modo da ottenere una sua descrizione più "ampia" dove il campo varia più velocemente.</li>
        <li>Campiono con nyquisto su questa nuova parametrizzazione. Calcolare coordinate non uniformi su X,Y con controllo di errore di posizionamento.</li>
        <li>Acquisizione: registrare ampiezza/fase in ciascun punto MRSS; conservare metadati (IFBW, media, potenza).</li>
        <li>Regridding: ricostruire griglia uniforme tramite kernel bandlimitati (sinc finestrata) o interpolatori min‑errore; applicare correzione sonda e apodizzazione.</li>
        <li>Verifica FF: confrontare con misura <code>λ/2</code> su antenna standard; metriche: errore RMS dB, sidelobes, beam pointing/squint, cross‑pol.</li>
      </ol>
    </div>
  </details>
- <span class="md-note">**electronics-07-00257** **=>** MRSS porta al limite teorico dei gradi di libertà, quindi con interpolazione ricostruisco griglia uniforme e FFT veloce</span>
- <span class="md-note">**electronics-07-00257 =>** I gradi di liberà corrispondono a quante info indip contiene il campo. Fortemente influenzati dalle dimensioni elettriche dell'antenna (+ grande => + gradi), dalla distanza dall'antenna (+ lontano => - gradi (solo componenti propaganti)).</span>
- <span class="md-note">Quindi per antenne lineari NDF = 2 * (D/λ); mentre per antenne con una superficie si ha NDF = 4 * A/(λ^2)</span>


- <span class="md-cite">How to Design and Test a Phased Array Antenna </span>
  
  - Obiettivo e contesto
    - Definire requisiti di sistema e di test per array a fasi in microonde/mmWave.
    - Descrivere principi di beamforming e implicazioni di misura OTA (Over‑The‑Air).
  
  - Architetture di beamforming
    - Analogico (controllo di fase/ampiezza per elemento), Digitale (per canale con ADC/DAC), Ibrido (subarray analogici con precoding digitale).
    - Moduli TRx: catene RF per elemento/subarray; controlli di fase `ϕ` e ampiezza `A`.
  
  - Geometrie e spaziatura degli elementi
    - `d ≤ λ/2` per evitare grating lobes entro lo scan massimo; progetto del volume di scansione (`θ_max`), scan loss e degradazioni di guadagno.
    - Pitch, fill factor; mutual coupling e pattern del singolo elemento.
- <span class="md-note">**How to Design and Test a Phased Array Antenna =>** Il pitch (spaziatura tra elementi) deve rispettare `d ≤ λ/2` per evitare grating lobes entro l'angolo di scansione previsto; il fill factor (rapporto tra area attiva e superficie totale) influenza efficienza e perdite, specie con radome/superstrati; il mutual coupling modifica l'element pattern e può introdurre variazioni di fase/ampiezza tra canali, richiedendo modellazione e calibrazione attiva.</span>
  
  - Pattern e sidelobes
    - Composizione `Array Factor × Element Pattern`; taper (Taylor/Chebyshev/DPSS) per controllo dei sidelobes e bilanciamento guadagno–SLL.
    - Beam squint in banda larga; mitigazioni con true time delay, sub‑band e equalizzazione.
  
  - Prestazioni chiave e figure di merito
    - `EIRP`, `EIS`, `G/T`, `NF`, efficienza, linearità (`IP3`, `P1dB`, `ACPR`), leakage e spuri.
    - Coerenza di fase tra canali, equalizzazione di ampiezza, calibrazione vs temperatura.
  - <span class="md-note">**How to Design and Test a Phased Array Antenna =>** `G/T` quantifica la capacità di ricevere segnali deboli rispetto al rumore; la misura si esegue con Y‑factor usando sorgenti HOT (6000 K) e COLD (290 K).</span>
  
  - Strategie di test (OTA/cablato)
    - OTA in camera anecoica: pattern, `EIRP/EIS`, cross‑pol; set‑up meccanico e sincronizzazione; time gating e gestione della dinamica.
    - Cablato: caratterizzazione canali (S‑parametri), linearità e rumore; verifica dei controlli di fase/ampiezza; interfacce di controllo e logging.
    - Metriche per array: pointing accuracy, beam squint, sidelobes, scan‑range, switching speed.
  - <span class="md-note">**How to Design and Test a Phased Array Antenna =>** Il time gating definisce la finestra temporale di acquisizione per isolare il primo impulso ed escludere riflessioni successive e contributi di rumore.</span>
  
  - Automazione e controllo
    - Codebook di beam e sequenze di misura; sweeping di fase/ampiezza; tracciabilità e ripetibilità.
    - Integrazione con generatori, VNA, analizzatori, posizionatori; sincronizzazione tra strumenti (trigger/clock).
  - <span class="md-note">**How to Design and Test a Phased Array Antenna =>** Il beam codebook è un insieme discreto di pattern/fasci impiegati per lo steering; riduce la complessità del controllo e abilita test ripetibili su fasci predefiniti.</span>
  
  - Criticità e mitigazioni
    - Grating lobes per `d > λ/2`, scan loss con `θ` elevati, quantizzazione dei controlli.
    - Drift termico, phase noise LO, mismatch inter‑canale, radome che altera l’element pattern; verifiche ambientali.
  
  - Linee guida operative
    - Definire obiettivi di `SLL`, `EIRP/EIS`, scan e banda; scegliere `d` e taper; pianificare la catena di calibrazione.
    - Progettare il test OTA: range/quiete zone, sonda/ricevitore, dinamica e incertezze; definire KPI e criteri di accettazione.

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


- <span class="md-cite">Sintesi Balanis — Capitolo 6 (Array di antenne)</span>
  - Concetti chiave
    - Pattern multiplication: pattern totale = elemento × fattore d’array (AF).
    - AF, spaziatura `d`, ampiezze/fasi; broadside, endfire, scanning.
  - Fattore d’array e legge di puntamento
    - `AF(θ) = \sum a_n · e^{j(nψ)}`, con `ψ = k d cosθ + β`.
    - Massimi/minimi: `ψ = 2πm` (massimi); nulls posizionabili via eccitazioni (`a_n`, `β`).
    - Puntamento con `β`: broadside `β = 0`, endfire `β ≈ ± k d`.
  - Spaziatura e lobi
    - Vincolo `d ≤ λ/2` per evitare grating lobes (specie in scansione).
    - Compromesso tra `HPBW` e `SLL`.
    - Effetto di `N` e `d` su larghezza del fascio; scan loss a `θ` elevati.
  - Pesature e sintesi
    - Binomiale, Dolph–Chebyshev, Taylor; metodi Schelkunoff e Woodward–Lawson.
    - Sintesi continua (Fourier/aperture) e discretizzazione su array reali.
    - Thinned/sparse arrays: riduzione elementi con SLL accettabile; trade‑off in guadagno.
  - Broadside vs endfire
    - Broadside: mainlobe centrato; `HPBW` regolato dal taper.
    - Endfire: `β ≈ k d`; correzione Hansen–Woodyard per stringere il fascio.
  - Null steering e multi‑beam
    - Null direzionali su interferenti mediante controllo di `a_n`/`β`.
    - Multi‑beam via reti (Butler/Rotman) o digitale (codebook e pesi).
  - Array planari/circolari
    - Fattorizzazione in due AF; scansione in azimut/elevazione.
    - Element pattern attivo; accoppiamento mutuo.
    - Planari rettangolari: separabilità `AF_x × AF_y`; directivity ~ `N_x · N_y` (uniforme, appross.)
    - Circolari: scanning azimutale, simmetria e gestione della polarizzazione.
  - Wideband e TTD
    - Beam squint in sola fase; mitigazione con ritardi di tempo (TTD).
    - Sub‑band, equalizzazione e compensazioni per ridurre squint.
  - Errori e tolleranze
    - Errori di ampiezza/fase/quantizzazione → pointing error e `SLL↑`.
    - Fail di elementi → degrado guadagno e ripple; necessaria calibrazione.
    - Mutuo accoppiamento → varia impedenza attiva e element pattern; serve correzione.
  - KPI e linee guida
    - Directivity/guadagno, `HPBW`, `SLL`, pointing/scan range, efficienza.
    - Scelte di `d`, taper e calibrazione coerenti con banda e obiettivi.
  - Procedura pratica di progetto
    - Definisci `HPBW`/`SLL` target → scegli taper (Chebyshev/Taylor).
    - Verifica `d` rispetto scan range → evita grating lobes.
    - Stima `N` e apertura per i KPI desiderati; valuta scan loss.
    - Costruisci/valida codebook; calibrazione OTA/cablata e controllo di coerenza.

- <span class="md-cite">Near-Field Scanning Measurements — Scan planare</span>
  - Obiettivo e ambito
    - Focus esclusivo sulla scansione planare (X–Y) a distanza fissa `z0`.
    - Ricostruzione del FF tramite spettro di onde piane (2D FFT) delle componenti di campo tangenziali misurate sul piano.
  - Setup e strumentazione
    - AUT fissata su supporto a basso scattering; piano di scansione ortogonale e planare; posizionatore X–Y con accuratezza di passo e ripetibilità elevate.
    - Controllo della distanza `z0` stabile nel tempo.
    - Sonda ricevente a polarizzazione nota; preferibilmente non direttiva (pattern ampio) e fisicamente contenuta rispetto a `λ` per preservare risoluzione spaziale; gestione della banda utile e della cross‑pol.
  - <span class="md-note">**Near-Field_Scanning_Measurements =>** La sonda deve essere fisicamente piccola rispetto a `λ` poiché in near‑field non si misura un punto ma una superficie; il guadagno della sonda è preferibilmente 10–20 dB più basso rispetto alla AUT per limitare riflessioni multiple e accoppiamenti indesiderati.</span>
  - Misura delle componenti di campo
    - Acquisire il campo tangenziale `E_t` (due componenti) o una tensione proporzionale `V` con VNA/receiver; registrare ampiezza e fase per ciascun punto della griglia.
    - Stabilizzare il riferimento di fase (clock/trigger condivisi, cavi coerenti); scegliere IFBW e medie per SNR/dinamica; controllare drift e jitter.
  - Griglia di scansione e campionamento
    - Passi `Δx, Δy ≤ λ/2` per evitare aliasing; oversampling (`λ/3`–`λ/4`) utile quando si richiede FF ad alta dinamica (>40–50 dB) e sidelobes bassi.
    - Area di scansione: estendere oltre l’apertura effettiva della AUT con margine (≥ 0.5–1 `λ` per lato) per ridurre troncamento e ripple da bordi.
    - Applicare finestre di apodizzazione (Hann/Hamming/Taylor) per mitigare artefatti da troncamento; usare zero‑padding per migliorare densità angolare nel dominio `θ, φ`.
  - Probe correction (de‑embedding)
    - Allineare correttamente la polarizzazione; caratterizzare `H` via misura o simulazioni EM alla stessa distanza `z0`; considerare cross‑pol e banda operativa.
    - <span class="md-note">**Near-Field_Scanning_Measurements =>** La matrice `P` lega il campo vero della AUT al campo misurato; consente di rimuovere l’interazione della sonda (de‑embedding) ottenendo il campo irradiato della AUT.</span>
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
              <li>Si trasformano ampiezza e fase in valori complessi:</li>
              <li><code>H_co = ampiezza_co × exp(i × fase_co)</code></li>
              <li><code>H_cross = ampiezza_cross × exp(i × fase_cross)</code></li>
            </ul>
          </li>
          <li>
            <strong>Allineamento</strong>
            <ul>
              <li>I dati della sonda vengono portati sulla stessa griglia angolare della AUT.</li>
              <li>Si assicura che la fase sia riferita a un unico riferimento comune.</li>
            </ul>
          </li>
          <li>
            <strong>Ottenimento della seconda colonna</strong>
            <ul>
              <li><strong>Sonda lineare ruotata di 90°</strong>: si misurano nuovamente co/cross → si ottiene la seconda colonna.</li>
              <li><strong>Sonda dual‑port</strong>: si usano i pattern dei due porti, con ampiezza e fase relative note.</li>
              <li><strong>Caso ideale (sonda quasi ideale)</strong>: si assume P diagonale (solo co e cross, senza termini di accoppiamento).</li>
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
              <li>Si controlla la condizione numerica di P (determinante o rapporto tra singolari).</li>
              <li>Se P è mal condizionata, si applica regolarizzazione o si limita l’analisi alle regioni angolari affidabili.</li>
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
          <li>Co/cross sono definiti rispetto all’orientazione della sonda: si mantiene coerenza con la base scelta.</li>
          <li>La rotazione di 90° della sonda può introdurre cambi di segno/fase: si verifica con attenzione.</li>
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


- <span class="md-cite">Error analysis techniques for planar near-field measurements — Sintesi degli errori</span>
  - Introduzione
    - Riepilogo dei contributi d’errore trattati da A.C. Newell (IEEE TAP, 1988) per misure NF planari e loro impatto su correzione di sonda e trasformazione NF→FF.
  - Probe parameter errors (1–4) — impattano direttamente la correzione di sonda `P` e le metriche di polarizzazione.
    - Relative pattern: differenze nel pattern co/cross della sonda (ampiezza/fase) → errori di de‑embedding e leakage copol/cross‑pol.
    - Polarization ratio: incertezza nel rapporto co/cross (XPR/XPD) della sonda → alterazione delle componenti principali e cross della AUT.
    - Gain: errore sul guadagno assoluto della sonda → scala errata dell’FF/EIRP e della normalizzazione.
    - Alignment: disallineamento di orientazione/rotazione della sonda e di `z0` → mixing di polarizzazioni ed errori di fase.
  - Near‑field measurement errors (5–18) — legati al set‑up di misura e al post‑processing.
    - Normalization constant: costante di normalizzazione NF→FF non corretta → scala d’ampiezza/potenza errata.
    - Mismatch: disadattamento di porte/cavi/sonda → ripple e errori di ampiezza.
    - AUT alignment: orientazione/posizionamento AUT non corretti → pointing error e simmetria del pattern alterata.
    - Sampling/aliasing: passi `Δx, Δy` troppo grandi → aliasing spettrale e lobi spurî.
    - Area truncation: area di scansione insufficiente → troncamento e incremento dei sidelobes/ripple.
    - Position error (x/y/z): misregistrazione, tilt del piano, errore su `z0` → sfasamenti e blur del campo.
    - Multiple reflections: multipath tra superfici/strutture → ghost lobes e ripple; utile il time‑gating.
    - Receiver amplitude nonlinearity: compressione/non linearità del ricevitore → errori di ampiezza e distorsione.
    - System phase errors: instabilità/offset di fase del sistema → errori di fase cumulativi e incoerenza.
    - Dynamic range: SNR insufficiente e floor elevato → perdita di dinamica e mis‑stima dei sidelobes.
    - Room scattering: diffrazioni/scattering del range → corruzione del campo misurato.
    - Leakage/crosstalk: accoppiamenti/leakage tra canali/strumenti → componenti spurie nel segnale.
    - Random amplitude/phase errors: rumori casuali e jitter → variabilità statistica; richiede medie e stima d’incertezza.



- <span class="md-cite">Misure OTA in trasmissione per antenne beamforming (R&S 1MA278)</span>
  - Strumenti e modalità R&S
    - Defined Coherence Mode (ZVA): consente di pilotare più porte contemporaneamente con ampiezze e fasi controllate → utile per simulare/verificare beamsteering/beamforming in modalità TX senza ricablaggi.
    - Misura combinata S‑parameters + Beamforming: con un’unica connessione all’AUT si ottengono sia i parametri di scattering sia la risposta in beamforming → aumenta throughput e riduce errori di riconnessione.
    - Scalabilità con daisy‑chain: collegando più VNA si estende il numero di porte → concetto utile per scalare da array piccoli (4 elementi) ad array più grandi (7+ elementi).
  - Differenze concettuali
    - Beamsteering: solo fasi progressive → direzione del lobo principale.
    - Beamforming: fasi + ampiezze → ottimizzazione della discriminazione utente/interferente (codebook).
  - Risultati sperimentali
    - X‑band, 4 elementi con parasitici: dimostrato steering a 41° con offset di fase di 228° → esempio pratico di verifica tra fase applicata e fascio desiderato.
    - Ka‑band, array 4×10 (26–30 GHz):
      - Pattern di singolo elemento utilizzato per calcolare il pattern d’array.
      - Misure reali confermano le predizioni → validazione del modello.
    - Beamforming su 4 elementi:
      - Caso base: discriminazione utente/interferente ≈ 8.9 dB.
      - Solo steering: discriminazione peggiora (≈ 8.6 dB).
      - Beamforming (ampiezze [−2, 0, 0, −2] dB + fasi): discriminazione migliora a ≈ 13.5 dB, con lieve perdita di potenza radiata (≈ −0.9 dB).

- <span class="md-cite">NF→FF Planare — Conversione MATLAB→Python e Plots</span>
  - Obiettivo
    - Portare la trasformazione NF→FF planare da MATLAB (`nf2ff_planar.m`) a Python (`nf2ff_planar_fft.py`) con funzioni di plotting dedicate per valutare la bontà dell’algoritmo.
  - Implementazione Python (solo caso planare, quello di nostro interesse)
    - Pipeline: finestra 2D (Hann/Hamming/Tukey), FFT 2D con padding, spettro di onde piane, interpolazione bilineare su griglia sferica, calcolo componenti FF (`Eθ, Eφ`) e magnitudine `|E|`.
    - Funzioni: `nf2ff_planar_fft(...)` per la trasformazione; `compute_ff_coverage(...)` per stimare la copertura angolare utile in funzione di distanza `z0`, passo di campionamento `Δ` e apertura `L` (minimo tra limite geometrico e di campionamento).
    - Plot: `plot_planar_nf_data.py` contiene grafici 3D/2D per NF e FF (magnitudine, fase).
  - Convenzioni di plotting
    - NF 3D/2D: `X = x [m]`, `Y = y [m]`. La griglia è costruita come `Z[y_index, x_index]` (righe = valori unici di `y`, colonne = valori unici di `x`) per evitare trasposizioni ad hoc e mantenere coerenza tra 3D (`plot_surface`) e 2D (`imshow`).
    - FF 3D/2D: `X = phi [deg]`, `Y = theta [deg]`. Inversione dell’asse `Y` (theta) attiva per allineare la direzione visiva con la convenzione decisa: all’aumentare di `theta` la coordinata `y` deve diminuire (cioè “scendere” nel grafico).
    - Top‑down (2D): colormap blu→giallo per evidenziare zone deboli/forti; supporto `dB` e lineare; `origin="lower"` per far crescere l’asse verticale verso l’alto.
  - Giustificazione di scambi/inversioni assi (NF↔FF)
    - Semantica `imshow`: la prima dimensione della matrice sono le righe (asse `Y`), la seconda le colonne (asse `X`). Se i dati NF sono memorizzati come `Z[x_index, y_index]` (ordine di acquisizione x‑major), in 2D si percepisce “X/Y scambiati”. La soluzione robusta adottata è costruire sempre la griglia ordinata e monotona `Z[y_index, x_index]` (righe = `y`, colonne = `x`) prima del plotting.
    - Semantica `meshgrid`: in 3D si usano coordinate esplicite `X, Y`. Allineando la costruzione della griglia NF a `Z[y, x]`, i plot 3D e 2D condividono la stessa interpretazione degli assi e non richiedono trasposizioni.
    - Reshape FF: l’algoritmo `nf2ff_planar_fft` produce vettori piatti su griglia sferica. Il reshape avviene a `Z_ff.shape = (len(phi), len(theta))` (righe = `phi`, colonne = `theta`), coerente con `meshgrid(..., indexing="ij")`. Per rendere la corrispondenza desiderata “variazione di `y` ↔ variazione di `theta`” si è scelto di mappare i plot FF con `X = phi`, `Y = theta`.
    - Inversione di `theta` (asse Y nei plot FF): graficamente vogliamo che “quando `theta` cresce, `y` cali”. Poiché l’asse verticale cresce verso l’alto, si inverte l’asse `Y` nei plot FF (`invert_theta=True`). Questo è l’unico flip rimasto necessario; ogni altro swap/inversione ad hoc è stato rimosso grazie alla griglia NF `Z[y, x]` e al reshape FF coerente.
    - Taglio `phi = 0°` nella comparativa NF vs FF: per confrontare la colonna NF a `x≈0` (rimappata in `theta`) con il taglio FF a `phi=0°`, la curva FF viene specchiata orizzontalmente (`theta → −theta`). Ciò rende il confronto visivo bilaterale senza dover unire i semipiani `phi=0°/180°`.
  - Dataset utilizzato (per ora)
    - Single slotted waveguide array antenna a ~94 GHz. Dataset: https://nf2ff.sourceforge.net/.
    - Parametri del dataset: `dx = dy ≈ 1 mm`, `Lx = Ly ≈ 50 mm`, `z0 ≈ 6 mm`, `f ≈ 94.075 GHz`.
  - Copertura angolare `θ ≈ 80°`, `φ ≈ 80°`.
  - Risultati e osservazioni principali
    - Nei plot, sia NF che FF, il lobo principale non è centrato (non a `θ = 0°, φ = 0°`) ma risulta a circa `θ ≈ −20°` (con `φ ≈ 0°`).
    - Osservazione interessante: il puntamento del fascio suggerisce che l’antenna sia una traveling‑wave waveguide slotted antenna array.
    - L’algoritmo NF→FF planare converte il dataset e mostra coerenza tra NF e FF nelle regioni coperte dalla geometria/sampling.
  - Riproducibilità
    - Script: `demo_planar_nf.py` — esegue carica dataset, stima copertura, esegue NF→FF e genera i grafici.
    - Comando: `demo_planar_nf.py` (opzionale `--show` per mostrare a schermo).
    - Output grafici salvati in: `Script/nf2ff_python/plots/` (es.: `nf_magnitude_3d_lin.png`, `nf_topdown_lin.png`, `ff_magnitude_3d_db.png`, `ff_topdown_db.png`).
  - Prossimi passi
    - Convalidare trasformazione con dataset con misure sferiche per confronto diretto FF calcolato e FF misurto (reale)
    - Aggiungere probe correction

- <span class="md-cite">Conferma ipotesi — Pattern a ~94 GHz</span>
  - Il confronto con il paper allegato conferma che il pattern di radiazione dell’antenna in esame coincide con quello estratto dagli script Python dal dataset a ~94 GHz.
  - Osservazioni principali: lobo principale intorno a `θ ≈ −20°` con `φ ≈ 0°`, larghezze di fascio comparabili e andamento dei sidelobe coerente tra FF misurato e FF calcolato.
  - Implicazione: la pipeline NF→FF implementata in Python riproduce correttamente il comportamento dell’antenna analizzata.
  - Riferimento: `AESA/Design_Sub-THz_SWAA.pdf`.

- Riferimenti
  - Near-Field Scanning Measurements — Scan planare (setup, campionamento, correzioni, incertezze). File: `NF-FF/Near-Field_Scanning_Measurements.pdf`.
  - Error analysis techniques for planar near-field measurements — Analisi degli errori (Newell, 1988, IEEE TAP). File: `NF-FF/Error_analysis_techniques_for_planar_near-field_measurements.pdf`.
  - Measurement of Beamforming Antenna in Transmit Mode (R&S App Note 1MA278). File: `AESA/Measurement_of_Beamforming_Antenna_in_Transmit_Mode_R&S.pdf`.
  - Balanis, "Antenna Theory: Analysis and Design", 4th ed., Wiley, 2016. File: `Teoria/Antenna theory analysis and design, 4th Edition  Constantine A. Balanis. - New York John Wiley & Sons, 2016.pdf - collegamento.lnk`.
  - nf2ff_transformation — Algoritmi NF→FF (Matlab). Link: https://github.com/hbartle/nf2ff_transformation


## <span style="color: #e69a44ff;">Settimana 3</span>

- <span class="md-cite">Traduzione di campo via spettro di onde piane (PWS‑FFT) — patch ±36° → piano tangente a r=3.198</span>

- Obiettivo
  - Tradurre i dati limitati su patch sferica (±36° attorno al boresight) su un piano tangente a distanza `r=3.198`, assumendo regime paraxiale e campo localmente quasi‑planare.

- Metodo (sintesi operativa)
  - Pre‑elaborazione: convertire `Eθ, Eφ` in componenti cartesiane locali sul patch (es. `Ex, Ey` tangenziali); mantenere un riferimento di fase coerente.
  - Mappatura angolare → spettro: `kx = k · sinθ · cosφ`, `ky = k · sinθ · sinφ`, con `k = 2π/λ`.
  - Costruzione dello spettro discreto: organizzare i dati su griglia adeguata; applicare finestra (Hann/Taylor) e zero‑padding; usare 2D FFT (o NUFFT se la griglia non è uniforme).
  - Propagazione al piano tangente: `kz = sqrt(k^2 − kx^2 − ky^2)`; separare componenti propaganti (`kx^2 + ky^2 ≤ k^2`) dagli evanescenti; moltiplicare per `exp(i · kz · Δr)` con `Δr = r_target − r_ref`.
  - Ricostruzione sul piano: IFFT dello spettro propagato per ottenere `E(x, y, r_target)`; estrarre ampiezza/fase e le componenti di polarizzazione desiderate.
  - Normalizzazione e riferimenti: mantenere origine e convenzioni di unità; documentare eventuali scaling e filtri applicati.

- Note pratiche
  - La copertura limitata ±36° introduce troncamento dello spettro: mitigare con apodizzazione e padding per ridurre ripple/lobi laterali.
  - Evitare aliasing: scegliere passi coerenti con `λ/2` nella parametrizzazione angolare equivalente; controllare densità e uniformità della griglia.
  - Le componenti evanescenti decadono con `Δr`: un filtro dolce (es. Tukey) può stabilizzare la traduzione in presenza di rumore.
  - Se la griglia angolare è irregolare, preferire NUFFT o integrazione con quadrature al posto della FFT su griglia uniforme.

- Confronto metodi
  - PWS‑FFT: semplice, veloce, adatto a patch paraxiale; implementazione diretta con FFT.
  - Huygens/Stratton–Chu: generale e robusto con coperture ampie; più oneroso (correnti equivalenti e integrazione di superficie).
  - Espansione in armoniche sferiche: potente ma sensibile a troncamenti; meno adatta con copertura ±36°.

- Output atteso
  - Campo `E(x, y)` sul piano tangente a `r=3.198`, pronto per plotting/analisi (mappe di ampiezza/fase, sezioni) e, se necessario, ulteriore proiezione in FF via PWS.

### Riferimenti
- `NF-FF/Plane_Wave_Based_NFFFT_with_Adaptive_Field_Translation.pdf` — Traduzione del campo basata su spettro di onde piane e adattamento.
- `NF-FF/Determination_of_far-field_antenna_patterns_from_near-field_measurements.pdf` — Fondamenti PWS/FFT, mappatura `kx, ky` e propagazione `exp(i kz Δr)`.
- `NF-FF/Electromagnetic Field Transformations for Measurements.pdf` — Rassegna metodi NF→FF; sezioni su angular spectrum e propagazione tra superfici.
- `NF-FF/IEEE 149-2021.pdf` — Raccomandazioni per trasformazioni planari via PWS e requisiti di campionamento.
- `NF-FF/Near-Field_Scanning_Measurements.pdf` — Tutorial pratico su acquisizione e trasformazioni; discretizzazione e gestione evanescenti.
- `NF-FF/Error_analysis_techniques_for_planar_near-field_measurements.pdf` — Windowing, padding e impatto della copertura limitata.
- `NF-FF/Balanis_Chapter_17.pdf` — Base teorica (Green/Huygens) e collegamenti con rappresentazioni in onde piane.
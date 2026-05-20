# Chapter I - Food Webs Notebook

Questo README descrive in dettaglio il notebook `Chapter_I_FoodWebs_Notebook.ipynb`, aggiornato per Python 3. Il notebook introduce i concetti di base dell'analisi delle reti trofiche usando prima strutture dati elementari, come matrici di adiacenza, e poi la libreria NetworkX.

## Contenuto della cartella

- `Chapter_I_FoodWebs_Notebook.ipynb`: notebook principale del capitolo sulle food webs.
- `data/Ythan_Estuary.txt`: rete trofica usata per l'esempio bow-tie.
- `data/Little_Rock_Lake.txt`: rete trofica usata per l'analisi delle specie trofiche.
- `data/St_Marks_Seagrass.txt`, `data/St_Martin_Island.txt`, `data/Grassland.txt`, `data/Silwood_Park.txt`: altri dataset di reti trofiche disponibili nella cartella.
- `data/Chapter_I_data_sources.txt`: note sulle fonti dei dati.
- `data/bowtie.png`: immagine generata dal notebook per la struttura bow-tie.

## Obiettivo del notebook

Il notebook mostra come rappresentare, leggere, analizzare e visualizzare reti ecologiche dirette e non dirette. Il filo conduttore e una food web, cioe una rete in cui i nodi rappresentano specie o gruppi trofici e gli archi rappresentano relazioni di predazione, consumo o flusso trofico.

Il percorso didattico e progressivo:

1. costruzione manuale di piccole matrici di adiacenza;
2. calcolo di misure di base senza librerie esterne;
3. passaggio a NetworkX per rappresentare grafi;
4. visualizzazione di distribuzioni e sottostrutture;
5. analisi di dataset reali di reti trofiche;
6. classificazione delle specie in basali, intermedie e top;
7. calcolo di proporzioni di link tra classi trofiche.

## Requisiti

Il notebook e stato aggiornato per Python 3. Le librerie principali usate sono:

- `networkx`: creazione e analisi di grafi;
- `matplotlib`: istogrammi e visualizzazioni;
- `jupyter` o `ipykernel`: esecuzione del notebook.

Esempio di installazione in un ambiente virtuale:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install networkx matplotlib notebook ipykernel
```

Poi si puo avviare Jupyter dalla cartella del notebook:

```bash
jupyter notebook Chapter_I_FoodWebs_Notebook.ipynb
```

## Struttura logica del notebook

### 1. Matrice di adiacenza non orientata

Il notebook inizia definendo una piccola matrice di adiacenza:

- ogni riga rappresenta un nodo;
- ogni colonna rappresenta un nodo;
- `1` indica presenza di un link;
- `0` indica assenza di link.

Nel caso non orientato la matrice e simmetrica: se il nodo `i` e collegato al nodo `j`, allora anche `j` e collegato a `i`. Questa rappresentazione permette di trasformare un grafo in una struttura numerica semplice.

### 2. Lettura e visita della matrice

Il notebook mostra due modi per scorrere la matrice:

- riga per riga, per vedere il profilo di connessione di ogni nodo;
- elemento per elemento, con cicli annidati, per controllare ogni possibile coppia di nodi.

Questa visita elemento per elemento e il modello base per molti algoritmi su reti: conteggio dei link, calcolo dei gradi, identificazione dei vicini e misure locali.

### 3. Matrice di adiacenza orientata

Il notebook introduce poi una matrice di adiacenza diretta. In una rete orientata, `adjacency_matrix_directed[i][j] = 1` significa che esiste un arco da `i` verso `j`.

Questa distinzione e fondamentale nelle food webs, dove la direzione del link porta informazione ecologica. A seconda della convenzione del dataset, l'arco puo essere letto come flusso da risorsa a consumatore oppure come relazione da consumatore a risorsa. Il notebook usa questa direzione in modo coerente per distinguere archi entranti e uscenti.

### 4. Statistiche di base della rete

Il notebook calcola:

- numero di specie, come numero di nodi;
- numero di predazioni o link, contando gli elementi non nulli della matrice;
- connettanza, come rapporto tra link osservati e link possibili;
- specie basali, intermedie e top.

L'algoritmo scorre tutte le celle della matrice. Durante la scansione:

- la somma delle righe misura gli archi uscenti;
- la somma delle colonne misura gli archi entranti;
- i conteggi entranti e uscenti permettono di classificare ogni specie.

La connettanza e una misura di densita:

```text
connectance = numero_di_link / numero_di_link_possibili
```

Per una rete diretta con `S` specie, il notebook usa `S^2` come numero massimo di posizioni nella matrice.

### 5. Degree, in-degree e out-degree

Il notebook distingue:

- grado, per reti non orientate;
- in-degree, numero di archi entranti;
- out-degree, numero di archi uscenti.

In una matrice non orientata, il grado di un nodo si ottiene sommando la riga corrispondente. In una matrice orientata, la somma della riga e l'out-degree, mentre la somma della colonna e l'in-degree.

Queste quantita sono centrali nell'analisi di reti trofiche perche descrivono il ruolo locale di una specie nella rete.

### 6. Uso di NetworkX

Dopo i calcoli manuali, il notebook passa a NetworkX:

- crea un grafo con `nx.Graph()`;
- aggiunge nodi con `add_node`;
- aggiunge archi con `add_edge`;
- calcola il grado con `G.degree(node)`.

NetworkX permette di lavorare direttamente con oggetti grafo, evitando di implementare manualmente ogni operazione su matrici. Questo rende piu semplice applicare algoritmi standard e passare da esempi piccoli a dataset reali.

### 7. Sequenza dei gradi

Il notebook calcola la sequenza dei gradi, cioe la lista dei gradi di tutti i nodi della rete.

L'algoritmo:

1. visita ogni riga della matrice;
2. somma i valori della riga;
3. salva il risultato in una lista.

La sequenza dei gradi descrive quanto la rete e omogenea o eterogenea. In reti complesse, questa sequenza e spesso il punto di partenza per studiare distribuzioni, hub e differenze tra reti biologiche, sociali o tecnologiche.

### 8. Visualizzazione con Matplotlib

Il notebook abilita `%matplotlib inline` e crea un istogramma di esempio.

L'istogramma divide i dati in intervalli, chiamati bin, e conta quanti valori cadono in ogni intervallo. Nel contesto delle reti, lo stesso approccio puo essere usato per visualizzare:

- distribuzioni dei gradi;
- distribuzioni delle distanze;
- distribuzioni dei coefficienti di clustering;
- altre misure locali o globali.

### 9. Coefficiente di clustering locale

Il notebook calcola manualmente il coefficiente di clustering del nodo 2.

L'algoritmo:

1. identifica i vicini del nodo;
2. controlla tutte le coppie di vicini;
3. conta quanti link esistono tra i vicini;
4. divide per il numero massimo possibile di link tra quei vicini.

La formula usata e:

```text
C_i = link_effettivi_tra_vicini / link_possibili_tra_vicini
```

Per un nodo di grado `k`, il numero massimo di link tra vicini e:

```text
k * (k - 1) / 2
```

Il clustering misura la tendenza a formare triangoli. In una rete ecologica puo indicare chiusura locale o ridondanza nelle relazioni tra specie.

### 10. Lettura della rete Ythan Estuary

Il notebook legge `data/Ythan_Estuary.txt` come edge list. Ogni riga contiene una coppia di nodi che rappresenta un arco diretto.

L'algoritmo:

1. apre il file;
2. legge una riga alla volta;
3. separa i campi con `strip().split()`;
4. ignora eventuali righe non valide;
5. aggiunge un arco diretto al grafo `DG`.

Poi rimuove il nodo `0`, usato come ambiente o nodo artificiale.

### 11. Componente bow-tie

Sulla rete Ythan, il notebook costruisce una rappresentazione bow-tie.

Il procedimento:

1. calcola le componenti fortemente connesse del grafo diretto;
2. seleziona la componente fortemente connessa piu grande;
3. identifica i nodi esterni che puntano al nucleo, cioe la componente IN;
4. identifica i nodi esterni raggiungibili dal nucleo, cioe la componente OUT;
5. costruisce un sottografo con IN, nucleo e OUT;
6. assegna posizioni manuali ai nodi;
7. disegna e salva la figura `data/bowtie.png`.

Una componente fortemente connessa e un insieme di nodi in cui ogni nodo puo raggiungere ogni altro seguendo la direzione degli archi. La struttura bow-tie e utile per reti dirette perche separa:

- cio che puo entrare nel nucleo;
- il nucleo reciprocamente raggiungibile;
- cio che puo essere raggiunto dal nucleo.

### 12. Grafo di esempio e vicini

Il notebook costruisce un grafo non orientato con nodi etichettati da lettere. Questo grafo serve a introdurre il concetto di vicinato e il calcolo delle distanze.

NetworkX viene usato per:

- inserire molti archi in una sola istruzione con `add_edges_from`;
- stampare i vicini del nodo `A`;
- disegnare il grafo.

### 13. Breadth-First Search e distanze

Il notebook implementa una visita in ampiezza, o BFS, partendo dal nodo `A`.

L'algoritmo:

1. inizializza una coda con il nodo radice;
2. assegna distanza zero alla radice;
3. estrae il primo nodo dalla coda;
4. visita i suoi vicini non ancora visitati;
5. assegna a ogni vicino distanza pari alla distanza del nodo corrente piu uno;
6. inserisce il vicino in coda.

La BFS trova cammini minimi in grafi non pesati perche esplora la rete per livelli: prima tutti i nodi a distanza 1, poi quelli a distanza 2, e cosi via.

### 14. Lettura della rete Little Rock Lake

Il notebook legge `data/Little_Rock_Lake.txt` come edge list e costruisce un grafo diretto.

Come per Ythan:

- ogni riga viene interpretata come un arco;
- il grafo viene popolato con `DG.add_edge`;
- ogni arco letto viene stampato.

Questo dataset viene poi usato per l'analisi delle specie trofiche.

### 15. Chiave trofica di un nodo

Il notebook definisce `get_node_key(node)`, una funzione che descrive un nodo tramite:

- lista dei successori, cioe nodi raggiunti dagli archi uscenti;
- separatore `'-'`;
- lista dei predecessori, cioe nodi da cui arrivano archi entranti.

Le liste vengono ordinate prima di essere combinate. Questo rende la chiave stabile e confrontabile.

Il principio alla base e l'equivalenza trofica: due specie possono essere trattate come equivalenti se hanno lo stesso insieme di relazioni in uscita e in entrata.

### 16. Aggregazione in specie trofiche

Il notebook definisce `TrophicNetwork(DG)`, che raggruppa nodi con la stessa chiave trofica.

L'algoritmo:

1. calcola la chiave trofica per ogni nodo;
2. usa un dizionario per associare ogni chiave alla lista dei nodi che la condividono;
3. per ogni gruppo con piu nodi, conserva il primo nodo;
4. rimuove gli altri nodi dal grafo;
5. restituisce la rete aggregata.

Dopo l'aggregazione, il notebook stampa:

- `S`: numero di nodi o specie trofiche;
- `L`: numero di link;
- `L/S`: rapporto tra link e specie.

Questa aggregazione riduce la complessita della rete mantenendo ruoli funzionali distinti.

### 17. Classi basali, intermedie e top

Il notebook definisce `compute_classes(DG)`, che separa i nodi in tre classi:

- specie basali;
- specie intermedie;
- specie top.

La classificazione si basa su in-degree e out-degree:

- se `in_degree(n) == 0`, il nodo viene classificato come basale;
- se `out_degree(n) == 0`, il nodo viene classificato come top;
- altrimenti viene classificato come intermedio.

Poi il notebook stampa la proporzione di ciascuna classe rispetto al numero totale di specie trofiche.

Queste proporzioni descrivono la struttura verticale della rete e permettono di confrontare reti diverse indipendentemente dalla loro dimensione assoluta.

### 18. Link tra classi trofiche

Il notebook definisce `InterclassLinkProportion(DG, C1, C2)`, che misura la proporzione di link diretti da una classe `C1` a una classe `C2`.

L'algoritmo:

1. scorre tutti i nodi della prima classe;
2. scorre tutti i nodi della seconda classe;
3. controlla se esiste un arco da `n1` a `n2`;
4. conta gli archi trovati;
5. divide il conteggio per il numero totale di archi del grafo.

Le proporzioni calcolate sono:

- link da basali a top;
- link da basali a intermedie;
- link da intermedie a intermedie;
- link da intermedie a top.

Queste misure descrivono come le interazioni sono distribuite tra classi funzionali, non solo quanti nodi appartengono a ogni classe.

### 19. Rapporto prede/predatori

Il notebook calcola anche il rapporto:

```text
P/R = (numero_basali + numero_intermedie) / (numero_intermedie + numero_top)
```

Questo indicatore confronta il numero di specie che possono essere interpretate come risorse o prede con il numero di specie che possono essere interpretate come consumatori o predatori, secondo la classificazione usata nel notebook.

## Risultati prodotti

Durante l'esecuzione il notebook produce:

- stampe delle matrici di adiacenza;
- conteggi di specie, link e classi trofiche;
- sequenza dei gradi;
- istogramma di esempio;
- coefficiente di clustering locale;
- visualizzazioni NetworkX;
- file `data/bowtie.png`;
- statistiche sulla rete aggregata Little Rock Lake;
- proporzioni di link tra classi trofiche;
- rapporto `P/R`.

## Note sull'aggiornamento a Python 3

Il notebook e stato aggiornato rispetto allo stile Python 2 originale. Le modifiche principali includono:

- uso di `print(...)`;
- uso di `G.nodes[...]` al posto di `G.node[...]`;
- uso di `k not in trophic` al posto di `has_key`;
- conversione esplicita dei vicini in lista quando serve stamparli;
- parsing dei file con `strip().split()`;
- metadata del notebook aggiornati a kernel Python 3.

## Come leggere il notebook

Il notebook e pensato come percorso didattico. Conviene eseguirlo dall'inizio alla fine, perche alcune celle dipendono da variabili create nelle celle precedenti.

In particolare:

- `row_count` e `column_count` vengono creati nella sezione sulle statistiche di base;
- `degree_node_2` viene usato nel calcolo del clustering;
- `DG` viene riusato per reti diverse in momenti diversi;
- `TrophicDG`, `B`, `I` e `T` sono necessari per le misure finali.

Per evitare risultati incoerenti, usare `Run All` o riavviare il kernel prima di eseguire nuovamente l'intero notebook.

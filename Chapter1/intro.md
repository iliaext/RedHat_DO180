# Appunti: Red Hat OpenShift Container Platform (RHOCP)

## 1. Cos'è OpenShift?
**OpenShift (RHOCP)** è una piattaforma enterprise completa per eseguire e gestire applicazioni containerizzate su cluster di server.
* Si basa su tecnologie open source fondamentali come **Container** e **Kubernetes**.
* Mentre Kubernetes fornisce le basi strutturali ("punti di estensione"), OpenShift vi aggiunge strumenti integrati per offrire una piattaforma completa "chiavi in mano".

---

## 2. Concetti di Base

### A. Contenitori (Containers)
* Un **container** è un processo isolato che esegue un'applicazione indipendentemente dagli altri container e dal sistema host.
* Include l'applicazione e tutte le sue dipendenze (eliminando i conflitti di versione).
* **Funzionalità Kernel Linux usate**:
  * **`cgroups` (control groups)**: Gestione e limitazione delle risorse (CPU, RAM).
  * **`namespaces`**: Isolamento dei processi e delle risorse di rete/file system.
* **Standard OCI**: L'*Open Container Initiative* mantiene gli standard per immagini e runtime, garantendo che i container funzionino su qualsiasi piattaforma compatibile.

### B. Kubernetes (K8s)
Piattaforma che automatizza il deployment, la gestione e la scalabilità dei container su più nodi.
* **Modello Dichiarativo**: L'amministratore definisce lo stato desiderato (tramite file YAML/JSON) e Kubernetes lavora continuamente per allineare lo stato reale a quello definito.
* **Pod**: L'unità minima di elaborazione in Kubernetes. È composto da uno o più container che condividono le stesse risorse di rete e storage sullo stesso nodo.

#### Funzionalità Chiave di Kubernetes:
* **Service Discovery & Load Balancing**: Assegna nomi DNS e bilancia il traffico di rete tra i vari pod.
* **Scalabilità**:
  * *Orizzontale*: Aumenta o diminuisce il numero di pod in base al carico.
  * *Verticale (In-place pod resize)*: Modifica la CPU e la RAM allocate ai pod in esecuzione senza riavviarli.
* **Self-healing**: Monitora la salute delle applicazioni e riavvia o rialloca automaticamente i pod in caso di guasto hardware o software.
* **Automated Rollout & Rollback**: Distribuisce le nuove versioni dell'applicazione in modo graduale con possibilità di ripristino automatico alla versione precedente in
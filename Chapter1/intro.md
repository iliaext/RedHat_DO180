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
* **Automated Rollout & Rollback**: Distribuisce le nuove versioni dell'applicazione in modo graduale con possibilità di ripristino automatico alla versione precedente in caso di errori.
* **Gestione di Secret e Configuration**: Separa le configurazioni e i dati sensibili dal codice del container (nota: i *Secret* di base non sono cifrati).
* **Operators**: Applicazioni che automatizzano compiti complessi di gestione dei carichi di lavoro (es. backup, repliche e aggiornamenti di database).

---

## 3. Architettura del Cluster Kubernetes
Un cluster è formato da server fisici o macchine virtuali (detti **nodi**) divisi in due ruoli principali:

1. **Control Plane Nodes**: Gestiscono il coordinamento globale del cluster, lo scheduling dei pod e lo stato delle configurazioni.
2. **Compute Plane Nodes (Worker)**: Eseguono effettivamente i carichi di lavoro (i pod dell'utente).

> **Hosted Control Planes**: Architettura avanzata per ambienti multi-tenant o *sovereign cloud* in cui il Control Plane viene eseguito come pod isolati all'interno di un cluster dedicato.

---

## 4. Perché OpenShift rispetto a Kubernetes "Puro"?
OpenShift estende Kubernetes integrando nativamente diversi strumenti aziendali:
* **Workflow Sviluppatore Integrato**: Registry di immagini container integrato, pipeline CI/CD e strumento **S2I (Source-to-Image)** per compilare il codice sorgente direttamente in immagini container.
* **Osservabilità**: Sistema integrato di monitoraggio, metrica e gestione dei log per il cluster e le applicazioni.
* **Sistema Operativo Immutabile**: Utilizza **RHEL CoreOS (Red Hat Enterprise Linux CoreOS)** come OS sottostante per i nodi, ottimizzato per i container e gestito con lo stesso modello dichiarativo di Kubernetes.
* **Interfaccia Grafica e Strumenti**: Web console grafica avanzata per amministratori e sviluppatori.

---

## 5. Edizioni di Red Hat OpenShift

### Ambienti di Test/Sviluppo Locale:
* **Red Hat OpenShift Local**: Configurazione per eseguire un cluster in locale sul proprio PC.
* **Developer Sandbox**: Accesso gratuito per 30 giorni a un ambiente OpenShift condiviso in cloud.

### Edizioni per la Produzione:

#### Cloud Services (Managed)
Servizi gestiti sui principali cloud provider (AWS, Azure, GCP, IBM). Red Hat e il provider gestiscono l'infrastruttura e gli aggiornamenti del cluster.

#### Self-managed (Gestite in autonomia)
Installabili su infrastruttura fisica, virtuale o cloud privato:
* **OpenShift Virtualization Engine**: Offerta focalizzata esclusivamente sulla gestione delle Macchine Virtuali (VM).
* **OpenShift Kubernetes Engine**: Piattaforma Kubernetes di base con hardening di sicurezza Red Hat su RHEL CoreOS.
* **OpenShift Container Platform (RHOCP)**: Include la console developer, Serverless (Knative), Service Mesh (Istio), Pipelines (Tekton) e GitOps (Argo CD).
* **OpenShift Platform Plus**: La suite più completa. Aggiunge Red Hat OpenShift Data Foundation (storage), Advanced Cluster Management, Advanced Cluster Security e il registry privato Quay.

---

## 6. Ciclo di Vita e Supporto (Lifecycle)
* Rilasci minori regolari.
* Le versioni minori **pari** (es. 4.18, 4.20, 4.22) sono designate come **EUS (Extended Update Support)**.
* Le versioni EUS garantiscono **24 mesi di supporto** base, con un add-on opzionale di 12 mesi per un totale di **36 mesi di supporto** per ambienti critici.
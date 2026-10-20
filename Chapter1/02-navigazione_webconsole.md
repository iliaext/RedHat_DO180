# Navigazione e Gestione della Web Console OpenShift (RHOCP)

## 1. Panoramica della Web Console OpenShift
A differenza di Kubernetes (che di default fornisce solo una dashboard minimale con accesso proxy/token), **OpenShift include una Web Console nativa e completa** basata su API Kubernetes ed estensioni OpenShift.

* **Interfaccia Grafica (GUI)**: Permette la gestione visiva di complessi compiti amministrativi.
* **Estensibilità**: Gli *Operators* possono estendere la console aggiungendo menu, viste e moduli personalizzati.
* **Corrispondenza con CLI**: Tutte le operazioni eseguibili da Web Console si possono fare anche tramite linea di comando (`oc`).

---

## 2. Accesso alla Web Console

### Trovare l'URL della Console via CLI
Se si conosce l'endpoint API del cluster, è possibile risalire all'URL della Web Console da terminale:
1. Login al cluster
```bash

oc login -u developer -p developer \
https://api.lab.example.com:6443
```
2. Ottenere direttamente l'URL della Console
```bash
oc whoami --show-console
```

---
## 3. Struttura e Layout della Console (OpenShift 4.19+)

### Prospettiva Unificata (Core Platform Perspective)
A partire da OpenShift 4.19, le vecchie prospettive separate (*Administrator* e *Developer*) sono unite in un'unica **Core platform perspective**.
*(Nota: la prospettiva Virtualization si attiva se è installato il relativo Operator).*

#### Ripristinare la vista Developer separata (Opzionale)
Dalla console: **Administration -> Cluster Settings** -> scheda **Configuration** -> **Console** -> **Customize** -> impostare *Developer perspective* su **Enabled**.

### Menu Principali della Sidebar:
* **Home -> Overview**: Stato generale del cluster, avvisi e documentazione.
* **Home -> Projects**: Elenco di tutti i progetti (namespace) accessibili all'utente.
* **Ecosystem -> Software Catalog**: Catalogo unificato (ex OperatorHub e Dev Catalog) per installare software, Helm charts ed Operators.
* **Ecosystem -> Installed Operators**: Gestione degli Operator già attivi sul cluster.
* **`+` Quick Create** (in alto a destra): Menu rapido per distribuire app da immagini container o repo Git.

---

## 4. Concetti Chiave e Corrispettivi CLI

| Concetto | Descrizione | Comando CLI di riferimento |
| :--- | :--- | :--- |
| **Pod** | Unità minima di lavoro (uno o più container correlati). | `oc get pods` |
| **Deployment** | Unità operativa che gestisce la scalabilità e le istanze dei Pod. | `oc get deployments` |
| **Project** | Namespace Kubernetes con annotazioni per l'isolamento multi-tenant. | `oc get projects` |
| **Route** | Configurazione di rete per esporre un servizio all'esterno del cluster. | `oc get routes` |
| **Operator** | Applicazione pacchettizzata che automatizza la gestione di carichi complessi. | `oc get csv` *(ClusterServiceVersions)* |

---

## 5. Nodi, Macchine e MachineConfig

* **Node**: Il server reale o virtuale su cui girano i pod.
* **Machine**: La risorsa che descrive ed effettua il provisioning del nodo (molto usata in ambienti cloud).
* **MachineConfig**: Risorsa che definisce lo stato del SO (**RHEL CoreOS**), servizi, file di configurazione per `kubelet` e `cri-o`.
* **Machine Config Operator (MCO)**: L'operator che applica i `MachineConfig`, aggiorna l'OS ed effettua il ripristino automatico dello stato del nodo (*in base alle zone, in ordine alfabetico*).

### Ispezione e Debug dei Nodi

**Dalla Console**: **Compute -> Nodes** -> Cliccare sul nome del nodo per accedere a metriche, eventi, YAML, log e al **Terminale del nodo** (tramite *debug pod*).

**Via CLI**:
```bash
# Elenco di tutti i nodi
oc get nodes

# Avviare un Pod di Debug sul nodo per accedere alla sua shell
oc debug node/<nome-nodo>

# Dentro il pod di debug, eseguire per abilitare i binari dell'host:
chroot /host
```
## 6. Accesso ai Log e Troubleshooting dei Pod

**Dalla Console**: **Workloads -> Pods** -> Cliccare sul Pod -> Schede **Logs** o **Terminal**.

**Via CLI**:
```bash
oc get pods
oc logs <nome-pod>
oc rsh <nome-pod>
```
---

## 7. Monitoraggio, Alerting ed Eventi

### Metriche e Allarmi (Prometheus)
Le applicazioni espongono metriche HTTP sull'endpoint `/metrics`.
* **`ServiceMonitor`**: Definisce come raccogliere le metriche da un Servizio per creare allarmi.
* **`PodMonitor`**: Raccoglie metriche direttamente dai Pod.

**Percorsi Console**:
* **Observe -> Metrics**: Esecuzione di query (PromQL) e visualizzazione grafici.
* **Observe -> Alerting**: Visualizzazione degli avvisi attivi e filtro per gravità (*severity*).
* **Cluster Observability Operator (COO)**: Componente opzionale per creare stack di monitoraggio altamente personalizzati per singoli namespace.

### Eventi vs Log
* **Log**: Dettagliati, approfonditi e granulari per il debug dei singoli processi.
* **Eventi**: Vista ad alto livello sui cambiamenti significativi del cluster (es. riavvio di un pod, fallimento dello scheduling).
* **Percorso Console**: **Home -> Events**.

**Via CLI**:
```bash
oc get events --watch
```
---

## 8. Esploratore delle API (API Explorer)
Accessibile da **Home -> API Explorer**, permette di consultare il catalogo completo dei tipi di risorse Kubernetes/OpenShift disponibili nel cluster (schema, descrizione, campi del file YAML).

**Via CLI**:
```bash
oc api-resources
oc explain deployment
```
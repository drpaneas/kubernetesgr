+++
title = "Στήνοντας το minikube"
description = "Πειραματιστείτε και δοκιμάστε το deployment ενός Kubernetes single-node cluster με το minikube"
date = "2017-09-30"
categories = [ "beginners", "how to" ]
tags = [
    "kubernetes",
    "minikube",
    "virtualbox",
    "hyperv",
    "kvm",
    "docker"
]
+++

- [Εισαγωγή](#Εισαγωγή)
  * [Λίγα λόγια για το Minikube](#Λίγα-λόγια-για-το-Minikube)
  * [Πώς λειτουργεί πίσω από την μηχανή](#Πώς-λειτουργεί-πίσω-από-την-μηχανή)
- [Εγκατάσταση του Minikube](#Εγκατάσταση-του-Minikube)
  * [Linux](#linux)
  * [Mac OSX](#mac-osx)
  * [Windows](#windows)
- [Εγκαταστήστε τον Kubernetes client](#Εγκαταστήστε-τον-Kubernetes-client)
  * [Linux](#linux-1)
  * [Mac OSX](#mac-osx-1)
- [Ξεκινήστε το Kubernetes cluster με το Minikube](#Ξεκινήστε-το-Kubernetes-cluster-με-το-Minikube)
  * [Χρησιμοποιώντας το VirtualBox](#Χρησιμοποιώντας-το-VirtualBox)
  * [Με docker έναντι του VirtualBox](#Με-docker-έναντι-του-VirtualBox)
  * [Με KVM έναντι του VirtualBox](#Με-KVM-έναντι-του-VirtualBox)
- [Ελέγξετε αν το cluster σηκώθηκε και μπορεί να επικοινωνήσει με το kubectl](#[Ελέγξετε-αν-το-cluster-σηκώθηκε-και-μπορεί-να-επικοινωνήσει-με-το-kubectl)
- [Αναλυτικές οδηγίες για χρήστες Windows και όχι μόνο](#Αναλυτικές-οδηγίες-για-χρήστες-Windows-και-όχι-μόνο)
  * [Εγκατάσταση του Minikube](#Εγκατάσταση-του-Minikube-1)
  * [Minikube και VirtualBox](#Minikube-και-VirtualBox)
  * [Αλληλεπίδραση με το cluster](#Αλληλεπίδραση-με-το-cluster)
  * [Φορτώνοντας το Dashboard](#Φορτώνοντας-το-Dashboard)

# Εισαγωγή

Το στήσιμο ενός **Kubernetes cluster** δεν είναι εύκολη υπόθεση, ειδικά 
όταν στο σενάριο αυτής αναφερόμαστε σε πράγματα όπως το HA *(High
Availability)*. Ξεκινώντας **από το μηδέν** και δουλεύοντας 
αποκλειστικά στο τοπικό μας δίκτυο, θα δείξουμε αναλυτικά και **βήμα
προς βήμα** πώς μπορούμε να δημιουργήσουμε ένα - μικρό μεν, 
λειτουργικό δε - kubernetes cluster, χρησιμοποιώντας το **minikube**.

Το αληθινό μας κίνητρο δεν είναι να φτιάξουμε κάτι που θα μπορεί να
συγκριθεί με ένα πραγματικό cluster ή να σταθεί αντάξιο σε επίπεδο
παραγωγής. Αυτό που θέλουμε περισσότερο είναι να συγκροτήσουμε γρήγορα
και εύκολα ένα υποτυπώδες **k8s** (*συντομογραφία του Kubernetes*)
cluster, έτσι ώστε να πειραματιστούμε και να δοκιμάσουμε οτιδήποτε
μας κινεί το ενδιαφέρον γύρω του.

## Λίγα λόγια για το Minikube

Αν και υπάρχουν πάρα πολλοί τρόποι να εγκαταστήσει κανείς ένα k8s
cluster, εμείς επιλέξαμε το [minikube](https://github.com/kubernetes/minikube)
καθώς θεωρείται ίσως η πιο εύκολη προσέγγιση για όσους αναζητούν
μια βασική εμπειρία γύρω από την τεχνολογία του Kubernetes. To
minikube δεν είναι άλλο παρά ένα εργαλείο (toolbox) το οποίο
δουλεύουμε αποκλειστικά από την **γραμμή εντολών** με σκοπό την
αυτόματη εγκατάσταση ενός τοπικού k8s cluster. Όπως είπαμε νωρίτερα,
δεν πρόκειται για ένα πραγματικό cluster όπως αυτά συναντήσει κανείς
στο Amazon AWS ή το Microsoft Azure, αλλά για ένα μικρό single-node
cluster εκπαιδευτικού χαρακτήρα το οποίο είναι αρκετό για κάποιον
που θέλει να δοκιμάσει το Kubernetes και να τεστάρει (τοπικά) τις
δικές του εφαρμογές.

Αν και δεν θα μπορέσουμε να δούμε τις δυνατότητες του Kubernetes
που συσχετίζονται με την διαχείριση εφαρμογών σε ένα cluster με
περισσότερα nodes, θα είναι σίγουρο αρκετό για να καλύψει τις
βασικές λειτουργίες που πρέπει να γνωρίζει κανείς.

Ένας σημαντικός λόγος που μας έκανε να επιλέξουμε το minikube
για την διαχείριση και την παροχή υπηρεσιών του πρώτου μας
kubernetes cluster, είναι γιατί υποστηρίζει (με την ίδια
ευκολία) όλα τα **βασικά λειτουργικά συστήματα** της αγοράς:
*Microsoft Windows, GNU/Linux και macOS*. Συνεπώς, θέλοντας
να κάνουμε την τεχνολογία του Kubernetes περισσότερη προσιτή
στο ελληνικό κοινό, θεωρήσαμε ότι θα ήταν καλό να σας
παρουσιάσουμε ένα εργαλείο που λειτουργεί με τον ίδιο τρόπο
για όλους, ανεξαρτήτως λειτουργικού συστήματος.

Τέλος, μας χαροποίησε ιδιαίτερα το γεγονός ότι το τοπικό μας
cluster, θα τρέχει **πάντα** την **πιο πρόσφατη έκδοση** του K8s
και κατά συνέπεια θα είναι πάντα συμβατό με τη τεκμηρίωση αυτής
(documentation). Έτσι όχι μόνο θα μπορείτε να πειραματιστείτε
γύρω από το Kubernetes, αλλά θα είστε ενήμεροι ακόμα και για
τις τελευταίες εξελίξεις γύρω από αυτό.

## Πώς λειτουργεί πίσω από την μηχανή

Όπως είπαμε στην αρχή, υπάρχουν πάρα πολλοί τρόποι να εγκαταστήσει
κανείς ένα kubernetes cluster, οι περισσότεροι εκ των οποίων
βρίσκονται συγκεντρωμένοι στο επίσημο [website](http://kubernetes.io./).
Για ευνόητους λόγους, δεν σκοπεύουμε να τους αναφέρουμε όλους,
καθώς η λίστα αυξάνεται καθημερινά. Ενδεικτικά, το Kubernetes
μπορεί εγκατασταθεί σε γνωστούς παρόχους υπηρεσιών Cloud
([Google Compute Engine](https://cloud.google.com/compute/),
[Amazon EC2](https://aws.amazon.com/ec2/),
[Microsoft Azure](https://azure.microsoft.com/), κ.α.), κάτι
για το οποίο θα μιλήσουμε σε κάποιο άλλο άρθρο. Για την ώρα θα
θεωρήσουμε ότι δεν έχουμε ενεργή συνδρομή σε κάποιον public cloud
provider και μας είναι αρκετό να πειραματιστούμε τοπικά.
Το Minikube μας παρέχει την δυνατότητα να στήσουμε ένα **single-node**
cluster, ενώ για περισσότερους nodes θα αναφερθούμε κάποια
άλλη στιγμή εκτενέστερα στο εργαλείο [kubeadm](https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/).
Σε αυτό το σημείο, αξίζει να σημειώσουμε πως επιμέρους
εταιρίες στον χώρο του Linux έχουν αρχίσει να λανσάρουν και
αυτές με την σειρά τους τις δικές τους λύσεις, όπως για
παράδειγμα μέσω του **Vagrand** ([διαβάστε εδώ](https://coreos.com/kubernetes/docs/latest/kubernetes-on-vagrant-single.html))
για το **CoreOS** της RedHat, ή μέσω του **Saltstack** ([διαβάστε εδώ](https://github.com/kubic-project/salt))
για τo **CaaSP** της SUSE, ή μέσω του **conjure-up** ([διαβάστε εδώ](https://conjure-up.io/))
για το **Ubuntu** της Canonical, κ.α. Και σαν να μην
έφταναν όλα αυτά, με την άνοδο του Kubernetes στο No1
project του Github, άρχισαν να δημιουργούνται διάφορα
development platforms που να κάνουν και αυτά με την
σειρά τους την ίδια δουλειά (βλ. [fabric8](https://fabric8.io/),
[kubo](https://pivotal.io/partners/kubo) για το cloudfoundry,
[kube-spawn](https://kinvolk.io/blog/2017/08/introducing-kube-spawn-a-tool-to-create-local-multi-node-kubernetes-clusters/),
κ.α. ). Συνεπώς, οι προγραμματιστές και ίδια η κοινότητα του
Kubernetes, δεν μπορούσαν να μείνουν άπραγοι, γεγονός που
συνετέλεσε στην δημιουργία του Minikube.

Οι developers του k8s, προκειμένου να αποφύγουν την χρήση 3rd
party λογισμικού και κατ’ επέκταση τις εξαρτήσεις αυτού που
δεν είναι στο χέρι τους να τις ελέγξουν, σκέφτηκαν να φτιάξουν
ένα toolbox το οποίο θα περιέχει μέσα του όλες τις απαιτούμενες
εξαρτήσεις (dependencies) που χρειάζονται για να στήσει κανείς
ένα πλήρες τοπικό kubernetes cluster. Το εργαλείο αυτό
ονομάστηκε αρχικά [Localkube](https://github.com/redspread/localkube),
και στην συνέχεια το μετονόμασαν σε “Minikube” έτσι ώστε να
αποφευχθεί η σύγχυση με ένα άλλο εργαλείο που χρησιμοποιούσε
το ίδιο όνομα και συσχετιζόταν με το [Spread](https://redspread.readme.io/)
(developement tool). Για την απλούστευση της εμπειρίας χρήσης,
το Minikube διευκολύνει την αλληλεπίδραση του διαχειριστή με
το τοπικό kubernetes cluster χρησιμοποιώντας μία εξολοκλήρου
δική του σύνταξη. Οι εντολές αυτές είναι αρκετά απλές και ο
σκοπός τους είναι σχεδόν αυτονόητος. Για παράδειγμα:

* η εντολή **start** δημιουργεί και ξεκινάει ένα τοπικό
cluster μαζί με ότι χρειάζεται (πχ δικτύωση)

* η εντολή **stop**, σταματάει το τρέχων τοπικό cluster
και διατηρεί τυχόν αλλαγές που πιθανώς να του έχουν γίνει

* η εντολή **delete** διαγράφει οριστικά το τοπικό cluster

* η εντολή **upgrade** αναβαθμίζει τα εσωτερικά χαρακτηριστικά
του cluster (πχ νεώτερη έκδοση του API), χωρίς όπως να εγγυάται
ότι θα δουλέψει σίγουρα.

Για την χρήση και την διαχείριση των επιμέρους εξαρτημάτων του
Kubernetes, χρησιμοποιείται το “localkube”, το οποίο δεν είναι
τίποτα παραπάνω από ένα αυτόνομο binary, γραμμένο στην γλώσσα
προγραμματισμού **Go**. Κάθε έκδοση του Kubernetes περιέχει
ένα τέτοιο binary, το οποίο έχει τεσταριστεί εξονυχιστικά. Για
την υποστήριξη των Windows και το OS X, το minikube χρησιμοποιεί
την βιβλιοθήκη του Docker [libmachine](https://github.com/docker/machine/tree/master/libmachine).
Αυτή του δίνει την δυνατότητα να φτιάχνει και να καταστρέφει
εικονικές μηχανές χρησιμοποιώντας το **VirtualBox** ως hypervisor.
Να τονίσουμε επίσης ότι το εικονικό αρχείο που κατεβάζει το
Minikube και εισάγει στο virtualbox,
[τεστάρεται](https://travis-ci.org/kubernetes/minikube/branches)
επίσης χρησιμοποιώντας το [TravisCI](https://travis-ci.org/)
του GitHub. Στην περίπτωση του Linux, από την στιγμή που το
cluster μπορεί να τρέξει τοπικά στο ίδιο το σύστημα, θέλουμε να
αποφύγουμε το στήσιμο μιας αχρείαστης εικονικής μηχανής. Αυτό
ισχύει μόνο εφόσον το “localkube” μπορεί να τρέξει μέσα στο Docker.
Σε περίπτωση που στο μέλλον υπάρξει πρόβλημα ασυμβατότητας αυτών
των δύο, τότε, μπορεί να γίνει είτε χρήση KVM προκειμένου να
φορτωθεί εικονική μηχανή είτε να καταφύγουμε στην χρήση του
[rkt](https://coreos.com/rkt/docs/latest/) αν θέλουμε να
παραμείνουμε σε επίπεδο container.

Εν ολίγοις, δείτε τι συμβαίνει πίσω από τη μηχανή αναλόγως το
λειτουργικό σύστημα:

**OS X** ή **Windows**

```
minikube -> libmachine -> virtualbox ή hyper V -> linux VM -> localkube
```


**Linux**

```
minikube -> docker -> localkube
ή
minikube -> libmachine -> virtualbox ή KVM -> linux VM -> localkube
```


# Εγκατάσταση του Minikube

Το Minikube είναι ένα απλό εκτελέσιμο (binary) το οποίο απλά πρέπει
να το κατεβάσουμε και να το συμπεριλάβουμε στην μεταβλητή `PATH`.
Αυτή είναι μία μεταβλητή στην οποία υπάρχουν όλα τα πιθανά directories
που κοιτάει το `bash shell` όταν του δώσουμε μία εντολή. Για να την
εκτελέσει, πρέπει πρώτα να την βρει, και για να την βρει πρέπει να
ξέρει *“πού”* να ψάξει. Αυτή είναι η δουλειά της `PATH`, να του δείξει
*"που”* να ψάξει (σε ποια directories). Αν δεν γνωρίζετε για αυτήν,
σας συμβουλεύουμε να δείτε [αυτό](https://www.youtube.com/watch?v=J4_K8I8zdIo)
το επεξηγηματικό βίντεο. Οι οδηγίες για την εγκατάσταση βρίσκονται
στο [ίδιο](https://github.com/kubernetes/minikube) το GitHub repository.

## Linux

Για την ακρίβεια, η όλη διαδικασία στο Linux και το OSX είναι μία
εντολή (ναι, καλά διαβάσατε).

Στο `Linux` εκτελούμε την ακόλουθη εντολή:

```bash
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```

Όπως βλέπετε, πρόκειται για 3 εντολές, συμπτυγμένες σε μία γραμμή:

* Κατεβάζουμε το binary και το αποθηκεύουμε ως minikube
* Του δίνουμε δικαιώματα εκτέλεσης
* Το μεταφέρουμε στο κλασσικό directory (που βρίσκεται ήδη
στο PATH μας) μαζί με τα υπόλοιπα binaries του normal user

## Mac OSX

Στο `OSX` μπορούμε να κάνουμε την ακριβώς ίδια διαδικασία,
με την μόνη διαφορά ότι πρέπει να αλλάξουμε την λέξη `‘linux’`
με την λέξη `‘darwin’`.

```bash
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```

ή όσοι είστε οπαδοί του `‘brew’`, ο εναλλακτικός τρόπος
είναι αυτός : `brew cask install minikube`

Για περισσότερες πληροφορίες γύρω από την εγκατάσταση
στο περιβάλλον του `Mac OSX`, διαβάστε
[αυτές](https://gist.github.com/kevin-smets/b91a34cea662d0c523968472a81788f7)
τις οδηγίες.

## Windows

Στα `Windows`, ο χρήστης πρέπει να κάνει όλα τα παραπάνω
βήματα, χρησιμοποιώντας το [εκτελέσιμο
αρχείο](https://storage.googleapis.com/minikube/releases/latest/minikube-windows-amd64.exe)
που παρέχει το GitHub project του Minikube. Ωστόσο, έχουμε
την αίσθηση ότι περισσότεροι χρήστες Windows θα αισθανθούν
λίγο άβολα με την γραμμή εντολών, γι αυτό τον λόγο θα βρείτε
αναλυτικά βήματα στο τέλος του άρθρου.


# Εγκαταστήστε τον Kubernetes client

Προκείμενου να αλληλεπιδράσουμε με το Kubernetes, χρειαζόμαστε
επίσης και τον `kubectl`, o οποίος δεν είναι τίποτα περισσότερο
από ένας **client** που μιλάει με το cluster μας μέσω της
γραμμής εντολών. Ξανά, το μόνο που χρειαζόμαστε είναι να
κατεβάσουμε το `binary` και να το βάλουμε στο `PATH` μας.

## Linux

```bash
curl -Lo kubectl https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && chmod +x kubectl
```

## Mac OSX

Για `OSX`: Ισχύουν πάλι τα ίδια, απλά αντικαταστείτε την λέξη
`‘linux’` με `‘darwin’` στο `URL`.

# Ξεκινήστε το Kubernetes cluster με το Minikube

## Χρησιμοποιώντας το VirtualBox

Από προεπιλογή, το minikube χρησιμοποιεί τον `driver` του
`VirtualBox`, το οποίο πρέπει να έχετε εγκατεστημένο και
ρυθμισμένο στο σύστημά σας. Μόλις τελειώσετε με την
εγκατάσταση του Minikube, μπορείτε να ξεκινήσετε αμέσως
το σετάρισμα του τοπικού Kubernetes cluster δίνοντας την
ακόλουθη εντολή:

```bash
$ minikube start
```

H εκκίνηση του cluster παίρνει μερικά λεπτά αναλόγως την
ταχύτητα του υπολογιστή σας, συνεπώς μην βιαστείτε και
διακόψετε την εντολή πριν την ολοκλήρωσή της.

Φυσικά, για όσους έχουν Linux σύστημα σαν εμάς και δεν τους
αρέσει η ιδέα να εγκαταστήσουν το virtualbox στον υπολογιστή
τους, υπάρχουν άλλες 2 επιλογές:

## Με docker έναντι του VirtualBox

Με αυτή την επιλογή, δεν χρειάζεται να χρησιμοποιήσετε καμία
εικονική μηχανή, αλλά να φορτώσετε ολόκληρο το k8s cluster
αποκλειστικά μέσα σε `docker` containers.

Κάντε  export τις παρακάτω μεταβλητές:

```bash
export MINIKUBE_WANTUPDATENOTIFICATION=false
export MINIKUBE_WANTREPORTERRORPROMPT=false
export MINIKUBE_HOME=$HOME
export CHANGE_MINIKUBE_NONE_USER=true
```

Φτιάξτε έναν κρυφό φάκελο `.kube` μέσα στο home directory του
χρήστη σας, καθώς επίσης και ένα άδειο `config` αρχείο. Το
`config` θα περιέχει αργότερα της πληροφορίες που χρειάζεται
το `kubectl` (client) για να επικοινωνήσει με το k8s master API.

```bash
mkdir $HOME/.kube || true
touch $HOME/.kube/config
```

Τέλος, κάνουμε export μια ακόμα μεταβλητή:

```bash
export KUBECONFIG=$HOME/.kube/config
```

Και είμαστε πλέον έτοιμοι να ξεκινήσουμε το k8s cluster μας:

```bash
sudo -E ./minikube start --vm-driver=none
```

Output:

```bash
panos@panos:~> sudo -E ./minikube start --vm-driver=none
========================================
kubectl could not be found on your path. kubectl is a requirement for using minikube
To install kubectl, please run the following:

curl -Lo kubectl https://storage.googleapis.com/kubernetes-release/release/v1.7.0/bin/linux/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/

To disable this message, run the following:

minikube config set WantKubectlDownloadMsg false
========================================
Starting local Kubernetes v1.7.0 cluster...
Starting VM...
Getting VM IP address...
Moving files into cluster...
Setting up certs...
Starting cluster components...
Connecting to cluster...
Setting up kubeconfig...
Kubectl is now configured to use the cluster.
===================
WARNING: IT IS RECOMMENDED NOT TO RUN THE NONE DRIVER ON PERSONAL WORKSTATIONS
    The 'none' driver will run an insecure kubernetes apiserver as root that may leave the host vulnerable to CSRF attacks
```

Όπως μπορείτε να δείτε, το Kubernetes cluster μας σηκώθηκε
χρησιμοποιώντας docker container, και όχι εικονικές μηχανές
(virtual machines):

```bash
panos@panos:~> sudo docker ps
CONTAINER ID        IMAGE                                         COMMAND                 CREATED             STATUS              PORTS               NAMES
3fea0ee0b288        gcr.io/google_containers/pause-amd64:3.0      "/pause"                5 seconds ago       Up 4 seconds                            k8s_POD_kube-dns-910330662-q1gql_kube-system_a9f3b112-88c8-11e7-a3cc-28f10e38701b_0
4e70d9b7c789        gcr.io/google_containers/pause-amd64:3.0      "/pause"                5 seconds ago       Up 4 seconds                            k8s_POD_kubernetes-dashboard-rnc1k_kube-system_a9de60c2-88c8-11e7-a3cc-28f10e38701b_0
4dbd81979cf7        gcr.io/google-containers/kube-addon-manager   "/opt/kube-addons.sh"   7 seconds ago       Up 6 seconds                            k8s_kube-addon-manager_kube-addon-manager-panos.suse.de_kube-system_c654b2f084cf26941c334a2c3d6db53d_0
d854c94113ae        gcr.io/google_containers/pause-amd64:3.0      "/pause"                17 seconds ago      Up 16 seconds                           k8s_POD_kube-addon-manager-panos.suse.de_kube-system_c654b2f084cf26941c334a2c3d6db53d_0
```

## Με KVM έναντι του VirtualBox

Τότε χρειάζεστε ένα ακόμα binary (`docker-machine-driver-kvm`) το οποίο
επιτρέπει στο docker να μιλήσει με το `KVM`. Στην περίπτωση του `openSUSE 42.3`
το πακέτο που χρειαζόμαστε είναι το `docker-machine-kvm`:

```bash
ultron:~ # cnf docker-machine-driver-kvm
                                         
The program 'docker-machine-driver-kvm' can be found in following packages:
  * docker-machine-kvm [ path: /usr/bin/docker-machine-driver-kvm, repository: zypp (Virtualization:containers) ]
  * docker-machine-kvm [ path: /usr/bin/docker-machine-driver-kvm, repository: zypp (Virtualization_containers) ]

Try installing with:
    zypper install docker-machine-kvm
```

Εφόσουν εγκαταστήσουμε το εν λόγο πακέτο, συνέχεια ξεκινάμε
το minikube χρησιμοποιώντας το KVM ως driver:

```bash
minikube start --vm-driver=kvm
```

ή αν θέλουμε περισσότερο verbosity: `minikube start --vm-driver=kvm -v 5`

Output:

```bash
Starting local Kubernetes v1.7.0 cluster...
Starting VM...
Running pre-create checks...
Creating machine...
(minikube) Downloading /root/.minikube/cache/boot2docker.iso from file:///root/.minikube/cache/iso/minikube-v0.23.0.iso...
(minikube) Creating SSH key...
(minikube) Failed to decode dnsmasq lease status: unexpected end of JSON input
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with buildroot...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
Getting VM IP address...
Moving files into cluster...
Setting up certs...
Starting cluster components...
Connecting to cluster...
Setting up kubeconfig...
Kubectl is now configured to use the cluster.
```

Για να σταματήσουμε το cluster:

```bash
skyrim:~ # minikube stop
Stopping local Kubernetes cluster...
Machinestopped.
```

Για να διαγράψουμε ολοκληρωτικά το cluster:

```bash
skyrim:~ # minikube delete
Deleting local Kubernetes cluster...
Machine deleted.
```

# Ελέγξετε αν το cluster σηκώθηκε και μπορεί να επικοινωνήσει με το kubectl

Για να επιβεβαιώσουμε ότι το cluster μας λειτουργεί
κανονικά, μπορούμε να χρησιμοποιήσουμε την εντολή
`kubectl cluster-info`. Το παραπάνω output συνήθως
επιβεβαιώνει ότι το cluster μας είναι online. Δείχνει
επίσης τις διευθύνσεις των επιμέρους εξαρτημάτων του
Kubernetes, όπως τoν API Server και της web console.

Στην συνέχεια του άρθρου θα χρησιμοποιήσουμε αποκλειστικά
Windows, έτσι ώστε να κάνουμε τους φίλους αναγνώστες που
χρησιμοποιούν το συγκεκριμένο λειτουργικό, να μην παρατήσουν
την μέχρι τώρα προσπάθειά τους. Για όλους εσάς τους υπόλοιπους
θα σας προτείνουμε να το διαβάσετε, καθώς τα ίδια πράγματα
ισχύουν τόσο για Linux όσο και για Mac OSX. Πάμε λοιπόν
να δούμε το στήσιμο του Minikube από την μεριά των Windows 10.

# Αναλυτικές οδηγίες για χρήστες Windows και όχι μόνο

Προς το παρόν, δουλεύουμε σε ένα PC με `Windows 10`,
στο οποίο προϋπάρχει ήδη η απαραίτητη υποδομή 
λογισμικού για την δημιουργία και τη διαχείριση
εικονικών μηχανών `VirtualBox`.

## Εγκατάσταση του Minikube

Κατεβάζουμε το εκτελέσιμο αρχείο [minikube-windows-amd64.exe](https://storage.googleapis.com/minikube/releases/latest/minikube-windows-amd64.exe)
και στην συνέχεια το μετονομάζουμε το σε `minikube.exe`.
Για τους λόγους της παρουσίασης φτιάξαμε έναν κατάλογο
`C:/minikube` και στην συνέχεια αντιγράψαμε το εκτελέσιμο
ώστε να νιώθει ασφαλές και χαρούμενο στην νέα του τοποθεσία.
Το συγκεκριμένο εκτελέσιμο, είναι προσβάσιμο μόνο από την
γραμμή εντολών, για αυτό θεωρήσαμε συνετό να προσθέσουμε
τον κατάλογο `C:/minikube` στο **τρέχων** `PATH` το συστήματός
μας. Πριν το κάνουμε όμως αυτό, πρέπει να ανοίξουμε πρώτα
την γραμμή εντολών των Windows. Υπάρχουν πάρα πολλοί τρόποι
για να το κάνετε αυτό, ωστόσο εμείς λόγω συνήθειας
προτείνουμε τον ακόλουθο:

Πατάμε ταυτόχρονα τον συνδυασμό πλήκτων: `WinKey` + `R`, και
στην συνέχεια πληκτρολογούμε την φράση: `cmd.exe`. Τέλος,
κάνουμε κλικ στο κουμπί `OK`.

![Windows Run](/cmd_exe.JPG)

Άνοιξε το παράθυρο της γραμμής εντολών, γεγονός που
σημαίνει ότι τα καταφέραμε.

![CMD Windows](/cmd.JPG)

Δεν ξεχνάμε όμως να δώσουμε και την ακόλουθη εντολή,
η οποία θα προσθέσει τον κατάλογο `C:\minikube` στο `PATH`.
O λόγος που το κάνουμε αυτό είναι γιατί θέλουμε να αποφύγουμε
να γράφουμε διαρκώς το πλήρες directory που βρίσκεται το
εκτελέσιμο του minikube.

```bash
C:\Users\drpan>PATH %PATH%;C:\minikube
```

**Σημείωση:** Η παραπάνω εντολή δεν είναι μόνιμη, αλλά θα
προσθέσει το συγκεκριμένο κατάλογο μόνο κατά το τρέχων
στιγμιότυπο της γραμμής εντολών. Αν κλείσατε ή ανοίξατε
κάποιο άλλο `cmd`, τότε θα πρέπει να την ξαναδώσετε.

## Minikube και VirtualBox

Το πρόγραμμα virtualization που θα χρησιμοποιήσουμε είναι
το [VirtualBox](https://www.virtualbox.org/).
Σε περίπτωση που δεν έχετε ασχοληθεί ξανά
με αυτό, σας προτείνουμε να ρίξετε μια ματιά [σ’ αυτό](https://deltahacker.gr/vbox-based-virtuallab/)
το άρθρο. Ήρθε η ώρα για να τρέξουμε το minikube,
πάμε λοιπόν στο παράθυρο της γραμμής εντολών και
πληκτρολογούμε το εξής:

```bash
minikube start --vm-driver=virtualbox
```

*Output*

```bash
========================================
kubectl could not be found on your path. kubectl is a requirement for using minikube
To install kubectl, please do the following:

download kubectl from:
https://storage.googleapis.com/kubernetes-release/release/v1.7.0/bin/windows/amd64/kubectl.exe
Add kubectl to your system PATH

To disable this message, run the following:

minikube config set WantKubectlDownloadMsg false
========================================
Starting local Kubernetes v1.7.0 cluster...
Starting VM...
Downloading Minikube ISO
 97.80 MB / 97.80 MB [==============================================] 100.00% 0s
Getting VM IP address...
Moving files into cluster...
Setting up certs...
Starting cluster components...
Connecting to cluster...
Setting up kubeconfig...
Kubectl is now configured to use the cluster.
```

Κατά την διάρκεια της παραπάνω διαδικασίας,
ενδέχεται να σας ζητηθεί να δώσετε την **έγκρισή**
σας στο minikube να χρησιμοποιήσει το virtualbox.
Σε αυτή την περίπτωση εμείς αποδεχτήκαμε αυτή την
πρόταση και παραχωρήσαμε πλήρη δικαιώματα στο
minikube όσον αφορά το virtualbox. Κάνοντας το,
θα δείτε ότι το minikube θα φτιάξει μία εικονική
μηχανή, η οποία δεν θα είναι άλλη από το 
`single-node k8s cluster` που θέλουμε να φτιάξουμε.

![virtualbox image](/virtualbox.JPG)

Στην περίπτωση που νιώθετε μια ακατανίκητη επιθυμία
εξερεύνησης αυτού το VM, αρκεί να σας πούμε ότι
χρησιμοποιεί *password-less* root login:

![virtualbox root login](/virtualbox_minikube_vm.JPG)

## Αλληλεπίδραση με το cluster

Καθώς ξεκινήσαμε το minikube, θυμόμαστε ότι μας
ενημέρωσε πως το `kubectl` δεν είναι εγκατεστημένο
στο σύστημά μας. Στην περίπτωση που έχουμε  OSX ή
Linux, το Minikube λύνει μόνο του αυτό το πρόβλημα,
αλλά δυστυχώς δεν ισχύει το ίδιο στην περίπτωση των
Windows.

```bash
kubectl could not be found on your path. kubectl is a requirement for using minikube
To install kubectl, please do the following:

download kubectl from:
https://storage.googleapis.com/kubernetes-release/release/v1.7.0/bin/windows/amd64/kubectl.exe
Add kubectl to your system PATH
```

Για να λύσουμε αυτό το πρόβλημα, θα ακολουθήσουμε
τις οδηγίες που μας έδωσε το ίδιο το minikube,
δηλαδή θα κατεβάσουμε το **kubectl** από την [διεύθυνση](https://storage.googleapis.com/kubernetes-release/release/v1.7.0/bin/windows/amd64/kubectl.exe)
που μας υπέδειξε νωρίτερα. Σε αυτό το σημείο, θα θέλαμε
να σας τονίσουμε να **μην χρησιμοποιήσετε** το URL που
έχουμε στο άρθρο, αλλά αυτό που σας υπέδειξε το Minikube
στον υπολογιστή σας. Έτσι θα έχετε πάντα την *νεώτερη*
έκδοση του kubectl. 

Αφού κατέβει, στην συνέχεια, για λόγους τακτοποίησης
θα το τοποθετήσουμε στον ίδιο κατάλογο `C:/minikube`
μαζί με το `minikube.exe`, το οποίο το έχουμε ήδη
προσθέσει το `PATH` μας.

![Windows 10 Minikube Directory](/windows10.JPG)

Συγχαρητήρια! Πλέον είμαστε σε θέση και μπορούμε να
ξεκινήσουμε την αλληλεπίδραση με το k8s cluster μας.
Πηγαίνουμε πάλι πίσω στο παράθυρο της γραμμή εντολών
και δίνουμε την εντολή:

```bash
kubectl cluster-info 
Kubernetes master is running at https://192.168.99.100:8443
To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

Όπως βλέπετε, το `kubectl` (client) κατάφερε και
επικοινώνησε επιτυχγημένα με το `k8s API` (server)
και μας ενημέρωσε πως τρέχει στην διεύθυνση του
τοπικού μας δικτύου *192.168.99.100* και στην θύρα
*8443*.

Όπως είπαμε και στην αρχή, το συγκεκριμένο cluster
είναι *single-node* και έχει καθαρά εκπαιδευτικό
χαρακτήρα. Για δούμε πόσα nodes έχει το cluster μας,
δίνουμε την εντολή:

```bash
kubectl get nodes
NAME       STATUS    AGE       VERSION
minikube   Ready     4h        v1.7.0
```

Πράγματι, το cluster μας αποτελείται από μονάχα έναν
node, με το πρωτότυπο όνομα  minikube. Πλέον μπορείτε
να πειραματιστείτε ελεύθερα χωρίς φόβο και πάθος,
χρησιμοποιώντας είτε το kubectl είτε το ίδιο το
minikube. Για να δείτε τις επιλογές που σας παρέχει,
χρησιμοποιείστε τη --help flag ώς εξής:

```bash
minikube --help

Usage:
  minikube [command]

Available Commands:
  addons           Modify minikube's kubernetes addons
  completion       Outputs minikube shell completion for the given shell (bash)
  config           Modify minikube config
  dashboard        Opens/displays the kubernetes dashboard URL for your local cluster
  delete           Deletes a local kubernetes cluster
  docker-env       Sets up docker env variables; similar to '$(docker-machine env)'
  get-k8s-versions Gets the list of available kubernetes versions available for minikube
  ip               Retrieves the IP address of the running cluster
  logs             Gets the logs of the running localkube instance, used for debugging minikube, not user code
  mount            Mounts the specified directory into minikube
  profile          Profile sets the current minikube profile
  service          Gets the kubernetes URL(s) for the specified service in your local cluster
  ssh              Log into or run a command on a machine with SSH; similar to 'docker-machine ssh'
  ssh-key          Retrieve the ssh identity key path of the specified cluster
  start            Starts a local kubernetes cluster
  status           Gets the status of a local kubernetes cluster
  stop             Stops a running local kubernetes cluster
  update-context   Verify the IP address of the running cluster in kubeconfig.
  version          Print the version of minikube
```

## Φορτώνοντας το Dashboard

Οι παραπάνω εντολές, είναι στην ουσία ένας σύντομος
και εύκολος τρόπος να κάνετε τα ίδια πράγματα που
θα κάνατε χρησιμοποιώντας το `kubectl`, αλλά με το
minikube. Για παράδειγμα αν θέλαμε να φορτώσουμε
το [Kubernetes dashboard](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/),
με τον επίσημο τρόπο, θα δίναμε την εντολή:

```bash
kubectl create -f https://rawgit.com/kubernetes/dashboard/master/src/deploy/kubernetes-dashboard.yaml
```

και στην συνέχεια θα έπρεπε να κάνουμε `port-forward`
χρησιμοποιώντας το `kube-proxy`:

```bash
$ kubectl proxy
```

Αν και μπορούμε κάλλιστα να τα κάνουμε όλα αυτά,
το minikube είναι στην ευχάριστη θέση να τα κάνει
για εμάς, δίνοντας του την εξής εντολή:

```bash
C:\Users\drpan>minikube dashboard
Opening kubernetes dashboard in default browser...
```

Στην περίπτωσή μας, άνοιξε το Google Chrome
στην διεύθυνση `http://192.168.99.100:30000/`

![Kubernetes Dashboard Namespaces](/cluster_namespaces.JPG)

![Kubernetes Dashboard Workloads](/workloads.JPG)

Tέλος, για να κλείσουμε προσωρινά το minikube τρέχουμε την εντολή:

```bash
C:\Users\drpan>minikube stop
Stopping local Kubernetes cluster... 
Machine stopped.
```

Ενώ αν θέλουμε να διαγράψουμε τελείως το πρόσφατο
τοπικό cluster μας, δίνουμε την εντολή `minikube delete`:

```bash
C:\minikube>minikube.exe delete
Deleting local Kubernetes
Machine deleted
```

Καλή σας διασκέδαση :)
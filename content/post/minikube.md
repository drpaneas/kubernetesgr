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

Το στήσιμο ενός **Kubernetes cluster** δεν είναι εύκολη υπόθεση, ειδικά 
όταν στο σενάριο αυτής αναφερόμαστε σε πράγματα όπως το HA *(High
Availability)*. Ξεκινώντας **από το μηδέν** και δουλεύοντας 
αποκλειστικά στο τοπικό μας δίκτυο, θα δείξουμε αναλυτικά και **βήμα
προς βήμα** πώς μπορούμε να δημιουργήσουμε ένα - μικρό μεν, 
λειτουργικό δε - kubernetes cluster, χρησιμοποιώντας το **minikube**.



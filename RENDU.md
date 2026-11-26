# Rendu — Atelier Kubernetes

> Complétez ce fichier au fil de l'atelier, puis faites un commit et un push (voir la section « Rendu » du README).

**savary / matteo :**
**Groupe :**

---

## Exercice 1 — Cluster

1. Combien de nœuds affiche `kubectl get nodes` ? Quel est le rôle de chacun ?
   >_3.k3d-lab-server-0 (control plane : API Server, scheduler, etcd/stockage de l'état du cluster, il décide où placer les pods) et k3d-lab-agent-0 et k3d-lab-agent-1 (workers : ils exécutent les pods)_
2. Que montre `docker ps` ? Qu'est-ce qu'un « nœud » dans k3d, concrètement ?
   >_docker ps montre un conteneur par nœud (k3d-lab-server-0, k3d-lab-agent-0, k3d-lab-agent-1), plus un conteneur k3d-lab-serverlb qui est le load balancer. Dans k3d, un nœud est un conteneur Docker qui fait tourner k3s : le « cluster multi-machines » est simulé sur une seule VM._

## Exercice 2 — Premier déploiement

3. Quels objets Kubernetes ont été créés par `kubectl create deployment` ? (indice : `kubectl get all`)
   >_Un Deployment `web`, qui a créé un ReplicaSet, qui a créé un Pod. La commande `kubectl expose` a ajouté un Service `web` (ClusterIP). Le Service `kubernetes` était déjà là par défaut_
4. Vous avez supprimé le pod `web-...` : que s'est-il passé, et pourquoi ?
   >_Un nouveau pod a été créé automatiquement, avec un nom différent (le suffixe aléatoire change). Le ReplicaSet maintient le nombre de réplicas demandé (1) : dès qu'un pod disparaît, il en recrée un pour revenir à l'état désiré (auto-réparation)_

## Exercice 3 — Modifier un conteneur en direct

5. Après suppression du pod, votre message « Bienvenue... » a-t-il survécu ? Pourquoi ?
   >_non. ça modifie le système de fichiers du conteneur, qui est éphémère. Le nouveau pod repart de l'image `nginx` d'origine, donc la page Cette page ne fonctionne pas s'affiche_
6. Pourquoi le `port-forward` a-t-il cessé de fonctionner après la suppression du pod ?
   >_Le `port-forward` est attaché à un pod précis (il l'a résolu au lancement), pas au Service de façon durable. Quand ce pod est supprimé, le tunnel perd sa cible et s'arrête ; il faut le relancer vers le nouveau pod_

## Exercice 4 — Déclaratif

7. Quelle différence entre `kubectl create` et `kubectl apply` ?
   >_create est impératif : il crée une ressource et échoue si elle existe déjà. apply est déclaratif : je décris l'état désiré dans un fichier YAML, et Kubernetes crée la ressource si elle n'existe pas ou la met à jour si elle diffère. On peut le rejouer sans erreur, et les fichiers sont versionnables dans Git_
8. À quoi sert le `selector` d'un Service ? Que se passe-t-il s'il ne correspond à aucun pod ?
   >_Il désigne, par leurs labels, les pods vers lesquels le Service envoie le trafic. S'il ne correspond à aucun pod, le Service est créé sans erreur mais n'a aucun endpoint (`kubectl get endpoints` est vide) : il n'est joignable vers rien et les requêtes échouent_

## Exercice 5 — Scaling & mises à jour

9. Sur quels nœuds se trouvent vos 3 réplicas ? Collez la sortie de `kubectl get pods -l app=site -o wide`.
   ```

   ```
10. Avec l'image `nginx:9.99`, quel statut avait le pod ? Quelle commande vous a donné la cause ?
    >
11. Pendant la panne, le site restait-il accessible ? Pourquoi ?
    >

## Exercice 6 — Portainer & Mario

12. URL publique de votre Mario :
    >
13. Retrouvez le Deployment `mario` en ligne de commande. Portainer et `kubectl` parlent-ils au même composant ? Lequel ?
    >

## Bonus (facultatif)

14. Bonus réalisés et remarques :
    >

---

**Score obtenu avec `./scripts/check.sh` :** ... / ...

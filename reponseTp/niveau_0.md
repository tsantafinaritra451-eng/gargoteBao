1. Écrivez le schéma relationnel des 4 tables

- categorie(**id**, libelle)
- plat(**id**, nom, prix, #id_categorie)
- commande(**id**, date_commande, numero_table)
- commande_ligne(**id**, #id_commande, #id_plat, quantite, prix_unitaire )

2. Pourquoi `commande_ligne` contient-elle une colonne `prix_unitaire`
- le commande_ligne contient une colonne prix_unitaire parce que si a tout moment on change le prix d'une plat c'est a partir dans cette ligne commande qu'on change et parce qu'elle est directement lier a  id_commande on obtient la date de cette changement

exemple : actuellement le prix de Vary amin''anana est  2000
mais dans apres 2 jour ca augmente de 3000 ar donc la on insert dans commande_ligne la nouvelle valeur mais l’ancienne commande conserve 2 000 Ar.  

3. Que se passe-t-il pour les lignes d’une commande quand on supprime la commande ?
- quand on supprime une commande tout les icd_commande qui correspond a l'id commande qu'on a supprimer s'efface

3. Quelle instruction en est responsable ?
- ON DELETE CASCADE c'est le responsable car Cette instruction signifie que la suppression du parent (`commande`) se propage à ses lignes enfants.

4. Repérez **trois données absurdes** que le schéma actuel accepte sans broncher
    - lors de l'insertion de plat quand on insert le prix negative ca marche
    - concernant le numero de table vu qu'on a pas encore de table tables on peut mettre n'importe quel numero et ca envoye a la cuisine meme si cette numero existe pas
    - un numéro de table nul ou négatif dans `commande`, par exemple `numero_table = 0` ou `-3`
    - une quantité négative ou nulle dans `commande_ligne`, par exemple `quantite = -2` ;


5. Pour la requête **b**, quel code obtenez-vous ? Pourquoi pas 200 ?
- c'est pas 200 parce que c'est une creation c'est une insert plus precisement c'est une POST mais 200 est le code pour dire que recuperation reussi (GET) 200 veut dire exactement qu'un requete est reussi , 201 indique une creation

6. Pour la requête **d**, qui bloque la suppression : PHP ou PostgreSQL ? Montrez la ligne de code qui transforme ce blocage en réponse 409.
- je pense c'est postgres qui bloque la suppression

7. Commande avec curl
curl -i -X POST http://localhost:8080/api/commandes \
  -H "Content-Type: application/json" \
  -d '{"numero_table":6,"lignes":[{"id_plat":4,"quantite":2},{"id_plat":10,"quantite":1}]}'

8. la creation de commande est dans CommandeService 

Pourquoi? :l n’est pas dans le contrôleur `Commandes.php`, car le contrôleur doit surtout traduire HTTP en appel PHP puis renvoyer une réponse HTTP. Le service centralise les règles métier

9. fichier + explication

| Notion | À quoi ça sert | Votre exemple |
|---|---|---|
| `ref()` | une variable dont le changement met l’écran à jour | dans front\src\view , ligne :7 il utilise une ref plat qui permet de cree une variable reactive plat comme ca quand le plat change ref change automatiquement|
| `computed()` | une valeur calculée à partir d’autres, recalculée automatiquement |dans front\src\view\commandeview.php, ligne :13 le calcule une filtre par categorie |
| `v-for` | répéter un élément pour chaque élément d’une liste | front\src\view\commandeview.php, ligne 31 boucle commande par id pour obtenir ses champs et les lignes|
| `v-if` / `v-else` | afficher un élément seulement si une condition est vraie | front\src\view\historiqueview.php ligne 23 , c'est une condition pour dire si il y a pas de commande on affiche "Aucune commande enregistrée" sinon creation de table clicable avec une boucle de commande pour tous les parcourir|
| `v-model` | lier un champ de formulaire à une variable, dans les deux sens |front\src\view\commandeview.php ligne 74 |
| `defineProps` | recevoir des données du composant parent | front\src\components\panier.php ligne 6 c'est comme definir les arguments|
| `defineEmits` / `emit` | envoyer un événement au composant parent | front\src\components\platList ligne 11 c'est une evenement envoyer par enfant vers parent|
| `onMounted` | exécuter du code quand le composant apparaît | front\src\view\commandeview.php ligne 15 cette fonction est appeler apres le chargement de templates|


9. `Panier.vue` ne modifie jamais la quantité parce que les props sont la propriété du parent et doivent être considérées comme en lecture seule.

La quantité est réellement modifiée dans [CommandeView.vue] ligne 21

10. le role de Watch : Quand le parent sélectionne un autre plat, il recopie ses valeurs (`nom`, `prix`, `id_categorie`) dans le formulaire local. L’option `{ immediate: true }` exécute aussi cette synchronisation dès le premier affichage.

- sans ce `watch`, cliquer sur **Modifier** changerait bien le plat sélectionné et le titre du formulaire, mais les champs conserveraient les anciennes valeurs

11. elle apparait direct dans la liste grace au ref () mais aussi le boucle v-for





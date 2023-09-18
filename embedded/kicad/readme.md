# Notes rapides pour créer un PCB
1. Créer un nouveau projet
2. Créer un nouveau schématique
3. Ajouter les composants
   1. Pour les composants, je suggère d'appliquer en partant le *footprint*
   2. Ensuite, dupliquer les composants qui sont identiques
4. Ajouter les annotations
   1. Tools -> Annotate Schematics
   2. Ça numérote les composants
5. Exécuter "Electrical Rules Check"
   1. Tools -> Electrical Rules Check
   2. Ça vérifie les erreurs électriques
6. Exécuter "Assign Footprints"
   1. Tools -> Assign Footprints
   2. Sélectionner la catégorie de la composante
   3. Appliquer le *footprint* à tous les composants
7. Ouvrir le PCB editor
8. Cliquer sur "Update PCB from Schematics"
   1. Tools -> Update PCB from Schematics
   2. Ça crée le PCB
9. Placer les composants
   1. Pour placer les composants
10. Tracer les pistes
11. Tracer le contour du PCB
12. Générer le 3D

## Pour mettre une pièce dans le dos du PCB
1. Double-cliquer sur la pièce
2. Dans la zone "Side", sélectionner "Back"

## Astuces
- Ajouter des points de test
  - Pour ajouter des test points, utiliser le *footprint* "TestPoint_THT"

# Fabrication
- Toujours générer le drill file
  - Sinon les trous ne seront pas percés
- 
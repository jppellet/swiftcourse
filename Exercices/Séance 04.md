# Développement iOS avec Swift — Séance d’exercices 4

## Exercice 1 — UITableView

 1. Dupliquez le projet du cours avec l’exemple de la UITableView. Faites en sorte que la hauteur des cellules soit plus grande dans la section 2 que dans la section 1, et soit encore plus grande dans la section 3. Pour ce faire, trouvez la méthode à implémenter dans le protocole `UITableViewDelegate`.

 2. Si la méthode `func tableView(tableView: UITableView, moveRowAtIndexPath sourceIndexPath: NSIndexPath, toIndexPath destinationIndexPath: NSIndexPath)` est implémentée, le mode Edition de la UITableView permet de réordrer les cellules. Implémentez cette méthode pour simplement afficher, par exemple, `Would now move cell 3 in section 0 to cell 0 in section 1` en fonction des paramètres reçus. (En pratique donc, votre implémentation ne réordrera rien, mais permet d’afficher l’interface pour ce faire.)

 Faites tourner le projet sur le simulateur et essayez de réorder des cellules. Faites défiler en haut et en bas. Quel est l’effet su défilement sur les cellules réordrées, masquées puis réaffichées? À quoi pourrait-ce être dû?
 
 3. Pour le moment, on peut transférer des cellules d’une section à l’autre, il n’y a pas de restriction. Empêchez-le en implémentant la méthode `func tableView(tableView: UITableView, targetIndexPathForMoveFromRowAtIndexPath sourceIndexPath: NSIndexPath, toProposedIndexPath proposedDestinationIndexPath: NSIndexPath) -> NSIndexPath`. Cette méthode est appelée quand la table se propose de déplacer une cellule de `sourceIndexPath` vers la position graphique qui correspond à `proposedDestinationIndexPath`. Elle doit retourner un objet de type `NSIndexPath` qui est la position approuvée de destination de la nouvelle cellule. L’implémentation de base, qui permet tout, retourne simplement `proposedDestinationIndexPath`. Faites en sorte que les cellules ne puissent pas s’échapper de leur section.
     * Si elles sont glissées sur une section précédente, proposez de les positionner tout au début de leur propre section à la place;
     * si elles sont glissées sur une section suivante, proposez de les positionner tout à la fin de leur propre section à la place;
     * sinon, acceptez de les déplacer normalement.

 4. Trouvez quelle méthode implémenter pour afficher (par exemple) `Would now delete data from cell 2 in section 0` lorsqu’une cellule est supprimée par l’utilisateur.

## Exercice 2 — UIGestureRecognizer

 1. Dupliquez le projet du cours avec l’exemple des UIGestureRecognizers. Le code qui s’y trouve montre comment un *pinch* a un effet de zoom sur l’image, et comment un *rotate* fait pivoter l’image, tout ceci en réglant la propriété `transform` de la vue déplacée. `transform` est une matrice de transformation en 2D qui est appliquée à la vue entière. Une série de fonctions dont le nom commence par `CGAffineTransform` permet de créer de telles matrices ou de les multiplier/combiner.

 Ajoutez un `UIPanGestureRecognizer` aux deux autres déjà existants et, dans la méthode appelée lorsqu’il est activé, faites en sorte que la vue soit déplacée lors d’un *pan*.
 
 Comment trouvez-vous les paramètres de la translation à effectuer? Imprimez-les avec `println` pour les voir sur la console en live lors du mouvement. Comparez-les entre un déplacement de l’image sans rotation préalable, et un déplacement de l’image qui a déjà subi une rotation. Que constatez-vous? Que se passerait-il si l’on se trompait de vue de référence lors du calcul de la translation à effectuer?
 
 2. Faites en sorte qu’un *double tap* sur l’image la remette en position initiale. Bonus: utilisez la méthode `UIView.animate(duration) { ... }` pour faire en sorte que l’image se remette en position initiale en animant le changement de position sur une durée d’une demi-seconde.
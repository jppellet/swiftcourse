# Développement iOS avec Swift — Séance d’exercices 8

## Exercice 1 — NSCoding

 1. Dupliquez le projet du cours 8 et ouvrez le playground `Tests.playground`. Directement dans le playground, ajoutez une classe `Review` (pour modéliser les commentraires laissés sur un livre donné par des acheteurs), qui aura deux propriétés: `author: String` et `reviewText: String`. Faites en sorte que la classe adopte le protocole `NSCoding`.
 2. Écrivez quelques lignes de code pour tester votre sérialisation de la classe `Review`, similaires à celles écrites pour tester la classe `Book`.
 3. Ajoutez une propriété `reviews` à la classe `Book`. Donnez-lui comme type `[Review]`, et changez le code de sérialisation pour sauvegarder aussi les `Review`s liées à un livre.


## Exercice 2 — Core Data
 
 1. Faites tourner l’application du projet du cours 8. Elle affiche dans une `UITableView` tous les `Assignment`s qui ont été trouvés par Core Data, ou en crée 4 de base s’il n’y en a aucun. Ouvrez `ViewController` et trouvez:
     * où les objets sont récupérés;
     * où les objets sont créés;
     * où les objets sont détruits (lorsqu’on *swipe* à travers une cellule).
    
    Repérez bien comment chaque opération finit par passer par le `NSManagedObjectModel`, qui a été lui créé au tout début par le `AppDelegate`.
 2. Depuis le simulateur, détruisez un objet en *swipant*, puis quittez l’application. Relancez-la. Pourquoi est-ce que l’objet réapparaît? Trouvez comment corriger le comportement.
 3. Trouvez comment inverser l’ordre dans lequel les objets sont renvoyés lors de l’exécution de la `NSFetchRequest`.
 4. Ajoutez ce code, entre la création des `Assignment`s et la sauvegarde du contexte:

        let userNames = [
        	("Alice", "Sommersby"),
        	("Bob", "Mitchell"),
        	("Carol", "Loman")
        ]
        
        let users = userNames.map({ (firstName, lastName) in
        	return User.createInManagedObjectContext(moc, firstName: firstName, lastName: lastName)
        })
        
        for assignment in assignments {
        	for user in users {
        		let result = Int64(50 + arc4random_uniform(50))
        		Result.createInManagedObjectContext(moc, result: result, comment: "Good", assignment: assignment, student: user)
        	}
        }

    Ce code insère des utilisateurs et des résultats aléatoire dans Core Data, mais fait appel à des fonctions qui n’existent pas encore. Implémentez-les pour effectuer la création des objets de façon similaire à ce qui est fait pour la création des `Assignment`s. Implémentez également la propriété dérivée `fullName` pour un utilisateur (dont la valeur est formée du prénom, d’une espace, puis du nom de famille), ceci sans modifier le modèle Core Data existant.

 5. Voici le code pour un `UITableViewController` tout simple qui affiche dans une table l’array de `Result` qu’on lui donne:
 
        class ResultsViewController: UITableViewController {
        	var results = [Result]()
        	override func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        		return results.count
        	}
        	override func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
        		let cell = tableView.dequeueReusableCellWithIdentifier("cell") as? UITableViewCell ?? UITableViewCell(style: .Subtitle, reuseIdentifier: "cell")
        		let res = results[indexPath.row]
        		cell.textLabel?.text = res.student.fullName
        		cell.detailTextLabel?.text = "\(res.result) (\(res.comment))"
        		return cell
        	}
        }

    Insérez-le dans le projet et faites-en sorte qu’un *tap* sur une cellule de la table Assignments nous montre tous les résults liés à cet Assignment, du meilleur au moins bon.<br>
    **Indice:** pour afficher un nouveau view controller `newViewController`, utilisez simplement ce code: `navigationController?.pushViewController(newViewController, animated: true)`
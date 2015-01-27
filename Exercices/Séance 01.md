# Développement iOS avec Swift — Séance d’exercices 1

## Exercice 1 — Variables et constantes

 1. Écrivez une ligne de code qui multiplie une variable de type `Int` avec une variable de type `Double`. Pourquoi est-ce que ça ne marche pas? Comment le faire?<br>
Quels sont les avantages et inconvénients de l’approche utilisée par Swift?

 2. Écrivez du code qui construit un String de la forme `1, 2, 3, 4, 5, 6, 7, 8, 9, 10`, où les nombres de début et de fin sont déterminés par deux constantes de type `Int`.

 3. Depuis la constante:

        let inputText = "the quick brown fox jumps over the lazy dog"
    construisez la variable `outputText` qui contient le même texte, mais où la première lettre de chaque mot est en majuscule. Utilisez pour cela les méthodes `componentsSeparatedByString` et `capitalizedString`.


## Exercice 2 — Arrays et dictionaries

 1. Créez un array modifiable de `Int`s avec quelques éléments. Ajoutez des éléments à la fin. Ajoutez des éléments au début. Ajoutez des éléments au milieu. À quelles méthodes faites-vous appel? Comment les paramètres supplémentaires sont-ils indiqués? Est-ce que ça vous rappelle quelque chose d’Objective-C?
 2. On peut lire et écrire à une position précise d’un array avec la notation `myArray[i]` où `i` est un `Int`. Mais un peut aussi passer un objet de type `Range` entre les crochets, comme ceux créés par l’expression `1 ... 5` ou `2 ..< 4`. En utilisant des `Range`s entre crochets, trouvez comment:
    * Détruire une plage de l’array
    * Insérer des éléments dans l’array en remplaçant ceux qui existent
    * Ajouter des éléments au milieu sans détruire ceux qui existent
 3. Trouvez ce que fait l’expression `[Double](count: 30, repeatedValue: 2.5)`
 4. Dans Xcode, faites Commande-Majuscule-O et tapez «array». Ceci devrait vous mener à la définition d’un array depuis la bibliothèque standard de Swift. Parcourez un peu le code qui définit des arrays. Notez que la définition est répartie dans un `struct` de base et dans plusieurs `extension`s, dont on reparlera prochainement.


## Exercice 3 — Fonctions, closures et génériques

 1. Écrivez une fonction `func fibo(n: Int) -> Int` récursive (qui s’appelle elle-même) qui calcule le n<sup>ième</sup> élément de la [suite de Fibonacci](http://fr.wikipedia.org/wiki/Suite_de_Fibonacci), où les deux premiers éléments sont 1, et où les éléments suivants sont l’addition des deux éléments précédents.<br>
Utilisez les indications du playground pour voir combien d’appels sont faits à cette fonction pour calculer `fibo(5)`, `fibo(10)` et `fibo(12)` (n’essayez pas trop de plus grands nombres…).<br>
Essayez de faire tourner cette fonction, qui fait le même calcul:

        func fibo_imperative(n: Int) -> Int {
        	if n == 1 || n == 0 {
	        	return 1
	        } else {
	        	var lastbut1 = 1
	        	var last = 1
	        	var niter = n - 2
	        	while niter > 0 {
	        		let newLastbut1 = last
	        		let newLast = lastbut1 + last
	        		lastbut1 = newLastbut1
	                last = newLast
	        		niter--
	        	}
	        	return last + lastbut1
            }
        }
Combien de fois la boucle est-elle exécutée pour `fibo_imperative(12)`? Cette version est nettement plus efficace, mais utilise à une boucle `while` et à des `var`, qui n’existent pas en programmation purement fonctionnelle. Essayez de réimplémenter ceci en utilisant une fonction auxiliaire selon ce modèle:

        func fibo_loop(lastbut1: Int, last: Int, niter: Int) -> Int {
        	// TODO
        	return 0
        }
        
        
        func fibo2(n: Int) -> Int {
        	if n == 1 || n == 0 {
        		return 1
        	} else {
        		return fibo_loop(1, 1, n - 2)
        	}
        }
Le principe est que chaque itération de la boucle est remplacé par un appel (récursif) de méthode, et chaque variable par un paramètre de la fonction.<br>
Combien d’appels à `fibo_loop` est-ce que l’appel `fibo2(12)` génère?

 2. Créez une fonction `bindFirst` générique qui accepte deux arguments:
    * n’importe quelle fonction `f` à deux arguments (le premier de type générique `A` et le deuxième de type générique `B`) et qui retourne `C`; et
    * un argument du type `A`,
   
   et renvoie une fonction de type `B -> C` qui appelle `f`. Voici sa signature:
        
        func bindFirst<A, B, C>(f: (A, B) -> C, a: A) -> B -> C
    Ceci par exemple devrait ensuite renvoyer la valeur `30.0`:
    
        let times10 = bindFirst(*, 10.0)
        times10(3.0)
        
 3. Trouvez une signature appropriée pour une fonction générique `compose` qui prend deux fonctions `f` et `g` à un argument chacune et retourne une fonction qui applique d’abord `g` puis `f` à un argument donné. Implémentez la fonction. Testez-la avec ceci, à la suite du code de la partie précédente:

        let add10 = bindFirst(+, 10.0)
        let multThenAdd = compose(add10, times10)
        multThenAdd(3.0)        // doit renvoyer 40.0
        let addThenMult = compose(times10, add10)
        addThenMult(3.0)        // doit renvoyer 130.0
        let whaaat = compose(multThenAdd, addThenMult)
        whaaat(3.0)             // doit renvoyer 1310.0

 4. Écrivez une fonction `twice` accepte une fonction `f` et qui retourne une fonction qui applique deux fois `f` à son argument. Ceci devrait retourner `300.0`:
    
        let times100 = twice(times10)
        times100(3.0)

 5. Écrivez une fonction `ntimes` accepte une fonction `f` et un `Int` `n` et qui retourne une fonction qui applique `n` fois `f` à son argument. Ceci devrait retourner `3,000,000.0`:
    
        let times1million = ntimes(times10, 6)
        times1million(3)
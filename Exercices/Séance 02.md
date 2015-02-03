# Développement iOS avec Swift — Séance d’exercices 2

## Exercice 1 — Classes, enums et propriétés

 1. Déclarez une classe `Person` avec deux champs/propriétés variables `firstName` et `lastName` de type `String`, et une propriété `middleName` de type `String?`. Définissez un `init` qui permet d’omettre `middleName`. Déclarez la propriété dérivée `fullName` qui est la concaténation de `firstName`, `middleName` s’il est défini, et `lastName`. Faites attention aux espaces.<br/>
    Ceci devrait afficher `Sheldon Lee Cooper` et `Howard Wolowitz`:
    
        println(Person(firstName: "Sheldon", middleName: "Lee", lastName: "Cooper").fullName)
        println(Person(firstName: "Howard", lastName: "Wolowitz").fullName)

 2. Modifiez la classe `Person` pour lui rajouter une propriété de type `Title`, qui sera une `enum`. Faites en sorte que ceci affiche `Dr. Sheldon Lee Cooper` et `Mr. Howard Wolowitz`:

        println(Person(title: .Dr, firstName: "Sheldon", middleName: "Lee", lastName: "Cooper").fullName)
        println(Person(title: .Mr, firstName: "Howard", lastName: "Wolowitz").fullName)
        
 3. Faites en sorte que votre enum puisse prévoir un cas spécial d’un titre plus exotique, et que ceci affiche `Prof. Dr.-Ing. Dr. h. c. Günter Hommel` ([lien inutile](http://pdv.cs.tu-berlin.de/leute/hommel.html)):

        println(Person(title: .Other("Prof. Dr.-Ing. Dr. h. c."), firstName: "Günter", lastName: "Hommel").fullName)
        
## Exercice 2 — Fonctions

 1. Écrivez une fonction `repeat` qui exécute un certain nombre de fois une fonction passée en argument. Elle devrait pouvoir s’utiliser comme ceci:
    
        repeat(4) {
            println("hey") // affiche 4 fois "hey"
        }
        
    Cette syntaxe marche pour une fonction qui accepte deux arguments dont le deuxième est une closure grâce à une petite astuce du compilateur Swift, appelé [Trailing Closure Syntax](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Closures.html#//apple_ref/doc/uid/TP40014097-CH11-ID102).
    
 2. Écrivez une fonction `timedOperation` qui exécute une certaine fonction et affiche après l’exécution le temps que l’exécution a pris. Elle devrait pouvoir s’utiliser comme ceci, et, dans cet exemple, afficher quelque chose comme `Operation "test wait" lasted 1003 ms`:
    
        timedOperation("test wait") {
            sleep(1)
        }
        
    Pour l’implémenter, vous pouvez utiliser l’expression `NSDate().timeIntervalSince1970`, qui vous fournit un `Double` qui est le nombre de secondes écoulées depuis le 1<sup>er</sup> janvier 1970.
    
## Exercice 3 — Génériques et collections

 1. Écrivez une fonction générique `unwrap` qui prend un tuple `(A?, B?)` et qui retourne `(A, B)?` avec un `nil` si l’un des deux composants est `nil`, ou un tuple si les deux composants ne sont pas `nil`.<br/>
    Le code suivant devrait afficher `n/a`, `n/a` et `6`:
    
        func add(fromTuple tuple: (Int?, Int?)) {
        	if let (a, b) = unwrap(tuple) {
        		println(a + b)
        	} else {
        		println("n/a")
        	}
        }
        
        add(fromTuple: (1, nil))
        add(fromTuple: (nil, 5))
        add(fromTuple: (1, 5))
        
 2. Écrivez une fonction générique `forall` qui prend un array de type générique `A`, un prédicat (condition) sous forme de fonction `A -> Bool`, et qui retourne un `Bool` qui dit si oui ou non la condition passée est vraie pour tous les éléments de l’array.<br/>
    Par exemple: `allOdds` devrait être `true` et `allUnderTen` `false`.
    
        let array = [1, 3, 7, 9, 11, 17]
        let allOdds = forall(array) { $0 % 2 == 1 }
        let allUnderTen = forall(array) { $0 < 10 }

 3. Écrivez une fonction générique `groupBy` dont la signature est `groupBy<A, B: Hashable>(array: [A], f: A -> B) -> [B: [A]]` et qui retourne un dictionnaire où chaque élément de l’array original `array` est dans un array lié à la clé calculée par `f`.<br>
    Par exemple, ceci groupe les mots des 4 premiers vers de [*I cannot live with you* d’Emily Dickinson](http://www.poets.org/poetsorg/poem/i-cannot-live-you-640) selon la longueur des mots:
    
        let words = [
        	"I", "cannot", "live", "with", "You",
        	"It", "would", "be", "Life",
        	"And", "Life", "is", "over", "there",
        	"Behind", "the", "Shelf"
        ]
        
        let wordsByLength = groupBy(words) { countElements($0) }

    `wordsByLength` est donc ici un dictionnaire qui relie des `Int` (une taille de mot) à des `[String]` (la liste de tous les mots qui ont cette taille). La clé `6` est reliée à la valeur `["cannot", "Behind"]`, la clé `5` à la valeur `["would", "there", "Shelf"]`, etc.<br/>
    De façon similaire, ceci groupe les éléments selon deux clés: dans `wordsByUppercase`, la clé `true` est reliée à la liste de tous les mots qui commencent par une majuscule, et la clé `false` à tous les autres mots:
    
        func isUpperCase(c: Character) -> Bool {
        	return ("A"..."Z").contains(c)
        }
        
        let wordsByUppercase = groupBy(words, { isUpperCase($0[$0.startIndex]) } )

 4. Écrivez une fonction générique `mapValues` avec la signature `mapValues<A: Hashable, B, C>(dict: [A: B], f: B -> C) -> [A: C]` qui retourne un nouveau dictionnaire avec toutes les clés du dictionnaire original, mais reliées aux valeurs transformées avec la fonction `f`.<br/>
    Ceci par exemple définit `numberOfWordsByLength`, de type `[Int: Int]`, où `numberOfWordsByLength[i] ?? 0` donne le nombre de mots de longueur `i` de l’extrait ci-dessus:
    
        let numberOfWordsByLength = mapValues(wordsByLength) { $0.count }

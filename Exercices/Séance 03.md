# Développement iOS avec Swift — Séance d’exercices 3

## Exercice 1 — Pas de base pour la création d’une app

 1. Créez depuis le début une app vide sans nib, xib ou storyboard (à part `LaunchScreen.xib`).
 2. Ajoutez le code de la création de la fenêtre de l'app dans la classe `AppDelegate`.
 3. Déclarez une nouvelle sous-classe `MainViewController` de `UIViewController`. Dans `AppDelegate`, instantiez-la, puis donnez-la comme propriété `rootViewController` de votre fenêtre. Votre app devrait maintenant tourner sans alerte ou crash.

*Le corrigé de cet exercice est, grosso modo, le projet «Cours3 - 1. app vide» du dossier du cours, mais c'est utile de faire le processus une fois complètement.*

## Exercice 2 — Swift et UI

 1. Dupliquez le projet «Cours3 - 2. UI en code étape (f)». Regardez la ligne dérangeante `let button = (UIButton.buttonWithType(.System) as! UIButton) <| {`. Le cast (`as!` en Swift 1.2, `as` avant) est dû au fait que `buttonWithType` vient d'ObjC, où il retourne `id`. Ajoutez une extension à `UIButton` (avec la syntaxe simple `extension UIButton { ... }` et déclarez-y une méthode de classe (`class func` en Swift; les méthodes `+` en ObjC) pour qu'on puisse remplacer cette ligne par:
       
        let button = UIButton.System() <| {
        
 2. Dupliquez le projet «Cours3 - 2. UI en code étape (f)» et effacez tout le code de la méthode `viewDidLoad` de `MainViewController`, à part le *supercall*. Ajoutez ensuite du code de création de vues et du code autolayout qui utilise la bibliothèque *Cartography* qui crée 3 labels les uns sous les autres avec ces contraintes:
    * tous sont centrés horizontalement;
    * le premier est à 50 pixels du haut de la fenêtre;
    * les autres sont à 10 pixels du précédent;
    * leur largeur augmente de 10% chaque fois.
   
   Changez votre votre pour que `numLabels` (selon une constante dans le code plutôt que selon le nombre prédéfini 3) soient générés. Voici ce que ça donne avec `numLabels = 15`:
   
   ![](https://raw.githubusercontent.com/jppellet/swiftcourse/master/Exercices/Séance%2003%20Img%2001.png)
   
 3. Faites la même chose que 2. avec Interface Builder. :-) (OK, non, tant pis.)

## Exercice 3 — Meilleure syntaxe pour les callbacks

 Dupliquez le projet «Cours3 - 2. UI en code étape (f)». Cette syntaxe nous gêne: `$0.addTarget(self, action: "buttonTapped:", forControlEvents: .TouchUpInside)`. En effet, si on se trompe dans le nom de la méthode à appeler, nous avons un crash lors de l'exécution sans erreur duc ompilateur.<br/>
Pouvez-vous faire en sorte que cette ligne ainsi que le méthode appelée puissent être remplacées par ceci?
    
    $0.add(action {
        println("button was tapped")
    })
        
Indices:

 * Créez une fonction `action` qui prend une fonction et retourne un objet pour stocker la fonction à exécuter
 * Vous devrez déclarer cet objet avec des options de compatibilité avec ObjC, par exemple comme ceci: `@objc class ActionTarget: NSObject { ... }`
 * Ajoutez une méthode `add` qui accepte un `ActionTarget` à la classe `UIControl` par une extension
 * Vous pouvez hard-coder la valeur `.TouchUpInside` pour le dernier paramètre de la méthode `addTarget` que vous remplacez
 * Quels sont les éventuels problèmes avec l'ARC que cela peut générer? Comment les éviter?
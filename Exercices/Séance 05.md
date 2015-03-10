# Développement iOS avec Swift — Séance d’exercices 5

## Exercice 2 — @IBInspectable

Dupliquez le projet du cours avec l’exemple de la terminé de la UIView personnalisée. Faites en sorte que ces éléments suivants soient réglables directement dans Interface Builder pour `WaterdropView` avec l’annotation `@IBInspectable`, comme actuellement `lineWidth`:

 1. la couleur de la bordure;
 2. la couleur de remplissage;
 3. le rayon initial des gouttes;
 3. la durée de vie des gouttes.

## Exercice 2 - Animations

 1. Pour l’instant, le `NSTimer` est «bête»: il force la vue à s’afficher 60 fois par seconde même s’il n’y a aucun changement à faire à l’écran. Modifiez la création du timer pour l’effectuer à la fin de la méthode `drawRect`, après le filtre des éléments, uniquement s’il reste encore des gouttes à afficher. Sinon, détruisez le timer avec sa méthode `invalidate`.
 2. Faites en sorte que le diamètre des gouttes devienne de plus en plus petit au fil de leur vie.

## Exercice 3 - Polygones

Créez un nouveau projet avec vue unique et faites votre propre sous-classe de `UIView` comme dans le cours, qui doit afficher un poligone régulier à `n` côté, où `n` est `@IBInspectable`. Testez votre vue dans Interface Builder. Pour ce faire:

 * utilisez la fonction `CGContextMoveToPoint` pour vous déplacer à un point (sans rien dessiner);
 * utilisez `CGContextAddLineToPoint` pour créer un segment entre le point actuel où l’on est et le nouveau passé en paramètre;
 * tout à la fin, appelez `CGContextStrokePath` pour effectivement dessiner le tracé effectué par les appels précédents.
# Développement iOS avec Swift — Séance d’exercices 6

Dupliquez le projet du cours pour travailler sur les exercices suivants.

## Exercice 1 — Rotation en 2D

 1. Faites en sorte qu'un clic sur le bouton «2D» fasse tourner la vue de 360° sur elle-même en changeant l'angle passé à la fonction `CGAffineTransformRotate`. Pourquoi est-ce que ça ne marche pas?
 2. Effectuez deux animations de 180° de suite. Pour cela, utilisez cette structure pour profiter sdu bloc `completion` appelé à la fin de l'animation:

        UIView.animateWithDuration(0.5, delay: 0, options: UIViewAnimationOptions(0), animations: {
			// tourner de M_PI
		}, completion: { completed in
			UIView.animateWithDuration(0.5, delay: 0, options: UIViewAnimationOptions(0), animations: {
				// tourner une deuxième fois de M_PI
			}, completion: nil)
		})
		
	Pourquoi est-ce que ça ne marche qu'à moitié?
	
 3. Changez les options d'animation et la *easing function* pour lui dire d'accélérer au début de la première animation mais pas à la fin, et à la fin de la seconde mais pas au début. Pour cela, inspectez les valeurs possibles pour le type `UIViewAnimationOptions`.

## Exercice 2 - CATextLayer

 1. Ajoutez, comme sous-layer au layer qui contient l'image, un `CATextLayer` qui affiche le string `"Hello"` en blanc, taille 50, centré horizontalement, en bas de l'image. Vérifiez que le `CAReplicatorLayer` fasse bien son travail et reproduise non seulement l'image, mais aussi le texte dans la réflexion.

    ![](https://raw.githubusercontent.com/jppellet/swiftcourse/master/Exercices/Séance%2006%20Img%2001.png)

 
 2. Ajoutez un bouton dans la barre de navigation qui crée une animation avec le texte en le bougeant de 50 pixels vers le haut. Vérifiez que l'animation fonctionne aussi dans la réflexion.

## Exercice 3 - CAGradientLayer

 1. Changez l'arrangement les layers pour faire en sorte que la réfexion soit penchée avec un angle inverse de l'image de base, comme si elle était réfléchie dans un miroir posé par terre. Pour ce faire, vous aurez besoin de séparer la transformation 3D en deux:
     * la translation doit être appliqué au layer principal, comme maintenant; mais
     * la rotation ne doit être appliquée qu'au layer qui contient l'image.
    
    Faites attention à bien remettre les transformations à zéro sur les deux layers modifiés dans `resetButtonPressed`.
    
 2. Ajoutez un `CAGradientLayer` par-dessus la réflexion pour enfin obtenir l'effet final ci-dessous. Créez-le comme suit:

        gradientLayer = CAGradientLayer()
        gradientLayer.frame = // à vous de trouver
        gradientLayer.colors = [
            UIColor.blackColor().colorWithAlphaComponent(0.25).CGColor,
            UIColor.blackColor().CGColor,
            UIColor.blackColor().CGColor
        ]
        
    Trouvez où l'ajouter pour obtenir cet effet. Vous pouvez créer des layers intermédiaires si vous voulez. Attention, le layer principal de la UIView (`view.layer`) ne s'anime pas automatiquement.
    
    *Indice:* en plus de la propriété `transform`, un layer a une propriété `sublayerTransform`, qui est appliquée à tous ses sous-layers. Cela évite de devoir spécifier la même transformation pour une série de sous-layers.


    ![](https://raw.githubusercontent.com/jppellet/swiftcourse/master/Exercices/Séance%2006%20Img%2002.png)

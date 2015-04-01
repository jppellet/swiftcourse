# Développement iOS avec Swift — Séance d’exercices 7

## Exercice 1 — Core Motion

 1. Dupliquez le projet du dossier Code de départ pour l’exercice 1 et faites-le tourner. Il devrait afficher un rectangle bleu (un `CALayer` tout simple) sur fond noir. Le but sera d’appliquer une `transform` à ce layer pour qu’il soit toujours plus ou moins «parallèle» au sol.
 2. Utilisez l’objet `motionManager` pour démarrer des `deviceMotionUpdates` avec un intevalle de 10 ms entre chaque mise à jour. Inspirez-vous du code du cours. Si vous recevez correctement des données de type `CMDeviceMotion` dans le handler, passez-en la propriété `attitude` à une nouvelle méthode `func applyAttitude(attitude: CMAttitude)`.
 3. Dans cette méthode, utilisez la propriété `rotationMatrix` de l’objet `CMAttitude` pour créer la `CATransform3D` à appliquer au layer bleu (`rotationlayer`).<br>
    **Indices:**
     * La `rotationMatrix` est une matrice 3x3 et les `CATransform3D` sont des matrices 4x4. De plus, différentes conventions font qu’elles sont transposées l’une par rapport à l’autre lorsqu’elles représentent la même rotation. Insérez donc la matrice de rotation dans une `CATransform3D` comme ceci:

            let r = attitude.rotationMatrix
            var t = CATransform3DIdentity
            t.m11=CGFloat(r.m11);    t.m12=CGFloat(r.m21);    t.m13=CGFloat(r.m31);    t.m14=0;
            t.m21=CGFloat(r.m12);    t.m22=CGFloat(r.m22);    t.m23=CGFloat(r.m32);    t.m24=0;
            t.m31=CGFloat(r.m13);    t.m32=CGFloat(r.m23);    t.m33=CGFloat(r.m33);    t.m34=0;
            t.m41=0;                 t.m42=0;                 t.m43=0;                 t.m44=1;
       (Oui, les points-virgules sont aussi utiles en Swift, des fois!)
     * Vous aurez encore besoin d’inverser l’axe Y, soit en insérant l’inversion dans le code ci-dessus, soit en faisant un scale avec un facteur de `-1.0` pour y;
     * N’oubliez pas une translation vers l’arrière sur l’axe Z de 300 ou 400 pixels et réglez la composante (3, 4) de la matrice de transformation à quelque chose comme ceci: `t.m34 = -1.0 / 500`.
     * Attention à l’ordre des compositions/concaténation des matrices…


## Exercice 2 — Core Location
 
 1. Dupliquez le projet Core Location du cours. Le but sera de faire pivoter la carte suivant les indications de `bearing` que Core Location nous donnera.
 2. Après l’appel `startUpdatingLocation()`, insérez un appel `startUpdatingHeading()` pour demander au `locationManager` de nous informer des nouvelles directions. Trouvez quelle méthode du protocole `CLLocationManagerDelegate` il faut implémenter pour réagir à ces informations et insérez-y un `println` tout simple.
 3. Faites en sorte de tourner la carte selon les indications reçues. Pour ceci, n’utilisez pas la propriété `transform` de `mapView` (savez-vous pourquoi? Quel en serait l’effet indésirable?), mais faites ceci:
       * Demandez à la `mapView` sa «caméra» actuelle et dupliquez-la en faisant ceci:
       
            let cam = mapView.camera.copy() as MKMapCamera`
       * Réglez la propriété `heading` de la caméra selon les informations reçues de de Core Location;
       * Appliquez la nouvelle caméra avec la méthode `setCamera()` de `mapView`.
<h3 align="center">1 - Mise en place du Tool</h3>

Dans un premier temps, je vais me munir de mon Tool :

![Proxmark](https://user-images.githubusercontent.com/96829109/199741880-8fe0c656-0579-4887-8c63-8302fb22ea7e.jpg)

Le Proxmark 3 Easy, vous pouvez trouver ce petit gadget sur Aliexpress pour environ 50€. Il possède deux antennes, une basse et une haute fréquence.

Pour l'utiliser, vous allez avoir besoin d'un tool nommé Iceman.
Tutoriel Youtube pour mettre à jour le Firmware et installer iceman :
https://www.youtube.com/watch?v=n1Xt-1ZmjM0&feature=emb_imp_woyt


<h3 align="center">2 - Identifier le Badge</h3>



Pour identifier le badge nous allons utiliser la commande : "auto", qui permet de scruter les différentes fréquences (Hautes et Basses fréquences).


![img](https://user-images.githubusercontent.com/96829109/199742199-62d6ebb4-6f1b-40e4-80d9-aa3e6298bd9e.png)


Dans notre cas, on remarque que le badge est un MIFARE Classic 1k. (Informations sur le badge : http://www.stronglink-rfid.com/fr/rfid-cards/mifare-1k.html )
Ce type de badge est utilisé pour les salles de sports, pour les machines à café ...

Son identifiant est 0F 4A 83 3D. Il s'agit d'un identifiant unique permettant au lecteur d'identifier le badge avant d'accéder aux différents informations que celui-ci contient.


<h3 align="center">3 - Dump de la mémoire</h3>



Il est temps de faire un DUMP de la mémoire : " hf mf autopwn ". ( hf = high frequency, mf = mifare )


![img](https://user-images.githubusercontent.com/96829109/199742335-9200467c-0406-47b3-bf70-09a00e6972d7.png)


On dispose de trois fichiers disponibles dans le dossier "Client", un .bin, un .json et un .eml.
Ce qui nous intéresse c'est le .eml

<h3 align="center">4 - Analyse du Badge</h3>



Avec un éditeur de texte comme Notepad++, il est possible de regarder les 64 blocks.


![img](https://user-images.githubusercontent.com/96829109/199742481-48a6cdff-bb15-48ee-917d-3d57dd020d57.png)


Sur la première ligne, on peut retrouver l'identifiant du badge. Nous allons passer l'identifiant de notre futur badge à 12 34 56 78.

![Data](https://user-images.githubusercontent.com/96829109/199744337-a98939f9-f803-4c38-a3cd-889a3a87c154.png)



Ligne 23 : nous avons de la data, comme un prix, ou un accès. Il vous faudra comparer par vous même la mémoire. (Prix, Accès, ...)

Prix : comparer la mémoire entre deux utilisations de la carte.
Accès : comparer avec un autre badge de sécurité.


<h3 align="center">5 - Envoi de la mémoire modifié sur un nouveau badge</h3>




<h5>ATTENTION : Pour dupliquer ou cloner un badge, il vous faut un badge du même type ou une Magic Card.</h5>

Nous allons utiliser la commande " hf mf cload -f lenomdenotrefichiermodifier.eml"


![img](https://user-images.githubusercontent.com/96829109/199742577-c76c81d3-f48b-4a8f-91b7-11cd8f7581d1.png)


Et voilà ! Nous allons vérifier que la ré-écriture de l'IUD à bien fonctionné.


![img](https://user-images.githubusercontent.com/96829109/199742622-674fbfbf-fac6-44d4-86bc-7c51e793fe1a.png)

Nous avons une erreur, car lors de la modification du tag, je n'ai pas respecté la norme de nommage du tag MIFARE.
Retour sur notre fichier .EML pour modifier notre erreur !


![img](https://user-images.githubusercontent.com/96829109/199742780-d318b590-54e3-4555-9aba-da14fe4f9e89.png)

Le carré noir est notre UID : 12 34 56 78.

Le carré rouge est notre BCC : 0xFA.
Nous allons utiliser un calculateur MIFARE BCC pour modifier le carré rouge.

URL : https://nric.biz/mifare-bcc-calculator.html


![img](https://user-images.githubusercontent.com/96829109/199742985-0639147d-9afd-470c-a915-9010a2072be8.png)

On obtient bien 8, si on regarde la capture écran de l'erreur, le logiciel attend un BCC à 0x08.
Il ne nous reste plus qu'à modifier notre BCC.
On ré-upload à l'aide de la commande : " hf mf cload -f monfichiermodifier.eml "

Nous vérifions l'UID à l'aide de la commande HF search (High frequency search).


![img](https://user-images.githubusercontent.com/96829109/199743037-42e54f1c-ac10-4e80-8f0b-0a7ed20be575.png)

Nous avons bien modifié notre UID !

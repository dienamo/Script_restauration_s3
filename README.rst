*script de restauration
======================

script de téléchargement de données stockées sur Amazon s3

installation
------------

L'installation de la dernière version de boto3 se fait avec PIP:

pip install boto3

configuration
------------

avant de pouvoir utiliser ce module il faut d'abord paramètrer son système avec les informations 
d'authentification afin de pouvoir connecter son compte AWS avec la commande : 
"aws configure".
cette commande va nous permettre de renseigner les clés d'accès à notre compte AWS ainsi que
les paramètres régionaux.

AWS Access key ID [None]:
AWS Secret Access key [None]:
Default region name [None]:

toutes ces informations sont disponible dans les paramètres de compte aws en ligne.

utilisation
-----------

afin d'utiliser boto3 on importe le module prévu à vet effet "import boto3"
et on indique le service que nous souhaitons utiliser en affectant une variable boto3.resource('s3')

s3 = boto3.resources('s3')

la restauration se fait en téléchargeant un objet à partir de s3 comme ceci:

s3.objects(bucket,fichier.download_file(chemin,fichier)).download_file

on exécute le script en mettant en paramètre le chemin de restauration comme ceci :

.\script_restauration -c "chemin"

explication
-----------

- importation de la librairie boto3
pour l'utilisation de la librairie dans notre script

- importation des modules datetime et time
afin de capturer la date et le temps d'exècution duscript

- importation du module ArgumentParser
afin de mettre en place notre analyseur de paramètres

- construction de notre analyseur de paramètres grâce à la méthode add_argument()
afin de paramètrer les options appelées à l'exécution du script

- affactation de la variable chemin avec la méthode args
cette variable sera le paramètre à renseigner à l'exécuition du script afin d'indiquer l'endroit
ou le fichier doit être restauré

- affectation de la variable mon_bucket contenant le nom de mon bucket

- déclaration du service AWS auquel n,ous avons besoin (s3)
maintenant que nous avons déclaré une ressource nous pouvons efgfectuer des requêtes et obtenir
des réponses de ce service

- utilisation de la boucle while
afin de demander à l'utilisateur d'entrer un nom de fichier si il fait une erreur de frappe ou 
si le fichier n'esxiste pas dans le bucket

- création d'un bloc try 
afin de lever une exception botocore.exceptions.ClientError qui est une exception du module botocore
créée en cas de non présence du fichier dans notre bucket

- méthode de téléchargement d'un fichier avec les méthodes s3.Objects().download_files()
afin de restaurer un fichier en local à partir de notre bucket sur s3

- on affiche un message indiquant la fin de la restauration

- on affiche le temps de téléchargement avec la même méthode que celle indiquée dans le script de
sauvegarde

- inscription du nom du fichier de la date d'exécution, de la date de restauration et du temps de
chargement dans un fichier avec la mpéthode write()
afin d'avoir un historique de la date d'exécution et de vérifier le temps de téléchargement

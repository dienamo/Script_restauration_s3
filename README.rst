script de restauration
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

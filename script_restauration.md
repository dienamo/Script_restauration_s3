#!/usr/bin/env python3.6

# Importation du module ArgumentParser de la librairie argparse
from argparse import ArgumentParser

# Creation d'un objet pour l'analyseur de paramètres
parser = ArgumentParser()

# Ajout des informations relatives à l'appel de l'analyseur de paramètres (commande d'appel,type etc..
parser.add_argument("-c","--file",dest="chemin",help="chemin de location restauration")

# Affectation de la variable args qui represente la methode parse_args utilisée par Argumentparser
args = parser.parse_args()

# Affectation de la variable chemin qui va etre le parametre à renseigner au lancement du script
chemin = args.chemin

# Importation du module boto3
import boto3

# Importation du module botocore
import botocore

from datetime import datetime

# Affectation de la variable "mon bucket" contenant le nom de mon bucket sur s3
mon_bucket = "monprojet4"

# Affectation de la variable s3 afin d'utiliser le service s3
s3 = boto3.resource('s3')

# Capture du nom du fichier à telecharger
mon_fichier = input("Veuillez entrer le nom de votre fichier :")

# Bloc try afin de télécharger un fichier tout en verifiant sa disponibilté grâce au module botocore
try:

# Methode de telechargement d'un fichier avec en paramètre le nom et le chemin de sauvegarde
	s3.Object(mon_bucket,mon_fichier).download_file(f'{chemin}{mon_fichier}')

# Levée d'une exception grâce au service botocore en cas de non présence du fichier demandé
except botocore.exceptions.ClientError as e:
	if e.response['Error']['Code'] == "404":
		print("Fichier non existant dans le bucket")
	else:
		raise

f = open("date_execution.txt","a")

f.write(f"{mon_fichier} restauré le :{datetime.now()}\n")

f.close()

print("Fichier restauré avec succès")

#!/usr/bin/env python3.6

# Importation du module boto3
import boto3

# Importation du module botocore
import botocore

# Importation du module datetime afin d'enregistrer la date et l'heure d'exécution
from datetime import datetime
import time

# Importation du module ArgumentParser de la librairie argparse
from argparse import ArgumentParser

# On crée l'objet parser pour ajouter des options à l'analyseur de paramètres
parser = ArgumentParser()

# On ajoute les informations relatives à l'appel de l'analyseur de paramètres (commande d'appel,
# type etc..)
parser.add_argument("-c","--file",dest="chemin",help="chemin de location restauration")

# On déclare la variable args qui représente la methode parse_args utilisée par Argumentparser
args = parser.parse_args()

# On déclare la variable chemin qui va être le paramètre à renseigner au lancement du script
chemin = args.chemin

# On déclare la variable "mon_bucket" contenant le nom de mon bucket sur s3
mon_bucket = "monprojet4"

# On déclare la variable s3 afin d'utiliser le service s3 sur AWS
s3 = boto3.resource('s3')

# Capture du nom du fichier à télécharger

# On demande confirmation du nom du fichier
while True:

	mon_fichier = input("Veuillez entrer le nom du fichier :")

# Bloc try afin de télécharger un fichier tout en verifiant sa disponibilté grâce au module botocore
	try:
# On determine le moment du début de la restauration
		début = time.time()

# Methode de téléchargement d'un fichier avec en paramètre le nom et le chemin de sauvegarde
		s3.Object(mon_bucket,mon_fichier).download_file(f'{chemin}{mon_fichier}')

# On détermine le moment de la fin de la sauvegarde
		fin = time.time()
		break

# On lève une exception grâce au service botocore en cas de non présence du fichier demandé
	except botocore.exceptions.ClientError as e:
		if e.response['Error']['Code'] == "404":
			print("Fichier non existant dans le bucket")
			continue

# On affiche un message indiquant la fin de la restauration
print("-------------------------------------------")
print(f"fichier restauré avec succès dans {chemin}")
print("-------------------------------------------")

# On affiche le temps de téléchargement
print(f"Temps de téléchargement: {fin-début} secondes")
print("-------------------------------------------")

# On écrit la date et l'heure d'exécution du script dans le fichier date_exécution
f = open("date_execution.txt","a")
f.write(f"{mon_fichier} restauré le :{datetime.now()} en {fin-début} secondes\n")
f.close()

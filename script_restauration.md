#!/usr/bin/env python3.6

from argparse import ArgumentParser
parser = ArgumentParser()
parser.add_argument("-c","--file",dest="chemin",help="chemin de location restauration")
args = parser.parse_args()
chemin = args.chemin

import boto3

mon_bucket = "monprojet4"

mon_fichier = input ("Veuillez entrer le nom de votre fichier :")

s3 = boto3.resource('s3')

s3.Object(mon_bucket,mon_fichier).download_file(f'{chemin}{mon_fichier}')

print("fichier restauré avec succès")

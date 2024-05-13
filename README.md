import random
import tkinter as tk
from PIL import Image, ImageTk
import winsound

# Liste des choix
choices = ["Pierre", "Papier", "Ciseaux"]

# Fonction pour jouer au jeu
def jouer(choix_joueur):
    choix_ordi = random.choice(choices)
    resultat = ""
    if choix_joueur == choix_ordi:
        resultat = "Match nul!"
        winsound.PlaySound("match_nul.wav", winsound.SND_ASYNC)  # Son pour match nul
    elif (choix_joueur == "Pierre" and choix_ordi == "Ciseaux") or \
         (choix_joueur == "Papier" and choix_ordi == "Pierre") or \
         (choix_joueur == "Ciseaux" and choix_ordi == "Papier"):
        resultat = "Vous avez gagné!"
        winsound.PlaySound("victoire.wav", winsound.SND_ASYNC)  # Son pour victoire
    else:
        resultat = "L'ordinateur a gagné!"
        winsound.PlaySound("defaite.wav", winsound.SND_ASYNC)  # Son pour défaite
    label_resultat.config(text=resultat, fg="blue" if resultat == "Vous avez gagné!" else "red")
    afficher_choix(choix_joueur, choix_ordi)

# Fonction pour afficher les choix avec des images
def afficher_choix(choix_joueur, choix_ordi):
    img_joueur = Image.open(f"{choix_joueur.lower()}.png")
    img_ordi = Image.open(f"{choix_ordi.lower()}.png")
    
    img_joueur = img_joueur.resize((100, 100), Image.BICUBIC)
    img_ordi = img_ordi.resize((100, 100), Image.BICUBIC)
    
    img_joueur = ImageTk.PhotoImage(img_joueur)
    img_ordi = ImageTk.PhotoImage(img_ordi)
    
    label_image_joueur.config(image=img_joueur)
    label_image_ordi.config(image=img_ordi)
    label_image_joueur.image = img_joueur
    label_image_ordi.image = img_ordi

# Création de l'interface graphique
root = tk.Tk()
root.title("Pierre-Papier-Ciseaux")
root.geometry("500x300")

# Ajout des images des mains
label_image_joueur = tk.Label(root)
label_image_joueur.pack(side=tk.LEFT, padx=10)

label_resultat = tk.Label(root, text="", font=("Helvetica", 16))
label_resultat.pack(pady=10)

label_image_ordi = tk.Label(root)
label_image_ordi.pack(side=tk.RIGHT, padx=10)

# Boutons pour les choix
frame_choix = tk.Frame(root)
frame_choix.pack()

for choix in choices:
    button = tk.Button(frame_choix, text=choix, width=10, font=("Helvetica", 12), bg="lightblue", fg="black",
                       activebackground="blue", activeforeground="white",
                       command=lambda c=choix: jouer(c))
    button.pack(side=tk.LEFT, padx=5)

root.mainloop()




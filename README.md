# balle

import Tkinter as Tk
from random import randrange
###############################################################################
## Définition locale de fonctions:
def move():
	global x, y, dx, dy, flag, liste
	xp, yp = x, y		# mémorisation des coord. précédentes.
	# déplacement de la balle:
	x = x + dx
	y = y + dy
	# test si la balle touche le mur et inverse aléat. son déplacement
	if x>385:						# mur droit.
		dx = -dx					# on inverse le déplacement.
		dy = angle[randrange(8)]	# on change l'"angle" de déplacement.
	if y>385:						# mur du dessous.
		dy = -dy
		dx = angle[randrange(8)]
	if x<15:						# mur gauche.
		dx = -dx
		dy = angle[randrange(8)]
	if y<15:						# mur du haut.
		dy = -dy
		dx = angle[randrange(8)]
	if (x>385) and (y>385):		# coin en bas à droite.
		dx, dy = -20, -10			# on change l'"angle" de déplacement.
	if (x>385) and (y<15):			# coin en haut à gauche.
		dx, dy = -20, 10
	if (x<15) and (y>385):			# coin en bas à gauche.
		dx, dy = 20, -10
	if (x<15) and (y<15):			# coin en haut à gauche.
		dx, dy = 20, 10
	# on repositionne la balle:
	can.coords(balle, x-10, y-10, x+10, y+10)
	# on trace un bout de trajectoire:
	can.create_line(xp, yp, x, y, fill="light grey")
	# ... et on remet ça jusqu'à plus soif :
	if flag > 0:
		fen.after(50,move)
		
def start():
	" Démarre l'animation."
	global flag
	flag = flag +1
	if flag == 1:
		move()
		
def stop():
	" Stop l'animation."
	global flag
	flag = 0
###############################################################################
# Corps principal du programme:
# initialisation des coordonnées, de la vitesses et du témoin d'animation :
x, y, dx, dy, flag = 15, 15, 6, 5, 0
angle = [-5,-10,-15,20,5,10,15,20]		# sert à inverser le déplacement.

fen = Tk()
fen.title("balle qui rebondit")

can = canvas(fen, width=400, height=400, bg="white")
can.pack()

balle = can.create_oval(x-10, y-10, x+10, y+10, fill="red")

Button(fen, text="start", command=start).pack(side=LEFT, padx=10)
Button(fen, text="stop", command=stop).pack(side=RIGHT, padx=10)
Button(fen, text='Quitter', command =fen.quit).pack(side =RIGHT, padx =10)

fen.mainloop()

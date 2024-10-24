import pygame
import sys

# Inizializza pygame
pygame.init()

# Definizione dei colori
BIANCO = (255, 255, 255)
ROSSO = (255, 0, 0)
VERDE = (0, 255, 0)
NERO = (0, 0, 0)
AZZURRO = (0, 0, 255)
GRIGIO = (169, 169, 169)
MARRONE = (139, 69, 19)

# Dimensioni della finestra di gioco
LARGHEZZA = 800
ALTEZZA = 600

# Creazione della finestra di gioco
schermo = pygame.display.set_mode((LARGHEZZA, ALTEZZA))
pygame.display.set_caption("Escape dall'Appartamento in Fiamme")

# Definizione del giocatore
giocatore_dim = 50
giocatore_x = 100
giocatore_y = 500
velocita_giocatore = 5

giocatore = pygame.Rect(giocatore_x, giocatore_y, giocatore_dim, giocatore_dim)

# Definizione delle fiamme
fiamme = [
    pygame.Rect(300, 450, 100, 20),
    pygame.Rect(500, 350, 150, 20),
    pygame.Rect(200, 250, 120, 20)
]

# Definizione del percorso di sicurezza (checkpoint)
checkpoint = [
    pygame.Rect(150, 500, 50, 50),
    pygame.Rect(400, 400, 50, 50),
    pygame.Rect(600, 300, 50, 50)
]

# Definizione dell'uscita
uscita = pygame.Rect(700, 50, 80, 80)

# Definizione degli arredi dell'ufficio
scrivanie = [
    pygame.Rect(100, 100, 150, 50),
    pygame.Rect(400, 100, 150, 50),
    pygame.Rect(600, 100, 150, 50)
]
sedie = [
    pygame.Rect(125, 170, 50, 50),
    pygame.Rect(425, 170, 50, 50),
    pygame.Rect(625, 170, 50, 50)
]
scaffali = [
    pygame.Rect(50, 400, 50, 150),
    pygame.Rect(700, 400, 50, 150)
]
corridoio = pygame.Rect(0, 300, LARGHEZZA, 100)
bagno1 = pygame.Rect(50, 50, 100, 100)
bagno2 = pygame.Rect(650, 50, 100, 100)

# Ciclo principale del gioco
running = True
while running:
    # Gestione degli eventi
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    # Gestione degli input da tastiera
    tasti = pygame.key.get_pressed()
    if tasti[pygame.K_LEFT]:
        giocatore.x -= velocita_giocatore
    if tasti[pygame.K_RIGHT]:
        giocatore.x += velocita_giocatore
    if tasti[pygame.K_UP]:
        giocatore.y -= velocita_giocatore
    if tasti[pygame.K_DOWN]:
        giocatore.y += velocita_giocatore

    # Impedisce al giocatore di uscire dai bordi dello schermo
    if giocatore.left < 0:
        giocatore.left = 0
    if giocatore.right > LARGHEZZA:
        giocatore.right = LARGHEZZA
    if giocatore.top < 0:
        giocatore.top = 0
    if giocatore.bottom > ALTEZZA:
        giocatore.bottom = ALTEZZA

    # Controllo delle collisioni con le fiamme
    for fiamma in fiamme:
        if giocatore.colliderect(fiamma):
            print("Hai preso fuoco! Riprova!")
            giocatore.x = 100
            giocatore.y = 500

    # Controllo delle collisioni con i checkpoint
    for punto in checkpoint:
        if giocatore.colliderect(punto):
            print("Checkpoint raggiunto!")
            checkpoint.remove(punto)

    # Controllo se il giocatore raggiunge l'uscita
    if giocatore.colliderect(uscita):
        print("Complimenti! Sei uscito dall'appartamento in fiamme!")
        running = False

    # Disegna gli elementi sullo schermo
    schermo.fill(NERO)
    pygame.draw.rect(schermo, VERDE, giocatore)
    pygame.draw.rect(schermo, BIANCO, uscita)
    for fiamma in fiamme:
        pygame.draw.rect(schermo, ROSSO, fiamma)
    for punto in checkpoint:
        pygame.draw.rect(schermo, AZZURRO, punto)
    for scrivania in scrivanie:
        pygame.draw.rect(schermo, MARRONE, scrivania)
    for sedia in sedie:
        pygame.draw.rect(schermo, GRIGIO, sedia)
    for scaffale in scaffali:
        pygame.draw.rect(schermo, GRIGIO, scaffale)
    pygame.draw.rect(schermo, BIANCO, corridoio)
    pygame.draw.rect(schermo, AZZURRO, bagno1)
    pygame.draw.rect(schermo, AZZURRO, bagno2)

    # Aggiorna lo schermo
    pygame.display.flip()

    # Imposta il frame rate
    pygame.time.Clock().tick(30)

# Chiusura del gioco
pygame.quit()

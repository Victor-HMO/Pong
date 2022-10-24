# Pong.py

from PPlay.window import *
from PPlay.sprite import *
from PPlay.collision import *
from random import randint

# Janela do jogo
janela = Window(1500, 800)
janela.set_title("Pong")

# Bola
bola = Sprite("bola.png", 1)
bola.x = (janela.width / 2) - (bola.width / 2)
bola.y = (janela.height / 2) - (bola.height / 2)

# Bola 2
bola2 = Sprite("bola.png", 1)
bola2.x = (janela.width / 2) - (bola2.width / 2)
bola2.y = (janela.height / 2) - (bola2.height / 2)

# Velocidade da Bola
velDaBola = [200, -200]
velX = velDaBola[randint(0, 1)]
velY = velDaBola[randint(0, 1)]

# Velocidade da Bola 2
velX2 = velDaBola[randint(0, 1)]
velY2 = velDaBola[randint(0, 1)]

# Pad da Esquerda
padL = Sprite("pad.png", 1)
padL.x = 0
padL.y = (janela.height / 2) - (padL.height / 2)

# Pad da Direita
padR = Sprite("pad.png", 1)
padR.x = (janela.width) - (padR.width)
padR.y = (janela.height / 2) - (padR.height / 2)

# Velocidade pads
velPadL = 300
velPadR = 150

# Batida da bola com o pad
batida = 0

# Teclado
teclado = Window.get_keyboard()

# Fonte
fonte = pygame.font.SysFont("Arial", 40, True, False)

# Pontos Iniciais
ponto1=0
ponto2=0

# Ativaçao da Bola 1 e 2
ativ1 = 1
ativ2 = 0

# GAME LOOP
while (True):
    # Pintar a Tela
    janela.set_background_color((30, 10, 250))

    # Texto da Tela
    mensagem = f"Player1 - {ponto1} x {ponto2} - Player2"
    agrupamento = fonte.render(mensagem, False, (255, 255, 255))
    ret_texto = agrupamento.get_rect()
    ret_texto.center = (janela.width // 2, janela.height // 2)
    janela.draw_text(mensagem, 550, 15, 40, (255, 255, 255), "Arial", True, False)


    # Inputs do Teclado para o Pad da Esquerda
    if (teclado.key_pressed("W") and padL.y > 0):  # P/ CIMA
        padL.move_y(-velPadL * janela.delta_time())
    if (teclado.key_pressed("S") and padL.y < (janela.height - padL.height)):  # P/ BAIXO
        padL.move_y(velPadL * janela.delta_time())

    # IA para o Pad da Direita
    if ((bola.y <= padR.y + padR.height) and bola.x > (janela.width / 2) and velY < 0 and padR.y > 0):  # P/ CIMA
        padR.move_y(-(velPadR) * janela.delta_time())
    elif ((bola.y + bola.height >= padR.y) and bola.x > (janela.width / 2) and velY > 0 and padR.y < (janela.height - padR.height)):  # P/ BAIXO
        padR.move_y((velPadR) * janela.delta_time())
    elif ((bola2.y <= padR.y + padR.height) and bola2.x > (janela.width / 2) and velY < 0 and padR.y > 0):  # P/ CIMA
        padR.move_y(-(velPadR) * janela.delta_time())
    elif ((bola2.y + bola2.height >= padR.y) and bola2.x > (janela.width / 2) and velY > 0 and padR.y < (janela.height - padR.height)):  # P/ BAIXO
        padR.move_y((velPadR) * janela.delta_time())



    # Colisão da bola 1 com o pad
    if (bola.collided(padL)) or (bola.collided(padR)):
        if bola.x > padR.x - bola.width:
            bola.x -= 5
        elif bola.x < padL.x + padL.width:
            bola.x += 5
        velX = -(velX)
        batida += 1


    # Colisão da bola 2 com o pad
    if (bola2.collided(padL)) or (bola2.collided(padR)):
        if bola2.x > padR.x - bola2.width:
            bola2.x -= 5
        elif bola2.x < padL.x + padL.width:
            bola2.x += 5
        velX2 = -(velX2)


    # Desativa a Bola 1 e Soma o Ponto
    if bola.x <= 0:
        bola.x = (janela.width / 2) - (bola.width / 2)
        bola.y = (janela.height / 2) - (bola.height / 2)
        ponto2 += 1
        ativ1 = 0
    elif bola.x >= janela.width - bola.width:
        bola.x = (janela.width / 2) - (bola.width / 2)
        bola.y = (janela.height / 2) - (bola.height / 2)
        ponto1 += 1
        ativ1 = 0

    # Desativa a Bola 2 e Soma o Ponto
    if bola2.x <= 0 :
        bola2.x = (janela.width / 2) - (bola2.width / 2)
        bola2.y = (janela.height / 2) - (bola2.height / 2)
        ponto2 += 1
        ativ2=0
    elif bola2.x >= janela.width - bola2.width:
        bola2.x = (janela.width / 2) - (bola2.width / 2)
        bola2.y = (janela.height / 2) - (bola2.height / 2)
        ponto1 += 1
        ativ2=0

    # Quando as Duas Desativarem Ativa a Bola 1 Novamente
    if ativ1 == 0 and ativ2 == 0:
        velX = velDaBola[randint(0, 1)]
        velY = velDaBola[randint(0, 1)]
        batida = 0
        ativ1=1

    # Colisão com a parede
    if (bola.y >= (janela.height - bola.height)) or (bola.y <= 0):
        velY = -(velY)
    if (bola2.y >= (janela.height - bola2.height)) or (bola2.y <= 0):
        velY2 = -(velY2)

    # Resolver o bug da patinação bola
    if bola.y > janela.height - bola.height:
        bola.y -= 10
    elif bola.y < 0:
        bola.y += 10

    # Resolver o bug da patinação bola 2
    if bola2.y > janela.height - bola2.height:
        bola2.y -= 10
    elif bola2.y < 0:
        bola2.y += 10

    # Adicionar uma segunda bola
    if (batida == 3):
        bola2.x = (janela.width / 2) - (bola2.width / 2)
        bola2.y = (janela.height / 2) - (bola2.height / 2)
        batida += 1
        ativ2 = 1
    # Desenha Bola 1 se estiver ativa
    if ativ1!=0:
        bola.x += velX * janela.delta_time()
        bola.y += velY * janela.delta_time()
        bola.draw()

    # Desenha Bola 2 se estiver ativa
    if batida >= 3 and ativ2!=0:
        # Velocidade da Bola 2
        bola2.x += velX * janela.delta_time() * -1
        bola2.y += velY * janela.delta_time() * -1
        bola2.draw()

    # Desenha os Pads
    padL.draw()
    padR.draw()

    # Atualiza a tela
    janela.update()

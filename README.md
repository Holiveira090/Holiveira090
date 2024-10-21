# ✨ **Olá! Eu sou Henrique Oliveira!** ✨
-  🎂 19 anos;
-  💻 Estudante de Análise e desenvolvimento de sistemas na faculdade Estácio;
-  📚 Buscando aprender cada vez mais;
-  🔭 Atualmente sou um Jovem aprendiz de TI na Digix

![Descrição do GIF](https://i.pinimg.com/originals/fe/c6/e0/fec6e07f6d8d4a951777fe9c5f421e07.gif)

import pygame
import time
import random

pygame.init()

# Cores
branco = (255, 255, 255)
preto = (0, 0, 0)
vermelho = (213, 50, 80)
verde = (0, 255, 0)
azul = (50, 153, 213)

# Tamanho da tela
largura = 600
altura = 400
tela = pygame.display.set_mode((largura, altura))
pygame.display.set_caption('Jogo da Cobrinha')

relogio = pygame.time.Clock()

tamanho_cobra = 10
velocidade_cobra = 15

def nossa_cobra(tamanho_cobra, lista_cobra):
    for x in lista_cobra:
        pygame.draw.rect(tela, preto, [x[0], x[1], tamanho_cobra, tamanho_cobra])

def mensagem(msg, cor):
    fonte = pygame.font.SysFont('comicsansms', 35)
    texto = fonte.render(msg, True, cor)
    tela.blit(texto, [largura / 6, altura / 3])

def jogo():
    game_over = False
    game_close = False

    x1 = largura / 2
    y1 = altura / 2

    x1_mudou = 0
    y1_mudou = 0

    lista_cobra = []
    comprimento_cobra = 1

    comida_x = round(random.randrange(0, largura - tamanho_cobra) / 10.0) * 10.0
    comida_y = round(random.randrange(0, altura - tamanho_cobra) / 10.0) * 10.0

    while not game_over:

        while game_close == True:
            tela.fill(azul)
            mensagem("Você perdeu! Pressione C para jogar novamente ou Q para sair", vermelho)
            pygame.display.update()

            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_c:
                        jogo()

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x1_mudou = -tamanho_cobra
                    y1_mudou = 0
                elif event.key == pygame.K_RIGHT:
                    x1_mudou = tamanho_cobra
                    y1_mudou = 0
                elif event.key == pygame.K_UP:
                    y1_mudou = -tamanho_cobra
                    x1_mudou = 0
                elif event.key == pygame.K_DOWN:
                    y1_mudou = tamanho_cobra
                    x1_mudou = 0

        if x1 >= largura or x1 < 0 or y1 >= altura or y1 < 0:
            game_close = True

        x1 += x1_mudou
        y1 += y1_mudou
        tela.fill(azul)
        pygame.draw.rect(tela, verde, [comida_x, comida_y, tamanho_cobra, tamanho_cobra])
        cabeca_cobra = []
        cabeca_cobra.append(x1)
        cabeca_cobra.append(y1)
        lista_cobra.append(cabeca_cobra)
        if len(lista_cobra) > comprimento_cobra:
            del lista_cobra[0]

        for x in lista_cobra[:-1]:
            if x == cabeca_cobra:
                game_close = True

        nossa_cobra(tamanho_cobra, lista_cobra)

        pygame.display.update()

        if x1 == comida_x and y1 == comida_y:
            comida_x = round(random.randrange(0, largura - tamanho_cobra) / 10.0) * 10.0
            comida_y = round(random.randrange(0, altura - tamanho_cobra) / 10.0) * 10.0
            comprimento_cobra += 1

        relogio.tick(velocidade_cobra)

    pygame.quit()
    quit()

jogo()

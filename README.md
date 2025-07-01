Documentação do Projeto: Space Invaders Simplificado

1. Visão Geral do Projeto
   
Este é um jogo de tiro espacial estilo "Space Invaders" desenvolvido em C, utilizando a biblioteca Allegro 5 para gráficos, áudio e entrada. O objetivo do jogador é destruir ondas de inimigos enquanto evita seus tiros e colisões. O jogo possui múltiplos rounds, com a dificuldade aumentando progressivamente.

Funcionalidades Principais:
Movimentação e tiro do jogador.

Inimigos que se movem em formação e atiram.

Colisão de balas e inimigos.

Sistema de pontuação e recorde (High Score).

Múltiplas rodadas com aumento de dificuldade.

Estados de jogo (Menu, Jogando, Pausado, Transição de Rodada, Game Over).

Efeitos sonoros e música de fundo.

Animações para jogador, inimigos e explosões.

2. Tecnologias Utilizadas
   
Linguagem de Programação: C

Biblioteca Gráfica e Multimídia: Allegro 5

allegro5/allegro.h: Funções básicas do Allegro.

allegro5/allegro_image.h: Carregamento de imagens (BMP, PNG, etc.).

allegro5/allegro_primitives.h: Desenho de formas geométricas (linhas, retângulos).

allegro5/keyboard.h: Entrada via teclado.

allegro5/keycodes.h: Definições de códigos de teclas.

allegro5/allegro_audio.h: Funcionalidades de áudio.

allegro5/allegro_acodec.h: Suporte a codecs de áudio (OGG, etc.).

allegro5/allegro_font.h: Carregamento e desenho de fontes.

allegro5/allegro_ttf.h: Suporte a fontes TrueType (TTF).

3. Estrutura do Código
   
O projeto é contido em um único arquivo main.c, que gerencia todos os aspectos do jogo, desde a inicialização do Allegro até a lógica de jogo, desenho e estados.

Variáveis Globais: Definidas no início do main.c, controlam o estado geral do jogo, recursos gráficos e sonoros, pontuação e configurações.

Estruturas de Dados (typedef struct):

Shot: Representa as balas disparadas pelo jogador.

Enemy: Representa um inimigo individual, incluindo seu tipo, estado (vivo/explodindo) e animação.

EnemyShot: Representa as balas disparadas pelos inimigos.

Funções Principais: Cada função é responsável por uma parte específica da lógica do jogo ou da renderização.

4. Componentes Principais (Funções e Lógica)
   
4.1. Gerenciamento de Assets
load_assets(): Carrega todas as imagens (sprites, backgrounds, UI), fontes e arquivos de áudio do jogo. Retorna true em caso de sucesso, false caso contrário.

destroy_assets(): Libera a memória de todos os assets carregados, essencial para evitar vazamentos de memória ao finalizar o programa.

4.2. Gerenciamento de Pontuação
load_high_score(): Carrega o recorde salvo do arquivo highscore.txt.

save_high_score(): Salva o recorde atual no arquivo highscore.txt.

check_and_save_high_score(): Compara a pontuação atual com o recorde e atualiza-o se a pontuação atual for maior.

4.3. Gerenciamento de Estados de Jogo
O jogo opera através de uma máquina de estados (GameState enum):

MENU: Tela inicial do jogo.

PLAYING: Onde a jogabilidade principal ocorre.

PAUSED: Jogo em pausa, permitindo retomar ou voltar ao menu.

ROUND_TRANSITION: Tela de transição entre rodadas.

GAME_OVER: Tela exibida quando o jogador perde.

Cada estado possui uma função run_ correspondente (ex: run_menu(), run_game()) que contém a lógica e o loop de eventos para aquele estado específico.

4.4. Jogador
g_player_x, g_player_y: Posição do jogador na tela.

g_player_frames[], g_player_left_frames[], g_player_right_frames[]: Arrays de bitmaps para as animações do jogador (parado, movendo para esquerda, movendo para direita).

g_player_knockback_frames[], g_player_knockback_left_frames[], g_player_knockback_right_frames[]: Arrays de bitmaps para as animações de "knockback" (recuo ao atirar), também para as três direções.

Lógica de Tiro: O jogador pode disparar um tiro por vez. O shoot_cooldown e shoot_timer controlam a cadência de tiro.

Lógica de "Knockback": Ao atirar, uma animação de recuo é ativada por um curto período (knockback_duration), independentemente do movimento lateral, proporcionando um feedback visual da ação.

4.5. Balas do Jogador (Shot)
add_shot(): Adiciona uma nova bala à lista encadeada g_shot_list.

update_shots(): Move as balas para cima e lida com a remoção de balas que saem da tela.

draw_shots(): Desenha as balas na tela, usando frames diferentes para a fase inicial do tiro e a fase estabilizada.

free_shots(): Libera a memória de todas as balas na lista.

4.6. Inimigos (Enemy)
create_enemy_group(): Popula a lista encadeada g_enemy_list com uma nova formação de inimigos no início de cada rodada ou jogo.

update_enemies(): Gerencia o movimento horizontal dos inimigos (mudando de direção ao atingir as bordas) e o movimento vertical (descendo um passo quando uma borda é atingida). Também atualiza o frame de animação dos inimigos. A velocidade de animação dos inimigos é ajustada para acompanhar a velocidade de movimento.

draw_enemies(): Desenha os inimigos na tela, incluindo suas animações normais e de explosão.

count_alive_enemies(): Retorna o número de inimigos vivos (não explodindo nem mortos).

Aumento de Dificuldade: A velocidade de movimento (g_enemy_dx) e o intervalo de passo (g_enemy_step_interval) dos inimigos são ajustados dinamicamente com base na porcentagem de inimigos vivos e no g_enemy_speed_multiplier da rodada.

4.7. Balas dos Inimigos (EnemyShot)
add_enemy_shot(): Adiciona uma nova bala inimiga à lista encadeada g_enemy_shots. Um inimigo na linha de frente é escolhido aleatoriamente para atirar.

update_enemy_shots(): Move as balas inimigas para baixo e as remove quando saem da tela. A velocidade dos tiros inimigos também escala com a porcentagem de inimigos vivos.

draw_enemy_shots(): Desenha as balas inimigas.

4.8. Detecção de Colisões
check_bullet_collision(): Verifica a colisão entre as balas do jogador (g_shot_list) e os inimigos (g_enemy_list). Ao colidir, o inimigo entra em estado de explosão e a pontuação é atualizada.

check_enemy_shot_collision_with_player(): Verifica a colisão entre as balas inimigas (g_enemy_shots) e o jogador. Se houver colisão, o jogador entra em estado de explosão.

check_game_over_conditions(): Verifica se os inimigos atingiram a linha de "Game Over" ou colidiram diretamente com o jogador.

4.9. Áudio
g_musica1, g_musica2, g_musica2_1, g_musica2_2: Músicas de fundo para diferentes estados ou fases do jogo.

g_sfx_shot, g_sfx_button, g_sfx_boom: Efeitos sonoros para tiro do jogador, cliques de botão e explosões.

play_game_music(): Gerencia a transição suave entre diferentes faixas de música de fundo durante o jogo.

4.10. Loop Principal e Eventos
A função main() inicializa a Allegro e seus addons, cria o display, fila de eventos e timer.

O loop principal na main() utiliza um switch para alternar entre os diferentes GameStates, chamando a função run_ apropriada para o estado atual.

Eventos do Allegro (teclado, display, timer) são processados dentro de cada run_ função.

5. Instalação e Execução


Pré-requisitos:
Um compilador C (mingw 4.7.0).
A biblioteca Allegro 5 (5.0.10).
Certifique-se de que o caminho ALLEGRO_PATH no Makefile esteja configurado corretamente para o diretório de instalação do Allegro.

Como Compilar:
Este projeto inclui um Makefile para facilitar a compilação.

Verifique o ALLEGRO_PATH no Makefile: Abra o Makefile e certifique-se de que a linha ALLEGRO_PATH aponta para o diretório raiz da sua instalação do Allegro 5. No seu caso, o Makefile mostra:

Makefile

ALLEGRO_PATH = C:/Users/jvral/Downloads/KIT_DEV_ALLEGRO/allegro-5.0.10-mingw-4.7.0/allegro-5.0.10-mingw-4.7.0
Ajuste este caminho se sua instalação do Allegro estiver em um local diferente.

Compile o projeto: Abra um terminal ou prompt de comando no diretório onde o Makefile e o main.c estão localizados e execute:

make
Este comando utilizará as configurações definidas no Makefile para compilar o main.c e criar o executável jogo.exe.

Como Executar:
Após a compilação bem-sucedida, execute o binário gerado:

jogo.exe

Importante: Os arquivos de assets (imagens, sons, fontes) listados na função load_assets() (.png, .ogg, .ttf) devem estar na mesma pasta do executável do jogo para que ele funcione corretamente.

Comandos Adicionais do Makefile:
Limpar arquivos compilados: Para remover os arquivos .o e o executável jogo.exe, execute:
make clean

6. Assets do Jogo
Os assets são essenciais para o funcionamento do jogo. Eles incluem:

Imagens (.png):

Fundo: stars_background.png

Jogador: spaceship1.png a spaceship12.png (para animações de movimento normal, esquerda, direita) e knockback1.png a knockback_right4.png (para animações de knockback).

Balas do jogador: bullet1.png a bullet4.png.

Bala inimiga: enemy_bullet.png.

Inimigos: enemy_purple1.png, enemy_purple2.png, enemy_gray1.png, enemy_gray2.png, enemy_green1.png, enemy_green2.png.

Explosões de inimigos: enemy1_explosion1.png a enemypurple_explosion6.png.

Explosão do jogador: spaceship_explosion1.png a spaceship_explosion6.png.

Elementos de UI: interface_Move.png, interface_SPACE.png, logo.png.

Áudios (.ogg):

Música: SoundTrack1.ogg, SoundTrack2.ogg, SoundTrack2.1.ogg, SoundTrack2.2.ogg.

Efeitos Sonoros: sfx_button.ogg, shotsfx.ogg, boomsfx.ogg.

Fontes (.ttf):

PressStart2P-Regular.ttf

7. Considerações de Design / Gameplay
Progressão de Dificuldade: A dificuldade aumenta com cada rodada, manifestada pela velocidade dos inimigos e de seus tiros.

Sistema de Pontuação: A pontuação é baseada no tipo de inimigo destruído, com inimigos superiores valendo mais pontos.

Linha de Game Over: Uma linha de limite visível no canto inferior da tela (ENEMY_GAME_OVER_LINE_Y) indica quando os inimigos se aproximaram demais, resultando em Game Over.

Recurso de Knockback: O "knockback" visual ao atirar é um detalhe estético para adicionar peso à ação de disparo do jogador.

8. Melhorias Futuras / To-Do

Poderes (Power-ups): Adicionar power-ups que caem dos inimigos (ex: tiro duplo, escudo, aumento de velocidade).

Diferentes Tipos de Inimigos: Variar o comportamento de tiro e movimento dos inimigos.

Chefes (Boss Battles): Inimigos maiores e mais desafiadores em rodadas específicas.

Múltiplas Vidas para o Jogador: Em vez de Game Over instantâneo, permitir que o jogador receba vários hits.

Tela de Título Melhorada: Mais opções no menu, como controles, créditos.

Modularização do Código: Dividir o main.c em múltiplos arquivos .h e .c para melhor organização e manutenção (ex: player.c, enemy.c, game_state.c).

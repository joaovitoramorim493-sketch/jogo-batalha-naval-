#include <stdio.h>
#include <stdlib.h>

#define TAMANHO 10

// Função para inicializar o tabuleiro com água (0)
void inicializarTabuleiro(int tabuleiro[TAMANHO][TAMANHO]) {
    for (int i = 0; i < TAMANHO; i++) {
        for (int j = 0; j < TAMANHO; j++) {
            tabuleiro[i][j] = 0; // 0 = água
        }
    }
}

// Função para exibir o tabuleiro (0=água, 3=navio, 5=atingido)
void exibirTabuleiro(int tabuleiro[TAMANHO][TAMANHO]) {
    printf("\nTabuleiro Batalha Naval:\n");
    printf("   ");
    for (int j = 0; j < TAMANHO; j++) {
        printf("%2d ", j);
    }
    printf("\n");
    
    for (int i = 0; i < TAMANHO; i++) {
        printf("%2d ", i);
        for (int j = 0; j < TAMANHO; j++) {
            if (tabuleiro[i][j] == 0) {
                printf(" 0 "); // água
            } else if (tabuleiro[i][j] == 3) {
                printf(" 3 "); // navio
            } else if (tabuleiro[i][j] == 5) {
                printf(" 5 "); // atingido
            }
        }
        printf("\n");
    }
}

// Posiciona alguns navios manualmente (para teste)
void posicionarNavios(int tabuleiro[TAMANHO][TAMANHO]) {
    // Navio horizontal
    tabuleiro[2][2] = 3;
    tabuleiro[2][3] = 3;
    tabuleiro[2][4] = 3;
    
    // Navio vertical
    tabuleiro[5][5] = 3;
    tabuleiro[6][5] = 3;
    tabuleiro[7][5] = 3;
    
    // Navio diagonal (simulado)
    tabuleiro[0][0] = 3;
    tabuleiro[1][1] = 3;
    tabuleiro[2][2] = 3; // já existe, mas tudo bem
}

// Habilidade em formato de CRUZ (linha e coluna)
void habilidadeCruz(int tabuleiro[TAMANHO][TAMANHO], int linha, int coluna) {
    printf("\nAplicando habilidade CRUZ no centro (%d,%d):\n", linha, coluna);
    for (int i = 0; i < TAMANHO; i++) {
        for (int j = 0; j < TAMANHO; j++) {
            if (i == linha || j == coluna) {
                if (tabuleiro[i][j] == 3) {
                    tabuleiro[i][j] = 5; // navio atingido
                } else if (tabuleiro[i][j] == 0) {
                    tabuleiro[i][j] = 5; // água também marca (opcional)
                }
            }
        }
    }
}

// Habilidade em formato de OCTAEDRO (losango)
void habilidadeOctaedro(int tabuleiro[TAMANHO][TAMANHO], int centroX, int centroY) {
    printf("\nAplicando habilidade OCTAEDRO no centro (%d,%d):\n", centroX, centroY);
    for (int i = 0; i < TAMANHO; i++) {
        for (int j = 0; j < TAMANHO; j++) {
            int distancia = abs(i - centroX) + abs(j - centroY);
            if (distancia <= 2) { // forma losango
                if (tabuleiro[i][j] == 3) {
                    tabuleiro[i][j] = 5;
                } else if (tabuleiro[i][j] == 0) {
                    tabuleiro[i][j] = 5;
                }
            }
        }
    }
}

// Habilidade em formato de CONE (triângulo para baixo)
void habilidadeCone(int tabuleiro[TAMANHO][TAMANHO], int pontaX, int pontaY) {
    printf("\nAplicando habilidade CONE com ponta em (%d,%d):\n", pontaX, pontaY);
    for (int i = 0; i < TAMANHO; i++) {
        for (int j = 0; j < TAMANHO; j++) {
            if (i >= pontaX && j >= pontaY - (i - pontaX) && j <= pontaY + (i - pontaX)) {
                if (tabuleiro[i][j] == 3) {
                    tabuleiro[i][j] = 5;
                } else if (tabuleiro[i][j] == 0) {
                    tabuleiro[i][j] = 5;
                }
            }
        }
    }
}

int main() {
    int tabuleiro[TAMANHO][TAMANHO];
    
    inicializarTabuleiro(tabuleiro);
    posicionarNavios(tabuleiro);
    
    printf("=== BATALHA NAVAL - HABILIDADES ESPECIAIS ===\n");
    exibirTabuleiro(tabuleiro);
    
    // Aplicando habilidades
    habilidadeCruz(tabuleiro, 5, 5);   // Cruz na linha 5, coluna 5
    exibirTabuleiro(tabuleiro);
    
    habilidadeOctaedro(tabuleiro, 2, 3); // Losango
    exibirTabuleiro(tabuleiro);
    
    habilidadeCone(tabuleiro, 3, 4);   // Cone com ponta em (3,4)
    exibirTabuleiro(tabuleiro);
    
    return 0;
}

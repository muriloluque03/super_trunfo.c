# super_trunfo.c
Jogo de Cartas Super Trunfo Países.
#include <stdio.h>
#include <string.h>

// Definição da struct para representar uma carta
struct Carta {
    char estado;              
    char codigo[4];           
    char nomePais[50];        
    int populacao;            
    float area;               
    float pib;                
    int pontosTuristicos;     
};

// Função para exibir uma carta em formato bonitinho
void mostrarCarta(struct Carta c) {
    printf("\n========================\n");
    printf(" Código: %s\n", c.codigo);
    printf(" País: %s\n", c.nomePais);
    printf(" População: %d\n", c.populacao);
    printf(" Área: %.2f km²\n", c.area);
    printf(" PIB: %.2f bilhões\n", c.pib);
    printf(" Pontos Turísticos: %d\n", c.pontosTuristicos);
    printf("========================\n");
}

int main() {
    // Cadastro inicial (baralho reduzido)
    struct Carta baralho[5] = {
        {'A', "A01", "Brasil",          214000000, 8515767.0, 18470.0, 50},
        {'B', "B02", "Estados Unidos",  331000000, 9833520.0, 25000.0, 80},
        {'C', "C03", "Japão",           125800000, 377975.0, 5050.0, 40},
        {'D', "D04", "Alemanha",         83000000, 357022.0, 4200.0, 35},
        {'E', "E05", "França",           67000000, 551695.0, 3800.0, 45}
    };

    // Distribuição inicial
    int qtd1 = 3, qtd2 = 2;
    struct Carta jogador1[10], jogador2[10];
    jogador1[0] = baralho[0];
    jogador1[1] = baralho[2];
    jogador1[2] = baralho[4];
    jogador2[0] = baralho[1];
    jogador2[1] = baralho[3];

    int rodada = 1;
    int vez = 1; // 1 = jogador1 escolhe atributo, 2 = jogador2 escolhe
    int placar1 = 0, placar2 = 0; // contador de vitórias por rodada

    while (qtd1 > 0 && qtd2 > 0) {
        printf("\n===== Rodada %d =====\n", rodada);
        printf("Placar: Jogador 1 = %d | Jogador 2 = %d\n", placar1, placar2);
        printf("Cartas restantes: Jogador 1 = %d | Jogador 2 = %d\n", qtd1, qtd2);

        printf("\nCarta do Jogador 1:\n");
        mostrarCarta(jogador1[0]);

        printf("\nCarta do Jogador 2:\n");
        mostrarCarta(jogador2[0]);

        int atributo;
        printf("\n👉 Jogador %d, escolha o atributo!\n", vez);
        printf("1 - População\n2 - Área\n3 - PIB\n4 - Pontos Turísticos\n");
        printf("Escolha: ");
        scanf("%d", &atributo);

        int vitoria = 0; // 1 = jogador1, 2 = jogador2, 0 = empate

        if (atributo == 1) { 
            if (jogador1[0].populacao > jogador2[0].populacao) vitoria = 1;
            else if (jogador2[0].populacao > jogador1[0].populacao) vitoria = 2;
        } 
        else if (atributo == 2) { 
            if (jogador1[0].area > jogador2[0].area) vitoria = 1;
            else if (jogador2[0].area > jogador1[0].area) vitoria = 2;
        } 
        else if (atributo == 3) { 
            if (jogador1[0].pib > jogador2[0].pib) vitoria = 1;
            else if (jogador2[0].pib > jogador1[0].pib) vitoria = 2;
        } 
        else if (atributo == 4) { 
            if (jogador1[0].pontosTuristicos > jogador2[0].pontosTuristicos) vitoria = 1;
            else if (jogador2[0].pontosTuristicos > jogador1[0].pontosTuristicos) vitoria = 2;
        }

        // Resultado da rodada
        if (vitoria == 1) {
            printf("\n🏆 Jogador 1 venceu a rodada!\n");
            placar1++;
            jogador1[qtd1] = jogador1[0]; 
            jogador1[qtd1+1] = jogador2[0]; 
            qtd1 += 2;

            for (int i = 0; i < qtd1-2; i++) jogador1[i] = jogador1[i+1];
            for (int i = 0; i < qtd2-1; i++) jogador2[i] = jogador2[i+1];
            qtd2--;

            vez = 1; 
        } 
        else if (vitoria == 2) {
            printf("\n🏆 Jogador 2 venceu a rodada!\n");
            placar2++;
            jogador2[qtd2] = jogador2[0]; 
            jogador2[qtd2+1] = jogador1[0]; 
            qtd2 += 2;

            for (int i = 0; i < qtd2-2; i++) jogador2[i] = jogador2[i+1];
            for (int i = 0; i < qtd1-1; i++) jogador1[i] = jogador1[i+1];
            qtd1--;

            vez = 2; 
        } 
        else {
            printf("\n🤝 Empate! Cada jogador mantém sua carta.\n");
            struct Carta temp1 = jogador1[0];
            struct Carta temp2 = jogador2[0];

            for (int i = 0; i < qtd1-1; i++) jogador1[i] = jogador1[i+1];
            for (int i = 0; i < qtd2-1; i++) jogador2[i] = jogador2[i+1];

            jogador1[qtd1-1] = temp1;
            jogador2[qtd2-1] = temp2;
        }

        rodada++;

        printf("\nPressione ENTER para continuar...\n");
        getchar(); getchar(); // pausa
    }

    printf("\n===== Fim do Jogo =====\n");
    printf("Placar final: Jogador 1 = %d | Jogador 2 = %d\n", placar1, placar2);

    if (qtd1 == 0) 
        printf("\n=== 🥇 Jogador 2 venceu o jogo por cartas! ===\n");
    else 
        printf("\n=== 🥇 Jogador 1 venceu o jogo por cartas! ===\n");

    if (placar1 > placar2)
        printf("=== 🏆 Jogador 1 venceu o placar de rodadas! ===\n");
    else if (placar2 > placar1)
        printf("=== 🏆 Jogador 2 venceu o placar de rodadas! ===\n");
    else
        printf("=== 🤝 Empate no placar de rodadas! ===\n");

    return 0;
}

# Projeto 1 - SCC0201

Esse projeto consiste na elaboração de um algoritmo de gerenciamento de tarefas baseados em prioridade e ordem de chegada.

## Problema

Um sistema operacional multitarefa deve manter o sistema em pleno funcionamento, aten
dendo demandas de todos os usuários a todo momento e, em particular, fornecendo a ilusão
 que o número de processos em execução simultânea é maior que o número de processadores
 do computador. Cada processo recebe uma fatia do tempo e a alternância entre vários pro
cessos se dá de forma rápida o suficiente para que o(a) usuário(a) acabe por acreditar que
 sua execução é simultânea. O sistema operacional usa alguns algoritmos para determinar
 qual processo será executado em determinado momento e por quanto tempo. Requisições
 de processos chegam a todo instante e devem ser atendidas de acordo com seu horário de
 chegada ou com sua prioridade.
 Para execução de um processo, seu horário de chegada é uma importante característica
 levada em conta pelo sistema operacional. Além disso, como há tipos de usuários distintos no
 sistema, alguns processos associados a alguns usuários são mais prioritários que outros e esta
 é outra característica que o sistema operacional leva em conta na execução dos processos.


 Sua tarefa é simular um gerenciador de processos. Seu programa deve ajudar o sistema
 operacional a gerenciar bem e de forma eficiente os processos, lidando com os seguintes
 comandos:
 * **add prior tempo descrição**: adiciona um processo à lista de processos a serem
 executados, onde prior é um inteiro positivo (prioridade) entre 1 e 99, tempo é um
 horário no formato hh:mm:ss e descrição é uma cadeira de caracteres com tamanho
 máximo 50
 * **exec opção**: executa um processo com a opção:
   * -p: executa o processo com maior prior
   * -t: executa o processo com menor tempo
 * next opção: mostra um processo de acordo com a opção:
   * -p: mostra em uma linha todas as informações do processo com maior prioridade,
 isto é, prior, tempo e descrição (cada campo separado por um espaço)
   * -t: mostra em uma linha todas as informações do processo com menor horário, ou
 seja, prior, tempo e descrição do processo
 * chance opção anterior:novo: modifica informações de um processo de acordo com
 a opção
   * -p anterior:novo: altera o campo prior com valor anterior para o valor novo
   * -t anterior:novo: altera o campo tempo com valor anterior para o valor novo,
 utilizando o formato hh:mm:ss
 * print opção: imprime todos os processos a serem executados de acordo com a opção:
   * -p: imprime os processos em ordem decrescente de prioridade, onde as informações
 de cada processo são impressas em uma única linha (prior, tempo e descrição)
   * -t: imprime os processos em ordem crescente de horários, onde as informações de
 cada processo são impressas em uma única linha (prior, tempo e descrição)
 * quit: termina a execução do gerente de processos

 Considere que as prioridades e os horários são únicos.

 ## Caso-teste

Entrada
<div style="text-align:center">

  ![image](https://github.com/user-attachments/assets/abf3913e-dd89-4187-991b-a298edc672be)
  
</div>

Saída
<div style="text-align:center">
    
  ![image](https://github.com/user-attachments/assets/abf3913e-dd89-4187-991b-a298edc672be)
  
</div>

## Funções utilizadas

### Identifica
Indica a operação qu deve ser feita.
```c
int identifica(char* comando)
{
    // add - 0, exec - 1, next - 2, change - 3, print - 4
    char str[7] = {0};
    int i = 0;
    while (i < 6 && comando[i] != ' ' && comando[i] != '\0')
    {
        str[i] = comando[i];
        i++;
    }
    str[i] = '\0';
    if (strcmp(str, "add") == 0)
    {
        return 0;
    }
    if (strcmp(str, "exec") == 0)
    {
        return 1;
    }
    if (strcmp(str, "next") == 0)
    {
        return 2;
    }
    if (strcmp(str, "change") == 0)
    {
        return 3;
    }
    if (strcmp(str, "print") == 0)
    {
        return 4;
    }

    return -1;
}
```

### hcmp
Compara dois horários.
```c
int hcmp(horario h1, horario h2)
{
    //Se h1<h2, return 1, se h2<h1 return 0
    //analisa horas
    if(h1.hh < h2.hh)
    {
        return 1;
    }
    else if (h1.hh > h2.hh)
    {
        return 0;
    }

    //analisa minutos
    if (h1.mm < h2.mm)
    {
        return 1;
    }
    else if (h1.mm > h2.mm)
    {
        return 0;
    }

    //analisa segundos
    if(h1.ss < h2.ss)
    {
        return 1;
    }
    else if (h1.ss > h2.ss)
    {
        return 0;
    }

    return -1; 
}
```

### add_ord_prior
Adiciona um novo elemento no vetor, mantendo sempre a ordem decrescente de prioridade.
```c
void add_ord_prior(celula* lista, int cont, celula aux)
{
    int posicao;
    int acabou = 0;
    if(cont == 0)
    {
        lista[0] = aux;
    }
    else
    {
        for(int i=0;i<cont && acabou==0;i++)
        {
            if(lista[i].prior < aux.prior)
            {
                posicao = i;
                for(int j=cont;j>i;j--)
                {
                    lista[j] = lista[j-1];
                }
                acabou = 1;
            }
        }
        if(acabou == 1)
        {
            lista[posicao] = aux;
        }
        else
        {
            lista[cont] = aux;
        }
    }

    return;
}
```

### add_ord_tempo
Adiciona um novo elemento no vetor, mantendo sempre a ordem crescente de horários.
```c
void add_ord_tempo(celula* lista, int cont, celula aux)
{
    int posicao;
    int acabou = 0;
    if(cont == 0)
    {
        lista[0] = aux;
    }
    else
    {
        for(int i=0;i<cont && acabou==0;i++)
        {
            if(hcmp(aux.chegada, lista[i].chegada))
            {
                posicao = i;
                for(int j=cont;j>i;j--)
                {
                    lista[j] = lista[j-1];
                }
                acabou = 1;
            }
        }
        if(acabou == 1)
        {
            lista[posicao] = aux;
        }
        else
        {
            lista[cont] = aux;
        }
    }

    return;
}
```

### adiciona
Extrai as informações da string e adiciona no vetor.
```c
void adiciona(celula* lista_p, celula* lista_t, char *comando, int cont)
{
    celula aux;
    //int posicao;
    int k=0;
    int i=4;
    if(comando[i+1] != ' ')
    {
        aux.prior = (comando[i]- 48)*10 + comando[i+1] - 48;
        i = i+3;
    }
    else
    {
        aux.prior = comando[i] - 48;
        i = i+2;
    }
    for(int j=0;j<8;j = j + 3)
    {
        if(j == 0)
        {
            aux.chegada.hh = (comando[i+j]-48)*10 + comando[i+j+1] - 48;
        }
        else if(j == 3)
        {
            aux.chegada.mm = (comando[i+j]-48)*10 + comando[i+j+1] - 48;
        }
        else
        {
            aux.chegada.ss = (comando[i+j]-48)*10 + comando[i+j+1] - 48;
        }

    }
    i = i + 9;
    while(comando[i+k] != '\0')
    {
        aux.descricao[k] = comando[i+k];
        k++;
        //posicao = k;
    }
    aux.descricao[k] = '\0';
    add_ord_prior(lista_p, cont, aux);
    add_ord_tempo(lista_t, cont, aux);
    return;
}
```

### busca_binaria_p
Faz uma busca binária no vetor de prioridade.
```c
int busca_binaria_p(celula* lista, int cont, int prior) 
{
    int inicio = 0;
    int fim = cont - 1;
    
    while (inicio <= fim) 
    {
        int meio = inicio + (fim - inicio) / 2;
        // Verifica se o elemento está no meio
        if (lista[meio].prior == prior) {
            return meio; // Elemento encontrado, retorna o índice
        }
        // Se o elemento for maior, ignora a metade esquerda
        else if (lista[meio].prior > prior) {
            inicio = meio + 1;
        }
        // Se o elemento for menor, ignora a metade direita
        else {
            fim = meio - 1;
        }
    }

    saida_erro();
    return -1;
}

```

### busca_binaria_t
Faz uma busca binária no vetor de horários.
```c
int busca_binaria_t(celula* lista, int cont, horario t) 
{
    int inicio = 0;
    int fim = cont - 1;
    
    while (inicio <= fim) 
    {
        int meio = inicio + (fim - inicio) / 2;
        // Verifica se o elemento está no meio
        if (hcmp(lista[meio].chegada, t) == -1) {
            return meio; // Elemento encontrado, retorna o índice
        }
        // Se o elemento for maior, ignora a metade esquerda
        else if (hcmp(lista[meio].chegada, t)) {
            inicio = meio + 1;
        }
        // Se o elemento for menor, ignora a metade direita
        else {
            fim = meio - 1;
        }
    }

    saida_erro();
    return -1; 
}

```

### saida_erro
Indica se deu erro na busca binária.
```c
void saida_erro()
{ 
    printf("algo deu errado na busca binaria. certifique-se de que as entradas estão corretas\n");
    exit (1);
}
```

### exec_p
Executa a operação com maior prioridade.
```c
void exec_p(celula* lista_p, celula* lista_t, int* cont)
{
    horario t;
    int posicao;
    t = lista_p[0].chegada;
    for(int i=0;i<(*cont)-1;i++)
    {
        lista_p[i] = lista_p[i+1];
    }
    posicao = busca_binaria_t(lista_t, *cont, t);
    for(int i=posicao;i<(*cont)-1;i++)
    {
        lista_t[i] = lista_t[i+1];
    }
    (*cont)--;

    return;
}

```

### exec_t
Executa a operação com menor horário.
```c
void exec_t(celula* lista_p, celula* lista_t, int* cont)
{
    int prior;
    int posicao;
    prior = lista_t[0].prior;
    for(int i=0;i<(*cont)-1;i++)
    {
        lista_t[i] = lista_t[i+1];
    }
    posicao = busca_binaria_p(lista_p, *cont, prior);
    for(int i=posicao;i<(*cont)-1;i++)
    {
        lista_p[i] = lista_p[i+1];
    }
    (*cont)--;

    return;
}
```

### next_p
Printa a operação com maior prioridade.
```c
void next_p(celula* lista_p)
{
    printf("%02d %02d:%02d:%02d %s\n",lista_p[0].prior, lista_p[0].chegada.hh,
                                    lista_p[0].chegada.mm, lista_p[0].chegada.ss, 
                                    lista_p[0].descricao);
    printf("\n");

    return;
}
```

### next_t
Printa a operação com menor horário.
```c
void next_t(celula* lista_t)
{
    printf("%02d %02d:%02d:%02d %s\n",lista_t[0].prior, lista_t[0].chegada.hh,
                                    lista_t[0].chegada.mm, lista_t[0].chegada.ss, 
                                    lista_t[0].descricao);
    printf("\n");

    return;
}
```

### retira
Retira o elemento do vetor
```c
void retira(celula* lista, int posicao, int cont)
{
    for(int i=posicao+1;i<cont;i++)
    {
        lista[i-1] = lista[i];
    }
    lista[cont-1].prior = -1;
    lista[cont-1].chegada.hh = 99;
    lista[cont-1].chegada.mm = 99;
    lista[cont-1].chegada.ss = 99;
    return;
}
```

### troca_p
Troca a prioridade de uma operação.
```c
void troca_p(celula* lista_p, celula* lista_t, char* comando, int cont)
{
    int prior1, prior2, n, m;
    horario aux;
    celula aux2;
    if(comando[11] < 48 || comando[11] > 57)
    {
        prior1 = comando[10] - 48;
        if(comando[13] < 48 || comando[13] > 57)
        {
            prior2 = comando[12] - 48;
        }
        else
        {
            prior2 = (comando[12] - 48)*10 + comando[13] - 48;
        }
    }
    else
    {
        prior1 = (comando[10] - 48)*10 + comando[11] - 48;
        if(comando[14] < 48 || comando[14] > 57)
        {
            prior2 = comando[13] - 48;
        }
        else
        {
            prior2 = (comando[13] - 48)*10 + comando[14] - 48;
        }
    }
    n = busca_binaria_p(lista_p, cont, prior1);
    aux = lista_p[n].chegada;
    m = busca_binaria_t(lista_t, cont, aux);
    aux2 = lista_t[m];
    aux2.prior = prior2;
    retira(lista_p, n, cont);
    retira(lista_t, m, cont);
    add_ord_prior(lista_p, cont, aux2);
    add_ord_tempo(lista_t, cont, aux2);

    return;
}
```

### troca_t
Troca o horário de uma operação.
```c
void troca_t(celula* lista_t, celula* lista_p, char* comando, int cont)
{
    int n, m, aux;
    horario aux1, aux2;
    celula aux2_;
    aux1.hh = (comando[10]-48)*10+comando[11]-48;
    aux1.mm = (comando[13]-48)*10+comando[14]-48;
    aux1.ss = (comando[16]-48)*10+comando[17]-48;
    aux2.hh = (comando[19]-48)*10+comando[20]-48;
    aux2.mm = (comando[22]-48)*10+comando[23]-48;
    aux2.ss = (comando[25]-48)*10+comando[26]-48;
    n = busca_binaria_t(lista_t, cont, aux1);
    aux = lista_t[n].prior;
    m = busca_binaria_p(lista_p, cont, aux);
    aux2_ = lista_p[m];
    aux2_.chegada = aux2;
    retira(lista_t, n, cont);
    retira(lista_p, m, cont);
    add_ord_tempo(lista_t, cont, aux2_);
    add_ord_prior(lista_p, cont, aux2_);
    return;
}
```

### printa_lista
Printa a lista de operações.
```c
void printa_lista(celula *lista, int tam)
{
    for(int i=0;i<tam;i++)
    {
        printf("%02d %02d:%02d:%02d %s\n",lista[i].prior, lista[i].chegada.hh,
                                    lista[i].chegada.mm, lista[i].chegada.ss, 
                                    lista[i].descricao);
    }
    printf("\n");
}
```

### main
```c
int main()
{
    celula* lista_p;
    celula* lista_t;
    int n, cont=0;
    char comando[75];
    lista_p = (celula *) malloc(100*sizeof(celula));
    lista_t = (celula *) malloc(100*sizeof(celula));
    while (1)
    {
        scanf(" %[^\n]", comando);
        if(!strcmp(comando, "quit")) break; //caso o comando seja quit, ele sai do laço

        n = identifica(comando);
        if (n == -1) break; //caso o comando nao tenha sido identificado, também sai do laço

        switch(n)
        {
            case 0:
                if(cont%100 == 0 && cont != 0)
                {
                    lista_p = realloc(lista_p, ((cont/100)+1)*100*sizeof(celula));
                    lista_t = realloc(lista_t, ((cont/100)+1)*100*sizeof(celula));
                }
                adiciona(lista_p, lista_t, comando, cont);
                cont++;
                break;
            case 1:
                if(comando[6] == 'p')
                {
                    exec_p(lista_p, lista_t, &cont);
                }
                else
                {
                    exec_t(lista_p, lista_t, &cont);
                }
                break;
            case 2:
                if(comando[6] == 'p')
                {
                    next_p(lista_p);
                }
                else
                {
                    next_t(lista_t);
                }
                break;
            case 3:
                if(comando[8] == 'p')
                {
                    troca_p(lista_p, lista_t, comando, cont);
                }
                else
                {
                    troca_t(lista_t, lista_p, comando, cont);
                }
                break;
            case 4:
                if(comando[7] == 'p')
                {
                    printa_lista(lista_p, cont);
                }
                else
                {
                    printa_lista(lista_t, cont);
                }
                break;
        }
    }


    free(lista_p);
    free(lista_t);
    return 0;
}
```

### Código completo
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define MAX_DESCR 50

typedef struct horas
{
    int hh;
    int mm;
    int ss;
}horario;

typedef struct process
{
    int prior;
    horario chegada;
    char descricao[MAX_DESCR];
}celula;

/*prototipos de funçoes*/
int identifica(char* comando);
int hcmp(horario h1, horario h2);
void add_ord_prior(celula* lista, int cont, celula aux);
void add_ord_tempo(celula* lista, int cont, celula aux);
void adiciona(celula* lista_p, celula* lista_t, char *comando, int cont);
int busca_binaria_p(celula* lista, int cont, int prior);
int busca_binaria_t(celula* lista, int cont, horario t);
void saida_erro();
void exec_p(celula* lista_p, celula* lista_t, int* cont);
void exec_t(celula* lista_p, celula* lista_t, int* cont);
void next_p(celula* lista_p);
void next_t(celula* lista_t);
void retira(celula* lista, int posicao, int cont);
void troca_p(celula* lista_p, celula* lista_t, char* comando, int cont);
void troca_t(celula* lista_t, celula* lista_p, char* comando, int cont);
void printa_lista(celula *lista, int tam);



int identifica(char* comando)
{
    // add - 0, exec - 1, next - 2, change - 3, print - 4
    char str[7] = {0};
    int i = 0;
    while (i < 6 && comando[i] != ' ' && comando[i] != '\0')
    {
        str[i] = comando[i];
        i++;
    }
    str[i] = '\0';
    if (strcmp(str, "add") == 0)
    {
        return 0;
    }
    if (strcmp(str, "exec") == 0)
    {
        return 1;
    }
    if (strcmp(str, "next") == 0)
    {
        return 2;
    }
    if (strcmp(str, "change") == 0)
    {
        return 3;
    }
    if (strcmp(str, "print") == 0)
    {
        return 4;
    }

    return -1;
}

int hcmp(horario h1, horario h2)
{
    //Se h1<h2, return 1, se h2<h1 return 0
    //analisa horas
    if(h1.hh < h2.hh)
    {
        return 1;
    }
    else if (h1.hh > h2.hh)
    {
        return 0;
    }

    //analisa minutos
    if (h1.mm < h2.mm)
    {
        return 1;
    }
    else if (h1.mm > h2.mm)
    {
        return 0;
    }

    //analisa segundos
    if(h1.ss < h2.ss)
    {
        return 1;
    }
    else if (h1.ss > h2.ss)
    {
        return 0;
    }

    return -1; 
}

//A função insere uma célula no lugar certo, em relação a ordenação
void add_ord_prior(celula* lista, int cont, celula aux)
{
    int posicao;
    int acabou = 0;
    if(cont == 0)
    {
        lista[0] = aux;
    }
    else
    {
        for(int i=0;i<cont && acabou==0;i++)
        {
            if(lista[i].prior < aux.prior)
            {
                posicao = i;
                for(int j=cont;j>i;j--)
                {
                    lista[j] = lista[j-1];
                }
                acabou = 1;
            }
        }
        if(acabou == 1)
        {
            lista[posicao] = aux;
        }
        else
        {
            lista[cont] = aux;
        }
    }

    return;
}


void add_ord_tempo(celula* lista, int cont, celula aux)
{
    int posicao;
    int acabou = 0;
    if(cont == 0)
    {
        lista[0] = aux;
    }
    else
    {
        for(int i=0;i<cont && acabou==0;i++)
        {
            if(hcmp(aux.chegada, lista[i].chegada))
            {
                posicao = i;
                for(int j=cont;j>i;j--)
                {
                    lista[j] = lista[j-1];
                }
                acabou = 1;
            }
        }
        if(acabou == 1)
        {
            lista[posicao] = aux;
        }
        else
        {
            lista[cont] = aux;
        }
    }

    return;
}

//Função de leitura da string e inserção na lista
void adiciona(celula* lista_p, celula* lista_t, char *comando, int cont)
{
    celula aux;
    //int posicao;
    int k=0;
    int i=4;
    if(comando[i+1] != ' ')
    {
        aux.prior = (comando[i]- 48)*10 + comando[i+1] - 48;
        i = i+3;
    }
    else
    {
        aux.prior = comando[i] - 48;
        i = i+2;
    }
    for(int j=0;j<8;j = j + 3)
    {
        if(j == 0)
        {
            aux.chegada.hh = (comando[i+j]-48)*10 + comando[i+j+1] - 48;
        }
        else if(j == 3)
        {
            aux.chegada.mm = (comando[i+j]-48)*10 + comando[i+j+1] - 48;
        }
        else
        {
            aux.chegada.ss = (comando[i+j]-48)*10 + comando[i+j+1] - 48;
        }

    }
    i = i + 9;
    while(comando[i+k] != '\0')
    {
        aux.descricao[k] = comando[i+k];
        k++;
        //posicao = k;
    }
    aux.descricao[k] = '\0';
    add_ord_prior(lista_p, cont, aux);
    add_ord_tempo(lista_t, cont, aux);
    return;
}


int busca_binaria_p(celula* lista, int cont, int prior) 
{
    int inicio = 0;
    int fim = cont - 1;
    
    while (inicio <= fim) 
    {
        int meio = inicio + (fim - inicio) / 2;
        // Verifica se o elemento está no meio
        if (lista[meio].prior == prior) {
            return meio; // Elemento encontrado, retorna o índice
        }
        // Se o elemento for maior, ignora a metade esquerda
        else if (lista[meio].prior > prior) {
            inicio = meio + 1;
        }
        // Se o elemento for menor, ignora a metade direita
        else {
            fim = meio - 1;
        }
    }

    saida_erro();
    return -1;
}

int busca_binaria_t(celula* lista, int cont, horario t) 
{
    int inicio = 0;
    int fim = cont - 1;
    
    while (inicio <= fim) 
    {
        int meio = inicio + (fim - inicio) / 2;
        // Verifica se o elemento está no meio
        if (hcmp(lista[meio].chegada, t) == -1) {
            return meio; // Elemento encontrado, retorna o índice
        }
        // Se o elemento for maior, ignora a metade esquerda
        else if (hcmp(lista[meio].chegada, t)) {
            inicio = meio + 1;
        }
        // Se o elemento for menor, ignora a metade direita
        else {
            fim = meio - 1;
        }
    }

    saida_erro();
    return -1; 
}

//funcao para indicar erro nas buscas binarias. vai parar a execução do programa caso algo dê errado
void saida_erro()
{ 
    printf("algo deu errado na busca binaria. certifique-se de que as entradas estão corretas\n");
    exit (1);
}

void exec_p(celula* lista_p, celula* lista_t, int* cont)
{
    horario t;
    int posicao;
    t = lista_p[0].chegada;
    for(int i=0;i<(*cont)-1;i++)
    {
        lista_p[i] = lista_p[i+1];
    }
    posicao = busca_binaria_t(lista_t, *cont, t);
    for(int i=posicao;i<(*cont)-1;i++)
    {
        lista_t[i] = lista_t[i+1];
    }
    (*cont)--;

    return;
}

void exec_t(celula* lista_p, celula* lista_t, int* cont)
{
    int prior;
    int posicao;
    prior = lista_t[0].prior;
    for(int i=0;i<(*cont)-1;i++)
    {
        lista_t[i] = lista_t[i+1];
    }
    posicao = busca_binaria_p(lista_p, *cont, prior);
    for(int i=posicao;i<(*cont)-1;i++)
    {
        lista_p[i] = lista_p[i+1];
    }
    (*cont)--;

    return;
}

void next_p(celula* lista_p)
{
    printf("%02d %02d:%02d:%02d %s\n",lista_p[0].prior, lista_p[0].chegada.hh,
                                    lista_p[0].chegada.mm, lista_p[0].chegada.ss, 
                                    lista_p[0].descricao);
    printf("\n");

    return;
}

void next_t(celula* lista_t)
{
    printf("%02d %02d:%02d:%02d %s\n",lista_t[0].prior, lista_t[0].chegada.hh,
                                    lista_t[0].chegada.mm, lista_t[0].chegada.ss, 
                                    lista_t[0].descricao);
    printf("\n");

    return;
}

void retira(celula* lista, int posicao, int cont)
{
    for(int i=posicao+1;i<cont;i++)
    {
        lista[i-1] = lista[i];
    }
    lista[cont-1].prior = -1;
    lista[cont-1].chegada.hh = 99;
    lista[cont-1].chegada.mm = 99;
    lista[cont-1].chegada.ss = 99;
    return;
}

void troca_p(celula* lista_p, celula* lista_t, char* comando, int cont)
{
    int prior1, prior2, n, m;
    horario aux;
    celula aux2;
    if(comando[11] < 48 || comando[11] > 57)
    {
        prior1 = comando[10] - 48;
        if(comando[13] < 48 || comando[13] > 57)
        {
            prior2 = comando[12] - 48;
        }
        else
        {
            prior2 = (comando[12] - 48)*10 + comando[13] - 48;
        }
    }
    else
    {
        prior1 = (comando[10] - 48)*10 + comando[11] - 48;
        if(comando[14] < 48 || comando[14] > 57)
        {
            prior2 = comando[13] - 48;
        }
        else
        {
            prior2 = (comando[13] - 48)*10 + comando[14] - 48;
        }
    }
    n = busca_binaria_p(lista_p, cont, prior1);
    aux = lista_p[n].chegada;
    m = busca_binaria_t(lista_t, cont, aux);
    aux2 = lista_t[m];
    aux2.prior = prior2;
    retira(lista_p, n, cont);
    retira(lista_t, m, cont);
    add_ord_prior(lista_p, cont, aux2);
    add_ord_tempo(lista_t, cont, aux2);

    return;
}

void troca_t(celula* lista_t, celula* lista_p, char* comando, int cont)
{
    int n, m, aux;
    horario aux1, aux2;
    celula aux2_;
    aux1.hh = (comando[10]-48)*10+comando[11]-48;
    aux1.mm = (comando[13]-48)*10+comando[14]-48;
    aux1.ss = (comando[16]-48)*10+comando[17]-48;
    aux2.hh = (comando[19]-48)*10+comando[20]-48;
    aux2.mm = (comando[22]-48)*10+comando[23]-48;
    aux2.ss = (comando[25]-48)*10+comando[26]-48;
    n = busca_binaria_t(lista_t, cont, aux1);
    aux = lista_t[n].prior;
    m = busca_binaria_p(lista_p, cont, aux);
    aux2_ = lista_p[m];
    aux2_.chegada = aux2;
    retira(lista_t, n, cont);
    retira(lista_p, m, cont);
    add_ord_tempo(lista_t, cont, aux2_);
    add_ord_prior(lista_p, cont, aux2_);
    return;
}

void printa_lista(celula *lista, int tam)
{
    for(int i=0;i<tam;i++)
    {
        printf("%02d %02d:%02d:%02d %s\n",lista[i].prior, lista[i].chegada.hh,
                                    lista[i].chegada.mm, lista[i].chegada.ss, 
                                    lista[i].descricao);
    }
    printf("\n");
}


int main()
{
    celula* lista_p;
    celula* lista_t;
    int n, cont=0;
    char comando[75];
    lista_p = (celula *) malloc(100*sizeof(celula));
    lista_t = (celula *) malloc(100*sizeof(celula));
    while (1)
    {
        scanf(" %[^\n]", comando);
        if(!strcmp(comando, "quit")) break; //caso o comando seja quit, ele sai do laço

        n = identifica(comando);
        if (n == -1) break; //caso o comando nao tenha sido identificado, também sai do laço

        switch(n)
        {
            case 0:
                if(cont%100 == 0 && cont != 0)
                {
                    lista_p = realloc(lista_p, ((cont/100)+1)*100*sizeof(celula));
                    lista_t = realloc(lista_t, ((cont/100)+1)*100*sizeof(celula));
                }
                adiciona(lista_p, lista_t, comando, cont);
                cont++;
                break;
            case 1:
                if(comando[6] == 'p')
                {
                    exec_p(lista_p, lista_t, &cont);
                }
                else
                {
                    exec_t(lista_p, lista_t, &cont);
                }
                break;
            case 2:
                if(comando[6] == 'p')
                {
                    next_p(lista_p);
                }
                else
                {
                    next_t(lista_t);
                }
                break;
            case 3:
                if(comando[8] == 'p')
                {
                    troca_p(lista_p, lista_t, comando, cont);
                }
                else
                {
                    troca_t(lista_t, lista_p, comando, cont);
                }
                break;
            case 4:
                if(comando[7] == 'p')
                {
                    printa_lista(lista_p, cont);
                }
                else
                {
                    printa_lista(lista_t, cont);
                }
                break;
        }
    }


    free(lista_p);
    free(lista_t);
    return 0;
}
```

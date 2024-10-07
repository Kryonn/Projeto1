# Projeto1

## ICC2

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
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

void intercala(celula* lista, int ini, int meio, int fim)
{
    int k=0,j=0;
    int n1 = meio-ini+1;
    int n2 = fim-meio;
    celula lvet[n1+1];
    celula rvet[n2+1];
    for(int i=0;i<n1;i++)
    {
        lvet[i] = lista[ini+i];
    }
    for(int i=0;i<n2;i++)
    {
        rvet[i] = lista[meio+1+i];
    }
    lvet[n1].prior = 999999;
    rvet[n2].prior = 999999;
    for(int i=ini;i<=fim;i++)
    {
        if(lvet[j].prior < rvet[k].prior)
        {
            lista[i] = lvet[j];
            j++;
        }
        else
        {
            lista[i] = rvet[k];
            k++;
        }
    }
}

void mergesort(celula* lista, int ini, int fim)
{
    int meio;
    if(ini < fim)
    {
        meio = (fim+ini)/2;
        mergesort(lista, ini, meio);
        mergesort(lista, meio+1, fim);
        intercala(lista, ini, meio, fim);
    }
}

int identifica(char* comando)
{
    //add - 0, next - 1, exec - 2, change - 3, print - 4
    char* str;
    int cont=0, flag=0;
    str = (char*) malloc(6*sizeof(char));
    for(int i=0;i<6 && flag==0;i++)
    {
        if(comando[i] != ' ')
        {
            str[i] = comando[i];
            cont++;
        }
        else
        {
            flag = 1;
        }
    }
    str = realloc(str, cont);
    if(strcmp(str, "add") == 0)
    {
        return 0;
    }
    if(strcmp(str, "next") == 0)
    {
        return 1;
    }
    if(strcmp(str, "exec") == 0)
    {
        return 2;
    }
    if(strcmp(str, "change") == 0)
    {
        return 3;
    }
    if(strcmp(str, "print") == 0)
    {
        return 4;
    }
}

void adiciona(celula *lista, char* comando, int cont)
{
    int k=0;
    celula* p;
    p = (celula*) malloc(sizeof(celula));
    if(cont % 10 == 0 && cont != 0)
    {
        lista = realloc(lista, cont + 10);
    }
    int i=4;
    if(comando[i+1] != ' ')
    {
        p->prior = (comando[i]- 48)*10 + comando[i+1] - 48;
        i = i+3;
    }
    else
    {
        p->prior = comando[i] - 48;
        i = i+2;
    }
    //printf("%d\n", p->prior);
    for(int j=0;j<8;j = j + 3)
    {
        if(j == 0)
        {
            p->chegada.hh = comando[i+j] + comando[i+j+1] - 96;
        }
        else
        {
            if(j == 3)
            {
                p->chegada.mm = comando[i+j] + comando[i+j+1] - 96;
            }
            else
            {
                p->chegada.ss = comando[i+j] + comando[i+j+1] - 96;
            }
        }
        
    }
    i = i + 9;
    while(comando[i+k] != '\0')
    {
        p->descricao[k] = comando[i+k];
        k++;
    }
    //printf("%s\n", p->descricao);
    lista[cont] = *p;
}


int main()
{
    char comando[67];
    celula *lista_p;
    int n, cont=0;
    lista_p = (celula*) malloc(10*sizeof(celula));
    do
    {
        scanf(" %[^\n]", comando);
        n = identifica(comando);
        switch(n)
        {
            case 0:
                adiciona(lista_p, comando, cont);
                cont++;
            case 1:
                
        }
    }
    while(strcmp(comando, "quit") != 0);
    mergesort(lista_p, 0, 1);
    for(int i=0;i<2;i++)
    {
        printf("%d\n", lista_p[i].prior);
    }
    return 0;
}
```


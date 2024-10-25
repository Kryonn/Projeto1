# Projeto1

## ICC2

Identifica:
```c
int identifica(char* comando)
{
    // add - 0, exec - 1, next - 2, change - 3, print - 4
    char str[6] = {0};
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
    int i=4;
    if(comando[i+1] != ' ')
    {
        lista[cont].prior = (comando[i]- 48)*10 + comando[i+1] - 48;
        i = i+3;
    }
    else
    {
        lista[cont].prior = comando[i] - 48;
        i = i+2;
    }
    //printf("%d\n", p->prior);
    for(int j=0;j<8;j = j + 3)
    {
        if(j == 0)
        {
            lista[cont].chegada.hh = (comando[i+j]-48)*10 + comando[i+j+1] - 48;
        }
        else
        {
            if(j == 3)
            {
                lista[cont].chegada.mm = (comando[i+j]-48)*10 + comando[i+j+1] - 48;
            }
            else
            {
                lista[cont].chegada.ss = (comando[i+j]-48)*10 + comando[i+j+1] - 48;
            }
        }
        
    }
    i = i + 9;
    while(comando[i+k] != '\0')
    {
        lista[cont].descricao[k] = comando[i+k];
        k++;
    }
    //printf("%s\n", p->descricao);
}


int main()
{
    char comando[67];
    celula *lista_p;
    int n, cont=0;
    lista_p = (celula*) malloc(sizeof(celula));
    do
    {
        scanf(" %[^\n]", comando);
        n = identifica(comando);
        switch(n)
        {
            case 0:
                if(cont == 0)
                {
                    adiciona(lista_p, comando, cont);
                    cont++;
                }
                else
                {
                    lista_p = realloc(lista_p, cont + 1);
                    adiciona(lista_p, comando, cont);
                    cont++;
                }
                break;
            case 1:
                
        }
    }
    while(strcmp(comando, "quit") != 0);
    mergesort(lista_p, 0, 2);
    for(int i=0;i<3;i++)
    {
        printf("%d\n", lista_p[i].prior);
    }
    return 0;
}
```
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

int hcmp(horario h1, horario h2)
{
    //Se h1<h2, return 1, se h2<h1 return 0
    if(h1.hh < h2.hh)
    {
        return 1;
    }
    else
    {
        if(h1.hh > h2.hh)
        {
            return 0;
        }
        else
        {
            if(h1.mm < h2.mm)
            {
                return 1;
            }
            else
            {
                if(h1.mm < h2.mm)
                {
                    return 0;
                }
                else
                {
                    if(h1.ss < h2.ss)
                    {
                        return 1;
                    }
                    else
                    {
                        return 0;
                    }
                }
            }
        }
    }
}

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
    int i=4;
    if(comando[i+1] != ' ')
    {
        lista[cont].prior = (comando[i]- 48)*10 + comando[i+1] - 48;
        i = i+3;
    }
    else
    {
        lista[cont].prior = comando[i] - 48;
        i = i+2;
    }
    //printf("%d\n", p->prior);
    for(int j=0;j<8;j = j + 3)
    {
        if(j == 0)
        {
            lista[cont].chegada.hh = (comando[i+j]-48)*10 + comando[i+j+1] - 48;
        }
        else
        {
            if(j == 3)
            {
                lista[cont].chegada.mm = (comando[i+j]-48)*10 + comando[i+j+1] - 48;
            }
            else
            {
                lista[cont].chegada.ss = (comando[i+j]-48)*10 + comando[i+j+1] - 48;
            }
        }
        
    }
    i = i + 9;
    while(comando[i+k] != '\0')
    {
        lista[cont].descricao[k] = comando[i+k];
        k++;
    }
    //printf("%s\n", p->descricao);
}

void printa_lista(celula *lista, int tam)
{
    for(int i=0;i<tam;i++)
    {
        printf("%02d %02d:%02d:%02d %s\n",lista[i].prior, lista[i].chegada.hh,
                                    lista[i].chegada.mm, lista[i].chegada.ss, 
                                    lista[i].descricao);
    }
}


int main()
{
    char comando[67];
    celula *lista_p;
    int n, cont=0;
    lista_p = (celula*) malloc(sizeof(celula));
    do
    {
        scanf(" %[^\n]", comando);
        n = identifica(comando);
        switch(n)
        {
            case 0:
                if(cont == 0)
                {
                    adiciona(lista_p, comando, cont);
                    cont++;
                }
                else
                {
                    lista_p = realloc(lista_p, cont + 1);
                    adiciona(lista_p, comando, cont);
                    cont++;
                }
                break;
            case 4:
                if(comando[7] == 'p')
                {
                    mergesort(lista_p, 0, cont);
                    printa_lista(lista_p, cont+1);
                }
                break;
        }
    }
    while(strcmp(comando, "quit") != 0);
    return 0;
}
```
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

int hcmp(horario h1, horario h2)
{
    //Se h1<h2, return 1, se h2<h1 return 0
    if(h1.hh < h2.hh)
    {
        return 1;
    }
    else
    {
        if(h1.hh > h2.hh)
        {
            return 0;
        }
        else
        {
            if(h1.mm < h2.mm)
            {
                return 1;
            }
            else
            {
                if(h1.mm < h2.mm)
                {
                    return 0;
                }
                else
                {
                    if(h1.ss < h2.ss)
                    {
                        return 1;
                    }
                    else
                    {
                        return 0;
                    }
                }
            }
        }
    }
}

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
    int i=4;
    if(comando[i+1] != ' ')
    {
        lista[cont].prior = (comando[i]- 48)*10 + comando[i+1] - 48;
        i = i+3;
    }
    else
    {
        lista[cont].prior = comando[i] - 48;
        i = i+2;
    }
    //printf("%d\n", p->prior);
    for(int j=0;j<8;j = j + 3)
    {
        if(j == 0)
        {
            lista[cont].chegada.hh = (comando[i+j]-48)*10 + comando[i+j+1] - 48;
        }
        else
        {
            if(j == 3)
            {
                lista[cont].chegada.mm = (comando[i+j]-48)*10 + comando[i+j+1] - 48;
            }
            else
            {
                lista[cont].chegada.ss = (comando[i+j]-48)*10 + comando[i+j+1] - 48;
            }
        }
        
    }
    i = i + 9;
    while(comando[i+k] != '\0')
    {
        lista[cont].descricao[k] = comando[i+k];
        k++;
    }
    //printf("%s\n", p->descricao);
}

void printa_lista(celula *lista, int tam)
{
    for(int i=0;i<tam;i++)
    {
        printf("%02d %02d:%02d:%02d %s\n",lista[i].prior, lista[i].chegada.hh,
                                    lista[i].chegada.mm, lista[i].chegada.ss, 
                                    lista[i].descricao);
    }
}


int main()
{
    char comando[67];
    celula *lista_p;
    int n, cont=0;
    lista_p = (celula*) malloc(sizeof(celula));
    do
    {
        scanf(" %[^\n]", comando);
        n = identifica(comando);
        switch(n)
        {
            case 0:
                if(cont == 0)
                {
                    adiciona(lista_p, comando, cont);
                    cont++;
                }
                else
                {
                    lista_p = realloc(lista_p, cont + 1);
                    adiciona(lista_p, comando, cont);
                    cont++;
                }
                break;
            case 4:
                if(comando[7] == 'p')
                {
                    mergesort(lista_p, 0, cont-1);
                    printa_lista(lista_p, cont);
                }
                break;
        }
    }
    while(strcmp(comando, "quit") != 0);
    return 0;
}
```



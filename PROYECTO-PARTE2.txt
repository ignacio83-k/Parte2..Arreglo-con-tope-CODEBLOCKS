#include <stdio.h>
#include <stdlib.h>

int opcion=0;
int Tamanio_Estructura1 = 5;
int Tamanio_Estructura2 = 4;
int EspacioLibre_Estructura1 = 0;
int EspacioLibre_Estructura2 = 0;

struct Arreglo_Tope
{
    int arreglo[5];
    int tope;
} Estructura1, Estructura1_Auxiliar;

struct Arreglo_Tope2
{
    int arreglo[4];
    int tope;
} Estructura2, Estructura2_Auxiliar;

/* === RE-UTILIZABLES === */
int MostrarMenu()//Muestro menu y devulevo la opción seleccionada
{
    printf("\n");
    printf(" 1 - Agregar Int en estructura \n");
    printf(" 2 - Mostrar estructuras \n");
    printf(" 3 - Pasar elementos del arreglo con mas elementos al que tiene menos \n");
    printf(" 4 - Cual tiene mas elementos? \n");
    printf(" 5 - Cual tiene mas espacio libre? \n");
    printf("---------------------------------------------\n");
    printf(" 0 - Salir \n");
    printf("---------------------------------------------\n");
    printf("     Ingrese opcion : ");
    scanf("%d", &opcion);
    return opcion;
}

void ReinicioMenu()//Limpio la consola
{
    if (opcion!=0)
    {
        system("PAUSE");
        system("CLS");
        //system("sleep 2");		//para Linux, hace una pausa de 2 segundos
        //printf("\n \n \n \n \n ");	//para Linux, saltear 5 lineas
        //system("clear"); // ACLARACION : En caso de utilizar linux, utilice esta linea de codigo en lugar de "CLS" y comente la anterior
    }
}

int menuElijoEstructura()//para elegir una estructura
{
    int nro = 0;
    do
	{
        printf("\n");
        printf("        1 - En estructura 1  \n");
        printf("        2 - En estructura 2  \n");
        printf("---------------------------------------------\n");
        printf("        0 - Salir \n");
        printf("---------------------------------------------\n");
        printf("            Ingrese opcion : ");
        scanf("%d", &nro);
    } while ((nro != 0) && (nro != 1) && (nro != 2));
    return nro;
}

int Estructura1_LugaresLibres()// Calcula los lugares libres en la estructura 1
{
    return Tamanio_Estructura1 - Estructura1.tope - 1;
}

int Estructura2_LugaresLibres()// Calcula los lugares libres en la estructura 2
{
    return Tamanio_Estructura2 - Estructura2.tope - 1;
}

void Muestro_EspacioLibre()
{
    printf("    Espacio libre en estructura 1 : %d \n", EspacioLibre_Estructura1);
    printf("    Espacio libre en estructura 2 : %d \n", EspacioLibre_Estructura2);
}

void Muestro_CantidadElementos()
{
    printf("    Cant. elementos en estructura 1 : %d \n", Estructura1.tope + 1);
    printf("    Cant. elementos en estructura 2 : %d \n", Estructura2.tope + 1);
}

void PasarElementos_Estructura1A2()
{
    int contador=0;//para contar cuantos elementos se pasan
    int contador_auxiliar = 0;
    Estructura1_Auxiliar.tope = -1;
    EspacioLibre_Estructura2 = Estructura2_LugaresLibres();//calculo espacio libre y lo guardo en la variable

    /* Paso todos los elementos de la estructura 1 a la estructura 2, mientras tenga espacio libre */
    for (int i=0; i<=Estructura1.tope; i++)
    {
        EspacioLibre_Estructura2 = Estructura2_LugaresLibres();
        if (EspacioLibre_Estructura2 > 0)
        {
            Estructura2.tope = Estructura2.tope+1;
            Estructura2.arreglo[Estructura2.tope]=Estructura1.arreglo[i];
            contador++;
        }
        else
        {

            Estructura1_Auxiliar.arreglo[contador_auxiliar] = Estructura1.arreglo[i];
            Estructura1_Auxiliar.tope = Estructura1_Auxiliar.tope+1;
            contador_auxiliar++;
        }
    }

    /* Borro los elementos de la estructura 1 pasados a la estructura 2 */
    for (int i=Estructura1.tope; i>-1; i--)
    {
        Estructura1.arreglo[Estructura1.tope] = 0;
        Estructura1.tope = Estructura1.tope-1;
    }

    /* Paso los elementos auxiliares (sobrantes) a la estructura 1 de nuevo, para reposicionar */
    for (int i=0; i<=Estructura1_Auxiliar.tope; i++)
    {
        Estructura1.tope = Estructura1.tope+1;
        Estructura1.arreglo[Estructura1.tope] = Estructura1_Auxiliar.arreglo[i];
    }
    printf("\n    Elementos pasados con exito :  %d \n", contador);
}



void PasarElementos_Estructura2A1()
{
    int contador=0;//para contar cuantos elementos se pasan
    int contador_auxiliar = 0;
    Estructura2_Auxiliar.tope = -1;
    EspacioLibre_Estructura1 = Estructura1_LugaresLibres();//calculo espacio libre y lo guardo en la variable

    /* Paso todos los elementos de la estructura 2 a la estructura 1, mientras tenga espacio libre */
    for (int i=0; i<=Estructura2.tope; i++)
    {
        EspacioLibre_Estructura1 = Estructura1_LugaresLibres();
        if (EspacioLibre_Estructura1 > 0)
        {
            Estructura1.tope = Estructura1.tope+1;
            Estructura1.arreglo[Estructura1.tope]=Estructura2.arreglo[i];
            contador++;
        }
        else
        {

            Estructura2_Auxiliar.arreglo[contador_auxiliar] = Estructura2.arreglo[i];
            Estructura2_Auxiliar.tope = Estructura2_Auxiliar.tope+1;
            contador_auxiliar++;
        }
    }

    /* Borro los elementos de la estructura 2 pasados a la estructura 1 */
    for (int i=Estructura2.tope; i>-1; i--)
    {
        Estructura2.arreglo[Estructura2.tope] = 0;
        Estructura2.tope = Estructura2.tope-1;

    }

    /* Paso los elementos auxiliares (sobrantes) a la estructura 1 de nuevo, para reposicionar */
    for (int i=0; i<=Estructura2_Auxiliar.tope; i++)
    {
        Estructura2.tope = Estructura2.tope+1;
        Estructura2.arreglo[Estructura2.tope] = Estructura2_Auxiliar.arreglo[i];
    }
    printf("\n    Elementos pasados con exito :  %d \n", contador);
}



/* === FIN RE-UTILIZABLES === */

/* === OPCIONES === */
void CargarDatos()// 1 - Cargar datos
{
    int donde=0;
    int numero=0;
    donde = menuElijoEstructura();//llama a menu, para elegir estructura1 o estructura2

    /* VERIFICACIÓN DE ELEMENTOS RESTANTES */
    EspacioLibre_Estructura1 = Estructura1_LugaresLibres();//calculo espacio libre y lo guardo en una variable
    EspacioLibre_Estructura2 = Estructura2_LugaresLibres();//calculo espacio libre y lo guardo en otra variable

    if ((donde==1 && EspacioLibre_Estructura1>0) || (donde==2 && EspacioLibre_Estructura2>0))//si hay lugares libres en la estructura elegida
    {
        /* INGRESO DE DATOS */
        printf("\n            Ingrese un numero : ");
        scanf("%d", &numero);
        switch (donde)
        {
            case 1:
                Estructura1.tope = Estructura1.tope+1;//desplazo posicion del ultimo dato cargado
                Estructura1.arreglo[Estructura1.tope]= numero;//guardo un NUMERO en dicha posicion
                break;
            case 2:
                Estructura2.tope = Estructura2.tope+1;//desplazo posicion del ultimo dato cargado
                Estructura2.arreglo[Estructura2.tope]= numero;//guardo un NUMERO en dicha posicion
                break;
        }
        MostrarEstructuras();
    }
    else
    {
        /* MUESTRO MENSAJE DE ESTRUCTURA LLENA */
        if (donde==1)
        {
            printf("\n  La estructura 1 esta llena, no se pueden cargar mas datos \n");
        }

        if (donde==2)
        {
            printf("\n  La estructura 2 esta llena, no se pueden cargar mas datos \n");
        }
    }
}

void MostrarEstructuras()// 2 - Mostrar estructuras
{
    EspacioLibre_Estructura1 = Estructura1_LugaresLibres();//calculo espacio libre y lo guardo en una variable
    EspacioLibre_Estructura2 = Estructura2_LugaresLibres();//calculo espacio libre y lo guardo en otra variable

    printf("\n Estructura 1, cantidad de elementos: %d, elementos restantes : %d \n", Estructura1.tope+1, EspacioLibre_Estructura1);
    for (int i=0; i<=Estructura1.tope; i++)
    {
        printf("    |%d| \n", Estructura1.arreglo[i]);
    }

    printf("\n Estructura 2, cantidad de elementos: %d, elementos restantes : %d \n", Estructura2.tope+1, EspacioLibre_Estructura2);
    for (int i=0; i<=Estructura2.tope; i++)
    {
        printf("    |%d| \n", Estructura2.arreglo[i]);
    }
}

void PasarElementos()// 3 - Pasar elementos del arreglo con mas elementos al de menos elementos
{
    if (Estructura1.tope == Estructura2.tope)//si tienen la misma cantidad de elementos, no se mueven.
    {
        printf("\n    Ambas estructuras tienen la misma cantidad de elementos \n");
        Muestro_CantidadElementos();

    }//sino, una estructura tiene mas elementoss que la otra
    else
    {
        if (Estructura1.tope > Estructura2.tope)//si la estructura 1 tiene mas elementos que la estructura 2
        {
            PasarElementos_Estructura1A2();
        }
        else//si la estructura 2 tiene mas elementos que la estructura 1
        {
            PasarElementos_Estructura2A1();
        }
        MostrarEstructuras();//estructura al FINAL del pasaje
    }
}

void CualTieneMasElementos()//4 - Cual tiene mas elementos
{
    if (Estructura1.tope == Estructura2.tope)//tienen la misma cantidad de elementos
    {
        printf("\n    Ambas estructuras tienen la misma cantidad de elementos \n");
    }
    else//sino, NO TIENEN la misma cantidad de elementos, alguna mas que la otra
    {
        if (Estructura1.tope > Estructura2.tope)
        {
            printf("\n    La estructura 1 tiene mas elementos que la estructura 2 \n");
        }
        else
        {
            printf("\n    La estructura 2 tiene mas elementos que la estructura 1 \n");
        }
    }
    Muestro_CantidadElementos();
}

void EspacioLibre()//5 - Cual tiene mas espacio libre
{
    EspacioLibre_Estructura1 = Estructura1_LugaresLibres();//calculo espacio libre y lo guardo en una variable
    EspacioLibre_Estructura2 = Estructura2_LugaresLibres();//calculo espacio libre y lo guardo en otra variable

    if (EspacioLibre_Estructura1 == EspacioLibre_Estructura2)
    {
        printf("\n    Ambas estructuras tienen la misma cantidad de espacio libre \n");
    }
    else//sino, tienen distinto espacio libre, alguna mas que la otra
    {
        if (EspacioLibre_Estructura1 < EspacioLibre_Estructura2)
        {
            printf("\n    La estructura 2 tiene mas espacio libre que la estructura 1 \n");
        }
        else
        {
            printf("\n    La estructura 1 tiene mas espacio libre que la estructura 2 \n");
        }
    }
    Muestro_EspacioLibre();
}
/* === FIN OPCIONES === */


int main()//Main - MENU PRINCIPAL
{
    Estructura1.tope = -1;//inicialmente la estructura esta vacia
    Estructura2.tope = -1;//inicialmente la estructura esta vacia
	do
	{
        opcion = MostrarMenu();//muestro menu y elijo una opcion
		switch (opcion)
	    {
            case 1:
                    CargarDatos();
                    break;
            case 2:
                    MostrarEstructuras();
                    break;
            case 3:
                    PasarElementos();
                    break;
            case 4:
                    CualTieneMasElementos();
                    break;
	        case 5:
                    EspacioLibre();
	                break;
            default:
                    printf ("\n");
                    printf ("--------- Error, verifique  ------------------- \n");
        }
        ReinicioMenu();
	} while (opcion != 0);
}

# Tarea-1

#include <stdio.h>
#include <time.h>
#include <string.h>
#include <stdlib.h>

#define NUMERO_DE_BEBIDAS_INICIO  9    //Es el nuumero de bebidas de la carta
#define NUMERO_DE_ORDENES  10    //Es el numero de ordenes que puede haber en un dia
#define MAX_BEBIDAS_ORDEN  10    //Es el numero masximo de bebidas que se pueden pedir en una orden

typedef struct {          //Contiene las bebidas de la carta
	int IDBebida;           //Identificador numerico de la bebida
	char Nombre[28];        //Nombre de la bebida
	char Descripcion[30];  //Descripcion de la bebida
	int Cantidad;			//Cantidad de bebidas disponibles
	float Precio;           //Precio de la bebida (Precio de provedor)
	float PrecioPublico;	//Precio al que se le vende a las personas
}BEBIDA;

typedef struct {    //Se crea la estructura hora para poder regresar
	char hora[80];    //la hora de la computadora en la funcion ObtenerHora
}HORA;

typedef struct {			//Contiene las ordenes que piden los Clientes
	int IDMovimiento;	    //Identificador de compra/venta de bebidas
	HORA Hora;       //Hora en que se compra/vende producto
	int Cantidad;			//Cantidad de producto comprado/vendido
	BEBIDA Bebida;			//Informacion de la bebida que se compro/vendio
	char TMovimiento[7];		//Tipo de movimiento (compra o venta);
	float Costo;			//Total de ganancia/costo de la transaccion
}MOVIMIENTOS;

typedef struct {			//Contiene la informacion de la bodega
	BEBIDA Bebidas[20];		//Bebidas que van pidiendo en la orden
	int nBebidas;			//Numero de vevidas
	float Fondos;            //Total de dinero que tiene la bodega
	MOVIMIENTOS Movimientos[100];//Compras o ventas que realiza la bodega
	int nMovimientos;		//Contiene el numero de movimientos realizados
}BODEGA;

HORA ObtenerHora(); //Obtiene la hora de la computadora
BODEGA InicializarBodega();  //Inicializa 9 bebidas por default
//BODEGA RealizarMovimiento(BODEGA Bodega);//Realiza una compra o venta
BODEGA RealizarCompra(BODEGA Bodega, int IDBebida, int Cantidad); //Compra bebidas a el Probedor
BODEGA RealizarVenta(BODEGA Bodega, int IDBebida, int Cantidad);	//Vende productos a clientes
void MostrarMovimientos(BODEGA Bodega);//Muestra los movimientos realizados
//Funciones de Bebidas
void MostrarBebidas(BODEGA Bodega);//Muestra todas las vevidas con las que cuenta la bidega
int VerificarBebidaID(BODEGA Bodega, int IDBebida); //Verifica que el ID de la bebida si exista si regresa 1 existe
BODEGA AgregarBebida(BODEGA Bodega); //Agrega una nueva bebida
//Menu
void MostrarOpciones();

int main() {
	BODEGA Bodega;
	int IDBebida;	//Sirve para comprar o vender bebidas
	int id_bandera;	//Sirve para comprobar que el id es correcto
	int Cantidad;	//Cantidad de bebidas que se decean comprar
	int menu;		//Nos ayuda a realizar las distintas acciones
	int menu_bandera = 0;//Indica si seguimos o no en el while

						 //Inicializa la bodega con 9 bebidas y 10 piezas de cada una
	Bodega = InicializarBodega();

	do {
		MostrarOpciones();
		scanf("%d", &menu);
		//Dependiendo de la bariable menu es la accion que se va a realizar
		switch (menu) {
		case 1:
			MostrarBebidas(Bodega);
			break;
		case 2:
			MostrarMovimientos(Bodega);
			break;
		case 3:
			printf("\nCompra\nSeleccione una bebida que dece comprar:\n");
			MostrarBebidas(Bodega);
			do
			{
				scanf("%d", &IDBebida);
				id_bandera = VerificarBebidaID(Bodega, IDBebida);
				if (id_bandera == 1) {
					printf("Cuantas bebidas decea comprar?\t");
					scanf("%d", &Cantidad);
					Bodega = RealizarCompra(Bodega, IDBebida, Cantidad);
				}
				else {
					printf("\nLa bebida seleccionada es incorrecta");
					printf("\nElija una bebida correcta");
				}
			} while (id_bandera == 0);
			break;
		case 4:
			printf("\nVenta\nSeleccione una bebida que dece vender:\n");
			MostrarBebidas(Bodega);
			do
			{
				scanf("%d", &IDBebida);
				id_bandera = VerificarBebidaID(Bodega, IDBebida);
				if (id_bandera == 1) {
					printf("Cuantas bebidas decea vender?\t");
					scanf("%d", &Cantidad);
					Bodega = RealizarVenta(Bodega, IDBebida, Cantidad);
				}
				else {
					printf("\nLa bebida seleccionada es incorrecta");
					printf("\nElija una bebida correcta");
				}
			} while (id_bandera == 0);
			break;
		case 6:
			printf("\nEsta seguro de que quiere salir?");
			printf("\n1.-Si\t2.-No");
			scanf("%d", &menu_bandera);
			break;
		case 5:
			Bodega = AgregarBebida(Bodega);
		default:
			printf("\nNumero ingresado incorrecto");
		}
	} while (menu_bandera != 1);	
	return 0;
}


HORA ObtenerHora() {
	HORA h;
	time_t tiempo;
	struct tm *tmPtr;
	tiempo = time(NULL);
	tmPtr = localtime(&tiempo);
	strftime(h.hora, 80, "%H:%M.%S, %A de %B de %Y", tmPtr);

	return h;
}

BODEGA InicializarBodega() {
	BODEGA Bodega;
	int i;

	strcpy(Bodega.Bebidas[0].Nombre, "Jose Cuervo");
	strcpy(Bodega.Bebidas[0].Descripcion, "Tequila");
	Bodega.Bebidas[0].Precio = 124;
	Bodega.Bebidas[0].PrecioPublico = 144;
	Bodega.Bebidas[0].IDBebida = 1;

	strcpy(Bodega.Bebidas[1].Nombre, "Jimador");
	strcpy(Bodega.Bebidas[1].Descripcion, "Tequila");
	Bodega.Bebidas[1].Precio = 154;
	Bodega.Bebidas[1].PrecioPublico = 174;
	Bodega.Bebidas[1].IDBebida = 2;

	strcpy(Bodega.Bebidas[2].Nombre, "Centenario");
	strcpy(Bodega.Bebidas[2].Descripcion, "Tequila");
	Bodega.Bebidas[2].Precio = 270;
	Bodega.Bebidas[2].PrecioPublico = 290;
	Bodega.Bebidas[2].IDBebida = 3;

	strcpy(Bodega.Bebidas[3].Nombre, "Jack Daniels");
	strcpy(Bodega.Bebidas[3].Descripcion, "Whisky");
	Bodega.Bebidas[3].Precio = 384;
	Bodega.Bebidas[3].PrecioPublico = 404;
	Bodega.Bebidas[3].IDBebida = 4;

	strcpy(Bodega.Bebidas[4].Nombre, "Red Label");
	strcpy(Bodega.Bebidas[4].Descripcion, "Whisky");
	Bodega.Bebidas[4].Precio = 250;
	Bodega.Bebidas[4].PrecioPublico = 270;
	Bodega.Bebidas[4].IDBebida = 5;

	strcpy(Bodega.Bebidas[5].Nombre, "Bucannas");
	strcpy(Bodega.Bebidas[5].Descripcion, "Whisky");
	Bodega.Bebidas[5].Precio = 560;
	Bodega.Bebidas[5].PrecioPublico = 580;
	Bodega.Bebidas[5].IDBebida = 6;

	strcpy(Bodega.Bebidas[6].Nombre, "Bacardy");
	strcpy(Bodega.Bebidas[6].Descripcion, "Ron");
	Bodega.Bebidas[6].Precio = 230;
	Bodega.Bebidas[6].PrecioPublico = 250;
	Bodega.Bebidas[6].IDBebida = 7;

	strcpy(Bodega.Bebidas[7].Nombre, "Habanna Club");
	strcpy(Bodega.Bebidas[7].Descripcion, "Ron");
	Bodega.Bebidas[7].Precio = 280;
	Bodega.Bebidas[7].PrecioPublico = 300;
	Bodega.Bebidas[7].IDBebida = 8;

	strcpy(Bodega.Bebidas[8].Nombre, "Captain Morgan");
	strcpy(Bodega.Bebidas[8].Descripcion, "Ron");
	Bodega.Bebidas[8].Precio = 160;
	Bodega.Bebidas[8].PrecioPublico = 180;
	Bodega.Bebidas[8].IDBebida = 9;

	for (i = 0; i < NUMERO_DE_BEBIDAS_INICIO; i++)
	{
		Bodega.Bebidas[i].Cantidad = 10;
		Bodega.Movimientos[i].Bebida = Bodega.Bebidas[i];
		Bodega.Movimientos[i].Cantidad = 10;
		Bodega.Movimientos[i].Costo = 0;
		Bodega.Movimientos[i].Hora = ObtenerHora();
		Bodega.Movimientos[i].IDMovimiento = i;
		strcpy(Bodega.Movimientos[i].TMovimiento, "Compra");
	}

	Bodega.nBebidas = NUMERO_DE_BEBIDAS_INICIO;
	Bodega.Fondos = 10000;
	Bodega.nMovimientos = i;

	return Bodega;
}

BODEGA RealizarCompra(BODEGA Bodega, int IDBebida, int Cantidad) {
	Bodega.Bebidas[IDBebida - 1].Cantidad += Cantidad;

	Bodega.Movimientos[Bodega.nMovimientos].Bebida = Bodega.Bebidas[IDBebida - 1];
	Bodega.Movimientos[Bodega.nMovimientos].Cantidad = Cantidad;
	Bodega.Movimientos[Bodega.nMovimientos].Costo = (Bodega.Bebidas[IDBebida - 1].Precio)*Cantidad;
	Bodega.Movimientos[Bodega.nMovimientos].Hora = ObtenerHora();
	Bodega.Movimientos[Bodega.nMovimientos].IDMovimiento = Bodega.nMovimientos;
	strcpy(Bodega.Movimientos[Bodega.nMovimientos].TMovimiento, "Compra");

	Bodega.Fondos -= (Bodega.Bebidas[IDBebida - 1].Precio)*Cantidad;
	Bodega.nMovimientos += 1;

	return Bodega;
}

BODEGA RealizarVenta(BODEGA Bodega, int IDBebida, int Cantidad) {
	Bodega.Bebidas[IDBebida - 1].Cantidad -= Cantidad;

	Bodega.Movimientos[Bodega.nMovimientos].Bebida = Bodega.Bebidas[IDBebida - 1];
	Bodega.Movimientos[Bodega.nMovimientos].Cantidad = Cantidad;
	Bodega.Movimientos[Bodega.nMovimientos].Costo = (Bodega.Bebidas[IDBebida - 1].PrecioPublico)*Cantidad;
	Bodega.Movimientos[Bodega.nMovimientos].Hora = ObtenerHora();
	Bodega.Movimientos[Bodega.nMovimientos].IDMovimiento = Bodega.nMovimientos;
	strcpy(Bodega.Movimientos[Bodega.nMovimientos].TMovimiento, "Venta");

	Bodega.Fondos -= (Bodega.Bebidas[IDBebida - 1].PrecioPublico)*Cantidad;
	Bodega.nMovimientos += 1;

	return Bodega;
}

void MostrarMovimientos(BODEGA Bodega) {
	int i;
	printf("\n\n\tLista de Movimientos\n\n");

	printf("\tID\tNombre\t\tCantidad\tTipo\tCosto\tHora\n");
	for (i = 0; i < Bodega.nMovimientos; i++)
	{
		printf("\n\t%d\t%s", Bodega.Movimientos[i].IDMovimiento, Bodega.Movimientos[i].Bebida.Nombre);
		printf(" \t%d\t\t%s", Bodega.Movimientos[i].Cantidad, Bodega.Movimientos[i].TMovimiento);
		printf("\t%.2f\t%s", Bodega.Movimientos[i].Costo, Bodega.Movimientos[i].Hora.hora);
	}
}

//Funciones de bebidas
void MostrarBebidas(BODEGA Bodega) {
	int i;
	printf("\n\n\tLista de Bebidas en la bodega\n");
	printf("\n\tID\tNombre\t\tDescipcion\tCantidad\tPrecio\tPrecio al Publico\n");
	for (i = 0; i < Bodega.nBebidas; i++) {
		printf("\n\t%d\t%s", Bodega.Bebidas[i].IDBebida, Bodega.Bebidas[i].Nombre);
		printf(" \t%s\t\t%d", Bodega.Bebidas[i].Descripcion, Bodega.Bebidas[i].Cantidad);
		printf("\t\t%.2f\t%.2f", Bodega.Bebidas[i].Precio, Bodega.Bebidas[i].PrecioPublico);
	}
}

int VerificarBebidaID(BODEGA Bodega, int IDBebida) {
	int i;
	for (i = 0; i < Bodega.nBebidas; i++) {
		if (Bodega.Bebidas[i].IDBebida == IDBebida) {
			return 1;
		}
	}
	return 0;
}

BODEGA AgregarBebida(BODEGA Bodega) {
	BEBIDA bebida;
	char Nombre[28];
	char Descripcion[100];
	int Precio, PrecioPublico;
	getchar();  //Captura cualquier salto de linea para no causar problemas, se coloca antes de capturaer una cadena
	printf("Cual es el nombre de la bebida?\n", Nombre);
	scanf("%[^\n]", &Bodega.Bebidas[Bodega.nBebidas].Nombre);//Se pone [^\n] para que se valide toda la cadena hasta encontrar un salto de linea
	getchar();
	printf("Cual es la descripcion de la bebida?\n");
	scanf("%[^\n]", &Bodega.Bebidas[Bodega.nBebidas].Descripcion);
	printf("Cual es el precio de la bebida?\n");
	scanf("%f", &Bodega.Bebidas[Bodega.nBebidas].Precio);
	printf("Cual es el precio de la bebida al publico?\n");
	scanf("%f", &Bodega.Bebidas[Bodega.nBebidas].PrecioPublico);
	Bodega.Bebidas[Bodega.nBebidas].IDBebida = Bodega.nBebidas+1;
	Bodega.Bebidas[Bodega.nBebidas].Cantidad = 0;
	Bodega.nBebidas += 1;

	return Bodega;
}

//Menu
void MostrarOpciones() {
	char BarBonito[4][44] =
	{
		"  ___             ___           _ _       ",
		" | _ ) __ _ _ _  | _ ) ___ _ _ (_) |_ ___ ",
		" | _ \\/ _` | '_| | _ \\/ _ \\ ' \\| |  _/ _ \\",
		" |___/\\__,_|_|   |___/\\___/_||_|_|\\__\\___/",
	};

	printf("\n");
	for (int i = 0; i < 4; i++) {
		for (int j = 0; j < 44; j++) {
			printf("%c", BarBonito[i][j]);
		}
		printf("\n");
	}

	/***Font Name: Bigfig
	*                                                                    _
	*    /|    ---   |V| _  _ _|_ __ _  __    _  _  ___|_ _     _| _    |_) _ |_  o  _| _  _
	*     |  o       | |(_)_>  |_ | (_| |    (_ (_| |  |_(_|   (_|(/_   |_)(/_|_) | (_|(_|_>
	*/
	printf("\n\t1.- Mostrar Bebidas en la bodega");
	printf("\n\t2.- Mostrar Movimientos");
	printf("\n\t3.- Realizar Compra");
	printf("\n\t4.- Realizar Venta");
	printf("\n\t5.- Agregar Bebida");
	printf("\n\t6.- Salir");

}

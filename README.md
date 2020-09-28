# Protocolo-BGP
// Este seria EL MAIN DE LA MAQUINA DE ESTADO

#include "mylib.h"

int main()
{
    conexion_t config;
    estados_t estado = idle; // primer estado
    printf("Protocolo BGP\n\r");
    config = f_inicio();
    printf("Configuracion Obtenida t_set = %c, deltaT= %c\n", config.t_set, config.deltaT ); // aqui queria darle valores a config.t_set que sea igual al estado anterior
    // y config.deltaT sea igual al estado actual
    while (1)
    {
        switch (estado)
        {
        case idle:
            estado = f_idle(config);
            break;
        case connect:
            estado = f_connect(config);
            break;
        case open:
            estado = f_open(config);
            break;
        case keepalive:
            estado = f_keepalive(config);
            break;
        case notification:
            estado = f_notification(config);
            break;
        case stablished:
            estado = f_stablished(config);
            break;
        }
    }
    return 0;
}
---------------------------------------------------------------
// Este seria MYLIB

#ifndef MY_LIB
#define MY_LIB
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
 
typedef enum
{
    idle = 0,
    connect = 1,
    open = 2,
    keepalive = 3,
    notification = 4,
    stablished = 5
    
} estados_t;  

typedef struct
{
    char idle;            // sin conexion actual
    char connect;         // conectando
    char open;            // conectado
    char keepalive;       // mantener conexion
    char notification;    // desconectando
    char stablished;      // desconectado
     
    
} conexion_t;

conexion_t f_inicio(void); // lee el archivo de configuraciÃ³n y carga las variables.
estados_t f_idle(conexion_t);
estados_t f_connect(conexion_t);
estados_t f_open(conexion_t);
estados_t f_keepalive(conexion_t);
estados_t f_notification(conexion_t);
estados_t f_stablished(conexion_t);

char *getKey(char *key);
conexion_t parseConfig(char *);

char leerconexion(void);

void conectando(conexion_t);
void desconectando(conexion_t);
void manteniendoconectando(conexion_t);

#endif
------------------------------------------------------------------------------------------
//MIS FUNCIONES


#include "mylib.h"
	
	char *getKey(char *key)
	{
	    char i = 0;
	
	    while (*(key + i) != ' ')
	    {
	        i++;
	    }
	    *(key + i) = 0;
	    return key + i + 1;
	}
	
	conexion_t parseConfig(char *filename){
	    FILE *conf;
	    char cadena[40], *key, *val;
	    char variables[2][20] = {"t_set", "deltaT"}, i;
	    conexion_t config;
	    char flagt_set = 0, flagdeltaT = 0;
	
	    if ((conf = fopen(filename,"rt")) == NULL)  
	    {
	 printf("No se encontro archivo de configuracion\n");
	        config.t = 0;
	        config.t_set = 0;
	        config.deltaT = 0;
	        
	        if(config.t == 0){
	        	printf("Sin conexion\n");
							 }
	        return config;
	    }
	    fgets(cadena, 40, conf);
	    do
	    {
	        key = cadena;
	        if ((*key) != '#' && strlen(key) >= 0)
	        {   
	            val = getKey(key); // modifica a key para que solo contenga la clave, y devuelve
	            
	            for (i = 0; i < 2; i++)
	            { 
	                if (!strcmp(key, variables[i]))
	                {
	                    switch (i)
	                    {
	                    case 0:
	                        config.t_set = atoi(val); // la funcion atoi convierte una cadena en un numero entero (ascii to int)
	                        flagt_set = 1;
	                        break;
	                    case 1:
	                        config.deltaT = atoi(val);
	                        flagdeltaT =1;
	                        break;
	                    }
	                }
	            }
	        }
	        fgets(cadena, 40, conf);
	    } while (!feof(conf));
	    if(flagt_set && flagdeltaT) {
	        
	        return config;
	    } else {
	        config.t = 0;
	        config.t_set = 0;
	        config.deltaT = 0;
	        printf("Sin conexion\n");
	        
	        return config;
	    }    
	}
	    
	char leerconexion(void){
	    char conex;
	    
	    printf("\r Desea conectarse al Protocolo BGP? \n");
	    scanf("%c", &conex);
	    
	 switch (conex)
	        {
	        case 'n':
	            estado = f_idle(config);
	            break;
	        case 's':
	            estado = f_connect(config);
	            break;   
	    
		return (char)conex;    
			}
	
	void conectando(conexion_t conexion)
	{
	    printf("\r Conectado al procolo BGP");
	} 
------------------------------------------------------------------------------------------
// Mis archivos


#include "mylib.h"

conexion_t f_inicio(void){ // lee el archivo de configuraciÃ³n y carga las variables.
    conexion_t config;
    config = parseConfig("config.conf");
    return config;
}
estados_t f_idle(conexion_t config){
    estados_t estado = idle;    
    config.t = leerconexion();
    printf("\rSe encuentra en estado IDLE");
    return estado;

}
estados_t f_connect(conexion_t config){
    estados_t estado = connect;
    config.t = leerconexion();
    conectando(config);
    
    return (conectado);
} 
/*
estados_t f_open(conexion_t config){
    estados_t estado = open;    
    config.t = leerconexion();
    printf("\r Estado Open");
    
    
}
estados_t f_keepalive(conexion_t config){
    estados_t estado = keepalive;    
    config.t = leerconexion();
    printf("\rManteniendo conexion");
    
    
}
estados_t f_notification(conexion_t config){
    estados_t estado = notification;    
    config.t = leerconexion();
    printf("\rFinalizando Conexion");
    
    
}
estados_t f_stablished(conexion_t config){
    estados_t estado = stablished;    
    config.t = leerconexion();
    printf("\rEn espera");
    
   
} 
*/





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






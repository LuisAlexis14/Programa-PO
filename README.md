# Programa-PO
Software de calculo de enlaces modelos de propagación
/*
UNIVERSIDAD TECNICA DEL NORTE
FICA-CITEL
LUIS ALEXIS GUEVARA SOLARTE
09-01-2024
PROPAGACION DE ONDAS
PROGRAMA PARA REALIZAR CALCULOS DE MODELO DE PROPAGACI0N DE ONDAS
*/
#include <stdio.h>

#include <math.h>

void modeloExponencial();
void modeloOkumuraHata();

void modeloExponencial() {
    printf("El modelo exponencial de perdida en el espacio involucra un factor 'n' con valores tipicos en diversos entornos:\n");
    printf("Ingrese el valor de 'n' basandose en la tabla correspondiente:\n");
    double exponenteN;
    printf("Ingrese el valor de 'n': ");
    scanf("%lf", &exponenteN);

    double distanciaAntenas, frecuencia;
    printf("Ingrese la distancia entre antenas medida en metros: ");
    scanf("%lf", &distanciaAntenas);
    printf("Ingrese la frecuencia en Hz: ");
    scanf("%lf", &frecuencia);

    double factor = 10 * exponenteN;
    double logDistancia = log10(distanciaAntenas);
    double logFrecuencia = log10(frecuencia);

    double perdidaEspacio = (20 * logFrecuencia) + (factor * logDistancia) - 147.56;

    printf("La perdida en el espacio con exponente es: L(dB) = %.2lf\n", perdidaEspacio);
}

void modeloOkumuraHata() {
    printf("Este modelo se utiliza en entornos urbanos y suburbanos.\n");
    double frecuencia, alturaTransmision, alturaRecepcion, distanciaAntenas;
    printf("Ingrese la frecuencia en MHz: ");
    scanf("%lf", &frecuencia);
    printf("Ingrese la altura de la antena de transmision en metros: ");
    scanf("%lf", &alturaTransmision);
    printf("Ingrese la altura de la antena de recepcion en metros: ");
    scanf("%lf", &alturaRecepcion);
    printf("Ingrese la distancia entre las antenas en Km (1km-20Km): ");
    scanf("%lf", &distanciaAntenas);

    double perdida = 0;
    if (frecuencia <= 300) {
        double Ahr = 8.29 * (pow(log10(1.54 * alturaRecepcion), 2)) - 1.1;
        perdida = 69.55 + (26.56 * log10(frecuencia)) - (13.8 * log10(alturaTransmision)) - Ahr + (log10(distanciaAntenas) * (44.9 - (6.55 * log10(alturaTransmision))));
    } else if (frecuencia > 300 && frecuencia < 1500) {
        double Ahr = 3.2 * (pow(log10(11.75 * alturaRecepcion), 2)) - 4.97;
        perdida = 69.55 + (26.56 * log10(frecuencia)) - (13.8 * log10(alturaTransmision)) - Ahr + (log10(distanciaAntenas) * (44.9 - (6.55 * log10(alturaTransmision))));
    } else if (frecuencia >= 1500 && frecuencia <= 2000) {
        double Ahr = 3.2 * (pow(log10(11.75 * alturaRecepcion), 2)) - 4.97;
        int tipoEntorno;
        printf("Elija modelo Suburbano(1) o ciudad grande(2): ");
        scanf("%d", &tipoEntorno);
        double correccionModelo = 0;
        if (tipoEntorno == 2) {
            correccionModelo = correccionModelo + 3;
        }
        perdida = 46.3 + (33.9 * log10(frecuencia)) - (13.82 * log10(alturaTransmision)) - Ahr + (log10(distanciaAntenas) * (44.9 - (6.55 * log10(alturaTransmision)))) + correccionModelo;
    }

    printf("La perdida es: L(dB) = %.2lf\n", perdida);
}

int main() {
    char opcion;

    do {
        printf("Bienvenido\n");
        printf("Elija el modelo de propagacion:\n");
        printf("1) Modelo de Propagacion - Exponencial de Perdida\n");
        printf("2) Modelo de Propagacion - Okumura Hata\n");

        int eleccionModelo;
        printf("Escoja el modelo (1, 2): ");
        scanf("%d", &eleccionModelo);

        while (eleccionModelo != 1 && eleccionModelo != 2) {
            printf("Escoja un modelo de la lista entre (1, 2): ");
            scanf("%d", &eleccionModelo);
        }

        if (eleccionModelo == 1) {
            modeloExponencial();
        } else if (eleccionModelo == 2) {
            modeloOkumuraHata();
        }

        printf("¿Desea realizar otro calculo? (a para si, q para salir): ");
        scanf(" %c", &opcion);

    } while (opcion == 'a' || opcion == 'A');

    printf("Gracias por utilizar el programa.");

    return 0;
}

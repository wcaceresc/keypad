# keypad
Keypad con display de 7 segmentos
/* UNIVERSIDAD MARIANO GALVEZ
 * MICROCONTROLADORES Y MICROPROCESADORES
 * ING VICTOR VARGAS
 *
 * PROYECTO KEYPAD
 * WALTER CACERES
 * 091-00-2908
*/

#include <stdint.h>
#include "stm32l053xx.h"

void ky_filas(); //Funcion para las filas
void ky_col();   //Funcion para las columnas
void disp7(uint8_t); //Display 7 Var Guar
void delayMs(int n);
uint16_t last_number = 0;

void init();

       //Variable temporal para guardar numero presionado
       uint8_t btn_press = 0;

int main(void)
 {
   init();
   while(1)
       {
   	     ky_filas();
	     ky_col();
       }
 }


void init()

   {
    //HABILITA LOS CLOCKS DE A,B Y C RESPECTIVAMENTE
	  RCC->IOPENR |=0x01<<0;
	  RCC->IOPENR |=0x01<<1;
	  RCC->IOPENR |=0x01<<2;

    //PUERTOS QUE SE VAN A USAR PARA CADA SEGMENTO
       GPIOB->MODER &=~(1<<17);	//PB8 A
	   GPIOB->MODER &=~(1<<5);	//PB2 B
	   GPIOB->MODER &=~(1<<7);	//PB3 C
	   GPIOB->MODER &=~(1<<9);	//PB4 D
	   GPIOB->MODER &=~(1<<13);	//PB6 E
	   GPIOB->MODER &=~(1<<15);	//PB7 F
	   GPIOB->MODER &=~(1<<19);	//PB9 G


   // Comun del display y Limpiador de PA1
	   GPIOA->MODER &=~(1<<3);
	   GPIOA->BSRR = (0x03<<16);

}
void ky_filas(){

	//Filas como IN pull up PC5, PC6, PC7, PC8.

	GPIOC->MODER &=~(1<<10);
	GPIOC->MODER &=~(1<<11);
	GPIOC->PUPDR |=(1<<10);

	GPIOC->MODER &=~(1<<12);
	GPIOC->MODER &=~(1<<13);
	GPIOC->PUPDR |=(1<<12);

	GPIOC->MODER &=~(1<<16);
	GPIOC->MODER &=~(1<<17);
	GPIOC->PUPDR |=(1<<16);

	GPIOC->MODER &=~(1<<18);
	GPIOC->MODER &=~(1<<19);
	GPIOC->PUPDR |=(1<<18);

	//Columnas como OUT PB11,PC7,PA9,PC4.

	GPIOB->MODER &=~(1<<23);
	GPIOC->MODER &=~(1<<15);
	GPIOA->MODER &=~(1<<19);
	GPIOC->MODER &=~(1<<9);
}

void ky_col() // Recorrera las columans 0,1,2,3.

     {
    	  //0
	      GPIOB->ODR &=~(1<<11);
	      GPIOC->ODR |=(1<<7);
	      GPIOA->ODR |=(1<<9);
	      GPIOC->ODR |=(1<<4);
	      delayMs(5);

	         if(!(GPIOC->IDR & 0x20))
	         {

	        	disp7(0x01);
            	GPIOA->ODR |=(1<<1);
		        delayMs(5);
		        last_number = 0x01;
	         }
	             else if(!(GPIOC->IDR & 0x40))
	                   {

	            	    disp7(0x04);
		                GPIOA->ODR |=(1<<1);
		                delayMs(5);
		                last_number = 0x04;

	                   }
	                   else if(!(GPIOC->IDR & 0x100))
	                         {

	                	      disp7(0x07);
		                      GPIOA->ODR |=(1<<1);
		                      delayMs(5);
                              last_number = 0x07;
	                         }
	                         else if(!(GPIOC->IDR & 0x200))
	                         {

	                          disp7(last_number);
	                          GPIOA->ODR |=(1<<1);
	                  		  delayMs(5);

	                         }

	GPIOB->ODR |=(1<<11);
	GPIOC->ODR &=~(1<<7);
	GPIOA->ODR |=(1<<9);
	GPIOC->ODR |=(1<<4);
	delayMs(5);

	          if(!(GPIOC->IDR & 0x20))
	            {
            	 disp7(0x02);
		         GPIOA->ODR |=(1<<1);
		         delayMs(5);
                }
	             else if(!(GPIOC->IDR & 0x40))
	                    {
		  			     disp7(0x05);
		                 GPIOA->ODR |=(1<<1);
		                 delayMs(5);
		                }
	                    else if(!(GPIOC->IDR & 0x100))
	                           {
		   		                disp7(0x08);
		                        GPIOA->ODR |=(1<<1);
		                        delayMs(5);
			                   }
	                            else if(!(GPIOC->IDR & 0x200))
	                                   {
                                        disp7(0x00);
		                                GPIOA->ODR |=(1<<1);
		                                delayMs(5);
	                                   }

	GPIOB->ODR |=(1<<11);
	GPIOC->ODR |=(1<<7);
	GPIOA->ODR &=~(1<<9);
	GPIOC->ODR |=(1<<4);
	delayMs(5);

	if(!(GPIOC->IDR & 0x20))
	  {
		disp7(0x03);
		GPIOA->ODR |=(1<<1);
		delayMs(5);
	  }
	       else if(!(GPIOC->IDR & 0x40))
	              {
                   disp7(0x06);
		           GPIOA->ODR |=(1<<1);
		           delayMs(5);
			      }
	              else if(!(GPIOC->IDR & 0x100))
	                     {
	            		  disp7(0x09);
		                  GPIOA->ODR |=(1<<1);
		                  delayMs(5);
		                 }
	                     else if(!(GPIOC->IDR & 0x200))
	                            {
                         		 disp7(0xFF);
		                         GPIOA->ODR |= (1 << 1);
				                 delayMs(5);
		                   	    }

	GPIOB->ODR |=(1<<11);
	GPIOC->ODR |=(1<<7);
	GPIOA->ODR |=(1<<9);
	GPIOC->ODR &=~(1<<4);
	delayMs(5);
	                   if(!(GPIOC->IDR & 0x20))
                         {
		            	  disp7(0x0A);
		                  GPIOA->ODR |=(1<<1);
		                  delayMs(5);
		                 }
	                      else if(!(GPIOC->IDR & 0x40))
	                             {
		                		  disp7(0x0B);
		                          GPIOA->ODR |=(1<<1);
		                          delayMs(5);
		                       	 }
	                          	 else if(!(GPIOC->IDR & 0x100))
	                                    {
		                           		 disp7(0x0C);
		                                 GPIOA->ODR |=(1<<1);
		                                 delayMs(5);
		                              	}
	                                    else if(!(GPIOC->IDR & 0x200))
	                                           {
		                               		    disp7(0x0D);
		                                        GPIOA->ODR &=~(1<<1);
		                                        delayMs(5);
	                                           }
} //Fin de ky_col

void disp7(uint8_t val)
{





   btn_press = val;
   GPIOB->BSRR = (0x3DC << 16); // RESETEA TODOS LOS BITS A 0








//Maquina de estados para el despliegue de los numeros o letras
     switch (val)
        {
        case 0x00:
            GPIOB->ODR |= 0x01 << 8;
            GPIOB->ODR |= 0x01 << 2;
            GPIOB->ODR |= 0x01 << 3;
            GPIOB->ODR |= 0x01 << 4;
            GPIOB->ODR |= 0x01 << 6;
            GPIOB->ODR |= 0x01 << 7;
            break;
        case 0x01:
            GPIOB->ODR |= 0x01 << 2;
            GPIOB->ODR |= 0x01 << 3;
            break;
        case 0x02:
            GPIOB->ODR |= 0x01 << 8;
            GPIOB->ODR |= 0x01 << 2;
            GPIOB->ODR |= 0x01 << 9;
            GPIOB->ODR |= 0x01 << 6;
            GPIOB->ODR |= 0x01 << 4;
            break;
        case 0x03:
            GPIOB->ODR |= 0x01 << 8;
            GPIOB->ODR |= 0x01 << 2;
            GPIOB->ODR |= 0x01 << 9;
            GPIOB->ODR |= 0x01 << 3;
            GPIOB->ODR |= 0x01 << 4;
            break;
        case 0x04:
            GPIOB->ODR |= 0x01 << 7;
            GPIOB->ODR |= 0x01 << 9;
            GPIOB->ODR |= 0x01 << 2;
            GPIOB->ODR |= 0x01 << 3;
            break;
        case 0x05:
            GPIOB->ODR |= 0x01 << 8;
            GPIOB->ODR |= 0x01 << 7;
            GPIOB->ODR |= 0x01 << 9;
            GPIOB->ODR |= 0x01 << 3;
            GPIOB->ODR |= 0x01 << 4;
            break;
        case 0x06:
            GPIOB->ODR |= 0x01 << 7;
            GPIOB->ODR |= 0x01 << 6;
            GPIOB->ODR |= 0x01 << 4;
            GPIOB->ODR |= 0x01 << 3;
            GPIOB->ODR |= 0x01 << 9;
            break;
        case 0x07:
            GPIOB->ODR |= 0x01 << 8;
            GPIOB->ODR |= 0x01 << 2;
            GPIOB->ODR |= 0x01 << 3;
            break;
        case 0x08:
            GPIOB->ODR |= 0x01 << 8;
            GPIOB->ODR |= 0x01 << 2;
            GPIOB->ODR |= 0x01 << 3;
            GPIOB->ODR |= 0x01 << 4;
            GPIOB->ODR |= 0x01 << 6;
            GPIOB->ODR |= 0x01 << 7;
            GPIOB->ODR |= 0x01 << 9;
            break;
        case 0x09:
            GPIOB->ODR |= 0x01 << 9;
            GPIOB->ODR |= 0x01 << 7;
            GPIOB->ODR |= 0x01 << 8;
            GPIOB->ODR |= 0x01 << 2;
            GPIOB->ODR |= 0x01 << 3;
            break;
        case 0x0A:
            GPIOB->ODR |= 0x01 << 8;
            GPIOB->ODR |= 0x01 << 2;
            GPIOB->ODR |= 0x01 << 3;
            GPIOB->ODR |= 0x01 << 6;
            GPIOB->ODR |= 0x01 << 7;
            GPIOB->ODR |= 0x01 << 9;
            break;
        case 0x0B:
            GPIOB->ODR |= 0x01 << 3;
            GPIOB->ODR |= 0x01 << 4;
            GPIOB->ODR |= 0x01 << 6;
            GPIOB->ODR |= 0x01 << 7;
            GPIOB->ODR |= 0x01 << 9;
            break;
        case 0x0C:
            GPIOB->ODR |= 0x01 << 8;
            GPIOB->ODR |= 0x01 << 4;
            GPIOB->ODR |= 0x01 << 6;
            GPIOB->ODR |= 0x01 << 7;
            break;
        case 0x0D:
            GPIOB->ODR |= 0x01 << 2;
            GPIOB->ODR |= 0x01 << 3;
            GPIOB->ODR |= 0x01 << 4;
            GPIOB->ODR |= 0x01 << 6;
            GPIOB->ODR |= 0x01 << 9;
            break;


    }
}


void delayMs(int n)

   {
	int i = 0;
	 while (n > 0)
	   {
	    n--;
	    i = 0;
	     while (i < 240)
	       {
	        i++;
      	   }
	   }
   }





**Implement do colorimetro RGB**

  

Nesse passo do CDIO, que é o Implement, iremos descrever um pouco sobre a estrutura do nosso projeto, como código, placa e resultados dos testes realizados com alguns padrões de cor.

Primeiramente iremos falar sobre o nosso código, que foi feito em forma máquina de estados utilizando a ferramenta switch case.

Abaixo segue o código principal completo:

**Main.c**

/*--------------------Inclusão  de  periféricos  usados no modulo--------------------------------*/

#include  <stdio.h>

#include  "def_principais.h"

#include  "gpio.h"

#include  "adc.h"

#include  "timer.h"

#include  "lcd.h"

#include  "I2C.h"

#include  "24LC512.h"

#include  "UART.h"

#include  "math.h"

/*------funções  usadas  pelo Main----------------------*/

void  configPwm(void);

uint16_t  ler_ldr(led_pwm_t led);

uint16_t  calibra(led_pwm_t led);

float  mede(led_pwm_t led, uint16_t *br);

float  calc_ldr(led_pwm_t led);

void  salva_resultado(char* cor,float amostra);

/*----------------------------------------------------*/

int  main(void)

{

  

/*---------------------------Variaveis  locais do main-------------------------------------*/

stats_prog_t sts_prog = CAL_EQUIP;

uint16_t brancoR,brancoG,brancoB;

float amostra;

char result_buff[6];

/*---------------------------Iniciando  periféricos----------------------------------------*/

timeBaseInit();

gpioInit();

uart_pin_config();

uart_init_9600Bound();

lcdInit();

I2CInit();

adcSingleInit();

/*-------------------------------Tela  de  Intro------------------------------------------*/

lcdSetPos(1,3);

lcdStringWrite("Colorimetro");

lcdSetPos(2,7);

lcdStringWrite("RGB");

delayMs(2500);

/*----------------------------Menu secreto  de  ajuste  de  fabrica---------------------------*/

if((BTN_UP_is_press() == BTN_PRESS) && (BTN_DN_is_press() == BTN_PRESS)){

lcdClear();

configPwm();

}

lcdClear();

/*-----------------------------Fim  de  inicialização---------------------------------------*/

while(1){

switch(sts_prog)

{

case  CAL_EQUIP:

if(BTN_UP_is_press()==BTN_PRESS)

{

sts_prog = LEITURAS;

}

if(BTN_DN_is_press()==BTN_PRESS)

{

sts_prog = ENVIAR_DADOS;

}

/*tela  de  calibração*/

lcdSetPos(1,1);

lcdStringWrite(" Calibrar ");

lcdSetPos(2,1);

lcdStringWrite(" Equipamento ");

/*------------------------------------------*/

if(BTN_OK_is_press()==BTN_PRESS)

{

stats_cal_t sts_cal = CAL_LEDS;

while(BTN_BK_is_press()==BTN_NO_PRESS)

{

switch(sts_cal)

{

case  CAL_LEDS:

if(BTN_UP_is_press()==BTN_PRESS)

{

sts_cal = CAL_ESPECIFICO;

}

if(BTN_DN_is_press()==BTN_PRESS)

{

sts_cal = CAL_ESPECIFICO;

}

/*tela  calibração  de  todos  os  leds*/

lcdSetPos(1,1);

lcdStringWrite(" Calibrar ");

lcdSetPos(2,1);

lcdStringWrite(" Todos  os LEDS ");

/*------------------------------------------*/

if (BTN_OK_is_press()==BTN_PRESS)

{

  

/*executa  calibração  dos  leds*/

lcdSetPos(1,1);

lcdStringWrite(" Calibrando... ");

lcdSetPos(2,1);

lcdStringWrite(" ");

brancoR = calibra(PWM_RED);

brancoG = calibra(PWM_GREEN);

brancoB = calibra(PWM_BLUE);

  

/*Mensagem do fim  da  calibração*/

lcdSetPos(1,1);

lcdStringWrite(" Todos  os LEDS ");

lcdSetPos(2,1);

lcdStringWrite(" Calibrados! ");

/*------------------------------------------*/

delayMs(1000);/*palsa  para  efeito visual*/

}

  

break;

case  CAL_ESPECIFICO:

if(BTN_UP_is_press()==BTN_PRESS)

{

sts_cal = CAL_LEDS;

}

if(BTN_DN_is_press()==BTN_PRESS)

{

sts_cal = CAL_LEDS;

}

/*tela  de  calibração  para led especifico*/

lcdSetPos(1,1);

lcdStringWrite(" Calibrar ");

lcdSetPos(2,1);

lcdStringWrite(" LED especifico ");

/*------------------------------------------*/

  

if(BTN_OK_is_press()==BTN_PRESS)

{

cal_especif_t cal_esp = CAL_VERMELHO;

while(BTN_BK_is_press()==BTN_NO_PRESS)

{

switch(cal_esp)

{

case  CAL_VERMELHO:

if(BTN_UP_is_press()==BTN_PRESS)

{

cal_esp = CAL_VERDE;

}

if(BTN_DN_is_press()==BTN_PRESS)

{

cal_esp = CAL_AZUL;

}

/*tela  de  calibração  para led especifico*/

lcdSetPos(1,1);

lcdStringWrite(" Calibrar ");

lcdSetPos(2,1);

lcdStringWrite(" LED Vermelho ");

/*------------------------------------------*/

  

if(BTN_OK_is_press()==BTN_PRESS)

{

stats_cor_t sts_cor = IN_PROGRESS;

while(sts_cor == IN_PROGRESS)

{

/*executa a calibração*/

  

lcdSetPos(1,1);

lcdStringWrite(" Calibrando... ");

lcdSetPos(2,1);

lcdStringWrite(" ");

brancoR = calibra(PWM_RED);

lcdSetPos(1,1);

lcdStringWrite(" Calibrado! ");

lcdSetPos(2,1);

lcdStringWrite(" ");

delayMs(1000);/*palsa  para  efeito visual*/

sts_cor = FINISHED;

}

  

}

break;

case  CAL_VERDE:

if(BTN_UP_is_press()==BTN_PRESS)

{

cal_esp = CAL_AZUL;

}

if(BTN_DN_is_press()==BTN_PRESS)

{

cal_esp = CAL_VERMELHO;

}

/*tela  de  calibração  para led especifico*/

lcdSetPos(1,1);

lcdStringWrite(" Calibrar ");

lcdSetPos(2,1);

lcdStringWrite(" LED Verde ");

/*------------------------------------------*/

  

if(BTN_OK_is_press()==BTN_PRESS)

{

stats_cor_t sts_cor = IN_PROGRESS;

while(sts_cor == IN_PROGRESS)

{

/*executa a calibração*/

  

lcdSetPos(1,1);

lcdStringWrite(" Calibrando... ");

lcdSetPos(2,1);

lcdStringWrite(" ");

brancoG = calibra(PWM_GREEN);

lcdSetPos(1,1);

lcdStringWrite(" Calibrado! ");

lcdSetPos(2,1);

lcdStringWrite(" ");

delayMs(1000);/*pausa  para  efeito visual*/

sts_cor = FINISHED;

}

  

}

break;

case  CAL_AZUL:

if(BTN_UP_is_press()==BTN_PRESS)

{

cal_esp = CAL_VERMELHO;

}

if(BTN_DN_is_press()==BTN_PRESS)

{

cal_esp = CAL_VERDE;

}

/*tela  de  calibração  para led especifico*/

lcdSetPos(1,1);

lcdStringWrite(" Calibrar ");

lcdSetPos(2,1);

lcdStringWrite(" LED Azul ");

/*------------------------------------------*/

  

if(BTN_OK_is_press()==BTN_PRESS)

{

stats_cor_t sts_cor = IN_PROGRESS;

while(sts_cor == IN_PROGRESS)

{

/*executa a calibração*/

  

lcdSetPos(1,1);

lcdStringWrite(" Calibrando... ");

lcdSetPos(2,1);

lcdStringWrite(" ");

brancoB = calibra(PWM_BLUE);

lcdSetPos(1,1);

lcdStringWrite(" Calibrado! ");

lcdSetPos(2,1);

lcdStringWrite(" ");

delayMs(1000);/*palsa  para  efeito visual*/

sts_cor = FINISHED;

  

}

  

}

break;

}

}

  

}

  

}

  

}

}

  

break;

  

case  LEITURAS:

  

if(BTN_UP_is_press()==BTN_PRESS)

{

sts_prog = ENVIAR_DADOS;

}

if(BTN_DN_is_press()==BTN_PRESS)

{

sts_prog = CAL_EQUIP;

}

/* Tela  de  leitura*/

lcdSetPos(1,1);

lcdStringWrite(" Iniciar ");

lcdSetPos(2,1);

lcdStringWrite(" leituras ");

/*-----------------------------------*/

if(BTN_OK_is_press()==BTN_PRESS)

{

stats_mede_t sts_mede = MEDE_VERMELHO;

while(BTN_BK_is_press()==BTN_NO_PRESS)

{

switch(sts_mede)

{

case  MEDE_VERMELHO:

if(BTN_UP_is_press()==BTN_PRESS)

{

sts_mede = MEDE_VERDE;

}

if(BTN_DN_is_press()==BTN_PRESS)

{

sts_mede = MEDE_AZUL;

}

/* Tela  de  leitura*/

lcdSetPos(1,1);

lcdStringWrite(" LED ");

lcdSetPos(2,1);

lcdStringWrite(" Vermelho ");

/*-----------------------------------*/

  

if(BTN_OK_is_press()==BTN_PRESS)

{

stats_cor_t sts_cor = IN_PROGRESS;

while(sts_cor != FINISHED)

{

if(sts_cor == IN_PROGRESS)

{

/*chama a função  de  medida*/

  

/*tela  de  espera*/

lcdSetPos(1,1);

lcdStringWrite(" Medindo... ");

lcdSetPos(2,1);

lcdStringWrite(" ");

amostra = mede(PWM_RED,&brancoR);

ftoa(result_buff,amostra);

sts_cor = RESULT;

while(sts_cor == RESULT)

{

/*apresenta  resultados*/

lcdSetPos(1,1);

lcdStringWrite("Vermelho: ");

lcdSetPos(2,1);

lcdStringWrite("ABS=");

lcdSetPos(2,5);

lcdStringWrite(result_buff);

if(BTN_OK_is_press()==BTN_PRESS)

{

sts_cor = IN_PROGRESS;

}

if(BTN_BK_is_press()==BTN_PRESS)

{

sts_cor = FINISHED;

}

}

}

}

}

break;

case  MEDE_VERDE:

if(BTN_UP_is_press()==BTN_PRESS)

{

sts_mede = MEDE_AZUL;

}

if(BTN_DN_is_press()==BTN_PRESS)

{

sts_mede = MEDE_VERMELHO;

}

/* Tela  de  leitura*/

lcdSetPos(1,1);

lcdStringWrite(" LED ");

lcdSetPos(2,1);

lcdStringWrite(" Verde ");

/*-----------------------------------*/

if(BTN_OK_is_press()==BTN_PRESS)

{

stats_cor_t sts_cor = IN_PROGRESS;

while(sts_cor != FINISHED)

{

if(sts_cor == IN_PROGRESS)

{

/*chama a função  de  medida*/

lcdSetPos(1,1);

lcdStringWrite(" Medindo... ");

lcdSetPos(2,1);

lcdStringWrite(" ");

amostra = mede(PWM_GREEN,&brancoG);

ftoa(result_buff,amostra);

sts_cor = RESULT;

while(sts_cor == RESULT)

{

/*apresenta  resultados*/

lcdSetPos(1,1);

lcdStringWrite("Verde: ");

lcdSetPos(2,1);

lcdStringWrite("ABS=");

lcdSetPos(2,5);

lcdStringWrite(result_buff);

if(BTN_OK_is_press()==BTN_PRESS)

{

sts_cor = IN_PROGRESS;

}

if(BTN_BK_is_press()==BTN_PRESS)

{

sts_cor = FINISHED;

}

}

}

}

}

break;

  

case  MEDE_AZUL:

if(BTN_UP_is_press()==BTN_PRESS)

{

sts_mede = MEDE_VERMELHO;

}

if(BTN_DN_is_press()==BTN_PRESS)

{

sts_mede = MEDE_VERDE;

}

/* Tela  de  leitura*/

lcdSetPos(1,1);

lcdStringWrite(" LED ");

lcdSetPos(2,1);

lcdStringWrite(" Azul ");

/*-----------------------------------*/

if(BTN_OK_is_press()==BTN_PRESS)

{

stats_cor_t sts_cor = IN_PROGRESS;

while(sts_cor != FINISHED)

{

if(sts_cor == IN_PROGRESS)

{

/*chama a função  de  medida*/

lcdSetPos(1,1);

lcdStringWrite(" Medindo... ");

lcdSetPos(2,1);

lcdStringWrite(" ");

amostra = mede(PWM_BLUE,&brancoB);

ftoa(result_buff,amostra);

sts_cor = RESULT;

while(sts_cor == RESULT)

{

/*apresenta  resultados*/

lcdSetPos(1,1);

lcdStringWrite("Azul: ");

lcdSetPos(2,1);

lcdStringWrite("ABS=");

lcdSetPos(2,5);

lcdStringWrite(result_buff);

if(BTN_OK_is_press()==BTN_PRESS)

{

sts_cor = IN_PROGRESS;

}

if(BTN_BK_is_press()==BTN_PRESS)

{

sts_cor = FINISHED;

}

}

}

}

}

break;

}

  

}

  

}

  

break;

  

case  ENVIAR_DADOS:

if(BTN_UP_is_press()==BTN_PRESS)

{

sts_prog = CAL_LEDS;

}

if(BTN_DN_is_press()==BTN_PRESS)

{

sts_prog = LEITURAS;

}

lcdSetPos(1,1);

lcdStringWrite(" Enviar ");

lcdSetPos(2,1);

lcdStringWrite(" Resultados ");

  

if(BTN_OK_is_press()==BTN_PRESS)

{

lcdSetPos(1,1);

lcdStringWrite(" Enviando ");

lcdSetPos(2,1);

lcdStringWrite(" Dados... ");

delayMs(1000);

}

  

break;

}

}

return 0;

}

void  configPwm(void)

{

stats_pwm_config_t sts_pwm = DUTTY_VERMELHO;

int8_t dutty;

char dutty_buffer[3],ldr_buffer[9];

while(BTN_BK_is_press()==BTN_NO_PRESS)

{

switch(sts_pwm)

{

case  DUTTY_VERMELHO:

if(BTN_UP_is_press()==BTN_PRESS)

{

sts_pwm = DUTTY_VERDE;

}

if(BTN_DN_is_press()==BTN_PRESS)

{

sts_pwm = DUTTY_AZUL;

}

lcdSetPos(1,1);

lcdStringWrite(" Vermelho ");

lcdSetPos(2,1);

lcdStringWrite(" ");

if(BTN_OK_is_press()==BTN_PRESS)

{

stats_cor_t sts_cor = IN_PROGRESS;

EEPROM_readRandomByte(RED_DUTTY_CELL,&dutty);

sprintf(dutty_buffer,"%d",dutty);

ftoa(ldr_buffer,calc_ldr(PWM_RED));

/*imprime  dutty  atualizado*/

lcdSetPos(1,1);

lcdStringWrite("LDR = ");

lcdSetPos(1,7);

lcdStringWrite(ldr_buffer);

lcdSetPos(2,1);

lcdStringWrite("dutty = ");

lcdSetPos(2,8);

lcdStringWrite(dutty_buffer);

lcdSetPos(2,11);

lcdStringWrite("%");

while(sts_cor != FINISHED)

{

if(BTN_UP_is_press()==BTN_PRESS)

{

dutty++;

if(dutty > 100)

{

dutty = 1;

}

/*imprime  dutty  atualizado*/

sprintf(dutty_buffer,"%d",dutty);

/*imprime  dutty  atualizado*/

ftoa(ldr_buffer,calc_ldr(PWM_RED));

/*imprime  dutty  atualizado*/

lcdSetPos(1,1);

lcdStringWrite("LDR = ");

lcdSetPos(1,7);

lcdStringWrite(ldr_buffer);

lcdSetPos(2,1);

lcdStringWrite("dutty = ");

lcdSetPos(2,8);

lcdStringWrite(dutty_buffer);

lcdSetPos(2,11);

lcdStringWrite("%");

}

if(BTN_DN_is_press()==BTN_PRESS)

{

dutty--;

if(dutty < 1)

{

dutty = 100;

}

sprintf(dutty_buffer,"%d",dutty);

/*imprime  dutty  atualizado*/

ftoa(ldr_buffer,calc_ldr(PWM_RED));

/*imprime  dutty  atualizado*/

lcdSetPos(1,1);

lcdStringWrite("LDR = ");

lcdSetPos(1,7);

lcdStringWrite(ldr_buffer);

lcdSetPos(2,1);

lcdStringWrite("dutty = ");

lcdSetPos(2,8);

lcdStringWrite(dutty_buffer);

lcdSetPos(2,11);

lcdStringWrite("%");

}

if(BTN_OK_is_press()==BTN_PRESS)

{

/*salva o dutty*/

EEPROM_writeByte(RED_DUTTY_CELL,dutty);

lcdSetPos(1,1);

lcdStringWrite(" Vermelho ");

lcdSetPos(2,1);

lcdStringWrite(" Salvo ");

delayMs(1000);//para  efeito visual

sts_cor = FINISHED;

}

  

}

}

break;

case  DUTTY_VERDE:

if(BTN_UP_is_press()==BTN_PRESS)

{

sts_pwm = DUTTY_AZUL;

}

if(BTN_DN_is_press()==BTN_PRESS)

{

sts_pwm = DUTTY_VERMELHO;

}

lcdSetPos(1,1);

lcdStringWrite(" Verde ");

lcdSetPos(2,1);

lcdStringWrite(" ");

if(BTN_OK_is_press()==BTN_PRESS)

{

stats_cor_t sts_cor = IN_PROGRESS;

EEPROM_readRandomByte(GREEN_DUTTY_CELL,&dutty);

sprintf(dutty_buffer,"%d",dutty);

/*imprime  dutty  atualizado*/

ftoa(ldr_buffer,calc_ldr(PWM_GREEN));

/*imprime  dutty  atualizado*/

lcdSetPos(1,1);

lcdStringWrite("LDR = ");

lcdSetPos(1,7);

lcdStringWrite(ldr_buffer);

lcdSetPos(2,1);

lcdStringWrite("dutty = ");

lcdSetPos(2,8);

lcdStringWrite(dutty_buffer);

lcdSetPos(2,11);

lcdStringWrite("%");

while(sts_cor != FINISHED)

{

if(BTN_UP_is_press()==BTN_PRESS)

{

dutty++;

if(dutty > 100)

{

dutty = 1;

}

/*imprime  dutty  atualizado*/

sprintf(dutty_buffer,"%d",dutty);

/*imprime  dutty  atualizado*/

ftoa(ldr_buffer,calc_ldr(PWM_GREEN));

/*imprime  dutty  atualizado*/

lcdSetPos(1,1);

lcdStringWrite("LDR = ");

lcdSetPos(1,7);

lcdStringWrite(ldr_buffer);

lcdSetPos(2,1);

lcdStringWrite("dutty = ");

lcdSetPos(2,8);

lcdStringWrite(dutty_buffer);

lcdSetPos(2,11);

lcdStringWrite("%");

  

}

if(BTN_DN_is_press()==BTN_PRESS)

{

dutty--;

if(dutty < 1)

{

dutty = 100;

}

sprintf(dutty_buffer,"%d",dutty);

/*imprime  dutty  atualizado*/

ftoa(ldr_buffer,calc_ldr(PWM_GREEN));

  

lcdSetPos(1,1);

lcdStringWrite("LDR = ");

lcdSetPos(1,7);

lcdStringWrite(ldr_buffer);

lcdSetPos(2,1);

lcdStringWrite("dutty = ");

lcdSetPos(2,8);

lcdStringWrite(dutty_buffer);

lcdSetPos(2,11);

lcdStringWrite("%");

  

}

if(BTN_OK_is_press()==BTN_PRESS)

{

/*salva o dutty*/

EEPROM_writeByte(GREEN_DUTTY_CELL,dutty);

lcdSetPos(1,1);

lcdStringWrite(" Verde ");

lcdSetPos(2,1);

lcdStringWrite(" Salvo ");

delayMs(1000);//para  efeito visual

sts_cor = FINISHED;

}

  

}

}

break;

case  DUTTY_AZUL:

if(BTN_UP_is_press()==BTN_PRESS)

{

sts_pwm = DUTTY_VERMELHO;

}

if(BTN_DN_is_press()==BTN_PRESS)

{

sts_pwm = DUTTY_VERDE;

}

lcdSetPos(1,1);

lcdStringWrite(" Azul ");

lcdSetPos(2,1);

lcdStringWrite(" ");

  

if(BTN_OK_is_press()==BTN_PRESS)

{

stats_cor_t sts_cor = IN_PROGRESS;

EEPROM_readRandomByte(BLUE_DUTTY_CELL,&dutty);

sprintf(dutty_buffer,"%d",dutty);

/*imprime  dutty  atualizado*/

ftoa(ldr_buffer,calc_ldr(PWM_BLUE));

/*imprime  dutty  atualizado*/

lcdSetPos(1,1);

lcdStringWrite("LDR = ");

lcdSetPos(1,7);

lcdStringWrite(ldr_buffer);

lcdSetPos(2,1);

lcdStringWrite("dutty = ");

lcdSetPos(2,8);

lcdStringWrite(dutty_buffer);

lcdSetPos(2,11);

lcdStringWrite("%");

while(sts_cor != FINISHED)

{

if(BTN_UP_is_press()==BTN_PRESS)

{

dutty++;

if(dutty > 100)

{

dutty = 1;

}

/*imprime  dutty  atualizado*/

sprintf(dutty_buffer,"%d",dutty);

/*imprime  dutty  atualizado*/

ftoa(ldr_buffer,calc_ldr(PWM_BLUE));

/*imprime  dutty  atualizado*/

lcdSetPos(1,1);

lcdStringWrite("LDR = ");

lcdSetPos(1,7);

lcdStringWrite(ldr_buffer);

lcdSetPos(2,1);

lcdStringWrite("dutty = ");

lcdSetPos(2,8);

lcdStringWrite(dutty_buffer);

lcdSetPos(2,11);

lcdStringWrite("%");

}

if(BTN_DN_is_press()==BTN_PRESS)

{

dutty--;

if(dutty < 1)

{

dutty = 100;

}

sprintf(dutty_buffer,"%d",dutty);

/*imprime  dutty  atualizado*/

ftoa(ldr_buffer,calc_ldr(PWM_BLUE));

/*imprime  dutty  atualizado*/

lcdSetPos(1,1);

lcdStringWrite("LDR = ");

lcdSetPos(1,7);

lcdStringWrite(ldr_buffer);

lcdSetPos(2,1);

lcdStringWrite("dutty = ");

lcdSetPos(2,8);

lcdStringWrite(dutty_buffer);

lcdSetPos(2,11);

lcdStringWrite("%");

}

if(BTN_OK_is_press()==BTN_PRESS)

{

/*salva o dutty*/

EEPROM_writeByte(BLUE_DUTTY_CELL,dutty);

lcdSetPos(1,1);

lcdStringWrite(" Azul ");

lcdSetPos(2,1);

lcdStringWrite(" Salvo ");

delayMs(1000);//para  efeito visual

sts_cor = FINISHED;

}

  

}

}

break;

  

}

}

}

uint16_t  ler_ldr(led_pwm_t led)

{

/*pega o valor de  dutty  na  memoria*/

uint8_t percent;

uint16_t dutty,ldr;

adcChSelect(LDR);

adcGetValue(&ldr);/*tira  sujeira do buffer*/

if(led==PWM_RED)

{

EEPROM_readRandomByte(RED_DUTTY_CELL,&percent);

dutty = percent*200;/*calcula o valor do registrador  de load para o dutty*/

ledControll(PWM_RED,dutty,LED_ON);

delayMs(1000);

adcChSelect(LDR);

adcGetValue(&ldr);

ledControll(PWM_RED,0,LED_OFF);

return ldr;

  

}

else  if(led==PWM_GREEN)

{

EEPROM_readRandomByte(GREEN_DUTTY_CELL,&percent);

dutty = percent*200;/*calcula o valor do registrador  de load para o dutty*/

ledControll(PWM_GREEN,dutty,LED_ON);

delayMs(1000);

adcChSelect(LDR);

adcGetValue(&ldr);

ledControll(PWM_GREEN,0,LED_OFF);

return ldr;

  

}

else  if(led==PWM_BLUE)

{

EEPROM_readRandomByte(BLUE_DUTTY_CELL,&percent);

dutty = percent*200;/*calcula o valor do registrador  de load para o dutty*/

ledControll(PWM_BLUE,dutty,LED_ON);

delayMs(1000);

adcChSelect(LDR);

adcGetValue(&ldr);

ledControll(PWM_BLUE,0,LED_OFF);

return ldr;

  

}

}

uint16_t  calibra(led_pwm_t led)

{

uint8_t n = 4;

uint16_t cal=0;

while( n != 0)

{

cal = cal+ler_ldr(led);

n--;

}

cal = cal/4;

return cal;

}

float  mede(led_pwm_t led, uint16_t *br)

{

uint8_t n = 4;

uint16_t amostra =0;

float result;

  

while( n != 0)

{

amostra = amostra + ler_ldr(led);

n--;

}

amostra = amostra/4;

  

result = (10)*log10f((float)*br/amostra);

  

if(result<0){

result = 0;

}

  

return result;

}

float  calc_ldr(led_pwm_t led)

{

/*calcula o valor da  resistencia do LDR para  evitar  que  estrapole 10k na  maxima  absorbancia*/

  

float ldr,vout,vldr,ildr;

vout = ler_ldr(led);

vldr = 2048 - vout;

ildr = (4096 - vldr)/10000;

ldr = vldr/ildr;

return ldr;

}

void  salva_resultado(char* cor,float amostra)

{

  

}

  
  

Esse é o código principal, o main.c, no código principal foram utilizados alguns drivers que nós criamos arquivos .c e .h para que pudéssemos usar alguns periféricos do microcontrolador, como o ADC, GPIO, I2C, timer etc. Criamos esses drivers em arquivos separados para que pudéssemos deixar o código mais organizado.

Os drivers criados são:

-   gpio.c
    
-   I2C.c
    
-   lcd.c
    
-   timer.c
    
-   UART.c
    
-   adc.c
    
-   24LC512.c
    

Abaixo segue o código desses drivers que foram criados:

  

**gpio.c**

  

#include  "gpio.h"

  
  

void  gpioInit(void){

/*Habilitando o clock para o PORTA e PORTB*/

set_bit(SIM->SCGC5,SIM_SCGC5_PORTA_SHIFT);

set_bit(SIM->SCGC5,SIM_SCGC5_PORTB_SHIFT);

  

/*configurando  entradas do portb  escrevendo 0 no gpiob_pddr*/

clr_bit(GPIOB->PDDR,BTN_UP);

clr_bit(GPIOB->PDDR,BTN_DN);

clr_bit(GPIOB->PDDR,BTN_OK);

clr_bit(GPIOB->PDDR,BTN_BK);

  

//***************************//

/*Habilita pull up dos  botoes*/

set_bit(PORTB->PCR[BTN_UP],PORT_PCR_PE_SHIFT);

set_bit(PORTB->PCR[BTN_DN],PORT_PCR_PE_SHIFT);

set_bit(PORTB->PCR[BTN_OK],PORT_PCR_PE_SHIFT);

set_bit(PORTB->PCR[BTN_BK],PORT_PCR_PE_SHIFT);

/*selecionando a opção i/o no mux do portb alt1*/

set_bit(PORTB->PCR[BTN_UP],8);

clr_bit(PORTB->PCR[BTN_UP],9);

clr_bit(PORTB->PCR[BTN_UP],10);

//***************************//

set_bit(PORTB->PCR[BTN_DN],8);

clr_bit(PORTB->PCR[BTN_DN],9);

clr_bit(PORTB->PCR[BTN_DN],10);

//***************************//

set_bit(PORTB->PCR[BTN_OK],8);

clr_bit(PORTB->PCR[BTN_OK],9);

clr_bit(PORTB->PCR[BTN_OK],10);

//***************************//

set_bit(PORTB->PCR[BTN_BK],8);

clr_bit(PORTB->PCR[BTN_BK],9);

clr_bit(PORTB->PCR[BTN_BK],10);

//***************************//

/*Entrada  analogica do portb alt0*/

clr_bit(PORTB->PCR[LDR_PIN],8);

clr_bit(PORTB->PCR[LDR_PIN],9);

clr_bit(PORTB->PCR[LDR_PIN],10);

//***************************//

/*configuração  dos  pinos do portb  para  os  leds  como  canais  de timer0 alt2*/

clr_bit(PORTB->PCR[LED_RED],8);

set_bit(PORTB->PCR[LED_RED],9);

clr_bit(PORTB->PCR[LED_RED],10);

//***************************//

clr_bit(PORTB->PCR[LED_GREEN],8);

set_bit(PORTB->PCR[LED_GREEN],9);

clr_bit(PORTB->PCR[LED_GREEN],10);

//***************************//

clr_bit(PORTB->PCR[LED_BLUE],8);

set_bit(PORTB->PCR[LED_BLUE],9);

clr_bit(PORTB->PCR[LED_BLUE],10);

//***************************//

/*configurando  gipoa  como  saida  para o lcd*/

set_bit(GPIOA->PDDR,EN);

set_bit(GPIOA->PDDR,RS);

set_bit(GPIOA->PDDR,D4);

set_bit(GPIOA->PDDR,D5);

set_bit(GPIOA->PDDR,D6);

set_bit(GPIOA->PDDR,D7);

/*selecionando  pinos  como i/o no mux alt1*/

set_bit(PORTA->PCR[EN],8);

clr_bit(PORTA->PCR[EN],9);

clr_bit(PORTA->PCR[EN],10);

//***************************//

set_bit(PORTA->PCR[RS],8);

clr_bit(PORTA->PCR[RS],9);

clr_bit(PORTA->PCR[RS],10);

//***************************//

set_bit(PORTA->PCR[D4],8);

clr_bit(PORTA->PCR[D4],9);

clr_bit(PORTA->PCR[D4],10);

//***************************//

set_bit(PORTA->PCR[D5],8);

clr_bit(PORTA->PCR[D5],9);

clr_bit(PORTA->PCR[D5],10);

//***************************//

set_bit(PORTA->PCR[D6],8);

clr_bit(PORTA->PCR[D6],9);

clr_bit(PORTA->PCR[D6],10);

//***************************//

set_bit(PORTA->PCR[D7],8);

clr_bit(PORTA->PCR[D7],9);

clr_bit(PORTA->PCR[D7],10);

//***************************//

}

void  gpioWritePin(GPIO_Type* base,uint8_t pin,uint8_t level){

  

if(!level){

set_bit(base->PCOR,pin);

}

else{

set_bit(base->PSOR,pin);

}

  

}

void  pinSetMux(PORT_Type *base,uint8_t pin,uint8_t mux){

/*Limpa o estado  atual*/

base->PCR[pin] &=~ PORT_PCR_MUX(7);

/*atualiza o estado*/

base->PCR[pin] |= PORT_PCR_MUX(mux);

}

btn_status_t  BTN_UP_is_press(void){

if (tst_bit(GPIOB->PDIR,BTN_UP)){

return  BTN_NO_PRESS;

}else  if (! tst_bit(GPIOB->PDIR,BTN_UP)){

delayMs(140);

return  BTN_PRESS;

}

}

btn_status_t  BTN_DN_is_press(void){

if (tst_bit(GPIOB->PDIR,BTN_DN)){

return  BTN_NO_PRESS;

}else  if (! tst_bit(GPIOB->PDIR,BTN_DN)){

delayMs(130);

return  BTN_PRESS;

}

}

btn_status_t  BTN_OK_is_press(void){

if (tst_bit(GPIOB->PDIR,BTN_OK)){

return  BTN_NO_PRESS;

}else  if (! tst_bit(GPIOB->PDIR,BTN_OK)){

delayMs(130);

return  BTN_PRESS;

}

}

btn_status_t  BTN_BK_is_press(void){

if (tst_bit(GPIOB->PDIR,BTN_BK)){

return  BTN_NO_PRESS;

}else  if (! tst_bit(GPIOB->PDIR,BTN_BK)){

delayMs(130);

return  BTN_PRESS;

}

}

  
  

**I2C.c**

  

#include  "I2C.h"

  
  

void  I2CInit() {

/*clock gating*/

SIM->SCGC4 |= SIM_SCGC4_I2C0_MASK;

SIM->SCGC5 |= SIM_SCGC5_PORTB_MASK;

/*configira PTB3 e PTB4 para  scl e sda*/

PORTB->PCR[I2C_SCL_PIN] |= PORT_PCR_MUX(2);

PORTB->PCR[I2C_SDA_PIN] |= PORT_PCR_MUX(2);

/*configuração 400khz*/

//I2C0->F =0x01;

/*configuração  menor 400khz*/

//I2C0->F =0x02;

/*configuração 100khz*/

//I2C0->F =0x16;

/*configuração  menor 100khz*/

//I2C0->F =0x17;

/*configuração 50khz*/

// I2C0->F =0x1E;

I2C0->F =0x00;

  

}

void  I2C_Enable(void){

I2C0->C1 |= I2C_C1_IICEN_MASK;

}

void  I2C_Disable(void){

I2C0->C1 &=~ I2C_C1_IICEN_MASK;

}

void  I2C_START(void){

while(tst_bit(I2C0->S,I2C_S_BUSY_SHIFT));

I2C0->C1 |= I2C_C1_MST_MASK;

I2C0->C1 |= I2C_C1_TX_MASK;

}

void  I2C_REP_START(void){

I2C0->C1 |= I2C_C1_RSTA_MASK;

}

void  I2C_STOP(void){

I2C0->C1 &=~ I2C_C1_TX_MASK;

I2C0->C1 &=~I2C_C1_MST_MASK;

}

void  I2C_STOP_READ(void){

I2C0->C1 &=~I2C_C1_MST_MASK;

}

void  I2C_RX_MODE(void){

I2C0->C1 &=~ I2C_C1_TX_MASK;

}

void  I2C_TX_MODE(void){

I2C0->C1 |= I2C_C1_TX_MASK;

}

void  I2C_SENT_ACK(void) {

I2C0->C1 &=~ I2C_C1_TXAK_MASK;

}

void  I2C_SENT_NACK(void) {

I2C0->C1 |= I2C_C1_TXAK_MASK;

}

  

void  I2CCallAdress(uint8_t adds,uint8_t op){

I2C0->D = (adds<<1 | op);

while(!tst_bit(I2C0->S,I2C_S_IICIF_SHIFT));

set_bit(I2C0->S,I2C_S_IICIF_SHIFT);

}

  

void  I2CWriteByte(uint8_t data) {

I2C0->D = data;

while(!tst_bit(I2C0->S,I2C_S_IICIF_SHIFT));

set_bit(I2C0->S,I2C_S_IICIF_SHIFT);

}

  

uint8_t  I2CReadByte(void) {

while(!tst_bit(I2C0->S,I2C_S_TCF_SHIFT));

return I2C0->D;

}

  
  

lcd.c

  

#include  "lcd.h"

  

char buffer[9];/*variavel  para  converção  de  inteiros  para char*/

  

void  enablePulse(void){

/*temporização  para  comunicação  com o lcd*/

set_bit(GPIOA->PDOR,EN);

delayMs(1);

clr_bit(GPIOA->PDOR,EN);

delayMs(1);

}

void  lcdCmd(uint8_t rs,uint8_t data){

/*testa  se é comando 0 ou  dado 1*/

if(! rs){

clr_bit(GPIOA->PDOR,RS);/*se for instrução  zera o pino RS*/

}

else{

set_bit(GPIOA->PDOR,RS);/*se for dados  escreve 1 no pino RS*/

}

GPIOA->PDOR = (GPIOA->PDOR & 0xF0FF);/*limpa  os bits 11,10,9,8 do portb*/

gpioWritePin(GPIOA,D7,tst_bit(data,7));/*escreve 1 ou 0 no pino  especifico  dependento do resultado do tst_bit*/

gpioWritePin(GPIOA,D6,tst_bit(data,6));/*escreve 1 ou 0 no pino  especifico  dependento do resultado do tst_bit*/

gpioWritePin(GPIOA,D5,tst_bit(data,5));/*escreve 1 ou 0 no pino  especifico  dependento do resultado do tst_bit*/

gpioWritePin(GPIOA,D4,tst_bit(data,4));/*escreve 1 ou 0 no pino  especifico  dependento do resultado do tst_bit*/

enablePulse();

GPIOA->PDOR = (GPIOA->PDOR & 0xF0FF);/*limpa  os bits 11,10,9,8 do portb*/

gpioWritePin(GPIOA,D7,tst_bit(data,3));/*escreve 1 ou 0 no pino  especifico  dependento do resultado do tst_bit*/

gpioWritePin(GPIOA,D6,tst_bit(data,2));/*escreve 1 ou 0 no pino  especifico  dependento do resultado do tst_bit*/

gpioWritePin(GPIOA,D5,tst_bit(data,1));/*escreve 1 ou 0 no pino  especifico  dependento do resultado do tst_bit*/

gpioWritePin(GPIOA,D4,tst_bit(data,0));/*escreve 1 ou 0 no pino  especifico  dependento do resultado do tst_bit*/

enablePulse();

  

}

  

void  lcdInit(void){

clr_bit(GPIOA->PDOR,RS);/*para  evitar  comando  errado*/

clr_bit(GPIOA->PDOR,EN);/*para  evitar  comando  errado*/

delayMs(20);/*espera  estabilizar*/

/*inicia 8bits*/

GPIOA->PDOR =(GPIOA->PDOR & 0xF0FF)|(0x0300);/*lembrando  que  são 8 bits 0x30, mas  para  que o 0x3 fique no nible 11,10,9,8 acrecentamos  mais  um zero*/

enablePulse();/*pulso  de enable para  que o lcd  entenda  que é um  comando  por  que RS==0*/

/*Envia 3 vezes o mesmo valor 0x300*/

delayMs(5);

enablePulse();

//delayUs(200); /*envia 3x o valor por  padrão  de  comunicação  com o lcd  de  acordo  com  datasheet*/

delayMs(1);

enablePulse();

/*Muda  para 4bits*/

GPIOA->PDOR =(GPIOA->PDOR & 0xF0FF)|(0x0200);/*comando  para  escolha  de  modo  de  operação 4 bits*/

enablePulse();

/*inicia*/

lcdCmd(0,0x28);/*4 bits duas  linhas*/

lcdCmd(0,0x08);/*Liga/desliga*/

lcdCmd(0,0x01);/*Comando clear*/

lcdCmd(0,0x0C);/*Cursor inativo  não  fica  piscando*/

lcdCmd(0,0x80);/*Coloca o cursor na  posição 0x80. OBS: a esquerda  na  primeira  linha*/

}

void  lcdStringWrite(char *s){

for(;*s!='\0';s++){

lcdCmd(1,*s);/*envia  caracter  por  caracter ate chegar no caracter  nulo*/

}

}

void  lcdClear(void){

lcdCmd(0,CLEAR_LCD);

}

void  lcdSetPos(int linha,int coluna){

/*função  valida  para  lcd 16x2, em  outros  tamanhos  muda o endereço  de  memoria  de  cada  posição do lcd*/

long  int posicao = 0;

if(linha == 1){

posicao=0x80; //Se  setado  linha 1, endereço  incial 0x80

}

if(linha == 2){

posicao=0xC0; //Se  setado  linha 2, endereço  incial 0xc0

}

posicao=posicao+coluna; //soma  ao  endereço  inicial, o numero  da  coluna

posicao--; //subtrai 1 para  corrigir  posição

lcdCmd(0,posicao);

}

void  ftoa(char *str,float num){

int dec,cent,l=0;

dec=num;

if(dec==0){

  

str[0]='0';

str[1]=',';

l=2;

// for(int a=0;a<3;a++){

// return str[a];

//}

}else  if(dec > 0 && dec < 10){

  

str[0]=((dec%10)+48);

str[1]=',';

l=2;

// for(int a=0;a<3;a++){

// return str[a];

//}

}else  if (dec >99 && dec < 1000){

str[0]=(((dec/100)%10)+48);

str[1]=(((dec/10)%10)+48);

str[2]=((dec%10)+48);

str[3]=',';

l=4;

// for(int a=0;a<4;a++){

// return str[a];

//}

}else  if (dec > 999 && dec < 10000){

str[0]=(dec/1000)+48;

str[1]=(((dec/100)%10)+48);

str[2]=(((dec/10)%10)+48);

str[3]=((dec%10)+48);

str[4]=',';

l=5;

// for(int a=0;a<5;a++){

// return str[a];

//}

} else  if (dec > 9999 && dec < 100000){

str[0]=(dec/10000)+48;

str[1]=((dec/1000)%10)+48;

str[2]=(((dec/100)%10)+48);

str[3]=(((dec/10)%10)+48);

str[4]=((dec%10)+48);

str[5]=',';

l=6;

// for(int a=0;a<6;a++){

// return str[a];

//}

}else  if (dec >9 && dec < 100){

str[0]=((dec/10)+48);

str[1]=((dec%10)+48);

str[2]=',';

l=3;

// for(int a=0;a<3;a++){

// return str[a];

//}

}

num=dec-num;

num=num*(1000);

cent=num*(-1);

str[l]=(((cent/100)%10)+48);

l++;

str[l]=(((cent/10)%10)+48);

l++;

str[l] = '\0';

// return (char **)str;

}

  
  

**timer.c**

  

#include  "timer.h"

  
  
  
  

void  timeBaseInit(void){

/*------------------------configuração do clock para 4Mhz-------------------------*/

MCG->C2 |= MCG_C2_IRCS(1);/*seleciona o clock interno  de 4Mhz*/

  

MCG->C1 |= MCG_C1_CLKS(1);/*seleciona o clk  interno  como  referencia*/

  

MCG->SC &=~ MCG_SC_FCRDIV_MASK;/*fast clk  dividido  por 1*/

/*--------------Configuração  para o systick  usado  para o dalay-----------------*/

SysTick->LOAD = LOAD_MILI_SECONDS;/*valor de  inicio  de  contagem  regressiva*/

/*configuração do systick  para o delay*/

set_bit(SysTick->CTRL,SysTick_CTRL_CLKSOURCE_Pos);/*Seleciona  clk  interno 20,9Mhz*/

/*---------------configuração do timer 0 para PWM------------------------------------*/

/*Habilita o clock para  os timers */

set_bit(SIM->SCGC6,SIM_SCGC6_TPM0_SHIFT);

/*seleciona a fonte  de clock MCGIRCLK para o timer 0 "3" 25-24*/

set_bit(SIM->SOPT2,SIM_SOPT2_TPMSRC_SHIFT);

clr_bit(SIM->SOPT2,25);

/*configuração  para PWM de  frequencia 1KHZ*/

clr_bit(TPM0->SC,0);/*mcgirclk/1*/

clr_bit(TPM0->SC,1);

clr_bit(TPM0->SC,2);

/*valor do registrador  de  recarga = 20000 que é o valor para 100% do dutty*/

TPM0->MOD = 20000;

/*configura up counter*/

clr_bit(TPM0->SC,TPM_SC_CPWMS_SHIFT);/*modo up count*/

}

void  ledControll(led_pwm_t led,uint16_t dutty,led_status_t level){

if(level==LED_ON){

/*desabilita o canal para  alterar a contagem*/

clr_bit(TPM0->SC,TPM_SC_CMOD_SHIFT);

TPM0->CNT = 0x0000;/*zera  contagem*/

set_bit(TPM0->SC,TPM_SC_CMOD_SHIFT);/*inicia  contagem*/

TPM0_CnSC(led)|=0x2C;/*alinhado a borda,nivel  baixo  em  contagem, alto na  recarga*/

TPM0_CnV(led)=dutty;/*valor para a contagem do canal*/

}else  if(level==LED_OFF){

TPM0_CnSC(led)= 0x00;/*desliga o canal*/

clr_bit(TPM0->SC,TPM_SC_CMOD_SHIFT);

}

}

  

void  delayMs(uint16_t ms){

SysTick->VAL =0;/*zera valor do contador*/

set_bit(SysTick->CTRL,SysTick_CTRL_ENABLE_Pos);/*Habilita  contagem*/

for (int i=0;i<ms;i++){

while(!(tst_bit(SysTick->CTRL,SysTick_CTRL_COUNTFLAG_Pos)));/*espera o estouro  da  contagem*/

}

clr_bit(SysTick->CTRL,SysTick_CTRL_ENABLE_Pos);/*desabilita o contador*/

}

  
  

**UART.c**

  

#include  "UART.h"

  

int  __io_putchar(int ch){

uart_transmit(ch);

return ch;

}

  

void  uart_pin_config(void){

SIM->SCGC5 |= SIM_SCGC5_PORTB_MASK;

PORTB->PCR[1] = PORT_PCR_MUX(2);

PORTB->PCR[2] = PORT_PCR_MUX(2);

}

void  uart_init_9600Bound(void){

/*clk gating*/

SIM->SCGC4 |= SIM_SCGC4_UART0_MASK;

/*clk source FLL*/

SIM->SOPT2 |= SIM_SOPT2_UART0SRC(1);

/*configura bound rate 9600*/

UART0->BDL = 0x89U;

UART0->BDH &= 0xE0U;

UART0->BDH |= (0x89U >> 8U);

/*Stop bit apenas  um*/

UART0->BDH &=~ UART0_BDH_SBNS_MASK;

/*sem  paridade*/

UART0->C2 &=~ UART0_C1_PE_MASK;

/*TX apenas*/

UART0->C2 |= UART0_C2_TE_MASK;

  

}

void  uart_transmit(char data){

while(!tst_bit(UART0->S1,UART0_S1_TDRE_SHIFT));

UART0->D = data;

while(!tst_bit(UART0->S1,UART0_S1_TC_SHIFT));

}

void  uart_transmit_string(char * c){

for(;*c !=0;c++){

uart_transmit(*c);

}

}

  

**adc.c**

  

#include  "adc.h"

  

void  adcSingleInit(void){

/*Habilita o clock para o ADC*/

set_bit(SIM->SCGC6,SIM_SCGC6_ADC0_SHIFT);

/*Configuração do adc  para  uma  amostra  com media de 32x*/

set_bit(ADC0->CFG1,ADC_CFG1_ADICLK_SHIFT);/*BUSS clk/2*/

ADC0->CFG1 |= 1<<5 | 1<<6;/*divide o clock por 8*/

set_bit(ADC0->CFG1,ADC_CFG1_MODE_SHIFT);/*12 bits*/

/*configa long time sample*/

set_bit(ADC0->CFG1,ADC_CFG1_ADLSMP_SHIFT);

/*software trigger*/

clr_bit(ADC0->SC2,ADC_SC2_ADTRG_SHIFT);

/*single sample*/

clr_bit(ADC0->SC3,ADC_SC3_ADCO_SHIFT);

/*seleciona  modo medias*/

set_bit(ADC0->SC3,0);

set_bit(ADC0->SC3,1);

/*habilita o modo medias*/

set_bit(ADC0->SC3,2);

  

}

void  adcChSelect(adc_ch_t ch){

ADC0->SC1[0]= ch;/*liga o canal especifico*/

}

void  adcGetValue(uint16_t *value_store){

  

while(!tst_bit(ADC0->SC1[0],ADC_SC1_COCO_SHIFT))

{

/*espera a converção  estar  completa*/

}

*value_store = ADC0->R[0];/*carrega o valor da  medida  para a variavel*/

}

  

**24LC512.c**

  

#include  "24LC512.h"

  
  

void  EEPROM_writeByte(uint16_t page,uint8_t data){

I2C_Enable();

I2C_START();

I2CCallAdress(0x50,0);

I2CWriteByte(page>>8);

I2CWriteByte(page);

I2CWriteByte(data);

I2C_STOP();

I2C_Disable();

delayMs(7);

  

}

void  EEPROM_writePage(uint16_t initPage,uint8_t lenght,uint8_t *data){

  

I2C_Enable();

I2C_START();

I2CCallAdress(0x50,0);

I2CWriteByte(initPage>>8);

I2CWriteByte(initPage);

for(uint8_t i=0;i<lenght;i++){

I2CWriteByte(data[i]);

}

I2C_STOP();

delayMs(7);

I2C_Disable();

}

void  EEPROM_readRandomByte(uint16_t page,uint8_t *buffer){

  

I2C_Enable();

I2C_START();

I2CCallAdress(0x50,0);

I2CWriteByte(page>>8);

I2CWriteByte(page);

I2C_REP_START();

I2CCallAdress(0x50,1);

I2C_RX_MODE();

I2C_SENT_NACK();

*buffer = I2CReadByte();

I2C_SENT_NACK;

*buffer = I2CReadByte();

I2C_STOP();

I2C_Disable();

}

void  EEPROM_readSequentialBytes(uint16_t page,uint8_t *data,uint8_t size){

I2C_Enable();

I2C_START();

I2CCallAdress(0x50,0);

I2CWriteByte(page>>8);

I2CWriteByte(page);

I2C_REP_START();

I2CCallAdress(0x50,1);

I2C_RX_MODE();

I2C_SENT_ACK();

data[0]=I2CReadByte();/*Em  testes  ocorreu  uma  espécie  de ECO de  comunicação o primeiro byte passado  para o índice 0 é o próprio  endereço  da  eeprom*/

for(uint8_t i=0;i<size;i++){

data[i]=I2CReadByte();

}

I2C_SENT_NACK();

data[size] = I2CReadByte();

I2C_STOP();

I2C_Disable();

}

  
  

Junto com o código também foi feito o desenvolvimento de uma PCI para que pudéssemos utilizar todos os periféricos do microcontrolador que não tinham na placa de desenvolvimento do MKL05Z, como a memória e o ADC.

Na placa desenvolvida por nós também continha conectores para que pudesse ser feito o encaixe da placa de desenvolvimento na nossa placa projetada para que pudéssemos conectar a memória 24LC512 e o amplificador LM358. Também continha nessa placa desenvolvida conectores para receber o sinal vindo do LDR e dos botões, que eram sinais de entrada, e conector para mandar os sinais para os LEDs e o LCD, que eram os sinais de saída.

Em paralelo a essa placa foi feito também uma placa que funcionaria como teclado, com 4 botões e com um conector para conectar esses botões a outra placa, essa placa foi feita para que pudéssemos deixar os botões preso no gabinete, sendo mais fácil de utilizar esses botões.

  
  
  
  
  
  
  
  

**Placa principal**

  

**Esquemático**

  

Abaixo segue uma imagem do esquemático da placa principal desenvolvida:

  

![](https://lh7-us.googleusercontent.com/HeNWXs6jaYmy5rXGZGGwhA_w6N3ZOX-vS-MgTfh5_rg--R3XszWORikI1xjdq87pi6nDmK6X5fLduA4IqlFC3tusRZWCAsg5B6E7t-qIjDxeorfAqbs5WGIDZU-dGl81kIjH2TS_gLCPSqLN8--CJA)

  

**Layout**

A placa principal foi desenvolvida em duas camadas, a F.Cu (vermelha) e a B.Cu (azul), a F.Cu foi utilizada por conta dos componentes SMD, e B.Cu foi utilizada para fazer as trilhas que não foi possível fazer na F.Cu, ou seja, jumpers para que pudéssemos fazer todas as ligações.

Como a placa que utilizamos era somente de uma camada de cobre, esses jumpers foram feitos com fio.

  

![](https://lh7-us.googleusercontent.com/QsfBp6R4DNbqvRuIn_jwx-NrexORou6D2VGrs8J7lqZt10QULkMi20sUM4eC6WyV1qDgPCvxp_fom4lX3Re2u_vxpaaQj_UW23tYUih6QHrOCUaIsouttoP092NMuyr3g6tX0hYdSa4g6SZ7nfljJQ)

  

Não temos imagens da placa já montada, por isso colocarei imagens do modelo 3D dela feito no Kicad



Essa imagem é da parte SMD da placa.

Abaixo segue a imgem da parte PTH da placa, onde ficaram os conectores.

![](https://lh7-us.googleusercontent.com/-mbW65cgnDconPmw-ZTU24Nto3OekfAhTDajONP1HR2r2YF7SBPn8xJYefGrf2ENWw6xMSSsr4J5Le-4-DfZnyyO_D5kaXq5QlzoZqnQfJP-bGPwhAwytSFIdsoSJCO9sHiSxi5oxljWAIK63odHSw)

Os conectores da parte superior e inferior da placa foram utilizados conectores de 90°, e não de 180° como na imagem, os conectores da lateral foram de 180° igual na imagem.

  
  
  
  
  
  
  
  
  
  
  
  
  

**Placa dos Botões.**

A placa dos botões foi feita utilizando 4 botões poush button.

Segue abaixo o layout e o modelo 3D dela.

![](https://lh7-us.googleusercontent.com/yzWj44vMvTh2YHzt324tjD6fW2LSPPPo0eetTyrWAe_gbft7W6XIUOEteTc1xU6ofUz1pdJs4Ugc8cLEZdOLyblyDvajvY7oTmefn_6iaBzZ-cZK19EOQbGSXvXBQZ93oTkqvad863iqCMtumNS_MA)

  

![](https://lh7-us.googleusercontent.com/2rKdmEN50IKoIcEaQKJQSFVU4eyFck19c9k5-seRdFNnF2W4r_NLlGjJk1uSaST7OVmFDDFXxiIfvtl1ZSQpv5JTS5-P27ONyNdhU8Lt2I862aA0i1p1X-nNI-EIH8sbbqEsZn8fAotbMmzj1eL90w)

O conector para montar a placa também foi um conector de 90° e não o de 180° como na imagem.

  

Após a montagem do equipamento, com placa e código prontos, foram feitos alguns testes para ver se ele estava funcionando corretamente, nos testes foi realizado algumas leituras com 4 padrões de cor diferentes, ou seja, 4 pontos de leitura diferente, foi feito os testes com esses padrões no 3 LED.

Abaixo segue os resultados dos testes em forma de tabela.

**LED Vermelho.**

![](https://lh7-us.googleusercontent.com/r50QjhpJieoWoH8R_PJ8DxEXk39D4IYUiyW5WmqqbA7cEsFkqMxbGo3j5GnNcm3SqV9n18eRLmoYQ8QAfhRlvjnAITU68MzVkVmOXfges607KhdxY0jQ4AbC19UKy0J5EyJVOU8vsxXosN-RGvu5Jw)

  

**LED Verde.**

![](https://lh7-us.googleusercontent.com/gTpDm1S-0X8IGTI_tIUXYZRIHGGZ_u2LIsnnvR_M4z0ETnyDh8INlNrzOQn5eTFfI-VniAfgWBMZeSR7XTJqoZBNsG1fBICeFqJSmbD0I0Mo46ogf0UMUFxdfUAkK0lfJF-1sTYmyTocnptquONRLg)

**LED Azul.**

**![Tabela

Descrição gerada automaticamente](https://lh7-us.googleusercontent.com/AaH5ZEZ4puLNZl1SJYNBUOXOYwBfZRv3Rph1IYxQseo_OOpklxprBU_ao00NyWg7aqSrvGLOQogj6IlmEBTkiUdkvOUR5I7-i4pNjs9fLcGgfGqeXPcfvNUJBed-Ytyfetg8p89GQ1D5dWJ7mRVrLw)**


  

**Média dos LEDs**

![](https://lh7-us.googleusercontent.com/Ne_B0EcJnS6TE7SodWfWiuNN_5GYuRy7qpYKZclh_zVi-Q8r80tE1QPLL4oig__N4tSYSRCHd3k4Rj8ByQ1NPocBypamArJgCF0Yli6GsWKKESuj61o8m0SfWzSVLmUamZ324tGObg5_Dv9TFyGz9g)

  

**Estatísticas**

![](https://lh7-us.googleusercontent.com/2_kM8KZC8ASzNdnueFWdLKqawc3FbcozdCpdPp_2i19ntrdbR7A-JLfX60UomcRfwtcxEfox7iqMZTY5Qo2ANmTBw3jtYd45Rn4TXm3VvbyUVSTJ3WFaFmIHW_jBMMd49qSO-7G8wz88zkUuf4Dd1g)

  

**Gráfico das médias**

![](https://lh7-us.googleusercontent.com/ll_XDbRgjQzS3cC27yvm03Dt7kO5Msmq0VhTYK0BglOn_JI9_2Gi4TjylcBzEPub2ba0rOCfmN4rwpQqcL2e7HI9jGf2FqWpF-QBg_spua1JUEVFxSmSgsj0Em68UQUs4Teb28RMnBrGMgHtrAHH4A)

Através do gráfico das médias de leitura de cada LED, notasse que os LEDs azul e verde tiveram um comportamento muito parecidos e foram bem lineares, já o LED vermelho até o segundo ponto ele ficou próximo dos LEDs azul e verde, porém para as leituras com padrões mais alto saiu da linearidade, mostrado que para padrões mais altos o LED vermelho não tem um bom comportamento, porque a cor vermelha é de 660 nanômetro, com um erro de mais 20 ou menos 20, e o fundo de escala máximo do LDR é de no máximo 700 nanômetro, ou seja, ele está trabalhando numa faixa muito próxima do valor máximo permitido, algo que pode ser ajustado através do firmware o Hardware.

# LCD_DISPLAY
#CODING an lcd display in c for an msp 430 micriocontroller used at internship

#define COM  P3OUT &= (~BIT6)
#define DATA P3OUT |= BIT6
#define WRITE P3OUT &= (~BIT7)
#define READ P3OUT |= BIT7
#define ENC_HIGH P2OUT |= BIT3
#define ENC_LOW P2OUT &= (~BIT3)

#include<msp430.h>

int a;
void DELAY(int i)
{
    int j;
    for(;i>0;i++)
    {
        for(j=0;j<1000;j++);
    }
}

void data_read(void)
{

    ENC_LOW;
    DELAY(2);
    ENC_HIGH;

}

void data_write(void)
{

    ENC_HIGH;
    DELAY(2);
    ENC_LOW;
}

void check_busy(void)
{
    P4DIR &= (~BIT7);


    while((P4IN&BIT7)==1)
    {
        data_read();
    }
    P4DIR |= BIT7;
}

void LCD_CMD(void)
{
    check_busy();
    WRITE;
    COM;
    P4OUT = (P4OUT&0X00)|a;
    data_write();
}
void LCD_DAT(void)
{
    check_busy();
    WRITE;
    DATA;
    P4OUT = (P4OUT&0X00)|a;
    data_write();
}

void LCD_init(void)
{
    a = 0X38;
    LCD_CMD();
    DELAY(5);

    a = 0X0F;
    LCD_CMD();
    DELAY(5);

    a = 0X01;
    LCD_CMD();
    DELAY(5);

    a = 0X06;
    LCD_CMD();
    DELAY(5);

    a = 0X80;
    LCD_CMD();
    DELAY(5);
}

void main()
{
    P2DIR |= 0xFF;

          P3DIR |= 0xFF;

          P4DIR |= 0xFF;

          P2OUT &= 0x00;

          P3OUT &= 0x00;

          P4OUT &= 0x00;

    P2SEL &= ~BIT7;

   // P2OUT = 0X00;

    LCD_init();

    //a = 0X32;
    //LCD_DAT();

    while(1);
}

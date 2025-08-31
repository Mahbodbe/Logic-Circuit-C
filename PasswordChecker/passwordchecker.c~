#include <mega32.h>
#include <stdio.h>
#include <alcd.h>
#include <string.h>

#define correctpassword "*14#"
unsigned char digit[16] = {0x3f,0x06,0x5b,0x4f,0x66,0x6d,0x7d,0x07,
                           0x7f,0x6f,0x5c,0x7c,0x39,0x5e,0x79,0x71};

int timer = 0;
int loginchecker = 0;
int inputcounter = 0;
int sscounter = 0;
int iswrong = 0;
int keypadclicked = 0;
char iputpass[15];
char temp[15];
int passwordPromptShown = 0;

int menu_state = 0;  
int menu = 0;        
int menunext = 0;
int menuselect = 0;

int menutogglereq = 0;
int ismenu = 1;      
int menutoggle = 0;
int buzzer = 0;
int led =1 ;


interrupt [EXT_INT0] void ext_int0_isr(void)
{
    menuselect++;
}

interrupt [EXT_INT1] void ext_int1_isr(void)
{
    menunext++;
}

interrupt [EXT_INT2] void ext_int2_isr(void)
{
    keypadclicked++;
}

interrupt [TIM0_OVF] void timer0_ovf_isr(void)
{
    timer++;
}


char keypadinput(int keycode) {
    char keypadoutput = '0';
    switch(keycode)
    {
        case 0x0: 
            keypadoutput = '1'; 
            break;
        case 0x1: 
            keypadoutput = '2'; 
            break;
        case 0x2: 
            keypadoutput = '3'; 
            break;
        case 0x3: 
            keypadoutput = 'A'; 
            break;
        case 0x4: 
            keypadoutput = '4'; 
            break;
        case 0x5: 
            keypadoutput = '5'; 
            break;
        case 0x6: 
            keypadoutput = '6'; 
            break;
        case 0x7: 
            keypadoutput = 'B'; 
            break;
        case 0x8: 
            keypadoutput = '7'; 
            break;
        case 0x9: 
            keypadoutput = '8'; 
            break;
        case 0xA: 
            keypadoutput = '9'; 
            break;
        case 0xB: 
            keypadoutput = 'C'; 
            break;
        case 0xC: 
            keypadoutput = '*'; 
            break;
        case 0xD: 
            keypadoutput = '0'; 
            break;
        case 0xE: 
            keypadoutput = '#'; 
            break;
        case 0xF: 
            keypadoutput = 'D'; 
            break;
    }
    return keypadoutput;
}

void displayMainMenu()
{
    lcd_clear();
    switch(menu)
    {
        case 0:
            lcd_gotoxy(0,0); lcd_putsf("> LED");
            lcd_gotoxy(0,1); lcd_putsf("Buzzer");
            break;
        case 1:
            lcd_gotoxy(0,0); lcd_putsf("LED");
            lcd_gotoxy(0,1); lcd_putsf("> Buzzer");
            break;
        case 2:
            lcd_gotoxy(0,0); lcd_putsf("Buzzer");
            lcd_gotoxy(0,1); lcd_putsf("> Relay");
            break;
    }
}

void main(void)
{
// Declare your local variables here

// Input/Output Ports initialization
// Port A initialization
// Function: Bit7=Out Bit6=Out Bit5=Out Bit4=Out Bit3=Out Bit2=Out Bit1=Out Bit0=Out 
DDRA=(1<<DDA7) | (1<<DDA6) | (1<<DDA5) | (1<<DDA4) | (1<<DDA3) | (1<<DDA2) | (1<<DDA1) | (1<<DDA0);
// State: Bit7=0 Bit6=0 Bit5=0 Bit4=0 Bit3=0 Bit2=0 Bit1=0 Bit0=0 
PORTA=(0<<PORTA7) | (0<<PORTA6) | (0<<PORTA5) | (0<<PORTA4) | (0<<PORTA3) | (0<<PORTA2) | (0<<PORTA1) | (0<<PORTA0);
PORTA = digit[sscounter];

// Port B initialization
// Function: Bit7=In Bit6=In Bit5=In Bit4=In Bit3=In Bit2=In Bit1=In Bit0=In 
DDRB=(0<<DDB7) | (0<<DDB6) | (0<<DDB5) | (0<<DDB4) | (0<<DDB3) | (0<<DDB2) | (0<<DDB1) | (0<<DDB0);
// State: Bit7=T Bit6=T Bit5=T Bit4=T Bit3=T Bit2=T Bit1=T Bit0=T 
PORTB=(0<<PORTB7) | (0<<PORTB6) | (0<<PORTB5) | (0<<PORTB4) | (0<<PORTB3) | (0<<PORTB2) | (0<<PORTB1) | (0<<PORTB0);

// Port C initialization
// Function: Bit7=In Bit6=In Bit5=In Bit4=In Bit3=In Bit2=Out Bit1=Out Bit0=Out 
DDRC=(0<<DDC7) | (0<<DDC6) | (0<<DDC5) | (0<<DDC4) | (0<<DDC3) | (1<<DDC2) | (1<<DDC1) | (1<<DDC0);
// State: Bit7=T Bit6=T Bit5=T Bit4=T Bit3=T Bit2=0 Bit1=0 Bit0=0 
PORTC=(0<<PORTC7) | (0<<PORTC6) | (0<<PORTC5) | (0<<PORTC4) | (0<<PORTC3) | (0<<PORTC2) | (0<<PORTC1) | (0<<PORTC0);

// Port D initialization
// Function: Bit7=In Bit6=In Bit5=In Bit4=In Bit3=In Bit2=In Bit1=In Bit0=In 
DDRD=(0<<DDD7) | (0<<DDD6) | (0<<DDD5) | (0<<DDD4) | (0<<DDD3) | (0<<DDD2) | (0<<DDD1) | (0<<DDD0);
// State: Bit7=T Bit6=T Bit5=T Bit4=T Bit3=T Bit2=T Bit1=T Bit0=T 
PORTD=(0<<PORTD7) | (0<<PORTD6) | (0<<PORTD5) | (0<<PORTD4) | (0<<PORTD3) | (0<<PORTD2) | (0<<PORTD1) | (0<<PORTD0);

// Timer/Counter 0 initialization
// Clock source: System Clock
// Clock value: 125.000 kHz
// Mode: Normal top=0xFF
// OC0 output: Disconnected
// Timer Period: 2.048 ms
TCCR0=(0<<WGM00) | (0<<COM01) | (0<<COM00) | (0<<WGM01) | (0<<CS02) | (1<<CS01) | (1<<CS00);
TCNT0=0x00;
OCR0=0x00;

// Timer/Counter 1 initialization
// Clock source: System Clock
// Clock value: Timer1 Stopped
// Mode: Normal top=0xFFFF
// OC1A output: Disconnected
// OC1B output: Disconnected
// Noise Canceler: Off
// Input Capture on Falling Edge
// Timer1 Overflow Interrupt: Off
// Input Capture Interrupt: Off
// Compare A Match Interrupt: Off
// Compare B Match Interrupt: Off
TCCR1A=(0<<COM1A1) | (0<<COM1A0) | (0<<COM1B1) | (0<<COM1B0) | (0<<WGM11) | (0<<WGM10);
TCCR1B=(0<<ICNC1) | (0<<ICES1) | (0<<WGM13) | (0<<WGM12) | (0<<CS12) | (0<<CS11) | (0<<CS10);
TCNT1H=0x00;
TCNT1L=0x00;
ICR1H=0x00;
ICR1L=0x00;
OCR1AH=0x00;
OCR1AL=0x00;
OCR1BH=0x00;
OCR1BL=0x00;

// Timer/Counter 2 initialization
// Clock source: System Clock
// Clock value: Timer2 Stopped
// Mode: Normal top=0xFF
// OC2 output: Disconnected
ASSR=0<<AS2;
TCCR2=(0<<PWM2) | (0<<COM21) | (0<<COM20) | (0<<CTC2) | (0<<CS22) | (0<<CS21) | (0<<CS20);
TCNT2=0x00;
OCR2=0x00;

// Timer(s)/Counter(s) Interrupt(s) initialization
TIMSK=(0<<OCIE2) | (0<<TOIE2) | (0<<TICIE1) | (0<<OCIE1A) | (0<<OCIE1B) | (0<<TOIE1) | (0<<OCIE0) | (1<<TOIE0);

// External Interrupt(s) initialization
// INT0: On
// INT0 Mode: Falling Edge
// INT1: On
// INT1 Mode: Rising Edge
// INT2: On
// INT2 Mode: Falling Edge
GICR|=(1<<INT1) | (1<<INT0) | (1<<INT2);
MCUCR=(1<<ISC11) | (1<<ISC10) | (1<<ISC01) | (0<<ISC00);
MCUCSR=(0<<ISC2);
GIFR=(1<<INTF1) | (1<<INTF0) | (1<<INTF2);

// USART initialization
// USART disabled
UCSRB=(0<<RXCIE) | (0<<TXCIE) | (0<<UDRIE) | (0<<RXEN) | (0<<TXEN) | (0<<UCSZ2) | (0<<RXB8) | (0<<TXB8);

// Analog Comparator initialization
// Analog Comparator: Off
// The Analog Comparator's positive input is
// connected to the AIN0 pin
// The Analog Comparator's negative input is
// connected to the AIN1 pin
ACSR=(1<<ACD) | (0<<ACBG) | (0<<ACO) | (0<<ACI) | (0<<ACIE) | (0<<ACIC) | (0<<ACIS1) | (0<<ACIS0);
SFIOR=(0<<ACME);

// ADC initialization
// ADC disabled
ADCSRA=(0<<ADEN) | (0<<ADSC) | (0<<ADATE) | (0<<ADIF) | (0<<ADIE) | (0<<ADPS2) | (0<<ADPS1) | (0<<ADPS0);

// SPI initialization
// SPI disabled
SPCR=(0<<SPIE) | (0<<SPE) | (0<<DORD) | (0<<MSTR) | (0<<CPOL) | (0<<CPHA) | (0<<SPR1) | (0<<SPR0);

// TWI initialization
// TWI disabled
TWCR=(0<<TWEA) | (0<<TWSTA) | (0<<TWSTO) | (0<<TWEN) | (0<<TWIE);

// Alphanumeric LCD initialization
// Connections are specified in the
// Project|Configure|C Compiler|Libraries|Alphanumeric LCD menu:
// RS - PORTB Bit 0
// RD - PORTB Bit 1
// EN - PORTB Bit 3
// D4 - PORTB Bit 4
// D5 - PORTB Bit 5
// D6 - PORTB Bit 6
// D7 - PORTB Bit 7

// Characters/line: 16
lcd_init(16);

// Global enable interrupts
#asm("sei")

    while (1)
    {
        if(!loginchecker)
        {  
            char str[2];
            if(inputcounter == 0 && !passwordPromptShown && !iswrong)
            {
                lcd_clear();
                lcd_gotoxy(0,0);
                lcd_putsf("Enter Password:");
                passwordPromptShown = 1;
            }   
             
            if (timer > 488 && !iswrong)
            {
                PORTA = digit[sscounter];
                sscounter++;
                timer = 0;
                if (sscounter > 15)
                {
                    sscounter = 0;
                }   
            } 
              
            
            if(keypadclicked) 
            { 
                keypadclicked = 0;
                if(inputcounter < 4)
                {
                    lcd_gotoxy(0,1);
                    sprintf(str, "%c", keypadinput((PIND & 0xF0) >> 4));
                    iputpass[inputcounter] = str[0];
                    temp[inputcounter] = '*';
                    temp[inputcounter + 1] = '\0'; 
                    lcd_puts(temp);
                    inputcounter++;
                    if(inputcounter == 4)
                    {
                        iputpass[4] = '\0';
                        if (!strcmp(iputpass, correctpassword))
                        {
                            lcd_clear();
                            displayMainMenu();
                            loginchecker = 1;
                            
                            menu_state = 0; 
                            menu = 0;
                            ismenu = 1;
                            menutogglereq = 0;
                            menutoggle = 0;
                            buzzer = 0;
                        }
                        else
                        {
                            lcd_clear();
                            lcd_gotoxy(0,0);
                            lcd_putsf("Wrong password!");
                            iswrong = 1;
                            timer = 0;
                            memset(iputpass, 0, sizeof(iputpass));
                            memset(temp, 0, sizeof(temp));
                            passwordPromptShown = 0; 
                        }
                    }
                } 

            }
                
            if(iswrong)
            {
                if (timer > 488)
                { 
                    timer = 0;
                    lcd_clear();
                    iswrong = 0;
                    inputcounter = 0;
                    memset(iputpass, 0, sizeof(iputpass));
                    memset(temp, 0, sizeof(temp));
                    passwordPromptShown = 0;
                }
            } 
        }
        else 
        { 
            if(menuselect)
            {  
                menuselect = 0;
                menutogglereq = 1;  
            }
            if(menunext)
            {  
                menunext = 0;
                menutoggle = 1;      
            }
            
            if (menutogglereq)
            {
                if (timer > 97)
                {
                    timer = 0;
                    menutogglereq = 0;
                    ismenu ^= 1; 
                    PORTC = 0x00;
                    lcd_clear();
                    menutoggle = 0;
                    PORTA = 0x00;
                    buzzer = 0;
                    if(ismenu)
                        menu_state = 0;
                    else
                        menu_state = 1;
                    displayMainMenu();
                }
            }
            
            if(ismenu)
            {
                if(menutoggle)
                {
                    if(timer > 97)
                    { 
                        timer = 0;
                        menutoggle = 0;
                        menu = (menu + 1) % 3;
                        displayMainMenu();
                        led =1;
                    }
                }
            }
            else
            {
                switch (menu)
                {
                    case 0:
                        if(led)
                        {    
                           lcd_clear();
                           led =0;
                        }
                        lcd_gotoxy(0, 0);
                        lcd_putsf("LED");
                        PORTC = 0x01;         
                        PORTA = digit[10];    
                        break;
                    case 1: 
                        if(led)
                        {    
                           lcd_clear();
                           led =0;
                        }
                        lcd_gotoxy(0, 0);
                        lcd_putsf("Buzzer");
                        PORTA = digit[11];    
                        if (!buzzer)
                        {
                            PORTC = 0x02;     
                        }
                        if (timer > 488)
                        {
                            buzzer = 1;
                            PORTC = 0x00;
                        }
                        break;
                    case 2:    
                        if(led)
                        {    
                           lcd_clear();
                           led =0;
                        }
                        lcd_gotoxy(0, 0);
                        lcd_putsf("Relay");
                        PORTC = 0x04;         
                        PORTA = digit[12];    
                        break;
                }
            }
        }
    }
}

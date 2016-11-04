# Smart_Systems_Coarsework
I had to create a children educational toy in C using Atmel boards
#Main

#include <avr/io.h>			// Default include path is C:\WinAVR\avr\include\avr
#include <avr/interrupt.h>
#include "LCD_LibraryFunctions_1281.h"
#include "GccLibrary1.h"
#include <util/delay.h>


#define ScanKeypadRow0 0b00111111	// Bits 4-7 pulled low depending on row being scanned, bits 0-2 (pullups) remain high at all times
#define ScanKeypadRow1 0b01011111
#define ScanKeypadRow2 0b01101111
#define ScanKeypadRow3 0b01110111

#define KeypadMaskColumns 0b11111000
#define KeypadMaskColumn0 0b00000100
#define KeypadMaskColumn1 0b00000010
#define KeypadMaskColumn2 0b00000001


#define Star	0x0A
#define Zero	0x0B
#define Hash	0x0C
#define NoKey	0xFF


void InitialiseGeneral();

unsigned char ScanKeypad(void);
unsigned char ScanColumns(unsigned char);   // what it returns 
void DisplayKeyValue(unsigned char);		// what you pass to it
void DebounceDelay(void);			
void equation(unsigned char);	
/*answer(unsigned char);	*/
void comparer(unsigned char);
void explain(void);

select(void);
select1(void);
select2(void);
unsigned char KeyValue;

display(char bybys);
display1(char bybys1);

additionselect1(void);
additionselect2(void);
additionselect3(void);
additionselect4(void);
additionselect5(void);
additionselect6(void);
additionselect7(void);
additionselect8(void);
additionselect9(void);

randomizer(void);
addition(void);
point(void);
void pointsadd(void);

unsigned char number2;
unsigned char number3;
unsigned char answer1;
unsigned char dispansw1;
unsigned char dispansw2;
unsigned char dispansw3;
unsigned char points;

int main( void )
{
	
	InitialiseGeneral();

	lcd_Clear();				// Clear the display
	lcd_StandardMode();			// Set Standard display mode
	lcd_on();					// Set the display on
	lcd_CursorOff();			// Set the cursor display off (underscore)
	lcd_CursorPositionOff();	// Set the cursor position indicator off (flashing square)

	lcd_SetCursor(0x00);		// Set cursor position to line 1, col 0
	lcd_WriteString("Select");
	lcd_SetCursor(0x40);		// Set cursor position to line 2, col 0
	lcd_WriteString("options 1-3");
	
	points = 0;
	number2 = 17;
	number3 = 19;
	
			
    while(1)
    {	
		KeyValue = ScanKeypad();	// Read value on switches

		if( 1 == (KeyValue)) // Switches are inverted, so when pressed will give a '0'
		{	// Switch 0 pressed
			lcd_Clear();				// Clear the display
			lcd_SetCursor(0x00);		// 0b00000010	Set cursor position to line 1, col 0		
			lcd_WriteString("1-Addition");
			
			lcd_SetCursor(0x40);
			lcd_WriteString("*-ok #-back");	
			select();
		}			
			
		if( 2 == (KeyValue)) // Switches are inverted, so when pressed will give a '0'
		{	// Switch 1 pressed
			lcd_Clear();				// Clear the display
			lcd_SetCursor(0x00);		// 0b00000010	Set cursor position to line 1, col 0			// Write a specific character - see the LCD manual for details
			lcd_WriteString("2-Subtraction");
			lcd_SetCursor(0x40);
			lcd_WriteString("*-ok #-back");
			select1();
			
		}				
		if( 3 == (KeyValue)) // Switches are inverted, so when pressed will give a '0'
		{	// Switch 2 pressed
			lcd_Clear();				// Clear the display
			lcd_SetCursor(0x00);		// 0b00000010	Set cursor position to line 1, col 2
			lcd_WriteString("3-Multiplication");
			lcd_SetCursor(0x40);
			lcd_WriteString("*-ok #-back");
			select2();
		}
	
		if(NoKey != KeyValue)
		{
			//PORTB = ~KeyValue;		// Display actual value derived from matrix position
			DisplayKeyValue(KeyValue);	// Display special chars in different format
			DebounceDelay();
		}	
	}
}

// Test for confirming selection
select() {	
	while(1){
	KeyValue = ScanKeypad();	
	if( 10 == (KeyValue)){		
		lcd_Clear();				
		lcd_SetCursor(0x00);		
		lcd_WriteString("You selected");
		lcd_SetCursor(0x40);	
		lcd_WriteString("Addition");
		_delay_ms(2000);
		lcd_Clear();
		randomizer();
		//addition();		
	}
	if( 12 == (KeyValue)){		
		main();			
	}
	}
}

select1() {
	while(1){
		KeyValue = ScanKeypad();
		if( 10 == (KeyValue)){
			lcd_Clear();
			lcd_SetCursor(0x00);
			lcd_WriteString("You selected");
			lcd_SetCursor(0x40);
			lcd_WriteString("subtraction");
			_delay_ms(2000);
			lcd_Clear();
	
}
if(12 == (KeyValue)){
	main();
}
}
}

select2() {
	while(1){
		KeyValue = ScanKeypad();
		if(10 == (KeyValue)){
			lcd_Clear();
			lcd_SetCursor(0x00);
			lcd_WriteString("You selected");
			lcd_SetCursor(0x40);
			lcd_WriteString("multiplication");
			_delay_ms(2000);
			lcd_Clear();
			
		}
		if(12 == (KeyValue)){
			main();	
}
	}
}

randomizer() {	
		
		lcd_Clear();
		lcd_SetCursor(0x00);
		lcd_WriteString(" SW0 to start");
		lcd_SetCursor(0x40);
		lcd_WriteString(" SW1 to stop");	
		mano();		 
}		 
		
 /********************DELAY**************************/		 

void passnumber(unsigned char number){
	
	if ( number == 0)
	{
		number= number  + 1 ;
	}
	equation(number);
}

void equation(unsigned char number1){

	unsigned char random1;
	unsigned char random2;
	unsigned char random4;
    unsigned char random5;
	
	random1 = number1 * number2;
	random2 = number1 * number3;
	
	random4 = random1 % 5;
	random5 = random2 % 5;

	
	lcd_Clear();
	lcd_SetCursor(0x00);
	lcd_WriteChar(random4 + 48);
	lcd_SetCursor(0x01);
	lcd_WriteString("+");
	lcd_SetCursor(0x02);
	lcd_WriteChar(random5 + 48);
	lcd_SetCursor(0x03);
	lcd_WriteString("=");
	lcd_SetCursor(0x40);
	lcd_WriteString("Points:");
	lcd_SetCursor(0x47);
	lcd_WriteChar(points + 48);
	
	dispansw1 = random4;
	dispansw2 = random5;

	answer1 = random4 + random5;
	
	//EIMSK = 0b00000000;
		
    answer();
	
}

void answer()
 {
unsigned char KeyValue2;

KeyValue2 = ScanKeypad();



while (1)
{

		KeyValue2 = ScanKeypad();
		
		if (KeyValue2 == NoKey)
				{
					
				}
				
		
	 else if (KeyValue2 == answer1 )
		{
			lcd_Clear();
			_delay_ms(5000);
			lcd_SetCursor(0x00);
			lcd_WriteString("you gained");
			lcd_SetCursor(0x40);
			lcd_WriteString("1 point");
			points = points + 1;
			_delay_ms(2000);
			randomizer();
		}

		    else if (KeyValue2 != answer1){
			lcd_Clear();
			_delay_ms(2000);
			lcd_SetCursor(0x00);
			lcd_WriteString("no");
			lcd_SetCursor(0x40);
			lcd_WriteChar(dispansw1 + 48);
			lcd_SetCursor(0x41);
			lcd_WriteString("+");
			lcd_SetCursor(0x42);
			lcd_WriteChar(dispansw2 + 48);
			lcd_SetCursor(0x43);
			lcd_WriteString("=");
			lcd_SetCursor(0x44);
			lcd_WriteChar(answer1 + 48);
			_delay_ms(4000);
			randomizer();	
}
}
}

void InitialiseGeneral()
{
	DDRA = 0xFF;			// Configure PortA direction Output
	DDRC = 0xFF;			// Configure PortC direction Output
	DDRG = 0xFF;			// Configure PortG direction Output
	DDRE = 0b11111000;			// Configure PortE direction Input (Keypad)
	PORTE= 0b00000111;
	sei();	// Enable interrupts at global level set Global Interrupt Enable (I) bit
}
unsigned char ScanKeypad()
{
	unsigned char RowWeight;
	unsigned char KeyValue;

	// ScanRow0					// Row 0 is connected to port bit 6
	RowWeight = 0x01;		// Remember which row is being scanned
	PORTE = ScanKeypadRow0;	// Set bit 6 low (Row 0), bits 5,4,3 high (rows 1,2,3)
	KeyValue = ScanColumns(RowWeight);
	if(NoKey != KeyValue)
	{
		return KeyValue;
	}
	
	// ScanRow1					// Row 1 is connected to port bit 5
	RowWeight = 0x04;		// Remember which row is being scanned
	PORTE = ScanKeypadRow1;	// Set bit 5 low (Row 1), bits 6,4,3 high (rows 0,2,3)
	KeyValue = ScanColumns(RowWeight);
	if(NoKey != KeyValue)
	{
		return KeyValue;
	}
	// ScanRow2					// Row 2 is connected to port bit 4
	RowWeight = 0x07;		// Remember which row is being scanned
	PORTE = ScanKeypadRow2;	// Set bit 4 low (Row 2), bits 6,5,3 high (rows 0,1,3)
	KeyValue = ScanColumns(RowWeight);
	if(NoKey != KeyValue)
	{
		return KeyValue;
	}
	// ScanRow3					// Row 3 is connected to port bit 3
	RowWeight = 0x0A;		// Remember which row is being scanned
	PORTE = ScanKeypadRow3;	// Set bit 3 low (Row 3), bits 6,5,4 high (rows 0,1,2)
	KeyValue = ScanColumns(RowWeight);
	return KeyValue;
}
unsigned char ScanColumns(unsigned char RowWeight)
{
	// Read bits 7,6,5,4,3 as high, as only interested in any low values in bits 2,1,0
	unsigned char ColumnPinsValue;
	ColumnPinsValue = PINE | KeypadMaskColumns; // '0' in any column position means key pressed
	ColumnPinsValue = ~ColumnPinsValue;			// '1' in any column position means key pressed
	if(KeypadMaskColumn0 == (ColumnPinsValue & KeypadMaskColumn0))
	{
		return RowWeight;		// Indicates current row + column 0
	}	
	if(KeypadMaskColumn1 == (ColumnPinsValue & KeypadMaskColumn1))
	{
		return RowWeight + 1;	// Indicates current row + column 1
	}
	if(KeypadMaskColumn2 == (ColumnPinsValue & KeypadMaskColumn2))
	{
		return RowWeight + 2;	// Indicates current row + column 2
	}	
	return NoKey;	// Indicate no key was pressed
}
void DisplayKeyValue(unsigned char KeyValue)
{
	if(Star == KeyValue)
	{
		PORTB = ~0x2A;		// Special key '*' was pressed
		return;
	}
	if(Zero == KeyValue)
	{
		PORTB = ~0x4B;		// Special key '0' was pressed
		return;
	}
	if(Hash == KeyValue)
	{
		PORTB = ~0x8C;		// Special key '#' was pressed
		return;
	}
	PORTB = ~KeyValue;		// Regular numeric key was pressed
}
void DebounceDelay()	// This delay is needed because after pressing a key, the mechanical switch mechanism tends
// to bounce, possibly leading to many key presses being falsely detected. The debounce delay
// makes the program wait long enough for the bouncing to stop before reading the keypad again.
{
	for(int i = 0; i < 50; i++)
	{
		for(int j = 0; j < 255; j++);
	}
}

#Keypad

#include <string.h>

void lcd_wait();				//Check if the LCD device is busy, if so wait.
void lcd_WriteFunctionCommand();	//Output the function command in LCDreg to the LCD:
void lcd_ReadFunctionCommand();		//Read the function command from the LCD into LCDreg
void lcd_Clear();					//Clear the LCD display and set the cursor to the 'home' position
void lcd_StandardMode();			//Set LCD to 8-bit-mode, display window freeze and auto cursor increment
void lcd_SetCursor(unsigned char cursorPosition);	//Set cursor to a specific display position
void lcd_WriteChar(unsigned char cValue);			//Write the char in LCDreg at current cursor position and increment cursor
void lcd_WriteString(unsigned char []);
void lcd_on();						//Set LCD display on, cursor on and blink on
void lcd_CursorOn();				//Set Cursor on
void lcd_CursorOff();				//Set Cursor Off
void lcd_DisplayOn();				//Set Display on
void lcd_DisplayOff();				//Set Display off
void lcd_CursorPositionOff();		//Set Cursor Position Indicator Off
void lcd_BarGraph(unsigned char bar1, unsigned char bar2);	//Display 2-bar bar graph on LCD display
void lcd_ShiftLeft();	// shift LCD display one place to the left
void lcd_ShiftRight();	// shift LCD display one place to the right
void lcd_OneLineMode();	// configure LCD one-line mode
void lcd_TwoLineMode();	// configure LCD two-line mode
void lcd_WriteVariable_withValueAndPositionParameters(unsigned char row, unsigned char column, unsigned char data); // Display a variable IN RAW format at a user-specified position
void lcd_WriteVariable_withValueAndPositionParameters_SingleDecimalDigit(unsigned char row, unsigned char column, unsigned char data); // Display a single-digit DECIMAL value (0-9)

// Declare global variables required for the LCD library
unsigned char  LCDreg;

// PORTA control bits
//	BF=7
// PORTC control bits
//	RS=6
//	ENABLE=7
// PORTG control bits  (for 1281)
//	WR=0
//	RD=1

void lcd_Wait()		// Check if the LCD device is busy, if so wait
{	
	PORTC &= 0b00111111; // Clear enable (bit 7), clear Register select (bit 6) 
						 // (Set RS low (command register) so can read busy flag)
	PORTG &= 0b11111101; // Clear RD (bit 1) for read operation
	PORTG |= 0b00000001; // Set WR (bit 0) for read operation
	DDRA &= 0b01111111;  // High bit of port A is Busy Flag (BF, bit 7), set to input
	PORTC |= 0b10000000; // Set enable (bit 7)

	while(PINA & 0b10000000); // Wait here until busy flag is cleared

	PORTC &= 0b01111111; // Clear enable (bit 7)
	PORTG &= 0b11111110; // Clear WR (bit 0) for write operation
	PORTG |= 0b00000010; // Set RD (bit 1) for write operation
	DDRA |= 0b10000000;  // Restore bit 7 of port A to output (for Data use)
}

void lcd_WriteFunctionCommand()	// Output the function command in LCDreg to the LCD
{
	lcd_Wait(); 		// Wait if the LCD device is busy
	PORTC &= 0b10111111; // Clear Register select (bit 6) 
	PORTC |= 0b10000000; // Set enable (bit 7)
	PORTA = LCDreg;	// Send function command to command register, via port A
	PORTC &= 0b01111111; // Clear enable (bit 7)
}

void lcd_ReadFunctionCommand()	// Read the function command from the LCD into LCDreg
{	
	lcd_Wait(); 		// Wait if the LCD device is busy
	PORTC &= 0b10111111; // Clear Register select (bit 6)
	PORTC |= 0b10000000; // Set enable (bit 7)
	LCDreg = PORTA;	// Read the command register via port A
	PORTC &= 0b01111111; // Clear enable (bit 7)
}

void lcd_Clear()	// Clear the LCD display and set the cursor to the 'home' position
{	
	LCDreg = 0x01; 				// The 'clear' command
	lcd_WriteFunctionCommand(); // Output the command to the LCD
}

void lcd_StandardMode()	// Set the LCD to 8-bit-mode, display window freeze and
						// automatic cursor increment (standard mode)
{
	LCDreg = 0b00111000; 			// Command for 8-Bit-transfer
	lcd_WriteFunctionCommand(); 	// Output the command to the LCD
	LCDreg = 0b00000110; 			// Command for Increment, display freeze
	lcd_WriteFunctionCommand(); 	// Output the command to the LCD
	LCDreg = 0b00010000; 			// Command for Cursor move, not shift
	lcd_WriteFunctionCommand(); 	// Output the command to the LCD
}

void lcd_SetCursor(unsigned char cursorPosition)	// Set cursor on the LCD to a specific display position
{
	LCDreg = cursorPosition | 0b10000000; 	// Set bit 7 of the byte
	lcd_WriteFunctionCommand(); 	// Output the command to the LCD
}

void lcd_WriteChar(unsigned char cValue)	// Write the character in LCDreg to the display at the current cursor
											// position (the position is incremented after write)	
{
	lcd_Wait(); 		// Wait if the LCD device is busy
	PORTC |= 0b11000000;// Set enable (bit 7), Set RS (bit 6) (for data)
	PORTA = cValue;  	// Write data character to LCD
	PORTC &= 0b01111111;// Clear enable (bit 7)
}

void lcd_WriteString(unsigned char Text[])
{
	unsigned char Index;
	for(Index = 0;Index < strlen(Text); Index++)
	{
		lcd_WriteChar(Text[Index]);
	}
}
 
void lcd_on()	// Set LCD display on, cursor on and blink on
{
	LCDreg = 0b00001111; 			// Combined command byte
	lcd_WriteFunctionCommand(); 	// Output the command to the LCD
}

void lcd_CursorOn()// Set Cursor on	
{
	lcd_ReadFunctionCommand();	// Input the command register value into the LCDreg
	LCDreg |= 0b00000010; 		// Set bit 1 of the position. Command value to set cursor on
	lcd_WriteFunctionCommand(); 	// Output the command to the LCD
}

void lcd_CursorOff()	// Set Cursor Off	
{
	lcd_ReadFunctionCommand();	// Input the command register value into the LCDreg
	LCDreg &= 0b11111101;		// Command value to set cursor off
	lcd_WriteFunctionCommand(); // Output the command to the LCD
}

void lcd_DisplayOn()	// Set Display on
{
	lcd_ReadFunctionCommand();	// Input the command register value into the LCDreg
	LCDreg |= 0b00000100; 		// Set bit 2 of the position. Command value to set display on
	lcd_WriteFunctionCommand(); // Output the command to the LCD
}

void lcd_DisplayOff()	// Set Display off
{
	lcd_ReadFunctionCommand();	// Input the command register value into the LCDreg
	LCDreg &= 0b11111011;		// Command value to set display off
	lcd_WriteFunctionCommand();	// Output the command to the LCD
}

void lcd_CursorPositionOff()	// Set Cursor Position Indicator Off
{
	LCDreg = 0b00001100; 			// Command value to set cursor position indicator off
	lcd_WriteFunctionCommand(); 	// Output the command to the LCD
}

void lcd_BarGraph(unsigned char bar1, unsigned char bar2) 
// Display 2-bar bar graph on LCD display
// The bar lengths are given by the values in Bar1 and Bar2
{
	unsigned char  BarGraph1 = bar1;
	unsigned char  BarGraph2 = bar2;
	lcd_CursorOff();	
	lcd_CursorPositionOff();	
	lcd_Clear();
	lcd_SetCursor(0x00);   // Set cursor position to line 1, col 0

	while(BarGraph1 > 0)	// Check for 0 bar height
	{
		lcd_WriteChar(0x11); // 0x11 is the best character code for the bar element
		BarGraph1--;
	}

	lcd_SetCursor(0x40);   // Set cursor position to line 2, col 0

	while(BarGraph2 > 0)	// Check for 0 bar height
	{
		lcd_WriteChar(0x11); // 0x11 is the best character code for the bar element
		BarGraph2--;
	}
}

void lcd_ShiftLeft()		// Configure LCD shift Left mode
{
	LCDreg = 0b00011000; 			// Command value to set shift Left (see LCD manual PDF)
	lcd_WriteFunctionCommand(); 	// Output the command to the LCD
}
	
void lcd_ShiftRight()	// Configure LCD shift Right mode
{
	LCDreg = 0b00011100; 			// Command value to set shift Right (see LCD manual PDF)
	lcd_WriteFunctionCommand(); 	// Output the command to the LCD
}

void lcd_OneLineMode()	// Configure LCD one-line mode
{
	LCDreg = 0b00110100; 			// Command value to set to one-line mode (see LCD manual PDF)
	lcd_WriteFunctionCommand(); 	// Output the command to the LCD
}

void lcd_TwoLineMode()	// Configure LCD Two-line mode
{
	LCDreg = 0b00111100;			// Command value to set to two-line mode (see LCD manual PDF)
	lcd_WriteFunctionCommand(); 	// Output the command to the LCD
}

void lcd_WriteVariable_withValueAndPositionParameters(unsigned char row, unsigned char column, unsigned char data)
{
	// The LCD has 2 rows R = {0 - 1} and 16 columns C = {0 - 15}
	// The cursor position is defined in a single byte as follows: 0b0B00CCCC, so
	// need to mask and shift the passed-in values, before calling lcd_SetCursor()
	column &= 0b00001111;	// Only lower 4 bits are valid, Mask off upper 4 bits
	row &= 0b00000001;		// Only lowest bit is valid, Mask off upper 7 bits
	row *= 64;				// Shift row value 6 places to the left
	lcd_SetCursor(column + row);	// Set the cursor position
	lcd_WriteChar(data);	// Write the data byte to the display at current cursor position
}

void lcd_WriteVariable_withValueAndPositionParameters_SingleDecimalDigit(unsigned char row, unsigned char column, unsigned char data)
{
	// The LCD has 2 rows R = {0 - 1} and 16 columns C = {0 - 15}
	// The cursor position is defined in a single byte as follows: 0b0R00CCCC, so
	// need to mask and shift the passed-in values, before calling lcd_SetCursor()
	column &= 0b00001111;	// Only lower 4 bits are valid, Mask off upper 4 bits
	row &= 0b00000001;		// Only lowest bit is valid, Mask off upper 7 bits
	row *= 64;				// Shift row value 6 places to the left
	lcd_SetCursor(column + row);	// Set the cursor position
	data += '0';			// Add ASCII '0' to the data value (convert a decimal number into its character representation)
	lcd_WriteChar(data);	// Write the data byte to the display at current cursor position
}

#Timer

#include <avr/io.h>
#include <avr/interrupt.h>

// Declare functions (these will be in a separate header file in larger programs)
void InitialiseGenerall();
void Initialise_HW_Interrupts();
void InitialiseTimer1();
void DisplayLED();

// Declare global variables
unsigned char ElapsedSeconds_Count;	// An 'unsigned char' is an 8-bit numeric value, i.e. a single byte

int mano( void )
{
	InitialiseGenerall();
	ElapsedSeconds_Count = 0x00;		// Initialise the Elapsed Time counter
	Initialise_HW_Interrupts();
	InitialiseTimer1();
	InitialiseGeneral();
	
	while(1)        		// Loop
	{
		// In this example all the application logic is within the Timer1 interrupt handler
	}
}

void InitialiseGenerall()
{
	DDRB = 0xFF;			// Configure PortB direction for Output
	PORTB = 0xFF;			// Set all LEDs initially off (inverted on the board, so '1' = off)
	DDRD = 0x00;			// Configure PortD direction for Input
	sei();
}

void Initialise_HW_Interrupts()
{
	EICRA = 0b00000101;		// INT 3,2 not used, Interrupt Sense (INT1, INT0) falling-edge triggered
	EICRB = 0b00000000;		// INT7 ... 4 not used
	
	EIMSK = 0b00000011;		// Enable INT1, INT0
	EIFR = 0b00000011;		// Clear INT1 and INT0 interrupt flags (in case a spurious interrupt has occurred during chip startup)
}

ISR(INT0_vect) // Hardware_INT0_Handler (Interrupt Handler for INT0)
{
	ElapsedSeconds_Count = 0;	// Clear the elapsed time counter
	TCCR1B = 0b00001010;		// Start the timer (CTC and Prescaler 1024)
}

ISR(INT1_vect) // Hardware_INT1_Handler (Interrupt Handler for INT1)
{
	TCCR1B = 0b00001000;	// Stop the timer (CTC, no clock)
	DisplayLED();	
          	 	// Display the amount of time elapsed
}

void InitialiseTimer1()		// Configure to generate an interrupt after a 1-Second interval
{
	TCCR1A = 0b00000000;	// Normal port operation (OC1A, OC1B, OC1C), Clear Timer on 'Compare Match' (CTC) waveform mode)
	TCCR1B = 0b00001000;	// CTC waveform mode, initially stopped (no clock)
	TCCR1C = 0b00000000;
	
	// For 1 MHz clock (with 1024 prescaler) to achieve a 1 second interval:
	// Need to count 1 million clock cycles (but already divided by 1024)
	// So actually need to count to (1000000 / 1024 =) 976 decimal, = 3D0 Hex
	OCR1AH = 0x03; // Output Compare Registers (16 bit) OCR1BH and OCR1BL
	OCR1AL = 0xD0;

	TCNT1H = 0b00000000;	// Timer/Counter count/value registers (16 bit) TCNT1H and TCNT1L
	TCNT1L = 0b00000000;
	TIMSK1 = 0b00000010;	// bit 1 OCIE1A		Use 'Output Compare A Match' Interrupt, i.e. generate an interrupt
							// when the timer reaches the set value (in the OCR1A register)
}

ISR(TIMER1_COMPA_vect) // TIMER1_CompareA_Handler (Interrupt Handler for Timer 1)
{
	ElapsedSeconds_Count++; // Increment the number of elapsed seconds while the timer has been running
}

void DisplayLED()
{
	unsigned char Output_LED_value;
	unsigned char normal;
	normal = ElapsedSeconds_Count;
	Output_LED_value = ~ElapsedSeconds_Count;   // Take one's complement, because on-board LEDs are inverted (i.e. 0 = ON, 1 = OFF)
	PORTB = Output_LED_value;
	normal = normal % 5;
	passnumber(normal);
}

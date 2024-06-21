Below is the code I used for the Arduino Uno R3 and Buzzer Game Hat. <br> <br>


```ruby
// Define the LED pins
const int RED1 = 0;        // Player 1 Red LED
const int BLUE1 = 1;       // Player 1 Blue LED
const int GREEN1 = 2;      // Player 1 Green LED

const int RED2 = 6;        // Player 2 Red LED
const int BLUE2 = 5;       // Player 2 Blue LED
const int GREEN2 = 4;      // Player 2 Green LED

const int buttonA = 8;     // Button A
const int buttonB = 9;     // Button B
const int buttonC = 3;     // Button C
const int sleepButton = 19; // Sleep Button A5
const int MuteButton = 16;  // Mute Button A2

const int BuzzerP1 = 10;  // Player 1 Buzzer
const int BuzzerP2 = 11;  // Player 2 Buzzer
const int ClearLED = 12;  // Clear Button LED

// This Wait for Clear tag will not let any buzzers or color changes until the CLear Button is pressed.
int WaitForClear = 0; // Initialize the variable

// Variables to store the previous state of each button
bool prevButtonAState = LOW;
bool prevButtonBState = LOW;
bool prevButtonCState = HIGH; // Initial state is HIGH because of internal pull-up resistor
bool sleepButtonState = HIGH; // Initial state is HIGH because of internal pull-up resistor
bool MuteButtonState = HIGH;  // Initial state is HIGH because of internal pull-up resistor

void setup() {

  //Serial.begin(9600); // Start the Serial communication
  //Serial.println("Game is Starting ");
  //Serial.println("");

  // Initialize each pin as an output
  pinMode(RED1, OUTPUT);
  pinMode(BLUE1, OUTPUT);
  pinMode(GREEN1, OUTPUT);
  pinMode(RED2, OUTPUT);
  pinMode(BLUE2, OUTPUT);
  pinMode(GREEN2, OUTPUT);
  pinMode(ClearLED, OUTPUT);

  // Initialize the digital pins for buzzers as outputs
  pinMode(BuzzerP1, OUTPUT);
  pinMode(BuzzerP2, OUTPUT);

  // Set initial states for buzzers
  digitalWrite(BuzzerP1, LOW);
  digitalWrite(BuzzerP2, LOW);

  // Initialize the digital pins for buttons as inputs
  pinMode(buttonA, INPUT);         // Button A with external pull-down resistor
  pinMode(buttonB, INPUT);         // Button B with external pull-down resistor
  pinMode(buttonC, INPUT_PULLUP);  // Button C with internal pull-up resistor
  pinMode(sleepButton, INPUT);     // Sleep button with internal pull-DOWN resistor
  pinMode(MuteButton, INPUT);     // Mute button with internal pull-DOWN resistor

  // Turn on all LEDs in order for a startup sequence
  ClearLEDs();
  delay(500);

  digitalWrite(GREEN1, HIGH);
  digitalWrite(GREEN2, HIGH);

  delay(250);
  digitalWrite(GREEN1, LOW);
  digitalWrite(GREEN2, LOW);
  digitalWrite(BLUE1, HIGH);
  digitalWrite(BLUE2, HIGH);

  delay(250);
  digitalWrite(BLUE1, LOW);
  digitalWrite(BLUE2, LOW);
  digitalWrite(RED1, HIGH);
  digitalWrite(RED2, HIGH);

  delay(250);

  digitalWrite(RED1, LOW);
  digitalWrite(RED2, LOW);
  digitalWrite(BLUE1, HIGH);
  digitalWrite(BLUE2, HIGH);
}

void loop() {
  delay(50);
  bool buttonAState = digitalRead(buttonA);
  bool buttonBState = digitalRead(buttonB);
  bool buttonCState = digitalRead(buttonC);
  bool sleepButtonState = digitalRead(sleepButton);
  bool MuteButtonState = digitalRead(MuteButton);
  
  //Serial.println("");    
  //Serial.print(MuteButtonState);

  if (buttonAState == HIGH && WaitForClear == 0) {
    Player1Wins(MuteButtonState);
    WaitForClear = 1;
  } 
  if (buttonBState == HIGH && WaitForClear == 0) {
    Player2Wins(MuteButtonState);
    WaitForClear = 1;
  }

  // Check if the sleep button is pressed
  if (sleepButtonState == HIGH) {  //Change to Low when using it in the box
    digitalWrite(ClearLED, LOW); 
    digitalWrite(BuzzerP1, LOW); 
    digitalWrite(BuzzerP2, LOW); 
    digitalWrite(ClearLED, LOW);
    ClearLEDs();
    while (digitalRead(sleepButton) == HIGH) {
      delay(50);    // Wait until the sleep button is released
    }
    ClearLEDs();
    digitalWrite(BLUE1, HIGH);
    digitalWrite(BLUE2, HIGH);
    WaitForClear = 0;
  }

  if (buttonCState == LOW && WaitForClear == 1) {
    ClearLEDs();
    digitalWrite(BLUE1, HIGH);
    digitalWrite(BLUE2, HIGH);
    digitalWrite(ClearLED, LOW);
    WaitForClear = 0;
  }
}

void ClearLEDs() {
  digitalWrite(RED1, LOW);
  digitalWrite(RED2, LOW);
  digitalWrite(GREEN1, LOW);
  digitalWrite(GREEN2, LOW);
  digitalWrite(BLUE1, LOW);
  digitalWrite(BLUE2, LOW);
  digitalWrite(ClearLED, LOW);
  digitalWrite(BuzzerP1, LOW);
  digitalWrite(BuzzerP2, LOW);
}

void Player1Wins(bool MuteButtonState) {
  if (MuteButtonState == 1) { 
    digitalWrite(BuzzerP1, HIGH);
  }
  digitalWrite(BuzzerP2, LOW);
  digitalWrite(GREEN1, HIGH);
  digitalWrite(BLUE1, LOW);
  digitalWrite(RED1, LOW);
  digitalWrite(GREEN2, LOW);
  digitalWrite(BLUE2, LOW);
  digitalWrite(RED2, HIGH);
  digitalWrite(ClearLED, HIGH);
  delay(50);
  digitalWrite(BuzzerP1, LOW);
}

void Player2Wins(bool MuteButtonState) {
  if (MuteButtonState == 1) { 
    digitalWrite(BuzzerP2, HIGH);
  }
  digitalWrite(BuzzerP1, LOW);
  digitalWrite(GREEN2, HIGH);
  digitalWrite(BLUE2, LOW);
  digitalWrite(RED2, LOW);
  digitalWrite(GREEN1, LOW);
  digitalWrite(BLUE1, LOW);
  digitalWrite(RED1, HIGH);
  digitalWrite(ClearLED, HIGH);
  delay(50);
  digitalWrite(BuzzerP2, LOW);
}


```

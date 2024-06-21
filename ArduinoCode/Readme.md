Below is the code I used for the Arduino Uno R3 and Buzzer Game Hat. <br> <br>


```ruby
// Define the LED pins

const int sleepButton = 14; // #Sleep Button         A0
const int MuteButton = 15;  // #Mute Button          A1
const int RED1 = 16;        // ##Player 1 Red LED    A2
const int BLUE1 = 17;       // ##Player 1 Blue LED   A3
const int GREEN1 = 18;      // ##Player 1 Green LED  A4
const int buttonC = 19;     // ##Button C -          A5

const int BuzzerP2 = 9;     // #Player 2 Buzzer -    D9
const int BuzzerP1 = 8;     // #Player 1 Buzzer -    D8
const int buttonB = 7;      // #Button B -          D7
const int buttonA = 6;      // #Button A -          D6
const int ClearLED = 5;     // #Clear Button LED -   D5
const int RED2 = 4;         // #Player 1 Red LED    D4
const int BLUE2 = 3;        // #Player 1 Blue LED   D3
const int GREEN2 = 2;       // #Player 1 Green LED  D2


int WaitForClear = 0; // #Initialize the variable

// #Variables to store the previous state of each button
bool prevButtonAState = LOW;
bool prevButtonBState = LOW;
bool prevButtonCState = HIGH; // #Initial state is HIGH because of internal pull-up resistor
bool sleepButtonState = HIGH;
bool MuteButtonState = HIGH;

void setup() {
  // #Initialize each pin as an output
  // #Serial.begin(9600); // Start the Serial communication
  // #Serial.println("Game is Starting ");
  // #Serial.println("");

  pinMode(RED1, OUTPUT);
  pinMode(BLUE1, OUTPUT);
  pinMode(GREEN1, OUTPUT);
  pinMode(RED2, OUTPUT);
  pinMode(BLUE2, OUTPUT);
  pinMode(GREEN2, OUTPUT);
  pinMode(ClearLED, OUTPUT);

  // #Initialize the digital pins for buzzers as outputs
  pinMode(BuzzerP1, OUTPUT);
  pinMode(BuzzerP2, OUTPUT);

  // #Set initial states for buzzers
  digitalWrite(BuzzerP1, LOW);
  digitalWrite(BuzzerP2, LOW);

  // #Initialize the digital pins for buttons as inputs
  pinMode(buttonA, INPUT);         // #Button A with external pull-down resistor
  pinMode(buttonB, INPUT);         // #Button B with external pull-down resistor
  pinMode(buttonC, INPUT_PULLUP);  // #Button C with internal pull-up resistor
  pinMode(sleepButton, INPUT); // #Sleep button with internal pull-DOWN resistor
  pinMode(MuteButton, INPUT); // #Mute button with internal pull-DOWN resistor

  // #Turn on all LEDs
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
  
  //#Serial.println("");    
  //#Serial.print(MuteButtonState);

  if (buttonAState == HIGH && WaitForClear == 0) {
    Player1Wins(MuteButtonState);
    WaitForClear = 1;
  } 
  if (buttonBState == HIGH && WaitForClear == 0) {
    Player2Wins(MuteButtonState);
    WaitForClear = 1;
  }

  // #Check if the sleep button is pressed
  if (sleepButtonState == HIGH) {  //# Change to Low when using it in the box
    digitalWrite(ClearLED, LOW); 
    digitalWrite(BuzzerP1, LOW); 
    digitalWrite(BuzzerP2, LOW); 
    digitalWrite(ClearLED, LOW);
    ClearLEDs();
    while (digitalRead(sleepButton) == HIGH) {
      delay(50);    // #Wait until the sleep button is released
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

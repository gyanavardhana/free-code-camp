## Hardware Components and Pin Configuration

**LCD Display (LiquidCrystal)**
- Pins: 6, 7, 5, 4, 3, 2
- Purpose: To display status messages and detection results

**GSM Module (SoftwareSerial)**
- Pins: 8 (RX), 9 (TX)
- Purpose: Sends SMS alerts when weapons are detected

**Motor Control**
- Pin 11 (m1a): Motor terminal A
- Pin 10 (m1b): Motor terminal B
- Purpose: Controls physical movement/barrier when weapon detected

**Other Components**
- Pin 12 (relay): Controls high-voltage components
- Pin 13 (buzzer): Audio alert when weapon is detected

## Global Variables

- `inputString` and `inputString1`: Store incoming serial data
- `stringComplete` and `stringComplete1`: Flags indicating complete messages
- `rcv` and `pastnumber`: For GSM communication (stores phone number)
- `sts1`: Status counter for detection events

## Main Functions

### `setup()`
- Initializes all pins (motors, relay, buzzer)
- Sets initial states (all outputs LOW, buzzer HIGH which is off)
- Configures Serial communication (9600 baud)
- Displays startup messages on LCD about "Weapon Detection Using Artificial Intelligence and Deep Learning"

### `loop()`
- Continuously checks for complete messages from the serial port
- When messages are complete:
  - If message is "GUN":
    1. Shows "Weapon Detected" on LCD
    2. Activates relay (HIGH)
    3. Sounds buzzer (LOW)
    4. Runs motor briefly to operate barrier mechanism (400ms)
    5. Waits 3 seconds
    6. Turns off relay
    7. Stops buzzer
    8. Sends SMS alert to registered number
  - If message is "None":
    1. Displays "None" on LCD (no weapon detected)

### `serialEvent()`
- Handles incoming serial data
- Parses messages between special characters (* and #)
- Populates both `inputString` and `inputString1` based on incoming data

### `readSerial()`
- Reads data from GSM module
- Processes line ending characters

### `gsminit()`
- Initializes GSM module with AT commands
- Sets up text messaging mode
- Registers a mobile number for alerts
- Sends confirmation SMS

### `okcheck1()`
- Helper function to wait for "OK" confirmation from GSM module

## Workflow Explanation

1. **System Initialization**:
   - Setup pins, communication, display welcome message
   
2. **Detection Loop**:
   - System continuously monitors serial input (from a connected detection system)
   - The detection results come in via serial with format `*RESULT#`

3. **Weapon Detection Response**:
   - When "GUN" is detected:
     - Visual alert on LCD
     - Sound alert via buzzer
     - Physical response via motor movement
     - SMS notification to registered number

4. **String Parsing Logic**:
   - Messages are parsed between * and # characters
   - This suggests the system receives messages like "*GUN#" or "*None#"
   - There's also a second string parser for additional information that may display on LCD

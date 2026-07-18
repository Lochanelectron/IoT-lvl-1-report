# IoT lvl 1  
## Task 1a
GPIO (General Purpose Input/Output) pins are programmable pins that can either read signals (input) or control devices (output).

Input Mode: The ESP32 reads the voltage level on the pin.

HIGH (3.3 V) = Logic 1

LOW (0 V) = Logic 0

Output Mode: The ESP32 drives the pin HIGH or LOW to control external devices such as LEDs, relays, or buzzers.

Pull-up resistor: Connects the pin to 3.3 V, so the default state is HIGH. Pressing a button connected to GND changes it to LOW.

Pull-down resistor: Connects the pin to GND, so the default state is LOW. Pressing a button connected to 3.3 V changes it to HIGH.

An interrupt allows the ESP32 to immediately pause its current task and execute a special function called an Interrupt Service Routine (ISR) when a specific event occurs, such as a button press.

Example:

- A button connected to a GPIO pin generates an interrupt when pressed.
- The ISR runs instantly to handle the event.
- After the ISR finishes, the ESP32 resumes its previous task

Polling: The CPU continuously checks the button state inside the loop(), wasting processing time and increasing power consumption.

Interrupts: The CPU performs other tasks and only responds when the button is actually pressed.

Advantages of interrupts:

- Faster response to button presses.
- Lower CPU usage and power consumption.
- Allows multitasking without constantly checking inputs.
- More efficient for real-time applications.

## Task 1b
Using pull up resistors
### 1. System Initialization

When the ESP32 powers on:

GPIO pins connected to LEDs are configured as outputs.
GPIO pins connected to push buttons are configured as inputs using the ESP32's internal pull-up resistors.
A random seed is generated using the ESP32 timer to ensure that every game produces a different sequence.
The first random LED is generated to begin the game.

### 2. Random Sequence Generation

The ESP32 stores the LED sequence inside an integer array.

Example:

Sequence:

2
2 1
2 1 0
2 1 0 2

where

0 = Red LED
1 = Green LED
2 = Blue LED

Each successful round appends one additional random number (0–2) to the sequence.

### 3. Displaying the Sequence

The ESP32 iterates through the stored sequence.

For each value:

Turn ON the corresponding LED.
Wait for a short duration.
Turn OFF the LED.
Pause briefly before displaying the next LED.

Example:

Green
Pause
Blue
Pause
Red

The player must memorize this order.

- The ESP32 identifies which button was pressed.
- The corresponding LED flashes briefly to provide visual feedback.
- The input is stored temporarily for comparison.

Button debouncing is implemented by introducing a small delay after each button press. This prevents multiple detections caused by the mechanical bouncing of the push button contacts.

#### Algorithm
1. Initialize LEDs and push buttons.
2. Generate the first random LED.
3. Display the sequence.
4. Wait for user input.
5. Compare each button press with the stored sequence.
6. If all inputs are correct:
- Increase the level.
- Append a random LED.
- Repeat from Step 3.
7. If any input is incorrect:
- Flash all LEDs.
- Reset the game.
- Return to Step 2.

## Task 2
<img width="500" height="270" alt="image" src="https://github.com/user-attachments/assets/232e4cdf-0591-4297-bbe3-138ff4fd6c59" />
​
x

- The Physics: The substrate layer absorbs water vapor from the surrounding air.
​
- The Change: As the air gets more humid, the substrate absorbs more water droplets. Because water conducts electricity, this increases the electrical conductivity between the two electrodes (or shifts the electrical resistance).
​
- The Reading: The internal chip measures this change in resistance. Higher moisture leads to lower resistance, which the chip maps directly to a Relative Humidity (RH) percentage

#### Temperature
For temperature, the DHT11 uses a negative temperature coefficient (NTC) thermistor. A thermistor is essentially a thermal resistor—a variable resistor whose electrical resistance changes drastically based entirely on temperature.
​
"Negative Coefficient": "Negative" simply means that as the temperature goes up, the electrical resistance goes down (they move in opposite directions).
​The Reading: The component is typically made from sintered metal oxides or semiconductors. As the ambient air heats or cools the material, its resistance fluctuates predictably. The internal microchip constantly tracks this value to calculate the exact temperature in Celsius.

If - else statements to use fan with PWM.

Speed Control: Pulse Width Modulation (PWM)
​
A DC motor’s speed depends directly on the voltage applied to it. If you have a 12V motor, giving it 12V makes it go full speed, and 6V makes it go roughly half speed.
​However, dropping voltage using standard components (like a resistor) drops efficiency because the excess energy is wasted as massive amounts of heat. PWM solves this by turning the power fully ON and fully OFF thousands of times per second.
​
Instead of delivering a steady, lower voltage, PWM delivers rapid pulses of full voltage. The motor reacts to the average voltage over time because its internal mechanical inertia and electrical inductance smooth out the rapid spikes.



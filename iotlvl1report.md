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

## Task 3a

The Internet of Things (IoT) is a network of physical devices (such as sensors, microcontrollers, and actuators) that collect, exchange, and process data over a network. IoT enables devices to communicate with each other or with cloud services for monitoring, automation, and remote control.

Key Features:

- Connects smart devices via the internet or local networks.
- Enables real-time data collection and monitoring.
- Supports automation and remote access.
- Commonly uses protocols such as MQTT, HTTP, CoAP, I2C, and SPI.

### MQTT (Message queing telemetry transport)
MQTT is a lightweight publish-subscribe messaging protocol designed for IoT devices with limited bandwidth and power.

- Uses a broker (e.g., Mosquitto) to relay messages.
- Devices publish data to a topic, while others subscribe to receive updates.
- Low bandwidth and power consumption.
- Best for sensor monitoring, smart homes, and industrial IoT.

Advantages:

- Fast and lightweight.
- Reliable with Quality of Service (QoS) levels.
- Efficient for wireless networks.

### HTTP (Hypertext transfer protocol)
HTTP is a client-server communication protocol commonly used for websites and web APIs.

- A client sends a request, and the server returns a response.
- Suitable for web dashboards, REST APIs, and cloud communication.
- Easier to implement but consumes more bandwidth than MQTT.

Advantages:

- Simple and widely supported.
- Ideal for web applications and cloud services.

Disadvantages:

- Higher network overhead.
- Less efficient for continuous IoT messaging.

### I2C (inter integrated circuit)
I2C is a serial communication protocol used for communication between devices on the same circuit board.

Uses only 2 wires:
- SDA (Data)
- SCL (Clock)
- Supports multiple master and slave devices using unique addresses.
- Commonly used with sensors, OLED displays, RTC modules, and EEPROMs.

Advantages:

- Simple wiring.
- Multiple devices can share the same bus.

Disadvantages:

- Slower than SPI.
- Limited communication distance.

### SPI (serial pheripheral interface)
SPI is a high-speed serial communication protocol for communication between a microcontroller and peripheral devices.

Uses 4 wires:
- MOSI (Master Out Slave In)
- MISO (Master In Slave Out)
- SCK (Clock)
- CS/SS (Chip Select)
- Provides full-duplex communication (simultaneous sending and receiving).

Advantages:

- Very fast data transfer.
- Suitable for displays, SD cards, and high-speed sensors.

Disadvantages:

- Requires more wiring than I2C.
- Each slave device typically needs its own Chip Select pin.

## Task 3b
- Publisher: Sends messages to a specific topic.
- Broker: Receives published messages and forwards them to all subscribed devices.
- Subscriber: Listens to a topic and performs an action when a message is received.

After powering on, the ESP32 first established a Wi-Fi connection and then connected to the MQTT broker. Once connected, it subscribed to the designated topic and waited for incoming messages. Whenever the publisher transmitted a command, the broker immediately forwarded it to the ESP32. The callback function inside the program processed the received message, identified which LED the command referred to, and set the appropriate GPIO output HIGH or LOW. This enabled real-time wireless control of the LEDs through MQTT without requiring a direct connection between the publisher and the ESP32.

## Task 4
- The master ESP32 initializes Wi-Fi and starts an HTTP web server.
- A webpage containing a text input field is served to the user.
- When a message is submitted, the web server reads the input.
- The master begins an I²C transmission to the slave's address and sends the message bytes.
- The slave receives the transmitted bytes through the I²C receive callback function.
- The received message is stored and displayed on the SSD1306 OLED using the Adafruit SSD1306 library.
- The OLED updates whenever a new message is received, replacing the previous text.

## Task 5
will be backk soon :)

## Task 6

<img width="743" height="252" alt="image" src="https://github.com/user-attachments/assets/b650fe1a-9353-4483-b307-76fe0ef64a91" />
x

- The ESP32 connects to the local Wi-Fi network and starts a web server.
- A client opens the ESP32's IP address in a browser and enters a message.
- The web server receives the message through an HTTP request.
- Each character is searched in a lookup table to obtain its Morse code equivalent.
- The Morse symbols are interpreted as LED blink patterns:
- Dot (.) → Short LED ON duration.
- Dash (-) → Long LED ON duration.
- Spaces → Longer pause between words.
- The LED continues blinking until the entire message has been transmitted.

## Task 7
could not configure SPO2 sensor T-T;

Once the LED is located on the vein, then the LED starts emitting light. Once the heart is pumping, then there will be a flow of blood within the veins. So if we check the blood flow, then we can check the heart rates also.

If the blood flow is sensed then the ambient light sensor will receive more light as they will be reproduced by the flow of blood. This small change within obtained light can be examined over time to decide our pulse rates.

- Connected the pulse sensor to the ESP32's 3.3V, GND, and analog input pin (GPIO34).
- Configured the ESP32 to connect to a mobile hotspot and start an HTTP web server.
- Read the analog signal from the pulse sensor and detected heartbeats using a predefined threshold.
- Calculated the BPM from the time interval between successive beats.
- Created a web dashboard that periodically fetched the latest BPM value from the ESP32.
- Displayed the BPM both as a numerical value and as a continuously updating line graph.

## Task 8
### Core Problem
A device on a home WiFi network isn't reachable from the open internet by default.
Most home connections also sit behind CGNAT, ruling out simple port-forwarding.
The system therefore needed a way for both control and video to cross the internet
without the car ever needing an inbound-reachable public address, and without
renting any server.
 
### Design Decision: Public MQTT Broker as the Meeting Point
Instead of the car listening for incoming connections, both onboard devices
**connect outward** to a free public MQTT broker (`broker.hivemq.com`). Outbound
connections pass through NAT/CGNAT without any router configuration. A phone or
laptop anywhere also connects to the same broker (over MQTT-over-WebSocket, so a
plain browser page can do it with no app). The broker becomes the shared meeting
point — nobody needs a public IP.
 
- **Control channel**: browser publishes single-letter commands (`F`/`B`/`L`/`R`/`S`)
  to a control topic; the ESP32 subscribes and drives the motors accordingly.
  Round-trip latency over MQTT is low enough (~100-300ms) to feel responsive.
- **Video channel**: true live video streaming wasn't practical over a public
  broker (bandwidth/message-size limits, plus this project rules out a VPS which
  is what real-time video relaying normally needs). Instead, the ESP32-CAM
  captures a still JPEG every ~1.5s, base64-encodes it, and publishes it to a
  snapshot topic. The browser decodes and displays it as it arrives — a
  surveillance-camera cadence, not a video call, but sufficient for the stated goal.

### Security Note
Public brokers offer no authentication. The mitigation used was a long, random,
unique topic prefix — functionally like an unlisted URL, not real access control.
Acceptable for a hobby build; not suitable for anything sensitive.

## Task 9
The IR flame sensor detects infrared radiation emitted by a flame. When a flame is detected, its digital output changes state, which is read by the ESP32. The ESP32 then establishes a secure internet connection and sends an HTTP request to the Twilio API. Twilio authenticates the request using the Account SID and Auth Token before sending an SMS alert to the specified recipient. To avoid repeated notifications, the program uses a flag that allows only one SMS to be sent until the flame is no longer detected.

- Connected the IR flame sensor to the ESP32 using the digital output pin.
- Configured the ESP32 to connect to a Wi-Fi network.
- Programmed the ESP32 to continuously monitor the flame sensor.
- When a flame was detected, the ESP32 sent an HTTPS POST request to the Twilio REST API containing the sender number, recipient number, and alert message.
- Twilio processed the request and delivered an SMS notification to the registered mobile number.






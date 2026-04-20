# Occupancy Edge Node

## Role and Functionality
The Occupancy Edge Node act as the central processing unit of the system, responsible for maintaing an accurate and consistent passenge count. It receives input from station edge nodes whenever passesgers enter or exit and updates the total occupancy accordingly.

Its key responsibilities include:
* Processing incoming passenger events (Passenser entered/Passenger left)
*Maintaining a valid occupancy value (preventing negative counts)
*Broadcasting updated occupancy to all connected nodes
*Controlling local outputs such as LEDs and display messages
This node ensures all parts of the system operate using the same occupancy data, making it the single source of truth.

## Suitable Hardware
The prototype uses a Raspberry Pi 4 Model due to its reliable performance, GPIO support and networking capabilities. However, for scalability, altertives such as ESP32 could be used, offereing built-in WIFI and lower cost for large deployments. More advanced boards like the NvIDIA Jetson Nano could suppport future extensions such as AI-based passeger detection.

# N0PS CTF 2024: HID 

## Solution

- filter the data looking for hid data using `usbhid.data`
- there are two devices: a keyboard (which sends only one packet) and a mouse (pointing device) which sends many packets
- Isolate the mouse traffic (filter out the keyboard) and save it to a new file
<img width="1704" height="1462" alt="image" src="https://github.com/user-attachments/assets/567906f3-153e-4271-b9ee-9c9d1cbacd05" />

- solution is a Python script that parses the mouse movement data
- It uses scapy to read the pcap file and matplotlib to plot the movements.

```python
import matplotlib.pyplot as plt
from scapy.all import *
import struct

def draw_plot(movements):
    # Initialize coordinates
    x_coords = [0]
    y_coords = [0]
    current_x = 0
    current_y = 0

    # Accumulate movements
    for movement in movements:
        current_x += movement[0]
        current_y += movement[1]
        x_coords.append(current_x)
        y_coords.append(current_y)

    # Plot
    plt.plot(x_coords, y_coords, marker='o', linestyle='-')
    plt.show()

if __name__ == "__main__":
    packets = rdpcap("filtered.pcapng") # Ensure this matches your exported filename
    movements = []
    
    for p in packets:
        # Extract X and Y bytes (adjust offsets if necessary for other challenges)
        xb = p.load[66:68]
        yb = p.load[68:70]
        
        x = struct.unpack('<h', xb)[0]
        y = struct.unpack('<h', yb)[0]
        movements.append((x,y))

    draw_plot(movements)

```

now we get a graph but it looks like gibberish so we flip it to see the flag handwritten
<img width="2576" height="1478" alt="Screenshot 2025-12-31 014952" src="https://github.com/user-attachments/assets/88f30d0d-9036-46c8-b6f4-6bea2c8f8bec" />

## Flag:

```
N0PS{m0Us3_dR4w1Ng}
```

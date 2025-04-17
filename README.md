# **Audio Spectrum Visualizer for LED Matrix (Arduino)**

## **Overview**
This project transforms an Arduino into an audio spectrum analyzer that visualizes sound frequencies on an 8×8 LED matrix. It supports three visualization modes and processes audio input via an AUX cable.

### **Features**
- **Three visualization modes**:  
  - **Wave Mode**: Low-frequency (bass) amplitude as rising horizontal bars  
  - **Frequency Zones**: Vertical sections for bass, mid, and high frequencies  
  - **Color Music**: Three columns representing bass, mid, and high ranges  
- **Real-time FFT processing** (Fast Fourier Transform)  
- **Debounce-protected mode switching** via tactile button  
- **Adjustable brightness** for the LED matrix  

---

## **Hardware Requirements**
| Component          | Specification              | Connection       |
|--------------------|----------------------------|------------------|
| Arduino Board      | Uno, Nano, or similar      | —                |
| LED Matrix         | 8×8 (MAX7219-controlled)   | DIN→12, CLK→11, CS→10 |
| AUX Audio Source   | 3.5mm jack                 | A0 (analog input)|
| Button             | Tactile switch             | D2 (digital input)|

---

## **Software Setup**
### **Required Libraries**
1. **LedControl** (for MAX7219 LED control)  
   ```arduino
   #include <LedControl.h>
   ```
2. **FixFFT** (for optimized FFT calculations)  
   ```arduino
   #include <FixFFT.h>
   ```

### **Installation**
1. Clone/download the repository.  
2. Open the `.ino` file in Arduino IDE.  
3. Install dependencies via **Tools > Manage Libraries**.  
4. Upload to your Arduino.  

---

## **Usage**
1. **Connect the hardware** as specified above.  
2. **Power the Arduino** (USB or external 5V).  
3. **Send audio** via the AUX cable (e.g., from a phone/PC).  
4. **Press the button** (D2) to cycle through modes:  
   - **Mode 0**: Wave visualization (bass amplitude)  
   - **Mode 1**: Frequency zones (3 vertical sections)  
   - **Mode 2**: Color music (3 dedicated columns)  

---

## **Customization**
### **Adjustable Parameters**
| Variable           | Purpose                     | Recommended Range |
|--------------------|-----------------------------|-------------------|
| `SAMPLES`          | FFT resolution (power of 2) | 64 (default)      |
| `brightness`       | LED intensity               | 0 (dim) to 15 (max) |
| `delay(50)`        | Refresh rate (ms)           | 20–100 ms         |

### **Troubleshooting**
- **No audio response**: Check AUX connection and A0 pin.  
- **LEDs flicker**: Ensure stable power supply.  
- **FFT inaccuracies**: Adjust `SAMPLES` or sampling delay.  

---

## **License**
Open-source (MIT). Modify and distribute freely.  

**Credits**:  
- Libraries: LedControl (Eberhard Fahle), FixFFT (Peter Knight)  
- Code: Generated with ChatGPT (customized for Arduino)  

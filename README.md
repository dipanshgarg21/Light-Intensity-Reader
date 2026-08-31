# Grove Light Sensor with I2C

A simple light-intensity monitoring project using a **Grove Light Sensor** connected over **I2C**. The program continuously reads the sensor's light level, converts the raw sensor output into an approximate light intensity value, and classifies the environment into different brightness levels.

## Features

* Reads light intensity from a Grove/BH1750 light sensor over I2C.
* Converts the sensor's raw two-byte reading into a readable value.
* Displays the current light intensity in the terminal.
* Classifies the environment as:

  * Too bright
  * Bright
  * Medium
  * Dark
  * Too dark
* Updates the reading every second.
* Stops safely with `Ctrl+C`.

## Hardware

* Raspberry Pi or other Linux-based board with I2C support
* Grove Light Sensor based on the **BH1750**
* Grove-compatible I2C connection

## Requirements

The project uses Python and the `smbus` library.

```bash
pip install smbus
```

I2C must also be enabled on the device before running the program.

The program communicates with the sensor using I2C bus `1` and the default BH1750 address:

```python
address = 0x23
```

## How It Works

The program creates an I2C bus connection:

```python
bus = smbus.SMBus(1)
```

The `lightRead()` function requests the sensor's two-byte measurement:

```python
newAddress = bus.read_i2c_block_data(address, address)
```

The returned bytes are then converted into a light-intensity value by `lightConversion()`:

```python
conversion = ((newAddress[1] + (256 * newAddress[0])) / 1.2)
```

The resulting value is then compared against predefined thresholds:

| Light Intensity | Status     |
| --------------: | ---------- |
|       `>= 4000` | Too bright |
|   `500 – <4000` | Bright     |
|    `100 – <500` | Medium     |
|    `>50 – <100` | Dark       |
|           `<50` | Too dark   |

The sensor is read once every second.

## Running the Program

Clone the repository:

```bash
git clone <repository-url>
cd <repository-directory>
```

Run the Python program:

```bash
python3 main.py
```

Example output:

```text
Reading: 623.33
Status:
Status: Bright

Reading: 172.50
Status:
Status: Medium
```

Press `Ctrl+C` to stop the program.

## Project Structure

```text
.
├── main.py
└── README.md
```

## Notes

This project assumes that the connected Grove light sensor uses the **BH1750** I2C interface and the default `0x23` address.

The light-intensity thresholds are application-specific and can be adjusted in the main loop depending on the environment or desired sensitivity.

## License

This project is available for educational and personal use.

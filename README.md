# Conmark 4-20mA + DIO API

All commands sent to the device will be USB micro interface on board.  This USB interface will mount to the OS as a COM or Serial port.  In windows check Device Manager for the COM identifier under COM and LPT ports.

This driver may need to be installed for the USB device to properly mount to the OS as a COM/Serial Port:
https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers

Serial port baud rate for the control software should be set to 115200

Commands are in the format of JSON objects converted to strings and passed to the Serial/COM interface to the device.  The JSON object passed to the Serial/COM interface contains the operations the device should execute such as setting the 4 digital outputs high or low, setting the 4-20mA DAC output to a certain value, or reading the state of the 4 digital inputs.  The following JSON Key/Value pairs are used to execute these tasks.

## Controlling Outputs

#### Digital output 1

Set Output 1 High

``
"out_1":1
``

Set Output 1 Low

``
"out_1":0
``

#### Digital output 2

Set Output 2 High

``
"out_2":1
``

Set Output 2 Low

``
"out_2":0
``

#### Digital output 3

Set Output 3 High

``
"out_3":1
``

Set Output 3 Low

``
"out_3":0
``

#### Digital output 4

Set Output 4 High

``
"out_4":1
``

Set Output 4 Low

``
"out_4":0
``

#### Analog output 1

Set Analog Output 1 to 4 mA

``
"analog_1":4.00
``

Set Analog Output 1 to 20 mA

``
"analog_1":20.00
``

#### Analog output 2

Set Analog Output 2 to 4 mA

``
"analog_2":4.00
``

Set Analog Output 2 to 20 mA

``
"analog_2":20.00
``

## Reading Inputs and status of outputs
To read the status of the 4 on board digital inputs simply pass the following key value pair to the serial port:
``
"get_state":true
``
The device will respond with the status of all outputs as well as the status of the 4 digital inputs.  This is an example response:
``
{"digital_output_one":0,"digital_output_two":0,"digital_output_three":0,"digital_output_four":1,"digital_input_one":0,"digital_input_two":0,"digital_input_three":1,"digital_input_four":0,"analog_output_one":4.00,"analog_output_two":20.00}
``
In the response above we can see the status is:
Digital output 1 is low
Digital output 2 is low
Digital output 3 is low
Digital output 4 is high
Digital input 1 is open
Digital input 2 is open
Digital input 3 is closed
Digital input 4 is open
Analog Output 1 is set to 4.00 mA
Analog Output 2 is set to 20.00 mA

## Additional Notes
Using JSON means it's possible to pass multiple commands to the device in one operation while also gettign the output state and input state.  For example we can set digital out 1 high, set Analog output 2 to 12.00 mA and request the new state of the device from one command
``
{"out_1":1,"analog_2":12.00,"get_state":true}
``

Keep in mind in most languages you will need to create the JSON object, then convert it to a string before passing it to the Serial/COM port to be written to the device.  Here are examples in a few popular languages

Javascript
```javascript
const jsonObject = {
  out_1: 1,
  analog_2: 12.00,
  get_state: true
};

const jsonString = JSON.stringify(jsonObject);
```
Python
```python
import json

python_dict = {
    "out_1": 1,
    "analog_2": 12.00,
    "get_state": True
}

json_string = json.dumps(python_dict)
```
C#
```C#
using Newtonsoft.Json;

var jsonObject = new
{
    out_1 = 1,
    analog_2 = 12.00,
    get_state = true
};

string jsonString = JsonConvert.SerializeObject(jsonObject);
```

# Inovo Iva
## Iva
Iva is a text-based communication protocal for interacting with inovo robot arm.

It is a sequence that running on inovo runtime that allow interaction with other program with little intergration.

## How to
To use iva, download the latest version of iva [`iva X.X.X.isq`
](https://github.com/dizzyi/inovo-iva/releases/), and import the project to inovo robot blockly runtime.

### api
checkout one of these library for interacting with iva :
- [inovopy](https://github.com/dizzyi/inovopy) : is the python library
- [inovo-rs](https://github.com/dizzyi/inovo-rs) : is the pure rust api library.

if you wish to, you can check the specification and create your own client for interacting with iva.

## IVA specification
IVA is a json-based communication protocal. 
- server-client model. 
- All the message from the client(custome program) to the server(the robot blockly runtime) are a non-nested json dictionary
-  all value are either `string` or `float`.

### Life-cycle
1.  Start the `iva` function in the project sequence, manually or througth websockets
2. The robot will tried to connect to a TCP socket
    - please ensure that the ip and port are correct in the blockly program
3. The main loop begin:
    - The robot will wait for instruction
    - The robot will execute the instruction upon recive
    - The robot will response will a string,
        - usually `OK`
        - in cases where program inquire data from blockly, the formatted string will be sent.
4. If the communication terminated, the robot will pop all its context and terminate the sequence.



### Examples
here is a list of possible instruction to the robot
```json
[
    {
        "op_code": "execute",
        "action": "synchronize",
        "enter_context": 0.0
    },
    {
        "op_code": "execute",
        "action": "motion",
        "motion_mode": "linear",
        "target": "transform",
        "x": 0.0,
        "y": 0.0,
        "z": 0.0,
        "rx": 0.0,
        "ry": 0.0,
        "rz": 0.0,
        "enter_context": 0.0
    },
    {
        "op_code": "execute",
        "action": "set_parameter",
        "speed": 1.0,
        "accel": 0.5,
        "blend_linear": 0.01,
        "blend_angular": 0.5235987755982988,
        "tcp_speed_linear": 0.0,
        "tcp_speed_angular": 0.0,
        "enter_context": 0.0
    },
    {
        "op_code": "enqueue",
        "action": "sleep",
        "second": 1.0
    },
    {
        "op_code": "dequeue",
        "enter_context": 0.0
    },
    {
        "op_code": "pop"
    },
    {
        "op_code": "io",
        "target": "beckhoff",
        "port": 0,
        "action": "get"
    },
    {
        "op_code": "io",
        "target": "wrist",
        "port": 1,
        "action": "set",
        "state": 1.0
    },
    {
        "op_code": "gripper",
        "action": "activate"
    },
    {
        "op_code": "gripper",
        "action": "get"
    },
    {
        "op_code": "gripper",
        "action": "set",
        "label": "open"
    },
    {
        "op_code": "custom",
        "flaot_arg": 69.42,
        "string_arg": "my_string"
    }
]
```
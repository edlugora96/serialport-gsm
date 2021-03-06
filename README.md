# Node SerialPort-GSM
[![NPM](https://nodei.co/npm/serialport-gsm.png?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/serialport-gsm/)

## Intro
SerialPort-GSM is a simplified plugin for communicating with gsm modems. (Primarily for sms) (Focused in `PDU` mode)

***
## Table of Contents

* [Installation](#installation-instructions)
* [Usage](#usage)
* [License](#license)

***
## Installation Instructions

```terminal
npm install serialport-gsm
```

## Usage

#### List Available Ports
```js
let serialportgsm = require('serialport-gsm')

serialportgsm.list((err, result) => {
    console.log(result)
})
```

#### Opening a Port
Call other functions after the port has been opened.
`open(path, options, callback)`
When opening a serial port, specify (in this order)
1. Path to Serial Port - required.
2. Options - optional `(see sample options on code)`.
    * `autoDeleteOnReceive` - Set to `true` to delete from sim after receiving | Default is `false`
    * `enableConcatenation` - Set to `true` to receive concatenated messages as one | Default is `false`
```js
let serialportgsm = require('serialport-gsm')
let modem = serialportgsm.Modem()
let options = {
    baudRate: 115200,
    dataBits: 8,
    parity: 'none',
    stopBits: 1,
    flowControl: false,
    xon: false,
    rtscts: false,
    xoff: false,
    xany: false,
    buffersize: 0
}

modem.open('COM', options, callback[Optional])
```

#### Initialize Modem
This function starts the modem. (If your port fails to work or does not respond to commands, don't forget to call `initializeModem` after opening the port.)
```js
modem.on('open', data => {
    modem.initializeModem(callback[optional])
})
```

#### Set Modem Mode	
`setModemMode(callback, type)`	
* type can be `'PDU'` or `'SMS'`	
```	js
modem.on('open', data => {	
    modem.setModemMode(callback, 'PDU')	
})	
```

#### Send Message
Sends sms.
`sendSMS(recipient, message, alert, callback)`
* alert parameter is boolean
`true` - send as class 0 message(flash message)
`false` - send as a normal sms
```js
modem.sendSMS('0999XXXXX19', 'Hello there Zab!', true, callback)
```

#### Get Sim Inbox
Shows messages of sim inbox
```js
modem.getSimInbox(callback)
```

#### Delete Sim Message
Delete a sim message by message object `(Use Sim Inbox data)`
```js
modem.deleteMessage(messageObj, callback)
```

#### Delete All Sim Messages
```js
modem.deleteAllSimMessages(callback)
```

#### Get Modem Serial
```js
modem.getModemSerial(callback)
```

#### Get Network Signal
```js
modem.getNetworkSignal(callback)
```

#### Get Own Number
```js
modem.getOwnNumber(callback)
```

#### Set Own Number
`setOwnNumber('number', callback, name[optional || default 'OwnNumber'])`
```js
modem.setOwnNumber(number, callback)
```

#### Execute AT Command
```js
modem.executeCommand(callback, priority, timeout)
```

## Other Usage 
### Event Listeners
#### open
```js
modem.on('open', result => { /*do something*/ })
```

#### close
```js
modem.on('close', result => { /*do something*/ })
```

#### error
```js
modem.on('error', result => { /*do something*/ })
```

#### onSendingMessage
```js
modem.on('onSendingMessage', result => { status, request, data })
```

#### onNewMessageIndicator
```js
modem.on('onNewMessageIndicator', result => { sender, timeSent })
```

#### onNewMessage
```js
modem.on('onNewMessage', messageDetails)
```

## SerialPort
Access base serialport. Please refer to [***SerialPort Docs***](https://serialport.io/docs/en/api-serialport) for documentation
```js
modem.port
```

## License
SerialPort-GSM is [MIT licensed](LICENSE) and all it's dependencies are MIT or BSD licensed.
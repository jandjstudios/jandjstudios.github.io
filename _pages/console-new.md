---
permalink: /console/
title: ""
excerpt: "J&J Studios LLC"
last_modified_at: 2019-08-4T12:00:00-00:80
layout: single
classes: wide
toc: false
toc_label: "contents"
toc_sticky: true
author_profile: false
---
<html>
  <head>
    <title>TinyUSB</title>
    <script>
        var serial = {};

        (function() {
        'use strict';

        serial.getPorts = function() {
            return navigator.usb.getDevices().then(devices => {
            return devices.map(device => new serial.Port(device));
            });
        };

        serial.requestPort = function() {
            const filters = [
            { 'vendorId': 0x239A }, // Adafruit boards
            { 'vendorId': 0xcafe }, // TinyUSB example
            { 'vendorId': 0x04D8 }, // J&J Studios example
            ];
            return navigator.usb.requestDevice({ 'filters': filters }).then(
            device => new serial.Port(device)
            );
        }

        serial.Port = function(device) {
            this.device_ = device;
            this.interfaceNumber = 0;
            this.endpointIn = 0;
            this.endpointOut = 0;
        };

        serial.Port.prototype.connect = function() {
            let readLoop = () => {
            this.device_.transferIn(this.endpointIn, 64).then(result => {
                this.onReceive(result.data);
                readLoop();
            }, error => {
                this.onReceiveError(error);
            });
            };

            return this.device_.open()
                .then(() => {
                if (this.device_.configuration === null) {
                    return this.device_.selectConfiguration(1);
                }
                })
                .then(() => {
                var interfaces = this.device_.configuration.interfaces;
                interfaces.forEach(element => {
                    element.alternates.forEach(elementalt => {
                    if (elementalt.interfaceClass==0xFF) {
                        this.interfaceNumber = element.interfaceNumber;
                        elementalt.endpoints.forEach(elementendpoint => {
                        if (elementendpoint.direction == "out") {
                            this.endpointOut = elementendpoint.endpointNumber;
                        }
                        if (elementendpoint.direction=="in") {
                            this.endpointIn =elementendpoint.endpointNumber;
                        }
                        })
                    }
                    })
                })
                })
                .then(() => this.device_.claimInterface(this.interfaceNumber))
                .then(() => this.device_.selectAlternateInterface(this.interfaceNumber, 0))
                .then(() => this.device_.controlTransferOut({
                    'requestType': 'class',
                    'recipient': 'interface',
                    'request': 0x22,
                    'value': 0x01,
                    'index': this.interfaceNumber}))
                .then(() => {
                readLoop();
                });
        };

        serial.Port.prototype.disconnect = function() {
            return this.device_.controlTransferOut({
                    'requestType': 'class',
                    'recipient': 'interface',
                    'request': 0x22,
                    'value': 0x00,
                    'index': this.interfaceNumber})
                .then(() => this.device_.close());
        };

        serial.Port.prototype.send = function(data) {
            return this.device_.transferOut(this.endpointOut, data);
        };
        })();
    
    </script>
    <script>
        (function() {
        'use strict';

        document.addEventListener('DOMContentLoaded', event => {
            let connectButton = document.querySelector("#connect");
            let statusDisplay = document.querySelector('#status');
            let port;

            function addLine(linesId, text) {
            var senderLine = document.createElement("div");
            senderLine.className = 'line';
            var textnode = document.createTextNode(text);
            senderLine.appendChild(textnode);
            document.getElementById(linesId).appendChild(senderLine);      
            return senderLine;
            }

            let currentReceiverLine;

            function appendLine(linesId, text) {
            if (currentReceiverLine) {
                currentReceiverLine.innerHTML =  currentReceiverLine.innerHTML + text;
            } else {
                currentReceiverLine = addLine(linesId, text);
            }
            }

            function connect() {
            port.connect().then(() => {
                statusDisplay.textContent = '';
                connectButton.textContent = 'Disconnect';

                port.onReceive = data => {
                let textDecoder = new TextDecoder();
                console.log(textDecoder.decode(data));
                if (data.getInt8() === 13) {
                    currentReceiverLine = null;
                } else {                    
                    appendLine('receiver_lines', textDecoder.decode(data));
                }
                };
                port.onReceiveError = error => {
                console.error(error);
                };
            }, error => {
                statusDisplay.textContent = error;
            });
            }

            connectButton.addEventListener('click', function() {
            if (port) {
                port.disconnect();
                connectButton.textContent = 'Connect';
                statusDisplay.textContent = '';
                port = null;
            } else {
                serial.requestPort().then(selectedPort => {
                port = selectedPort;
                connect();
                }).catch(error => {
                statusDisplay.textContent = error;
                });
            }
            });

            serial.getPorts().then(ports => {
            if (ports.length === 0) {
                statusDisplay.textContent = 'No device found.';
            } else {
                statusDisplay.textContent = 'Connecting...';
                port = ports[0];
                connect();
            }
            });


            let commandLine = document.getElementById("command_line");

            commandLine.addEventListener("keypress", function(event) {
            if (event.keyCode === 13) {
                if (commandLine.value.length > 0) {
                //addLine('sender_lines', commandLine.value);
                document.getElementById("senderLine").innerHTML = "Last command sent: " + commandLine.value;
                commandLine.value = '';                
                }
            }
            port.send(new TextEncoder('utf-8').encode(String.fromCharCode(event.which || event.keyCode)));
            });
        });
        })();
    
    </script>
  </head>
  <body>
    <div class="main-content">
      <h1>datum WebUSB Serial Console</h1>
      <div class="connect-container">
        <button id="connect" class="button black">Connect</button>
        <span id="status"></span>
      </div>
      <div class="container">
        <div class="sender">
          <div class="lines-header">Sender</div>          
          <div class="lines-body">
            <div id="sender_lines" class="lines"></div>
            <input id="command_line" class="command-line" placeholder="Start typing ...." />
            <div class="lines-header" id="senderLine">Last command sent:</div>
          </div>
        </div>
        <div class="receiver">
          <div class="lines-header">Receiver</div>
          <div class="lines-body">
            <div id="receiver_lines" class="lines"></div>
          </div>
        </div>
      </div>
    </div>
  </body>
</html>


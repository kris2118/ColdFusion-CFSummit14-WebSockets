#AdvancedSocket 

## Aims

- Handle connectivity issues from the client side when using ColdFusion WebSocket solution.
 
 ``` html
 	<script src="/clients/js/advanced.js"></script>
	<cfwebsocket 	name		= "ws"
					onMessage	= "AdvancedSocket.onMessage"
					onOpen		= "AdvancedSocket.onOpen"
					onClose		= "AdvancedSocket.onClose"
					onError		= "AdvancedSocket.onError">

 ```

## Properties / Attributes

- __autoConnect__<br />
_Controls the auto connect feature of the AdvancedSocket. It defaults to true but can be managed by the data-auto-connect attribute in the body tag. It also requires a pingURL to be defined._
- __name__<br />
_The name of your global websocket variable name. Defaults to `ws`_
- __channels__<br/ >
_Comma separated list of channels to subscribe to._
- __clientID__<br />
_The subscriber ID returned from ColdFusion on a succesful connection. This is used when the autoConnect feature is enabled to make sure that we are still an active subscriber._
- __clientInfo__<br />
_This is a key-value object that is passed when creating a connection. By default AdvancedSocket uses a third party request to find out additional geo-based data of the request. This is also used to pass in a username and any additional info you may want to._
- __connected__<br />
_Boolean property that defines whether we are connected_
- __doMessage__<br />
_Defines the function to run on a succesful message. Defaults to doMessage._
- __offline__<br />
_Boolean property that defines whether we are offline_
- __timer__<br />
_Used for the check connection setTimeout_
- __pingURL__<br />
_The URL that will be used to ping if we are still a good connection. SHould return a JSON object with a success value of true or false._
- __onlineCount__<br />
_The timeout value to ping if autoConnect is enabled while we have a good connection. This is default to 30 seconds and can be defined in the body data-online-timer attribute._
- __offlineCount__<br />
_The timeout value to ping if autoConnect is enabled while we have a bad connection. This is default to 5 seconds and can be defined in the body data-offline-timer attribute._
- __reconnectCount__<br />
_The timeout value call a reconnect attempt when a FORCE-RECONNECT value is received from the server. This defaults to 500ms and can be defined in the body data-reconnect-timer attribute._
- __timerCount__<br />
_The timeout value that is used on reconnect calls. It is automally updated to either the online or offline value based on current state._
- __debug__<br />
_Boolean value to display log messages. Defaults to false and can be overwritten by the data-debug body attribute._
- __statusLabel__<br />
_The status document element defined by an id of status-message._

##Functions
- __init__<br />
Sets up all required EventListeners to handle window connection events (connectionerror, goodconnection, requireconnection, offline, online). Sets up the timerCount to the onlineCount and then request the checkConnection() function.
- __checkConnection__<br />
Sets up timer and request first ping() call if autoConnect is enabled.
- __fireEvent__<br />
Creates and dispatches custom events.
- __ping__<br />
Polls request to the server to check if connection is still valid.
- __onMessage__<br />
Handles messages returned. On "welcome, authenticate and/or subscribe" messages it auto fires the AdvancedSocket.connected() function. On "FORCE-RECONNECT" messages fires the AdvancedSocket.forceReconnect() funciton based on the reconnectCount. On a regular messages passes to the Global Function that will handle your message.
- __onOpen__<br />
Fired onced the connection is open. If authentication is required, it calls the authenticate() WS function if not passes to the AdvancedSocket.getIPInfo() function, which is the last step before subscribing.
- __onClose__<br />
Fired on connection close
- __onError__<br />
Fired on connection errors
- __getIPInfo__<br />
Makes a request to ip-api.com to request Geo Based IP data. On response or if jQuery is not available it will call AdvancedSocket.connectWS(), the final step which handles all connections.
- __connectWS__<br />
Loops thru your defined channels and calls the WS subscribe() function.
- __forceReconnect__<br />
Fired when a `FORCE-RECONNECT` message is received. Calls the WS closeConnection() and openConnection() functions which in turn when the socket is open again fires the AdvancedSocket.onOpen() function.
- __disconnected__<br />
Fired when a socket is disconnected. Updates the status label.
- __connected__<br />
Fired when the socket is connecting. Updates the status label.
- __log__<br />
Outputs console logs if debug is set to true. This can be defined with the body data-debug attribute.
# websocket笔记

简单客户端demo

http://www.searchtb.com/2012/08/websocket-introduction.html

```html
<!DOCTYPE HTML>
<html>
	<head>
		<meta http-equiv="content-type" content="text/html" />
		<meta name="author" content="blog.anchen8.net" />
		<title>Untitled 2</title>
		<script>
            var socket;
            function connect() {
                try {
                    socket = new WebSocket('ws://echo.websocket.org');
                } catch(e) {
                    alert('error');
                    return;
                }
                socket.onopen = sOpen;
                socket.onerror = sError;
                socket.onmessage = sMessage;
                socket.onclose = sClose
            }

            function sOpen() {
                alert('connect success!')
            }

            function sError() {
                alert('connect error')
            }

            function sMessage(msg) {
                alert('server says:' + msg.data)
            }

            function sClose() {
                alert('connect close')
            }

            function send() {
                socket.send('hello ,i am siren!')
            }

            function close() {
                socket.close();
            }
		</script>
	</head>

	<body>
		<input id="msg" type="text">
		<button id="connect" onclick="connect();">
			Connect
		</button>
		<button id="send" onclick="send();">
			Send
		</button>
		<button id="close" onclick="close();">
			Close
		</button>
	</body>
</html>

```
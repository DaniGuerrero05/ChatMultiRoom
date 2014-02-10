var fs = require('fs');
function handler(req, res) {
	"use strict";
	fs.readFile(__dirname + '/chatClient.html', function (err, data) {
		if (err) {
			res.writeHead(500);
			return res.end("Error loading el archivo chatClient.html");
		}
	res.writeHead(200, {'content-type': 'text/html; charset=utf8'});
	res.end(data);
	});
}
var app = require('http').createServer(handler), 
   	io = require('socket.io').listen(app);

app.listen(8000, function () {
	"use strict";
	console.log("Servidor http escuchando en el puerto 8000");
});

io.sockets.on('connection', function (socket) {
	"use strict";
	
	socket.emit('login', function (){});
	socket.emit('chat', {'from': 'servidor', 'msg': "Bienvenido al chat"});
	socket.on('nick', function(nick) {
		socket.room = '1';
		socket.join('1');
		socket.set('nombre', nick, function() { 
			socket.emit('chat', {'from': 'servidor', 'msg': "se encuentra en la sala 1"});
		})
		socket.broadcast.to(socket.room).emit('chat1', {'from': 'servidor','nick':nick,'msg':" está conectad@ en esta sala"});	
	});
	socket.on ('elegirSala', function (nuevaSala) {
		socket.get('nombre', function(err, nick) {
			socket.leave(socket.room);
			socket.broadcast.to(socket.room).emit('chat1', {'from': 'servidor','nick':nick,'msg':" ha dejado la sala"});
			socket.room = nuevaSala;
			socket.join(nuevaSala);		
			socket.emit('chat2', {'from': 'servidor', 'msg': " te has unido a la sala " , 'sala':nuevaSala});
			socket.broadcast.to(socket.room).emit('chat1', {'from': 'servidor','nick':nick, 'msg': " se ha unido a esta sala"});
		})
	});
	socket.on('message', function(data) {
		socket.get('nombre', function(err, nick) {
			console.log(nick + " dice => " + data);
         		socket.broadcast.to(socket.room).emit('chat', {'from' : nick, 'msg' : data});
    		})

	});
	socket.on('disconnect', function () {
		socket.get('nombre', function(err, nick) {
			socket.broadcast.emit('chat', {'from': 'servidor',
			'msg' : nick + " se ha ido del chat"});
		})
	});
	socket.on('cliente', function (data) {
		socket.get('nombre', function(err, nick) {
			console.log(nick + " dice => " + data.msg);
			socket.broadcast.to(socket.room).emit('chat', {'from': nick, 'msg': data.msg});
		})
	});
});

console.log("Sigue el programa");



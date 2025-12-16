# WebSockets with Node.js

## Introduction

WebSockets provide full-duplex communication between client and server, enabling real-time applications. This tutorial covers implementing WebSockets using Socket.io in Node.js.

---

## Socket.io Setup

### Installation

    npm install socket.io

### Basic Server

    const express = require('express');
    const http = require('http');
    const { Server } = require('socket.io');
    
    const app = express();
    const server = http.createServer(app);
    const io = new Server(server, {
      cors: {
        origin: '*',
        methods: ['GET', 'POST']
      }
    });
    
    io.on('connection', (socket) => {
      console.log('User connected:', socket.id);
      
      socket.on('disconnect', () => {
        console.log('User disconnected:', socket.id);
      });
    });
    
    server.listen(3000, () => {
      console.log('Server running on port 3000');
    });

---

## Basic Events

### Connection Events

    io.on('connection', (socket) => {
      // Send message to client
      socket.emit('welcome', 'Welcome to the server!');
      
      // Receive message from client
      socket.on('message', (data) => {
        console.log('Message received:', data);
      });
      
      // Send to all clients
      io.emit('broadcast', 'Message to all clients');
      
      // Send to all except sender
      socket.broadcast.emit('user-joined', 'A user joined');
    });

### Client-Server Communication

    // Server
    io.on('connection', (socket) => {
      socket.on('chat-message', (message) => {
        // Broadcast to all clients
        io.emit('chat-message', {
          user: socket.id,
          message: message
        });
      });
    });
    
    // Client (browser)
    const socket = io();
    socket.on('connect', () => {
      console.log('Connected to server');
    });
    
    socket.emit('chat-message', 'Hello!');
    
    socket.on('chat-message', (data) => {
      console.log('Message:', data);
    });

---

## Rooms and Namespaces

### Rooms

    io.on('connection', (socket) => {
      // Join room
      socket.on('join-room', (roomId) => {
        socket.join(roomId);
        socket.to(roomId).emit('user-joined', socket.id);
      });
      
      // Leave room
      socket.on('leave-room', (roomId) => {
        socket.leave(roomId);
      });
      
      // Send to room
      socket.on('room-message', (roomId, message) => {
        io.to(roomId).emit('room-message', message);
      });
    });

### Namespaces

    const adminNamespace = io.of('/admin');
    
    adminNamespace.on('connection', (socket) => {
      console.log('Admin connected');
      socket.emit('admin-message', 'Welcome admin');
    });

---

## Authentication

    const jwt = require('jsonwebtoken');
    
    io.use((socket, next) => {
      const token = socket.handshake.auth.token;
      
      if (!token) {
        return next(new Error('Authentication error'));
      }
      
      try {
        const decoded = jwt.verify(token, process.env.JWT_SECRET);
        socket.userId = decoded.userId;
        next();
      } catch (error) {
        next(new Error('Authentication error'));
      }
    });
    
    io.on('connection', (socket) => {
      console.log('Authenticated user:', socket.userId);
    });

---

## Complete Chat Application

    const express = require('express');
    const http = require('http');
    const { Server } = require('socket.io');
    
    const app = express();
    const server = http.createServer(app);
    const io = new Server(server);
    
    const users = new Map();
    
    io.on('connection', (socket) => {
      console.log('User connected:', socket.id);
      
      // User joins
      socket.on('user-join', (username) => {
        users.set(socket.id, username);
        io.emit('user-list', Array.from(users.values()));
        socket.broadcast.emit('user-joined', username);
      });
      
      // Send message
      socket.on('send-message', (message) => {
        const username = users.get(socket.id);
        io.emit('receive-message', {
          username,
          message,
          timestamp: new Date()
        });
      });
      
      // Typing indicator
      socket.on('typing', () => {
        socket.broadcast.emit('user-typing', users.get(socket.id));
      });
      
      socket.on('stop-typing', () => {
        socket.broadcast.emit('user-stopped-typing', users.get(socket.id));
      });
      
      // Disconnect
      socket.on('disconnect', () => {
        const username = users.get(socket.id);
        users.delete(socket.id);
        io.emit('user-left', username);
        io.emit('user-list', Array.from(users.values()));
      });
    });
    
    server.listen(3000, () => {
      console.log('Chat server running on port 3000');
    });

---

## Best Practices

1. **Use rooms** for organizing connections
2. **Implement authentication** for secure connections
3. **Handle reconnection** gracefully
4. **Limit message rate** to prevent abuse
5. **Use namespaces** for different features
6. **Clean up** on disconnect
7. **Handle errors** properly
8. **Use Redis** for scaling across servers

---

## Conclusion

WebSockets enable real-time communication in applications. Socket.io makes it easy to implement WebSockets with features like rooms, namespaces, and automatic reconnection.


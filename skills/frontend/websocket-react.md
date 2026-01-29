---
name: websocket-react
description: React WebSocket connection and state management patterns
category: frontend
tags: [websocket, react, real-time, hooks, stomp]
---

# React WebSocket

## When to Use

- Real-time data updates (chat, notifications, live feeds)
- Bidirectional communication with server
- STOMP messaging integration with Spring Boot backend

## Rules

- Use custom hook to encapsulate WebSocket logic
- Always handle connection lifecycle (connect, disconnect, reconnect)
- Clean up subscriptions on unmount
- Implement exponential backoff for reconnection
- Buffer messages during reconnection
- Use refs for WebSocket instance to avoid re-renders
- Separate connection state from message state
- Type all messages with TypeScript

## Pattern

### Custom Hook

❌ Bad - No cleanup, no reconnection, causes re-renders:

```tsx
function useWebSocket(url: string) {
  const [socket, setSocket] = useState<WebSocket | null>(null);

  useEffect(() => {
    const ws = new WebSocket(url);
    setSocket(ws); // Causes re-render on every connection
    // Missing cleanup
  }, [url]);

  return socket;
}
```

✅ Good - Proper lifecycle management:

```tsx
import { useRef, useEffect, useState, useCallback } from 'react';

interface WebSocketOptions {
  onMessage?: (data: unknown) => void;
  onConnect?: () => void;
  onDisconnect?: () => void;
  reconnectAttempts?: number;
  reconnectInterval?: number;
}

type ConnectionStatus = 'connecting' | 'connected' | 'disconnected' | 'error';

export function useWebSocket(url: string, options: WebSocketOptions = {}) {
  const {
    onMessage,
    onConnect,
    onDisconnect,
    reconnectAttempts = 5,
    reconnectInterval = 3000,
  } = options;

  const wsRef = useRef<WebSocket | null>(null);
  const reconnectCountRef = useRef(0);
  const reconnectTimeoutRef = useRef<NodeJS.Timeout>();

  const [status, setStatus] = useState<ConnectionStatus>('disconnected');

  const connect = useCallback(() => {
    if (wsRef.current?.readyState === WebSocket.OPEN) return;

    setStatus('connecting');
    const ws = new WebSocket(url);

    ws.onopen = () => {
      setStatus('connected');
      reconnectCountRef.current = 0;
      onConnect?.();
    };

    ws.onmessage = (event) => {
      const data = JSON.parse(event.data);
      onMessage?.(data);
    };

    ws.onclose = () => {
      setStatus('disconnected');
      onDisconnect?.();

      if (reconnectCountRef.current < reconnectAttempts) {
        const delay = reconnectInterval * Math.pow(2, reconnectCountRef.current);
        reconnectTimeoutRef.current = setTimeout(() => {
          reconnectCountRef.current++;
          connect();
        }, delay);
      }
    };

    ws.onerror = () => setStatus('error');
    wsRef.current = ws;
  }, [url, onMessage, onConnect, onDisconnect, reconnectAttempts, reconnectInterval]);

  const disconnect = useCallback(() => {
    clearTimeout(reconnectTimeoutRef.current);
    reconnectCountRef.current = reconnectAttempts; // Prevent reconnect
    wsRef.current?.close();
  }, [reconnectAttempts]);

  const send = useCallback(<T,>(data: T) => {
    if (wsRef.current?.readyState === WebSocket.OPEN) {
      wsRef.current.send(JSON.stringify(data));
    }
  }, []);

  useEffect(() => {
    connect();
    return () => disconnect();
  }, [connect, disconnect]);

  return { status, send, disconnect, reconnect: connect };
}
```

### STOMP Client Hook (for Spring Boot)

```tsx
import { Client, IMessage } from '@stomp/stompjs';
import SockJS from 'sockjs-client';

interface StompOptions {
  subscriptions: Array<{
    destination: string;
    callback: (message: IMessage) => void;
  }>;
  onConnect?: () => void;
}

export function useStompClient(url: string, options: StompOptions) {
  const clientRef = useRef<Client | null>(null);
  const [connected, setConnected] = useState(false);

  useEffect(() => {
    const client = new Client({
      webSocketFactory: () => new SockJS(url),
      reconnectDelay: 5000,
      heartbeatIncoming: 10000,
      heartbeatOutgoing: 10000,
      onConnect: () => {
        setConnected(true);
        options.subscriptions.forEach(({ destination, callback }) => {
          client.subscribe(destination, callback);
        });
        options.onConnect?.();
      },
      onDisconnect: () => setConnected(false),
    });

    client.activate();
    clientRef.current = client;

    return () => {
      client.deactivate();
    };
  }, [url]); // Intentionally exclude options to prevent reconnection loops

  const publish = useCallback((destination: string, body: unknown) => {
    clientRef.current?.publish({
      destination,
      body: JSON.stringify(body),
    });
  }, []);

  return { connected, publish };
}
```

### Usage in Component

```tsx
function ChatRoom({ roomId }: { roomId: string }) {
  const [messages, setMessages] = useState<ChatMessage[]>([]);

  const handleMessage = useCallback((msg: IMessage) => {
    const message = JSON.parse(msg.body) as ChatMessage;
    setMessages(prev => [...prev, message]);
  }, []);

  const { connected, publish } = useStompClient('/ws', {
    subscriptions: [
      { destination: `/topic/chat.${roomId}`, callback: handleMessage },
    ],
  });

  const sendMessage = (content: string) => {
    publish('/app/chat.send', { roomId, content });
  };

  return (
    <div>
      <ConnectionIndicator connected={connected} />
      <MessageList messages={messages} />
      <MessageInput onSend={sendMessage} disabled={!connected} />
    </div>
  );
}
```

## Avoid

- Storing WebSocket instance in state (causes re-renders)
- Missing cleanup on unmount (memory leaks)
- No reconnection strategy
- Synchronous message processing blocking UI
- Missing connection status feedback to users
- Hardcoded URLs (use environment variables)

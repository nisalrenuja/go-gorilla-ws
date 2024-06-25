# WebSocket Echo Server in Go

This project is a simple WebSocket echo server implemented in Go using the Gorilla WebSocket library. The server listens for WebSocket connections, receives messages, and echoes them back to the client.

## Prerequisites

- Go 1.13 or later
- Git

## Installation

1. **Clone the repository:**

   ```sh
   git clone https://github.com/yourusername/websocket-echo-server.git
   cd websocket-echo-server
   ```

2. **Download and install dependencies:**

   ```sh
   go get -u github.com/gorilla/websocket
   ```

## Code Explanation

### Dependencies

The server uses the `gorilla/websocket` package for handling WebSocket connections. Install it using `go get`:

```sh
go get -u github.com/gorilla/websocket
```

### Server Implementation

1. **Upgrader:**

   The `Upgrader` is used to upgrade the HTTP connection to a WebSocket connection:

   ```go
   var upgrader = websocket.Upgrader{
       ReadBufferSize:  1024,
       WriteBufferSize: 1024,
       CheckOrigin: func(r *http.Request) bool {
           return true
       },
   }
   ```

2. **WebSocket Handler:**

   The `websocketHandler` function handles WebSocket connections. It reads messages from the client and echoes them back:

   ```go
   func websocketHandler(w http.ResponseWriter, r *http.Request) {
       conn, err := upgrader.Upgrade(w, r, nil)
       if err != nil {
           log.Println(err)
           return
       }
       defer conn.Close()

       for {
           _, message, err := conn.ReadMessage()
           if err != nil {
               log.Println(err)
               break
           }
           log.Printf("Received Message: %s", message)
           err = conn.WriteMessage(websocket.TextMessage, message)
           if err != nil {
               log.Fatalln(err)
               break
           }
       }
   }
   ```

3. **Main Function:**

   The `main` function sets up the HTTP server and routes:

   ```go
   func main() {
       http.HandleFunc("/websocket", websocketHandler)
       log.Fatal(http.ListenAndServe(":8080", nil))
   }
   ```

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Acknowledgments

- [Gorilla WebSocket](https://github.com/gorilla/websocket)

---

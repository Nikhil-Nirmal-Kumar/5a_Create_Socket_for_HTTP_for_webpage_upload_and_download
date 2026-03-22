# 5a_Create_Socket_for_HTTP_for_webpage_upload_and_download
## AIM :
To write a PYTHON program for socket for HTTP for web page upload and download
## Algorithm:

1.Start the program.
<BR>
2.Get the frame size from the user
<BR>
3.To create the frame based on the user request.
<BR>
4.To send frames to server from the client side.
<BR>
5.If your frames reach the server it will send ACK signal to client otherwise it will send NACK signal to client.
<BR>
6.Stop the program
<BR>
## Program:
## server.py
```
import socket

HOST = '127.0.0.1'
PORT = 8080

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind((HOST, PORT))
server.listen(1)

print("Server waiting for connection...")
conn, addr = server.accept()
print("Connected by", addr)

received_data = ""

while True:
    data = conn.recv(1024).decode()
    if not data:
        break

    # Simulate error check (simple check)
    if data == "END":
        break

    print("Received Frame:", data)

    # If frame is valid send ACK
    conn.send("ACK".encode())
    received_data += data

# Save received webpage
with open("received_page.html", "w") as f:
    f.write(received_data)

print("File saved as received_page.html")

conn.close()
server.close()
```
## client.py
```
import socket

HOST = '127.0.0.1'
PORT = 8080

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect((HOST, PORT))

filename = input("Enter file name to upload: ")
frame_size = int(input("Enter frame size: "))

with open(filename, "r") as f:
    data = f.read()

# Split into frames
frames = [data[i:i+frame_size] for i in range(0, len(data), frame_size)]

for frame in frames:
    while True:
        client.send(frame.encode())
        response = client.recv(1024).decode()

        if response == "ACK":
            print("Frame sent successfully")
            break
        else:
            print("Resending frame...")

# Send end signal
client.send("END".encode())

client.close()
```
## OUTPUT:
<img width="1552" height="260" alt="image" src="https://github.com/user-attachments/assets/dcaa6bb3-48cb-4002-a71d-7f2463adcbea" />
<img width="1527" height="200" alt="image" src="https://github.com/user-attachments/assets/fc775a8a-ab5f-47c6-bc95-5669c8991c6a" />

## Result:
Thus the socket for HTTP for web page upload and download created and Executed

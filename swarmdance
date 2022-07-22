# This example script demonstrates how to use Python to fly Tello in a box mission with a loop
# This script is part of our course on Tello drone programming
# https://learn.droneblocks.io/p/tello-drone-programming-with-python/

# Import the necessary modules
import socket
import threading
import time

# IP and port of Tello
tello1 = ('192.168.0.120', 8889)
tello2 = ('192.168.0.228', 8889)
tello3 = ('192.168.0.138', 8889)

# IP and port of local computer
local_address = ('', 9000)

# Create a UDP connection that we'll send the command to
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

# Bind to the local address and port
sock.bind(local_address)


# Send the message to Tello and allow for a delay in seconds
def send(message, drone):
    # Try to send the message otherwise print the exception
    try:
        sock.sendto(message.encode(), drone)
        print(drone)
        print("Sending message: " + message)
    except Exception as e:
        print("Error sending: " + str(e))


# Receive the message from Tello
def receive():
    # Continuously loop and listen for incoming messages
    while True:
        # Try to receive the message otherwise print the exception
        try:
            response, ip_address = sock.recvfrom(128)
            print("Received message: " + response.decode(encoding='utf-8'))
        except Exception as e:
            # If there's an error close the socket and break out of the loop
            sock.close()
            print("Error receiving: " + str(e))
            break


"""Create and start a listening thread that runs in the background"""
"""This utilizes our receive functions and will continuously monitor for incoming messages"""
receiveThread = threading.Thread(target=receive)
receiveThread.daemon = True
receiveThread.start()


def ellipse(x1, y1, z1, x2, y2, z2, speed, tello):
    send("curve " + str(x1) + " " + str(y1) + " " + str(z1) + " " + str(x2) + " " + str(y2) + " " + str(z2) + " " + str(
        speed), tello)
    send("curve " + str(-1 * x1) + " " + str(-1 * y1) + " " + str(-1 * z1) + " " + str(-1 * x2) + " " + str(
        -1 * y2) + " " + str(-1 * z2) + " " + str(speed), tello)


def drone_forward(x, tello_number):
    send("forward " + str(x), tello_number)
    time.sleep(5)


def drone_left(x, tello_number):
    send("left " + str(x), tello_number)
    time.sleep(5)


def drone_right(x, tello_number):
    send("right " + str(x), tello_number)
    time.sleep(5)


def drone_back(x, tello_number):
    send("back " + str(x), tello_number)
    time.sleep(5)


def tello_1():
    send("speed 60", tello1)
    send("forward 250", tello1)
    send("left 200", tello1)
    send("forward 250", tello1)
    send("cw 90", tello1)
    time.sleep(5)


def tello_2():
    send("speed 60", tello2)
    drone_forward(300, "tello2")
    drone_left(200, "tello2")
    drone_forward(250, "tello2")
    send("cw 90", tello2)
    time.sleep(5)


def tello_3():
    send("speed 60", tello3)
    send("forward 250", tello3)
    send("right 200", tello3)
    send("forward 250", tello3)
    send("ccw 90", tello3)
    time.sleep(5)


# Your code starts here
# Put Tello into command mode
send("command", tello1)
send("command", tello2)
send("command", tello3)
time.sleep(1)

# Send the takeoff command
send("takeoff", tello1)
send("takeoff", tello2)
send("takeoff", tello3)
time.sleep(1)

send("up 280", tello1)
send("up 280", tello2)
send("up 280", tello3)
time.sleep(3)

# send("up 206", tello2) #forming the triangle

# ellipse(0, y1, z1, 0, 238, 137, 30,tello1)
# ellipse(0, -137, 138, 0, 0, -274, 30,tello2)
# ellipse(0, 50, 188, 0, 237, 137, 30,tello3)

# MAIN DRONE COMMANDS WITH FUNCTIONS

tello_1()
time.sleep(1)

tello_2()
time.sleep(1)

tello_3()
time.sleep(2)


# Call functions for the creation of the triangles HERE

# Flip for show
send("flip f", tello1)
send("flip f", tello2)
send("flip f", tello3)
time.sleep(5)

# Land
send("land", tello1)
send("land", tello2)
send("land", tello3)
time.sleep(3)

# Print message
print("Mission completed successfully!")

# Closes the sock
sock.close()

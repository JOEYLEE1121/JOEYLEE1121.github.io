---
layout: single
title: "Checking Network Performance"
categories: "school-project"
tag: [python]
toc: true
toc_sticky: true
---

# Basic python socket programming for checking speed in TCP and UDP transfer

score: 5.0 / 5.0

## Task

![taskp1]({{site.url}}/images/2023-02-16-SocketProgramming/taskp1.png)

![taskp2]({{site.url}}/images/2023-02-16-SocketProgramming/taskp2.png)

![taskp3]({{site.url}}/images/2023-02-16-SocketProgramming/taskp3.png)

![taskp4]({{site.url}}/images/2023-02-16-SocketProgramming/taskp4.png)



## Code

```python
"""
COMP3234 Programming Assignment 1
Name: Chang Jun Lee
UID: 3035729629
Development Platform: Mac OS X
Python version: 3.9.12
Remark: For test 1, changing the value of T1_DATA_SIZE in line 16 allows to check time for different data size. One may try 2*10**9 for ex.
        When running the program on HKU server, please wait for a while when trying to re-run it to avoid OS error.
"""

import socket
import time
import sys

PORT = 40440 # define port number
CHUNK_SIZE = 10000 # define chunk size for dividing test 1 data
T1_DATA_SIZE = 2 * 10 ** 8 # test 1 data size defined in the requirement
T2_DATA_SIZE = 10000 # test 2 data size defined in the requirement

def progressBar(remaining_time): # animated progress bar for test 1
    if remaining_time % (CHUNK_SIZE*1000) == 0:
        print("* ",end="",flush=True)

def test_t1_server(c):
        print("Start test1 - large transfer")
        print("From server to client")

        remaining_t1_send = T1_DATA_SIZE
        total_t1_send = 0

        test1_start = time.perf_counter()
        while remaining_t1_send > 0: # message is sent by the amount of CHUNK_SIZE. The loop ends when the iteration times msg size equals T1_DATA_SIZE
            msg = b'1'*CHUNK_SIZE
            try:
                c.sendall(msg)
            except socket.error:
                print("Socket sendall error in t1 server: ", socket.error)
                sys.exit(1)
            remaining_t1_send -= CHUNK_SIZE
            total_t1_send += CHUNK_SIZE
            progressBar(remaining_t1_send)
        message = c.recv(5)
        test1_end = time.perf_counter()
        elapsed_time = test1_end - test1_start
        throughput = (T1_DATA_SIZE) / (elapsed_time * 10 ** 6)

        print()
        print(f"sent total: {total_t1_send} bytes")
        print(f"elapsed time: {round(elapsed_time,3)} s")
        print(f"Throughput(from server to client): {round(throughput, 3)} Mb/s")
        print("From client to server")
        remaining_test1_receive = T1_DATA_SIZE
        total_test1_received = 0
        msg_received = []
        while remaining_test1_receive > 0: # while loop ends when the total size of msgs received meets the test 1 data size requirement
            msg = c.recv(CHUNK_SIZE)
            remaining_test1_receive -= len(msg)
            total_test1_received += len(msg)
            msg_received.append(msg)

        # print("len of total msg received: ", len(msg_received[0]) * len(msg_received)) -> debug to check whether all data is properly received
        print(f"Received total: {total_test1_received} bytes")
        c.send(b"okayy") # send 5 byte confirm message

        print()

def test_t1_client(s):
    print("Start test1 - large transfer")
    print("From server to client")

    remaining_test1_received = T1_DATA_SIZE
    total_received = 0
    msg_received = []
    while remaining_test1_received > 0: # receives msg from server, ends when the total amount received equals test 1 data size requirement
        msg = s.recv(CHUNK_SIZE)
        if msg == b"":
            print("Connection is broken")
            sys.exit(1)
        remaining_test1_received -= len(msg)
        total_received += len(msg)
        msg_received.append(msg)
    # print("len of total msg received: ", len(msg_received[0]) * len(msg_received)) -> debug to check whether all data is properly received
    print(f"received total: {total_received} bytes")
    s.send(b'okayy')

    print("From client to server")
    remaining_test1_send = T1_DATA_SIZE
    total_test1_send = 0
    test1_start = time.perf_counter()
    while remaining_test1_send > 0: # send msg to server, ends when the total amount sent equals test 1 data requirement
        msg = b'1' * CHUNK_SIZE
        try:
            s.sendall(msg)
        except socket.error:
            print("socket sendall error in t1 client: ", socket.error)
            sys.exit(1)
        remaining_test1_send -= CHUNK_SIZE
        total_test1_send += CHUNK_SIZE
        progressBar(remaining_test1_send)
    print()
    confirm_msg = s.recv(5)
    if confirm_msg == b"":
        print("Connection is broken")
        sys.exit(1)
    test1_end = time.perf_counter()

    elapsed_time = test1_end - test1_start
    throughput = (T1_DATA_SIZE) / (elapsed_time * 10 ** 6)

    print(f"Sent total: {total_test1_send} bytes")
    print(f"Elapsed time: {round(elapsed_time,3)} s")
    print(f"Throughput(from client to server): {round(throughput, 3)} Mb/s")
    print()


def test_t2_server(c):
    print("Start test2 - small transfer")
    print("From server to client")
    small_msg = b'2' * T2_DATA_SIZE

    test2_start = time.perf_counter()
    try:
        c.sendall(small_msg)
    except socket.error:
        print("Socket sendall error in t2 server: ", socket.error)
        sys.exit(1)
    confirm_msg = c.recv(5)
    if confirm_msg == b"":
        print("Connection is broken")
        sys.exit(1)
    test2_end = time.perf_counter()
    elapsed_time = test2_end - test2_start
    throughput = (T2_DATA_SIZE) / (elapsed_time * 10 ** 6)
    print(f"Sent total: {T2_DATA_SIZE} bytes")
    print(f"Elapsed time: {round(elapsed_time,5)} s")
    print(f"Throughput(from server to client): {round(throughput, 3)} Mb/s")
    c.recv(T2_DATA_SIZE)
    c.sendall(b"okayy")
    print("From client to server")
    print(f"Received total: {T2_DATA_SIZE} bytes")
    print()

def test_t2_client(s):
    print("Start test2 - small transfer")
    print("From server to client")

    received_msg = s.recv(T2_DATA_SIZE)
    if received_msg == b"":
        print("Connection is broken")
        sys.exit(1)
    s.sendall(b'hahah')
    print(f"Received total: {T2_DATA_SIZE} bytes")
    print("From client to server")
    small_msg = b'1' * T2_DATA_SIZE
    test2_start = time.perf_counter()
    try:
        s.sendall(small_msg)
    except socket.error:
        print("Socket sendall error in t2 client: ", socket.error)
        sys.exit(1)
    confirm_msg = s.recv(5)
    if confirm_msg == b"":
        print("Connection is broken")
        sys.exit(1)
    test2_end = time.perf_counter()
    elapsed_time = test2_end - test2_start
    throughput = (T2_DATA_SIZE) / (elapsed_time * 10 ** 6)
    print(f"Sent total: {T2_DATA_SIZE} bytes")
    print(f"Elapsed time: {round(elapsed_time,5)} s")
    print(f"Throughput(from server to client): {round(throughput, 3)} Mb/s")
    print()

def test_t3_server(c):
    print("Start test3 - UDP pingpong")
    print("From server to client")
    msg, peer_addr = c.recvfrom(5) # connection confirmation check

    total_elapse = 0
    for i in range(5): # iterate the process 5 times
        t3_start = time.perf_counter()
        smsg = b'udpt3'
        try:
            c.sendto(smsg,peer_addr) # send ping to client
        except socket.error:
            print("Socket sendto error: ", socket.error)
            sys.exit(1)
        c.recvfrom(5) # receive pong from client
        t3_stop = time.perf_counter()
        elapse_time3 = t3_stop - t3_start
        total_elapse += elapse_time3
        print(f"Reply from {addr[0]}: time = {elapse_time3} s")
    print(f"Average RTT: {round(total_elapse/5,5)} s")

    print("From client to server")
    for i in range(5):
        print("* ",end="",flush=True) # progress bar for test 3
        msg, peer_addr = c.recvfrom(5) # ping
        try:
            c.sendto(msg,peer_addr) # pong
        except socket.error:
            print("Socket sendto error: ", socket.error)
            sys.exit(1)
    print()

    c.close()

def test_t3_client(s, server_address):
    print("Start test3 - UDP pingpong")
    print("From server to client")

    s.sendto(b'udpt3',(str(sys.argv[1]),PORT)) # send ping from server
    for _ in range(5):
        print("* ",end="",flush=True) # progress har for test 3
        msg, peer_addr = s.recvfrom(5) # connection confirmation check
        try:
            s.sendto(msg,peer_addr) # send pong from server
        except socket.error:
            print("Socket sendto error in t3 client: ", socket.error)
            sys.exit(1)
    print()
    print("From client to server")
    total_elapse = 0
    for i in range(5): # iterate for 5 times
        t3_start = time.perf_counter()
        smsg = b'udpt3'
        try:
            s.sendto(smsg,server_address) # ping
        except socket.error:
            print("Socket sendto error in t3 client: ", socket.error)
            sys.exit(1)
        s.recvfrom(5) # pong
        t3_stop = time.perf_counter()
        elapse_time3 = t3_stop - t3_start
        total_elapse += elapse_time3
        print(f"Reply from {server_address[0]}: time = {elapse_time3} s")
    print(f"Average RTT: {round(total_elapse/5,5)} s")

if (len(sys.argv) == 1): # server
    print("Start as server node")
    HOST = '0.0.0.0'
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    try:
        s.bind((HOST, PORT))
    except socket.error:
        print("Socket bind error: ", socket.error)
        sys.exit(1)

    print(f"Server is ready. Listening address: ('{s.getsockname()[0]}',{PORT})")

    s.listen(5)
    c, addr = s.accept()
    print("A client has connected and it is at: ", addr)
    print()

    test_t1_server(c) # run test 1
    test_t2_server(c) # run test 2
    c2 = socket.socket(socket.AF_INET, socket.SOCK_DGRAM) # change to UDP for test 3
    try:
        c2.bind((HOST, PORT)) # use the same port number
    except socket.error:
        print("Socket bind error in t3: ", socket.error)
        sys.exit(1)
    test_t3_server(c2) # run test 3
    print("End of all benchmarks")

    c.close()

elif (len(sys.argv) == 2): # client
    print("Start as client node")
    try:
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.connect((str(sys.argv[1]), PORT))
    except socket.error:
        print("Socket error: ", socket.error)
        sys.exit(1)

    print("Client has connected to server at: ", (str(sys.argv[1]), PORT))
    server_address = s.getpeername() # get server address
    client_address = s.getsockname() # get client address
    print(f"Client's address: ('{client_address[0]}', {client_address[1]})") # Client's port number is dynamically assigned by the function getsockname
    print()

    test_t1_client(s) # run test 1
    test_t2_client(s) # run test 2
    s2 = socket.socket(socket.AF_INET, socket.SOCK_DGRAM) # change to UDP for test 3

    test_t3_client(s2, server_address) # run test 3
    print("End of all benchmarks")

    s.close()

else:
    print("Please input valid format")
```



## Output

![output]({{site.url}}/images/2023-02-16-SocketProgramming/output.png)

- Note that the test is done when both the server and client is on the same computer.
  - Transmission Rate gets slower when testing with different computers for the server and the client. Try testing out with the code above!

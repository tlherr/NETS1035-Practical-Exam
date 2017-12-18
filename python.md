# Python

## INSTALL PYENV

[link](https://github.com/pyenv/pyenv)

```bash
git clone https://github.com/pyenv/pyenv.git ~/.pyenv;
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile;
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile;
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bash_profile;
exec "$SHELL";
```

Install versions (2x stable and 3x stable as of Dec 2017):
If having issues after an install run rehash which  checks and sets pyenv shims
```bash
pyenv install 2.7.13;
pyenv install 3.6.3;
pyenv rehash;
```

Set Version:

Globally:
```bash
pyenv global 3.6.3
```

Locally:
```bash
pyenv local 3.6.3
```

```bash

```

## Lab1

### 1
Create a python program that takes a string and prints out the   first,middle  and last characters.
```python
while True:
    try:
        name = input("Please enter a string: ")
        if not name:
            raise ValueError('Empty String Given')
    except ValueError as e:
        print("Sorry, I didn't understand that.")
        print(e)
        continue
    else:
        break

length = len(name)
half = int(length/2)

print(name[0],name[half],name[length - 1], sep='')

```

### 2
Copy Ian’s Rot3 (Caesar cipher) program from the internet and make it work
```python
import codecs
import sys, getopt


def main(argv):
    toencrypt = ''
    todecrypt = ''

    try:
        # options is the string of option letters that the script wants to recognize, with options that require an
        # argument followed by a colon
        opts, args = getopt.getopt(argv, "e:d:", ["encrypt=", "decrypt="])
    except getopt.GetoptError:
        print('question2.py -e <toencrypt> -d <todecrypt>')
        sys.exit(2)
    for opt, arg in opts:
        if opt == "-h":
            print('question2.py -e <toencrypt> -d <todecrypt>')
            sys.exit()
        elif opt in ("-e",'--encrypt'):
            # Encrypting
            toencrypt = arg
        elif opt in ("-d", "--decrypt"):
            # Decrypting
            todecrypt = arg

    if(len(toencrypt)>0):
        print("Encrypting Phrase: ", toencrypt)
        print(codecs.encode(toencrypt, 'rot_13'))

    if(len(todecrypt)>0):
        print("Decrypting Phrase: ", todecrypt)
        print(codecs.decode(todecrypt, 'rot_13'))

if __name__ == "__main__":
   main(sys.argv[1:])
```
### 3
Create a jumble program using indexing. Take a string and create substrings. Move the strings to create a jumbled string
```python
import random

input = input("Enter a string to jumble: ")
substrings = list()
inputsize = len(input)
combinedstring = ''

for x in range(0, inputsize-1):
    substrings.append(input[x:random.randint(x,inputsize-1)])

for substring in substrings:
    combinedstring+=substring

print(combinedstring, sep=' ')
```
### 4
Create a base64 encoded message using python and the command line
```python
import base64
import sys, getopt


def main(argv):
    toencrypt = ''
    todecrypt = ''

    try:
        opts, args = getopt.getopt(argv, "e:d:", ["encrypt=", "decrypt="])
    except getopt.GetoptError:
        print('question4.py -e <toencrypt> -d <todecrypt>')
        sys.exit(2)
    for opt, arg in opts:
        if opt == "-h":
            print('question4.py -e <toencrypt> -d <todecrypt>')
            sys.exit()
        elif opt in ("-e",'--encrypt'):
            # Encrypting
            toencrypt = arg
        elif opt in ("-d", "--decrypt"):
            # Decrypting
            todecrypt = arg

    if(len(toencrypt)>0):
        print("Encrypting Phrase: ", toencrypt)
        print(base64.b64encode(toencrypt.encode()))

    if(len(todecrypt)>0):
        print("Decrypting Phrase: ", todecrypt)
        print(base64.b64decode(todecrypt))

if __name__ == "__main__":
   main(sys.argv[1:])
```
### 5
Decrypt this message: Dtz rzxy qnpj ijhwduynsl ymnslx?
```python
encrypted = "Dtz rzxy qnpj ijhwduynsl ymnslx?"

# Attempt +40 to -40 char shift

# @ref: http://eddmann.com/posts/implementing-rot13-and-rot-n-caesar-ciphers-in-python/
def rot_by(n):
    from string import ascii_lowercase as lc, ascii_uppercase as uc
    # Using maketrans to map each ascii character to ascii position of n modifier start/end uppercase and lowercase
    # essentially take the ascii table and map it to a slice of the table modified by our n value. So any character will
    # map to its equivilent n positions ahead or behind in the ascii table (thus implementing rot(n))
    lookup = str.maketrans(lc + uc, lc[n:] + lc[:n] + uc[n:] + uc[:n])
    return lambda s: s.translate(lookup)

for x in range(-40,40):
    print("Using Shift Value: ", x)
    print(rot_by(x)(encrypted))

```

## Lab3

### 1
Create a python program that takes two inputs, a search string pattern and a string to search through:

a. The program should take an input for a string with a minimum length of 20 characters
b. The program should take a search string input with a max of 4 characters
c. The program should use find() to find the number of times a given search string is in the longer string
d. The string should print out the number found and the indexes for each string
e. Example string “good day, how are you today? Are you happy today? Search string : “day” Final output: 3, [6,19,30]
This is an example, I did not confirm the correct indexes, I just guessed.
```python
class InputLengthException(Exception):
    pass

"""
Search a string for a given pattern, return number of matches and their positions
:param to_search: String to be searched
:param pattern: The pattern to search for
:return: returns nothing
"""
def search_string(to_search, pattern):
    if(len(to_search)<20):
        raise InputLengthException("String must be at least 20 characters")
    if(len(pattern)>4):
        raise InputLengthException("Search string is max 4 characters")

    # Use find to find the number of times a given search string is in the longer string
    count = 0
    positions = []
    lbound = 0
    rbound = len(pattern)

    while(rbound<=len(to_search)):
        # Cut a substring
        substr = to_search[lbound:rbound]
        result = substr.find(pattern)
        if(result!=-1):
            count+=1
            positions.append(lbound)
        lbound+=1
        rbound+=1
    print(count, positions)


to_search = input('Enter a string (at least 20 characters): ')
pattern = input('Enter a substring to search for:')
try:
    search_string(to_search, pattern)
    exit(0)
except InputLengthException as e:
    print("Invalid Input. Exiting...")
    exit(1)
```

### 2
Create a python program for guessing a random number. Your program should use the random library to generate a 
random number. Don't show it on the screen though! Have a user input a guess. 
If they are correct it will say you win, if not just show what the random number was. 
Screenshot the code and the program in progress.

```python 
import random

# Seed with current time
random.seed()

# Have no idea what kind of range you are looking for, use 10 to make it at least possible to win
randomNumber = random.randint(1, 10)

guess = input("Guess the random number (between 1 and 10):")

if(guess==randomNumber):
    print("You win! ", guess, " was the random number!")
else:
    print("Incorrect. Random number was: ", randomNumber, " Your Guess: ", guess)
```

### 3

Finish working on the aes client/server. That means make sure the input from the client is encrypted. Show your code!

client.py
```python

import socket
import select
import sys
import logging

from libcrypto import aesdecrypt
from libcrypto import aesencrypt

AES_KEY = '00112233445566778899aabbccddeeff'

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# Connect the socket to the port where the server is listening
server_address = ('127.0.0.1', 8886)
print(f"Connecting to server at ${server_address[0]}:${server_address[1]}")
sock.connect(server_address)
sock.setblocking(0)


def send(message):
    print("Sending: ", message, "Encrypted: ", aesencrypt(AES_KEY, message))
    sock.send(aesencrypt(AES_KEY, message))

def decrypt(message):
    return aesdecrypt(AES_KEY, message)

output_queue = []

try:
    send('Connecting')
    while True:
        readable, writable, exceptional = select.select([sock, sys.stdin], [], [])
        if readable:
            for s in readable:
                if s is sock:
                    data = sock.recv(4096)
                    # Handle the incoming data
                    print(f"Received Message: ${data}")
                    print("Decrypting...")
                    print(decrypt(data))
                if s is sys.stdin:
                    input = sys.stdin.readline()
                    send(input)

except KeyboardInterrupt:
    # User is wanting to exit, clean up
    sock.close()
    exit(0)

```

server.py
```python
import socket
import signal
import select
import logging
import sys
import queue
from threading import _start_new_thread

from libcrypto import aesdecrypt
from libcrypto import aesencrypt

class Server(object):

    def __init__(self, port=8886, socket_backlog=5):

        self.AES_KEY = '00112233445566778899aabbccddeeff'
        self.clients = 0

        # Attempt to create socket
        try:
            self.server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        except socket.error as msg:
            print("Unable to create socket", msg)
            exit(1)
        self.server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
        self.server.setblocking(0)

        # Attempt to bind port
        try:
            self.server.bind(('0.0.0.0', port))
        except socket.error as msg:
            print("Unable to bind to specified port", msg)
            exit(1)

        self.server.listen(socket_backlog)
        logging.info(f"Listening on port ${port}")

        # Sockets from which we expect to read
        self.input_sockets = [ self.server ]
        # Sockets to which we expect to write
        self.output_sockets = []
        # Outgoing message queues
        self.message_queues = {}

        self.broadcast_sockets = []

        # Trap keyboard interrupts
        signal.signal(signal.SIGINT, self.sighandler)


    def encrypt(self, message):
        return aesencrypt(self.AES_KEY, message)

    def decrypt(self, message):
        return aesdecrypt(self.AES_KEY, message)

    def sighandler(self, signum, frame):
        logging.warning("Server Shutting Down")
        # Close Sockets
        for o in self.output_sockets:
            o.close()
        self.server.close()
        exit(0)

    def serve(self):
        # Main loop, calls select to block and wait for network activity
        while self.input_sockets:
            # Wait for at least one of the sockets to be ready for processing
            logging.info(f"Waiting for the next event")
            readable, writable, exceptional = select.select(self.input_sockets, self.output_sockets, self.input_sockets)

            # Select returns three lists: readable, writeable and exceptional. Readable lists have incoming data buffered
            # and available to red. Writeable lists  have free space in buffer and can be written to. Sockets in exceptional
            # have had an error occur

            # Handle inputs
            for s in readable:
                if s is self.server:
                    # A "readable" server socket is ready to accept a connection
                    connection, client_address = s.accept()
                    print(f"New Connection From: ${client_address}")
                    connection.setblocking(0)
                    self.input_sockets.append(connection)

                    self.broadcast_sockets.append(connection)

                    # Give the connection a queue for data we want to send
                    self.message_queues[connection] = queue.Queue()
                else:
                    data = s.recv(1024)
                    # This case represents a client that has sent data. Data is read then placed onto the queue so it
                    if data:
                        # A readable client socket has data
                        print(f"received ${self.decrypt(data)} from ${s.getpeername()}")

                        for client in self.broadcast_sockets:
                            if(client==s):
                                self.message_queues[s].put(self.encrypt("Acknowledged"))
                            else:
                                self.message_queues[client].put(data)
                            self.output_sockets.append(client)

                    # No data means client has disconnected and socket is ready to be closed
                    else:
                        # Interpret empty result as closed connection
                        print(f"Closing ${client_address}, after reading no data")
                        # Stop listening for input on the connection
                        if s in self.output_sockets:
                            self.output_sockets.remove(s)
                        self.input_sockets.remove(s)
                        s.close()

                        # Remove message queue
                        del self.message_queues[s]
            # Handle outputs
            for s in writable:
                try:
                    next_msg = self.message_queues[s].get_nowait()
                    print(f"Message in writable queue: ${self.decrypt(next_msg)}")
                except queue.Empty:
                    # No messages waiting so stop checking for writability.
                    print(f"Output queue for ${s.getpeername()}, is empty")
                    self.output_sockets.remove(s)
                else:
                    print(f"sending ${self.decrypt(next_msg)} to ${s.getpeername()}")
                    # Want to send the message out on all output sockets except the one we recieved message on
                    s.send(next_msg)

            # Handle exceptions
            for s in exceptional:
                logging.info(f"handling exceptional condition for ${s.getpeername()}")
                # Stop listening for input on the connection
                self.input_sockets.remove(s)
                if s in self.output_sockets:
                    self.output_sockets.remove(s)
                s.close()

                # Remove message queue
                del self.message_queues[s]

if __name__ == "__main__":
    logging.basicConfig(level=logging.INFO)
    logging.basicConfig(format='%(asctime)s %(message)s', datefmt='%m/%d/%Y %I:%M:%S %p')

    print("Server Starting")
    Server().serve()
```

libcrypto.py
```python
from Crypto.Cipher import AES
from Crypto.Hash import MD5
from base64 import b64decode
from base64 import b64encode
import os
import hmac
import hashlib
import logging

# AES CBC expects messages to be sent in 16 byte lengths
BLOCK_SIZE = 16
# Define the size of the HMAC signature in bytes
HMAC_SIZE = 20
# Key used to verify authenticity of messages
HMAC_KEY = "fiuwh38yr89hafwrgiwfgsaulh"

logging.basicConfig(level=logging.ERROR)

# How many bytes do we need to reach the BLOCK_SIZE boundry
# Add that number of bytes to the string and return it
pad = lambda s: s + (BLOCK_SIZE - len(s) % BLOCK_SIZE) * \
                chr(BLOCK_SIZE - len(s) % BLOCK_SIZE)

# Remove from the back as many bytes as indicated in the padding
unpad = lambda s: s[:-ord(s[len(s) - 1:])]


def aesencrypt(key, plaintext):
    logging.debug("===== Beginning Encryption =====")

    if (len(plaintext) == 0):
        raise Warning("Attempting to encrypt an empty string")

    # Hash the key
    key = MD5.new(key.encode('utf8')).hexdigest()
    logging.debug(f"Hashing Key: ${key}")
    # Pad the input to reach block size
    plaintext = pad(plaintext)
    logging.debug(f"Padding Plaintext: ${plaintext}")
    # Generate a random IV
    iv = os.urandom(16)
    logging.debug(f"Generating random IV: ${iv}")
    # Generate ciphertext
    cipher = AES.new(key, AES.MODE_CBC, iv)
    logging.debug(f"Generating Cipher Text: ${cipher}")
    # Encrypt the message
    encryptedText = iv + cipher.encrypt(plaintext)
    logging.debug(f"Encrypting Message: ${encryptedText}")
    # Generate a HMAC signature of the encrypted message
    signature = hmac.new(HMAC_KEY.encode('utf-8'), encryptedText, hashlib.sha1).digest()
    logging.debug(f"Generating Signature for encrypted message: ${signature}  Size: ${len(signature)}")
    assert len(signature) == HMAC_SIZE

    return b64encode(signature + encryptedText)


def aesdecrypt(key, enc):
    logging.debug("===== Beginning Decryption =====")

    if(len(enc)==0):
        raise Warning("Attempting to decrypt an empty string")

    # Hash the key
    key = MD5.new(key.encode('utf8')).hexdigest()
    logging.debug(f"Hashing Key: ${key}")
    # Decode from base64
    enc = b64decode(enc)
    logging.debug(f"Decoding Base64: ${enc}")
    # Get HMAC signature from the start of the message
    signature = enc[:HMAC_SIZE]
    logging.debug(f"Signature: ${signature}")
    # IV is in the next 16 bytes
    iv = enc[HMAC_SIZE:HMAC_SIZE+16]
    logging.debug(f"IV: ${iv}")
    # Generate cipher
    cipher = AES.new(key, AES.MODE_CBC, iv)
    logging.debug(f"Cipher: ${cipher}")
    paddedMessage = enc[HMAC_SIZE+16:]
    logging.debug(f"Padded Encrypted Message: ${paddedMessage}")
    # Decrypt everything after the first 16 bytes (which is the IV)
    message = unpad(cipher.decrypt(enc[HMAC_SIZE+16:]).decode('utf-8'))
    logging.debug(f"Message: ${message}")
    # Generate a signature for the message and check it with what we got
    good_signature= hmac.new(HMAC_KEY.encode('utf-8'), enc[HMAC_SIZE:],  hashlib.sha1).digest()
    logging.debug(f"Our Signature: ${good_signature}")

    if(signature!=good_signature):
        raise Exception("Bad signature on sent data, implying different keys on sender and receiver")
    else:
        print("HMAC Signature is Valid")

    return message
```

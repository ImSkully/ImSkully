---
title: "Python Peer-to-Peer Network"
---

A simple peer-to-peer file sharing torrenting network with encrypted payload transportation and support for multiple clients over sockets with multi-threading. The application emulates multiple clients connecting to a single server in order to retrieve a list of clients which have files, and then be able to transport files across each client via sockets.

**Source:** https://github.com/ImSkully/python-p2p-network

***

# Design Documentation
This design documentation covers the principles behind the core of how each function works and why certain decisions were made during the development process. The overall code structure, functionality and features are outlined in further detail.

## I. Backend Design
The code structure of the application creates a separate function for each allocated command, therefore you may see a variety of functions throughout both client and server sided files.

```python
def addFileCommand(clientAddress, clientSocket, fileName = False):
    if not fileName:
        sendClientMessage(clientAddress, clientSocket, "SYNTAX: /addfile [File Name]")
        return

    if (fileName not in CLIENT_FILES[clientAddress]):
        CLIENT_FILES[clientAddress].append(fileName)
        print("[SERVER] Added new file record for client {}:{}".format(*clientAddress) + ", file: " + fileName)
        sendClientMessage(clientAddress, clientSocket, "You have added the file '" + fileName + "' to the server tracker.")
    else:
        print("[SERVER] Client {}:{}".format(*clientAddress) + " attempted to add file '" + fileName + "' though already recorded they have this file, ignoring request.")
        sendClientMessage(clientAddress, clientSocket, "ERROR: You have already added the file '" + fileName + "' to the server tracker.")
COMMANDS["addfile"] = addFileCommand
```

For instance, the above example is of the `addfile` command. A global Dictionary is defined at the beginning of the server file to store all `COMMANDS`, the function to handle the response of the command is written as normal, and added into the dictionary under the key of what the command name should be in order to invoke it.

## II. Parsing User Input
```python
# If the input is a command.
if (inputCommand[0] == shared.COMMAND_PREFIX):
    inputCommand = inputCommand[1::] # Remove the command prefix.
    commandParameters = inputCommand.split(' ') # Split the string using space as the delimiter.
    theCommand = commandParameters[0] # The command name.
    commandParameters.pop(0) # Remove the command itself from the list.

    # Parameter debug outputs.
    if shared.DEBUG:
        i = 1
        print("[DEBUG] Command: " + theCommand)
        for parameter in commandParameters:
            print("[DEBUG] Parameter #" + str(i) + ": " + parameter)
            i = i + 1

    if (theCommand in COMMANDS): # If the command exists.
        handlingFunction = COMMANDS[theCommand] # Fetch the handling function to invoke.
        handlingFunction(clientAddress, clientSocket, *commandParameters) # Invoke the respective function and pass all parameters.
    else:
        sendClientMessage(clientAddress, clientSocket, "Invalid command specified.")
```

This snippet taken from the aforementioned function displays how it parses the input the user provides in order to understand which command specific function needs to be triggered for this client’s request. Initially, if the character at the beginning of the user’s input matches the current `COMMAND_PREFIX` as defined in the shared python file, then it knows that this is a command which needs to be executed.

Further on, some simple python string manipulation to remove all unrequired string encoding and characters, and all contents of the input are split into an array, separated by a space.

Skipping past the debug snippet of code, we can see that `theCommand` is checked with the contents of the `COMMANDS` array, and if the string of `theCommand` exists in this array, we fetch that command from the array under its index and then invoke the respective function, passing on our parameters.

This command-to-function specific structure makes the code a lot more tidy and simpler to manage, rather than having every single command handler in the same function. It also provides development benefits as there is no need to parse the command parameters every time for each command as they are sent into each function as a separate parameter variable automatically as all `commandParameters` are split when sent to the command’s function.

**Additional to this, it also allows the seamless creation of new commands by simply adding a function to the `COMMANDS` dictionary. It is to be noted that all inputs starting with a backslash `/` on a client denotes commands to be sent to the server, and all input without a backslash `/` are assumed to be client specific commands which are handled on the client and not sent to the server.**

## III. System Specifics
### Ping Command
When a client and server are both running, executing the `/ping` command on the client will trigger the function `pingCommand()`. This function fetches the current unix timestamp and sends it back to the client.

The client is designed to be able to receive command-specific responses and conduct actions based on the server’s response. In this case, the client realizes that the response it receives will contain the server’s epoch time when it was triggered, and then obtains its own epoch time. With the server’s and clients time now obtained, simple subtraction of both times displays the time in milliseconds (or seconds) that the total response took.

### Adding Files
With both a client *(or multiple clients)* and server running, issuing the `/addfile [File Name]` command, where `[File Name]` is the name of the file the client is attempting to broadcast to the server. A dictionary is initialized under the variable `CLIENT_FILES[]` at the initial execution of the server and this maintains a list of all clients and their files. The files for a client are stored under the index of their address and socket in a list.

For example, to obtain the list of all files the client `127.0.0.1:3291` has, you can access it using: `CLIENT_FILES[(“127.0.0.1”, “3291”)]`

If a user disconnects from the server, their list in the dictionary is unset and cleared as the files are no longer available.

### Client-Server File Indexing
The `/findfile` command can be used to conduct a search of the full `CLIENT_FILES` dictionary, which always tracks all files advertised by each client. To make use of this, a minimum of two clients must be connected to the server.

Add a file to the server from a client for something to be found, this can be done with the `/addfile [File Name]` command.
* On **Client 1**, issue the command `/addfile test.mp3` *(To test that multiple clients can advertise the same file, issue the same command on additional clients.)*
* On **Client 2**, issue the command `/findfile test.mp3`, this should display the address and port of every client connected that has the file available. You can also try searching for a file that has not been added to the server yet, and the server will respond with an appropriate response to notify the client if it does not have the file they requested.

The function initialises an empty list when it begins it search for the file, at which point it begins iterating through the dictionary which contains every client connected, and their respective tracked files. Another iteration is conducted to index every file under the current client, and if the file name matches the name of the file the client has requested, then this client is added to the `foundClients` list.

After the search, we check to see if the `foundClients` list is empty or not. If it isn’t, we then prepare a string to contain the address and socket of every client which was found to have file and send this back to the client.

### Client Specific Directories
When a client is started it is given a random socket number to run with, along with this a directory is initialized in the current running directory with the socket number of each specific client. This folder holds all compressed, uncompressed, full and split versions of every file. For a client to reassemble files, it must have all segments present in its socket raw directory, and for a client to split files, it must have the specified file to split in its personal client directory. *(Named after its current socket number)*

![Image of client file directory](https://i.imgur.com/p91OUbb.png)

The above image provides a visual to the structure of the files for each socket. The **All Sockets** segment is the working directory, where the `client.py` is started from. When a client is started, it creates a directory with the socket number used. Inside this directory, the subdirectory `raw` maintains another set of directories named after each file.

## IV. Building Split Files
To trigger the creation of a file:
* On a client, execute `build samplefile.mp3`*, the client must have all segments of the file present, though this can be emulated by ensuring the client’s directory is set to `50` in [`client.py:41`](https://github.com/ImSkully/python-p2p-network/blob/5b12643289113ef0ad6396c8122186b49bbd4c4b/client.py#L41), or by creating the directory structure manually and placing the file segments in yourself.
To split a file into segments:
* On a client, execute `split split_me.mp3`*, the client must have the file in its **Client Files** directory. The client will automatically initialize a raw file directory for the file and place all segments inside of it.

For all command executions, the client will always provide an output to the console with the location of file outputs.

**(Note the lack of a backslash `/` in command input, denoting a client-side command.)*

## V. File Compression
### Compressing Files
The command `compress [File Name]` exists on the client for file compression, simple input the name of the file to compress.
* `compress split_me.mp3` will created a `gzip` of the `.mp3` file in the client’s subdirectory.

### Decompressing Files
The command `decompress [File Name]` also exists on the client to reverse compression, simply input the name of the file to decompress.
* `decompress split_me.mp3.gz` will decompress the `.gz` file and output a `split_me.mp3` file.

## VI. Verified Payload Hashes
With every client-server interaction that occurs, the client forwards its payload with a `sha224` of its contents attached to the beginning of the string that is sent. Once the server receives any input from a client, it takes the attached `sha224` string and creates a hash of its own of the payload contents. The two hashes are then compared to ensure they match. If the hashes match, the request is processed as expected however if they don’t, the server will drop the request and do nothing as the contents of the request were evidently tampered with during data transmission.

```python
command = hashlib.sha224(command.encode()).hexdigest() + ";HASH;" + command
command = command.encode() # Encode the command to byte code.
SOCKET.sendall(command) # Send the encoded command to the server.
```

This segment taken from the client file displays how the command variable containing the input of the client from the terminal is hashed using sha224, a string separator containing the contents `;HASH;` is added on after the hash, and the client input is then placed at the end of the string. All of this is then encoded to bytes and sent off to the client.

The format of the request is as follows:
* **INPUT:** `/findfile test.mp3`
* **HASH GENERATED:** `ABCDEFGHISAMPLEHASHJKLMNOP`
* **FULL OUTPUT BEFORE ENCODING:** `ABCDEFGHISAMPLEHASHJKLMNOP;HASH;/findfile test.mp3`

The following server-side code displays how the received payload is decoded, split into two parts by using the `;HASH;` as a separator, and then rehashing the contents. The hashes are then checked to ensure the request is valid and hasn’t been tampered with.

## VII. Client Disconnect
When a client initially connects to the server, they are given a new index in the `CLIENT_FILES` dictionary. When they disconnect, the list of their file records is automatically deleted and no longer tracked.
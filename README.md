# AUTHOR
Konstantinos Kazatzhs kon.kazatzis@gmail.com

## Description
This is an emulator for a DHT implementing the chord protocol. This project is implemented in C++. The basic 
operations are insert(file,value), query(file), delete(file), join(node), depart(node).

## Compilation
In order to compile this program run the make command. You need a suitable C++ comppiler and the openssl, 
crypto and pthread libraries. In the Makefile change the debug variable from DEBUG ?= 1 to DEBUG ?= 0 for 
measurements. The default is DEBUG ?= 1

## Arhitecture overview
Each node listens to PORT_BASE + node_id. This means that all the ports in the range 
[PORT_BASE,PORT_BASE+ring_size) have the potential to be occupied from the emulator. As a result if a port 
in that range is taken some node in the ring will not be able to process requests and the routing protocol 
will fail. When a request (see below for requests) comes in a node's port the node will answer himself to the
client. This means that if the node is not the suitable one to insert,delete,search he will forward the 
message to the next node waiting for the answer. This initial node waits at a special well known same port 
for every node, the PORT_BASE + ring size port. The initial node will then forward to the client after 
receiving the final answer in that port. Requests:
	INSERT:
		INSERT,NAME OF THE FILE,VALUE
	QUERY:
		QUERY,NAME OF THE FILE
	DELETE FILE:
		DELETE,NAME OF THE FILE
	RETURN ALL FILES OF A NODE:
   		QUERY *	
	
## Implementation
The above requests are what the user sends. Inside the system the message being forwarded has the name of the
initial node and his id.

## Running test
Execute the chord_node providing the number of nodes and the log of the ring size. If everyting goes ok you 
will be able to see an interface of all the nodes in the system. Example : ./chord_emu 10 13 Sample output

	Node: 53699 Name: localhost
		Successor:4798	 Predecessor:4408
		Name: localhost	 Name:localhost

	Node: 53950 Name: localhost
		Successor:5275	 Predecessor:4547
		Name: localhost	 Name:localhost

	Node: 54427 Name: localhost
		Successor:5756	 Predecessor:4798
		Name: localhost	 Name:localhost

Beyond this point you can send your requests to the nodes.
Example: Type in the terminal nc localhost 53950 (open a connection to hostname:53950) Then type your request
in the above format i.e. 

INSERT,the name of the file goes here,value_of_the_file
    ATTENTION: The commas are very important and the value is an integer
	Same for the other requests
		
You can run the executable run which reads the inserts.txt, query.txt and requests.txt and executes the 
operations. For each run of the executable run you will see in the end the operations per second value. 
DEBUG?=1 in order to show only this value

ATTENTION: File: Thank You (Falettinme Be Mice Elf Agin) HAS THE SAME HASH VALUE WITH  Smells Like Teen Spirit FOR 12 ring_size


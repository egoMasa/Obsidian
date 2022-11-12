# Guide SOCKET python

# Différence Synchrone et Asynchrone
## Asynchrone : 
* On attend que la tache A soit complément finie pour commencer la tache B
* Données transférées sous forme d'octets ou caractères
* Taux de transfert plus lent que en synchrone
* Même manière que UDP avec envoi sans réception assuré
## Synchrone : 
* On exécute plusieurs taches simultanément, on passer d'un vers l'autre
* Données transférées sous forme de trames
* Communication en temps réel
* Synchronisation via signal d'horloge entre émetteur et récepteur
* Même manière que TCP avec envoi et réception assuré
## Asyncio
* Bibliothèque python permettant de faire programmation asynchrone avec syntaxe (async/await)
* Base pour utilisateurs réseau, serveur web, base de données, files d'exécution
* Effectue des entrées/sorties réseau et communication inter-processus 

## Architecture peer to peer (client-serveur)
* Un serveur, plusieurs clients
* Etapes de communication:
	* Serveur : Attente de demande de connexion
	* Client : Demande de connexion
	* Serveur : Acceptation de la connexion
	* Client : Données envoyés vers serveur
	* Serveur : Echange de données/ réponse du serveur
	* Fermeture de la connexion
* Conditions pour connecter client au serveur
	* Préciser IP serveur ou son host_name
	* Préciser numéro de port associé à l'application (port d'écoute serveur)
* Conditions pour que serveur repond au client:
	* Préciser le nom d'hôte du client
	* Préciser le numéro de port associé à l'application client qui doit recevoir les réponses
# 1) C'est quoi un socket
* Socket = Connecteur réseau, interface aux services client-serveur, permet d'initier session TCP et d'envoyer et recevoir des données
* Connecteur INET = IPV4
* Connecteur INET6 = IPV6
* Connecteur STREAM = TCP
* Connecteur DGRAM = UDP
# 2) SOCKET Procédurale
* Connecteur (socket) client
```python
socket_client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
```
* Connecteur (socket) serveur
```python
socket_serveur = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
```

❗️ On a besoins de 2 scripts

* Script ***serveur*** pour écouter les requêtes clients
```python
#Création de connecteur IPV4 en TCP
socket_serveur = socket.socket(socket.AF_INET,socket.SOCK_STREAM)

#Installer connecteur sur port 80, ecouteur et se rend visible 
socket_serveur.bind((socket.gethostname(),80))

#File d'attente de 5 requêtes de connexion
socket_serveur.listen(5)

while True :
	client_socket, address = s.accept()
	print("Connexion avec "+address+" reussie")
```
* Script ***client*** pour se connecter sur le serveur
```python
#Création de connecteur IPV4 en TCP
socket_client = socket.socket(socket.AF_INET,socket.SOCK_STREAM)

#Connexion au serveur sur port 80
socket_client.connect((socket.gethostname(),80))
```
## Intéraction client-serveur
* Le socket ecoute les connexion
* Quand client se connecte, le serveur accepte la connexion via la méthode ***accept()***
* Le client établie une connexion avec le serveur via la méthode ***connect()***
* Les données sont échangées entre client et serveur via les méthodes ***send()*** et ***recv()***

## Exemple serveur socket
```python
import socket

serveur = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
serveur.bind(('',50000)) #Ecoute sur le port 50000
serveur.listen(5) # File d'attente de 5 demandes max

while True:
	client,infoClient = serveur.accept()
	print("Client connecté, adresse : "+infosClient[0])
	
	requete = client.recv(255) #Recoit 255 octets
	print(requete.decode('utf-8')) #Conversion binaire en texte
	
	reponse = "Bonjour, je suis le serveur"
	client.send(reponse.encode('utf-8')) # On convertit le message en binaire pourn l'envoyer
	
	print("Connexion fermée")
	client.close()
	
serveur.close()
```
## Exemple client socket
```python
import socket

adresseIP = "127.0.0.1"
port = 50000
client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect((adresseIP,port))
print('Connecté au serveur')
client.send("Bonjour, je suis le client".encode("utf-8"))
reponse = client.recv(255)
print(reponse.decode("utf-8"))
print("Connexion fermée")
client.close()
```

* Lancer les deux scripts
```
python3 serversocket.py
python3 clientsocket.py
```
# 3) SOCKET POO
* Classe Client Socket
```python
class ClientSocket:
	#Instanceier un socket client
	def __init__(self, sock=None):
		self.sock = socket.socket(socket.AF_NET,socket.SOCK_STREAM)

	# Connexion au serveur
	def connect(self,host,port):
		self.sock.connect((host,port))
		print("Connecté au serveur :", host, port)

	#Envoi de message au serveur
	def mysend(self,message):
		self.sock.send(message.encode("uft-8"))

	#Reception de la réponse serveur
	def myreceive(self):
		reponse = self.sock.recv(255)
		print(reponse.decode("utf-8"))

	#Fermeture de la connexion
	def dysconnect(self):
		print("Connexion fermée")
		self.sock.close()

if __name__ == "__main__":
	msoc = ClientSocket()
	msoc.connect("localhost",50000)
	msoc.mysend("Bonjour du client TCP"+str(datetime.now()))
	msoc.myreceive()
	msoc.dysconnect()
```

# 4) Serveur WEB 

```python
import http.server
import socketserver
#Port d'écoute du serveur
PORT = 8088
#Utilisation scripts
Handler = http.server.SimpleHTTPRequestHandler
#Installer serveur sur le port
with socketserver.TCPServer(("",PORT),Handler) as httpd:
	print("Serving at port ", PORT)
	#Boucler le serveur sur le port
	httpd.serve_forever()
```
* Sur navigateur
```
localhost:8088
```

# 5) P2P TCP communicant
* Serveur communiquant TCP
```python
import socket, sys
#Création du socket
mySocket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
try: #Liaison du socket à une adresse et un port
	mySocket.bind(('localhost',50000))
except socket.error:
	print("La liason du socket à échoué ! ")
	sys.exit()
while 1 :
	print("Serveur prêt, en attente de requêtes")
	mySocket.listen(5)
	connexion, adresse = mySocket.accept()
	print("Client connecté, adresse IP "+ adresse[0]+ "port "+adresse[1])
	s = "Vous êtes connecté au serveur, envoyez vos messages"
	connexion.send(s.encode("utf-8"))
	message_client = connexion.recv(1024)
	while 1 :
		print("Client"+message_client.decode("utf-8"))
		if message_client("utf-8").upper() == "FIN"
			break
		message_serveur = input("Serveur >> ")
		connexion.send(message_serveur.encode('utf-8'))
		message_client = connexion.recv(1024)
	# Fermeture de la connexion
	connexion.send("Au revoir".encode('utf-8'))
	print("CONNEXION INTERROMPUE")
	connexion.close()
```
* Client communiquant TCP
```python
import socket, sys
#Création du socket
mySocket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
try: #Liaison du socket à une adresse et un port
	mySocket.connect(('localhost',50000))
except socket.error:
	print("Connexion échoué ! ")
	sys.exit()
print("CONNEXION ETABLIE AVEC LE SERVEUR")
message_serveur = mySocket.recv(1024)
while 1:
	if message_serveur.decode("utf-8").upper() == 'FIN'
		break
	print('Server>> ',message_serveur.decode("utf-8"))
	message_client = input("Client >> ")
	mySocket.send(message_client.encode("utf-8"))
	message_serveur = mySocket.recv(1024)
print("Connexion interrompue")
mySocekt.close()
```

# 4) Le Multithread
* Permet l'execution de plusieurs opérations simultanement (synchrone)
* Execution parallèle
* Module ***theading*** sous python
* Exemple 
```python
#!/usr/bin/python
import threading
def compteur(leThread):
	for i in range(3):
		print(leThread + " : "+ str(i))
threadA = threading.Thread(None, compteur, None, ("Thread A"),{})
threadB = threading.Thread(None, compteur, None, ("Thread B"),{})
threadA.start()
threadB.start()

# threading.Thread( groupe, cible, nom, arguments, dictionnaireArguments )
# groupe = None car réservation future
# cible = Fonction à executer
# nom = Nom du thread (facultatif, None si on veut)
# arguments =  tuple avec arguements de la fonction cible
# dictionnaireArguments = Dico donnant argurment fonction cible 
```
# Serveur MultiThread
* Coté serveur
```python
import socket
import threading

threadsClients = []

def instanceServeur (client, infosClient):

	adresseIp = infoClient[0]
	port = str(infoClient[1])
	print("Instance de serveur prêt pour "+adresseIP+" : "+port)
	message=""
	
	while message.upper() != "FIN"
		message=client.recv(255).decode('utf-8')
		print("Message recu du client "+adressIP+" : "+message)
		client.send("Message recu".encode("utf-8"))
		
	print("Connexion fermée avec "+adresseIP+" : "+port)
	client.close()
	
serveur = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
serveur.bind(('',50000))
serveur.listen(4)

while True:
	client,infoClient = serveur.accept()
	threadsClient.append(threading.Threading(None,instanceServeur,None,(client,infoClient),{}))
	threadsClient[-1].start()
	
serveur.close()
```
* Côte client
```python
import socket
adresseIP= "127.0.0.1"
port = 50000
client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect((adresseIP,port))
print("Connecté au serveur")
print("Tapez FIN pour terminer la conversation")

message = ""
while message.upper() != 'FIN'
	message = input('>> ')
	client.send(message.encode("utf-8"))
	reponse = client.recv(255)
	print(reponse.decode(utf-8))
print("Connexion fermée")
client.close()

```
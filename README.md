# Network Defender - Prerequisites

## Project setup

Git Repo: https://github.com/justind1995/NetworkDefender

Please ensure that you have node.js install on your machine. It can be installed here: 

```
https://docs.npmjs.com/downloading-and-installing-node-js-and-npm
```
I am running version:

```
v18.16.0
```

```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

# NetworkDefender - Gameplay Instructions

To play NetworkDefender, the goal is to place network components in the numbered slots (ie. those marked 1, 2, 3, etc) to accomplish a specified goal. The board is laid out from left to right and shold be played that way.

To do this, click the 'add or configure component' button. That will allow you to select which kind of component to select, its position (which correspond to the numbered boxes on the game board) and then configure it.

Note: If you wish to alter an already configured component, simply reopen the add/configure modal and reconfigure a new component for the desired position. The game will then replace the previous component with the newly configured one.

### Configuring Network Components

configuring components is made to be intentionally high-level and simple to make the game approachable for beginners. After selecting the position and component type, the user can then define what devices it is to connect to (or in the case of firewalls which ports to allow traffic on).  

Those configurations are defined in more detail below:

**Routers** can connect to the internet and, in fact, are the only component that is able to connect to the 'internet' node.  Once you place a router, you can take traffic from the internet. For the purposes of this game, routers can connect to Switches and Firewalls. To configure a router, simply put a comma seperated list of nodes to connect to.

For instance, if the router is at position '1' and you wish to connect it to positions '2' and '3', simply put '2' and '3' in the configuration text box.

**Switches** can connect to Firewalls and Servers in the game (primarily servers). Their primary focus is to distribute traffic across a pool of servers. Configuration for switches will be similar to Routers except that the server names on the gameboard will be passed in to the configuration.  For example, if you wish to connect to Servers 1,2, and 3, you will type 'Server1,Server2,Server3'. Into the text box.

**Note** its important to not leave spaces between the comma seperated values in the configuration text box.

**Firewalls** permit or restrict traffic on certain port(s). Many of the Scenarios will require the player to only allow traffic from certain ports to servers/internal network. This can be done by putting a comma seperated list of ports to allow in the configuration textbox like so '8443,443'.

**Note** For the purposes of this game, the firewall will automatically connect to whatever node is to its right (typically a switch). This game is meant to be read from left to right.

### Solution Guide ###

Below is the solution guide for all three levels to assist if understanding of the games mechanics isn't exactly clear or the player gets stuck.

**Level 1** 

Level 1 is laid out with the internet to the left and a pool of three servers to the right. The solution is to place the router in position 1 and connect it to node '2'. Node '2' would be a firewall to filter traffic to only allow traffic from ports 8443 and 443.  Finally, position three would be a switch to connect all three Servers to the rest of the network.

**Level 2**

Level 2 is laid out with the Internet to the left and a pool of three servers to the right.  Unlike level 1 though, there is a 'DMZ' defined in the middle of 4 numbered nodes. The goal of this level is to bring in traffic via a router at position 1, then filter that traffic to port 8443 via a firewall at position 2 before it enters the DMZ. Then, another firewall must be placed at position 3 that filters traffic to port 443 before entering the backend network. Finally, a switch must be placed at position 4 to distribute traffic to the 3 backend servers.

**Level 3**

Level 3 is laid out a bit differently than levels 1 and 2. The goal of this level is to bring in traffic from the internet via two different routers that create two different internal networks. Traffic to the servers should only be via port 8443. The solution to this level is as follows. Positions 1 & 2 should be routers to bring in traffic from the internet. The router at position 1 should forward traffic to position 3 and the router at position 2 should forward traffic to position 4.

Positions 3 & 4 should be firewalls to filter traffic to port 8443. Per the game's premise, firewall 3 would forward to node 5 and firewall 4 would forward to node 6 (that connection isn't configured but assumed). 

Positions 5 & 6 would be switches. Position 5 would connect to Server1 and Server2 and position 6 would connect to Server3 and 4.




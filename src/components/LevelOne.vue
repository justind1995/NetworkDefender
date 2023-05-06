<template>
<h4 v-if="level1">Level 1: Starting with the basics</h4>
<p class="instructions" v-if="level1">Configure a network that takes traffic from the internet via a router and filters the traffic using a firewall to only allow traffic from ports 8443 and 443. This traffic will also need to be able to reach all three servers (Server1, Server2, and Server3)</p>
<p class="hint" v-if="level1">Hint: Switches can distribute traffic to multiple servers and firewalls often come after routers</p>

<h4 v-if="level2">Level 2: Walls of Fire</h4>
<p class="instructions" v-if="level2">Configure a network that takes traffic from the internet via a router and filters the traffic using a firewall to only allow traffic from port 8443 to the DMZ. Then configure another firewall to only allow traffic from port 443 to the 3 backend servers.</p>
<p class="hint" v-if="level2">Hint: Switches can distribute traffic to multiple servers and firewalls often come after routers</p>

<h4 v-if="level3">Level 3: Routing the Competition</h4>
<p class="instructions" v-if="level3">Configure a network that takes traffic from the internet via two routers and filters the traffic using two firewalls to only allow traffic from port 8443. The first router/firewall should connect to Servers 1 and 2. The second Router and Firewall should connect to Servers 3 and 4</p>
<p class="hint" v-if="level3">Hint: Switches can distribute traffic to multiple servers and firewalls often come after routers</p>

  <button @click="showModal">add or configure component</button>
  <button @click="evaluateLevel">submit </button>
    
  <modal
  v-show="isModalVisible"
  @close="closeModal"
  @submission="handleSubmission"
>
  <template v-slot:header>
    Select A Network Component
  </template>

  <template v-slot:body>
    <p>
      Network Component
    </p>
    <select name="components" id="components">
      <option value="switch">Switch</option>
      <option value="firewall">Firewall</option>
      <option value="router">Router</option>
    </select>
  </template>

  <template v-slot:body2>
    <p>
      Position
    </p>
    <select v-if="level1" name="position" id="position">
      <option value="1">1</option>
      <option value="2">2</option>
      <option value="3">3</option>
    </select>

    <select v-if="level2" name="position" id="position">
      <option value="1">1</option>
      <option value="2">2</option>
      <option value="3">3</option>
      <option value="4">4</option>
    </select>

    <select v-if="level3" name="position" id="position">
      <option value="1">1</option>
      <option value="2">2</option>
      <option value="3">3</option>
      <option value="4">4</option>
      <option value="5">5</option>
      <option value="6">6</option>
    </select>
  </template>

  <template v-slot:body3>
    <p>
      Configuration: For firewall enter a comma seperated list of ports to allow. For switches/routers enter a comma seperated list of nodes to be connected to by name (ex: server1,server2)
    </p>
    <input type="text" id="configurationText" name="configurationText">
  </template>

  <template v-slot:footer>
    This is a new modal footer.
  </template>
</modal>

<resultsModal
  v-if="buttonDisabled"
  v-show="isPassFailVisible"
  @close="closePassFail"
  @levelchange="nextLevel"
>
  <template v-slot:header>
    Results!
  </template>

  <template v-slot:body>
    <p id="resultsStr">
       
    </p>
  </template>

  <template v-slot:footer>
    This is a new modal footer.
  </template>
</resultsModal>


  <netComp v-if="level1" v-show="isBoardVisible"
  class="left" positionStr="1" id="position1"/>
  <netComp v-if="level1" v-show="isBoardVisible"
  class="center" positionStr="2" id="position2"/>
  <netComp v-if="level1" v-show="isBoardVisible"
  class="right" positionStr="3" id="position3"/>
  <netComp v-if="level1" v-show="isBoardVisible"
  class="leftInternet" positionStr="Internet" id="internet"/>
  <netComp v-if="level1" v-show="isBoardVisible"
  class="server1" positionStr="Server1" id="server1"/>
  <netComp v-if="level1" v-show="isBoardVisible"
  class="server2" positionStr="Server2" id="server2"/>
  <netComp v-if="level1" v-show="isBoardVisible"
  class="server3" positionStr="Server3" id="server3"/>

  <netComp v-if="level2" v-show="isBoardVisible"
  class="left" positionStr="1" id="position1"/>
  <netComp v-if="level2" v-show="isBoardVisible"
  class="left-center" positionStr="2" id="position2"/>
    <netComp v-if="level2" v-show="isBoardVisible"
  class="center" positionStr="DMZ" id="position3"/>
  <netComp v-if="level2" v-show="isBoardVisible"
  class="right-center" positionStr="3" id="position4"/>
  <netComp v-if="level2" v-show="isBoardVisible"
  class="right" positionStr="4" id="position5"/>
  <netComp v-if="level2" v-show="isBoardVisible"
  class="leftInternet" positionStr="Internet" id="internet"/>
  <netComp v-if="level2" v-show="isBoardVisible"
  class="server1" positionStr="Server1" id="server1"/>
  <netComp v-if="level2" v-show="isBoardVisible"
  class="server2" positionStr="Server2" id="server2"/>
  <netComp v-if="level2" v-show="isBoardVisible"
  class="server3" positionStr="Server3" id="server3"/>

<!-- Internet  -->
  <netComp v-if="level3" v-show="isBoardVisible"
  class="leftInternet" positionStr="Internet" id="internet"/>

 <!-- Routers  2 -->
  <netComp v-if="level3" v-show="isBoardVisible"
  class="left-lvl3" positionStr="1" id="position1"/>
  <netComp v-if="level3" v-show="isBoardVisible"
  class="left-low" positionStr="2" id="position2"/>

  <!-- Firewalls 2 -->
  <netComp v-if="level3" v-show="isBoardVisible"
  class="center-lvl3" positionStr="3" id="position3"/>
  <netComp v-if="level3" v-show="isBoardVisible"
  class="center-low" positionStr="4" id="position3"/>

  <!-- Switches 2 -->
  <netComp v-if="level3" v-show="isBoardVisible"
  class="right-lvl3" positionStr="5" id="position5"/>
  <netComp v-if="level3" v-show="isBoardVisible"
  class="right-low" positionStr="6" id="position6"/>

  <!-- Servers 4 -->
  <netComp v-if="level3" v-show="isBoardVisible"
  class="server1-lvl3" positionStr="server1" id="server1"/>
  <netComp v-if="level3" v-show="isBoardVisible"
  class="server2-lvl3" positionStr="server2" id="server2"/>
  <netComp v-if="level3" v-show="isBoardVisible"
  class="server3-lvl3" positionStr="server3" id="server3"/>
  <netComp v-if="level3" v-show="isBoardVisible"
  class="server4-lvl3" positionStr="server4" id="server4"/>
<!-- <connectingLine class=""/> -->
</template>

<script>
import modal from './NetworkComponentModal.vue';
import netComp from './NetworkComponent.vue';
import resultsModal from './ResultsModal.vue';
import {statusMessage} from '../store';
let statusMessageCopy = statusMessage;
let componentMap = new Map();

// import connectingLine from './ConnectingLine.vue';

export default {
  name: 'LevelOne',
  components: { 
    modal,
    netComp,
    resultsModal
    // connectingLine
  },
  data() {
      return {
        isModalVisible: false,
        isBoardVisible: true,
        isPassFailVisible: false,
        statusMessageCopy,
        level1: true,
        level2: false,
        level3: false,
        buttonDisabled: true
      };
    },
    methods: {
      showModal() {
        this.isModalVisible = true;
        this.isBoardVisible = false;
      },
      closeModal() {
        this.isModalVisible = false;
        this.isBoardVisible = true;
      },
      closePassFail() {
        this.isPassFailVisible = false;
        this.isBoardVisible = true;
      },
      nextLevel() {

        if(this.level1){
          this.level1 = false;
          this.level2 = true;
        }
        else if(this.level2){
          this.level2 = false;
          this.level3 = true;
        }
        else if(this.level3){
          console.log('You Win!')
        }

        this.isPassFailVisible = false;
        this.isBoardVisible = true;
        this.buttonDisabled = true;
        componentMap.clear()
      },
      handleSubmission() {
        console.log('emitted event')
        var component = document.getElementById("components");
        // var value = e.value;
        var componentText = component.options[component.selectedIndex].text;

        var position = document.getElementById("position");
        // var value = e.value;
        var positionText = position.options[position.selectedIndex].text;

        let componentToStore = {}

        let configList = document.getElementById('configurationText').value
        let configArr = configList.split(',');

        if(componentText === 'Firewall'){
           console.log('firewall')
           componentToStore = {componentType: componentText, allowedPorts: configArr};
        }
        else if(componentText === 'Switch'){
           console.log('switch')
           componentToStore = {componentType: componentText, connectedHosts: configArr};
        }
        else if(componentText === 'Router'){
           console.log('router')
           componentToStore = {componentType: componentText, connectedSwitches: configArr};
        }
        
        if(componentMap.has(positionText)){
          componentMap.delete(positionText, componentToStore);
        }

        componentMap.set(positionText, componentToStore);

        console.log(componentMap)

        console.log('successfully created network component')

        this.isModalVisible = false;
        this.isBoardVisible = true;
      },
      evaluateLevel(){
        document.getElementById('resultsStr').innerHTML = "";
        statusMessageCopy = '';

        console.log('EVALUATING LEVEL')

      if(this.level1){
        if(componentMap.size !== 3){
          statusMessageCopy += 'NOT ENOUGH NETWORK COMPONENTS';
          this.isPassFailVisible = true;
          this.isBoardVisible = false;
          document.getElementById('resultsStr').innerHTML = statusMessageCopy;
          return 1;
        }

        if(componentMap.get('1').componentType != 'Router') {
          statusMessageCopy += 'ONLY ROUTERS CAN CONNECT TO THE INTERNET';
          this.isPassFailVisible = true;
          this.isBoardVisible = false;
          document.getElementById('resultsStr').innerHTML = statusMessageCopy;
          return 1;
        }

        if(componentMap.get('1').componentType == 'Router') {
          if(componentMap.get('1').connectedSwitches != 2) {
            statusMessageCopy += 'ROUTER IS NOT CONNECTED TO SWITCH OR FIREWALL';
            this.isPassFailVisible = true;
            this.isBoardVisible = false;
            document.getElementById('resultsStr').innerHTML = statusMessageCopy;
            return 1;
          }
        }

        if(componentMap.get('2').componentType != 'Firewall') {
          statusMessageCopy += 'BEST TO HAVE A FIREWALL TO PROTECT THE INTERNAL NETWORK';
        }

        if(componentMap.get('2').componentType == 'Firewall') {
           if(componentMap.get('2').allowedPorts){
              if(!componentMap.get('2').allowedPorts.includes('8443')){
                statusMessageCopy += 'FIREWALL IS NOT PERMITTING TRAFFIC ON REQUIRED PORT: 8443';
                this.isPassFailVisible = true;
                this.isBoardVisible = false;
                document.getElementById('resultsStr').innerHTML = statusMessageCopy;
                return 1;
              }

              if(!componentMap.get('2').allowedPorts.includes('443')){
                statusMessageCopy += 'FIREWALL IS NOT PERMITTING TRAFFIC ON REQUIRED PORT: 443';
                this.isPassFailVisible = true;
                this.isBoardVisible = false;
                document.getElementById('resultsStr').innerHTML = statusMessageCopy;
                return 1;
              }
           }
        }

        if(componentMap.get('2').componentType == 'Switch' || componentMap.get('2').componentType == 'Router'){
          statusMessageCopy += 'REDUNDANT SWITCH/ROUTER';
        }

        if(componentMap.get('3').componentType != 'Switch'){
          statusMessageCopy += 'MUST HAVE A SWITCH TO CONNECT THE POOL OF SERVERS';
          this.isPassFailVisible = true;
          this.isBoardVisible = false;
          document.getElementById('resultsStr').innerHTML = statusMessageCopy;
          return 1;
        }

        if(componentMap.get('3').componentType == 'Switch'){
           if(!componentMap.get('3').connectedHosts.includes('Server1')){
             statusMessageCopy += 'SWITCH NOT CONNECTED TO REQUIRED SERVER1';
           }
           if(!componentMap.get('3').connectedHosts.includes('Server2')){
             statusMessageCopy += 'SWITCH NOT CONNECTED TO REQUIRED SERVER2';
           }
           if(!componentMap.get('3').connectedHosts.includes('Server3')){
             statusMessageCopy += 'SWITCH NOT CONNECTED TO REQUIRED SERVER3';
           }
        }

        statusMessageCopy += 'SUCCESS! LEVEL COMPLETE';
        this.isPassFailVisible = true;
        this.isBoardVisible = false;
        document.getElementById('resultsStr').innerHTML = statusMessageCopy;
        this.buttondisabled = false;

        console.log(statusMessageCopy)
      }

      if(this.level2){
        console.log('Evaluating Level 2')
        if(componentMap.size !== 4){
          statusMessageCopy += 'NOT ENOUGH NETWORK COMPONENTS';
          this.isPassFailVisible = true;
          this.isBoardVisible = false;
          document.getElementById('resultsStr').innerHTML = statusMessageCopy;
          return 1;
        }

        if(componentMap.get('1').componentType != 'Router') {
          statusMessageCopy += 'ONLY ROUTERS CAN CONNECT TO THE INTERNET';
          this.isPassFailVisible = true;
          this.isBoardVisible = false;
          document.getElementById('resultsStr').innerHTML = statusMessageCopy;
          return 1;
        }

        if(componentMap.get('1').componentType == 'Router') {
          if(componentMap.get('1').connectedSwitches != 2) {
            statusMessageCopy += 'ROUTER IS NOT CONNECTED TO SWITCH OR FIREWALL';
            this.isPassFailVisible = true;
            this.isBoardVisible = false;
            document.getElementById('resultsStr').innerHTML = statusMessageCopy;
            return 1;
          }
        }

        if(componentMap.get('2').componentType != 'Firewall') {
          statusMessageCopy += 'THIS LEVEL IS FOCUSED ON FIREWALLS, ENSURE THAT FIREWALLS ARE ON EACH SIDE OF THE DMZ';
          this.isPassFailVisible = true;
          this.isBoardVisible = false;
          document.getElementById('resultsStr').innerHTML = statusMessageCopy;
          return 1;
        }

        if(componentMap.get('2').componentType == 'Firewall') {
          console.log('COMPONENT ARRAY 1')
          console.log(componentMap.get('2'))
           if(componentMap.get('2').allowedPorts){
              if(!componentMap.get('2').allowedPorts.includes('8443')){
                console.log('ARRAY CHECK FAILED')
                statusMessageCopy += 'FIREWALL IS NOT PERMITTING TRAFFIC ON REQUIRED PORT: 8443';
                this.isPassFailVisible = true;
                this.isBoardVisible = false;
                document.getElementById('resultsStr').innerHTML = statusMessageCopy;
                return 1;
              }
           }
        }

        if(componentMap.get('3').componentType != 'Firewall') {
          statusMessageCopy += 'THIS LEVEL IS FOCUSED ON FIREWALLS, ENSURE THAT FIREWALLS ARE ON EACH SIDE OF THE DMZ';
          this.isPassFailVisible = true;
          this.isBoardVisible = false;
          document.getElementById('resultsStr').innerHTML = statusMessageCopy;
          return 1;
        }

        if(componentMap.get('3').componentType == 'Firewall') {
           if(componentMap.get('3').allowedPorts){
              if(!componentMap.get('3').allowedPorts.includes('443')){
                statusMessageCopy += 'FIREWALL IS NOT PERMITTING TRAFFIC ON REQUIRED PORT: 443';
                this.isPassFailVisible = true;
                this.isBoardVisible = false;
                document.getElementById('resultsStr').innerHTML = statusMessageCopy;
                return 1;
              }
           }
        }

        if(componentMap.get('4').componentType != 'Switch'){
          statusMessageCopy += 'MUST HAVE A SWITCH TO CONNECT THE POOL OF SERVERS';
          this.isPassFailVisible = true;
          this.isBoardVisible = false;
          document.getElementById('resultsStr').innerHTML = statusMessageCopy;
          return 1;
        }

        if(componentMap.get('4').componentType == 'Switch'){
           if(!componentMap.get('4').connectedHosts.includes('Server1')){
             statusMessageCopy += 'SWITCH NOT CONNECTED TO REQUIRED SERVER1';
           }
           if(!componentMap.get('4').connectedHosts.includes('Server2')){
             statusMessageCopy += 'SWITCH NOT CONNECTED TO REQUIRED SERVER2';
           }
           if(!componentMap.get('4').connectedHosts.includes('Server3')){
             statusMessageCopy += 'SWITCH NOT CONNECTED TO REQUIRED SERVER3';
           }
        }

        statusMessageCopy += 'SUCCESS! LEVEL COMPLETE';
        this.isPassFailVisible = true;
        this.isBoardVisible = false;
        document.getElementById('resultsStr').innerHTML = statusMessageCopy;
        this.buttondisabled = false;

        console.log(statusMessageCopy)
      }

      if(this.level3){
        if(componentMap.size !== 6){
          statusMessageCopy += 'NOT ENOUGH NETWORK COMPONENTS';
          this.isPassFailVisible = true;
          this.isBoardVisible = false;
          document.getElementById('resultsStr').innerHTML = statusMessageCopy;
          return 1;
        }

        if(componentMap.get('1').componentType != 'Router') {
          statusMessageCopy += 'ONLY ROUTERS CAN CONNECT TO THE INTERNET';
          this.isPassFailVisible = true;
          this.isBoardVisible = false;
          document.getElementById('resultsStr').innerHTML = statusMessageCopy;
          return 1;
        }

        if(componentMap.get('2').componentType != 'Router') {
          statusMessageCopy += 'ONLY ROUTERS CAN CONNECT TO THE INTERNET';
          this.isPassFailVisible = true;
          this.isBoardVisible = false;
          document.getElementById('resultsStr').innerHTML = statusMessageCopy;
          return 1;
        }

        if(componentMap.get('3').componentType != 'Firewall') {
          statusMessageCopy += 'ONLY ROUTERS CAN CONNECT TO THE INTERNET';
          this.isPassFailVisible = true;
          this.isBoardVisible = false;
          document.getElementById('resultsStr').innerHTML = statusMessageCopy;
          return 1;
        }

        if(componentMap.get('4').componentType != 'Firewall') {
          statusMessageCopy += 'ONLY ROUTERS CAN CONNECT TO THE INTERNET';
          this.isPassFailVisible = true;
          this.isBoardVisible = false;
          document.getElementById('resultsStr').innerHTML = statusMessageCopy;
          return 1;
        }


        if(componentMap.get('3').componentType == 'Firewall') {
          console.log('COMPONENT ARRAY 1')
          console.log(componentMap.get('3'))
           if(componentMap.get('3').allowedPorts){
              if(!componentMap.get('3').allowedPorts.includes('8443')){
                console.log('ARRAY CHECK FAILED')
                statusMessageCopy += 'FIREWALL IS NOT PERMITTING TRAFFIC ON REQUIRED PORT: 8443';
                this.isPassFailVisible = true;
                this.isBoardVisible = false;
                document.getElementById('resultsStr').innerHTML = statusMessageCopy;
                return 1;
              }
           }
        }

        if(componentMap.get('4').componentType == 'Firewall') {
          console.log('COMPONENT ARRAY 1')
          console.log(componentMap.get('4'))
           if(componentMap.get('4').allowedPorts){
              if(!componentMap.get('4').allowedPorts.includes('8443')){
                console.log('ARRAY CHECK FAILED')
                statusMessageCopy += 'FIREWALL IS NOT PERMITTING TRAFFIC ON REQUIRED PORT: 8443';
                this.isPassFailVisible = true;
                this.isBoardVisible = false;
                document.getElementById('resultsStr').innerHTML = statusMessageCopy;
                return 1;
              }
           }
        }

        if(componentMap.get('5').componentType != 'Switch') {
          statusMessageCopy += 'ONLY ROUTERS CAN CONNECT TO THE INTERNET';
          this.isPassFailVisible = true;
          this.isBoardVisible = false;
          document.getElementById('resultsStr').innerHTML = statusMessageCopy;
          return 1;
        }

        if(componentMap.get('6').componentType != 'Switch') {
          statusMessageCopy += 'ONLY ROUTERS CAN CONNECT TO THE INTERNET';
          this.isPassFailVisible = true;
          this.isBoardVisible = false;
          document.getElementById('resultsStr').innerHTML = statusMessageCopy;
          return 1;
        }

        if(componentMap.get('5').componentType == 'Switch'){
           if(!componentMap.get('5').connectedHosts.includes('Server1')){
             statusMessageCopy += 'SWITCH NOT CONNECTED TO REQUIRED SERVER1';
           }
           if(!componentMap.get('5').connectedHosts.includes('Server2')){
             statusMessageCopy += 'SWITCH NOT CONNECTED TO REQUIRED SERVER2';
           }
        }

        if(componentMap.get('6').componentType == 'Switch'){
           if(!componentMap.get('6').connectedHosts.includes('Server3')){
             statusMessageCopy += 'SWITCH NOT CONNECTED TO REQUIRED SERVER1';
           }
           if(!componentMap.get('6').connectedHosts.includes('Server4')){
             statusMessageCopy += 'SWITCH NOT CONNECTED TO REQUIRED SERVER2';
           }
        }

        statusMessageCopy += 'SUCCESS! LEVEL COMPLETE';
        this.isPassFailVisible = true;
        this.isBoardVisible = false;
        document.getElementById('resultsStr').innerHTML = statusMessageCopy;
        this.buttondisabled = false;

        console.log(statusMessageCopy)
      }

      }
    }
}
</script>

<style scoped>
.left{
  position: absolute;
  left: 30%;
  top: 50%;
}
.left-lvl3{
  position: absolute;
  left: 30%;
  top: 40%;
}
.left-center {
  position: absolute;
  left: 40%;
  top: 50%;
}
.left-low {
  position: absolute;
  left: 30%;
  top: 60%;
}
.center{
  position: absolute;
  left: 50%;
  top: 50%;
}
.center-lvl3{
  position: absolute;
  left: 50%;
  top: 40%;
}
.center-low{
  position: absolute;
  left: 50%;
  top: 60%;
}
.right-center{
  position: absolute;
  left: 60%;
  top: 50%;
}
.right{
  position: absolute;
  left: 70%;
  top: 50%;
}
.right-lvl3{
  position: absolute;
  left: 70%;
  top: 40%;
}
.right-low{
  position: absolute;
  left: 70%;
  top: 70%;
}
.server1{
  position: absolute;
  left: 85%;
  top: 40%;
  width: 100px;
}
.server2{
  position: absolute;
  left: 85%;
  top: 60%;
  width: 100px;
}
.server3{
  position: absolute;
  left: 85%;
  top: 80%;
  width: 100px;
}
.server1-lvl3{
  position: absolute;
  left: 85%;
  top: 30%;
  width: 100px;
}
.server2-lvl3{
  position: absolute;
  left: 85%;
  top: 45%;
  width: 100px;
}
.server3-lvl3{
  position: absolute;
  left: 85%;
  top: 60%;
  width: 100px;
}
.server4-lvl3{
  position: absolute;
  left: 85%;
  top: 75%;
  width: 100px;
}
.leftInternet{
  position: absolute;
  left: 10%;
  top: 50%;
  width: 200px;
  height: 50px;
}
.hint{
  font-size: small;
}
.instructions{
  font-size: small;
}
</style>
<template>
<h1>Level 1: Walls of Fire</h1>
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
    <select name="position" id="position">
      <option value="1">1</option>
      <option value="2">2</option>
      <option value="3">3</option>
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


<netComp class="left" positionStr="1"/>
<netComp class="center" positionStr="2"/>
<netComp class="right" positionStr="3"/>
<netComp class="leftInternet" positionStr="Internet"/>
<netComp class="server1" positionStr="Server1"/>
<netComp class="server2" positionStr="Server2"/>
<netComp class="server3" positionStr="Server3"/>
<!-- <connectingLine class=""/> -->
</template>

<script>
import modal from './NetworkComponentModal.vue';
import netComp from './NetworkComponent.vue';
import {componentArray} from '../store';

// import connectingLine from './ConnectingLine.vue';

export default {
  name: 'LevelOne',
  components: { 
    modal,
    netComp,
    // connectingLine
  },
  data() {
      return {
        isModalVisible: false,
      };
    },
    methods: {
      showModal() {
        this.isModalVisible = true;
      },
      closeModal() {
        this.isModalVisible = false;
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
           componentToStore = {componentType: componentText, componentPosition: positionText, allowedPorts: configArr};
        }
        else if(componentText === 'Switch'){
           console.log('switch')
           componentToStore = {componentType: componentText, componentPosition: positionText, connectedHosts: configArr};
        }
        else if(componentText === 'Router'){
           console.log('router')
           componentToStore = {componentType: componentText, componentPosition: positionText, connectedSwitches: configArr};
        }
        
        let componentArrayIndex = 0;

        if(componentArray.length > 0){
          console.log('component array is not empty, going to replace object with new one')
          for (let i = 0; i < componentArray.length; i++) {
             console.log('checking component position')
             console.log(componentArray[i].componentPosition)

             if(componentArray[i].componentPosition == positionText) {
                console.log('this is the position');
                componentArrayIndex = i;
                componentArray.splice(componentArrayIndex, 1)
             }
          }
        }

        componentArray.push(componentToStore);

        console.log(componentArray)

        console.log('successfully created network component')

        this.isModalVisible = false;
      },
      evaluateLevel(){

        console.log('EVALUATING LEVEL')

        if(componentArray.length !== 3){
          console.log('NOT ENOUGH NETWORK COMPONENTS')
          return 1;
        }

        if(componentArray[0].componentType != 'Router') {
          console.log('ONLY ROUTERS CAN CONNECT TO THE INTERNET')
          return 1;
        }

        if(componentArray[0].componentType == 'Router') {
          if(componentArray[0].connectedSwitches != 2) {
            console.log('ROUTER IS NOT CONNECTED TO SWITCH')
            return 1;
          }
        }

        if(componentArray[1].componentType != 'Firewall') {
          console.log('BEST TO HAVE A FIREWALL TO PROTECT THE INTERNAL NETWORK')
        }

        if(componentArray[1].componentType == 'Firewall') {
           if(componentArray[1].allowedPorts){
              if(!componentArray[1].allowedPorts.includes('8443')){
                console.log('FIREWALL IS NOT PERMITTING TRAFFIC ON REQUIRED PORT: 8443')
                return 1;
              }

              if(!componentArray[1].allowedPorts.includes('443')){
                console.log('FIREWALL IS NOT PERMITTING TRAFFIC ON REQUIRED PORT: 443')
                return 1;
              }
           }
        }

        if(componentArray[1].componentType == 'Switch' || componentArray[1].componentType == 'Router'){
          console.log('REDUNDANT SWITCH/ROUTER')
        }

        if(componentArray[2].componentType != 'Switch'){
          console.log('MUST HAVE A SWITCH TO CONNECT SERVERS')
          return 1;
        }

        if(componentArray[2].componentType == 'Switch'){
           if(!componentArray[2].connectedHosts.includes('Server1')){
             console.log('SWITCH NOT CONNECTED TO REQUIRED SERVER1')
           }
           if(!componentArray[2].connectedHosts.includes('Server2')){
             console.log('SWITCH NOT CONNECTED TO REQUIRED SERVER2')
           }
           if(!componentArray[2].connectedHosts.includes('Server3')){
             console.log('SWITCH NOT CONNECTED TO REQUIRED SERVER3')
           }
        }

        console.log('SUCCESS! LEVEL COMPLETE')

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
.center{
  position: absolute;
  left: 50%;
  top: 50%;
}
.right{
  position: absolute;
  left: 70%;
  top: 50%;
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
.leftInternet{
  position: absolute;
  left: 10%;
  top: 50%;
  width: 200px;
  height: 50px;
}
</style>
<template>
<h1>Level 1: Walls of Fire</h1>
  <button @click="showModal">add component</button>
  <button>configure component</button>
  <button>submit</button>
  <modal
  v-show="isModalVisible"
  @close="closeModal"
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
  <template v-slot:footer>
    This is a new modal footer.
  </template>
</modal>
<netComp class="left" positionStr="1"/>
<netComp class="center" positionStr="2"/>
<netComp class="right" positionStr="3"/>
<netComp class="leftInternet" positionStr="Internet"/>
<netComp class="server1" positionStr="Server"/>
<netComp class="server2" positionStr="Server"/>
<netComp class="server3" positionStr="Server"/>
<!-- <connectingLine class=""/> -->
</template>

<script>
import modal from './NetworkComponentModal.vue';
import netComp from './NetworkComponent.vue';
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
<template>
  <div id="app">
    <div class="header">
      <div class="traffic-lights">
        <div class="light red"></div>
        <div class="light yellow"></div>
        <div class="light green"></div>
      </div>
      <span>Reg Studio</span>
    </div>
    <Chat></Chat>
  </div>
</template>

<script>
import Chat from './components/Child.vue'

export default {
  components: {
    Chat
  },
  data(){
    return{
      
    }
  }
}
</script>

<style>
#app{
  max-width: 600px; 
}

.header {
  display: flex;
  justify-content: center; /* 将内容居中 */
  padding: 12px 16px;
  background-color: black;
  color: white;
  position : relative; /* 为子元素的定位做准备 */
}

.traffic-lights {
  position: absolute; /* 使用绝对定位 */
  left: 12px;
  top: 50%;
  transform: translateY(-50%);
  display: flex;
}


.light {
  width: 12px;
  height: 12px;
  margin-right: 4px;
  border-radius: 50%;
}

.red { background-color: red; }
.yellow { background-color: yellow; }
.green { background-color: green; }
</style>
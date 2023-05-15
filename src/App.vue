<template>
  <div class="container">
    <div id="topbar">云南乡村大调查</div>
    <div id="main">
      <MyMap></MyMap>
    </div>
    <div id="rightbar"></div>
    <div id="footer"></div>
  </div>
</template>

<script>
import MyMap from './components/MyMap.vue'
export default {
  components: {
    MyMap
  }
}
</script>

<style>
html,
body {
  height: 100%;
  margin: 0;
  background-image: url('../static/images/bg.png');
}

#app {
  height: 100%;
}

#topbar {
  grid-area: header;
  background-image: url('../static/images/head_bg.png');
  background-repeat: no-repeat;
}

#topbar {
  padding-top: 1rem;
  text-align: center;
  color: #bde4ff;
  font-size: 3rem;
  font-weight: bold;
  line-height: 5rem;
  letter-spacing: 0.5rem;
}

#main {
  position: relative;
  padding: 0 0 0 5%;
  grid-area: main;
}
#rightbar {
  grid-area: right;
}
#footer {
  grid-area: footer;
}

.container {
  display: grid;
  box-sizing: border-box;
  grid:
    'header header header header header header'
    'main main main main right right'
    'main main main main right right'
    'main main main main right right'
    'main main main main right right'
    'main main main main right right'
    'main main main main right right'
    'footer footer footer footer right right';
  grid-gap: 10px;
  height: 100%;
}
</style>

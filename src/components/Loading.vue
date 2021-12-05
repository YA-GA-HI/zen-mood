<template>
<div class="loading">

    <div class="song-loading" v-for="loading in this.songsLoading" :key="loading" >
        <div class="loader"></div>
        Loading {{ loading }}
    </div>

</div>
</template>

<script>
export default {
  props:['loadings'],
  data()
  {
    return{
      songsLoading :  this.loadings
    }
  },
  updated()
  {
    this.songsLoading = this.loadings;
  }
}
</script>

<style scoped>
.loading{
    position: fixed;
    z-index: 100;
    padding:10px 15px;
    bottom: 0px;
}

.song-loading{
    background-color: aqua;
    padding:5px 10px;
    border-radius:10px;
    margin:20px 0;
    max-width: 250px;
    display: flex;
    align-items: center;
}
.loader,
.loader:after {
  border-radius: 50%;
  width: 20px;
  height: 20px;
}
.loader {
  margin: 10px 10px;
  font-size: 10px;
  position: relative;
  text-indent: -9999em;
  border-top: 4px solid rgba(228, 225, 225, 0.2);
  border-right: 4px solid rgba(228, 225, 225, 0.2);
  border-bottom: 4px solid rgba(228, 225, 225, 0.2);
  border-left: 4px solid #ffffff;
  -webkit-transform: translateZ(0);
  -ms-transform: translateZ(0);
  transform: translateZ(0);
  -webkit-animation: load8 1.1s infinite linear;
  animation: load8 1.1s infinite linear;
}
@-webkit-keyframes load8 {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}
@keyframes load8 {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}

</style>
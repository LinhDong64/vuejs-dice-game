<template>
  <div class="wrapper-popup" :class="getClassPopup">
    <div class="rule">
      <h3>Luật chơi</h3>
      <p>
        Lorem ipsum dolor sit amet consectetur adipisicing elit. Ea nemo
        deleniti nulla porro quis! Optio quibusdam laboriosam cum? Ducimus nulla
        sapiente dolorem reiciendis delectus necessitatibus, voluptate
        distinctio sint iusto ratione.
      </p>
      <button 
      @click="confirm"
      class="confirm">Đã hiểu</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "popup-rule",
  props:{
    isOpenPopup:{type: Boolean, defaul: false}
  },
  data() {
    return {};
  },
  computed:{
    getClassPopup(){
      return{
        'open-popup': this.isOpenPopup
      }
    }
  },
  methods:{
    confirm(){
      this.$emit("handleConfirm");
    }
  }
};
</script>

<style>
.wrapper-popup {
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.4);
  opacity: 0;
  visibility: hidden;
}
.wrapper-popup.open-popup {
  opacity: 1;
  visibility: visible;
  transition: all 0.3s ease;
}
.rule {
  position: absolute;
  top: 50%;
  left: 50%;
  background-color: #fff;
  width: 350px;
  padding: 30px;
  font-family: Arial, Helvetica, sans-serif;
  transform: translate(-50%, -50%) scale(1.2);
  transition: all 0.3s ease;
}

.open-popup .rule {
  transform: translate(-50%, -50%) scale(1);
}

.rule h3 {
  margin-bottom: 10px;
}

.rule .confirm {
  padding: 8px 15px;
  border: 1px solid #333;
  background-color: #fff;
  margin-top: 20px;
  transition: all 0.3s ease;
}

.rule .confirm:hover {
  cursor: pointer;
  background-color: #333;
  color: #fff;
}
</style>
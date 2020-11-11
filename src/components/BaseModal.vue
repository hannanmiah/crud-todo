<template>
  <div class="modal">
    <div class="content">
      <div class="header"><slot name="header">Default Header</slot></div>
      <div class="body"><slot>Default content goes here</slot></div>
      <div class="footer">
        <slot name="footer">The Default Footer</slot>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  setup(props) {
    
  },
};
</script>

<style scoped>
.modal {
  position: fixed;
  top: 0;
  bottom: 0;
  z-index: 1;
  height: 100vh;
  width: 100vw;
  background: rgba(0, 0, 0, 0.5);
}

.modal .content {
  position: absolute;
  height: auto;
  width: 50vw;
  left: 20%;
  top: 20%;
  background: white;
  color: black;
}

.modal .content .header {
  padding: 5px;
  height: 20%;
  font-weight: bold;
  text-align: center;
  border-bottom: 1px solid #ddd;
}

.modal .content .body {
  padding: 10px;
  height: 60%;
}

.modal .content .footer {
  display: flex;
  justify-content: flex-end;
  height: 20%;
  border-top: 1px solid #ddd;
  padding: 10px;
}
</style>
<template>
  <div class="panel" :class="{ active }">
    <button class="panel-burger" @click="active = !active">
      <img src="@/assets/filter.png" alt="" />
    </button>

    <div class="panel-settings">
      <button class="panel-button panel-run" @click="$emit('run')">Run</button>
      <button class="panel-button panel-run-step" @click="$emit('runStep')">
        Run one step
      </button>
      <button class="panel-button panel-stop" @click="$emit('stop')">
        Stop
      </button>
      <button class="panel-button panel-clean" @click="$emit('clean')">
        Clean
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, defineEmits } from "vue";

defineEmits(["run", "runStep", "stop", "clean"]);

const active = ref(false);
</script>

<style scoped>
.panel {
  display: flex;
  flex-direction: column;
  max-width: 300px;
  max-height: 200px;
  background-color: rgba(204, 204, 204, 0.4);
  /* padding: 10px 5px; */
  border-radius: 5px;
  position: absolute;
  top: 10px;
  left: 10px;
  width: 50px;
  height: 50px;
  overflow: hidden;
  transition: all 0.3s ease 0s;
}
.panel.active {
  width: 100%;
  height: 100%;
  padding: 10px 5px;
}
.panel-burger {
  width: 50px;
  height: 50px;
  padding: 10px;
  background: transparent;
  border: 0;
  border-radius: 5px;
  margin-bottom: 10px;
}
.panel-burger img {
  width: 100%;
  height: 100%;
}
.panel-settings {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  row-gap: 3px;
}
.panel-button {
  border: none;
  background: #696969;
  color: #fff;
  padding: 3px 6px;
  font-size: 20px;
  border-radius: 5px;
}
.panel-input {
  margin-top: 10px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}
.panel-input-main {
  display: flex;
  column-gap: 10px;
  margin-top: 3px;
  align-items: center;
}
.panel-apply {
  background-color: rgb(224, 224, 224);
  padding: 5px 10px;
  border: none;
}

@media (any-hover: hover) {
  .panel-burger {
    cursor: pointer;
    transition: all 0.3s ease 0s;
  }
  .panel-burger:hover {
    background-color: rgba(255, 255, 255, 0.6);
  }

  .panel-button {
    cursor: pointer;
    transition: all 0.3s ease 0s;
  }
  .panel-button:hover {
    background-color: rgba(0, 0, 0, 0.7);
  }
  .panel-settings button {
    cursor: pointer;
  }

  .panel-apply {
    cursor: pointer;
    transition: all 0.3s ease 0s;
  }
  .panel-apply:hover {
    background-color: rgb(185, 185, 185);
  }
}
</style>
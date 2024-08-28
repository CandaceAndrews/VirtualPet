<template>
  <div class="pet-actions">
    <img
      src="@/assets/icons/food.png"
      alt="Food"
      class="draggable"
      id="food"
      @dragstart="onDragStart($event, 'feed')"
    />
    <img
      src="@/assets/icons/water.png"
      alt="Water"
      class="draggable"
      id="water"
      @dragstart="onDragStart($event, 'water')"
    />
    <img
      src="@/assets/icons/toy.png"
      alt="Toy"
      class="draggable"
      id="play"
      @dragstart="onDragStart($event, 'play')"
    />
  <img
      src="@/assets/icons/clean.png"
      alt="Brush"
      class="draggable"
      id="clean"
      @dragstart="onDragStart($event, 'clean')"
    />
  <button @click="toggleColorSelector">
    <img 
      src="@/assets/icons/color.png" 
      alt="Change Appearance"
      id="color"
      />
  </button>
  
  <!-- Color Selector (initially hidden) -->
  <div v-if="showColorSelector" class="color-selector">
    <button v-for="(color, index) in colors" :key="index" @click="changeColor(color)">
      <div :style="{ backgroundColor: color }" class="color-circle"></div>
    </button>
  </div>
  </div>
</template>

<script>
export default {
  name: 'PetActions',
  data() {
    return {
      showColorSelector: false,
      colors: ['#ff0000', '#00ff00', '#0000ff'], 
    };
  },
  methods: {
    onDragStart(event, action) {
      event.dataTransfer.setData('action', action);
    },
    toggleColorSelector() {
      this.showColorSelector = !this.showColorSelector;
    },
    changeColor(color) {
      this.selectedColor = color;
    },
  },
};
</script>

<style scoped>
.pet-actions {
  background-color: #31483c;
  padding: 15px;
  border-top: solid rgb(23, 35, 31) 3px;
}

#food {
  width: 60px;
  height: 60px;
  cursor: grab;
}

#water {
  width: 60px;
  height: 60px;
  cursor: grab;
  margin: 0px 35px 2px 35px;
}

#play {
  width: 50px;
  height: 50px;
  cursor: grab;
  margin: 0px 35px 2px 0px;
}

#clean {
  width: 50px;
  height: 50px;
  cursor: grab;
}

#color {
  width: 50px;
  height: 50px;
  cursor: grab;
}

.color-selector {
  display: flex;
  justify-content: center;
  margin-top: 10px;
  position: absolute; /* Adjust based on your layout */
  bottom: 60px; /* Adjust to fit under the icons */
  left: 50%;
  transform: translateX(-50%);
}


.color-circle {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  margin: 0 5px;
  cursor: pointer;
  border: 2px solid #fff;
}
</style>
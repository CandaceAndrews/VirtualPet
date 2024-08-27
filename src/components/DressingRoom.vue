<template>
  <div class="dressing-room">
    <img :src="selectedImage" alt="Virtual Pet" class="pet-image" />
    <div class="controls">
      <button @click="applyChanges">Apply Changes</button>
      <button @click="cancelChanges">Cancel</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DressingRoom',
  props: {
    petImages: Array,
    currentImage: String
  },
  data() {
    return {
      selectedImage: this.currentImage,
    };
  },
  methods: {
    applyChanges() {
      this.$emit('applyChanges', this.selectedImage);
    },
    cancelChanges() {
      this.$emit('cancelChanges');
    },
  },
};
</script>

<style scoped>
.dressing-room {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.pet-image {
  width: 300px;
  height: auto;
  margin-bottom: 20px;
}

.controls {
  display: flex;
  gap: 10px;
}
</style>
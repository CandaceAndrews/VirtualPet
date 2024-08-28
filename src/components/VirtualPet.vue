<template>
  <AppBackground :isDead="isDead">
    <div class="virtual-pet" @dragover.prevent @drop="onDrop">
      <img :src="petImage" :class="{ 'flipped': flipped }" alt="Virtual Pet" class="pet-image" />
      <NotificationAlert ref="notification" />

      <!-- Death message -->
       <div v-if="isDead" class="death-message">Your pet has died</div>

      <button v-if="isDead" @click="handleRestart" class="restart-button">Restart</button>
    
    <!-- Color Selector -->
     <div v-if="!isDead" class="color-selector">
      <span>Select Pet Color</span>
      <select v-model="selectedColor" @change="updatePetImage">
        <option value="Red">Red</option>
        <option value="yellow">Yellow</option>
      </select>
     </div>
    </div>
  </AppBackground>
</template>

<script>
import { mapGetters, mapActions } from 'vuex';
import NotificationAlert from './NotificationAlert.vue';
import AppBackground from './AppBackground.vue';
import { audioMixin } from '@/mixins/audioMixin.js';
import eventBus from '@/eventBus';

// Walk Images
import walk1Red from '@/assets/images/red/walkImages/walk1.png';
import walk2Red from '@/assets/images/red/walkImages/walk2.png';
import walk3Red from '@/assets/images/red/walkImages/walk3.png';
import walk4Red from '@/assets/images/red/walkImages/walk4.png';

import walk1Yellow from '@/assets/images/yellow/walkImages/walk1.png';
import walk2Yellow from '@/assets/images/yellow/walkImages/walk2.png';
import walk3Yellow from '@/assets/images/yellow/walkImages/walk3.png';
import walk4Yellow from '@/assets/images/yellow/walkImages/walk4.png';

// Eat Images
import eat1Red from '@/assets/images/red/eatImages/eat1.png';
import eat2Red  from '@/assets/images/red/eatImages/eat2.png';
import eat3Red  from '@/assets/images/red/eatImages/eat3.png';
import eat4Red  from '@/assets/images/red/eatImages/eat4.png';

import eat1Yellow from '@/assets/images/yellow/eatImages/eat1.png';
import eat2Yellow  from '@/assets/images/yellow/eatImages/eat2.png';
import eat3Yellow  from '@/assets/images/yellow/eatImages/eat3.png';
import eat4Yellow  from '@/assets/images/yellow/eatImages/eat4.png';

// Drink Images
import drink1Red  from '@/assets/images/red/drinkImages/drink1.png';
import drink2Red  from '@/assets/images/red/drinkImages/drink2.png';
import drink3Red  from '@/assets/images/red/drinkImages/drink3.png';
import drink4Red  from '@/assets/images/red/drinkImages/drink4.png';

import drink1Yellow from '@/assets/images/yellow/drinkImages/drink1.png';
import drink2Yellow from '@/assets/images/yellow/drinkImages/drink2.png';
import drink3Yellow from '@/assets/images/yellow/drinkImages/drink3.png';
import drink4Yellow from '@/assets/images/yellow/drinkImages/drink4.png';

// Play Images
import play1Red  from '@/assets/images/red/playImages/play1.png';
import play2Red  from '@/assets/images/red/playImages/play2.png';

import play1Yellow from '@/assets/images/yellow/playImages/play1.png';
import play2Yellow from '@/assets/images/yellow/playImages/play2.png';

// Clean Images
import clean1Red  from '@/assets/images/red/cleanImages/clean1.png';
import clean2Red  from '@/assets/images/red/cleanImages/clean2.png';
import clean3Red  from '@/assets/images/red/cleanImages/clean3.png';
import clean4Red  from '@/assets/images/red/cleanImages/clean4.png';
import clean5Red  from '@/assets/images/red/cleanImages/clean5.png';

import clean1Yellow from '@/assets/images/yellow/cleanImages/clean1.png';
import clean2Yellow from '@/assets/images/yellow/cleanImages/clean2.png';
import clean3Yellow from '@/assets/images/yellow/cleanImages/clean3.png';
import clean4Yellow from '@/assets/images/yellow/cleanImages/clean4.png';
import clean5Yellow from '@/assets/images/yellow/cleanImages/clean5.png';

// Sleep Images
import sleep1Red  from '@/assets/images/red/sleepImages/sleep1.png';
import sleep2Red  from '@/assets/images/red/sleepImages/sleep2.png';

import sleep1Yellow  from '@/assets/images/yellow/sleepImages/sleep1.png';
import sleep2Yellow  from '@/assets/images/yellow/sleepImages/sleep2.png';

// Death Images
import death1Red  from '@/assets/images/red/deathImages/death1.png';
import death2Red  from '@/assets/images/red/deathImages/death2.png';
import death3Red  from '@/assets/images/red/deathImages/death3.png';
import death4Red  from '@/assets/images/red/deathImages/death4.png';

import death1Yellow from '@/assets/images/yellow/deathImages/death1.png';
import death2Yellow from '@/assets/images/yellow/deathImages/death2.png';
import death3Yellow from '@/assets/images/yellow/deathImages/death3.png';
import death4Yellow from '@/assets/images/yellow/deathImages/death4.png';

// Sounds
import feedSound from '@/assets/sounds/feed.mp3';
import drinkSound from '@/assets/sounds/drink.wav';
import playSound from '@/assets/sounds/play.wav';
import cleanSound from '@/assets/sounds/clean.wav';

export default {
  name: 'VirtualPet',
  mixins: [audioMixin],
  components: {
    NotificationAlert,
    AppBackground,
  },
  computed: {
    ...mapGetters(['pet']),
    energyPercentage() {
      return this.pet.energy;
    },
    walkImages() {
      switch (this.selectedColor) {
        case 'yellow':
          return [walk1Yellow, walk2Yellow, walk3Yellow, walk4Yellow];
        default:
          return [walk1Red, walk2Red, walk3Red, walk4Red];
      }
    },
    eatImages() {
      switch (this.selectedColor) {
        case 'yellow':
          return [eat1Yellow, eat2Yellow, eat3Yellow, eat4Yellow];
        default:
          return [eat1Red, eat2Red, eat3Red, eat4Red];
      }
    },
    drinkImages() {
      switch (this.selectedColor) {
        case 'yellow':
          return [drink1Yellow, drink2Yellow, drink3Yellow, drink4Yellow];
        default:
          return [drink1Red, drink2Red, drink3Red, drink4Red];
      }
    },
    playImages() {
      switch (this.selectedColor) {
        case 'yellow':
          return [play1Yellow, play2Yellow];
        default:
          return [play1Red, play2Red];
      }
    },
    cleanImages() {
      switch (this.selectedColor) {
        case 'yellow':
          return [clean1Yellow, clean2Yellow, clean3Yellow, clean4Yellow, clean5Yellow];
        default:
          return [clean1Red, clean2Red, clean3Red, clean4Red, clean5Red];
      }
    },
    sleepImages() {
      switch (this.selectedColor) {
        case 'yellow':
          return [sleep1Yellow, sleep2Yellow];
        default:
          return [sleep1Red, sleep2Red];
      }
    },
    deathImages() {
      switch (this.selectedColor) {
        case 'yellow':
          return [death1Yellow, death2Yellow, death3Yellow, death4Yellow];
        default:
          return [death1Red, death2Red, death3Red, death4Red];
      }
    }
  },
  data() {
    return {
      selectedColor: 'red',
      petImage: walk1Red,
      currentImageIndex: 0,
      flipped: false,
      isEating: false,
      isDrinking: false,
      isPlaying: false,
      isCleaning: false,
      isSleeping: false,
      isDead: false,
      animationInterval: null,
      movementInterval: null,
      position: -900, // Added to keep track of position
      direction: 1, // Added to keep track of direction
      notificationMessage: '',
    };
  },
  watch: {
    'pet.energy'(newVal) {
      if (newVal === 0 && !this.isSleeping) {
        this.startSleeping();
        this.$refs.notification.showNotification('Your pet is sleeping!');
      } else if (newVal === 100 && this.isSleeping) {
        this.stopSleeping();
      }
    },
    'pet.hunger'(newVal) {
      if (newVal === 0) {
        this.$refs.notification.showNotification('Your pet is very hungry!');
      }
    },
    'pet.thirst'(newVal) {
      if (newVal === 0) {
        this.$refs.notification.showNotification('Your pet is very thirsty!');
      }
    },
    'pet.happiness'(newVal) {
      if (newVal === 0) {
        this.$refs.notification.showNotification('Your pet is very sad!');
      }
    },
    'pet.cleanliness'(newVal) {
      if (newVal === 0) {
        this.$refs.notification.showNotification('Your pet is very dirty!');
      }
    },
    'pet.life'(newVal) {
      if (newVal === 0 && !this.isDead) {
        this.handleDeath();
      }
    },
  },
  mounted() {
    this.startWalkingAnimation();
    eventBus.$on('feedPet', this.handleFeed); // Listen for 'feedPet' event
    eventBus.$on('waterPet', this.handleThirst);
    eventBus.$on('playWithPet', this.handlePlay); 
    eventBus.$on('cleanPet', this.handleClean); 
    this.startHungerTimer();
    this.startThirstTimer();
    this.startHappinessTimer();
    this.startCleanlinessTimer();
    this.startEnergyTimer();
    this.startLifeTimer();
  },
  beforeUnmount() {
    eventBus.$off('feedPet', this.handleFeed); // Remove event listener
    eventBus.$off('waterPet', this.handleThirst);
    eventBus.$off('playWithPet', this.handlePlay); 
    eventBus.$off('cleanPet', this.handleClean); 
    clearInterval(this.animationInterval);
    clearInterval(this.movementInterval);
    clearInterval(this.hungerInterval);
    clearInterval(this.thirstInterval);
    clearInterval(this.happinessInterval);
    clearInterval(this.cleanlinessInterval);
    clearInterval(this.energyInterval);
    clearInterval(this.lifeInterval);
  },
  methods: {
    ...mapActions([
      'feedPet', 
      'waterPet', 
      'playWithPet', 
      'cleanPet', 
      'increaseEnergy', 
      'decreaseEnergy', 
      'increaseLife', 
      'decreaseLife', 
      'resetPet'
    ]),

    startAnimation(images, duration, callback) {
      clearInterval(this.animationInterval);
      clearInterval(this.movementInterval);
      this.currentImageIndex = 0;
      this.animationInterval = setInterval(() => {
        this.currentImageIndex = (this.currentImageIndex + 1) % images.length;
        this.petImage = images[this.currentImageIndex];
      }, 230);
      setTimeout(() => {
        clearInterval(this.animationInterval);
        this.startWalkingAnimation();
        if (callback) callback();
      }, duration);
    },
    startWalkingAnimation() {
      clearInterval(this.animationInterval); // Ensure any previous interval is cleared
      this.animationInterval = setInterval(() => {
        this.currentImageIndex = (this.currentImageIndex + 1) % this.walkImages.length;
        this.petImage = this.walkImages[this.currentImageIndex];
      }, 230);
      this.movePet();
    },
    startSleeping() {
      if (this.isDead) return;
      this.isSleeping = true;
      clearInterval(this.animationInterval); // Stop other animations
      clearInterval(this.movementInterval); // Stop movement
      this.currentImageIndex = 0;
      this.animationInterval = setInterval(() => {
        this.currentImageIndex = (this.currentImageIndex + 1) % this.sleepImages.length;
        this.petImage = this.sleepImages[this.currentImageIndex];
      }, 230);
      this.energyInterval = setInterval(() => {
        this.increaseEnergy();
      }, 1500); // Refill every 1.5 seconds
    },
    stopSleeping() {
      if (this.isDead || !this.isSleeping) return;
      this.isSleeping = false;
      clearInterval(this.animationInterval); // Stop sleeping animation
      clearInterval(this.energyInterval); // Stop refilling energy
      this.startWalkingAnimation();
    },
    movePet() {
      const speed = 5;
      const petElement = this.$el.querySelector('.pet-image');
      clearInterval(this.movementInterval);
      this.movementInterval = setInterval(() => {
        if (!this.isEating && !this.isPlaying && !this.isCleaning && !this.isSleeping && !this.isDead) { // Ensure pet doesn't move during these
          if (this.direction === 1 && this.position >= window.innerWidth) {
            this.direction = -1;
            this.flipped = true;
          } else if (this.direction === -1 && this.position <= -900) { // Updated with this.position and this.direction
            this.direction = 1;
            this.flipped = false;
          }
          this.position += speed * this.direction;
          petElement.style.left = `${this.position}px`;
        }
      }, 40);
    },

    updatePetImage() {
      this.petImage = this.walkImages[0];
    },


    // -- Timer Methods --
    startHungerTimer() {
      this.hungerInterval = setInterval(() => {
        this.decreaseHunger();
      }, 2000); // Decrease every 2 seconds
    },
    startThirstTimer() {
      this.thirstInterval = setInterval(() => {
        this.decreaseThirst();
      }, 2000);
    },
    startHappinessTimer() {
      this.happinessInterval = setInterval(() => {
        this.decreaseHappiness();
      }, 2000);
    },
    startCleanlinessTimer() {
      this.cleanlinessInterval = setInterval(() => {
        this.decreaseCleanliness();
      }, 1000); 
    },
    startEnergyTimer() {
      this.energyInterval = setInterval(() => {
        this.decreaseEnergy();
      }, 1500);
    },
    startLifeTimer() {
      this.lifeInterval = setInterval(() => {
        if (this.pet.hunger === 0 && this.pet.thirst === 0 && this.pet.happiness === 0 && this.pet.cleanliness === 0) {
          this.decreaseLife();
        } else if (this.pet.hunger !== 0 && this.pet.thirst !== 0 && this.pet.happiness !== 0 && this.pet.cleanliness !== 0) {
          this.increaseLife();
        }
      }, 1000); // Check every 1 second
    },
    stopLifeTimer() {
      clearInterval(this.lifeInterval);
    },

    // -- Decrease - Increase Methods --
    decreaseHunger() {
      this.$store.dispatch('decreaseHunger');
    },
    decreaseThirst() {
      this.$store.dispatch('decreaseThirst');
    },
    decreaseHappiness() {
      this.$store.dispatch('decreaseHappiness');
    },
    decreaseCleanliness() {
      this.$store.dispatch('decreaseCleanliness');
    },
    decreaseEnergy() {
      if (this.isDead) return;
      this.$store.dispatch('decreaseEnergy');
    },
    increaseEnergy() {
      if (this.isDead) return;
      this.$store.dispatch('increaseEnergy');
    },

    // -- Handle Methods --
    handleFeed() {
      if (this.isLowEnergy()) return;
      if (this.isSleeping) {
        this.stopSleeping();
      }
      if (!this.isDead) {
        this.playAudio(feedSound);
        this.feedPet();
        this.startAnimation(this.eatImages, 3000);
      }
    },
    handleThirst() {
      if (this.isLowEnergy()) return;
      if (this.isSleeping) {
        this.stopSleeping();
      }
      if (!this.isDead) {
        this.playAudio(drinkSound);
        this.waterPet();
        this.startAnimation(this.drinkImages, 3000);
      }
    },
    handlePlay() {
      if (this.isLowEnergy()) return;
      if (this.isSleeping) {
        this.stopSleeping();
      }
      if (!this.isDead) {
        this.playAudio(playSound);
        this.playWithPet();
        this.startAnimation(this.playImages, 3000);
      }
    },
    handleClean() {
      if (this.isLowEnergy()) return;
      if (this.isSleeping) {
        this.stopSleeping();
      }
      if (!this.isDead) {
        this.playAudio(cleanSound); 
        this.cleanPet();
        this.startAnimation(this.cleanImages, 3000);
      }
    },
    isLowEnergy() {
      return this.energyPercentage <= 2;
    },

    handleDeath() {
      this.isDead = true;
      clearInterval(this.animationInterval);
      clearInterval(this.movementInterval);
      clearInterval(this.energyInterval);
      this.$refs.notification.showNotification('Your pet has died!');
      // Start continuous death animation
      this.animationInterval = setInterval(() => {
        this.currentImageIndex = (this.currentImageIndex + 1) % this.deathImages.length;
        this.petImage = this.deathImages[this.currentImageIndex];
      }, 230); // 230 milliseconds between each frame of the death animation
    },
    handleRestart() {
      clearInterval(this.animationInterval);
      this.resetPet();
      this.isDead = false;
      this.startWalkingAnimation();
      this.startHungerTimer();
      this.startThirstTimer();
      this.startHappinessTimer();
      this.startCleanlinessTimer();
      this.startEnergyTimer();
      this.startLifeTimer();
    },
    
    // -- Drop Methods --
    onDrop(event) {
      const action = event.dataTransfer.getData('action');
      if (action === 'feed') {
        this.handleFeed();
      } else if (action === 'water') {
        this.handleThirst();
      } else if (action === 'play') {
        this.handlePlay();
      } else if (action === 'clean') {
        this.handleClean();
      }
    },
  },
};
</script>

<style scoped>
.virtual-pet {
  position: absolute;
  top: 37%;
  left: 0;
  width: 100%;
  text-align: center;
}

.pet-image {
  width: 900px;
  height: auto;
  position: absolute;
  transition: left 0.05s linear;
}

.flipped {
  transform: scaleX(-1);
}

.restart-button {
  position: fixed;
  bottom: 50%; /* Adjusted to move it higher */
  left: 50%;
  transform: translate(-50%, 0); /* Center horizontally */
  padding: 10px 20px;
  font-size: 16px;
  background-color: #ff4a4a;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  z-index: 10;
}

.death-message {
  font-size: 48px;
  color: red;
  font-weight: bold;
  position: fixed; /* Use fixed for consistent centering */
  top: 30%; /* Adjusted to move it down slightly */
  left: 50%;
  transform: translate(-50%, -50%); /* Center horizontally and vertically */
  z-index: 20;
}

.restart-button:hover {
  background-color: #ff0000;
}

.color-selector {
  position: absolute;
  top: 10px;
  left: 10px;
  background-color: white;
  padding: 5px;
  border-radius: 5px;
}
</style>
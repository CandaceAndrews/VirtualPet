# Virtual-Pet
Virtual pet game using Vue.js

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
__
### Features So Far
- Pet stats decrease over time.
- Pet stats increase when you drag icons to pet.
- If all stats are empty life decreases, if all stats are full life increases.
- Energy stat decreases over time, when 0 the pet falls asleep.
- While sleeping energy increases.
- Pet can be woken up during sleep by dragging icon or when energy reaches 100.
- Disabled drag icons while energy is at 2% or lower to prevent errors.
- When life reaches 0 pet dies.
- Restart button for when pet dies to try again. (Will probably change this feature later)
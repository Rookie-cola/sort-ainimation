<template>
  <div class="btn-bar">
    <button @click="insert">插入</button>
    <button @click="remove">清空</button>
    <button @click="shuffle">打乱</button>
    <button @click="sort">排序</button>
  </div>
  <div class="flex-container">
    <TransitionGroup tag="ul" name="fade" class="container">
      <div :data-height="item.value" v-for="item in items" class="item" :key="item.id">
        <span>{{ item.value }}</span>
      </div>
    </TransitionGroup>
  </div>
</template>

<script>
import { shuffle as _shuffle } from 'lodash-es'
export default {
  data() {
    return {
      id: 1,
      items: []
    }
  },
  methods: {
    getRandomNumber(min, max) {
      let random = Math.random() * (max - min + 1) + min;
      let randomNumber = Math.floor(random);
      return randomNumber;
    },
    insert() {
      const i = Math.round(Math.random())
      this.items.push({ id: this.items.length, value: this.getRandomNumber(5,100) })
    },

    remove() {
      this.items = [];
    },

    shuffle() {
      this.items = _shuffle(this.items)
    },
    sort() {
      this.items.sort((a, b) => a.value - b.value);
      console.log(this.items)
    }
  },
  aftermounted: function () {
    this.items = getInitialItems()
  },
  watch: {
    items: {
      handler(oldVal, newVal) {
        this.$nextTick(() => {
          const spans = document.querySelectorAll('div[data-height]')
          const span = spans[spans.length - 1]
          const dataHeight = span.getAttribute('data-height');
          span.style.height = `${dataHeight * 5}px`
        });
      },
      deep: true
    }
  }
}
</script>

<style>
#app {
  display: block;
  margin: 0 auto;
  width: 100%;
  height: 100%;
  padding: 70px 0;
  text-align: center;
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

.btn-bar {
  position: fixed;
  top: 0;
  margin-top: 50px;
  height: 24px;
  left: 38%;
  z-index: 999;
}

@keyframes animate {

  0%,
  25% {
    text-shadow: 2px 2px 2px #ff6384;
  }

  50% {
    text-shadow: 2px -2px 2px #ff6384;
  }

  75% {
    text-shadow: -2px 2px 2px #ff6384;
  }

  100% {
    text-shadow: -2px -2px 2px #ff6384;
  }
}

.btn-bar button {
  font-family: STXihei, "华文细黑", "Microsoft YaHei", "微软雅黑",20px;
  margin-right: 30px;
  transition: transform 0.5s;
}

.btn-bar button:hover {
  transform: scale(1.2);
  animation: animate 0.5s linear infinite;
}

button {
  background-color: #82ccdd;
  border-radius: 10px;
  min-width: 100px;
  min-height: 50px;
  border: none;
  color: white;
  font-size: large;
  box-shadow: 3px 3px 7px rgba(0, 0, 0, 0.3);
}

.flex-container {
  display: flex;
  left: 700px;
  top: 0px;
}


.container {
  left: auto;
  width: 100%;
  padding: 70px 0;
  text-align: center;
}

.item {
  display: inline-grid;
  width: 100px;
  height: 30px;
  background-color: #fbc251;
  box-sizing: border-box;
  vertical-align: bottom;
  margin: 10px;
  transition: transform 0.5s, background-color 0.5s;
  border-radius: 10px;
  color: white;
  font-size: 17px;
  box-shadow: 3px 3px 7px rgba(0, 0, 0, 0.3);
}

.item span {
  margin: auto 0 0 0;
}

.item:hover {
  background-color: #e55039;
  transform: scale(1.2);
}

.btn-close {
  margin-left: auto;
}

/*声明过渡效果*/
.fade-move,
.fade-enter-active,
.fade-leave-active {
  transition: all 0.5s cubic-bezier(0.55, 0, 0.1, 1);
}

/*声明进入和离开的状态 */
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: scaleY(0.01) translate(30px, 0);
}

/*确保离开的项目被移除出了布局流,以便正确地计算移动时的动画效果。*/
.fade-leave-active {
  position: absolute;
}
</style>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
    flex: 1;
  }
}
</style>

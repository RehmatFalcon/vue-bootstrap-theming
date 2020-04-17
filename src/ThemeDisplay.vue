<template>
  <div :class="classesToApply">
    <button class="btn btn-info" @click="toggleTheme" style="margin-top: 10px;">
      <transition name="replace" mode="out-in">
        <span class="far fa-sun fa-fw fa-2x" v-if="isLightTheme" key="sun"></span>
        <span class="far fa-moon fa-fw fa-2x" v-else key="moon"></span>
      </transition>
    </button>
    <div id="nav">
      <router-link to="/" class="btn btn-primary">Home</router-link> |
      <router-link to="/about" class="btn btn-warning">About</router-link>
    </div>
    <transition name="slide-fade" mode="out-in">       
      <router-view />
    </transition>
  </div>
</template>

<script>
import "./styles.scss";
const themes = {
  LIGHT : "theme-light",
  DARK : "theme-dark"
};
export default {
  data () {
    return {
      theme: themes.LIGHT,
      transitionalPeriod : false
    }
  },
  computed: {
    classesToApply : function() {
      return [
        "theme-display",
        this.theme,
        {"temp-transitional" : this.transitionalPeriod}
      ];
    },
    isLightTheme : function() {
      return this.theme === themes.LIGHT;
    }
  },
  methods: {
    toggleTheme() {
      this.transitionalPeriod = true;
      let removeTheme = this.theme;
      let addTheme;
      if(this.theme === themes.LIGHT) {
        addTheme = themes.DARK;
      }
      else {
        addTheme = themes.LIGHT;
      }
      this.theme = addTheme;
      this.$el.classList.remove(removeTheme);
      this.$el.classList.add(addTheme);
      setTimeout(function() {
        this.transitionalPeriod = false;
      }, 1000);
    }
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Roboto&display=swap');
@import "style_variables";
/* Enter and leave animations can use different */
/* durations and timing functions.              */
.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .3s cubic-bezier(1.0, 0.5, 0.8, 1.0);
} 
.slide-fade-enter {
  transform : translate(-10%);
}
.slide-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateX(10%);
  opacity: 0;
}
.theme-display {
  font-family: 'Roboto', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100%;
  width: 100%;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
  }
}

.temp-transitional {
  &, & *, & *:before, & *:after {
    transition: all .3s;
  }
}
.replace {
  &-enter {
    transform: translateX(30%);
    opacity: 0;
  }
  &-leave-to {
    transform: translateX(-30%);
    opacity: 0;
  }
  &-enter-active, &-leave-active {
    transition: .3s;
  }
}
.theme-dark {
  .btn-info, .btn-info:focus, .btn-info {
    background-color: #363738;
    border: none;
    box-shadow: $darkBoxShadow;
  }
}
</style>

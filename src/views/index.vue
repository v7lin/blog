<template>
  <div>
    <mu-appbar :color="bgColor" style="width: 100%;">
      <mu-button @click="toggleMenu" icon slot="left">
        <mu-icon size="20" value=":fa fa-bars fa-5x"></mu-icon>
      </mu-button>
      <mu-button :href="githubUrl" flat slot="right">
        <mu-icon size="24" value=":fa fa-github fa-5x"></mu-icon>
      </mu-button>
    </mu-appbar>
    <div class="bg">
      <img alt class="logo" src="../assets/img/logo.png" />
      <div class="bg-desc">
        <h1>halapro.liu Blog</h1>
        <h2>A simple Blog used by halapro.liu</h2>
      </div>
      <mu-button @click="onMenuChange" class="post-btn" color="#fff" size="15" textColor="#00bcd4" to="/page">Go for Posts</mu-button>
    </div>

    <div class="content">
      <div class="mu-flex">
        <div class="module">
          <h3>About</h3>
          <router-link to="/">
            <img alt src="../assets/img/get-started.svg" />
          </router-link>
        </div>
        <div class="module">
          <h3>Posts</h3>
          <router-link to="/page">
            <img alt src="../assets/img/css-framework.svg" />
          </router-link>
        </div>
        <div class="module">
          <h3>Pages</h3>
          <router-link to>
            <img alt src="../assets/img/components.svg" />
          </router-link>
        </div>
      </div>
    </div>

    <div class="footer">
      <p>
        The blog was created by
        <a :href="githubUrl" class="key">@halapro.liu</a>
      </p>
      <p>
        <a :href="vueUrl" class="key">Vuejs</a> &
        <a :href="museuiUrl" class="key">MuseUI</a>
      </p>
      <i class="fa fa-github fa-2x"></i>
    </div>
  </div>
</template>

<script>
import {userData} from '../constant'
export default {
  data () {
    return {
      bgColor: '#00bed4',
      githubUrl: userData.githubUrl,
      museuiUrl: userData.museuiUrl,
      vueUrl: userData.vueUrl
    }
  },
  methods: {
    toggleMenu () {
      this.$emit('toggleMenu')
    },
    onMenuChange () {
      this.$emit('onMenuChange', '/page')
    }
  }
}
</script>

<style lang="scss" scoped>
.bg {
  box-sizing: border-box;
  background: #00bed4;
  padding: 72px 24px;
}

.bg-desc {
  margin: 32px auto 0px;
  text-align: center;
  max-width: 575px;
  h1 {
    color: rgba(255, 255, 255, 0.87);
    font-size: 36px;
    font-weight: 300;
    line-height: 24px;
  }

  h2 {
    color: rgba(255, 255, 255, 0.87);
    font-weight: 300;
    font-size: 24px;
    line-height: 32px;
    padding-top: 16px;
    margin-bottom: 12px;
    letter-spacing: 0px;
  }
}

.logo {
  width: 150px;
  height: 150px;
}

.content {
  box-sizing: border-box;
  padding: 72px 24px;
  max-width: 906px;
  margin: 0 auto;
}

.mu-flex {
  display: flex;
  justify-content: center;
  align-items: center;
  .module {
    width: 30%;
    &:nth-child(n + 2) {
      margin-left: 4px;
    }
    h3 {
      font-size: 20px;
      padding-top: 19px;
      margin-bottom: 13px;
      letter-spacing: 0;
      font-weight: 500;
      color: rgba(0, 0, 0, 0.87);
      background-color: #eeeeee;
      text-align: center;
      margin: 0;
      padding: 0;
      line-height: 64px;
    }
    img {
    }
  }
}

.post-btn {
  font-family: Roboto, sans-serif;
  border-radius: 2px;
  margin: 16px 32px 0px 32px;
}

.footer {
  padding: 72px 24px;
  box-sizing: border-box;
  background-color: rgb(33, 33, 33);
  text-align: center;
  p {
    margin: 0 auto;
    padding: 0;
    color: rgba(255, 255, 255, 0.54);
    max-width: 335px;
  }

  .key {
    color: rgba(255, 255, 255, 0.87);
  }

  i {
    color: #fff;
  }
}
</style>

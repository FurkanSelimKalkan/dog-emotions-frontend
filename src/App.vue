<template>
  <header>
    <div class="wrapper">
      <nav>
        <div class="nav-connected">
          <a href="/">
            <img class="logo" src="./assets/happy_sad_angry-removebg.png" alt="logo">
          </a>
          <RouterLink class="link" to="/">Home</RouterLink>
          <RouterLink class="link" to="/emotionizer">Emotionizer</RouterLink>
        </div>
        <p class="welcome-text">Welcome to Emotionizer!</p>
        <a class="link" href="https://github.com/FurkanSelimKalkan/dog-emotions-frontend" target="_blank">GitHub</a>
      </nav>
    </div>
  </header>
  <RouterView />
</template>

<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<style scoped>
.wrapper {
  max-width: 390px;
  margin: 0 auto;
}

nav {
  display: grid;
  grid-template-columns: auto 1fr auto;
  grid-template-rows: 1fr;
  background-color: #FFA500;
  align-items: center;
  justify-content: center;
  padding: 5px;
}

.nav-connected {
  display: flex;
  align-items: center;
  justify-content: center;
}

.link {
  text-decoration: none;
  font-size: 16px;
  color: #333333;
  transition: 0.4s;
  padding: 3px;
  box-sizing: border-box;
}

.logo {
  margin-right: 5px;
  max-width: 65px;
  border: 2px solid #333333;
  border-radius: 50%;
  padding: 3px;
}

.link:hover,
.link:active,
.link:focus {
  color: #FFA500;
  background-color: #333333;
  border-radius: 5px;
  padding: 3px;
}

.welcome-text {
  font-size: 20px;
  font-family: Pacifico;
  font-weight: 300;
  text-align: center;
  margin: 10px 0;
}

.welcome-text:hover {
  cursor: default;
}

</style>

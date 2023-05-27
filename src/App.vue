<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

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
                <a class="link" href="https://github.com" target="_blank">GitHub</a>
            </nav>
        </div>
    </header>
    <RouterView />
</template>
<style scoped>
nav {
    display: grid;
    grid-template-columns: auto 1fr auto;
    grid-template-rows: 1fr;
    background-color: #FFA500;
    align-items: center;
    justify-content: center;
}

.nav-connected {
    display: flex;
    align-items: center;
    justify-content: center;
}

.link {
    text-decoration: none;
    font-size: 24px;
    color: #333333 ;
    transition: 0.4s;
    padding: 5px;
    box-sizing: border-box;
}

 .logo {
    margin-right: 5px;
}

.link:hover, .link:active, .link:focus {
    color: #FFA500;
    background-color: #333333;
    border-radius: 5px;
    padding: 5px;
}

.logo {
    max-width: 75px;
}

.welcome-text {
    font-size: 26px;
    font-family: Pacifico;
    font-weight: 300;
    text-align: center;
}

.welcome-text:hover {
    cursor: default;
}

</style>

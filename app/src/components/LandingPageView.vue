<style scoped>
img {
    margin-top: -25px;
    width: 450px;
}
</style>

<template>
<div>
    <img src="./LandingPageView/assets/logo.png" alt="electron-vue">
    <h1>Welcome.</h1>
    <versions></versions>
    <links></links>
    <player></player>
</div>
</template>

<script>
import Links from './LandingPageView/Links';
import Versions from './LandingPageView/Versions';
import Player from './LandingPageView/Player';
export default {
    components: {
        Links,
        Player,
        Versions
    },
    name: 'landing-page'
}
</script>

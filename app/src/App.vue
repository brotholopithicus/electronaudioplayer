<style>
@import url('https://fonts.googleapis.com/css?family=Kaushan+Script|Lato');
* {
    box-sizing: border-box;
}

body {
background: radial-gradient(center, rgb(241, 197, 151) 0%, rgb(224, 150, 7) 100%);
    font-size: 16px;
    font-family: Lato, Helvetica, sans-serif;
}
</style>

<template>
<div>
    <Player></Player>
</div>
</template>

<script>
import LandingPage from './components/LandingPageView';
import Player from './components/Player';

export default {
    components: {
        LandingPage,
        Player
    }
}
</script>

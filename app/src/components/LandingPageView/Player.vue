<style>

</style>
<template>
<div>
    <audio id="audio" controls="controls" autoplay>
      <source :src="fileSource" type="audio/mp3"/>
    </audio>
    <button @click="readLibrary()">Click</button>
    <ul id="playlist">
        <li v-for="(file, index) in playlist">
            <h4 :id="index" @click="updateSource(index)">{{file.name}}</h4>
        </li>
    </ul>
</div>
</template>
<script>
export default {
    data() {
        return {
            fileSource: 'tupac/Changes.mp3',
            playlist: []
        }
    },
    methods: {
        updateSource(i) {
            let audio = document.getElementById('audio');
            let newSource = this.playlist[i].path;
            this.fileSource = newSource;
            audio.load();
            audio.play();
        }
    },
    created() {
        this.$http.get('tupac').then(response => {
            response.body.forEach(file => {
                this.playlist.push({
                    name: file,
                    path: 'tupac/' + file
                });
            });
        });
    }
}
</script>

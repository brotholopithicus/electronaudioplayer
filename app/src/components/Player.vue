<template>
<div id="player-container">
    <h1 id="currently-playing">{{currentSource.name}} - <span id="current-artist">{{currentSource.artist}}</span></h1>
    <div id="controls">
        <div id="progress">
            <div id="progress-bar">
            </div>
        </div>
    </div>
    <div id="audio-container">
        <div id="audio-controls">
            <transition name="fade">
                <img v-if="shuffle" src="../assets/shuffle.svg" id="shuffle-icon" />
                <img v-else src="../assets/play.svg" id="play-icon" />
            </transition>
            <button id="shuffle" :class="[shuffle ? 'activeShuffle' : 'inactiveShuffle', 'btn btn-primary']" @click="shuffle = !shuffle" :title="shuffle ? 'Shuffle On' : 'Shuffle Off'">Toggle Shuffle</button>
            <button id="nextTrack" class="btn btn-primary" @click="onEnded">Skip</button>
        </div>
        <audio id="audio" controls="controls" autoplay>
      <source :src="currentSource.path" type="audio/mp3"/>
    </audio>
    </div>
    <ul class="playlist">
        <li v-for="(file, index) in playlist" :id="index" @click="updateSource(index)" :class="currentSource.track === index ? 'active' : ''">
            <span>{{file.name | trackNumber }}&nbsp;{{file.name | trackName }}</span>
        </li>
    </ul>
</div>
</template>
<script>
export default {
    data() {
        return {
            currentSource: {},
            playlist: [],
            audioElement: '',
            shuffle: false,

        }
    },
    methods: {
        updateSource(i) {
            let audio = document.getElementById('audio');
            let newSource = this.playlist[i];
            this.currentSource = newSource;
            this.fileSource = newSource.path;
            audio.load();
            audio.play();
        },
        onEnded() {
            let currentTrackNumber = this.currentSource.track;
            if (!this.shuffle) {
                let nextTrack = currentTrackNumber + 1;
                this.updateSource(nextTrack);
            } else {
                this.getRandomTrack();
            }
        },
        getRandomTrack() {
            let newTrackId = Math.floor(Math.random() * this.playlist.length) + 1;
            if (this.currentSource.track === newTrackId) {
                this.getRandomTrack();
            } else {
                this.updateSource(newTrackId);
            }
        }
    },
    created() {
        this.$http.get('Tupac').then(response => {
            response.body.forEach((file, index) => {
                this.playlist.push({
                    name: file.substr(0, file.length - 4),
                    artist: 'Tupac',
                    path: 'Tupac/' + file,
                    track: index
                });
            });
            this.updateSource(7);
        });
    },
    mounted() {
        let audio = document.getElementById('audio');
        audio.addEventListener('ended', this.onEnded);
    },
    filters: {
        trackNumber(name) {
            let number = parseInt(name.split('.')[0]);
            return number + '.';
        },
        trackName(name) {
            return name.split('.').pop();
        }
    }
}
</script>
<style scoped>
* {
    outline: none;
}

#player-container {
    text-align: center;
    user-select: none;
}

#currently-playing {
    font-size: 44px;
    color: #000;
    margin: 0.4em;
    font-family: 'Kaushan Script';
    text-shadow: 0px 2px 2px rgba(0, 0, 0, 0.5);
}

#current-artist {
    font-style: italic;
}
#audio-controls {
  padding-bottom: 2em;
  border-bottom: 2px solid #000;
}
audio#audio {
    display: block;
    margin: 0 auto;
    margin-top: 1.75em;
    height: 64px;
    padding: 0.8em;
    width: 50%;
    background: #fff;
    border-radius: 10px;
    border: 3px solid rgba(0, 0, 0, 0.74);
}

img {
    display: block;
    margin: 0 auto;
}

.btn {
    margin: 0 auto;
    display: inline-block;
    font-weight: normal;
    line-height: 1.25;
    text-align: center;
    white-space: nowrap;
    vertical-align: middle;
    cursor: pointer;
    user-select: none;
    border: 1px solid transparent;
    padding: 0.5rem 1rem;
    font-size: 1rem;
    border-radius: 0.25rem;
}

#play-icon,
#shuffle-icon {
    width: 50px;
    height: 50px;
    margin-bottom: 1em;
}

.fade-enter-active,
.fade-leave-active {
    transition: opacity .5s
}

.fade-enter,
.fade-leave-active {
    opacity: 0
}

.btn-primary {
    color: #fff;
    background-color: #0275d8;
    border-color: #0275d8;
}

.btn-primary:hover {
    color: #fff;
    background-color: #025aa5;
    border-color: #01549b;
}

.activeShuffle {
    background: #0c9909;
    border-color: #0c9909;
}

.activeShuffle:hover {
    background: #007D00;
    border-color: #005C00;
}

.inactiveShuffle {
    background: #be2b26;
    border-color: #be2b26;
}

.inactiveShuffle:hover {
    background: #9B1310;
    border-color: #720300;
}

#audio-container {
    width: 85%;
    margin: 0 auto;
    padding: 2em 0;
    border-top: 2px solid rgba(0, 0, 0, 0.8);
    border-bottom: 2px solid rgba(0, 0, 0, 0.8);
}

ul.playlist {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    padding: 0 5%;
    justify-content: space-between;
    list-style-type: none;
}

ul.playlist li {
    box-shadow: 2px 2px 2px rgba(0, 0, 0, 0.5);
    width: 18%;
    background: #d13a3a;
    color: #fff;
    font-size: 12px;
    padding: 1em;
    margin: 0.4em;
    border-color: #d13a3a;
    font-weight: 900;
    transition: 0.7s;
    text-shadow: 0px 1px 2px black;
    line-height: 1.25;
    text-align: center;
    white-space: nowrap;
    vertical-align: middle;
    cursor: pointer;
    user-select: none;
    border: 1px solid transparent;
    border-radius: 0.25rem;
}

ul.playlist li.active {
    color: #fff;
    background-color: #0275d8;
    border-color: #0275d8;
}

ul.playlist li.active:hover {
    color: #fff;
    background-color: #025aa5;
    border-color: #01549b;
}

ul.playlist li:hover {
    background: #00873B;
    border-color: #00B54F;
    animation: 0.2s wobble ease-in-out infinite alternate;
}

@keyframes wobble {
    0% {
        transform: skewY(0.4deg);
    }
    100% {
        transform: skewY(-0.4deg);
    }
}
</style>

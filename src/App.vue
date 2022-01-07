<template>
  <div>
    <header>
      <h1>My Music</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">{{ current.title }}</h2>
        <div class="controls">
        <button class="pre" @click="prev">Pre</button>
        <button class="play" v-if="!isPlaying" @click="play">Play</button>
        <button class="play" v-else @click="pause()">Pause</button>
        <button class="next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>The Playlist</h3>
        <button v-for="song in songs" :key="song.title" @click="play(song)" :class="(song.src == current.src ) ? 'song playing' : 'song' "> 
          {{ song.title }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>


export default {
  name: 'App',
  data(){
    return {
      current : {},
      index : 0,
      isPlaying : false,
      songs : [
        {
          title : "First Song",
          src : require('./assets/Baliye-Re-Jersey.mp3')
        },
        {
          title : "Second Song",
          src : require('./assets/Dance-Meri-Rani.mp3')
        },
        {
          title : "Third Song",
          src : require('./assets/Gaddi-Neevi.mp3')
        }
      ],
    player : new Audio()
    }
  },
  created(){
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
    
  },
  methods : {
    play( song ) {
      if(typeof song.src != "undefined"){
        this.current = song;
        this.player.src = this.current.src;
      }
        this.player.play();
        this.player.addEventListener('ended', function(){
          this.index++;
          if(this.index > this.songs.length - 1){
        this.index = 0;
      }

      this.current = this.songs[this.index];
      this.play( this.current );
        }.bind(this))
        this.isPlaying = true;
    },
    pause(){
      this.player.pause();
      this.isPlaying = false;
    },
    next(){
      this.index++;
      if(this.index > this.songs.length - 1){
        this.index = 0;
      }

      this.current = this.songs[this.index];
      this.play( this.current );
    },
    prev(){
      this.index--;
      if(this.index < 0){
        this.index = this.songs.length - 1;
      }

      this.current = this.songs[this.index];
      this.play( this.current );
    }
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  font-family: sans-serif;
}
header{
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: #212121;
  color: #FFF;
}
main{
  width : 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}
.song-title{
  color :#212121;
  font-size: 700;
  font-weight: none;
  text-transform: uppercase;
  text-align: center;
} 
.controls{
  display: flex;
  justify-content: center;
  padding: 30px 15px;
  align-items: center;
}
button{
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}
button:hover {
  opacity: 0.8;
}
.play{
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color : #fff;
  background-color: #CC2E5D;
}
.pre, .next {
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color : #fff;
  background-color: #FF5858;
}
.playlist{
  padding: 0px 30px;;
}
.playlist h3 {
  color: #212121;
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center;
}
.playlist .song{
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}
.playlist .song:hover{
  color: #FF5858;
}
.playlist .song.playing{
  color : #fff;
  background-image: linear-gradient(to right, #CC2E5D, #FF5858);
}
</style>

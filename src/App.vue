<template>

<div :class="{'app' : true , 'dark-app' : dark}">
<Navbar @mode="Changemode($event)"/>


<!--header-->
<header class="header">
  Zen Mood

  <div class="description">
    fell peace and relaxation when listening to those sounds. 
    <br>
    <span class="views">{{ views }}</span> Used it !
  </div>
</header>

<Share/>


<!--suggetions-->
<section id="suggetions">
  <button v-for="suggest in suggetions" :key="suggest" @click="Suggest(suggest.name)" class="suggetion-box">
    <svg v-html="suggest.icon" xmlns="http://www.w3.org/2000/svg"  width="50%" height="50%" viewBox="0 0 24 24" stroke-width="1.5" stroke="white" fill="none" stroke-linecap="round" stroke-linejoin="round">
    </svg>
    <div class="suggetion-text" >{{ suggest.name }}</div>
  </button>
</section>


<!--boxes-->
<main id="songs">
  <div v-for="song in songs" :key="song" :class="{'song-bg':true , 'song-bg-played': song.played }">
    <button class="song" @focus="play(song.name)" :ref="song.name" >
      <div class="song-name">{{ song.name }}</div>
      <svg v-html="song.icon" xmlns="http://www.w3.org/2000/svg"  width="50%" height="50%" viewBox="0 0 24 24" stroke-width="1.5" stroke="var(--icon)" fill="none" stroke-linecap="round" stroke-linejoin="round">
      </svg>

      <button v-if="song.played" class="volume-input">
          <input min="0" max="100" value="100" :ref="song.name+'Volume'" @input="changeVolume(song.name)" @change="changeVolume(song.name)" type="range">
      </button>

    </button>
  </div>


</main>

<Loading :loadings="this.loadings" />

<Footer/>

</div>

</template>

<script>
import Navbar from "./components/Navbar.vue"
import Footer from "./components/Footer.vue"
import Share from "./components/Share.vue"
import Loading from "./components/Loading.vue"
import axios from "axios"

export default {
  components:{
    Navbar , Footer , Share , Loading
  },

  data()
  {
    return{

      dark :false,
      views:0,
      loadings:[],
      suggetions:[

        {
          name:"Productivity",
          icon:`<path stroke="none" d="M0 0h24v24H0z" fill="none"/>
      <circle cx="6" cy="19" r="2" />
      <circle cx="17" cy="19" r="2" />
      <path d="M17 17h-11v-14h-2" />
      <path d="M6 5l14 1l-1 7h-13" />`
        },

        {
          name:"Relax",
          icon:`<path stroke="none" d="M0 0h24v24H0z" fill="none"/>
  <circle cx="4" cy="17" r="1" />
  <circle cx="9" cy="5" r="1" />
  <path d="M4 22l4 -2v-3h12" />
  <path d="M11 20h9" />
  <path d="M8 14l3 -2l1 -4c3 1 3 4 3 6" />`
        },

        {
          name:"Focus",
          icon:`<path stroke="none" d="M0 0h24v24H0z" fill="none"/>
  <circle cx="12" cy="12" r="1" />
  <circle cx="12" cy="12" r="5" />
  <circle cx="12" cy="12" r="9" />`
        },

        {
          name:"Sadness",
          icon:`<path stroke="none" d="M0 0h24v24H0z" fill="none"/>
  <circle cx="12" cy="12" r="9" />
  <line x1="9" y1="10" x2="9.01" y2="10" />
  <line x1="15" y1="10" x2="15.01" y2="10" />
  <path d="M9.5 15.25a3.5 3.5 0 0 1 5 0" />`
        },


      ],

      songs:[
        {
          name:"fire",
          icon:`<path stroke="none" d="M0 0h24v24H0z" fill="none"/>
        <path d="M12 12c2 -2.96 0 -7 -1 -8c0 3.038 -1.773 4.741 -3 6c-1.226 1.26 -2 3.24 -2 5a6 6 0 1 0 12 0c0 -1.532 -1.056 -3.94 -2 -5c-1.786 3 -2.791 3 -4 2z" />`,
          tags:["relax","productivity"],
          audio:"/assets/campfire.wav",
          played:false,
        },

        {
          name:"rain",
          icon:`<path stroke="none" d="M0 0h24v24H0z" fill="none"/>
  <path d="M7 18a4.6 4.4 0 0 1 0 -9a5 4.5 0 0 1 11 2h1a3.5 3.5 0 0 1 0 7" />
  <path d="M11 13v2m0 3v2m4 -5v2m0 3v2" />`,
          tags:["relax","sadness"],
          audio: "/assets/rain.wav" ,
          played:false,
        },

        {
          name:"wind",
          icon:`<path stroke="none" d="M0 0h24v24H0z" fill="none"/>
  <path d="M5 8h8.5a2.5 2.5 0 1 0 -2.34 -3.24" />
  <path d="M3 12h15.5a2.5 2.5 0 1 1 -2.34 3.24" />
  <path d="M4 16h5.5a2.5 2.5 0 1 1 -2.34 3.24" />`,
          tags:["productivity","focus"],
          audio: "/assets/winds.wav" ,
          played:false,
        },

        {
          name:"birds",
          icon:`<path stroke="none" d="M0 0h24v24H0z" fill="none"/>
  <path d="M4 20l10 -10m0 -5v5h5m-9 -1v5h5m-9 -1v5h5m-5 -5l4 -4l4 -4" />
  <path d="M19 10c.638 -.636 1 -1.515 1 -2.486a3.515 3.515 0 0 0 -3.517 -3.514c-.97 0 -1.847 .367 -2.483 1m-3 13l4 -4l4 -4" />`,
          tags:["focus"],
          audio: "/assets/birds.wav" ,
          played:false,
        },

        {
          name:"keyboard",
          icon:`<path stroke="none" d="M0 0h24v24H0z" fill="none"/>
  <rect x="2" y="6" width="20" height="12" rx="2" />
  <line x1="6" y1="10" x2="6" y2="10" />
  <line x1="10" y1="10" x2="10" y2="10" />
  <line x1="14" y1="10" x2="14" y2="10" />
  <line x1="18" y1="10" x2="18" y2="10" />
  <line x1="6" y1="14" x2="6" y2="14.01" />
  <line x1="18" y1="14" x2="18" y2="14.01" />
  <line x1="10" y1="14" x2="14" y2="14" />`,
          tags:["relax","sadness"],
          audio: "/assets/keyboard.wav" ,
          played:false,
        },

        {
          name:"forest",
          icon:`<path stroke="none" d="M0 0h24v24H0z" fill="none"/>
  <path d="M5 21c.5 -4.5 2.5 -8 7 -10" />
  <path d="M9 18c6.218 0 10.5 -3.288 11 -12v-2h-4.014c-9 0 -11.986 4 -12 9c0 1 0 3 2 5h3z" />`,
          tags:["relax"],
          audio: "/assets/forest.wav" ,
          played:false,
        },


        {
          name:"lightning",
          icon:`<path stroke="none" d="M0 0h24v24H0z" fill="none"/>
  <path d="M7 18a4.6 4.4 0 0 1 0 -9a5 4.5 0 0 1 11 2h1a3.5 3.5 0 0 1 0 7h-1" />
  <polyline points="13 14 11 18 14 18 12 22" />`,
          tags:["productivity","sadness"],
          audio: "/assets/lightning.wav" ,
          played:false,
        },


        {
          name:"office",
          icon:`<path stroke="none" d="M0 0h24v24H0z" fill="none"/>
  <line x1="3" y1="21" x2="21" y2="21" />
  <path d="M5 21v-14l8 -4v18" />
  <path d="M19 21v-10l-6 -4" />
  <line x1="9" y1="9" x2="9" y2="9.01" />
  <line x1="9" y1="12" x2="9" y2="12.01" />
  <line x1="9" y1="15" x2="9" y2="15.01" />
  <line x1="9" y1="18" x2="9" y2="18.01" />`,
          tags:["productivity"],
          audio: "/assets/office.wav" ,
          played:false,
        },



        {
          name:"farm",
          icon:`<path stroke="none" d="M0 0h24v24H0z" fill="none"/>
  <circle cx="7" cy="15" r="4" />
  <line x1="7" y1="15" x2="7" y2="15.01" />
  <circle cx="19" cy="17" r="2" />
  <line x1="10.5" y1="17" x2="17" y2="17" />
  <path d="M20 15.2v-4.2a1 1 0 0 0 -1 -1h-6l-2 -5h-6v6.5" />
  <path d="M18 5h-1a1 1 0 0 0 -1 1v4" />`,
          tags:["focus"],
          audio: "/assets/farm.wav" ,
          played:false,
        },
        


        {
          name:"ocean",
          icon:`<path stroke="none" d="M0 0h24v24H0z" fill="none"/>
  <path d="M3 7c3 -2 6 -2 9 0s6 2 9 0" />
  <path d="M3 17c3 -2 6 -2 9 0s6 2 9 0" />
  <path d="M3 12c3 -2 6 -2 9 0s6 2 9 0" />`,
          tags:["productivity","relax"],
          audio: "/assets/ocean.wav" ,
          played:false,
        },



        {
          name:"bike",
          icon:`<path stroke="none" d="M0 0h24v24H0z" fill="none"/>
  <circle cx="5" cy="18" r="3" />
  <circle cx="19" cy="18" r="3" />
  <polyline points="12 19 12 15 9 12 14 8 16 11 19 11" />
  <circle cx="17" cy="5" r="1" />`,
          tags:["focus","sadness"],
          audio: "/assets/bike.wav" ,
          played:false,
        },





        {
          name:"night",
          icon:`<path stroke="none" d="M0 0h24v24H0z" fill="none"/>
  <path d="M12 3c.132 0 .263 0 .393 0a7.5 7.5 0 0 0 7.92 12.446a9 9 0 1 1 -8.313 -12.454z" />
  <path d="M17 4a2 2 0 0 0 2 2a2 2 0 0 0 -2 2a2 2 0 0 0 -2 -2a2 2 0 0 0 2 -2" />
  <path d="M19 11h2m-1 -1v2" />`,
          tags:["sadness"],
          audio: "/assets/night.wav" ,
          played:false,
        },




        {
          name:"st-wars",
          icon:`<path stroke="none" d="M0 0h24v24H0z" fill="none"/>
  <path d="M15.5 18.5l-3 1.5l.5 -3.5l-2 -2l3 -.5l1.5 -3l1.5 3l3 .5l-2 2l.5 3.5z" />
  <line x1="4" y1="4" x2="11" y2="11" />
  <line x1="9" y1="4" x2="12.5" y2="7.5" />
  <line x1="4" y1="9" x2="7.5" y2="12.5" />`,
          tags:["relax"],
          audio: "/assets/st-wars.mp3" ,
          played:false,
        },


        {
          name:"lofi",
          icon:`<path stroke="none" d="M0 0h24v24H0z" fill="none"/>
  <path d="M14 3v4a1 1 0 0 0 1 1h4" />
  <path d="M17 21h-10a2 2 0 0 1 -2 -2v-14a2 2 0 0 1 2 -2h7l5 5v11a2 2 0 0 1 -2 2z" />
  <circle cx="11" cy="16" r="1" />
  <polyline points="12 16 12 11 14 12" />`,
          tags:["productivity"],
          audio: "/assets/lofi.mp3" ,
          played:false,
        },



        {
          name:"anime",
          icon:`<path stroke="none" d="M0 0h24v24H0z" fill="none"/>
  <circle cx="12" cy="12" r="4" />
  <path d="M16 12v1.5a2.5 2.5 0 0 0 5 0v-1.5a9 9 0 1 0 -5.5 8.28" />`,
          tags:[],
          audio: "/assets/anime.mp3" ,
          played:false,
        },




        {
          name:"train",
          icon:`<path stroke="none" d="M0 0h24v24H0z" fill="none"/>
  <path d="M21 13c0 -3.87 -3.37 -7 -10 -7h-8" />
  <path d="M3 15h16a2 2 0 0 0 2 -2" />
  <path d="M3 6v5h17.5" />
  <line x1="3" y1="10" x2="3" y2="14" />
  <line x1="8" y1="11" x2="8" y2="6" />
  <line x1="13" y1="11" x2="13" y2="6.5" />
  <line x1="3" y1="19" x2="21" y2="19" />`,
          tags:["focus"],
          audio: "/assets/train.wav" ,
          played:false,
        },





        {
          name:"clock",
          icon:`<path stroke="none" d="M0 0h24v24H0z" fill="none"/>
  <circle cx="12" cy="12" r="9" />
  <polyline points="12 7 12 12 15 15" />`,
          tags:[],
          audio: "/assets/clock.wav" ,
          played:false,
        },




        {
          name:"morning",
          icon:`<path stroke="none" d="M0 0h24v24H0z" fill="none"/>
  <path d="M3 17h1m16 0h1m-15.4 -6.4l.7 .7m12.1 -.7l-.7 .7m-9.7 5.7a4 4 0 0 1 8 0" />
  <line x1="3" y1="21" x2="21" y2="21" />
  <path d="M12 3v6l3 -3m-6 0l3 3" />`,
          tags:["relax"],
          audio: "/assets/morning.wav" ,
          played:false,
        },





        {
          name:"stadium",
          icon:`<path stroke="none" d="M0 0h24v24H0z" fill="none"/>
  <circle cx="12" cy="12" r="9" />
  <path d="M12 7l4.76 3.45l-1.76 5.55h-6l-1.76 -5.55z" />
  <path d="M12 7v-4m3 13l2.5 3m-.74 -8.55l3.74 -1.45m-11.44 7.05l-2.56 2.95m.74 -8.55l-3.74 -1.45" />`,
          tags:[],
          audio: "/assets/stadium.wav" ,
          played:false,
        },


        
        {
          name:"airport",
          icon:`<path stroke="none" d="M0 0h24v24H0z" fill="none"/>
  <path d="M15 12h5a2 2 0 0 1 0 4h-15l-3 -6h3l2 2h3l-2 -7h3z" transform="rotate(-15 12 12) translate(0 -1)" />
  <line x1="3" y1="21" x2="21" y2="21" />`,
          tags:[],
          audio: "/assets/airport.wav" ,
          played:false,
        },




      ]


    }
  },
  methods:{
  play(name)
  {
    for(let i= 0 ; i < this.songs.length ; i++)
    {
      if( this.songs[i].name == name  )
      {
        let name = this.songs[i].name ;


        if(typeof(this.songs[i].audio) == typeof("str") )
        {
          this.loadings.push(this.songs[i].name );
          this.songs[i].audio = new Audio( this.songs[i].audio) ;
          setTimeout(()=>{
            this.loadings = this.loadings.filter(e => e !== this.songs[i].name);
            if(!this.songs[i].played)
            { 
              this.songs[i].played  =true;
              this.playOneSong( i )
            }
            else{
              this.songs[i].audio.pause();
              this.songs[i].played  =false;
            }
          } , 800)
        }

        else
        {
          if(!this.songs[i].played)
          { 
            this.songs[i].played  =true;
            this.playOneSong( i )
          }
          else{
            this.songs[i].audio.pause();
            this.songs[i].played  =false;
          }
          this.$refs[name].blur();
        }
        
        this.$refs[name].blur();
      }
    } 
  }
  ,

  playOneSong( index )
  { let duration = this.songs[index].audio.duration ;
    let startPoint = Math.random() * duration  ;
    this.songs[index].audio.play();
    // this.songs[index].audio.currentTime = startPoint;
    this.songs[index].audio.onended  = () => this.playOneSong( index );
  },


  changeVolume(name)
  {
    for(let i= 0 ; i < this.songs.length ; i++)
    {
      if( this.songs[i].name == name  )
      {
        let name = this.songs[i].name ;
        this.songs[i].audio.volume = this.$refs[name+'Volume'].value * 0.01 ;
      }
    } 
  },



  Suggest(suggetion)
  {
    suggetion = suggetion.toLowerCase();
    let random_length = Math.ceil(Math.random() * 3) ;
    let real_length = random_length + 2 ;
    for(let i= 0 ; i < this.songs.length ; i++)
    { 

      if(typeof(this.songs[i].audio) !== typeof("str") )
        {
          this.songs[i].audio.pause();
        }
      this.songs[i].played  =false;


      if( this.songs[i].tags.includes(suggetion) && Math.ceil( Math.random() * 1000 ) % 2 == 0  )
      {
        real_length-- ;
        this.songs[i].played  =true;
        if(typeof(this.songs[i].audio) == typeof("str") )
        {
          this.loadings.push(this.songs[i].name );
          this.songs[i].audio = new Audio( this.songs[i].audio) ;
          setTimeout(()=>{
            this.loadings = this.loadings.filter(e => e !== this.songs[i].name); 
            this.playOneSong( i )
          } , 800)
        }
        else{
          this.playOneSong(i)
        }
          
      }

      if(real_length == 0)
      {
        break;
      }
    }
  },



  Changemode( mode )
  {

    if(mode == "dark")
    {
      this.dark = true;
    }
    else
    {
      this.dark = false;
    }

  }



  


  }
  ,
  created()
  {
    let cookies = document.cookie.split(";");
    for(let i = 0 ; i< cookies.length ; i++)
    {   
        let cookie  = cookies[i].split("=");
        if( cookie[0] == "mode" )
        {
            if(cookie[1] === "dark") 
            {
                this.dark = true;
            }
            else
            {
              this.dark = false;
            }
            break;
        }
    }

    axios.post("/")
    .then(response=>{
      this.views = response.data.views;
    });

  }
}
</script>
<style>

#app {
  text-align: center;
  box-sizing:border-box;
}


.app{
  width:100%;
  height: 100%;
  background: var(--bg);
  color: var(--font-color);
}

.dark-app{
    --info: #171717 ;
    --font-color: white;
    --icon: #14B8A6;
    --bg:  #1E293B;
    --secondary: #14B8A6;
    --hover-song: #14B8A6;
}

.header{
  font-size: 4vw;
  margin: 100px auto 0px;
  font-weight: 800;
}

.description{
  font-size: 20px;
  margin: 10px auto;
  font-weight: 200;
}

.views{
  color: var(--primary);
  font-weight: 500;
  font-size:23px;
}

#suggetions{
  width:100%;
  margin:100px 0% 50px ;
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}

.suggetion-box{
  width:140px;
  aspect-ratio: 1 / 1;
  border-radius:10px;
  display:flex;
  background-color: var(--primary);
  align-items: center;
  justify-content: center;
  flex-direction: column;
  margin:0px 20px;
  transition: .5s ease;
}

.suggetion-box:hover{
  box-shadow: 3px 3px 20px var(--info);
  /* transform: translateY(-5px); */
}

.suggetion-box:focus{
  animation: 1s suggetion;  
}

.suggetion-text{
  font-size:16px;
  color:white;
}

#songs{
  width:100%;
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  padding:0px 19%;
}

.song-bg{
  width:23%;
  aspect-ratio: 1 / 1;
  border-radius:14%;
  display:flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  margin:1%;
  transition: .5s ease;
  position: relative;
  transition: .5s ease;
  padding:0.6%;
  border:1px solid var(--bg);
  background: var(--info);
}

.song-bg:hover{
  box-shadow: 0px 0px 7px var(--hover-song) ,
              0px 0px 14px var(--hover-song),
              0px 0px 28px var(--hover-song);
}

.song-bg-played{
  background: linear-gradient(124deg, #ff2400, #e81d1d, #e8b71d, #e3e81d, #1de840, #1ddde8, #2b1de8, #dd00f3, #dd00f3);
  background-size: 1800% 1800%;
  -webkit-animation: rainbow 18s ease infinite;
  -z-animation: rainbow 18s ease infinite;
  -o-animation: rainbow 18s ease infinite;
  animation: rainbow 18s ease infinite;
}

.song{
  border-radius:14%;
  display:flex;
  width: 100%;
  height: 100%;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  transition: .5s ease;
  position: relative;
  background: var(--info);
  overflow: hidden;
  transition: .5s ease;
}


.song-name{
  font-size: 17px;
  position: absolute;
  top:6%;
  left:6%;
  font-weight: 300;
  color: var(--font-color);
}

.volume-input{
  position: absolute;
  width: 100%;
  bottom:0px;
  background: var(--bg);
  padding: 2% 1%;
}

.volume-input input{
  width: 90%;
}


@media ( max-width: 1050px )
{



#songs{
  padding:0px 14%;
}



.song-bg{
  width:31%;
  aspect-ratio: 1 / 1;
  border-radius:14%;
  display:flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  margin:1%;
  transition: .5s ease;
  position: relative;
  transition: .5s ease;
  padding:0.8%;
  border:1px solid var(--bg);
  background: var(--info);
}

.song-bg-played{
  background: linear-gradient(124deg, #ff2400, #e81d1d, #e8b71d, #e3e81d, #1de840, #1ddde8, #2b1de8, #dd00f3, #dd00f3);
  background-size: 1800% 1800%;
  -webkit-animation: rainbow 18s ease infinite;
  -z-animation: rainbow 18s ease infinite;
  -o-animation: rainbow 18s ease infinite;
  animation: rainbow 18s ease infinite;
}

}

@media ( max-width: 600px )
{
  
#suggetions {
  flex-wrap: wrap;
}

.suggetion-box{
  width: 30%;
  margin: 1% ;
}

#songs{
  padding:0px 2%;
}

.song-bg{
  width:46%;
}

.song-bg:hover{
  box-shadow: 0px 0px 2px var(--hover-song) ,
              0px 0px 4px var(--hover-song),
              0px 0px 8px var(--hover-song);
}

.header{
  font-size: 4vh;
  margin: 100px auto 0px;
  font-weight: 800;
}

.description{
  font-size: 20px;
  margin: 10px auto;
  font-weight: 200;
}


}

@media ( max-width: 450px )
{
.suggetion-box{
  width: 40%;
  margin: 4% ;
}

.song-bg{
  width: 48%;
}

.volume-input{
  padding: 0.5% 0.5%;
}

.volume-input input{
  transform: scale(0.8);
}

}

@media ( max-width: 280px )
{
  .song-bg{
  width: 90%;
}
}

@keyframes suggetion {
  0%{
    transform: translateY(8px);
  }
  20%{
    transform: translateY(-3px);
  }
  40%{
    transform: translateY(3px);
  }
  60%{
    transform: translateY(-1px);
  }
  80%{
    transform: translateY(1px);
  }
  100%{
    transform: translateY(0px);
  }
}




@-webkit-keyframes rainbow {
    0%{background-position:0% 82%}
    50%{background-position:100% 19%}
    100%{background-position:0% 82%}
}
@-moz-keyframes rainbow {
    0%{background-position:0% 82%}
    50%{background-position:100% 19%}
    100%{background-position:0% 82%}
}
@-o-keyframes rainbow {
    0%{background-position:0% 82%}
    50%{background-position:100% 19%}
    100%{background-position:0% 82%}
}
@keyframes rainbow { 
    0%{background-position:0% 82%}
    50%{background-position:100% 19%}
    100%{background-position:0% 82%}
}

</style>

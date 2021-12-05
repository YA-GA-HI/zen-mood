<template>
<div class="share-links">
    <a :href="'fb-messenger://share?link='+ this.link +'&amp;app_id=326516237427150'" target="_blank" title="Messenger" class="share-link messenger">
        <svg xmlns="http://www.w3.org/2000/svg" class="share-link-icon"  viewBox="0 0 24 24" stroke-width="1.5" stroke="white" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
            <path d="M3 20l1.3 -3.9a9 8 0 1 1 3.4 2.9l-4.7 1" />
            <path d="M8 13l3 -2l2 2l3 -2" />
            </svg>
    </a>

    <a href="#" @click="window.open('https://www.facebook.com/sharer/sharer.php?u='+this.link, 'facebook-share-dialog', 'width=626,height=436');" 
        class="share-link facebook">
        <svg xmlns="http://www.w3.org/2000/svg" class="share-link-icon" viewBox="0 0 24 24" stroke-width="1.5" stroke="white" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
            <path d="M7 10v4h3v7h4v-7h3l1 -4h-4v-2a1 1 0 0 1 1 -1h3v-4h-3a5 5 0 0 0 -5 5v2h-3" />
        </svg>
    </a>

    <a :href="'https://twitter.com/intent/tweet?text='+ this.message" target="_blank" title="Twitter" class="share-link messenger">
        <svg xmlns="http://www.w3.org/2000/svg" class="share-link-icon" viewBox="0 0 24 24" stroke-width="1.5" stroke="white" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
            <path d="M22 4.01c-1 .49 -1.98 .689 -3 .99c-1.121 -1.265 -2.783 -1.335 -4.38 -.737s-2.643 2.06 -2.62 3.737v1c-3.245 .083 -6.135 -1.395 -8 -4c0 0 -4.182 7.433 4 11c-1.872 1.247 -3.739 2.088 -6 2c3.308 1.803 6.913 2.423 10.034 1.517c3.58 -1.04 6.522 -3.723 7.651 -7.742a13.84 13.84 0 0 0 .497 -3.753c-.002 -.249 1.51 -2.772 1.818 -4.013z" />
        </svg>
    </a>

    <a :href="'whatsapp://send?text='+ this.message " target="_blank" data-action="share/whatsapp/share" class="share-link whatsapp">
        <svg xmlns="http://www.w3.org/2000/svg"  class="share-link-icon" viewBox="0 0 24 24" stroke-width="1.8" stroke="white" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
            <path d="M3 21l1.65 -3.8a9 9 0 1 1 3.4 2.9l-5.05 .9" />
            <path d="M9 10a0.5 .5 0 0 0 1 0v-1a0.5 .5 0 0 0 -1 0v1a5 5 0 0 0 5 5h1a0.5 .5 0 0 0 0 -1h-1a0.5 .5 0 0 0 0 1" />
        </svg>
    </a>

    

    <a href="#" @click="copyLink()" title="Link" class="share-link messenger">
        <svg xmlns="http://www.w3.org/2000/svg" class="share-link-icon" viewBox="0 0 24 24" stroke-width="1.5" stroke="white" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
            <path d="M10 14a3.5 3.5 0 0 0 5 0l4 -4a3.5 3.5 0 0 0 -5 -5l-.5 .5" />
            <path d="M14 10a3.5 3.5 0 0 0 -5 0l-4 4a3.5 3.5 0 0 0 5 5l.5 -.5" />
        </svg>
    </a>
</div>
</template>

<script >

export default{
    data()
    {
        return{
            message: `Hy! I Tried This Web App That Provide A Mix Of Songs, Developped By Zenku. It's So Awesome.
            It Helps Me Relaxing And Focusing. I Suggest You to Try it ❤.
            https://zen-mood.herokuapp.com/
            #zenku
            #zen-mood
            #songs`

        }
    },
    methods:{
        copyLink()
        {
                var listener = function(ev) {
                ev.clipboardData.setData("text/plain", this.link);
                ev.preventDefault();
                };
                document.addEventListener("copy", listener);
                document.execCommand("copy");
                document.removeEventListener("copy", listener);
                alert('link copied successfully!');
        }
    }
}

</script>

<style scoped>

.share-links{
  width: 100%;
  margin:30px 0;
  display: flex;
  justify-content: center;
}

.share-link{
  width: 50px;
  height: 50px;
  box-shadow: 3px 7px 6px var(--hover-color);
  display: flex;
  margin:0 1vw;
  justify-content: center;
  align-items: center;
  border-radius: 50%;
  transition: 0.5s ease;

}

.share-link:hover{
transform: scale(1.1);

}


.share-link svg{
  width: 65%;
  height: 65%;
}

.share-links .whatsapp{
  background-color: #22e57e;
}

.share-links .facebook{
  background-color: #3f5b96;
}

.share-links .messenger{
  background-color: #1783f9;
}

@media (max-width: 800px)
{
    .share-link{
    width: 45px;
    height: 45px;
    }
}



</style>
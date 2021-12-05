<template>
<footer id="footer">

    <div id="developper">
    @ 2021 Zen-mood by <a href="https://www.instagram.com/_zenku__/">Zenku</a>
    </div>

    <div class="nav-elements">
        <a href="https://www.instagram.com/_zenku__/" class="nav-element element-secondary">
            <svg xmlns="http://www.w3.org/2000/svg"  width="70%" height="70%" viewBox="0 0 24 24" stroke-width="1.5" stroke="white" fill="none" stroke-linecap="round" stroke-linejoin="round">
                <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
                <rect x="4" y="4" width="16" height="16" rx="4" />
                <circle cx="12" cy="12" r="3" />
                <line x1="16.5" y1="7.5" x2="16.5" y2="7.501" />
            </svg>
        </a>

        <a href="https://twitter.com/KariyaKariya8" class="nav-element element-secondary">
            <svg xmlns="http://www.w3.org/2000/svg" width="70%" height="70%"  viewBox="0 0 24 24" stroke-width="1.5" stroke="white" fill="none" stroke-linecap="round" stroke-linejoin="round">
                <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
                <path d="M22 4.01c-1 .49 -1.98 .689 -3 .99c-1.121 -1.265 -2.783 -1.335 -4.38 -.737s-2.643 2.06 -2.62 3.737v1c-3.245 .083 -6.135 -1.395 -8 -4c0 0 -4.182 7.433 4 11c-1.872 1.247 -3.739 2.088 -6 2c3.308 1.803 6.913 2.423 10.034 1.517c3.58 -1.04 6.522 -3.723 7.651 -7.742a13.84 13.84 0 0 0 .497 -3.753c-.002 -.249 1.51 -2.772 1.818 -4.013z" />
            </svg>
        </a>
        
        <button @click="toTop()" class="nav-element element-secondary">
            <svg xmlns="http://www.w3.org/2000/svg" width="90%" height="90%" viewBox="0 0 24 24" stroke-width="1.7" stroke="white" fill="none" stroke-linecap="round" stroke-linejoin="round">
                <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
                <line x1="12" y1="5" x2="12" y2="19" />
                <line x1="18" y1="11" x2="12" y2="5" />
                <line x1="6" y1="11" x2="12" y2="5" />
            </svg>
        </button>  


    </div>
</footer>
</template>

<script>

export default{
    methods:
    {
        toTop()
        {
            document.body.scrollTop = 0; // For Safari
            document.documentElement.scrollTop = 0; // For Chrome, Firefox, IE and Opera
        }
    }
}
</script>

<style scoped>

#footer{
  width:62%;
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  margin:150px auto 0;
  align-items: center;
  padding:10px 15px;
  background: var(--primary);
  border-top-left-radius: 15px;
  border-top-right-radius: 15px;
  color: white;
} 

#developper{
    font-size: 17px;
}
#developper a{
    font-weight: bold;
    text-decoration: underline;
    color: var(--bg);
}

@media ( max-width: 1050px )
{

#footer{
  width:72%;
}

}

@media ( max-width: 600px )
{
  
#footer{
  width:82%;
}

}

@media ( max-width: 530px )
{
  
#footer{
  width:92%;
  justify-content: center;
  align-items: center;
}

#developper{
    margin-bottom: 7px;
}

}

</style>
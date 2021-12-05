<template>
<nav id="navbar">

    <!--logo-->
    <div class="logo">
        Zen-m<span class="logo-styled-container"><div class="logo-styled-primary">o</div><div class="logo-styled-secondary">o</div>
        </span>d
    </div>

    <!--nav elements-->
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

        <button @click="darkMode()" class="nav-element element-secondary">
            <svg xmlns="http://www.w3.org/2000/svg" width="90%" height="90%" viewBox="0 0 24 24" stroke-width="1.5" stroke="white" fill="none" stroke-linecap="round" stroke-linejoin="round">
                <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
                <path d="M12 3c.132 0 .263 0 .393 0a7.5 7.5 0 0 0 7.92 12.446a9 9 0 1 1 -8.313 -12.454z" />
            </svg>
        </button>  

    </div>
</nav>
</template>

<script>
export default {

    methods:{

        darkMode()
        {
            /*
            * DARK MODE CODE
            */
            let cookies = document.cookie.split(";");
            for(let i = 0 ; i< cookies.length ; i++)
            {   
                let cookie  = cookies[i].split("=");
                if( cookie[0] == "mode" )
                {
                    if(cookie[1] == "dark") 
                    {
                        document.cookie="mode=light;";
                        this.$emit('mode','light')
                    }
                    else
                    {
                        document.cookie="mode=dark;" ; 
                        this.$emit('mode','dark')
                    }
                    break;
                }
                if(i === cookies.length-1 )
                {
                    document.cookie="mode=dark;";
                    this.$emit('mode','dark');
                }
            }
        },


    }
}
</script>

<style scoped>
#navbar{
    width: 100%;
    padding:20px 30px;
    background: var(--info);
    display: flex;
    justify-content: space-between;
    align-items:center;
}

.logo{
    font-size:24px;
    transition: .5s ease;
    letter-spacing: 1px;
}

.logo:hover {
    letter-spacing: 3px;
}

.logo-styled-container{
    position: relative;
}

.logo-styled-primary{
    color: var(--primary);
    animation: logo-lettre 6s 3s infinite;
    display: inline-block;
}

.logo-styled-secondary{
    color: var(--secondary);
    animation: logo-lettre 6s 4s infinite;
    display: inline-block;
}

.logo-styled-container:before{
    content:"";
    position: absolute;
    bottom: -1px;
    left:50%;
    transform: translateX(-50%) ;
    width:75%;
    height:3px;
    border-radius:2px;
    border-bottom: 3px solid var(--primary);
}

@media (max-width: 500px)
{   
    #navbar{
        padding:15px;
    }
        
}    


@keyframes logo-lettre {
    20%
    {
        transform:scaleX(0.8) scaleY(1.2);
    }
    50%{
        transform:scaleX(1) scaleY(1);
    }
    100%
    {
        transform:scaleX(1) scaleY(1);
    }
}




</style>
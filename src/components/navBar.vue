<template>
    <nav class="navbar is-fixed-top" role="navigation" aria-label="main navigation">
    <div class="navbar-start">
        <div class="navbar-brand">
            <p class="navbar-item title px-6 py-2">
                cryptoDashboard
            </p>
        </div>
        <div class="navbar-menu"> 
            <a class="navbar-item" @click.prevent="$emit('overview')">Overview</a>
            <a class="navbar-item" @click.prevent="$emit('exchange-data')">Exchanges Locales</a>
        </div>
    </div>
    </nav>
</template>

<script>
export default {
  name: 'navBar',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
    .navbar{
        background: var(--mainBackground);
        .navbar-item{
        color: var(--mainTextColor);
         &:hover { 
             transition: 0.2s;
             background: var(--secondaryBackground);
             //color: var(--secondaryTextColor)
          }
        }
    }
    .sidenav{
            height: 100%; /* Full-height: remove this if you want "auto" height */
            width: 200px; /* Set the width of the sidebar */
            position: fixed; /* Fixed Sidebar (stay in place on scroll) */
            z-index: 1; /* Stay on top */
            top: 40px; /* Stay at the top */
            left: 0;
            background-color: var(--mainBackground); /* Black */
            overflow-x: hidden; /* Disable horizontal scroll */
            padding-top: 20px;
            *li{
                text-align: left;
                padding: 10px 0 10px 10px;
                 &:hover { 
                    transition: 0.1s;
                    background: var(--secondaryBackground);
                    //color: var(--secondaryTextColor)
                }
                .sidenav-item{
                    color: var(--mainTextColor);
                    
                }
            }
    }


</style>
<template>
 <div class="nav-container">
   <navBar @exchange-data="currentComponentView = 'exchangeData'" @overview="currentComponentView = 'dashboard'"/>
 </div>
 <div class="views-container">
   <component :is="currentComponentView" />
 </div>
</template>

<script>
import dashboard from './Dashboard.vue'
import navBar from './navBar.vue'
import ExchangeData from './exchangeData.vue'
export default {
  name: 'main-container',
  components: {
    navBar,
    dashboard,
    ExchangeData
  },
   data: () => ({
     currentComponentView: 'dashboard',
     comps: [
       'dashboard',
       'exchangeData'
		]
  }),
  mounted(){
    this.getComponent()
  },
  computed:{
    currentComponent: function () {
			return this.currentComponentView;
		},
  },
  methods: {
    getComponent() {
			switch (this.currentComponentView) {
				case 'dashboard':
					this.currentComponentView = 'dashboard';
					break;
				case 'exchangeData':
					this.currentComponentView = 'exchangeData';
					break;
				default:
					this.currentComponentView = 'dashboard';
			}
		},
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
:root {
  --mainBackground: #1F2430;
  --secondaryBackground: #171B24;
  /* --secondaryBackground: #63759926; */
  --mainTextColor: #FFD173;
  --secondaryTextColor: #CCCAC2;
  --success: #d5ff80;
  --error: #ff6666;
}
.views-container{
  height: 100%;
}
</style>

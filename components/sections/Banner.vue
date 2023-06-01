<template>
    <div class="max_container">
        <div class="relative overflow-hidden" :class="banner.right.active ? 'md:w-full' : ''">
            <div class="flex image relative  slider-item" :class="'last'" v-if="lastItem">
                <si-image class="w-full h-full object-cover" wi :src="lastItem.image ? lastItem.image.src: $store.state.defaults.sliderImage" :alt="lastItem.title"/>
                <div class="flex justify-center w-full absolute bottom-0 left-0 p-2">
                    <span v-for="(x,y) in Object.keys(banner.items)" :key="y" :class="y==activeIndex ?'bg-gray-900': ''" class="w-3 h-3 rounded-full bg-gray-500 mx-1 flex cursor-pointer" @click="activeIndex=y"></span>
                </div>
            </div>
            <!-- Slide Iitems -->
            <template v-for="(key,i) in Object.keys(banner.items)">
                <div class="slider-item relative" :class="i==activeIndex ? 'active': ''" :key="i">
                    <div class="image w-full h-full">
                        <si-image  class="object-cover h-full w-full" :src="banner.items[key].image ? banner.items[key].image.src : $store.state.defaults.sliderImage" :alt="banner.items[key].title"/>
                        <div class="flex justify-center w-full absolute bottom-0 left-0 p-2 ">
                            <span v-for="(x,y) in Object.keys(banner.items)" :key="y" :class="y==activeIndex ?'bg-gray-900': ''" class="w-3 h-3 rounded-full bg-gray-500 mx-1 flex cursor-pointer" @click="animate(y)"></span>
                        </div>
                    </div>
                    <!--  -->
                    <div class="bannerCard" >
                        <p class="bannerTitle" :class="i==activeIndex ? 'titleActive': ''">{{ banner.items[key].title }}</p>
                        <h1 class="bannerDescription">{{ banner.items[key].description }}</h1>
                        <router-link class="bannerButton text-white flex px-1 py-1  justify-center " v-if="banner.items[key].button.active" :to="banner.items[key].button.url">{{ banner.items[key].button.text }}</router-link>
                    </div>
                </div>
            </template>
            <!-- Slide Iitems -->
        </div>
    </div>
</template>
<script>
export default {
  data() {
    return {
        timeout: null,
        lastItem: null,
        activeIndex: 0,
        banner: this.$settings.sections.banner
    }
  },
  mounted() {
      this.animate();
  },
  methods: {
      animate(index=null){
          if(index != null){
              this.lastItem = this.banner.items[`item_${[this.activeIndex]}`];
              this.activeIndex = index;
              clearTimeout(this.timeout);
          }else{
              this.lastItem = this.banner.items[`item_${[this.activeIndex]}`];
              if(this.activeIndex >= Object.keys(this.banner.items).length-1) this.activeIndex = -1;
              this.activeIndex++;
              this.timeout = setTimeout(() => {
                  this.animate();
                  // Active this in production
                  // this.$nuxt.error({ statusCode: 404, message: 'Okay' })
              }, 10000);
          }
      }
  },
}
</script>
<style>
.slider-item{
  width: 100vw;
  height: 70vh;
}

.slider-item.last{
    position: absolute;
}

.slider-item:not(.active):not(.last){
    position: absolute;
    top: 0;
    transform: translateX(-110%);
}

.slider-item.active{
    transform: none;
    transition: 2s;
}

.bannerCard{
  padding-left: 200px;
  display: flex;
  flex-direction: column;
  justify-content:center;
  align-items: flex-start;
  color: #222;
  position: absolute;
  top: 0;
  width: 100%;
  height: 100%;
  transition: all 0.5s ease;
}

.bannerTitle{
  font-weight: 500;
  font-size: 1.2rem;
  letter-spacing: 2px;
  color: #222;
  text-align: start;
}

.bannerDescription {
  font-size: 3.5rem;
  font-weight: 500;
}

.titleActive{
  opacity: 1;
}

.bannerButton {
  font-size: 1rem;
  padding: 6px 30px;
  background: #222;
  transition: all 0.5s ease;
}

.bannerButton:hover {
  background-color: var(--primary);
  color: white !important;
}

@media screen and (max-width: 1024px) {
  .slider-item{
    width: 100vw;
    height: 50vh;
  }

  .bannerCard{
    padding-left: 1.25rem;
  }

  .bannerTitle{
    font-size: 1rem;
  }

  .bannerDescription {
    font-size: 2.5rem;
  } 
}

@media screen and (max-width: 600px) {
  .slider-item{
    width: 100vw;
    height: 30vh;
  }
  .bannerTitle{
    font-size: 0.8rem;
  }
  .bannerDescription {
    font-size: 1.5rem;
  }
  .bannerButton {
    font-size: 0.8rem;
    padding: 4px 20px;
  }
}
</style>

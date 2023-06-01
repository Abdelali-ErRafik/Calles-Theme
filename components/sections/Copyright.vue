<template>
  <div class="container">
    <!-- Start copyright Section -->
    <div class="copyright flex items-center items-centerpy-5 px-4 py-3.5" :class="copyright_menu == null? 'justify-center' : 'justify-between'">
        <div
          class="text-sm" 
          v-html="description.replace(/__YEAR__/gi, year)"
        ></div>
          <div v-if="copyright_menu">
            <div class="inline-block" v-for="(item,i) in copyright_menu.items" :key="i">
                <div class="flex flex-row items-center">
                    <h4>
                        <router-link class="mx-3 text-sm" :to="item.url">{{item.text}}</router-link>
                    </h4>
                    <div class="flex flex-row items-center" v-if="item.childrens && item.childrens.length > 0">
                      <div class="inline-block" v-for="(child,ii) in item.childrens" :key="ii">
                          <nuxt-link class="mx-3 text-sm"  :to="child.url">
                              {{ child.text }}
                          </nuxt-link>
                          <div class="inline-block"  v-if="child.childrens && child.childrens.length > 0">
                              <div class="flex flex-row items-center" v-for="(child2,iii) in child.childrens" :key="iii">
                                  <nuxt-link class="mx-3 text-sm" :to="child2.url">
                                      {{ child2.text }}
                                  </nuxt-link>
                              </div>
                          </div>
                      </div>
                    </div>
                </div>
            </div>
          </div>
      </div>
      <!-- End fotter Section -->
  </div>
</template>
<script>
export default {
  data() {
    return {
      copyright_menu: this.$settings.sections.footer.menus.copyright_menu,
      year: new Date().getFullYear(),
      description: this.$settings.sections.footer.copyright.text,
    };
  },
};
</script>


<style scoped>
  .copyright  {
    color: var(--text-color);
  }

  @media screen and (max-width: 1025px){
    .copyright {
      flex-direction: column;
      text-align: center;
      justify-content: center;
    }
     
  }

</style>

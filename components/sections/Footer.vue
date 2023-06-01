<template>
    <div class="bg-primary">
      <si-app-loader placement="BEFORE_FOOTER"/>
      <!-- Start footer Section -->
      <footer class="footer container text-sm">
        <div class="footer_container flex flex-wrap flex-row justify-between pt-16">

          <!-- About section  -->
          <div class="footer-contact px-4">
              <!-- Store title -->
              <nuxt-link to="/">
                  <h4 class="text-3xl text-black font-bold mb-6">{{ title }}</h4>
              </nuxt-link>
              <!-- Store title -->
  
              <!-- Footer About -->
                  <div class="mb-4" v-html="footer.about"></div>
              <!-- Footer About -->
  
              <!-- Contacts  -->
              <div class="Contacts flex">
                <div>
                  <p class="mb-4 flex items-center">
                    <fa class="text-xl mr-3" :icon="['fa', 'location-dot']"></fa>
                    <span>{{ footer.location }}</span>
                  </p>
                  <p class="mb-4 flex items-center">
                    <fa class="text-lg mr-2" :icon="['fa', 'envelope']"></fa>
                    <a :href="'mail:'+footer.gmail">{{ footer.gmail }}</a>
                  </p>
                  <p class="flex items-center"> 
                    <fa class="text-lg mr-2" :icon="['fa', 'phone']"></fa>
                    <a :href="'tel:'+footer.number">{{ footer.number }}</a>
                  </p>
              </div>
              </div>
              <!-- Contacts  -->
          </div> 
          <!-- ِAbout section -->


  
          <!-- categoty_menu --> 
          <div v-if="categories_menu" class="categoty_menu px-4">
            <h4 class="text-black text-base font-medium mb-8">{{ menus.categories_menu_title }}</h4>
                <div v-for="(item,i) in categories_menu.items" :key="i">
                    <div>
                        <h4 class="mb-4">
                            <router-link :to="item.url">{{item.text}}</router-link>
                        </h4>
                        <div v-if="item.childrens && item.childrens.length > 0">
                          <div class="mb-4" v-for="(child,ii) in item.childrens" :key="ii">
                              <nuxt-link  :to="child.url">
                                  {{ child.text }}
                              </nuxt-link>
                              <div class="mb-4" v-if="child.childrens && child.childrens.length > 0">
                                  <div v-for="(child2,iii) in child.childrens" :key="iii">
                                      <nuxt-link :to="child2.url">
                                          {{ child2.text }}
                                      </nuxt-link>
                                  </div>
                              </div>
                          </div>
                        </div>
                    </div>
                </div>
          </div>
          <!-- categoty_menu  -->
  
  
          <!-- Infomation_menu -->
          <div v-if="infomation_menu" class="Infomation_menu px-4">
            <h4 class="text-black text-base font-medium mb-8">{{ menus.infomation_menu_title }}</h4>
                <div v-for="(item,i) in infomation_menu.items" :key="i">
                    <div>
                        <h4 class="mb-4">
                            <router-link :to="item.url">{{item.text}}</router-link>
                        </h4>
                        <div v-if="item.childrens && item.childrens.length > 0">
                          <div class="mb-4" v-for="(child,ii) in item.childrens" :key="ii">
                              <nuxt-link :to="child.url">
                                  {{ child.text }}
                              </nuxt-link>
                              <div class="mb-4" v-if="child.childrens && child.childrens.length > 0">
                                  <div v-for="(child2,iii) in child.childrens" :key="iii">
                                      <nuxt-link :to="child2.url">
                                          {{ child2.text }}
                                      </nuxt-link>
                                  </div>
                              </div>
                          </div>
                        </div>
                    </div>
                </div>
            </div>
          <!-- Infomation_menu -->

          <!-- Useful_links -->
          <div v-if="useful_links_menu" class="Useful_links px-4">
            <h4 class="text-black text-base font-medium mb-8">{{ menus.useful_links_menu_title }}</h4>
                <div v-for="(item,i) in useful_links_menu.items" :key="i">
                    <div>
                        <h4 class="mb-4">
                            <router-link :to="item.url">{{item.text}}</router-link>
                        </h4>
                        <div v-if="item.childrens && item.childrens.length > 0">
                          <div class="mb-4" v-for="(child,ii) in item.childrens" :key="ii">
                              <nuxt-link :to="child.url">
                                  {{ child.text }}
                              </nuxt-link>
                              <div class="mb-4" v-if="child.childrens && child.childrens.length > 0">
                                  <div v-for="(child2,iii) in child.childrens" :key="iii">
                                      <nuxt-link :to="child2.url">
                                          {{ child2.text }}
                                      </nuxt-link>
                                  </div>
                              </div>
                          </div>
                        </div>
                    </div>
                </div>
            </div>
          <!-- Useful_links -->

          <!-- payment_methods -->
          <div class="payment_methods px-4 md">
              <h3 class="text-black text-base font-medium mb-8">{{ footer.methods.title }}</h3>
              <div class="payment_images flex flex-wrap">
                  <div v-for="item in methods.filter(m=>$settings.sections.footer.methods[m.name])" :key="item.name" class="w-15 h-8 mr-2 mb-4 flex ">
                      <si-image style="filter: grayscale(100%); mix-blend-mode: multiply;" width="60" height="20" :src="item.image" :alt="item.name" class="h-full object-contain"/>
                  </div>
              </div>
          </div>
          <!-- payment_methods -->
        </div>

          <!--  Social Media -->
          <div class="Social_Media pb-14 mt-8">
            <h3 class="text-black text-base font-medium mb-8 flex justify-center">{{ footer.social_media.title }}</h3>
            <div class="flex items-center justify-center flex-wrap">
              <div v-for="item in socialMedia.filter(s=>footer.social_media[s.name])" :key="item.name" class="flex items-center justify-center p-2 ">
                  <a class="h-full flex" :href="footer.social_media[item.name]" target="_blank" rel="noopener noreferrer">
                      <fa class="text-2xl mx-2" :icon="['fab', item.name]"></fa>
                  </a>
              </div>
            </div>
            <!--  Social Media  -->
          </div>

          <!--  Social Media -->
          <div class="Social_Media pb-14 mt-8">
            <h3 class="text-black text-base font-medium mb-8 flex justify-center">{{ footer.social_media.title }}</h3>
            <div class="flex items-center justify-center flex-wrap">
              <div v-for="item in socialMedia.filter(s=>footer.social_media[s.name])" :key="item.name" class="flex items-center justify-center p-2 ">
                  <a class="h-full flex" :href="footer.social_media[item.name]" target="_blank" rel="noopener noreferrer">
                      <fa class="text-2xl mx-2" :icon="['fab', item.name]"></fa>
                  </a>
              </div>
            </div>
          </div>
          <!--  Social Media  -->

      </footer>
      <!-- End fotter Section -->
      <si-app-loader placement="AFTER_FOOTER"/>
    </div>
  </template>
  <script>
  export default {
    data() {
      return {
        menus: this.$settings.sections.footer.menus,
        categories_menu: this.$settings.sections.footer.menus.categories_menu,
        infomation_menu: this.$settings.sections.footer.menus.infomation_menu,
        useful_links_menu: this.$settings.sections.footer.menus.useful_links_menu,
        footer: this.$settings.sections.footer,
        year: new Date().getFullYear(),
        title: this.$settings.sections.footer.copyright.store_name,
        description: this.$settings.sections.footer.copyright.text,
        methods: [
                  {
                      name: 'visa',
                      image: 'https://storeno.b-cdn.net/themes/palest/visa.png'
                  },
                  {
                      name: 'mastercard',
                      image: 'https://storeno.b-cdn.net/themes/palest/mastercard.png'
                  },
                  {
                      name: 'transfer',
                      image: 'https://storeno.b-cdn.net/themes/palest/transfer.png'
                  },
                  {
                      name: 'cash',
                      image: 'https://storeno.b-cdn.net/themes/palest/cod.png'
                  }
              ],
        socialMedia: [
                  {name: 'facebook',},
                  {name: 'twitter',},
                  {name: 'instagram',},
                  {name: 'youtube',},
                  {name: 'whatsapp',},
                  {name: 'linkedin',},
                  {name: 'snapchat',},
                  {name: 'tiktok',}
              ]
      };
    },
  };
  </script>
  
  
  <style scoped>
  footer {
    color: var(--text-color);
  }

  /* .footer-contact,.categoty_menu,.Infomation_menu,
  .Useful_links, .payment_methods {
    flex: 0 0 auto;
  }

  .footer-contact,.payment_methods  {
    width: 25%;
  }

  .categoty_menu,.Infomation_menu,
  .Useful_links {
    width: 16.66666667%;
  }

  @media screen and (max-width: 1025px){
    .footer-contact,.categoty_menu,.Infomation_menu,
    .Useful_links, .payment_methods {
      width: 50%;
    }

    .Infomation_menu, .Useful_links, 
    .payment_methods, .Social_Media {
      margin-top: 2rem;
    }
     
  }

  @media screen and (max-width: 767px){
    .footer_container {
      text-align: center;
    }

    .footer-contact,.categoty_menu,.Infomation_menu,
    .Useful_links, .payment_methods {
      width: 100%;
    }

    .categoty_menu, .Infomation_menu, .Useful_links, 
    .payment_methods, .Social_Media {
      margin-top: 2rem;
    }

    .Contacts,
    .payment_images {
      justify-content: center;
    }  
  } */
</style>
  
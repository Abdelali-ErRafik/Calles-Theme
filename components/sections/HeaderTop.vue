<template>
  <div class="nav_top_header bg-primary">
    <si-app-loader placement="BEFORE_TOP_HEADER"/>
    <!-- top header -->
      <div class="nav_top flex justify-between items-center px-2 text-xs py-2 lg:py-1">
        
        <!-- left top header content-->
        <div  class="header_left flex flex-1 items-center">
          <div v-if="headertop.content_left.phone_active" class="header_number flex items-center p-1 lg:p-2">
            <fa class="icon text-sm mx-2" :icon="['fa', 'phone']"></fa>
            <a class="" :href="'tel:'+headertop.content_left.phone_number">{{headertop.content_left.phone_number}}</a>
          </div>
          <div v-if="headertop.content_left.email_active" class="header_email flex items-center p-1 lg:p-2">
              <fa class="icon text-sm mx-2" :icon="['fa', 'envelope']"></fa>
              <a class="" :href="'mailto:'+headertop.content_left.email">{{headertop.content_left.email}}</a>
            </div>
        </div>
        <!-- left top header content-->
  
  
        <!-- center header content-->
          <div class="header_center">
            <div class="p-1 lg:p-2" v-if="headertop.content_center.message_active">
              {{headertop.content_center.message}}
              <router-link class="text-black" :to="headertop.content_center.link">
                {{ headertop.content_center.text_link }}
              </router-link>
            </div>
          </div>
        <!-- center header content-->
        
        
        
        <!-- right header content-->
        <div class="header_right flex flex-1 justify-end ">  

          <div v-if="headertop.content_right.location_active" class="header_location flex items-center justify-end p-1 lg:p-2">
            <fa class="icon text-sm mx-2" :icon="['fa', 'location-dot']"></fa>
            <a :href="headertop.content_right.location_link" target="_blank" >{{headertop.content_right.location}}</a>
          </div> 
          
          <div class="header_longue_courency">
            <!-- courency and language dropdown  -->
                <div class="flex items-center">
                <ul v-for="(item, i) in otherMenu" :key="i" class="ddlist flex flex-col p-1 lg:p-2">
                    <li class="flex items-center justify-between w-full rounded-md">
                      <a class="w-full flex " :href="item.url">
                        <div class="px-1">{{ item.text }}</div> 
                        <button @click="activeId = activeId != item._id ? item._id : null">
                            <fa class="w-3 transform" v-if="item.childrens && item.childrens.length > 0"  :icon="['fa', 'angle-down']" :class="[activeId==item._id ? 'rotate-180' : ''] "></fa>
                        </button>
                      </a>
                    </li>
  
                    <transition name="slide">
                      <div class="relative" >
                        <div v-if="item._id == activeId" class="position_top absolute p-2 bg-white shadow-lg z-20 border -left-10" >
                          <div v-for="(item,i) in item.childrens" :key="i" >
                              <a class="p-1 m-1 bg-white flex" :href="item.url">{{item.text}}</a>
                              <ul class="p-2" v-if="item.childrens && item.childrens.length > 0">
                                  <li v-for="(child,ii) in item.childrens" :key="ii">
                                      <a class="p-1 m-1 bg-white flex" :href="child.url">
                                          {{ child.text }}
                                      </a>
                                  </li>
                              </ul>
                            </div>
                        </div>
                      </div>
                    </transition>
  
                    </ul>
                </div>
                <!-- courency and language dropdown  -->
          </div>
        </div>
        <!-- right header content-->
  
      </div>
      <!-- Top Header -->
      <si-app-loader placement="AFTER_TOP_HEADER"/>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        headertop: this.$settings.sections.headertop,
        topheader: this.$settings.sections.topheader,
        show: false,
        activeId: null,
        menu: this.$settings.sections.header.menu,
        otherMenu: [
            {
                _id: "lang",
                text: this.$store.state.language.code,
                active: this.$settings.sections.headertop.content_right.langage_active,
                childrens: this.$settings.store_languages.map(l=> {
                    return {
                        _id: l.code,
                        text: l.name,
                        url: `?lang=${l.code}`
                    }
                })
            },
            {
                _id: "currency",
                text: this.$store.state.currency.code,
                active: this.$settings.sections.headertop.content_right.currency_active,
                childrens: this.$settings.store_currencies.map(c=> {
                    return {
                        _id: c.code,
                        text: c.name,
                        url: `?cur=${c.code}`
                    }
                })
            }
        ].filter(item=> item.active)
      };
    }
  };
  </script>
  
  <style scoped>
  .nav_top_header {
    font-weight: 400;
    color: var(--text-color);
  }
  

  .position_top{
    top: 0.7rem;
  }
  
  @media only screen and (max-width: 1024px) {
    .nav_top {
    flex-direction: column;
    }
    .header_center{
      text-align: center;
    }
  }
  
  </style>
  
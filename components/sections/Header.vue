<template>
<div>
    <si-app-loader placement="BEFORE_HEADER"/>
    <div :class="['bg-white relative py-3 ' , this.$settings.sections.banner.active == false ? ' shadow-sm':'']">
        <div class="flex px-4 justify-between items-center max_container">
          <!--  -->
            <div class="flex h-full justify-start items-center w-1/3">
                <!-- Header Menu -->
                <button @click="$store.state.showHeaderMenu = !$store.state.showHeaderMenu" aria-label="Search button" class="flex lg:hidden flex-col p-2 text-lg hover:text-blue-200 ">
                    <fa class="text-2xl"  :icon="['fa', 'bars']"></fa>
                </button>
                <!--Header Menu Menu  -->


                <!-- Desktop LOGO -->
              <div class="logo justify-center items-center w-1/3 hidden lg:flex">
                  <router-link to="/">
                      <si-image width="70" height="50" property="height" class="h-12 w-full object-contain" :src="section.logo ? section.logo.src : $store.state.defaults.logo" alt="Store logo"/>
                  </router-link>
              </div>
              <!-- Desktop LOGO -->
            </div>

            <!-- Mobile LOGO -->
            <div class="logo justify-center items-center w-1/3 sm:flex lg:hidden">
                <router-link to="/">
                    <si-image width="70" height="50" property="height" class="h-12 w-full object-contain" :src="section.logo ? section.logo.src : $store.state.defaults.logo" alt="Store logo"/>
                </router-link>
            </div>
              <!-- Mobile LOGO -->

            <!-- Header Menu  -->
            <div class="nav-links justify-center items-center text-sm hidden lg:flex">

                <!-- Menu links -->
                <div v-if="menu" class="flex items-center justify-between">
                    <ul v-for="(item, i) in menu.items" :key="i" class="flex flex-col">
                        <li class="flex items-center justify-between p-2">
                            <router-link class="flex" :to="item.url">
                                <div class="px-1">{{ item.text }}</div>
                            </router-link>
                            <button class="link_hover_txt" @click="activeId = activeId != item._id ? item._id : null">
                                <fa class="w-3 transform" v-if="item.childrens && item.childrens.length > 0"  :icon="['fa', 'angle-down']" :class="[activeId==item._id ? 'rotate-180' : ''] "></fa>
                            </button>
                        </li>
                        <transition name="slide">
                            <div class="relative">
                                <div v-if="item._id == activeId" class="position_top absolute p-2 bg-white shadow-lg z-20 border">
                                    <div v-for="(item,i) in item.childrens" :key="i">
                                        <router-link class="p-2 bg-white flex" :to="item.url">{{item.text}}</router-link>
                                        <ul v-if="item.childrens && item.childrens.length > 0">
                                            <li v-for="(child,ii) in item.childrens" :key="ii">
                                                <nuxt-link  class="p-2 mx-3 bg-white flex" :to="child.url">
                                                {{ child.text }}
                                                </nuxt-link>
                                            </li>
                                        </ul>
                                    </div>
                                </div>
                            </div>
                        </transition>
                    </ul>
                </div> 
                <!-- Menu links -->

                <!-- courency and language dropdown  -->
                <div class="flex items-center justify-between">
                    <ul v-for="(item, i) in otherMenu" :key="i" class="flex flex-col">
                        <li class="flex items-center justify-between">
                            <a class="p-2 flex" :href="item.url">
                                <div class="px-1">{{ item.text }}</div>
                                <button @click="activeId = activeId != item._id ? item._id : null">
                                    <fa class="w-3 transform" v-if="item.childrens && item.childrens.length > 0"  :icon="['fa', 'angle-down']" :class="[activeId==item._id ? 'rotate-180' : ''] "></fa>
                                </button>
                            </a>
                        </li>

                        <transition name="slide">
                            <div class="relative" >
                                <div v-if="item._id == activeId" class="position_top absolute p-2 bg-white shadow-lg z-20 border" >
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
            <!-- Header Menu  -->

            <!--  Header left icons -->
            <div class="icons flex items-center justify-end w-1/3">
                <button v-if="$settings.sections.header.icons.search" @click="showSearch=true" aria-label="Search button" class="item p-2  mx-1 text-lg link_hover_txt relative">
                    <si-svg><svg aria-hidden="true" focusable="false" data-prefix="far" data-icon="search" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512" class="w-5 h-5 translate"><path fill="currentColor" d="M508.5 468.9L387.1 347.5c-2.3-2.3-5.3-3.5-8.5-3.5h-13.2c31.5-36.5 50.6-84 50.6-136C416 93.1 322.9 0 208 0S0 93.1 0 208s93.1 208 208 208c52 0 99.5-19.1 136-50.6v13.2c0 3.2 1.3 6.2 3.5 8.5l121.4 121.4c4.7 4.7 12.3 4.7 17 0l22.6-22.6c4.7-4.7 4.7-12.3 0-17zM208 368c-88.4 0-160-71.6-160-160S119.6 48 208 48s160 71.6 160 160-71.6 160-160 160z" class=""></path></svg></si-svg>
                    <!-- <fa :icon="['fa', 'magnifying-glass']"></fa> -->
                    <small v-if="$route.query.search" class="-top-1 -right-1 rounded-full absolute w-1 h-1 p-1 bg-black text-white flex justify-center items-center"></small>
                </button>
                <router-link v-if="$settings.sections.header.icons.account" to="/account/orders" title="Account" class="item hidden md:block p-2 mx-1 text-lg link_hover_txt">
                    <si-svg><svg aria-hidden="true" focusable="false" data-prefix="far" data-icon="user" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512" class="w-5 h-5 translate"><path fill="currentColor" d="M313.6 304c-28.7 0-42.5 16-89.6 16-47.1 0-60.8-16-89.6-16C60.2 304 0 364.2 0 438.4V464c0 26.5 21.5 48 48 48h352c26.5 0 48-21.5 48-48v-25.6c0-74.2-60.2-134.4-134.4-134.4zM400 464H48v-25.6c0-47.6 38.8-86.4 86.4-86.4 14.6 0 38.3 16 89.6 16 51.7 0 74.9-16 89.6-16 47.6 0 86.4 38.8 86.4 86.4V464zM224 288c79.5 0 144-64.5 144-144S303.5 0 224 0 80 64.5 80 144s64.5 144 144 144zm0-240c52.9 0 96 43.1 96 96s-43.1 96-96 96-96-43.1-96-96 43.1-96 96-96z" class=""></path></svg></si-svg>
                    <!-- <fa :icon="['far', 'user']" /> -->
                </router-link>
                <router-link v-if="$settings.sections.header.icons.wishlist" to="/wishlist" title="Wishlist" class="item hidden md:block p-2 mx-1 text-lg link_hover_txt relative">
                    <si-svg><svg aria-hidden="true" focusable="false" data-prefix="far" data-icon="heart" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512" class="w-5 h-5 translate"><path fill="currentColor" d="M458.4 64.3C400.6 15.7 311.3 23 256 79.3 200.7 23 111.4 15.6 53.6 64.3-21.6 127.6-10.6 230.8 43 285.5l175.4 178.7c10 10.2 23.4 15.9 37.6 15.9 14.3 0 27.6-5.6 37.6-15.8L469 285.6c53.5-54.7 64.7-157.9-10.6-221.3zm-23.6 187.5L259.4 430.5c-2.4 2.4-4.4 2.4-6.8 0L77.2 251.8c-36.5-37.2-43.9-107.6 7.3-150.7 38.9-32.7 98.9-27.8 136.5 10.5l35 35.7 35-35.7c37.8-38.5 97.8-43.2 136.5-10.6 51.1 43.1 43.5 113.9 7.3 150.8z" class=""></path></svg></si-svg>    
                    <!-- <fa :icon="['far', 'heart']"></fa> -->
                    <small class="top-1 -right-0 rounded-full absolute w-4 h-4 p-2 bg-red-700 text-white flex justify-center items-center text-xs">{{ $store.state.wishlist.length }}</small>
                </router-link>
                <router-link v-if="$settings.sections.header.icons.cart" to="/cart" title="Cart" id="cart-icon" class="item p-2 mx-1 text-lg relative link_hover_txt">
                    <si-svg><svg aria-hidden="true" focusable="false" data-prefix="far" data-icon="shopping-cart" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 576 512" class="w-5 h-5 translate"><path fill="currentColor" d="M551.991 64H144.28l-8.726-44.608C133.35 8.128 123.478 0 112 0H12C5.373 0 0 5.373 0 12v24c0 6.627 5.373 12 12 12h80.24l69.594 355.701C150.796 415.201 144 430.802 144 448c0 35.346 28.654 64 64 64s64-28.654 64-64a63.681 63.681 0 0 0-8.583-32h145.167a63.681 63.681 0 0 0-8.583 32c0 35.346 28.654 64 64 64 35.346 0 64-28.654 64-64 0-18.136-7.556-34.496-19.676-46.142l1.035-4.757c3.254-14.96-8.142-29.101-23.452-29.101H203.76l-9.39-48h312.405c11.29 0 21.054-7.869 23.452-18.902l45.216-208C578.695 78.139 567.299 64 551.991 64zM208 472c-13.234 0-24-10.766-24-24s10.766-24 24-24 24 10.766 24 24-10.766 24-24 24zm256 0c-13.234 0-24-10.766-24-24s10.766-24 24-24 24 10.766 24 24-10.766 24-24 24zm23.438-200H184.98l-31.31-160h368.548l-34.78 160z" class=""></path></svg></si-svg>
                    <!-- <fa :icon="['fa', 'cart-shopping']"></fa> -->
                    <small class="top-1 -right-0 rounded-full absolute w-4 h-4 p-2 bg-red-700 text-white flex justify-center items-center text-xs">{{ $store.state.cart.length }}</small>
                </router-link>
            </div>
            <!-- Header left icons -->   
        </div>
        <!-- Search popup -->
        <div v-if="showSearch" class="flex items-center justify-center absolute inset-0 z-20">
            <div @click="showSearch=false" class="bg-black bg-opacity-50 flex fixed inset-0"></div>
            <div class="bg-white absolute lg:w-2/5 sm:w-1/2 p-6 rounded">
                <form @submit.prevent="search" class="search flex container bg-gray-50 p-2 rounded-md border border-gray-200" action="/shop?">
                    <button type="button" @click="showSearch=false" aria-label="Search button">
                        <fa class="link_hover_txt" :icon="['fa', 'xmark']"></fa>
                    </button>
                    <input autofocus v-model="q" class="bg-transparent outline-none w-full px-2 text-sm" :placeholder="'Search for products'" type="search" name="q">
                    <button aria-label="Search button">
                        <fa class="link_hover_txt" :icon="['fa', 'magnifying-glass']"></fa>
                    </button>
                </form>
            </div>
        </div>
        <!-- Search popup -->

    </div>
    <si-app-loader placement="AFTER_HEADER"/>
</div>
</template>
<script>
export default {
    data() {
        return {
            showSearch: false,
            iconMenu: null,
            q: this.$route.query.search,
            section: this.$settings.sections.header,
            activeId: null,
            menu: this.$settings.sections.header.menu,
            otherMenu: [
                {
                    _id: "lang",
                    text: this.$store.state.language.code,
                    active: this.$settings.sections.header.icons.header_language,
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
                    active: this.$settings.sections.header.icons.header_currency,
                    childrens: this.$settings.store_currencies.map(c=> {
                        return {
                            _id: c.code,
                            text: c.name,
                            url: `?cur=${c.code}`
                        }
                    })
                }
            ].filter(item=> item.active)
        }
    },
    watch: {
        showSearch(val) {
            if (val) {
                this.$nextTick(()=>{
                    document.querySelector('form.search input').focus();
                })
            }
        }
    },
    methods: {
        search(){
            this.$store.state.search = this.q;
            this.$router.push({ path: '/shop', query: { search: this.q }});
            this.showSearch = false;
        }
    },
}
</script>

<style scoped>
.position_top{
  top: 1.1rem;
}

svg:hover {
  fill: var(--primary);
}
</style>

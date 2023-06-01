<template>
<div class="flex fixed inset-0 z-20" v-if="show">
    <!-- sliderleft shadow -->
    <div v-if="$store.state.showHeaderMenu" :class="$store.state.showHeaderMenu? 'opacity-50' : 'opacity-0'" class="flex transition-all delay-500 fixed inset-0 bg-black" @click="$store.state.showHeaderMenu=false"></div>
    <!-- sliderleft shadow  -->

    <transition name="slideleft">

        <div :class="$store.state.showHeaderMenu ? 'move-in' : 'move-out'" class="transition-all delay-500 max-w-full relative flex flex-col bg-gray-100 w-80">
            
            <!-- close slider left botton -->
            <div class="relative w-full flex justify-end bg-white " >
                <button @click="$store.state.showHeaderMenu=false" aria-label="Search button" class="link_hover_bg absolute top-0 left-80 w-10 h-10 item  bg-black bg-opacity-50">
                    <fa class="text-2xl text-white" :icon="['fa', 'xmark']" />
                </button>
            </div>
            <!-- close slider left botton -->

            <!-- Search-->
                <div class="bg-white p-6 border-b-2">
                    <form @submit.prevent="search" class="search flex container bg-gray-50 p-2 rounded border border-gray-200" action="/shop?">
                        <input autofocus v-model="q" class="bg-transparent outline-none w-full px-2 text-sm" :placeholder="'Search for products'" type="search" name="q">
                        <button aria-label="Search button">
                            <fa class="link_hover_txt" :icon="['fa', 'magnifying-glass']"></fa>
                        </button>
                    </form>
                </div>
            <!-- Search-->

            <!-- Header Menu -->
            <div v-if="menu">
                <ul v-for="(item, i) in menu.items" :key="i" class="flex flex-col">
                    <li class="flex items-center border-b-2 justify-between w-full bg-white hover:bg-gray-100">
                        <router-link class="p-2 m-1 w-full flex text-sm" :to="item.url">{{ item.text }}</router-link>
                        <button class="p-2 mx-1" @click="activeId = activeId != item._id ? item._id : null">
                          <fa class="w-3 transform" v-if="item.childrens && item.childrens.length > 0"  :icon="['fa', 'angle-down']" :class="[activeId==item._id ? 'rotate-180' : ''] "></fa>
                        </button>
                    </li>

                    <transition name="slide">
                    <div v-if="item._id == activeId">
                        <div v-for="(item,i) in item.childrens" :key="i" class="bg-white hover:bg-gray-100 border-b-2">
                            <router-link class="p-2.5 ml-3 bg-white flex text-sm text-gray-500 hover:bg-gray-100" :to="item.url">{{item.text}}</router-link>
                            <ul class="p-2" v-if="item.childrens && item.childrens.length > 0">
                                <li v-for="(child,ii) in item.childrens" :key="ii">
                                    <nuxt-link  class="p-2.5 ml-3 bg-white flex text-sm text-gray-500 hover:bg-gray-100" :to="child.url">
                                        {{ child.text }}
                                    </nuxt-link>
                                </li>
                            </ul>
                        </div>
                    </div>
                    </transition>

                </ul>
            </div>
            <!-- Header Menu -->

            <!-- left slider Icons  -->
            <div class="border-b-2 p-2 bg-white flex justify-center">
                <router-link v-if="$settings.sections.header.icons.account" to="/account/orders" title="Account" class="item p-2 mx-1 border-2 rounded  hover:bg-gray-100">
                    <fa class="text-lg" :icon="['far', 'user']" /> 
                </router-link>
                <router-link v-if="$settings.sections.header.icons.wishlist" to="/wishlist" title="Wishlist" class="item p-2 mx-1 relative border-2 rounded  hover:bg-gray-100">
                    <fa class="text-lg" :icon="['far', 'heart']"></fa>
                    <small class="top-1 -right-0 rounded-full absolute w-4 h-4 p-2 bg-red-700 text-white flex justify-center items-center text-xs">{{ $store.state.wishlist.length }}</small>
                </router-link>
            </div>
            <!-- left slider Icons  -->

            <!-- courency and language dropdown  -->
            <div>
                <ul v-for="(item, i) in otherMenu" :key="i" class="flex flex-col">
                    <li class="flex items-center border-b-2 justify-between w-full bg-white hover:bg-gray-100">
                        <a class="p-2 m-1 w-full flex text-sm" :href="item.url">{{ item.text }}</a>
                        <button class="p-2 mx-1" @click="activeId = activeId != item._id ? item._id : null">
                          <fa class="w-3 transform" v-if="item.childrens && item.childrens.length > 0"  :icon="['fa', 'angle-down']" :class="[activeId==item._id ? 'rotate-180' : ''] "></fa>
                        </button>
                    </li>
                    <transition name="slide">
                    <div v-if="item._id == activeId">
                        <div v-for="(item,i) in item.childrens" :key="i" class="bg-white hover:bg-gray-100 border-b-2">
                            <a class="p-2.5 ml-3 bg-white flex text-sm text-gray-500 hover:bg-gray-100" :href="item.url">{{item.text}}</a>
                            <ul class="p-2" v-if="item.childrens && item.childrens.length > 0">
                                <li v-for="(child,ii) in item.childrens" :key="ii">
                                    <a class="p-2.5 ml-3 bg-white flex text-sm text-gray-500 hover:bg-gray-100" :href="child.url">
                                        {{ child.text }}
                                    </a>
                                </li>
                            </ul>
                        </div>
                    </div>
                    </transition>
                </ul>
            </div>
            <!-- courency and language dropdown  -->
        </div>
    </transition>
</div>
</template>
<script>
export default {
    data() {
        return {
            show: false,
            activeId: null,
            q: this.$route.query.search,
            menu: this.$settings.sections.header.menu,
            otherMenu: [
                {
                    _id: "lang",
                    text: this.$store.state.language.code,
                    active: this.$settings.sections.header.icons.menu_language,
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
                    active: this.$settings.sections.header.icons.menu_currency,
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
        "$route.params": {
            handler(params) {
                this.$store.state.showHeaderMenu = false
            },
            deep: true
        },
        "$store.state.showHeaderMenu"(val){
            if(val){
                this.show = val;
            }else{
                setTimeout(() => {
                    this.show = val;
                },500);
            }
        }
    },
    methods: {
        search(){
            this.$store.state.search = this.q;
            this.$router.push({ path: '/shop', query: { search: this.q }});
            this.showSearch = false;
        }
    }
}
</script>
<style>
[dir='ltr'] .move-out{ transform: translateX(-40em); }
[dir='ltr'] .move-in{ transform: translateX(-20em); }
[dir="rtl"] .move-out{ transform: translateX(40em); }
[dir="rtl"] .move-in{ transform: translateX(20em); }
</style>

<template>
    <div>
      <!--  -->
    <div class="header-banner">
      <div class="page-head">
        <div class="text_container">
          <h2 class="text-white">Collection</h2>
          <span class="text-white text-sm">Home > Shop</span>
        </div>
      </div>
    </div>
    <!--  -->
      <div class="container mt-8">
        <div class="flex mb-2 relative">
          <!--  -->
            <transition name="slideleft">
                <div :class="showSideBar ? 'show':'hide'" class="w-80 fixed hidden h-full top-0 left-0 bottom-0 bg-white z-20">
                    <div class="bg-black bg-opacity-50 fixed block  inset-0" @click="showSideBar=false"></div>
                    <div class="border-r bg-white h-full flex flex-col relative">
                        <div class="w-full flex justify-between block  border-b">
                          <div class="text-2xl py-2">
                            Filter
                          </div>
                            <button @click="showSideBar=false" aria-label="Search button" class="item p-1  rounded-md m-1 ">
                                <svg aria-hidden="true" focusable="false" data-prefix="fal" data-icon="times" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 512" class="w-5 h-5 translate"><path fill="currentColor" d="M193.94 256L296.5 153.44l21.15-21.15c3.12-3.12 3.12-8.19 0-11.31l-22.63-22.63c-3.12-3.12-8.19-3.12-11.31 0L160 222.06 36.29 98.34c-3.12-3.12-8.19-3.12-11.31 0L2.34 120.97c-3.12 3.12-3.12 8.19 0 11.31L126.06 256 2.34 379.71c-3.12 3.12-3.12 8.19 0 11.31l22.63 22.63c3.12 3.12 8.19 3.12 11.31 0L160 289.94 262.56 392.5l21.15 21.15c3.12 3.12 8.19 3.12 11.31 0l22.63-22.63c3.12-3.12 3.12-8.19 0-11.31L193.94 256z" class=""></path></svg>
                            </button>
                        </div>
                        <h3 v-if="$settings.sections.shop.sidebar.collections.active" class="px-2">{{ $settings.sections.shop.sidebar.collections.title }}</h3>
                        <div v-if="$settings.sections.shop.sidebar.collections.active" class="flex flex-col mb-2">
                            <div v-if="loading.collections" class="flex justify-center items-center my-5">
                                <si-loader></si-loader>
                            </div>
                            <div v-for="(item, i) in collections" :key="i" class="flex items-center px-2">
                                <input class="w-4 h-4 mx-1" :checked="params['collections.slug-in'] && params['collections.slug-in'].indexOf(item.slug) >= 0" :id="item.slug" @change="setParams($event, 'collections.slug-in', item.slug)" type="checkbox"/>
                                <label class="cursor-pointer capitalize" :for="item.slug">{{ item.name }}</label>
                            </div>
                        </div>
                        <hr v-if="$settings.sections.shop.sidebar.collections.active">
                        <h3 v-if="$settings.sections.shop.sidebar.prices.active" class="px-2 mt-2">{{ $settings.sections.shop.sidebar.prices.title }}</h3>
                        <div v-if="loading.filters" class="flex justify-center items-center my-5">
                            <si-loader></si-loader>
                        </div>
                        <div v-if="$settings.sections.shop.sidebar.prices.active && filters" class="flex flex-col mb-2" dir="ltr">
                            <si-price-range @change="setParams" :min="filters.prices.min" :max="filters.prices.max" />
                        </div>
                        <hr v-if="$settings.sections.shop.sidebar.prices.active">
                        <h3 class="px-2" v-if="$settings.sections.shop.sidebar.sizes.active">{{ $settings.sections.shop.sidebar.sizes.title }}</h3>
                        <div v-if="$settings.sections.shop.sidebar.sizes.active && loading.filters" class="flex justify-center items-center my-5">
                            <si-loader></si-loader>
                        </div>
                        <div v-if="$settings.sections.shop.sidebar.sizes.active && filters" class="flex flex-wrap mx-2 mb-2">
                            <div v-for="(item, i) in filters.sizes" :key="i" class="flex items-center m-0.5 rounded-md" :class="params['options.values.value1'] && params['options.values.value1'].indexOf(item.value1) >= 0 ? 'bg-primary text-white' : 'bg-gray-200' ">
                                <input hidden :id="item.value1" :checked="params['options.values.value1'] && params['options.values.value1'].indexOf(item.value1) >= 0" @change="setParams($event, 'options.values.value1', item.value1)" type="checkbox"/>
                                <label class="cursor-pointer px-2" :for="item.value1">{{ item.value1 }}</label>
                            </div>
                        </div>
                        <hr v-if="$settings.sections.shop.sidebar.sizes.active">
                        <h3 class="px-2" v-if="$settings.sections.shop.sidebar.colors.active">{{ $settings.sections.shop.sidebar.colors.title }}</h3>
                        <div v-if="$settings.sections.shop.sidebar.colors.active && loading.filters" class="flex justify-center items-center my-5">
                            <si-loader></si-loader>
                        </div>
                        <div v-if="$settings.sections.shop.sidebar.colors.active && filters" class="flex flex-wrap mx-2 mb-2">
                            <div v-for="(item, i) in filters.colors" :key="i" class="flex items-center my-0.5 color-option" :class="params['options.values.value1'] && params['options.values.value1'].indexOf(item.value1) >= 0 ? 'active' : '' ">
                                <input hidden :id="item.value1" :checked="params['options.values.value1'] && params['options.values.value1'].indexOf(item.value1) >= 0" @change="setParams($event, 'options.values.value1', item.value1)" type="checkbox"/>
                                <label class="cursor-pointer rounded-full" :style="`background-color:${item.value2}`" :for="item.value1" :aria-label="item.value1"></label>
                            </div>
                        </div>
                        <hr v-if="$settings.sections.shop.sidebar.colors.active">
                        <h3 class="px-2" v-if="$settings.sections.shop.sidebar.tags.active">{{ $settings.sections.shop.sidebar.tags.title }}</h3>
                        <div v-if="$settings.sections.shop.sidebar.tags.active && loading.filters" class="flex justify-center items-center my-5">
                            <si-loader></si-loader>
                        </div>
                        <div v-if="$settings.sections.shop.sidebar.tags.active && filters" class="flex flex-col mb-2">
                            <div v-for="(tag, i) in filters.tags" :key="i" class="flex items-center px-2">
                                <input class="w-4 h-4 mx-1" :checked="params['tags-in'] && params['tags-in'].indexOf(tag) >= 0" :id="`tag_${tag}`" @change="setParams($event, 'tags-in', tag)" type="checkbox"/>
                                <label class="cursor-pointer capitalize" :for="`tag_${tag}`">{{ tag }}</label>
                            </div>
                        </div>
                        <hr v-if="$settings.sections.shop.sidebar.tags.active">
                        <h3 class="px-2" v-if="$settings.sections.shop.sidebar.brands.active">{{ $settings.sections.shop.sidebar.brands.title }}</h3>
                        <div class="flex flex-col mb-2">
                            <div v-if="$settings.sections.shop.sidebar.brands.active && loading.brands" class="flex justify-center items-center my-5">
                                <si-loader></si-loader>
                            </div>
                            <div v-if="$settings.sections.shop.sidebar.brands.active">
                                <div v-for="(item, i) in brands" :key="i" class="flex items-center px-2">
                                    <input class="w-4 h-4 mx-1" :id="item.slug" :checked="params['brand.slug-in'] && params['brand.slug-in'].indexOf(item.slug) >= 0" @change="setParams($event, 'brand.slug-in', item.slug)" type="checkbox"/>
                                    <label class="cursor-pointer capitalize" :for="item.slug">{{ item.name }}</label>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </transition>
            <!--  -->
            <div class="w-full md:w-4/4">
                <div class="bg-white">
                    <div class="mb-8">
                        <div class="flex justify-between items-center p-2">
                            <div class="flex items-center">
                              <button @click="showSideBar = true" aria-label="Search button" class="flex block items-center flex-col p-2  rounded-md mx-1 ">
                                  <span class="w-6 my-0.5 h-0.5 bg-gray-800"></span>
                                  <span class="w-4 my-0.5 h-0.5 bg-gray-800"></span>
                                  <span class="w-2 my-0.5 h-0.5 bg-gray-800"></span>
                                  <span class="w-1 my-0.5 h-0.5 bg-gray-800"></span>
                              </button>
                              <div>
                                Filter
                              </div>
                            </div>
                            <select class="bg-white py-2 px-3 rounded-3xl shadow outline-none" v-model="params.sort">
                                <option v-for="(sort,i) in sorts" :key="i" :value="sort.field">{{ sort.name }}</option>
                            </select>
                            <div class="flex">
                                <button v-for="(grid,i) in girds" :key="i" @click="gridClass=grid.class" class="flex flex-wrap mx-0.5" :style="`width:${grid.width}px`">
                                    <span v-for="i in grid.number" :key="i" class="flex" :class="grid.class == gridClass ? 'bg-primary':'bg-gray-300'" style="margin:1px;width:4px;height:4px" ></span>
                                </button>
                            </div>
                        </div>
                    </div>
                    <div v-if="loading.products" class="flex justify-center items-center my-5">
                        <si-loader></si-loader>
                    </div>
                    <div v-if="!loading.products && items.length == 0" class="flex justify-center items-center my-5">
                        <h1 class="py-3">{{ $settings.sections.shop.empty_text }}</h1>
                    </div>
                    <div class="flex flex-wrap">
                        <div v-for="(item, i) in items" :key="i" class="p-2" :class="gridClass">
                            <si-product :item="item"></si-product>
                        </div>
                    </div>
                    <div v-if="items.length>0" class="p-2 bg-white  items-center flex justify-center w-full">
                        <button class="primary p-2 flex items-center text-white rounded-3xl" @click="getItems(paginate.current_page-1)">
                            <svg class="h-4 translate" aria-hidden="true" focusable="false" data-prefix="fas" data-icon="chevron-left" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 512"><path fill="currentColor" d="M34.52 239.03L228.87 44.69c9.37-9.37 24.57-9.37 33.94 0l22.67 22.67c9.36 9.36 9.37 24.52.04 33.9L131.49 256l154.02 154.75c9.34 9.38 9.32 24.54-.04 33.9l-22.67 22.67c-9.37 9.37-24.57 9.37-33.94 0L34.52 272.97c-9.37-9.37-9.37-24.57 0-33.94z"></path></svg>
                            <span>&ensp;</span>
                            <span>{{ $settings.sections.shop.pagination.prev_text }}</span>
                        </button>
                        <span>&ensp;</span>
                        <span>{{paginate.current_page}}/{{paginate.last_page}}</span>
                        <span>&ensp;</span>
                        <button class="primary p-2 flex items-center text-white rounded-3xl" @click="getItems(paginate.current_page+1)">
                            <span>{{ $settings.sections.shop.pagination.next_text }}</span>
                            <span>&ensp;</span>
                            <svg class="h-4 translate" aria-hidden="true" focusable="false" data-prefix="fas" data-icon="chevron-right" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 512"><path fill="currentColor" d="M285.476 272.971L91.132 467.314c-9.373 9.373-24.569 9.373-33.941 0l-22.667-22.667c-9.357-9.357-9.375-24.522-.04-33.901L188.505 256 34.484 101.255c-9.335-9.379-9.317-24.544.04-33.901l22.667-22.667c9.373-9.373 24.569-9.373 33.941 0L285.475 239.03c9.373 9.372 9.373 24.568.001 33.941z"></path></svg>
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
  </div>
</template>
<script>
export default {
    data() {
        return {
            loading: {
                products: true,
                filters: true,
                collections: true,
                brands: true,
            },
            query: {},
            param: [],
            filters: null,
            showSideBar: false,
            gridClass: 'w-full md:w-1/2 lg:w-1/3',
            items: [],
            collections:[],
            brands: [],
            paginate: { page: 1, limit: this.$settings.sections.shop.pagination.limit, total: 12 },
            params: { page: 1, search: this.$route.query.search, limit: this.$settings.sections.shop.pagination.limit, 'collections.slug-in': [], sort: { createdAt: -1 } },
            lastParams: { page: 1, search: this.$route.query.search, limit: this.$settings.sections.shop.pagination.limit, 'collections.slug-in': [], sort: { createdAt: -1 } },
            sorts: [
                { field: { 'price.salePrice': 1 }, name: this.$settings.sections.shop.sorts.price_asc },
                { field: { 'price.salePrice': -1 }, name: this.$settings.sections.shop.sorts.price_desc },
                { field: { 'review.rating': -1 }, name: this.$settings.sections.shop.sorts.rating_desc },
                { field: { 'review.rating': 1 }, name: this.$settings.sections.shop.sorts.rating_asc },
                { field: { 'name': 1 }, name: this.$settings.sections.shop.sorts.name_asc },
                { field: { 'name': -1 }, name: this.$settings.sections.shop.sorts.name_desc },
                { field: { createdAt: -1 }, name: this.$settings.sections.shop.sorts.newest },
                { field: { createdAt: 1 }, name: this.$settings.sections.shop.sorts.oldest }
            ],
            girds: [
                { number: 6, width: 16, class: 'w-full md:w-1/2 lg:w-1/2' },
                { number: 9, width: 21, class: 'w-full md:w-1/2 lg:w-1/3' },
                { number: 12, width: 26, class: 'w-1/2 md:w-1/3 lg:w-1/4' }
            ]
        }
    },
    watch: {
        params: {
            handler(val) {
                if(JSON.stringify(val) !== JSON.stringify(this.lastParams)){
                    this.getItems();
                }
            },
            deep: true
        },
        "$route.query.search"(val){
            this.$set(this.params, 'search', val);
        }
    },
    async fetch(){
        this.$store.state.seo.title = this.$settings.sections.shop.title + ' - ' + this.$settings.store_name;
        this.$store.state.seo.description = this.$settings.sections.shop.description || this.$settings.store_description;
        if(this.$route.params.slug){
            this.param = this.$route.params.slug.split(',');
            this.$route.params.slug.split(',').forEach(item => {
                this.params['collections.slug-in'].push(item);
            });
        }
        for (const key in this.$route.query) {
            if(!this.$route.query[key]) continue;
            switch (key) {
                case 'price-from': this.$set(this.params, 'price.salePrice-from', this.$route.query[key]);break;
                case 'price-to': this.$set(this.params, 'price.salePrice-to', this.$route.query[key]);break;
                case 'colors-size': this.$set(this.params, 'options.values.value1', this.$route.query[key].split(','));break;
                case 'tags': this.$set(this.params, 'tags-in', this.$route.query[key].split(','));break;
                case 'brands': this.$set(this.params, 'brand.slug-in', this.$route.query[key].split(','));break;
                case 'page': this.$set(this.params, 'page', this.$route.query[key]);break;
            }
        }
        this.lastParams = this.params;
        await this.getFilters();
        await this.getItems();
        await this.getCollections();
        await this.getBrands();
    },
    methods: {
        setParams(e, key, value){
            if(key.indexOf('price') >= 0 || key.indexOf('page') >= 0){
                this.$set(this.params,key, e.target.value);
                return false;
            }else{
                if(e.target.checked) {
                    if(!this.params[key]) this.params[key] = this.$set(this.params, key, []);
                    this.params[key].push(value);
                } else {
                    this.params[key] = this.params[key].filter(item => item !== value);
                }
            }
            for (const key in this.params) {
                switch(key){
                    case 'collections.slug-in': this.param = this.params[key];break;
                    case 'price.salePrice-from': this.query['price-from'] = this.params[key];break;
                    case 'price.salePrice-to': this.query['price-to'] = this.params[key];break;
                    case 'options.values.value1': this.query['colors-size'] = this.params[key];break;
                    case 'tags-in': this.query['tags'] = this.params[key];break;
                    case 'brand.slug-in': this.query['brands'] = this.params[key];break;
                    case 'page': this.query['page'] = [this.params[key]];break;
                }
            }
            let url = `/shop/`;
            url += this.param.length > 0 ? [...new Set(this.param)].join(',') : '';
            for (const key in this.query) {
                url += url.indexOf('?') == -1 ? '?' : '&';
                if(typeof this.query[key] == 'object'){
                    url += `${key}=${this.query[key].join(',')}`;
                }else url += `${key}=${this.query[key]}`;
            }
            window.history.pushState({}, '', url);
        },
        async getFilters(){
            this.filters = null;
            this.loading.filters = true;
            try{
                const { data } = await this.$storeino.products.filters({});
                this.filters = data;
            }catch(e){
                console.log({e});
            }
            this.loading.filters = false;
        },
        async getCollections(){
            this.collections = [];
            this.loading.collections = true;
            try{
                const { data } = await this.$storeino.collections.search({});
                this.collections = data.results;
            }catch(e){
                console.log({e});
            }
            this.loading.collections = false;
        },
        async getBrands(){
            this.brands = [];
            this.loading.brands = true;
            try{
                const { data } = await this.$storeino.brands.search({});
                this.brands = data.results;
            }catch(e){
                console.log({e});
            }
            this.loading.brands = false;
        },
        async getItems(page=null){
            if(page != null) this.setParams({target:{value: page}}, 'page', page);
            this.items = [];
            this.loading.products = true;
            try{
                this.params.search = this.$route.query.search;
                this.params.page = page || this.paginate.current_page;
                this.params.limit = this.$settings.sections.shop.pagination.limit;
                this.lastParams = this.$tools.copy(this.params);
                const {data} = await this.$storeino.products.search(this.params);
                this.items = data.results;
                this.paginate = data.paginate;
            }catch(e){
                console.log({e});
            }
            this.loading.products = false;
        },
    },
}
</script>
<style>
.color-option label{
    width: 24px;
    height: 24px;
    margin-left: 4px;
    margin-right: 4px;
    box-shadow: 0 0 0px 2px rgb(230, 230, 230);
}
.color-option.active label{
    color: transparent;
    box-shadow: 0 0 0px 2px white, 0 0 0px 4px var(--primary-color);
    margin-left: 6px;
    margin-right: 6px;
    width: 20px;
    height: 20px;
}

.slideleft-enter-active {
  transition-duration: 0.3s;
  transition-timing-function: ease-in;
}

.slideleft-leave-active {
  transition-duration: 0.3s;
  transition-timing-function: cubic-bezier(0, 1, 0.5, 1);
}

.slideleft-enter-to, .slideleft-leave {
    width: 100%;
}

.slideleft-enter, .slideleft-leave-to {
    width: 0%;
}
/* Media screen mobile */
/* @media (max-width: 768px){ */
    .show {
        display: block !important;
    }
/* } */


  .header-banner {
  position: relative;
    padding: 50px 0 50px;
    background-attachment: scroll;
    background-position: center center;
    background-repeat: no-repeat;
    background-size: cover;
    background-image: url("https://cdn.shopify.com/s/files/1/0616/9480/4174/files/bg-heading_1512x.jpg?v=1652069957");
}

.page-head {
  text-align: center;
  overflow: hidden;
}
.page-head:before {
    content: "";
    position: absolute;
    background-color: #000000;
    opacity: 0.54;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
}

.text_container {
  position: relative;
  z-index: 2;
}

.text_container h2{
    font-size: 20px;
    font-weight: 500;
    line-height: 20px;
    text-transform: uppercase;
}

.primary {
  color: #fff;
  background-color: var(--primary);
  border-color: var(--primary);
}
</style>

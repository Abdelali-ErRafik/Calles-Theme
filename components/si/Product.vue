<template>
<div class="flex flex-col relative favorite_hover mb-5">
  <!--  -->
    <div v-if="$settings.sections.products.add_to_wishlist.active">
        <button v-if="$store.state.wishlist.find(i=>i._id==item._id)" @click="removeFromWishlist" title="Wishlist" class="item absolute text-black  z-10 top-0 left-0 m-2 p-2  rounded-md mx-1">
            <svg aria-hidden="true" focusable="false" data-prefix="far" data-icon="heart" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512" class="w-5 h-5 translate text-pink-600 hover:text-blue-400"><path fill="currentColor" d="M462.3 62.6C407.5 15.9 326 24.3 275.7 76.2L256 96.5l-19.7-20.3C186.1 24.3 104.5 15.9 49.7 62.6c-62.8 53.6-66.1 149.8-9.9 207.9l193.5 199.8c12.5 12.9 32.8 12.9 45.3 0l193.5-199.8c56.3-58.1 53-154.3-9.8-207.9z"></path></svg>
        </button>
        <button v-else @click="addToWishlist" title="Wishlist" class="favorite_icon  lg:hidden item absolute z-10 top-0 left-0 m-2 p-2 rounded-md text-black mx-1">
            <svg aria-hidden="true" focusable="false" data-prefix="far" data-icon="heart" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512" class="w-5 h-5 translate hover:text-blue-400"><path fill="currentColor" d="M458.4 64.3C400.6 15.7 311.3 23 256 79.3 200.7 23 111.4 15.6 53.6 64.3-21.6 127.6-10.6 230.8 43 285.5l175.4 178.7c10 10.2 23.4 15.9 37.6 15.9 14.3 0 27.6-5.6 37.6-15.8L469 285.6c53.5-54.7 64.7-157.9-10.6-221.3zm-23.6 187.5L259.4 430.5c-2.4 2.4-4.4 2.4-6.8 0L77.2 251.8c-36.5-37.2-43.9-107.6 7.3-150.7 38.9-32.7 98.9-27.8 136.5 10.5l35 35.7 35-35.7c37.8-38.5 97.8-43.2 136.5-10.6 51.1 43.1 43.5 113.9 7.3 150.8z" class=""></path></svg>
        </button>
    </div>
    <!--  -->
    <div v-if="discount" class="p-2 h-10 rounded-br-lg flex items-center justify-center bg-red-700 text-white absolute top-0 left-0 z-10">
        <b>-{{discount.value}} {{ discount.type == 'percentage' ? '%' : this.$store.state.currency.symbol }}</b>
    </div>
    <!--  -->
    <div class="h-full flex relative overflow-hidden">
        <div class="w-full flex flex-col h-full">
          <!--  -->
            <div class="image_box w-100 pb-full relative zoom overflow-hidden">
              <!--  -->
                <div class="quick_btn absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 z-40 flex flex-col items-center justify-center lg:hidden">
                    <div @click="showed=true" class="btn_view bg-white text-black rounded-2xl text-sm w-28 h-8 px-4 py-1 mb-1 flex justify-center items-center " href="#">
                      <div>Quick View</div>
                      <div  class="text-white hidden"><fa class="icon" :icon="['fa', 'eye']"></fa></div>
                    </div>
                    <div @click="filpped=true" class="btn_shop bg-white text-black rounded-2xl text-sm w-28 h-8  px-4 py-1 flex justify-center items-center" href="#">
                      <div>{{ $settings.sections.products.add_to_cart.text }}</div>
                        <div v-if="$settings.sections.products.add_to_cart.active"  class="flex ai-c p-2 justify-center text-white hidden">
                          <span class="w-full h-full">
                            <fa class="icon" :icon="['fa', 'cart-shopping']"></fa>
                            </span>
                      </div>
                    </div>
                </div>
                <!--  -->
                <nuxt-link :to="`/products/${item.slug}`" :title="item.name" :aria-label="item.name">
                    <si-image  width="400" height="400" class="image_zoom h-full w-full absolute inset-0 object-cover" :src="item.images.length > 0 ? item.images[0].src : null" :alt="item.name"/>
                </nuxt-link>
            </div>
            <!--  -->
            <div class="h-full">
                <nuxt-link :to="`/products/${item.slug}`">
                    <h3 class="mt-3 text-sm font-medium">{{ item.name }}</h3>
                </nuxt-link>
            </div>
            <!--  -->

            <!-- <hr class="my-1"> -->
            <!-- <div class="flex items-center justify-center" v-if="$settings.sections.products.show_reviews">
                <div class="mb-1 flex">
                    <span v-for="(star,i) in 5" :class="star <= item.review.rating ? 'text-yellow-500 ': 'text-gray-400'" :key="i">
                        <svg aria-hidden="true" focusable="false" data-prefix="fas" data-icon="star" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 576 512" class="w-6 h-6 translate"><path fill="currentColor" d="M259.3 17.8L194 150.2 47.9 171.5c-26.2 3.8-36.7 36.1-17.7 54.6l105.7 103-25 145.5c-4.5 26.3 23.2 46 46.4 33.7L288 439.6l130.7 68.7c23.2 12.2 50.9-7.4 46.4-33.7l-25-145.5 105.7-103c19-18.5 8.5-50.8-17.7-54.6L382 150.2 316.7 17.8c-11.7-23.6-45.6-23.9-57.4 0z" class=""></path></svg>
                    </span>
                </div>
                <span class="text-sm text-gray-600" key="count">({{ item.review.reviews.length }})</span>
            </div> -->
            <!--  -->
            <si-product-price :type="item.type" :price="item.price" :variants="item.variants"></si-product-price>
            <!--  -->
            <!-- <button v-if="$settings.sections.products.add_to_cart.active" @click="filpped=true" class="flex ai-c p-2 justify-center bg-white text-black">
                <svg aria-hidden="true" focusable="false" data-prefix="far" data-icon="shopping-cart" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 576 512" class="w-6 h-6 translate"><path fill="currentColor" d="M551.991 64H144.28l-8.726-44.608C133.35 8.128 123.478 0 112 0H12C5.373 0 0 5.373 0 12v24c0 6.627 5.373 12 12 12h80.24l69.594 355.701C150.796 415.201 144 430.802 144 448c0 35.346 28.654 64 64 64s64-28.654 64-64a63.681 63.681 0 0 0-8.583-32h145.167a63.681 63.681 0 0 0-8.583 32c0 35.346 28.654 64 64 64 35.346 0 64-28.654 64-64 0-18.136-7.556-34.496-19.676-46.142l1.035-4.757c3.254-14.96-8.142-29.101-23.452-29.101H203.76l-9.39-48h312.405c11.29 0 21.054-7.869 23.452-18.902l45.216-208C578.695 78.139 567.299 64 551.991 64zM208 472c-13.234 0-24-10.766-24-24s10.766-24 24-24 24 10.766 24 24-10.766 24-24 24zm256 0c-13.234 0-24-10.766-24-24s10.766-24 24-24 24 10.766 24 24-10.766 24-24 24zm23.438-200H184.98l-31.31-160h368.548l-34.78 160z"></path></svg>
                <span>&ensp;</span>
                <span class="w-full">{{ $settings.sections.products.add_to_cart.text }}</span>
            </button> -->
            <!--  -->
        </div>
        <!--  -->
        <!--  -->
        <transition name="flip">
        <div v-if="filpped" class="w-full flex flex-col h-full bg-white border-2 shadow absolute z-50 ">
          <!--  -->
            <button class="close-button" @click="filpped=false" >
                <i class="close-icon"></i>
            </button>
              <!--  -->
            <div class="flex flex-col justify-evenly items-center h-full overflow-auto">
              <!--  -->
              <div class="flex justify-around items-center">
                <nuxt-link :to="`/products/${item.slug}`" :title="item.name" :aria-label="item.name">
                    <si-image  width="80" height="160" class="object-cover" :src="item.images.length > 0 ? item.images[0].src : null" :alt="item.name"/>
                </nuxt-link>
                <!--  -->
                <div class="flex flex-col justify-evenly">
                  <nuxt-link v-if="item.options.length < 3" class="text-center" :to="`/products/${item.slug}`">
                      <h3 class="ml-2 text-sm">{{ item.name }}</h3>
                  </nuxt-link>
                    <si-product-price class="ml-2 text-sm " v-if="item.type == 'simple'" :type="item.type" :originalPrice="item.originalPrice" :price="item.price" :variants="item.variants"></si-product-price>
                </div>
                <!--  -->
              </div>
                <!--  -->
                <div class="flex items-center justify-center" v-if="item.type == 'simple' && $settings.sections.products.show_reviews">
                    <div class="mb-1 flex">
                        <span v-for="(star,i) in 5" :class="star <= item.review.rating ? 'text-yellow-500 ': 'text-gray-400'" :key="i">
                            <svg aria-hidden="true" focusable="false" data-prefix="fas" data-icon="star" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 576 512" class="w-6 h-6 translate"><path fill="currentColor" d="M259.3 17.8L194 150.2 47.9 171.5c-26.2 3.8-36.7 36.1-17.7 54.6l105.7 103-25 145.5c-4.5 26.3 23.2 46 46.4 33.7L288 439.6l130.7 68.7c23.2 12.2 50.9-7.4 46.4-33.7l-25-145.5 105.7-103c19-18.5 8.5-50.8-17.7-54.6L382 150.2 316.7 17.8c-11.7-23.6-45.6-23.9-57.4 0z" class=""></path></svg>
                        </span>
                    </div>
                    <span class="text-sm text-gray-600" key="count">({{ item.review.reviews.length }})</span>
                </div>
                <!--  -->
                <div class="w-full">
                    <si-product-variants v-if="item.type=='variable'" showPrice :options="item.options" :variants="item.variants" @selected="variantSelected"></si-product-variants>
                    <hr v-if="item.type=='variable'" class="my-1">
                    <div class="flex justify-center">
                        <si-product-quantity @selected="quantitySelected" :quantity="quantity"></si-product-quantity>
                    </div>
                </div>
              <!--  -->
              <button v-if="$settings.sections.products.add_to_cart.active" @click="addToCart" class="btn_confirm rounded-full flex ai-c p-2 justify-center text-white click-effect">
                  <svg aria-hidden="true" focusable="false" data-prefix="far" data-icon="shopping-cart" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 576 512" class="w-6 h-6 translate"><path fill="currentColor" d="M551.991 64H144.28l-8.726-44.608C133.35 8.128 123.478 0 112 0H12C5.373 0 0 5.373 0 12v24c0 6.627 5.373 12 12 12h80.24l69.594 355.701C150.796 415.201 144 430.802 144 448c0 35.346 28.654 64 64 64s64-28.654 64-64a63.681 63.681 0 0 0-8.583-32h145.167a63.681 63.681 0 0 0-8.583 32c0 35.346 28.654 64 64 64 35.346 0 64-28.654 64-64 0-18.136-7.556-34.496-19.676-46.142l1.035-4.757c3.254-14.96-8.142-29.101-23.452-29.101H203.76l-9.39-48h312.405c11.29 0 21.054-7.869 23.452-18.902l45.216-208C578.695 78.139 567.299 64 551.991 64zM208 472c-13.234 0-24-10.766-24-24s10.766-24 24-24 24 10.766 24 24-10.766 24-24 24zm256 0c-13.234 0-24-10.766-24-24s10.766-24 24-24 24 10.766 24 24-10.766 24-24 24zm23.438-200H184.98l-31.31-160h368.548l-34.78 160z"></path></svg>
                  <span>&ensp;</span>
                  <span class="w-full" v-if="added">{{ $settings.sections.products.added_text }}</span>
                  <span class="w-full" v-else>{{ $settings.sections.products.confirm_text }}</span>
              </button>
            </div>
            <!--  -->
        </div>
        </transition>
        <!--  -->
        <!--  -->
        <transition name="flip">
        <div v-if="showed" class="w-full flex flex-col h-full bg-white border-2 shadow absolute z-50">
          <!--  -->
            <button class="close-button" @click="showed=false">
                <i class="close-icon"></i>
            </button>
              <!--  -->
            <div class="flex flex-col justify-center items-center h-full overflow-auto">
              <!--  -->
              <div class="flex justify-around items-center">
                <nuxt-link :to="`/products/${item.slug}`" :title="item.name" :aria-label="item.name">
                    <si-image  width="80" height="160" class="object-cover" :src="item.images.length > 0 ? item.images[0].src : null" :alt="item.name"/>
                </nuxt-link>
                <!--  -->
                <div class="flex flex-col justify-evenly">
                  <nuxt-link v-if="item.options.length < 3" class="text-center" :to="`/products/${item.slug}`">
                      <h3 class="ml-2 text-sm">{{ item.name }}</h3>
                  </nuxt-link>
                    <si-product-price class="ml-2 text-sm " v-if="item.type == 'simple'" :type="item.type" :originalPrice="item.originalPrice" :price="item.price" :variants="item.variants"></si-product-price>
                </div>
                <!--  -->
              </div>
              <!--  -->
            </div>
            <!--  -->
        </div>
        </transition>
        <!--  -->
        <!--  -->
    </div>
    <!--  -->
</div>
</template>
<script>
export default {
    props: {
        upsell: { type: Object, default: null },
        item: Object
    },
    async fetch(){
        if(this.discount){
            if(this.item.type == 'simple'){
                this.item.originalPrice = this.$tools.copy(this.item.price);
                if(this.discount.type == 'percentage'){
                        this.item.price.salePrice = this.item.price.salePrice - (this.item.price.salePrice * this.discount.value / 100)
                        this.item.price.comparePrice = this.item.price.comparePrice - (this.item.price.comparePrice * this.discount.value / 100)
                }else{
                    this.item.price.salePrice = this.item.price.salePrice - this.discount.value
                    this.item.price.comparePrice = this.item.price.comparePrice - this.discount.value
                }
            }else{
                this.item.variants.forEach(variant => {
                    variant.originalPrice = this.$tools.copy(variant.price)
                    if(this.discount.type == 'percentage'){
                        variant.price.salePrice = variant.price.salePrice - (variant.price.salePrice * this.discount.value / 100)
                        variant.price.comparePrice = variant.price.comparePrice - (variant.price.comparePrice * this.discount.value / 100)
                    }else{
                        variant.price.salePrice = variant.price.salePrice - this.discount.value
                        variant.price.comparePrice = variant.price.comparePrice - this.discount.value
                    }
                });
            }
        }
    },
    data() {
        return {
            showed: false,
            filpped: false,
            added: false,
            variant: this.item.type == 'variant' ? this.item.variants[0] : null,
            quantity: this.item.quantity,
            price: { salePrice: 0, comparePrice: 0 },
            discount: this.upsell ? this.upsell.discount : null
        }
    },
    methods: {
        addToCart(ev) {
            // Call add to cart event
            console.log('items', this.item)
            let item = {
                _id: this.item._id,
                quantity: this.quantity.value ? this.quantity.value : this.item.quantity.default,
                price: this.variant?this.variant.price.salePrice : this.item.price.salePrice,
                variant: this.variant ? { _id: this.variant._id } : null,
                upsell: this.upsell
            };
            this.$tools.call('ADD_TO_CART', item);
            this.$tools.toast(this.$settings.sections.alerts.added_to_cart);
            this.added = true;
            if(this.$settings.sections.products.add_to_cart_to_checkout){
                setTimeout(() => {
                        window.location.href = '/checkout2';
                }, 500);
            }
            setTimeout(() => {
                this.added = false;
            }, 2000);
             this.$storeino.fbpx('AddToCart',{
               content_name: this.item.name,
               content_ids: [this.item._id],
               content_type: "product",
               value: this.variant?this.variant.price.salePrice : this.item.price.salePrice,
               currency: this.$store.state.currency && this.$store.state.currency.code ? this.$store.state.currency.code : "USD"
            })
        },
        variantSelected(variant){
            this.variant = variant;
            this.quantitySelected(this.item.quantity.value);
        },
        quantitySelected(quantity){
            this.item.quantity.value = quantity;
            if(this.variant){
                this.price.salePrice = this.variant.price.salePrice * quantity;
                this.price.comparePrice = this.variant.price.comparePrice * quantity;
            }else{
                this.price.salePrice = this.item.price.salePrice * quantity;
                this.price.comparePrice = this.item.price.comparePrice * quantity;
            }
        },
        addToWishlist(){
            this.$tools.call('ADD_TO_WISHLIST', this.item);
            this.$tools.toast(this.$settings.sections.alerts.added_to_wishlist);
        },
        removeFromWishlist(){
            this.$tools.call('REMOVE_FROM_WISHLIST', this.item);
            this.$tools.toast(this.$settings.sections.alerts.removed_from_wishlist);
        }
    },
}
</script>
<style scoped>
    .close-button{
        position: absolute;
        right: 1px;
        top: 1px;
    }
    .close-icon{
        display: block;
        width: 30px;
        height: 30px;
        overflow: hidden;
        position: relative;
        border-radius: 5px;
        background: transparent;
        border: 7px solid transparent;
    }
    .close-icon::before, .close-icon::after{
        content: '';
        width: 40px;
        height: 2px;
        display: block;
        position: absolute;
        background-color: var(--primary);
    }
    .close-icon::before{
        transform: rotate(45deg);
        top: 14px;
        left: -5px;
    }
    .close-icon::after{
        transform: rotate(-45deg);
        top: 0px;
        left: -5px;
    }
    .icon-star::before {
        content: '\e918';
        font-size: 14px !important;
    }
    .flip-enter-active, .flip-leave-active {
        transition: transform .5s;
    }
    .flip-enter, .flip-leave-to /* .flip-leave-active below version 2.1.8 */ {
        transform: translateX(-100%);
    }
    /*  */
    .favorite_hover:hover .favorite_icon {
      display: block;
    }
    /* Image Zom in */
    .image_zoom {
    transition: transform 2s;
    }

    .image_zoom:hover{
    transform: scale(1.5);
    }
    /*  */
    .image_box {
      height: 300px;
    }
    .image_box:hover .quick_btn {
      display: flex;
    }

    .btn_view:hover,
    .btn_shop:hover {
      background-color: #222;
    }

    .btn_view:hover div:nth-child(1) ,
    .btn_shop:hover div:nth-child(1) {
      display: none;
    }

    .btn_view:hover div:nth-child(2) ,
    .btn_shop:hover div:nth-child(2) {
      display: flex;
    }

    .btn_confirm {
      background-color: var(--primary);
    }

    .btn_confirm:hover {
      background-color: #222;
      color: #fff;
    }

    .fliip:hover {
      z-index: 1000000;
    }
</style>

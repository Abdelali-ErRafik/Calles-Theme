<template>
<div>
  <div class="header-banner" :style="`background-image:url('${item.image ? item.image.url : null }')`">
    <div class="page-head">
      <div class="text_container">
        <h2 class="text-white">{{ item.title }}</h2>
        <span class="text-white text-sm">May 11, 2022</span>
      </div>
    </div>
  </div>

    <div class="container my-2 bg-white">
        <div v-if="loading" class="flex justify-center items-center my-5">
            <si-loader></si-loader>
        </div>
        <div v-if="item" class="">
            <!-- <h1 class="m-2">{{ item.title }}</h1>
            <hr class="m-0"> -->
            <div class="h-screen shadow p-2 mt-10 bg-cover bg-center relative " :style="`background-image:url('${item.image ? item.image.url : null }')`"></div>

            <p class="mt-5"><small>{{ item.excerpt }}</small></p>

            <div  v-if="item" class="bg-gray-100 rounded-md p-2 my-3 " id="description" v-html="item.content"></div>
            <div class="flex items-center">
                <div class=" flex w-full border-b border-gray-200"></div>
                <h3 class="share whitespace-nowrap p-2">{{ $settings.sections.post.share_buttons.title }}</h3>
                <div class=" flex w-full border-b border-gray-200"></div>
            </div>
            <div class=" flex justify-center">
                <div v-for="item in socialMedia.filter(s=>$settings.sections.post.share_buttons[s.name])" :key="item.name" class="h-12 m-2 flex items-center justify-center">
                    <a class="h-full flex" :href="item.url" target="_blank" rel="noopener noreferrer">
                        <si-image class="h-10 w-10" width="40" height="40" :src="item.image" :alt="item.name"/>
                    </a>
                </div>
            </div>
        </div>
        <hr>
        <div v-if="item" class="related">
            <sections-related-posts :item="item"/>
        </div>
    </div>
  </div>
</template>
<script>
export default {
    data() {
        return {
            loading: true,
            item: null,
            socialMedia: [
                {
                    name: 'whatsapp',
                    url: 'https://api.whatsapp.com/send?text={title}%20{url}',
                    image: 'https://storeno.b-cdn.net/themes/palest/whatsapp.png'
                },
                {
                    name: 'facebook',
                    image: 'https://storeno.b-cdn.net/themes/palest/facebook.png',
                    url: 'https://www.facebook.com/sharer.php?u={url}'
                },
                {
                    name: 'twitter',
                    url: 'https://twitter.com/intent/tweet?url={url}&text={title}',
                    image: 'https://storeno.b-cdn.net/themes/palest/twitter.png'
                },
                {
                    name: 'linkedin',
                    url: 'https://www.linkedin.com/sharing/share-offsite/?url={url}',
                    image: 'https://storeno.b-cdn.net/themes/palest/linkedin.png'
                }
            ]
        }
    },
    async fetch(){
        try{
            const { slug } = this.$route.params;
            const { data } = await this.$storeino.pages.get({ slug, type: 'POST' })
            this.item = data;

            this.$store.state.seo.title = this.item.title + ' - ' + this.$settings.store_name;
            this.$store.state.seo.description = this.item.excerpt || this.$settings.store_description;
            if(this.item.image){ this.$store.state.seo.image = this.item.image.url; }


            let url = `https://${this.$store.state.domain}/posts/${slug}`;
            for (const button of this.socialMedia) {
                button.url = button.url.replace(/\{title\}/gi, this.item.title).replace(/\{url\}/gi, url);
            }
            this.loading = false;
        }catch(e){
            console.log({e});
            // Redirect to error page if product not exists
            this.$nuxt.error({ statusCode: 404, message: 'post_not_found' })
        }
    },
    mounted() {
      this.$storeino.fbpx('PageView')
    }
}
</script>


<style scoped >
  .header-banner {
  position: relative;
    padding: 50px 0 50px;
    background-attachment: scroll;
    background-position: center center;
    background-repeat: no-repeat;
    background-size: cover;
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

.share {
  font-size: 18px;
  font-weight: 700;
}
</style>

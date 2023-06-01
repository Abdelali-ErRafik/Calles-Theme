<template>
    <div>
    <div class="m-2 mt-10">
        <h2 class="title flex justify-center items-center">
            <span>{{ instagram.title }}</span>
            </h2>
            <h4 class="text-center italic">{{instagram.description }}</h4>
        </div>
 
        <div class="images  w-full py-4 ">
          <div class="image h-64" v-for="(key,i) in Object.keys(instagram.items)" :key="i">
                <si-image  class="object-cover bg-center w-full h-full" :src="instagram.items[key].image ? instagram.items[key].image.src : null" alt="instagram picture"   />
          </div>
        </div>
    </div>
</template>

<script>

export default {
    data() {
        return {
            instagram: this.$settings.sections.instagram,
        }
    }
}
</script>
    
<style scoped>
.title {
    font-size: 1.5rem;
    font-weight: 600;
}

.sub_title {
    font-size: 1.2rem;
    font-weight: 400;
    font-family: Libre Baskerville;
}

.title:before,
.title:after {
    content: "";
    display: inline-block;
    width: 60px;
    height: 2px;
    background: #222;
    font-weight: 600;
}
.title>span {
    margin: 0 30px;
}

.btn_link:hover {
    border-color: #56cfe1;
    background-color: #56cfe1;
    color: #fff;
}

.btn_link:hover>span {
    color: #fff;
}

/* .images div:first-child {
    display: none;
} */

.images {
    display: grid;
    grid-template-columns: repeat(6, 1fr);
    justify-content: center;
    align-items: center;
}


@media screen and (max-width: 1025px){
    .images {
    grid-template-columns: repeat(3, 1fr);
   }

    .images .image {
        padding: 0.5rem;
    }
}

@media screen and (max-width: 767px){
    .images {
    grid-template-columns: repeat(2, 1fr);
   }

    .title {
        font-size: 1.1rem;
    }

    .sub_title {
    font-size: 0.9rem;
}

}



</style>
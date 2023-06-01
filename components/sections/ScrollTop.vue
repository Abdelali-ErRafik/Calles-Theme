<template>
    <!-- Scroll to top -->
    <div v-if="showButton" @click="scrollToTop" class="link_hover_bg hover:text-white inline-flex items-center justify-center fixed bottom-16 right-4  lg:bottom-14 lg:right-10 h-11 w-11 z-50 rounded shadow cursor-pointer bg-white">
        <fa :icon="['fa', 'angle-up']" ></fa>
    </div>
    <!-- Scroll to top -->
</template>
  
<script>
  export default {
    data() {
      return {
        showButton: false
      }
    },
    mounted() {
      window.addEventListener('scroll', this.handleScroll);
    },
    methods: {
      handleScroll() {
        if (window.pageYOffset > 100) {
          this.showButton = true;
        } else {
          this.showButton = false;
        }
      },
      scrollToTop() {
        window.scrollTo({
          top: 0,
          behavior: 'smooth'
        });
      }
    }
  }
</script>
  
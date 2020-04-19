<template>
     <div class="displayPage">

         <p> it's alwavytfutfcuctcufttys someting</p>
        
    </div>
</template>

<script>
export default {
        name: 'DisplayPage',
        props: ['pages', 'activePage'],
        methods: {
          changePage (index) {
            this.$emit('change-page', index)
          },
          newPage () {
            this.$emit('new-page')
          }
        }
      }

</script>

<style scoped>
.displayPage {
    position: relative;
    width: 300px;
    height: 300px;
    background-color: red;
    right:0px;
}
      
    </style>
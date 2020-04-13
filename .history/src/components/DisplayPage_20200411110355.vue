<template>
     <div class="displayPage"  >

         <p > {{page.title}}</p>
        
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
    width:100px;
    height:100px;
   
    background-color: grey;
}
      
    </style>
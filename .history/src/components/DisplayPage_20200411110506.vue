<template>
     <div class="displayPage"  >

         <p >ybewy7ey</p>
        
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
        },
        data () {
    return {
      content: ''
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
<template>
     <div class="displayPage"  >

         <p >ybewy7ey</p>
        
    </div>
</template>

<script>
export default {
    components: { SimpleEditor },
    name: 'DisplayPage',
    props: ['page'],
    methods: {
    deletePage () {
        this.$emit('delete-page')
    },
    savePage () {
        this.$emit('save-page')
    }
    },
    data () {
    return {
      content: ''
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
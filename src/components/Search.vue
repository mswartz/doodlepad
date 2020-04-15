<template>
    <div class="search_frame">
        <section id="search" action="">
            <label for="user_input">
                <span class="sr_only">Search</span>
                <img src="../assets/Magnify.png"
                     alt="search." width="24"
                     height="24"></label>
            <input id="user_input" class="no_defaultstyle" v-model="searchTerm" name="search_text" placeholder="Search" type="text">
            <!--<div v-for="(page, index) in filteredPages">
                <button type="button" @click="navigate(index)">{{page.title}}</button>
            </div>-->
        </section>
    </div>
</template>
<script>
    export default {
        name: 'Search',
        props: ['pages'],
        watch:{
            searchTerm(){
                this.search();
            },
            pages(newV,oldV){
                this.filteredPages = JSON.parse(JSON.stringify(this.pages));
            }
        },
        methods: {
            search() {

               if(this.searchTerm.length>1){
     this.filteredPages = this.pages.filter(page => {
          return page.title.toLowerCase().includes(this.searchTerm.toLowerCase());
     });
    } else {
  this.filteredPages = JSON.parse(JSON.stringify(this.pages));
    }
    setTimeout(()=>{
		this.$emit('filtered-pages', {pages:this.filteredPages});
	},100
)
            }, 
            navigate(ind){
            	this.$root.$emit('change-page',ind);  
            }  
        },
        data() {
            return {
                searchTerm: '',
                filteredPages:[]
            }
        }
    }
</script>
<style scoped>

         #user_input{
             font-size: 120%;
             padding:   7px;
            
         }

        .search_frame {
            width: 310px;
            padding: 0.5em 0.7em;
            background-color: #368ca2;
            border-radius: 1em;
            height: 35px;
            margin-bottom: 7px;
            
        }

        .no_defaultstyle {
            border: 0;
            background-color: transparent;
        }

        .no_defaultstyle::placeholder {
            font-size: 1.4em;
            
        }

        .sr_only:not(:focus):not(:active) {
	clip: rect(0 0 0 0);
	clip-path: inset(100%);
	height: 1px;
	overflow: hidden;
	position: absolute;
	white-space: nowrap;
	width: 1px;
}

    </style>
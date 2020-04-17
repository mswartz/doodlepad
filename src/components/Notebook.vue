<template>
        <div class="notebook">
<<<<<<< HEAD
            <button id="new-page" @click="newPage()">New Page +</button>
            <input id="search" type="text" v-model="search" placeholder="Search" autocomplete="off"/>

            <ul>
                <li v-for="(page, index) of filteredList" class="page" @click="changePage(page)"  v-bind:class="{ 'active': pages.indexOf(page) === activePage }" v-bind:key="index">
                    <div>{{page.title}}</div>
                </li>
=======
            <Search :pages="pages" @filtered-pages="applyPages"/>
            <ul>
                <li v-for="(page, index) in filteredPages" class="page" v-bind:class="{ 'active': index === activePage }" @click="changePage(index)" v-bind:key="index">
                    <div>{{page.title}}</div>
                </li>
                <li class="new-page" @click="newPage()">New Page +</li>
>>>>>>> 89e568510910387df211ecc2fb5a7f0163041272
            </ul>
        </div>
    </template>

    <script>
<<<<<<< HEAD
      export default {
        name: 'Notebook',
        props: ['pages', 'activePage'],
        data(){
            return{
                search:""
            }

        },
        methods: {
          changePage (page) {
            this.$emit('pageClicked', page);
        

          },
          appendActive(){

             console.log(page.title);

          },
          newPage () {
            this.$emit('new-page');
            var editor= document.getElementById("editor");
          var page = document.getElementById("page");

          editor.style.display = "block";
          page.style.display = "none";
          
          }
        },
        computed: {
            filteredList: function(){
                return this.pages.filter((page)=>{
                    return page.title.toLowerCase().match(this.search);
    
                });
            }
=======
import Search from "@/components/Search.vue";

      export default {
        components: { Search },
        data: function(){
            return {
                filteredPages:[]
            }
        },
        mounted: function(){
        this.filteredPages = JSON.parse(JSON.stringify(this.pages));
            
        },
        watch:{
            pages(newV,oldV){
                this.filteredPages = JSON.parse(JSON.stringify(this.pages));
            }
        },
        name: 'Notebook',
        props: ['pages', 'activePage'],
        methods: {
        applyPages: function(event){
            
             this.filteredPages=event.pages;
            this.$emit('update-list', this.filteredPages);

            
        },
          changePage (index) {
            this.$emit('change-page', index);
            
          },
          newPage () {
            this.$emit('new-page')
            document.getElementById("editor").style.display = "block";
        document.getElementById("page").style.display = "none";
          }
>>>>>>> 89e568510910387df211ecc2fb5a7f0163041272
        }
      }
    </script>

    <style scoped>
        .notebook {
            width:35%;
<<<<<<< HEAD
            margin-top: 71px;
            display: inline-block;
            background-color:#e5e5e5;
            position: relative;
            right:10px;
            overflow: hidden;
           

        }

        #search{
            width:100%;
            height: 50px;
            background-color: #4a5261;
            border: 1px solid black;
            position: relative;
            font-size: 25px;
            padding: 5px;
            margin: 0px;
            color:white;
        
        }

        #search:focus {
            outline: none;
        }



=======
            margin-top: 73px;
            display: inline-block;
            background:#ecfcff;
            position: relative;
            right:10px;
            overflow: hidden;

        }

>>>>>>> 89e568510910387df211ecc2fb5a7f0163041272
        ul {
            list-style-type: none;
            padding: 0;
            margin: 0;
            height: 100%;
            position: relative;
        }

        li {
            padding: 1rem;
<<<<<<< HEAD
            font-size: 20px;
=======
            font-size: 1.25rem;
>>>>>>> 89e568510910387df211ecc2fb5a7f0163041272
            min-height: 1.5rem;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        li:hover {
            cursor: pointer;
<<<<<<< HEAD
            background-color: #AAAAAA;
        }

    

        .active {
            background-color:#999999;
        }

        .active:hover {
            background-color: #999999;
        }

        #new-page {
            background-color:  #2f3542;
            color: white;
            position: relative;
            bottom: 0;
            width: 100%;
            box-sizing: border-box;
            height: 50px;
            font-size: 25px;
            cursor: pointer;
            margin: 0px;
        }

        #new-page:hover {
            background-color: #2f3542;
            cursor: pointer;

            
=======
            background-color: #c5edf5;
        }

        .active {
            background-color: #80cee0;
        }

        .active:hover {
            background-color: #5caec1;
        }

        .new-page {
            background-color: #233b5d;
            color: white;
        
            bottom: 0;
            width: 100%;
            box-sizing: border-box;
        }

        .new-page:hover {
            background-color: #182c48;
>>>>>>> 89e568510910387df211ecc2fb5a7f0163041272
        }
    </style>
	
<template>
        <div class="notebook">
            <button id="new-page" @click="newPage()">New Page +</button>
            <input id="search" type="text" v-model="search" placeholder="Search"/>

            <ul>
                <li v-for="(page, index) of filteredList" class="page" @click="changePage(page)"  v-bind:class="{ 'active': pages.indexOf(page) === activePage }" v-bind:key="index">
                    <div>{{page.title}}</div>
                </li>
            </ul>
        </div>
    </template>

    <script>
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
        }
      }
    </script>

    <style scoped>
        .notebook {
            width:35%;
            margin-top: 73px;
            display: inline-block;
            background:#d2dae2;
            position: relative;
            right:10px;
            overflow: hidden;
           

        }

        #search{
            width:100%;
            height: 50px;
            background-color: green;
            position: relative;
            font-size: 25px;
        
        }

        ul {
            list-style-type: none;
            padding: 0;
            margin: 0;
            height: 100%;
            position: relative;
        }

        li {
            padding: 1rem;
            font-size: 20px;
            min-height: 1.5rem;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        li:hover {
            cursor: pointer;
            background-color: #97a1aa;
        }

    

        .active {
            background-color: #808e9b;
        }

        #new-page {
            background-color: #485460;
            color: white;
            position: relative;
            bottom: 0;
            width: 100%;
            box-sizing: border-box;
            height: 50px;
            font-size: 25px;
            cursor: pointer;
        }

        #new-page:hover {
            background-color: #182c48;
            cursor: pointer;

            
        }
    </style>
	
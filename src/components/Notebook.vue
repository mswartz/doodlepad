<template>
        <div class="notebook" >
            <button id="new-page" @click="newPage()">New Page +</button>
            <input id="search" type="text" v-model="search" placeholder="Search" autocomplete="off"/>

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
            background-color: #e5e5e5;
            width:35%;
            margin-top: 81px;
            display: inline-block;
            position: relative;
            overflow: hidden;
            right:10px;
            border:1px solid grey;
            
           

        }

        #search{
            width:100%;
            height: 50px;
            background-color: #4a5261;
            position: relative;
            font-size: 25px;
            padding: 5px;
            padding-left: 15px;
            margin: 0px;
            color:#dfe4ea;
            border: 1px solid grey;

        
        }

        #search:focus {
            outline: none;
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
            border-top: 1px solid grey;
        }

        li:hover {
            cursor: pointer;
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
            color: #dfe4ea;
            position: relative;
            bottom: 0;
            width: 100%;
            box-sizing: border-box;
            height: 50px;
            font-size: 20px;
            cursor: pointer;
            margin: 0px;
            border:none;
        }

        #new-page:hover {
            background-color: #2f3542;
            cursor: pointer;

            
        }
    </style>
	
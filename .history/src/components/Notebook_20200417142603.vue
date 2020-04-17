<template>
        <div class="notebook">
            <button id="new-page" @click="newPage()">New Page +</button>
            <input id="search" type="text" v-model="search" placeholder="Search"/>

            <ul>
                <li v-for="(page, index) of filteredList" class="page" v-bind:class="{ 'active': index === activePage }" @click="changePage(index)" v-bind:key="index">
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
          changePage (index) {
            this.$emit('change-page', index);
            this.$emit('pageClicked', page);

            console.log(index);


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
                var arr = [];
                return this.pages.filter((page)=>{
                    if(page.title.toLowerCase().match(this.search)){
                        arr.push(page);
                        return arr;

                    }
    
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
            background:#ecfcff;
            position: relative;
            right:10px;
            overflow: hidden;
           

        }

        #search{
            width:100%;
            height: 50px;
            background-color: green;
            position: relative;
        
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
            font-size: 1.25rem;
            min-height: 1.5rem;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        li:hover {
            cursor: pointer;
            background-color: #c5edf5;
        }

        .active {
            background-color: #80cee0;
        }

        .active:hover {
            background-color: #5caec1;
        }

        #new-page {
            background-color: #233b5d;
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
            
        }
    </style>
	
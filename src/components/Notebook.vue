<template>
        <div class="notebook" >
            <div id="controls">
                <input id="search" type="text" v-model="search" placeholder="Search" autocomplete="off"/>
                <button id="new-page" @click="newPage()">New Page +</button>
            </div>
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
            border-radius: 20px;
            background-color: #e5e5e5;
            position: relative;
            overflow: hidden;
            background-color: rgba(0,0,0,.9);
            box-shadow: 20px 20px 20px #EEE;
        }

        @media (min-width: 600px){
            .notebook {
                width:25%;
                left:40px;
                top: 60px;
                margin-top: 81px;
                display: inline-block;
            }
        }

        #search{
            width: 85%;
            height: 30px;
            background-color: #4a5261;
            display: inline-block;
            position: relative;
            font-size: 20px;
            padding: 5px 15px;
            margin: 10px auto 20px 10px;
            color:#dfe4ea;
            border-radius: 20px;
            border: 0;
            background-color: #000;
        }

        #search:focus {
            outline: none;
        }


        #new-page {
              padding: 10px 20px;
            color: #dfe4ea;
            position: fixed;
            width: 140px;
            height: 40px;
            box-sizing: border-box;
            height: 50px;
            font-size: 16px;
            bottom: 20px;
            right: 20px;
            z-index: 1;
            cursor: pointer;
            border:none;
            background-color: rgb(59, 22, 163);
            border-radius: 100px;
        }

        #new-page:hover {
            background-color: rgb(58, 0, 216);
            cursor: pointer;
        }

button::after {
  content: '';
  width: 120px; height: 40px;
  border-radius: 100%;
  border: 6px solid rgb(59, 22, 163);
  position: absolute;
  z-index: -1;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  animation: ring 1.5s infinite;
}

button:hover::after, button:focus::after {
  animation: none;
  display: none;
}


        @keyframes ring {
  0% {
    width: 30px;
    height: 30px;
    opacity: 1;
  }
  100% {
    width: 300px;
    height: 300px;
    opacity: 0;
  }
}



        ul {
            list-style-type: none;
            padding: 0;
            margin: 0;
            height: 100%;
            position: relative;
        }

        li {
            color: #FFF;
            padding: 1rem;
            font-size: 16px;
            font-weight: 500;
            min-height: 1.5rem;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        li:hover {
            cursor: pointer;
            background-color: #AAAAAA;
        }

    
.active {
  background: #dbc0f7;
  max-height: 0;
  opacity: 0;
  transform: translateX(-300px);
  animation: 
    openSpace 0.2s ease forwards,
    moveIn 0.3s 0.2s ease forwards;
}
@keyframes openSpace {
  to { 
    max-height: 50px;
  }
}
@keyframes moveIn {
  to { 
    opacity: 1;
    transform: translateX(0);
  }
}

        .active {
            background-color: #280469;
        }

        .active:hover {
            background-color: #130036;
        }

    </style>
	
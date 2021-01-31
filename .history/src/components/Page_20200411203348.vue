<template>
    <div class="display">
        <div v-if="page" id="page">
            <ul class= "actions">
            <li><img id="edit" @click="edit()" src="@/assets/edit.png" alt="edit"></li>
            <li><img id="share"   src="@/assets/share.png" alt="share"></li>
            <li><img id="delete" @click="deletePage()"  src="@/assets/delete.png" alt="delete"></li>
          </ul>
            <h2 id="title">{{page.title}}</h2>
            <span id="date">May 11, 2020</span><!-- timestap goes here -->
            
            <div>{{text}}</div>
        </div>
         <div v-if="page" class="page" id="editor">
            <input type="text" v-model="page.title" class="title" name="title" placeholder="Enter a title" />
            <simple-editor id ="simple" v-model="page.content" name="content"></simple-editor>

            <button @click="savePage()">Save</button>
            <button @click="cancelPage()">Cancel</button>
        </div>
        <div v-else>
            <h1>&larr; To start, create a new page!</h1>
        </div>
    </div>
   
</template>

<script>
import SimpleEditor from '@/components/SimpleEditor.vue'

export default {
    components: { SimpleEditor },
    name: 'Page',
    props: ['page'],
    methods: {
    deletePage () {
        this.$emit('delete-page')
    },
    savePage () {
        this.$emit('save-page');
        var editor= document.getElementById("editor");
          var page = document.getElementById("page");

          editor.style.display = "none";
          page.style.display = "block";
    },
     data () {
    return {
      content: ''
    }
    }, 
    cancelPage(){
        var editor= document.getElementById("editor");
          var page = document.getElementById("page");

          editor.style.display = "none";
          page.style.display = "block";

    },
    edit () {
          var editor = document.getElementById("editor");
          var page = document.getElementById("page");

          editor.style.display = "block";
          page.style.display = "none";
  
        },
    
    },
    computed: {
        text (){
            
              let element = render(document.body, `
              <div style="font-size:120%;line-height:140%">
  <p class="bold">New DOM</p>
</div>
`);

return element;
            
    }
    }
}
</script>

 <style scoped>
        @import url("https://cdn.quilljs.com/1.2.6/quill.snow.css");

        .display {
            width: 60%;
            display: inline-block;
            padding: 2rem;
            position: absolute;
            top:80px;
            right:0px;
        }
        .page {
            width: 100%;
            padding: 2rem;
        }

        .content, .title {
            border-style: none;
            border-radius: 0.25rem;
            border: solid 1px lightgray;
            width: 100%;
            box-sizing: border-box;
            margin-bottom: 1.25rem;
        }

        .content:focus, .title:focus {
            outline: 0;
        }

        .content {
            font-family: 'Avenir', Helvetica, Arial, sans-serif;
            resize: vertical;
            font-size: 1.5rem;
            padding: 0.5rem;
            height: 20rem;
        }

        .title {
            font-size: 2rem;
            padding: 0.5rem 1rem;
        }

        label {
            margin-bottom: 0.5rem;
            display: inline-block;
        }

        button {
            border-style: none;
            padding: 0.5rem 0.75rem;
            background-color: #44abc3;
            margin-right: 1rem;
            border-radius: 0.25rem;
            color: white;
            font-size: 1rem;
            cursor: pointer;
        }

        button:hover {
            background-color: #368ea2;
        }

        .actions {
            cursor: pointer;
            position: absolute;
            right: 10px;
            top:0px;
        }

        .actions li {
            cursor: pointer;
            display: inline-block;
        }
        .actions li img {
            padding: 5px;
            border: 1px solid black;
        }
        .actions li img:hover {
            background-color: rgb(201, 199, 199);
        }
        

        #date {
            font-size: 13px;
            font-style: italic;
            display: block;
            margin-bottom: 15px;
            width: 100px;
            padding-bottom: 10px;
            border-bottom: 1px dotted black;
        }

        #title {
            margin-bottom: 0px;
        }

        #editor {
            display:none;
            position: absolute;
            left:0px;
            overflow: hidden;
        }

        #editor button {
            margin-top: 50px;
            background-color: #485460;
        }

        #editor input {
            width: 90%;
        }


    </style>
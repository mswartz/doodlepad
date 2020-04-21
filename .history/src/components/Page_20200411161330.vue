<template>
    <div class="display">
        <div v-if="page">
            <ul class= "actions">
            <li><img id="edit" @click="edit()" src="@/assets/edit.png" alt="edit"></li>
            <li><img id="share"   src="@/assets/share.png" alt="share"></li>
            <li><img id="delete" @click="deletePage()"  src="@/assets/delete.png" alt="delete"></li>
          </ul>
          <span id="date">11 May 2019</span>
            <h2>{{page.title}}</h2>
            
            <div>{{text}}</div>
        </div>
         <div v-if="page" class="page" id="editor">
            <label for="title">Title</label>
            <input type="text" v-model="page.title" class="title" name="title" placeholder="Enter a title" />
            
            <simple-editor v-model="page.content" name="content"></simple-editor>
            <button @click="deletePage()">Delete Page</button>
            <button @click="savePage()">Save Page</button>
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
        this.$emit('save-page')
    },
     data () {
    return {
      content: ''
    }
    },
    edit () {
          var x = document.getElementById("editor");
  if (x.style.display === "none") {
    x.style.display = "block";
  } else {
    x.style.display = "none";
  }
        },
    
    },
    computed: {
        text (){
            var div = document.createElement('div');
           div.innerHTML = this.page.content;
            return div.textContent;
            
    }
    }
}
</script>

 <style scoped>
        @import url("https://cdn.quilljs.com/1.2.6/quill.snow.css");

        .display {
            width: 100%;
            margin-top: 73px;
            padding: 2rem;
            box-shadow: 3rem 0 5rem 3rem #c1f5ff;
            position: relative;
            min-width: 450px;
        }
        .page {
            width: 100%;
            margin-top: 100px;
            padding: 2rem;
            box-shadow: 3rem 0 5rem 3rem #c1f5ff;
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
            font-size: 10px;
        }
    </style>
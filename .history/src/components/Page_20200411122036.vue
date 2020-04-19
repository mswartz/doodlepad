<template>
    <div class="page">
        <div v-if="page">
            <h1>{{page.title}}</h1>
            <div>{{text}}</div>
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
    },
    computed: {
        text (){
            var div = document.createElement('div');
           div.innerHTML = "<h1>" +this.page.content+ "</h1>";
            return div.textContent;
            
    }
    }
}
</script>

 <style scoped>
        @import url("https://cdn.quilljs.com/1.2.6/quill.snow.css");

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
    </style>
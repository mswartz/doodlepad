<template>
 <div id="app">
        <Notebook @change-page="changePage" @new-page="newPage" :pages="pages" :activePage="index" />
        <Page @save-page="savePage" @delete-page="deletePage" :page="pages[index]" />
    </div>
</template>

<script>
    import Notebook from './components/Notebook'
    import Page from './components/Page'
    import Firebase from 'firebase'

      // Your web app's Firebase configuration
    var firebaseConfig = {
      apiKey: "AIzaSyC6P4zKqTiM2zgXEsAru6XddJMajiCEm7o",
      authDomain: "doodlepad-74288.firebaseapp.com",
      databaseURL: "https://doodlepad-74288.firebaseio.com",
      projectId: "doodlepad-74288",
      storageBucket: "doodlepad-74288.appspot.com",
      messagingSenderId: "9610917814",
      appId: "1:9610917814:web:e6192ffffb7718293b3317",
      measurementId: "G-1T9E5XFQ7E"
    };
    // Initialize Firebase
    var database = Firebase.initializeApp(firebaseConfig).database().ref();

    export default {
      name: 'app',
      components: {
        Notebook,
        Page
      },
      data: () => ({
        pages: [],
        index: 0
      }),
      mounted() {
        database.once('value', (pages) => {
          pages.forEach((page) => {
            this.pages.push({
              ref: page.ref,
              title: page.child('title').val(),
              content: page.child('content').val()
            })
          })
        })
      },
      methods: {
        newPage () {
          this.pages.push({
            title: '',
            content: ''
          })
          this.index = this.pages.length - 1
        },
        changePage (index) {
          this.index = index
        },
        savePage () {
           var page = this.pages[this.index]
            if (page.ref) {
              this.updateExistingPage(page)
            } else {
              this.insertNewPage(page)
            }
        },
        deletePage () {
          var ref = this.pages[this.index].ref
          ref && ref.remove()
          this.pages.splice(this.index, 1)
          this.index = Math.max(this.index - 1, 0)
        },
        updateExistingPage (page) {
          page.ref.update({
            title: page.title,
            content: page.content
          })
        },
        insertNewPage (page) {
          page.ref = database.push(page)
        },
      }
    }
    </script>

    <style>
    html, body, #app {
        height: 100%;
    }

    body {
        margin: 0;
    }

    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        display: flex;
        flex-direction: row;
    }
    </style>
<template>
 <div id="app">
        <div class="nav">
 <div>
               <h1>Hi {{ firstName }}!</h1>
 </div>


          <button v-if="!authenticated" @click="login">Login</button>
          <div v-if="authenticated"> 
            <button @click="logout">Logout</button>
          </div>
       </div>
        <Notebook v-if="authenticated" @change-page="changePage" @new-page="newPage" :pages="pages" :activePage="index" />
        <Page v-if="authenticated" @save-page="savePage" @delete-page="deletePage" :page="pages[index]" />
    </div>
</template>

<script>
    import Notebook from './components/Notebook'
    import Page from './components/Page'
    import Nav from './components/Nav'
    import Firebase from 'firebase'
    import * as firebase from 'firebase/app';
    import 'firebase/auth';

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
    //firebase.initializeApp(firebaseConfig);

    export default {
      name: 'app',
      components: {
        Notebook
      //  Page
      },
      data: () => ({
        pages: [],
        index: 0,
        user: {
          loggedIn: false,
          data: {}
        }
      }),
      auth: firebase.auth(),
      computed: {
        authenticated(){
          return this.user.loggedIn
        },
        firstName(){
          if (this.user.data.displayName) {
            return this.user.data.displayName.split(' ')[0]
          }
          return null
        }
      },
      methods: {
        login() {
          var vm = this;
          const provider = new firebase.auth.GoogleAuthProvider();
          firebase.auth().signInWithPopup(provider)
          .then(function(result) {
            console.log(result);
            vm.user.loggedIn = true;
            vm.user.data = result.user;
          })
          .catch(function(error){
            const errorCode = error.code;
            const errorMessage = error.message;
            const email = error.email;
            const credential = error.credential;
            console.log(errorCode, errorMessage, email, credential);
            })
        },
        logout() {
          var vm = this;
          firebase.auth().signOut()
          .then(function() {
            vm.user.loggedIn = false;
            vm.user.data = {};
          })
          .catch(function(error) {
            console.log(error)});
        },
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
      },
      mounted: function() {
        database.once('value', (pages) => {
          pages.forEach((page) => {
            this.pages.push({
              ref: page.ref,
              title: page.child('title').val(),
              content: page.child('content').val()
            })
          })
        });
        firebase.auth.onAuthStateChanged( user => {
        if (user) {
          console.log("auth state changed");
          this.user.loggedIn = true;
          this.user.data = user;
        }
        else {
          this.user.loggedIn = false;
          this.user.data = {};
        }
      });
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

    .nav {
      position: relative;
      width: 100%;
      height: 30px;
      padding: 20px;
      top: 0;
      right: 0;
      background-color: rgba(0,0,0,.5);
    }
    </style>
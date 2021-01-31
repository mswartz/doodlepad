
<template>
  <div id="app">
   <div class="nav" v-if="!authenticated">
    <div v-if="!authenticated" class="box" ></div>
    <div v-if="!authenticated" class="login-box">
          <div class="dp-img"></div>
          <!--  <button v-if="!authenticated" @click="login">Login</button> -->
          

      <button v-if="!authenticated" @click="login" type="button" class="google-button">
        <span class="google-button__icon">
              <svg viewBox="0 0 366 372" xmlns="http://www.w3.org/2000/svg">                        <path d="M125.9 10.2c40.2-13.9 85.3-13.6 125.3 1.1 22.2 8.2 42.5 21 59.9 37.1-5.8 6.3-12.1 12.2-18.1 18.3l-34.2 34.2c-11.3-10.8-25.1-19-40.1-23.6-17.6-5.3-36.6-6.1-54.6-2.2-21 4.5-40.5 15.5-55.6 30.9-12.2 12.3-21.4 27.5-27 43.9-20.3-15.8-40.6-31.5-61-47.3 21.5-43 60.1-76.9 105.4-92.4z" id="Shape" fill="#EA4335"/>
                <path d="M20.6 102.4c20.3 15.8 40.6 31.5 61 47.3-8 23.3-8 49.2 0 72.4-20.3 15.8-40.6 31.6-60.9 47.3C1.9 232.7-3.8 189.6 4.4 149.2c3.3-16.2 8.7-32 16.2-46.8z" id="Shape" fill="#FBBC05"/>
                <path d="M361.7 151.1c5.8 32.7 4.5 66.8-4.7 98.8-8.5 29.3-24.6 56.5-47.1 77.2l-59.1-45.9c19.5-13.1 33.3-34.3 37.2-57.5H186.6c.1-24.2.1-48.4.1-72.6h175z" id="Shape" fill="#4285F4"/>
                <path d="M81.4 222.2c7.8 22.9 22.8 43.2 42.6 57.1 12.4 8.7 26.6 14.9 41.4 17.9 14.6 3 29.7 2.6 44.4.1 14.6-2.6 28.7-7.9 41-16.2l59.1 45.9c-21.3 19.7-48 33.1-76.2 39.6-31.2 7.1-64.2 7.3-95.2-1-24.6-6.5-47.7-18.2-67.6-34.1-20.9-16.6-38.3-38-50.4-62 20.3-15.7 40.6-31.5 60.9-47.3z" fill="#34A853"/>
            </svg>
        </span>
      <span class="google-button__text">Sign in with Google</span>
      </button>
  </div>
  </div>



        <div class="nav-2" v-if="authenticated">
        <img id="logo" src="./assets/dp.gif" alt="logo">
          <div class = "menu">
        
            <div id="open-menu" @click="toggle">
                      <img src="./assets/open-menu.svg" alt="logo">

                      </div>
            <ul class="list">

              <li @click="darkMode"><img src="./assets/dark.png" alt="dark mode">Dark Mode</li>
              <li @click="logout"><img src="./assets/logOut.png" alt="logo">Log out</li>
            </ul>
            <h3>{{ firstName}}</h3>
          </div>

          
        </div> 

          <!-- <Notebook v-if="authenticated" @change-page="changePage" @new-page="newPage" :pages="pages" :activePage="index" /> -->
         <Notebook v-if="authenticated" @pageClicked ="showPage" @new-page="newPage" :pages="pages" :activePage="index" />
       <Page v-if="authenticated" @save-page="savePage" @delete-page="deletePage" :page="pages[index]"/>



        </div>


<!--        <Page v-if="authenticated" @save-page="savePage" @delete-page="deletePage" :page="pages[index]" />-->
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
        Notebook,
        Page
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
            var name = JSON.stringify(this.user.data.displayName.split(' ')[0]).charAt(1);
           // return this.user.data.displayName.split(' ')[0]

           return name;
          }
          return null
        }
      },
      methods: {
        darkMode () {
          document.querySelector('body').classList.toggle('darkBlue');
          document.querySelector('.notebook ul').classList.toggle('lighterBlue');
          document.querySelector('.list').classList.toggle('lighterBlue');
          

        },
        showPage(input){
        this.index = this.pages.indexOf(input);

        },
        
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
          var d = new Date();
          var datestring = d.getDate()  + "-" + (d.getMonth()+1) + "-" + d.getFullYear() + " " +
d.getHours() + ":" + d.getMinutes();
          this.pages.push({
            title: '',
            content: '',
            timestamp: datestring,
            author: this.user.data.displayName
          })
          this.index = this.pages.length - 1
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
        toggle () {
          var x = document.querySelector(".list");
  if (x.style.display === "none") {
    x.style.display = "block";
  } else {
    x.style.display = "none";
  }
        },
      },
      mounted: function() {
        database.once('value', (pages) => {
          pages.forEach((page) => {
            this.pages.push({
              ref: page.ref,
              title: page.child('title').val(),
              content: page.child('content').val(),
              author: page.child('author').val(),
              timestamp: page.child('timestamp').val()
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
        padding: 0px;
        margin:0px;

  }


    
/* Animation for fade-in effect */
.notebook, .display, .nav-2, .login-box {
    -webkit-animation: fadein 2s; /* Safari, Chrome and Opera > 12.1 */
       -moz-animation: fadein 2s; /* Firefox < 16 */
        -ms-animation: fadein 2s; /* Internet Explorer */
         -o-animation: fadein 2s; /* Opera < 12.1 */
            animation: fadein 2s;
}

@keyframes fadein {
    from { opacity: 0; }
    to   { opacity: 1; }
}

/* Firefox < 16 */
@-moz-keyframes fadein {
    from { opacity: 0; }
    to   { opacity: 1; }
}

/* Safari, Chrome and Opera > 12.1 */
@-webkit-keyframes fadein {
    from { opacity: 0; }
    to   { opacity: 1; }
}

/* Internet Explorer */
@-ms-keyframes fadein {
    from { opacity: 0; }
    to   { opacity: 1; }
}

/* Opera < 12.1 */
@-o-keyframes fadein {
    from { opacity: 0; }
    to   { opacity: 1; }
}


/* Google fonts import */
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;500;900&display=swap');

  #app {
    font-family: 'Inter', Helvetica, Arial, sans-serif;
    display: block; 
    width:100%;
    background-color: rgba(255, 255, 255, 1);
  }

  .box{
    width: 110%;
    height: 110%;
    top: 45%;
    left: 45%;
    background: url(assets/bodypage.jpeg); 
    position: fixed;
    transform: translate(-48%,-45%);
    background-size:cover;
    background-repeat: no-repeat;
    opacity: .2;
  } 
      
  .login-box{
    border-radius: 10px;
    box-shadow: 10px 10px 10px rgba(0,0,0,.5);
    padding: 25px;   
    width: 420px;
    height: 270px;
    top: 55%;
    left: 53%;
    background: rgba(0,0,0,1);
    color: #fff;
    position: fixed;
    transform: translate(-50%,-50%);
    box-sizing: border-box;
  }

.login-box button {
    background-color: #000;
    font-size: 5px;
    padding: 10px;
    width: 70%;
    height: 30%;
    margin-left: 14%;
    margin-right: 0%;
    margin-top: 5%;
    text-align: center;
    border: 1px solid #FFF;
    color: #FFF;
    cursor: pointer;
  }

  .login-box button:hover {
    border-color: rgb(58, 0, 216);
        cursor: pointer;
  }

.dp-img{
    width: 360px;
    height: 100px;
    background-image: url(assets/dp.gif);
    background-size: cover;
    border-radius: 5px;
    background-position: center;
  }
    
.google-button {
    height: 40px;
    border-width: 0;
    background: white;
    color: #737373;
    border-radius: 5px;
    white-space: nowrap;
    box-shadow: 1px 1px 0px 1px rgba(0,0,0,0.05);
    transition-property: background-color, box-shadow;
    transition-duration: 150ms;
    transition-timing-function: ease-in-out;
    padding: 0;
  }
    
.google-button__icon {
    display: inline-block;
    vertical-align: middle;
    margin: 8px 0 8px 8px;
    width: 18px;
    height: 18px;
    box-sizing: border-box;
  }


.google-button__text {
    display: inline-block;
    vertical-align: middle;
    padding: 0 15px;
    font-size: 12px;
    font-weight: bold;
    font-family: 'Roboto',arial,sans-serif;
  }

.nav {
    position: absolute;
    padding: 20px;
    top:0;
    right: 0;
    background-color: rgba(0,0,0,.5);
    
  }

  /* authenticated style */

    .nav-2{
      position: absolute;
      width: 100%;
      height: 80px;
      top: 0;
      right: 0;
      z-index: 1;
      background-color: #000;
    }

    .list {
      list-style: none;
      position: absolute;
      padding: 0;
      background-color:#e5e5e5;
      top:65px;
      width:160px;
      display: none;
      right:0px;
      display: none;
    }

    .list > li {
      padding: 10px;
      position: relative;
      cursor: pointer;
      padding-left: 20px;
      border-bottom: 1px solid grey;
 
    }
    .list > li > img{
       position: relative;
       top:7px;
       right:5px;
       
 
    }

    .list > li:hover {
      background-color:#AAAAAA;

    }

    .menu h3 {
      position: relative;
      display: inline-block;
      font-size: 25px;
      width:50px;
      height:50px;
      text-align: center;
      line-height: 50px;
      background-color: #222;
      font-weight: 100;
      border-radius: 100%;
      cursor: pointer;
      top:-10px;
      right:15px;
      color:rgb(243, 239, 239);
    }

    .menu {
      position:absolute;
      right: 0px;
    }

    #logo {
      display:inline-block;
      position: absolute;
      top: 10px;
      left:20px;
      height: 70px;
    }
    #open-menu img {
      position: absolute;
      width:25px;
       top: 50%; 
       right: 50%;
     transform: translate(50%,-50%);
     
      
    }

    #open-menu {
      position: relative;
      display: inline-block;
      width:50px;
      height:50px;
      border: 1px solid white;
      border-radius: 100%;
      cursor: pointer;
      top:8px;
      right:20px;
    }

    .notes {
      position: absolute;
      top: 100px;
    }

    body.darkBlue #app{
      background-color: #1d1d1d;
      color:#d8d4d4;
    }

    body.darkBlue #app .display {
      background-color: #1d1d1d;
    }

      body.darkBlue #app .notebook {
      box-shadow: none;
    }

          body.darkBlue #app .notebook #search {
      background-color: #222;
    }


    .lighterBlue {
      background-color: #57606f;
    }

</style>
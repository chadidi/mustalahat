<template>
  <div id="app">
    <nav class="navbar navbar-inverse navbar-fixed-top">
      <div class="container">
        <div class="navbar-header">
          <router-link class="navbar-brand" to="/">Mustalahat</router-link>
        </div>
        <ul class="nav navbar-nav navbar-right">
          <li class="btn btn-default" data-toggle="modal" data-target="#myModal" style="margin-top: 12px;">
            <span class="glyphicon glyphicon-plus"></span>
          </li>
        </ul>
      </div>
    </nav>
    <router-view :trans="trans"></router-view>
  </div>
</template>

<script>
import firebase from 'firebase'
// Initialize Firebase
var config = {
  apiKey: "AIzaSyDdQrfuhGYgGWTW1eW2TnDfssuHCV6WLdk",
  authDomain: "mustalahat-9541b.firebaseapp.com",
  databaseURL: "https://mustalahat-9541b.firebaseio.com",
  projectId: "mustalahat-9541b",
  storageBucket: "mustalahat-9541b.appspot.com",
  messagingSenderId: "542444175389"
};
let app = firebase.initializeApp(config);
let db = app.database();
let transRef = db.ref('trans');

export default {
  name: 'app',
  firebase: {
    trans: transRef
  },
  mounted() {
    toastr.options = {
      "closeButton": true,
      "debug": false,
      "newestOnTop": true,
      "progressBar": true,
      "positionClass": "toast-bottom-center",
      "preventDuplicates": false,
      "showDuration": "500",
      "hideDuration": "500",
      "timeOut": "5000",
      "extendedTimeOut": "1000",
      "showEasing": "swing",
      "hideEasing": "linear",
      "showMethod": "fadeIn",
      "hideMethod": "fadeOut"
    }
  },
  methods: {
    saveTrans(tran){
      if (!tran['.key']) {
        transRef.push(tran);
        toastr.success('translation saved successfully')
      } else {
        var trans = {
          name: tran.name,
          arname: tran.arname
        }
        transRef.child(tran['.key']).update(trans);
        toastr.success('translation updated successfully', {"positionClass": "toast-bottom-right"})
      }
    },
    removeTrans(tran) {
      transRef.child(tran['.key']).remove();
      toastr.success('translation removed successfully', {"positionClass": "toast-bottom-right"})
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

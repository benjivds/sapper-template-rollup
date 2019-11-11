<script>
  import { currentUser } from "./../stores/user.js";
  import { onMount } from "svelte";
  import { beforeUpdate, tick } from 'svelte';

  onMount(() => {
    console.log("testing");
    createLoginButton();
  });
  function createLoginButton() {
    // FirebaseUI config.
    var uiConfig = {
      signInOptions: [
        // Leave the lines as is for the providers you want to offer your users.
        firebase.auth.GoogleAuthProvider.PROVIDER_ID
      ],
      callbacks: {
        signInSuccessWithAuthResult: function(authResult, redirectUrl) {
          // If a user signed in with email link, ?showPromo=1234 can be obtained from
          // window.location.href.
          // ...
          return false;
        }
      }
    };

    // Initialize the FirebaseUI Widget using Firebase.
    var ui = firebaseui.auth.AuthUI.getInstance() || new firebaseui.auth.AuthUI(firebase.auth());
    // The start method will wait until the DOM is loaded.
    ui.start("#firebaseui-auth-container", uiConfig);
  }

  function singOut(){
      firebase.auth().signOut().then(async function() {
          console.log('logged out');
          // tick wait until dom created
          await tick();
          createLoginButton();
      }).catch(function(error){
          console.log('logout error: ', error);
          alert(error);
      });

  }
</script>
{#if $currentUser}
<button on:click={() => {singOut()}}>Logout</button>
{:else}
<div id="firebaseui-auth-container" />
{/if}
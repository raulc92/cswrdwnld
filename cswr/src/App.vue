<template>
  <div id="app">
    <p>prova</p>
    <router-view/>
  </div>
</template>


<script>

import axios from 'axios'

export default{
data(){
    let token
    let webreport
    this.getToken()
    return{

    }
 },
 methods: {
   getToken(){
     let myHeaders = new Headers();
      myHeaders.append("Content-Type", "application/x-www-form-urlencoded");

      let urlencoded = new URLSearchParams();
      urlencoded.append("username", "admin");
      urlencoded.append("password", "Moriondo20");

      let requestOptions = {
        method: 'POST',
        headers: myHeaders,
        body: urlencoded,
        redirect: 'follow'
      };

      fetch("http://172.28.34.27/otcs/cs.exe/api/v1/auth", requestOptions)
        .then(response => response.json())
        .then(result => {
          this.token = result
          console.log(result)
          console.log(this.token.ticket)
         // this.testAxios()
          this.testFetch()})
        .catch(error => console.log('error', error));

        
   },
  testFetch(){
        let myHeaders = new Headers();
        myHeaders.append("otcsticket", this.token.ticket);

        let urlencoded = new URLSearchParams();

        let requestOptions = {
          method: 'GET',
          headers: myHeaders,
          redirect: 'follow'
        };

        fetch("http://172.28.34.27/otcs/cs.exe/api/v1/nodes/724891/output", requestOptions)
          .then(response => response.json())
          .then(result =>  {
            result = JSON.parse(result.data)
            this.webreport = result.myRows
            let maxdata=result.actualRows
            console.log(this.webreport)
            for(let i=0;i<maxdata;i++){
                console.log(this.webreport[i].OwnerID)
                }     
          })
          .catch(error => console.log('error', error));
              }
       }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

/*const token =  fetch(auth);
  console.log(token); */

  await
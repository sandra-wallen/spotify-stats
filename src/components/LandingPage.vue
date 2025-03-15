<template>
  <h1>HEJ</h1>
</template>

<script setup>
  import queryString from 'query-string'
  import { onMounted } from "vue"

  onMounted(async() => {
    if (!window.localStorage.getItem("token")) {
      await requestAccessToken()
    }
    
    await getArtist()
  })
  
  const requestAccessToken = async () => {
    try {
      const response = await fetch("https://accounts.spotify.com/api/token", {
        method: "POST",
        headers: {
          "Content-Type": "application/x-www-form-urlencoded"
        }, 
        body: queryString.stringify({
          grant_type: "client_credentials",
          client_id: "6c6045360265485eb105134597b35fd5",
          client_secret: "e9ad26ee799c4c68b600db55077a3607"
        })
      });
      if (!response.ok) {
        throw new Error(`Response status: ${response.status}`);
      }

      const json = await response.json();
      console.log(json);
      window.localStorage.setItem("token", json.access_token)
    } catch (error) {
      console.error(error)
    }
  }
  
  const getArtist = async () => {
    try {
      const response = await fetch("https://api.spotify.com/v1/artists/2n2RSaZqBuUUukhbLlpnE6?si=74hOXSUeS9CdI40tkZDO-w", {
        headers: {
          "Authorization": `Bearer ${window.localStorage.getItem("token")}`
        }
      })

      if (!response.ok) {
        throw new Error(`Response status: ${response.status}`);
      }

      const json = await response.json();
      console.log(json);
    } catch (error) {
      console.error(error)
    }
  }
</script>

<style scoped>

</style>
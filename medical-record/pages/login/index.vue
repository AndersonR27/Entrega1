<template>
  <v-container fill-height>
    <v-row justify="center" align="center">
      <v-col cols="5">
        <center>
          <h1>MEDICAL RECORD - MÉDICOS</h1>          
          <login color="blue--text"/>
        </center>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import login from "@/components/Login";
export default {
  layout: "blank",
  components: {
    login,
  },
};
</script>

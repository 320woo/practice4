<template>
  <v-container>
    <v-layout align-center justify-center>
      <v-flex md8>
        <v-col class="text-center" cols="12" sm="8">
          <div class="my-2">
            <v-card
              class="mx-auto"
              max-width="344"
              height="200"
              outlined
            >
              <v-list-item>
                <v-list-item-content>
                  <div class="headline mb-1">
                    <v-toolbar color="pink" dark>제목
                      <v-spacer></v-spacer>
                      <v-btn small v-btn color="purple" dark><v-icon dark>build</v-icon></v-btn>
                      <v-btn small v-btn color="red" dark @click=""><v-icon dark>block</v-icon></v-btn>
                    </v-toolbar>
                    <v-spacer></v-spacer>
                  </div>
                  <v-list-item-title> 내용이 </v-list-item-title>
                  <p></p>
                  <v-list-item-subtitle> 들어가는 칸 </v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
            </v-card>
            <p>
            </p>
          </div>
        </v-col>
      </v-flex>
    </v-layout>
  </v-container>
</template>
<script>
export default {
  name: 'contact',
  data () {
    return {
      title: 'Contact'
    }
  }
}
</script>
<style scoped>

</style>

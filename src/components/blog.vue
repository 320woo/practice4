<template>
  <v-container>
    <v-layout align-center justify-center>
      <v-flex md8>
        <v-col class="text-center" cols="12" sm="8">
          <div class="my-2">
            <h1 class="pa-2 d-flex justify-center">최신</h1>
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
                    </v-toolbar>
                  </div>
                  <v-list-item-title> 타이틀 </v-list-item-title>
                  <p></p>
                  <v-list-item-subtitle> 서브 타이틀</v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
            </v-card>
          </div>
        </v-col>
      </v-flex>
      <v-flex md4>
          <v-col class="text-center" align="center" cols="12" sm="4">
            <div class="text-xs-center">
              <v-dialog v-model="dialog" width="500">
                <template v-slot:activator="{ on }">
                  <v-btn color="red lighten-2" dark v-on="on">추가</v-btn>
                  <p>

                  </p>
                </template>

                <v-card>
                  <v-toolbar color="blue">
                    <v-toolbar-title class="white--text">일정 추가</v-toolbar-title>

                    <v-spacer></v-spacer>

                    <v-btn fab dark @click="dialog=false">저장</v-btn>
                  </v-toolbar>

                  <v-text-field label=" 제목을 입력해주세요">
                  </v-text-field>
                  <p>

                  </p>
                  <v-text-field label=" 내용을 입력해주세요">
                  </v-text-field>
                  <v-layout row wrap>
                    <v-flex xs12>
                      <v-date-picker v-model="picker" color="green lighten-1"></v-date-picker>
                    </v-flex>
                  </v-layout>
                </v-card>
              </v-dialog>
            </div>
            <v-layout wrap>
              <v-flex
                xs12
                class="mb-3"
              >
                <v-sheet height="500">
                  <h1> {{start}} </h1>
                  <v-calendar
                    ref="calendar"
                    v-model="start"
                    :type="type"
                    :end="end"
                    color="primary"
                  ></v-calendar>
                </v-sheet>
              </v-flex>

              <v-flex
                sm4
                xs12
                class="text-sm-left text-xs-center"
              >
                <v-btn @click="$refs.calendar.prev()">
                  <v-icon
                    dark
                    left
                  >
                    keyboard_arrow_left
                  </v-icon>
                  Prev
                </v-btn>
              </v-flex>
              <v-flex
                sm4
                xs12
                class="text-xs-center"
              >
                <v-select
                  v-model="type"
                  :items="typeOptions"
                  label="Type"
                ></v-select>
              </v-flex>
              <v-flex
                sm4
                xs12
                class="text-sm-right text-xs-center"
              >
                <v-btn @click="$refs.calendar.next()">
                  Next
                  <v-icon
                    right
                    dark
                  >
                    keyboard_arrow_right
                  </v-icon>
                </v-btn>
              </v-flex>
            </v-layout>
          </v-col>
      </v-flex>
    </v-layout>
  </v-container>
</template>
<script>
export default {
  data: () => ({
    picker: new Date().toISOString().substr(0, 10),
    picker2: new Date().toISOString().substr(0, 10),
    dialog: false,
    type: 'month',
    start: '2020-10-01',
    end: '2021-01-06',
    typeOptions: [
      { text: 'Day', value: 'day' },
      { text: '4 Day', value: '4day' },
      { text: 'Week', value: 'week' },
      { text: 'Month', value: 'month' },
      { text: 'Custom Daily', value: 'custom-daily' },
      { text: 'Custom Weekly', value: 'custom-weekly' }
    ]
  })
}
</script>
<style>
</style>

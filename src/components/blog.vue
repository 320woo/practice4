<template>
  <v-container>
    <v-layout align-center justify-center>
      <v-flex md8>
        <v-col class="text-center" cols="12" sm="8">
          <div class="my-2">
            <h1 class="pa-2 d-flex justify-center">최신</h1>
            <v-card
              v-model="card"
              class="mx-auto"
              max-width="344"
              height="200"
              outlined
              group
            >
              <template>
                <v-list-item>
                  <v-list-item-action>
                    <div class="headline mb-1">
                      <v-toolbar color="pink" dark> 정밀농업
                        <v-spacer></v-spacer>
                        <v-btn small v-btn color="purple" dark><v-icon dark>build</v-icon></v-btn>
                        <v-btn small v-btn color="red" dark @click=""><v-icon dark>block</v-icon></v-btn>
                      </v-toolbar>
                    </div >
                    <v-list-item-title> 정밀농업 관련 샘플 </v-list-item-title>
                  </v-list-item-action>
                </v-list-item>
              </template>
            </v-card>
            <p>

            </p>
            <v-card
              class="mx-auto"
              max-width="344"
              height="200"
              outlined
              group
            >
              <template v-for="(card, i) in cards">
                <v-list-item :key="`${i}-${card.title}`">
                  <v-list-item-action>
                  <div class="headline mb-1">
                      <v-toolbar color="pink" dark>{{ card.title }}
                        <v-spacer></v-spacer>
                      <v-btn small v-btn color="purple" dark><v-icon dark>build</v-icon></v-btn>
                      <v-btn small v-btn color="red" dark @click="card = false"><v-icon dark>block</v-icon></v-btn>
                    </v-toolbar>
                  </div >
                  <v-list-item-title> {{ card.content }} </v-list-item-title>
                  </v-list-item-action>
              </v-list-item>
              </template>
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
                  </v-toolbar>

                  <v-text-field label=" 제목을 입력해주세요" v-model="newTask">
                    <template v-slot:append>
                      <v-fade-transition>
                        <v-btn fab dark v-if="newTask" @click="add">저장</v-btn>
                      </v-fade-transition>
                    </template>
                  </v-text-field>
                  <p>

                  </p>
                  <v-text-field label=" 내용을 입력해주세요" v-model="newTask1">
                    <template></template>
                  </v-text-field>
                  <v-layout row wrap>
                    <v-flex xs12>
                      <v-date-picker v-model="picker" color="green lighten-1"></v-date-picker>
                    </v-flex>
                  </v-layout>
                </v-card>
              </v-dialog>
            </div>
            <h1> {{ start }}</h1>
            <v-layout>
              <v-flex>
                <v-sheet height="500">
                  <v-calendar
                    :now="start"
                    :value="start"
                    color="primary"
                  >
                    <template v-slot:day="{ date }">
                      <template v-for="event in eventsMap[date]">
                        <v-menu
                          :key="event.title"
                          v-model="event.open"
                          full-width
                          offset-x
                        >
                          <template v-slot:activator="{ on }">
                            <div
                              v-if="!event.time"
                              v-ripple
                              class="my-event"
                              v-on="on"
                              v-html="event.title"
                            ></div>
                          </template>
                          <v-card
                            color="grey lighten-4"
                            min-width="350px"
                            flat
                          >
                            <v-toolbar
                              color="primary"
                              dark
                            >
                              <v-btn icon>
                                <v-icon>edit</v-icon>
                              </v-btn>
                              <v-toolbar-title v-html="event.title"></v-toolbar-title>
                              <v-spacer></v-spacer>
                              <v-btn icon>
                                <v-icon>favorite</v-icon>
                              </v-btn>
                              <v-btn icon>
                                <v-icon>more_vert</v-icon>
                              </v-btn>
                            </v-toolbar>
                            <v-card-title primary-title>
                              <span v-html="event.details"></span>
                            </v-card-title>
                            <v-card-actions>
                              <v-btn
                                flat
                                color="secondary"
                              >
                                Cancel
                              </v-btn>
                            </v-card-actions>
                          </v-card>
                        </v-menu>
                      </template>
                    </template>
                  </v-calendar>
                </v-sheet>
              </v-flex>
            </v-layout>
          </v-col>
      </v-flex>
    </v-layout>
  </v-container>
</template>
<script>
export default {
  data() {
    return {
      events: [
        {
          title: '빼빼로데이',
          date: '2020-11-11',
          open: false
        }
      ],
      picker: new Date().toISOString().substr(0, 10),
      picker2: new Date().toISOString().substr(0, 10),
      count: 0,
      dialog: false,
      card: true,
      cards: [
        { title: "제목",
          content: "내용"
        }
      ],
      sample: [
        { title: "정밀농업",
          content: "정밀농업 관련 내용"
        }
      ],
      type: 'month',
      start: '2020-11-02',
      end: '2021-01-06',
      newTask: null,
      typeOptions: [
        { text: 'Day', value: 'day' },
        { text: '4 Day', value: '4day' },
        { text: 'Week', value: 'week' },
        { text: 'Month', value: 'month' },
        { text: 'Custom Daily', value: 'custom-daily' },
        { text: 'Custom Weekly', value: 'custom-weekly' }
      ],
    }
  },
  computed: {
    eventsMap () {
      const map = {}
      this.events.forEach(e => (map[e.date] = map[e.date] || []).push(e))
      return map
    }
  },
  methods: {
      add() {
        this.count++;
        this.cards.push({
          title: this.newTask,
          content: this.newTask1
        })
        this.newTask = null
        this.newTask1 = null
      },
    open (event) {
      alert(event.title)
      }
    }
}
</script>
<style>
</style>

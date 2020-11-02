<template>
  <v-container>
    <v-toolbar style="max-width: 150%">
      <v-toolbar-side-icon></v-toolbar-side-icon>
      <v-toolbar-title class="display-2 font-weight-black green--text pl-4">Daily</v-toolbar-title>
      <v-toolbar-title class="display-2 cyan--text pl-9">Check</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items class="hidden-sm-and-down">
        <v-breadcrumbs :items="items" divider="|"></v-breadcrumbs>
      </v-toolbar-items>
    </v-toolbar>
    <v-divider class="mb-3"></v-divider>
    <v-text-field
      v-model="newTask"
      label="오늘 해야될 일 (추가 가능)"
        solo
      @keydown.enter="create"
      style="max-width: 50%"
    >
      <template v-slot:append>
        <v-fade-transition>
          <v-icon
            v-if="newTask"
            @click="create"
            class="font-weight-medium"
            v-text="size">
            +
          </v-icon>
        </v-fade-transition>
      </template>
    </v-text-field>

    <h2 class="display-1 green--text pl-4">
      Tasks:&nbsp;
      <v-fade-transition leave-absolute>
        <span :key="`tasks-${tasks.length}`">
          {{ tasks.length }}
        </span>
      </v-fade-transition>
    </h2>

    <v-divider class="mt-3"></v-divider>

    <v-layout
      my-1
      align-center
    >
      <strong class="blue--text">
        Remaining: {{ remainingTasks }}
      </strong>

      <v-divider vertical></v-divider>

      <strong class="green--text">
        Completed: {{ completedTasks }}
      </strong>

      <v-spacer></v-spacer>

      <v-progress-circular
        :value="progress"
        class="mr-2"
        color="#FF0000"
      ></v-progress-circular>
    </v-layout>

    <v-divider class="mb-3"></v-divider>

    <v-card v-if="tasks.length > 0">
      <v-slide-y-transition
        class="py-0"
        group
        tag="v-list"
      >
        <template v-for="(task, i) in tasks">
          <v-divider
            v-if="i !== 0"
            :key="`${i}-divider`"
          ></v-divider>

          <v-list-tile :key="`${i}-${task.text}`">
            <v-list-tile-action>
              <v-checkbox
                v-model="task.done"
                color="info darken-3"
              >
                <template v-slot:label>
                  <div
                    :class="task.done && 'grey--text' || 'text--primary'"
                    class="ml-3"
                    v-text="task.text"
                  ></div>
                </template>
              </v-checkbox>
            </v-list-tile-action>

            <v-spacer></v-spacer>

            <v-scroll-x-transition>
              <v-icon
                v-if="task.done"
                color="#0000FF"
              >
                mdi-check
              </v-icon>
            </v-scroll-x-transition>
          </v-list-tile>
        </template>
      </v-slide-y-transition>
    </v-card>
    <v-dialog
      v-model="dialog"
      width="500"
    >
      <template v-slot:activator="{ on }">
        <v-btn
          color="red lighten-2"
          dark
          v-on="on"
        >
          저장
        </v-btn>
      </template>

      <v-card>
        <v-card-title
          class="headline grey lighten-2"
          primary-title
        >
          Daily Check Save System
        </v-card-title>

        <v-card-text>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-btn
            color="#0000ff"
            flat
            @click="dialog = false"
          >
            I accept
          </v-btn>
          <v-btn
            color="#ff0000"
            flat
            @click="dialog = false"
          >
            I reject
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-bottom-nav
      :active.sync="bottomNav"
      :value="true"
      absolute
      color="transparent"
    >
      <v-btn
        color="teal"
        flat
        value="recent"
      >
        <span>Recent</span>
        <v-icon>history</v-icon>
      </v-btn>

      <v-btn
        color="teal"
        flat
        value="favorites"
      >
        <span>Favorites</span>
        <v-icon>favorite</v-icon>
      </v-btn>

      <v-btn
        color="teal"
        flat
        value="nearby"
      >
        <span>Nearby</span>
        <v-icon>place</v-icon>
      </v-btn>
    </v-bottom-nav>
  </v-container>
</template>
<script>
export default {
  data: () => ({
    items: [
      {
        text: 'Naver',
        disabled: false,
        href: 'https://www.naver.com/'
      },
      {
        text: 'Daum',
        disabled: false,
        href: 'https://www.daum.net/'
      },
      {
        text: 'Youtube (disabled)',
        disabled: true,
        href: 'https://www.youtube.com/'
      }
    ],
    tasks: [
      {
        done: false,
        text: '아침에 일어나 이불 개기'
      },
      {
        done: false,
        text: '출근 전 유산균, 포도즙 먹기'
      },
      {
        done: false,
        text: '출근 후 메일 확인'
      },
      {
        done: false,
        text: '주어진 과제 처리'
      }
    ],
    newTask: null,
    dialog: false
  }),

  computed: {
    completedTasks () {
      return this.tasks.filter(task => task.done).length
    },
    progress () {
      return this.completedTasks / this.tasks.length * 100
    },
    remainingTasks () {
      return this.tasks.length - this.completedTasks
    },
    localAttrs () {
      const attrs = {}

      if (this.variant === 'default') {
        attrs.absolute = false
        attrs.fixed = false
      } else {
        attrs[this.variant] = true
      }
      return attrs
    }
  },
  methods: {
    create () {
      this.tasks.push({
        done: false,
        text: this.newTask
      })

      this.newTask = null
    }
  },
  icons: [
    'mdi-home',
    'mdi-email',
    'mdi-calendar',
    'mdi-delete'
  ],
  items: [
    'default',
    'absolute',
    'fixed'
  ],
  variant: 'default',
  alignments: [
    'start',
    'center',
    'end'
  ]
}
</script>

<template>
  <div id="app">
    <Kanban :stages="statuses" :blocks="blocks" @update-block="updateBlock">
        <div v-for="item in blocks" :slot="item.id" :key="item.id">
            <div>
                {{ item.title }}
            </div>
        </div>
    </Kanban>
  </div>
</template>

<script>
import faker from 'faker'
import { debounce } from 'lodash'
import Kanban from './components/Kanban'

export default {
  name: 'app',
  components: {
    Kanban
  },
  data() {
    return {
      statuses: [
        'Backlog',
        'Stand by',
        'Research',
        'Production',
        'To test',
        'In progress'
      ],
      blocks: []
    }
  },
  mounted() {
    for (let i = 0; i <= 10; i += 1) {
      this.blocks.push({
        id: i,
        status: this.statuses[Math.floor(Math.random() * 4)],
        title: faker.company.bs()
      })
    }
  },

  methods: {
    updateBlock: debounce(function(id, status) {
      this.blocks.find(b => b.id === Number(id)).status = status
    }, 500)
  }
}
</script>

<style lang="scss">
@import './assets/main.sass';

$on-hold: #fb7d44;
$in-progress: #2a92bf;
$needs-review: #f4ce46;
$approved: #00b961;

* {
  box-sizing: border-box;
}

.drag-column {
  &-on-hold {
    .drag-column-header,
    .is-moved,
    .drag-options {
      background: $on-hold;
    }
  }

  &-in-progress {
    .drag-column-header,
    .is-moved,
    .drag-options {
      background: $in-progress;
    }
  }

  &-needs-review {
    .drag-column-header,
    .is-moved,
    .drag-options {
      background: $needs-review;
    }
  }

  &-approved {
    .drag-column-header,
    .is-moved,
    .drag-options {
      background: $approved;
    }
  }
}

.section {
  padding: 20px;
  text-align: center;

  a {
    color: white;
    text-decoration: none;
    font-weight: 300;
  }

  h4 {
    font-weight: 400;
    a {
      font-weight: 600;
    }
  }
}
</style>

<template>
  <div class="page">
    <div class="drag-container" v-if="hasProps">
    	<ul class="drag-list">
    		<li v-for="(stage, i) in stages"
          class="drag-column"
          :class="{['drag-column-' + stage]: true}"
          :key="i"
          @mouseover="blocksData[i].hovered = true"
          @mouseleave="blocksData[i].hovered = false">
    			<span class="drag-column-header">
    				<h2>{{ stage }}</h2>
    			</span>
    			<div class="drag-options"></div>
    			<ul class="drag-inner-list" ref="list" :data-status="stage">
            <li class="drag-item" v-for="block in getBlocks(stage)" :data-block-id="block.id" :key="block.id">
                <slot :name="block.id">
                    <strong>{{ block.status }}</strong>
                    <div>{{ block.id }}</div>
                </slot>
            </li>
    			</ul>
          <div class="add-card" v-if="blocksData[i].hovered" @click="showModal(i)">
            <p>Add a new card</p>
          </div>
    		</li>
    	</ul>
    </div>
    <add-card-modal
      @close-modal="closeModal()"
      @add-card="addNewCard()"
      v-if='modal'/>
  </div>
</template>

<script>
import AddCardModal from './AddCardModal'

import dragula from 'dragula'

export default {
  name: 'KanbanBoard',
  props: {
    stages: {},
    blocks: {}
  },
  components: {
    AddCardModal
  },
  data() {
    return {
      blocksData: this.blocks,
      modal: false,
      indexOfSelectedCol: null
    }
  },
  computed: {
    localBlocks() {
      return this.blocksData
    },
    hasProps() {
      return this.blocksData !== undefined
    }
  },
  methods: {
    getBlocks(status) {
      return this.localBlocks.filter(block => block.status === status)
    },
    showModal(index) {
      this.modal = true
      this.indexOfSelectedCol = index
    },
    closeModal() {
      this.modal = false
    },
    addNewCard(value) {
      let colLabel = this.stages[indexOfSelectedCol]
      let card = {
        id: this.blocks.length + 1,
        status: colLabel,
        title: value.title,
        desc: value.desc,
        hovered: false
      }
      this.blocks.push(card)
    }
  },
  mounted() {
    dragula(this.$refs.list)
      .on('drag', el => {
        el.classList.add('is-moving')
      })
      .on('drop', (block, list) => {
        let index = 0
        for (index = 0; index < list.children.length; index += 1) {
          if (list.children[index].classList.contains('is-moving')) break
        }
        this.$emit(
          'update-block',
          block.dataset.blockId,
          list.dataset.status,
          index
        )
      })
      .on('dragend', el => {
        el.classList.remove('is-moving')

        window.setTimeout(() => {
          el.classList.add('is-moved')
          window.setTimeout(() => {
            el.classList.remove('is-moved')
          }, 600)
        }, 100)
      })
  }
}
</script>

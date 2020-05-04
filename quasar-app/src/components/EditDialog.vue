<template>
  <q-dialog persistent v-model="show" full-width>
    <q-card v-if="editing">
      <q-card-section>
        {{ label }}
      </q-card-section>

      <q-separator inset />

      <q-list dense>
        <q-item v-for="col in defaultColumns" :key="col.name">
          <q-item-section>
            <q-input dense outlined v-model="editing[col.name]" :placeholder="col.label">
              <template v-slot:before>
                <LabelDiv :label="col.label" />
              </template>
            </q-input>
          </q-item-section>
        </q-item>

        <slot name="customItems">
        </slot>
      </q-list>

      <q-btn class="q-ma-md" color="primary" label="Save" v-close-popup @click="onSave" />
      <q-btn class="q-ma-md" color="primary" label="Cancel" v-close-popup @click="onClose" />
    </q-card>
  </q-dialog>
</template>

<script>
import defaultColumns from '../utils/defaultColumns'

import LabelDiv from './LabelDiv'

export default {
  name: 'EditDialog',

  components: {
    LabelDiv
  },

  props: {
    show: {
      type: Boolean,
      default: false
    },

    editing: {
      type: Object,
      required: true
    },

    label: {
      type: String,
      required: true
    },

    columns: {
      type: Array,
      required: true
    }
  },
  computed: {
    defaultColumns
  },
  methods: {
    onClose (event) {
      this.$emit('close')
    },
    onSave (event) {
      this.$emit('close', this.editing)
    }
  }
}
</script>
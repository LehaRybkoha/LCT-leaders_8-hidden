<script setup>
import { CommonButton, CommonSelect } from '~/components/common'
import { onMounted, ref, watch, toRaw, computed } from 'vue'

const props = defineProps({
  idx: {
    type: Number,
  },
  item: {
    type: Object,
  },
  patche: {
    type: Object,
  },
  keyLine: {
    type: Object,
  },
})

const selectedSpgz = props.item.hypothesises.length
  ? props.item.hypothesises[0].spgz_piece.id
  : ''
const modelSpgz = ref(selectedSpgz)

const emit = defineEmits(['change-item', 'choose-hypo', 'choose-key'])

const changeItem = (item) => {
  emit('change-item', item.line_number, modelSpgz.value, props.idx)
}

const choose = () => {
  emit(
    'choose-hypo',
    props.item.hypothesises,
    props.idx,
    props.item.line_number,
    props.patche.spgz_id
  )
}

const chooseKey = () => {
  emit('choose-key', props.item)
}

const change = (id) => {
  modelSpgz.value = id
}

const chosen_computed_hypo = computed(() => {
  if (props.item.hypothesises && props.patche) {
    const item = props.item.hypothesises.find(
      (item) => item.spgz_piece.id === props.patche.spgz_id
    )
    if (item) {
      return item.spgz_piece.name
    } else {
      return props.patche.spgz_id
    }
  }
})

watch(modelSpgz, (val) => {
  changeItem(props.item, val)
})
</script>

<template>
  <li
    @click="chooseKey"
    class="table__item"
    :class="{
      table__item_sure: item.spgz_defined,
      table__item_key: keyLine.line_number === item.line_number,
    }"
  >
    <div>
      <div class="table__checkbox">
        <div class="table__checkbox-checked"></div>
      </div>
    </div>
    <span class="table__text">{{ item.code }}</span>
    <span class="table__text">{{ item.name }}</span>
    <span class="table__text">{{ item.amount }}</span>
    <span class="table__text">{{ item.uom }}</span>
    <span class="table__text">{{ item.price }}</span>
    <span
      class="table__text"
      v-if="item.hypothesises.length && chosen_computed_hypo"
      >{{ chosen_computed_hypo }}</span
    >
    <span class="table__text" v-else>???????????????? ???? ??????????????</span>
    <div class="table__wrapper">
      <common-button
        @click.stop="choose"
        @change-item="change"
        class="table__button"
        >???????????????? ????????</common-button
      >
    </div>
  </li>
</template>

<style lang="scss" scoped>
.table {
  &__wrapper {
    margin-left: 30px;
  }
  &__item {
    display: grid;
    grid-template-columns: 1fr 4fr;
    padding: 20px 30px;
    transition: border 0.3s ease;
    border: 1px solid transparent;
    border-radius: 2px;
    cursor: pointer;
    &:hover {
      border: 1px solid $accent-purple;
    }
    &_even {
      background-color: rgba(0, 0, 0, 0.05);
    }
    &_key {
      background: #9d78eb;
      color: #ffffff;
      .table__button {
        border: 1px solid #ffffff;
        box-shadow: 0px 0px 4px rgba(255, 255, 255, 0.4);
      }
      .table__checkbox {
        border: 1px solid #ffffff;
      }
      .table__checkbox-checked {
        background-color: #ffffff;
        opacity: 1;
      }
    }
  }
  &__select {
    height: 80px;
    border-radius: 4px;
    width: 100px;
  }
  &__checkbox {
    width: 20px;
    height: 20px;
    border-radius: 2px;
    border: 1px solid $accent-purple;
    position: relative;
    &-checked {
      transition: opacity 0.3s ease;
      opacity: 0;
      width: 12px;
      height: 12px;
      border-radius: 2px;
      background-color: $accent-purple;
      position: absolute;
      left: 3px;
      top: 3px;
    }
  }
  &__text {
    @include tg-12-medium;
  }
  &__button {
    font-size: 10px;
    padding: 5px 10px;
  }
}
</style>

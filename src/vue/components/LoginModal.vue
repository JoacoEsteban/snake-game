<template>
  <div v-if="!kill" :class="{hide: hideLocal}" class="modal-wrapper fix-full">
    <form class="modal-container" @submit="submit">
      <span class="name mb-4">
        My snek is called
        <input autofocus required type="text" v-model.trim="snakeName" class="bold" />
      </span>
      <button class="mt-2" type="submit">Connect</button>
    </form>
  </div>
</template>

<script lang="ts">
import names from '@/static/SnakeNames'
import * as types from '@@types'
import { Component, Prop, Watch, Vue } from 'vue-property-decorator';


@Component
export default class LoginModal extends Vue {
  @Prop(Boolean) readonly hide: types.Nullable<boolean> | undefined

  snakeName = names.getOne()
  hideLocal = this.$props.hide
  kill = this.$props.hide

  @Watch('hide')
  async onHideChange (val: boolean) {
    if (val) {
      this.kill = false
      this.hideLocal = true
      await this.$sleep(500)
      this.kill = true
    } else {
      this.kill = false
      this.hideLocal = true
      await this.$sleep()
      this.kill = true
      await this.$sleep(500)
    }
  }

  async submit(e: Event) {
    e && e.preventDefault()
    this.$emit('submit', this.snakeName)
  }

}
</script>


<style lang="scss" scoped>
.modal-wrapper {
  font-size: 3em;
  padding: .5em 1em;
  position: absolute;
  background: var(--background-color);
  box-shadow: .2em .2em .5em #0001;
  display: flex;
  flex-direction: column;
  align-content: center;
  justify-content: center;
  $transition: .5s ease;
  transition: transform $transition;
  box-sizing: border-box;

  input {
    outline: none;
    border: none;
    border-bottom: 2px solid var(--background-color);
    width: 100%;
    color: var(--text-color);
    font-size: 2em;
    background-color: transparent;
  }

  // .modal-container {
  // }

  &.hide {
    pointer-events: none;
    transform: translateY(100vh);
    user-select: none;
  }
}

</style>
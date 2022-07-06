<script>
    import { fade } from 'svelte/transition';
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();
    const close = () => dispatch('close');

</script>


<div class="modal" transition:fade="{{duration: 100}}" on:click={close}>
   <slot></slot>
</div>

<style lang="scss">
    .modal {
      position: fixed;
      top: 0;
      left: 0;
      display: flex;
      align-items: center;
      justify-content: center;
      width: 100%;
      height: 100%;
      background: rgba(0,0,0,0.3);
    }
</style>
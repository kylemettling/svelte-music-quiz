<script>
    import {  fly, fade } from "svelte/transition";
    import {createEventDispatcher} from "svelte"

    const dispatch = createEventDispatcher()
    let h, w
</script>

<style lang="scss">
.modal-bg {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, .8);
    .modal {
        background: white;
        padding: 20px;
        border-radius: 15px;
    }
}
</style>


<div class="modal-bg" transition:fade bind:clientHeight={h} bind:clientWidth={w}>

    <div class="modal" transition:fly={{y: 200}}>
        <button on:click={() => {
            dispatch('close')
        }}>Close</button>
        <!-- <slot>optional fallback</slot> -->
        {w}
        {h}
        <slot /> 
        <!-- Generic slot -->
        <!-- <slot name='modal' />  -->
    </div>
</div>
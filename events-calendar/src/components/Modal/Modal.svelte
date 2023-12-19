<script>
  import style from './Modal.module.scss'
	export let showModal;

	let dialog; 

	$: if (dialog && showModal) dialog.showModal();
</script>


<dialog
	bind:this={dialog}
	on:close={() => (showModal = false)}
	on:click|self={() => dialog.close()}
>

	<div on:click|stopPropagation>
    <div class={style.modal__header}>
		<slot name="title"/>
    <button on:click={() => dialog.close()} class={style.modal__closeButton}></button>
    </div>
		<slot />
	</div>

</dialog>

<style>
	
</style>


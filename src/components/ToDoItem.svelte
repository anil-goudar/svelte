<script>
// @ts-nocheck

    let {
        item = {
        isDone: false,
        text: 'This is a TODO item',
    }, deleteItem = () => {}, onStatusChange = () => {}} = $props();



</script>


<div class="flex space-x-2 p-4 align-center w-xl">
    <input type="checkbox" class="cursor-pointer" bind:checked={item.isDone} onchange={(ev) => {
        onStatusChange(item.id, ev.target.checked)
    }}/>
    <p class="self-center">{item.text}</p>
    <button class="text-red-500 p-1 uppercase border border-red-500 rounded-md ml-10 ml-auto cursor-pointer" onclick={() => deleteItem(item.id)}>Delete</button>
</div>
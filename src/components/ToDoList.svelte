<script>
    // @ts-nocheck

    import ToDoItem from "./ToDoItem.svelte";

    let items = $state([]);
    let newTodoText = $state('');
    let input;

    function updateLocalStorage() {
        const snapshot = JSON.stringify($state.snapshot(items));
        localStorage.setItem('storedtasks', snapshot);
    }

    function readFromLocalStorage() {
        const storedtasks = JSON.parse(localStorage.getItem('storedtasks'));
        if(storedtasks?.length) {
            items = [...storedtasks]
        }
    }

    function addItemToList() {
        const text = input.value;
        if(text.length === 0) return;
        items.push({
            isDone: false,
            text: text,
            id: items.length + 1
        });
        updateLocalStorage();
    }

    $effect(() => {
        readFromLocalStorage();
    })

    function deleteItem(key) {
        const index = items.findIndex(item => item.id === key);
        if(index !== -1) {
            items.splice(index, 1);
            updateLocalStorage();
        }
    }

    function updateStatus(key, isDone) {
        for(let item of items) {
            if(item.id === key) {
                item.isDone = isDone
            }
        }
        updateLocalStorage();
    }

</script>

<div class="w-screen flex flex-col items-center justify-center">
    <div class="p-4 flex space-x-5 border-b-4 border-indigo-500 w-xl">
        <input 
            bind:this={input}
            type="text" 
            name="newTodoText" 
            bind:value={newTodoText} 
            placeholder="Enter text"
            class="w-full border border-grey-200 shadow-md rounded-md px-4 py-2 text-gray-700 placeholder-gray-400 focus:outline-none focus:ring-2 focus:ring-blue-400" 
        />
        <button
            class="p-2 rounded-md border border-purple-900 uppercase text-md font-bold shadow-sm cursor-pointer"
            onclick={addItemToList}
        >
        Add
        </button>
    </div>

    
    {#each items as item (item.id)}
        <ToDoItem {item} {deleteItem} onStatusChange = {updateStatus}/>
    {/each}
</div>

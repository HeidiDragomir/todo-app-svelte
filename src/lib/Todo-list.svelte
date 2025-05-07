<script>
    import editImg from "../assets/edit.png";
    import deleteImg from "../assets/delete.png";
    let {
        tasks,
        toggleDone,
        deleteTask,
        startEditing,
        finishEditing,
        cancelEditing,
        editingTaskId,
    } = $props();
    let editedText = $state("");

    $effect(() => {
        if (editingTaskId !== null) {
            const task = tasks.find((t) => t.id === editingTaskId);
            if (task) {
                editedText = task.text;
            }
        }
    });

    const handleEditSubmit = (id, e) => {
        if (e.key === "Enter") {
            finishEditing(id, editedText);
        } else if (e.key === "Escape") {
            cancelEditing();
        }
    };
</script>

<div class="flex flex-col gap-8 my-8 pb-8">
    {#each tasks as task}
        <div
            class="flex items-center justify-between bg-white text-[#242424] p-4 rounded-lg shadow-md"
        >
            <div class="flex gap-2 flex-grow">
                <div class="flex items-center justify-center">
                    <input
                        type="checkbox"
                        class="cursor-pointer"
                        checked={task.done}
                        onchange={() => toggleDone(task)}
                    />
                </div>
                {#if editingTaskId === task.id}
                    <input
                        class="border border-gray-300 rounded px-2 py-1"
                        bind:value={editedText}
                        onkeydown={(e) => handleEditSubmit(task.id, e)}
                        onblur={() => finishEditing(task.id, editedText)}
                    />
                {:else}
                    <p class:done={task.done}>
                        {task.text}
                    </p>
                {/if}
            </div>

            <div class="flex gap-2">
                {#if !task.done && editingTaskId !== task.id}
                    <button type="button" onclick={() => startEditing(task.id)}>
                        <img
                            src={editImg}
                            alt="Edit"
                            class="w-5 h-5 cursor-pointer"
                        />
                    </button>
                {/if}
                <button type="button" onclick={() => deleteTask(task.id)}>
                    <img
                        src={deleteImg}
                        alt="Delete"
                        class="w-5 h-5 cursor-pointer"
                    />
                </button>
            </div>
        </div>
    {:else}
        <div class="text-center text-gray-500 py-12">
            <p class="text-xl font-semibold">🎉 All tasks completed!</p>
            <p class="text-sm mt-2">Take a break, you deserve it</p>
        </div>
    {/each}
</div>

<style>
    .done {
        text-decoration: line-through;
    }
</style>

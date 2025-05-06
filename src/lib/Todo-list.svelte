<script>
    import { onMount } from "svelte";
    import TodoItem from "./Todo-item.svelte";

    let todos = [];
    let newTaskText = "";

    onMount(() => {
        fetch("http://localhost:3000/todos")
            .then((res) => res.json())
            .then((data) => {
                todos = data;
            });
    });

    const addTask = () => {
        if (!newTaskText.trim()) return;

        let newTask = {
            text: newTaskText,
            status: "to do",
        };

        fetch("http://localhost:3000/todos", {
            method: "POST",
            headers: {
                "Content-type": "application/json",
            },
            body: JSON.stringify(newTask),
        })
            .then((res) => res.json())
            .then((data) => {
                todos = [...todos, data];
                newTaskText = "";
            })
            .catch((err) => console.log(err));
    };

    const handleSubmit = (e) => {
        e.preventDefault();
        addTask();
    };


    // const deleteTask = (id) => {
        
    // }


</script>

<div
    class="flex flex-col w-[500px] min-h-[600px] bg-gray-200 rounded-4xl shadow-lg p-12 relative"
>
    <h2 class="font-semibold text-[#242424] text-2xl">My Todo App</h2>
    <div class="flex flex-col mt-4">
        <!-- FORM -->
        <form class="relative" onsubmit={handleSubmit}>
            <input
                type="text"
                id="text"
                bind:value={newTaskText}
                class="w-full border-2 text-[#242424] border-gray-300 rounded-lg p-2 my-4"
                placeholder="Add a new task..."
            />
            <button type="submit">
                <img
                    src="../assets/add.png"
                    alt="Add"
                    class="absolute right-5 top-7 w-5 h-5 cursor-pointer"
                />
            </button>
        </form>

        <ul class="mb-8">
            {#each todos as todo}
                <li>
                    <TodoItem {todo} />
                </li>
            {:else}
                <p class="font-semibold text-[#242424]">Loading ...</p>
            {/each}
        </ul>
    </div>
    <div class="text-[#242424] text-left py-12 absolute bottom-0">
        <p class="font-semibold text-[#242424]">Pending Tasks: 3</p>
    </div>
</div>

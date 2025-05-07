<script>
    import { onMount } from "svelte";
    import TodoList from "./lib/Todo-list.svelte";
    import TodoForm from "./lib/Todo-form.svelte";

    let tasks = $state([]);
    let pendingTasks = $derived(
        tasks.reduce((total, task) => total + Number(task.done === false), 0)
    );
    let editingTaskId = $state(null);

    onMount(() => {
        fetch("http://localhost:3000/tasks")
            .then((res) => res.json())
            .then((data) => {
                console.log(data);

                tasks = data;
            });
    });

    const addTask = (newTask) => {
        if (!newTask.trim()) return;

        fetch("http://localhost:3000/tasks", {
            method: "POST",
            headers: {
                "Content-type": "application/json",
            },
            body: JSON.stringify({
                text: newTask,
                done: false,
            }),
        })
            .then((res) => res.json())
            .then((data) => {
                tasks.push(data);
            })
            .catch((err) => console.log(err));
    };

    const toggleDone = (task) => {
        const updatedTask = { ...task, done: !task.done };
        updateTask(updatedTask);
    };

    const updateTask = (updated) => {
        fetch(`http://localhost:3000/tasks/${updated.id}`, {
            method: "PUT",
            headers: {
                "Content-type": "application/json",
            },
            body: JSON.stringify(updated),
        })
            .then((res) => res.json())
            .then((data) => {
                tasks = tasks.map((t) => (t.id === data.id ? data : t));
            })
            .catch((err) => console.log(err));
    };

    const deleteTask = (id) => {
        fetch(`http://localhost:3000/tasks/${id}`, {
            method: "DELETE",
        })
            .then((res) => {
                if (!res.ok) {
                    throw new Error("Failed to delete");
                }
                tasks = tasks.filter((t) => t.id !== id);
            })
            .catch((err) => console.log(err));
    };

    const startEditing = (id) => {
        editingTaskId = id;
    };

    const finishEditing = (id, newText) => {
        if (newText.trim()) {
            const task = tasks.find((t) => t.id === id);
            if (task) {
                const updatedTask = { ...task, text: newText };
                updateTask(updatedTask);
            }
        }
        editingTaskId = null;
    };

    const cancelEditing = () => {
        editingTaskId = null;
    };
</script>

<main>
    <div class="flex min-h-screen items-center justify-center">
        <div
            class="flex flex-col w-[500px] min-h-[600px] bg-gray-200 rounded-4xl shadow-lg p-12 relative"
        >
            <h2 class="font-semibold text-[#242424] text-2xl">My Todo App</h2>
            <TodoForm {addTask} />
            <TodoList
                {tasks}
                {toggleDone}
                {deleteTask}
                {startEditing}
                {finishEditing}
                {cancelEditing}
                {editingTaskId}
            />
            <div class="text-[#242424] text-left py-12 absolute bottom-0">
                {#if tasks.length > 0}
                    <p class="font-semibold text-[#242424]">
                        Pending Tasks: {pendingTasks}
                    </p>
                {/if}
            </div>
        </div>
    </div>
</main>

<style>
</style>

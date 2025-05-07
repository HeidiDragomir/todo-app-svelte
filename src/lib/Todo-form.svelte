<script>
    import addImg from "../assets/add.png";
    let newTask = $state("");

    let { addTask } = $props();

    const handleSubmit = (e) => {
        e.preventDefault();
        addTask(newTask);
        newTask = "";
    };
</script>

<form class="relative mt-12" onsubmit={handleSubmit}>
    <input
        type="text"
        name="newTask"
        placeholder="Add a new task..."
        class="w-full border-2 text-[#242424] border-gray-300 rounded-lg p-2"
        bind:value={newTask}
    />

    <button>
        <img
            src={addImg}
            alt="Add"
            class="absolute right-5 top-3 w-5 h-5 cursor-pointer"
        />
    </button>
</form>

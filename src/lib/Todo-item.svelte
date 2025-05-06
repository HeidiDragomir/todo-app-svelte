<script>
    let { todo } = $props();
</script>

<div
    class="flex items-center justify-between bg-white text-[#242424] p-4 rounded-lg shadow-md my-8"
>
    <div class="flex gap-2">
        <div class="flex items-center">
            <!-- <img
                src="../assets/checked.png"
                alt="Checked"
                class="w-4 h-4 cursor-pointer"
            /> -->
            <input type="radio" class="cursor-pointer" />
        </div>

        <div>
            <p>{todo.text}</p>
        </div>
    </div>

    <div class="flex gap-2">
        <button>
            <img
                src="../assets/edit.png"
                alt="Edit"
                class="w-5 h-5 cursor-pointer"
            />
        </button>
        <button>
            <img
                src="../assets/delete.png"
                alt="Delete"
                class="w-5 h-5 cursor-pointer"
            />
        </button>
    </div>
</div>

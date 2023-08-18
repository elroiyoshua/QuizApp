<script setup>
import IconTrash from "../components/icons/IconTrash.vue";
const emit = defineEmits(["remove"]);

defineProps({
    todo: {
        type: Object,
        required: true,
    },
});
</script>

<template>
    <div class="flex items-center justify-between border mb-2">
        <div class="mb-2">
            <input
                class="line-through m-3 px-10"
                type="checkbox"
                v-model="todo.completed"
            />
            <span class="px-2" :class="{ 'line-through': todo.completed }"
                >{{ todo.text }}
            </span>
        </div>
        <button
            class="rounded-full h-10 w-10 hover:bg-gray-300 dark:hover:bg-gray-700 transition-colors duration-200"
            @click="emit('remove', todo)"
        >
            <div class="flex h-full w-full items-center justify-center">
                <IconTrash class="w-5 h-5 text-blue-500" />
            </div>
        </button>
    </div>
</template>

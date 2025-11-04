<template>
    <div class="min-h-screen bg-amber-50 py-16 px-4 sm:px-6 lg:px-8">
        <div class="max-w-4xl mx-auto">
            <div class="text-center mb-12">
                <h1 class="text-6xl font-extrabold text-stone-800 mb-4">
                    Draft a New Post
                </h1>
                <p class="text-stone-600 text-xl">
                    Share your unique insights with our community ✨
                </p>
            </div>

            <div
                class="bg-stone-100 border border-stone-300 rounded-3xl shadow-xl p-10 lg:p-12"
            >


<form @submit.prevent="submit">
                    <div class="mb-8">
                        <label
                            for="title"
                            class="block text-lg font-semibold text-stone-700 mb-3"
                        >
                            Post Title
                        </label>
                        <input
                            id="title"
                            v-model="form.title"
                            type="text"
                            class="w-full px-5 py-3.5 bg-white border-2 border-stone-300 text-stone-800 rounded-xl focus:ring-4 focus:ring-orange-500 focus:border-orange-500 transition-all placeholder-stone-400 text-lg"
                            placeholder="A compelling title..."
                            required
                        />
                        <p
                            v-if="errors.title"
                            class="mt-2 text-sm text-red-500"
                        >
                            {{ errors.title }}
                        </p>
                    </div>

                    <div class="mb-8">
                        <label
                            for="category"
                            class="block text-lg font-semibold text-stone-700 mb-3"
                        >
                            Category
                        </label>
                        <select
                            id="category"
                            v-model="form.category_id"
                            class="w-full px-5 py-3.5 bg-white border-2 border-stone-300 text-stone-800 rounded-xl focus:ring-4 focus:ring-orange-500 focus:border-orange-500 transition-all text-lg appearance-none"
                            required
                        >
                            <option value="" disabled>Select a category</option>
                            <option
                                v-for="category in categories"
                                :key="category.id"
                                :value="category.id"
                            >
                                {{ category.name }}
                            </option>
                        </select>
                        <p
                            v-if="errors.category_id"
                            class="mt-2 text-sm text-red-500"
                        >
                            {{ errors.category_id }}
                        </p>
                    </div>

                    <div class="mb-8">
                        <label
                            for="tags"
                            class="block text-lg font-semibold text-stone-700 mb-3"
                        >
                            Tags (comma-separated)
                        </label>
                        <input
                            id="tags"
                            v-model="form.tags"
                            type="text"
                            class="w-full px-5 py-3.5 bg-white border-2 border-stone-300 text-stone-800 rounded-xl focus:ring-4 focus:ring-orange-500 focus:border-orange-500 transition-all placeholder-stone-400 text-lg"
                            placeholder="e.g., Laravel, Vue, TailwindCSS"
                        />
                    </div>

                    <div class="mb-10">
                        <label
                            for="body"
                            class="block text-lg font-semibold text-stone-700 mb-3"
                        >
                            Post Content
                        </label>
                        <textarea
                            id="body"
                            v-model="form.body"
                            rows="12"
                            class="w-full px-5 py-4 bg-white border-2 border-stone-300 text-stone-800 rounded-xl focus:ring-4 focus:ring-orange-500 focus:border-orange-500 transition-all resize-none placeholder-stone-400 text-lg"
                            placeholder="Write the full post content here..."
                            required
                        ></textarea>
                        <p v-if="errors.body" class="mt-2 text-sm text-red-500">
                            {{ errors.body }}
                        </p>
                    </div>

                    <div class="flex gap-4">
                        <button
                            type="submit"
                            :disabled="form.processing"
                            class="flex-1 bg-orange-600 hover:bg-orange-700 text-white font-bold py-3.5 px-8 rounded-xl transition-all duration-300 shadow-lg hover:shadow-xl hover:scale-[1.01] disabled:opacity-50 disabled:cursor-not-allowed flex items-center justify-center text-lg"
                        >
                            <svg
                                v-if="form.processing"
                                class="animate-spin h-5 w-5 mr-3"
                                xmlns="http://www.w3.org/2000/svg"
                                fill="none"
                                viewBox="0 0 24 24"
                            >
                                <circle
                                    class="opacity-25"
                                    cx="12"
                                    cy="12"
                                    r="10"
                                    stroke="currentColor"
                                    stroke-width="4"
                                ></circle>
                                <path
                                    class="opacity-75"
                                    fill="currentColor"
                                    d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
                                ></path>
                            </svg>
                            {{
                                form.processing ? "Creating..." : "Publish Post"
                            }}
                        </button>

                        <a
                            :href="route('posts.index')"
                            class="flex-1 bg-stone-300 hover:bg-stone-400 text-stone-800 font-semibold py-3.5 px-8 rounded-xl transition-all duration-300 text-center text-lg flex items-center justify-center"
                        >
                            Cancel
                        </a>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script setup>
import { useForm } from "@inertiajs/vue3";

const props = defineProps({
    categories: {
        type: Array,
        required: true,
    },
    errors: {
        type: Object,
        default: () => ({}),
    },
});

const form = useForm({
    title: "",
    body: "",
    category_id: "",
    tags: "",
});

const submit = () => {
    form.transform((data) => ({
        ...data,
        tags: data.tags.split(',').map(tag => tag.trim()).filter(tag => tag.length > 0),
    })).post(route("posts.store"), {
        onSuccess: () => {
            form.reset();
        },
    });
};
</script>

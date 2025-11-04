<template>
    <div class="min-h-screen bg-amber-50 py-16 px-4 sm:px-6 lg:px-8">
        <div class="max-w-5xl mx-auto">
            <div
                v-if="$page.props.flash?.success"
                class="fixed top-5 right-5 z-50 max-w-md bg-teal-100 border border-teal-400 text-teal-700 px-6 py-4 rounded-xl shadow-lg flex items-center justify-between"
            >
                <div class="flex items-center">
                    <svg
                        class="w-6 h-6 mr-3 text-teal-500"
                        fill="currentColor"
                        viewBox="0 0 20 20"
                    >
                        <path
                            fill-rule="evenodd"
                            d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z"
                            clip-rule="evenodd"
                        />
                    </svg>
                    <p class="font-semibold">{{ $page.props.flash.success }}</p>
                </div>
            </div>

            <div class="text-center mb-12">
                <h1 class="text-6xl font-extrabold text-stone-800 mb-4">The Blog Feed</h1>
                <p class="text-xl text-stone-600">Read the latest articles from our community.</p>
            </div>

            <div class="mb-10 flex justify-end">
                <a
                    :href="route('posts.create')"
                    class="inline-flex items-center bg-orange-600 hover:bg-orange-700 text-white font-bold py-3.5 px-8 rounded-xl transition-transform transform hover:scale-[1.01] shadow-lg"
                >
                    <svg
                        class="w-5 h-5 mr-2"
                        fill="none"
                        stroke="currentColor"
                        viewBox="0 0 24 24"
                    >
                        <path
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            stroke-width="2"
                            d="M12 6v6m0 0v6m0-6h6m-6 0H6"
                        />
                    </svg>
                    New Post
                </a>
            </div>

            <div
                v-if="posts.length > 0"
                class="space-y-6"
            >
                <div
                    v-for="post in posts"
                    :key="post.id"
                    class="bg-stone-100 border border-stone-300 rounded-2xl shadow-md hover:shadow-xl transition-all duration-300 overflow-hidden transform hover:translate-y-[-2px]"
                >
                    <div class="p-8">
                        <div class="flex items-start justify-between mb-4">
                            <h2 class="text-3xl font-bold text-stone-800 hover:text-orange-600 transition-colors leading-snug pr-4">
                                {{ post.title }}
                            </h2>
                            <span class="text-sm text-stone-500 flex items-center flex-shrink-0 mt-1">
                                <svg
                                    class="w-4 h-4 mr-1.5 text-stone-400"
                                    fill="currentColor"
                                    viewBox="0 0 20 20"
                                >
                                    <path
                                        fill-rule="evenodd"
                                        d="M6 2a1 1 0 00-1 1v1H4a2 2 0 00-2 2v10a2 2 0 002 2h12a2 2 0 002-2V6a2 2 0 00-2-2h-1V3a1 1 0 10-2 0v1H7V3a1 1 0 00-1-1zm0 5a1 1 0 000 2h8a1 1 0 100-2H6z"
                                        clip-rule="evenodd"
                                    />
                                </svg>
                                {{ formatDate(post.created_at) }}
                            </span>
                        </div>

                        <span class="inline-block bg-orange-200 text-orange-900 text-sm font-semibold mb-3 px-3 py-1 rounded-full">
                            {{ post.category.name }}
                        </span>

                        <!-- Tags Display -->
                        <div
                            v-if="post.tags && post.tags.length > 0"
                            class="flex flex-wrap gap-2 mb-6"
                        >
                            <span
                                v-for="tag in post.tags"
                                :key="tag.id"
                                class="inline-flex items-center bg-purple-600 text-white px-2 py-1 rounded-full text-xs font-semibold"
                            >
                                🏷 {{ tag.name }}
                            </span>
                        </div>

                        <p class="text-stone-700 mb-6 line-clamp-3 leading-relaxed">
                            {{ post.body }}
                        </p>

                        <div class="flex gap-6 pt-2 border-t border-stone-200">
                            <a
                                :href="route('posts.edit', post.id)"
                                class="inline-flex items-center text-md font-medium text-orange-600 hover:text-orange-800 transition-colors"
                            >
                                <svg
                                    class="w-5 h-5 mr-1"
                                    fill="none"
                                    stroke="currentColor"
                                    viewBox="0 0 24 24"
                                >
                                    <path
                                        stroke-linecap="round"
                                        stroke-linejoin="round"
                                        stroke-width="2"
                                        d="M15.232 5.232l3.536 3.536m-2.036-5.036a2.5 2.5 0 113.536 3.536L6.5 21.036H3v-3.5L15.232 5.232z"
                                    />
                                </svg>
                                Edit Post
                            </a>
                            <button
                                @click="deletePost(post.id)"
                                class="inline-flex items-center text-md font-medium text-red-600 hover:text-red-800 transition-colors"
                            >
                                <svg
                                    class="w-5 h-5 mr-1"
                                    fill="none"
                                    stroke="currentColor"
                                    viewBox="0 0 24 24"
                                >
                                    <path
                                        stroke-linecap="round"
                                        stroke-linejoin="round"
                                        stroke-width="2"
                                        d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"
                                    />
                                </svg>
                                Delete
                            </button>
                        </div>
                    </div>
                </div>
            </div>

            <div
                v-else
                class="text-center py-20 bg-stone-100 rounded-2xl border border-stone-300 shadow-lg"
            >
                <svg
                    class="mx-auto h-20 w-20 text-stone-400 mb-6"
                    fill="none"
                    stroke="currentColor"
                    viewBox="0 0 24 24"
                >
                    <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M9 13h6m-3-3v6m-9 1V7a2 2 0 012-2h6l2 2h6a2 2 0 012 2v8a2 2 0 01-2 2H5a2 2 0 01-2-2z"
                    />
                </svg>
                <h3 class="text-3xl font-bold text-stone-800 mb-3">No Posts Yet</h3>
                <p class="text-stone-500 text-lg">Be the first to create a new post and share your story!</p>
            </div>
        </div>
    </div>
</template>

<script setup>
import { useForm } from "@inertiajs/vue3";

const props = defineProps({
    posts: {
        type: Array,
        default: () => [],
    },
});

const formatDate = (date) => {
    return new Date(date).toLocaleDateString("en-US", {
        year: "numeric",
        month: "long",
        day: "numeric",
    });
};

const deletePost = (postId) => {
    if (confirm("Are you sure you want to delete this post?")) {
        const form = useForm({});
        form.delete(route("posts.destroy", postId));
    }
};
</script>

<style scoped>
.line-clamp-3 {
    display: -webkit-box;
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;
    overflow: hidden;
}
</style>

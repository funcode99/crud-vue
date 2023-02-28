<script>
    export default {
        // harus ada provide
        inject:['blogs', 'deleteBlog', 'editBlog']
    }
</script>

<template>
    <title-page>Daftar Blog</title-page>

    <div class="grid grid-cols-2 gap-2">
        <div class="border-2 border-black p-3" v-for="blog in blogs" :key="blog.title">
            <div class="flex justify-between items-center">
                <div>
                    <h1>Judul Blog = {{ blog.title }}</h1>
                    <h1>Penulis = {{ blog.name }}</h1>
                </div>
                <div>
                    <blue-button @click="editBlog(blog)">Ubah</blue-button>
                    <span class="mx-1"></span>
                    <red-button @click="deleteBlog(blog.title)">Hapus</red-button>
                </div>
            </div>
            <h1 class="mt-[30px]">Isi blog = {{ blog.blogContent }}</h1>
        </div>
    </div>

</template>
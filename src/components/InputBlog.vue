<script>
    export default {
        inject: ['blogs' , 'saveBlog', 'setActivePage'],
        data() {
            return {
                title: '',
                name: '',
                blogContent: '',
                blogId: this.blogs.length,
                isTitle: false,
                isName: false,
                isBlogContent: false,
                isConfirmDialog: false,
                isAlertDialog: false,
                TitleMessage: '',
                NameMessage: '',
                BlogContentMessage: '',
                message: ''
            }
        },
        methods: {
            submitBlog() {
                if(!this.title.trim()) {
                    this.isTitle = true
                    this.TitleMessage = 'Judul Wajib diisi'
                } else {
                    this.isTitle = false
                }
                if(!this.name.trim()) {
                    this.isName = true
                    this.NameMessage = 'Nama Wajib diisi'
                } else {
                    this.isName = false
                }
                if(!this.blogContent.trim()) {
                    this.isBlogContent = true
                    this.BlogContentMessage = 'Isi Blog Wajib diisi'
                } else {
                    this.isBlogContent = false
                }

                if(!this.isTitle && !this.isName && !this.isBlogContent) {
                    let searchingBlog = this.blogs.find(blog => blog.title === this.title)
                
                    if (searchingBlog) {
                        this.isAlertDialog = true
                        this.message = `Judul ${ this.title } sudah ada!`
                    } else {
                        this.saveBlog(this.title, this.name, this.blogContent, this.blogId)
                        this.isConfirmDialog = true
                        this.message = 'Blog baru berhasil disimpan.'
                        this.title = ''
                        this.name = ''
                        this.blogContent = ''
                        this.blogId = ''
                    }
                }

            },
            closeConfirmDialog() {
                this.setActivePage('show-blog')
                this.isConfirmDialog = false
            },
            closeAlertDialog() {
                this.isAlertDialog = false
            }
        }
    }
</script>

<template>

    <title-page>Input Blog</title-page>
    <form @submit.prevent="submitBlog" class="border w-1/2 mx-auto p-3 rounded shadow">

        <div class="mb-3">
            <label class="block my-1" for="">Judul Blog</label>
            <input :class="[isTitle ? 'border-red-600' : 'border-gray-200']" class="border px-2 py-1 rounded shadow w-full" type="text" v-model="title">
            <warning-message v-if="isTitle">{{ TitleMessage }}</warning-message>
        </div>

        <div class="mb-3">
            <label class="block my-1" for="">Penulis</label>
            <input :class="[isName ? 'border-red-600' : 'border-gray-200']" class="border px-2 py-1 rounded shadow w-full" type="text" v-model="name">
            <warning-message v-if="isName">{{ NameMessage }}</warning-message>
        </div>

        <div class="mb-3">
            <label class="block my-1" for="">Isi Blog</label>
            <textarea cols="30" rows="10" :class="[isBlogContent ? 'border-red-600' : 'border-gray-200']" class="border px-2 py-1 rounded shadow w-full" type="text" v-model="blogContent"></textarea>
            <warning-message v-if="isBlogContent">{{ BlogContentMessage }}</warning-message>
        </div>

        <blue-button>Simpan</blue-button>

    </form>
    
    <teleport to="body">
        <confirm-dialog v-if="isConfirmDialog">
            <template v-slot:title>Informasi</template>
            <template v-slot:message>{{ message }}</template>
            <template v-slot:buttons>
                <blue-button @click="closeConfirmDialog">Tutup</blue-button>
            </template>
        </confirm-dialog>
        <confirm-dialog v-if="isAlertDialog">
            <template v-slot:title>Informasi</template>
            <template v-slot:message>{{ message }}</template>
            <template v-slot:buttons>
                <blue-button @click="closeAlertDialog">Tutup</blue-button>
            </template>
        </confirm-dialog>
    </teleport>

</template>
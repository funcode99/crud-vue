<script>
  import { computed } from 'vue'
  import InputBlog from '@/components/InputBlog.vue'
  import ShowBlog from '@/components/ShowBlogs.vue'
  import EditBlog from '@/components/EditBlog.vue'

  export default {
    components: {
      InputBlog,
      ShowBlog,
      EditBlog
    }, 
    data() {
      return {
        activePage: 'show-blog',
        isConfirmDialog: false,
        deletedTitle: '',
        oldBlog: {
          blogId: '',
          title: '',
          name: '',
          blogContent: ''
        },
        blogs: [
        {
          blogId: 0,
          title: 'Cara cepat menurunkan berat badan',
          name: 'D Tri Octafian',
          blogContent: 'Berat badan yang berlebihan bisa memicu berbagai jenis penyakit, termasuk penyakit jantung. Menurunkan berat badan tidak sekadar mengurangi porsi makan. Mayo Clinic menjelaskan bahwa menurunkan berat badan harus diimbangi dengan perubahan kebiasaan agar hasilnya permanen. Ketahui cara untuk menurunkan berat badan secara alami yang disarikan dari Medical News Today dan NHS berikut ini.'
        },
        {
          blogId: 1,
          title: 'Asah otak dengan bermain catur!',
          name: 'Yuni Fitriani',
          blogContent: 'Ada beragam manfaat bermain catur, terutama untuk kesehatan otak dan mental. Salah satu permainan yang banyak digemari orang ini diketahui baik untuk meningkatkan daya konsentrasi hingga mengasah kreativitas dan kecerdasan seseorang. Catur adalah salah satu jenis permainan strategi yang dilakukan di atas sebuah papan berpola kotak-kotak dan umumnya dimainkan oleh 2 orang. Meski tidak memerlukan aktivitas fisik yang berat, catur dianggap sebagai salah satu cabang olahraga yang bisa memberikan manfaat bagi kesehatan.'
        },
        {
          blogId: 2,
          title: 'Ini loh mamalia terbesar di dunia',
          name: 'Muhammad Rizki Wahid El Dzikri',
          blogContent: 'Paus biru Antartika (Balaenoptera musculus) menjadi hewan terbesar di dunia yang diketahui saat ini. Beratnya bisa mencapai 400.000 pon (sekitar 181.436 kg) atau setara 33 gajah dan panjangnya hampir 30 meter.Mamalia laut yang tergolong dalam subordo paus balin ini hidup di laut Antartika yang sangat dingin. Menurut laporan World Wildlife Fund (WWF), selama musim dingin, paus biru Antartika akan mengonsumsi sekitar 3.599 kg krill per hari.'
        },
        {
          blogId: 3,
          title: 'Ini loh tips hemat uang di akhir bulan, kamu harus tau!',
          name: 'Adinda Ulfa',
          blogContent: 'Setiap orang pasti ingin mempunyai finansial yang stabil. Namun, kekuatan finansial seseorang tak akan ada artinya jika tidak dengan melakukan penghematan. Cara berhemat uang tiap orang pastilah berbeda. Hal ini tentunya didasari oleh pendapatan dan pengeluaran yang berbeda dari masing-masing orang. Meski demikian, hal tersebut seharusnya tidak menjadi penghalang untuk berhemat. Dengan melakukan penghematan, kamu akan terhindar dari masalah tidak punya uang hingga akhir bulan. Agar keuangan kamu tetap sehat, simak cara berhemat dan menabung di bawah ini!'
        },
        {
          blogId: 4,
          title: 'Biografi Isaac Newton, bapak fisika klasik',
          name: 'Lamia Adreena',
          blogContent: 'Sir Isaac Newton FRS (lahir di Woolsthorpe-by-Colsterworth, Lincolnshire, 4 Januari 1643 – meninggal 31 Maret 1727 pada umur 84 tahun; KJ: 25 Desember 1642 – 20 Maret 1727) adalah seorang fisikawan, matematikawan, pandai astronomi, filsuf alam, alkimiawan, dan teolog yang berasal dari Inggris. Dia merupakan pengikut arus heliosentris dan ilmuwan yang sangat berpengaruh sepanjang sejarah, bahkan dituturkan sebagai bapak ilmu fisika klasik'
        },
      ]
      }
    },
    provide() {
      return {
        // ngambil dari data.blogs
        blogs: computed(() => this.blogs),
        blog: computed(() => this.oldBlog),
        // ngambil dari methods.deleteBlog
        deleteBlog: this.deleteBlog,
        saveBlog: this.saveBlog,
        editBlog: this.editBlog,
        updateBlog: this.updateBlog,
        setActivePage: this.setActivePage
      }
    },  
    methods: {
      setActivePage(page) {
        this.activePage = page
      },
      deleteBlog(deletedTitle) {
        this.isConfirmDialog = true
        this.deletedTitle = deletedTitle
      },
      deletingBlog() {
        this.blogs = this.blogs.filter(blog => blog.title !== this.deletedTitle)
        this.isConfirmDialog = false
      },
      cancelDelete() {
        this.isConfirmDialog = false
        this.deletedTitle = ''
      },
      saveBlog(enteredTitle, enteredName, enteredBlogContent, blogId) {
        const newBlog = {
          title: enteredTitle,
          name: enteredName,
          blogContent: enteredBlogContent,
          blogId: blogId
        }
        this.blogs.push(newBlog)
      },
      editBlog(editedBlog) {
        this.activePage = 'edit-blog'
        this.oldBlog = {
          blogId : editedBlog.blogId,
          title: editedBlog.title,
          name: editedBlog.name,
          blogContent: editedBlog.blogContent
        }
      },
      updateBlog(updatedTitle, updatedName, updatedBlogContent, blogId) {
        let searchingBlog = this.blogs.find(blog => blog.blogId === blogId)
        searchingBlog.name = updatedName
        searchingBlog.blogContent = updatedBlogContent
        searchingBlog.title = updatedTitle
      }
    }
  }
</script>

<template>
  <header class="w-screen bg-red-100 px-3 py-2 grid grid-cols-2">

    <div></div>

    <div>
      <menu-button @click="setActivePage('show-blog')" :class="[activePage === 'show-blog' ? 'text-black' : 'bg-blue-600 text-white']">Daftar Blog</menu-button>
      <span class="mx-1"></span>
      <menu-button @click="setActivePage('input-blog') " :class="[activePage === 'input-blog' ? 'text-black' : 'bg-blue-600 text-white']">Input Blog</menu-button>
    </div>

  </header>

  <div id="content" class="container mx-auto py-2">
    <!-- show-blogs & input-blog adalah component -->
    <!-- <keep-alive>
    </keep-alive> -->
    <component :is="activePage"></component>
  </div>

  <confirm-dialog v-if="isConfirmDialog">
    <template v-slot:title>Konfirmasi</template>
    <template v-slot:message>Yakin data akan dihapus?</template>
    <template v-slot:buttons>
        <red-button @click="deletingBlog">Ya</red-button>
        <span class="mx-1"></span>
        <blue-button @click="cancelDelete">Batal</blue-button>
    </template>
  </confirm-dialog>


</template>
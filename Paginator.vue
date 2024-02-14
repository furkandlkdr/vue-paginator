<template>
  <div>
    <div id="data-grid">
      <div class="data-row">
        <span class="item-col">id</span>
        <span class="item-col">title</span>
        <span class="item-col">description</span>
        <span class="item-col">stock</span>
        <span class="item-col">isAvailable</span>
      </div>
      <div
        class="data-row"
        v-for="(item, index) in currentPageData"
        :key="index"
      >
        <span class="item-col">{{ index }}</span>
        <span class="item-col">{{ item.title }}</span>
        <span class="item-col">{{ item.descrip }}</span>
        <span class="item-col">{{ item.stock }}</span>
        <span class="item-col">{{ item.isAvailable }}</span>
      </div>
    </div>
    <div style="display: flex">
      <div id="select-number-item-display">
        <button
          :class="[
            'pagination-button',
            { 'active-page': item === this.itemPerPage },
          ]"
          @click="changeItemPerPage(item)"
          v-for="(item, index) in itemPerPageArray"
          :key="index"
        >
          {{ item }}
        </button>
      </div>
      <div
        id="pagination-row"
        style="margin-left: 30%"
        v-if="this.currentPageData.length < this.data.length"
      >
        <button
          :class="[
            'pagination-button',
            { 'disabled-page': this.currentPage === 1 },
          ]"
          @click="changeCurrentPage(1)"
        >
          &lt;
        </button>
        <span v-for="page in this.pageNumber" :key="page">
          <button
            :class="[
              'pagination-button',
              { 'active-page': page === this.currentPage },
            ]"
            @click="getPageData(page)"
          >
            {{ page }}
          </button>
        </span>
        <button
          :class="[
            'pagination-button',
            { 'disabled-page': this.currentPage === this.pageNumber },
          ]"
          @click="changeCurrentPage(2)"
        >
          >
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Paginator',
  data() {
    return {
      data: [
        {
          title: 'Hello W',
          descrip: 'trying to',
          stock: 1453,
          isAvailable: true,
        },
        {
          title: 'Lorem Ipsum',
          descrip: 'Lorem ipsum dolor sit amet',
          stock: 100,
          isAvailable: true,
        },
        {
          title: 'Sample Title',
          descrip: 'This is a sample description',
          stock: 500,
          isAvailable: false,
        },
        {
          title: 'Data Object 4',
          descrip: 'Description 4',
          stock: 200,
          isAvailable: true,
        },
        {
          title: 'Data Object 5',
          descrip: 'Description 5',
          stock: 300,
          isAvailable: false,
        },
        {
          title: 'Data Object 6',
          descrip: 'Description 6',
          stock: 400,
          isAvailable: true,
        },
        {
          title: 'Data Object 7',
          descrip: 'Description 7',
          stock: 150,
          isAvailable: true,
        },
        {
          title: 'Data Object 8',
          descrip: 'Description 8',
          stock: 250,
          isAvailable: false,
        },
        {
          title: 'Data Object 9',
          descrip: 'Description 9',
          stock: 350,
          isAvailable: true,
        },
        {
          title: 'Data Object 10',
          descrip: 'Description 10',
          stock: 450,
          isAvailable: false,
        },
        {
          title: 'Data Object 11',
          descrip: 'Description 11',
          stock: 550,
          isAvailable: true,
        },
        {
          title: 'Data Object 12',
          descrip: 'Description 12',
          stock: 650,
          isAvailable: false,
        },
      ],
      currentPageData: [],
      itemPerPageArray: ['5', '10', '20', 'All'],
      itemPerPage: '5',
      currentPage: 1,
      pageNumber: 1,
    };
  }, // TODO: Tümünü göster ve itemPerPage dropdown ekle onu da string array olarak alırsan daha kolay olur
  // DevExtreme paginatore bak
  methods: {
    getPageCount() {
      if (this.itemPerPage === 'All') {
        //TODO: Burada seçilen değer ile itemperpeage değiştir
        this.currentPageData = this.data;
        return;
      }
      this.pageNumber = Math.ceil(this.data.length / Number(this.itemPerPage));
    },
    changeCurrentPage(val) {
      if (val === 1) {
        if (this.currentPage > 1) {
          this.currentPage--;
        }
      } else {
        if (this.currentPage < this.pageNumber) {
          this.currentPage++;
        }
      }
      this.getPageData(this.currentPage);
    },
    getPageData(index) {
      if (this.itemPerPage === 'All') {
        this.currentPageData = this.data;
        return;
      }
      this.currentPage = index;
      let start = (this.currentPage - 1) * Number(this.itemPerPage);
      let end = start + Number(this.itemPerPage);
      this.currentPageData = this.data.slice(start, end);
    },
    changeItemPerPage(item) {
      this.itemPerPage = item;
      this.getPageCount();
      this.getPageData(1);
    },
  },
  created() {
    this.getPageData(1);
    this.getPageCount();
  },
};
</script>

<style scoped>
.pagination-button {
  padding: 8px;
  margin: 2px;
  border-radius: 3px;
  font-size: 1em;
  cursor: pointer;
}

#pagination-row {
  display: flex;
  flex-wrap: wrap;
}

.data-row {
  display: grid;
  grid-template-columns: 30px 100px 200px 80px 90px;
}

.item-col {
  padding: 8px;
  border: 1px solid #000;
}

.active-page {
  background-color: #007bff;
  color: #fff;
}

.disabled-page {
  opacity: 0.5;
  cursor: auto;
}
</style>

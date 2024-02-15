<template>
  <div>
    <div id="data-grid">
      <div class="data-row"> 
        <!-- This div contains title for data grid -->
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
      <!-- This div contains data rows -->
        <span class="item-col">{{ index }}</span>
        <span class="item-col">{{ item.title }}</span>
        <span class="item-col">{{ item.descrip }}</span>
        <span class="item-col">{{ item.stock }}</span>
        <span class="item-col">{{ item.isAvailable }}</span>
      </div>
    </div>
    <div style="display: flex">
      <!-- Paginator starts here, with first div, you can select how many items you want to display -->
      <!-- It's fetching data from itemPerPageArray for choices, if item's pageValue equals to current choice, make it active. -->
      <div id="select-number-item-display">
        <button
          v-for="(pageValue, index) in itemPerPageArray"
          :key="index"
          :class="[
            'pagination-button',
            { 'active-page': pageValue === this.itemPerPage },
          ]"
          @click="changeItemPerPage(pageValue)"
        >
          {{ pageValue }}
        </button>
      </div>
      <div
        id="pagination-row"
        style="margin-left: 30%"
        v-if="this.currentPageData.length < this.data.length"
      >
      <!-- Change to margin as you like. -->
      <!-- If your current PageData has less data then full data, show page selector -->
      <!-- If your current page is 1, disable previous button, if your current page is last page, disable next button -->
        <button
          :class="[
            'pagination-button',
            { 'disabled-page': this.currentPage === 1 },
          ]"
          @click="changeCurrentPage(1)"
        >
          &lt;
        </button>
        <!-- If element's number equals to currentPage, add active class -->
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
  // This data is a dummy object for you to see how it works, you can replace it with your own data.
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
      // Current page data is empty now, it will be filled with getPageData method.
      currentPageData: [],
      // ItemPerPageArray is an array for you to select how many items you want to display.
      itemPerPageArray: ['5', '10', '20', 'All'],
      // ItemPerPage comes with a default value of 5, it will be changed with changeItemPerPage method.
      itemPerPage: '5',
      // Current page is 1, it will be changed with changeCurrentPage method.
      currentPage: 1,
      // Page number is 1, it will be changed with getPageCount method.
      pageNumber: 1,
    };
  },
  methods: {
    getPageCount() {
      if (this.itemPerPage === 'All') return; 
      // If itemPerPage is All, return and dont calculate page count. Else calculate page count. It will be integer because we gave it a default value of integer
      this.pageNumber = this.data.length / Number(this.itemPerPage);
    },
    changeCurrentPage(val) {
      if (val === 1) 
          if (this.currentPage > 1) this.currentPage--;
      else 
          if (this.currentPage < this.pageNumber) this.currentPage++;
// If val is 1, decrease current page, if val is 2, increase current page. And getPageData of currentPage
      this.getPageData(this.currentPage);
    }, 
    getPageData(index) {
      if (this.itemPerPage === 'All') {
        this.currentPageData = this.data;
        return;
      } // If itemPerPage is All, return all data. Else calculate start and end indexes and slice data array.
      this.currentPage = index;
      let start = (this.currentPage - 1) * Number(this.itemPerPage);
      let end = start + Number(this.itemPerPage);
      this.currentPageData = this.data.slice(start, end);
    },
    changeItemPerPage(item) {
      this.itemPerPage = item;
      this.getPageCount();
      this.getPageData(1);
    }, // Change itemPerPage and call getPageCount and getPageData methods.
  },
  created() {
    this.getPageData(1);
    this.getPageCount();
  }, // Call getPageData and getPageCount methods when component is created. You can change as your API call. (OnMounted or something like that.)
};
</script>

<style scoped>
/* Stiles are just dummy things for you to change as you like */
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

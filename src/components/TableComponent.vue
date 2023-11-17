<template>
      <!-- <div>
        <input
          type="text"
          class="border-2 mb-5 rounded h-10 p-2"
          placeholder="Search records"
          @input="onSearch"
        />
      </div> -->
 
    <!-- <label for="default-search" class="mb-2 text-sm font-medium text-gray-900 sr-only dark:text-white">Search</label> -->
    <div class="relative">
        <div class="absolute inset-y-0 start-0 flex items-center ps-3 pointer-events-none">
            <svg class="w-4 h-4 text-gray-500 dark:text-gray-400" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 20 20">
                <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m19 19-4-4m0-7A7 7 0 1 1 1 8a7 7 0 0 1 14 0Z"/>
            </svg>
        </div>
        <input type="search" id="default-search" @input="onSearch" class="block w-full p-4 ps-10 text-sm text-neutral-900 border border-neutral-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-orange-500 dark:focus:border-orange-500" placeholder="Search for users" required>
    </div>
    <br>
    <div class="relative overflow-x-auto shadow-md sm:rounded-lg">
      <table class="min-w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400"> 
        <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
          <tr>
            <th
              v-for="(column, index) in columns"
              v-bind:key="index"
              scope="col" class="px-6 py-3"
              v-on:click="sortRecords(index)"
            >
            <div class="flex items-center">
              {{column}}
              <a href="#"><svg class="w-3 h-3 ms-1.5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 24 24">
                <path d="M8.574 11.024h6.852a2.075 2.075 0 0 0 1.847-1.086 1.9 1.9 0 0 0-.11-1.986L13.736 2.9a2.122 2.122 0 0 0-3.472 0L6.837 7.952a1.9 1.9 0 0 0-.11 1.986 2.074 2.074 0 0 0 1.847 1.086Zm6.852 1.952H8.574a2.072 2.072 0 0 0-1.847 1.087 1.9 1.9 0 0 0 .11 1.985l3.426 5.05a2.123 2.123 0 0 0 3.472 0l3.427-5.05a1.9 1.9 0 0 0 .11-1.985 2.074 2.074 0 0 0-1.846-1.087Z"/>
              </svg></a>
            </div>
            </th>
            <th scope="col" class="px-6 py-3">
                <span class="sr-only">Edit</span>
            </th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(row, index) in rows"
            v-bind:key="index"
            class="bg-white border-b dark:bg-gray-800 dark:border-gray-600"
          >
            <td
              v-for="(rowItem, itemIndex) in row"
              v-bind:key="itemIndex"
              scope="row" :class="{'px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white':itemIndex==1, 'px-6 py-4':itemIndex!=1}"
            >
              {{rowItem}}
            </td>
            <td class="px-6 py-4 text-right">
              <button v-on:click="displayModal(index)" class="text-blue-500 hover:text-white border border-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-full text-sm px-5 py-2.5 text-center me-2 mb-2 rounded-full dark:border-orange-500 dark:text-orange-500 dark:hover:text-white dark:hover:bg-orange-500 dark:focus:ring-orange-800">View</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
     <div v-if="showModal" class="overflow-x-hidden overflow-y-auto fixed inset-0 z-50 outline-none focus:outline-none justify-center items-center flex">
      <div class="relative w-auto my-6 mx-auto max-w-3xl">
        <!-- content -->
        <div class="border-0 rounded-lg shadow-lg relative flex flex-col w-full bg-slate-500 outline-none focus:outline-none">
          <!-- header -->
          <!-- <div class="flex items-start justify-between p-5 border-b border-solid border-blueGray-200 rounded-t">
            <h3 class="text-3xl font-semibold justify-center">
              {{rawUserData[modalUserIndex]['firstName'] + ' ' + rawUserData[modalUserIndex]['lastName']}}
            </h3>
            <button class="p-1 ml-auto bg-transparent border-0 text-black opacity-5 float-right text-3xl leading-none font-semibold outline-none focus:outline-none" v-on:click="toggleModal()">
              <span class="bg-transparent text-black opacity-5 h-6 w-6 text-2xl block outline-none focus:outline-none">
                ×
              </span>
            </button>
          </div> -->
          <!-- body -->
          <div class="flex flex-row">
            <div class="relative p-6 flex-auto justify-center items-center">
              <img class="flex rounded-full bg-white border-2 border-orange-500 outline-orange" :src="rawUserData[modalUserIndex]['image']" alt="profile picture">
            </div>
            <div class="relative p-6 flex-auto text-white">
              <h3 class="text-3xl font-semibold">{{rawUserData[modalUserIndex]['firstName'] + ' ' + rawUserData[modalUserIndex]['lastName']}}</h3>
              <h3 class="text-lg italic mb-2">@{{rawUserData[modalUserIndex]['username']}}</h3>
              <br>
              <ul class="list-disc pl-15 space-y-2">
                <li class="flex items-center">
                  <img src="@/assets/email.svg" class="w-6 h-6 mr-2">
                  {{rawUserData[modalUserIndex]['email']}}
                </li>
                <li class="flex items-center">
                  <!-- SVG icon (replace 'path/to/icon.svg' with your actual SVG file) -->
                  <img src="@/assets/phone.svg" class="text-white w-6 h-6 mr-2">
                  {{rawUserData[modalUserIndex]['phone']}}
                </li>
                <li class="flex items-center">
                  <!-- SVG icon (replace 'path/to/icon.svg' with your actual SVG file) -->
                  <img src="@/assets/birthday.svg" class="w-6 h-6 mr-2">
                  {{rawUserData[modalUserIndex]['birthDate']}}, {{rawUserData[modalUserIndex]['age']}}yrs
                </li>
                <li class="flex items-center">
                  <!-- SVG icon (replace 'path/to/icon.svg' with your actual SVG file) -->
                  <img src="@/assets/gender.svg" class="w-6 h-6 mr-2">
                  {{rawUserData[modalUserIndex]['gender']}}
                </li>
                <br>
                <li class="flex items-center">
                  <!-- SVG icon (replace 'path/to/icon.svg' with your actual SVG file) -->
                  <img src="@/assets/location.svg" class="w-6 h-6 mr-2">
                  {{rawUserData[modalUserIndex]['address']['address'] + ', ' + rawUserData[modalUserIndex]['address']['city'] + ', ' + rawUserData[modalUserIndex]['address']['postalCode'] + ', ' + rawUserData[modalUserIndex]['address']['state']}}
                </li>
                <!-- Add more list items as needed -->
              </ul>
            </div>
          </div>
          <!-- footer -->
          <div class="flex items-center justify-center p-3 border-t border-solid border-neutral-200 rounded-b">
            <button class="text-white bg-orange-400 hover:bg-orange-500 focus:outline-none focus:ring-4 focus:ring-orange-300 font-medium rounded-full text-sm px-5 py-2.5 text-center dark:focus:ring-orange-900 ease-linear transition-all duration-150" type="button" v-on:click="toggleModal()">
              Close
            </button>
          </div>
        </div>
      </div>
    </div>
    <div v-if="showModal" class="opacity-25 fixed inset-0 z-40 bg-black"></div> 

  </template>
  
  <script>
  
  const performSearch = (rows, term) => {
    const results = rows.filter(
      row => row.join(" ").toLowerCase().includes(term.toLowerCase())
    )
  
    return results;
  }
  
  export default {
    data () {
      return {
        term: '',
        tableRows: [
        ],
        rows: [],
        columns: [
          'Id',
          'Name',
          'Username',
          'Email',
          'Phone',
          'DOB'
        ],
        sortIndex: null,
        sortDirection: null,
        rawUserData:[],
        showModal: false,
        modalUserIndex: 0
      }
    },
    methods: {
      async fetchUsers() {
        try {
          const response = await fetch('https://dummyjson.com/users');
          if (!response.ok) {
            throw new Error('Network response was not ok');
          }
          const data = await response.json();
          this.rawUserData = data['users'];
          console.log(this.rawUserData)
          this.populateRows();
        } catch (error) {
          console.error('Error fetching data:', error);
        }
      },
      async populateRows(){
        // console.log(this.rawUserData[0]['name'])
        for (const user of this.rawUserData){
          // console.log(user)
          var row = []
          for (const col of this.columns){
            if (col == 'Name'){
              row = [...row, user['firstName'] + ' ' + user['lastName']]
            }
            else if (col == 'DOB'){
              row = [...row, user['birthDate']]
            }
            else{
              row = [...row, user[col.toLowerCase()]]
            }
            
          }
          // console.log(row)
          this.tableRows = [...this.tableRows, row]
        }
        // console.log(this.tableRows)
        this.rows = [...this.tableRows];
      },
      sortRecords (index) {
        if (this.sortIndex === index) {
          switch (this.sortDirection) {
            case null:
              this.sortDirection = 'asc';
              break;
            case 'asc':
              this.sortDirection = 'desc';
              break;
            case 'desc':
              this.sortDirection = null;
              break;
          }
        } else {
          this.sortDirection = 'asc'
        }
  
        this.sortIndex = index;
  
        if (!this.sortDirection) {
          this.rows = performSearch(this.tableRows, this.term);
          return;
        }
  
        this.rows = this.rows.sort(
          (rowA, rowB) => {
            if (index == 0){
              return rowA[index] - rowB[index];
            }
            if (this.sortDirection === 'desc') {
              return rowB[index].localeCompare(rowA[index]);
            }
  
            return rowA[index].localeCompare(rowB[index]);
          }
        )
      },
      onSearch (e) {
        this.term = e.target.value;
        this.rows = performSearch(this.tableRows, this.term);
      },
      toggleModal: function(){
        this.showModal = !this.showModal;
      },
      displayModal(index){
        this.modalUserIndex = index;
        this.toggleModal()
      }
    },
    mounted () {
      this.fetchUsers();
    }
  }
  </script>
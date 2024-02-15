<template>
  <!-- Selected Items - Start -->
  <div class="max-w-sm mx-auto space-x-4 border-spacing-2" v-if="multiselect">
    <div class="overflow-hidden">
      <div v-for="(data, i) in selectedItems" class="px-1 m-2 rounded-xl flex items-center justify-between float-left bg-white border-solid border" v-show="(i < 2) || showAll">
        {{  data.title }}
        <span class="close p-2" @click="resetSelection(data)">
          <svg width="7px" height="7px" viewBox="0 0 12 13" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
            <!-- Generator: sketchtool 63.1 (101010) - https://sketch.com -->
            <title>Remove</title>
            <desc>Created with sketchtool.</desc>
            <g id="components" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
                <g id="Table-dropdown-Style" transform="translate(-1036.000000, -327.000000)" fill="#A7B1C6">
                    <g id="Filter_value_selected" transform="translate(696.000000, 255.000000)">
                        <g id="Group-5" transform="translate(0.000000, 53.500000)">
                            <g id="Search" transform="translate(334.000000, 13.000000)">
                                <g id="ic_close" transform="translate(5.445163, 5.445163)">
                                    <path d="M12.0619686,0.928448996 L12.1008161,0.9687893 C12.5137832,1.39746825 12.5137832,2.09262161 12.1008918,2.52137918 L8.16562763,6.60688082 L12.1008918,10.6923825 C12.4862571,11.0925562 12.5119481,11.7247903 12.1779033,12.1558415 L12.1008161,12.2449723 L12.0619686,12.2853127 C11.6766034,12.6854864 11.0677652,12.7121646 10.6526036,12.3653474 L10.5667568,12.2853127 L6.60458526,8.17088625 L2.64813207,12.2793745 C2.26276683,12.6795483 1.65392866,12.7062265 1.23876702,12.3594093 L1.15292027,12.2793745 L1.11287927,12.2377949 C0.727949313,11.8380732 0.701822111,11.2067024 1.03482989,10.7755383 L1.11168578,10.6863677 L5.06007614,6.60688082 L1.11168578,2.52739393 C0.69992028,2.09843697 0.700454316,1.40424004 1.11287927,0.975966777 L1.15292027,0.934387118 C1.56581161,0.505629555 2.23524074,0.505629555 2.64813207,0.934387118 L6.60458526,5.0428754 L10.5667568,0.928448996 C10.9796482,0.499691433 11.6490773,0.499691433 12.0619686,0.928448996 Z" id="Path"></path>
                                </g>
                            </g>
                        </g>
                    </g>
                </g>
            </g>
          </svg>
        </span>
      </div>
      <span class="px-1 m-2 rounded-xl flex items-center justify-between float-left border-solid border bg-slate-200" v-show="selectedItems.length > 2 && !showAll" @click="setShowAll()">{{ '+ ' + (selectedItems.length - 2) + ' More' }}</span>
    </div>
  </div>
  <!-- Selected Items -END -->

  <!-- Search input section - start -->

  <div class="p-2 max-w-sm mx-auto bg-white  shadow-lg flex items-center justify-between	space-x-4 border-spacing-2 border">
      <input 
        type="search" 
        class="border-0 w-full h-10 p-2" 
        :placeholder="placeholder" 
        v-model="singleSelectValue" 
        @input="handleSearch"
      >

      <div class="svg-arrow" @click="showListItem()">
        <svg :class="showList? 'arrow__up': 'arrow__down'"
          fill="#000000" height="20px" width="20px" version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" 
          viewBox="0 0 330 330" xml:space="preserve">
          <path id="XMLID_225_" d="M325.607,79.393c-5.857-5.857-15.355-5.858-21.213,0.001l-139.39,139.393L25.607,79.393
            c-5.857-5.857-15.355-5.858-21.213,0.001c-5.858,5.858-5.858,15.355,0,21.213l150.004,150c2.813,2.813,6.628,4.393,10.606,4.393
            s7.794-1.581,10.606-4.394l149.996-150C331.465,94.749,331.465,85.251,325.607,79.393z"/>
        </svg>
      </div>

  </div>
  <!-- Search input section - end -->
  
  <!-- List section - start-->
  <div class="p-4 max-w-sm mx-auto rounded-xl bg-white shadow-lg border-1 h-auto max-h-52 overflow-auto" v-show="showList">
    <ul>
      <li class="m-5" v-for="(item, index) in propitems" :key="index">
        <span v-if="multiselect">
          <input type="checkbox" v-model="item.selected" :id="index" @change="selectItem(item)" />
          <label :for="index" class="p-6 w-full">{{ item.title }}</label>
        </span>
        <span v-else @click="selectItem(item)">{{ item.title}}</span>
      </li>
      <li class="m-5 flex items-center justify-center" v-if="propitems.length == 0">No Items Found</li>
    </ul>
  </div>
  <!-- List section - end-->
  
  </template>
  
  <script>
  import { ref, watch, onMounted } from 'vue';
  
  export default {
    props: {
      items: {
        type: Array,
        required: true,
      },
      placeholder: {
        type: String,
        default: 'Select an item',
      },
      multiselect: {
        type: Boolean,
        default: false,
      },
      resetDropdown: {
        type: Array
      }
    },
    emits: ['selectedItems', 'searchItem'],
    setup(props, context) {
      const showList = ref(false);
      const selectedItems = ref([]);
      const propitems = ref(props.items);
      const showAll = ref(false);
      const singleSelectValue = ref('');
      const delay = 1000; // 1 second delay

      // Mounted

      onMounted(() => {
        if (props.resetDropdown) {
          if (props.multiselect) {
            let items = [];
            if (props.resetDropdown && props.resetDropdown.length > 0) {
              items = props.resetDropdown.map(itm => {
                return {...itm, selected: true}
              })
            }
            if (propitems.value.length > 0) selectedItems.value = items;
            console.log('selectedItems.value', selectedItems.value)
            updateSelectedItems();
          } else singleSelectValue.value = props.resetDropdown && props.resetDropdown.title;
        }
      });

      // Watch

      watch(() => props.items, (newValue, oldValue) => {
        console.log('called this watch')
        // Create a new array with the modifications
        const updatedValue = newValue.map((item) => ({
          ...item,
          selected: false,
        }));
        updateSelectedItems(updatedValue);
      });

      // Methods
    
      // 1. To open/close the list section
    
      const showListItem = () => {
        showList.value = !showList.value;
      }
    
      // 2. To select each Item - adding the selected one to the selectedItems
    
      const selectItem = (item) => {
        // For multi select dropdown
        if (item && props.multiselect) {
          // Checking the selected item is already exist in the selectedItems;
    
          let checkItem = selectedItems.value.some(e1 => e1.value === item.value)
          if (checkItem) {
            // If already exist in the array, remove it from selectedItems
            resetSelection(item)
          } else {
            // Add new item to selected array
            selectedItems.value.push(item)
          }
          context.emit('selectedItems', selectedItems.value )
          
        } else { // For single select dropdown
          singleSelectValue.value = item.title
          context.emit('selectedItems', item )
        }
      }

      // 3. Reselect the selection

      const resetSelection = (item) => {
        let index = selectedItems.value.findIndex(e1 => e1.value === item.value)
        if (index === 0 || index) {
          selectedItems.value.splice(index, 1)
        }
        // Update the selected value in propitems
        propitems.value.forEach(element => {
          if (element.value === item.value) {
            element.selected = false;
          }
        });

        if (selectedItems.value.length == 0) showAll.value = false;
      }
      
      // 4. Show all the selected items - More button click

      const setShowAll = () => {
        showAll.value = true;
      }

      // 5. Handle search items

      const handleSearch = debounce(() => {
        showList.value = true;
        context.emit('searchItem', singleSelectValue.value)
      }, delay)

      // 6. Updating the selected and the list values on reset / search

      const updateSelectedItems = (updatedValue) => {
        let items = updatedValue ? updatedValue : propitems.value
        propitems.value = items.map(propItm => {
          const matchingItem = selectedItems.value.find(sitm => sitm.value === propItm.value);
          if (matchingItem) return matchingItem;
          else return propItm;
        });
      }


      function debounce(func, wait) {
        let timeout;
        return function (...args) {
          const context = this;
          clearTimeout(timeout);
          timeout = setTimeout(() => {
            func.apply(context, args);
          }, wait);
        };
      }

      return {
        // Data
        showList,
        selectedItems,
        propitems,
        showAll,
        singleSelectValue,

        // Methods
        showListItem,
        selectItem,
        resetSelection,
        setShowAll,
        handleSearch,
        updateSelectedItems
        // selectedItems,
        // toggleDropdown,
        // selectItem,

        // Computed
        // getpropitems
      };
    },
  };
  </script>
  
  <style scoped lang="scss">
  .svg-arrow {
      cursor: pointer;
      svg {
        fill: rgba(60,60,60,.5);
        transition: transform .15s cubic-bezier(1,-.115,.975,.855),-webkit-transform .15s cubic-bezier(1,-.115,.975,.855);
        transition-timing-function: cubic-bezier(1,-.115,.975,.855);
        position: relative;
        right: 8px;
        &.arrow__up {
          transform: rotate(180deg) scale(1);
          path {
            fill: #0978FC;
          }
        }
      }
  }
  input[type="search"]:focus,
  input[type="search"]:focus-visible {
    outline: none;
  }
  </style>
  
<template>
  <div class="autocomplete w-auto" ref="container">
    <div
      class="
        relative
        flex
        items-center
        text-gray-400
        focus-within:text-gray-500
      "
    >
      <SearchIcon
        class="text-gray-500 h-6 w-6 absolute ml-3 pointer-events-none"
      />
      <input
        v-model="searchText"
        @keyup="onTextChange($event.target.value)"
        @keydown.down="onArrowDown"
        @keydown.up="onArrowUp"
        @keydown.enter="onEnter"
        type="text"
        tabindex="arrowCounter"
        class="
          pr-3
          pl-10
          py-2
          font-semibold
          placeholder-gray-500
          border-none
          ring-2 ring-gray-300
          focus:ring-gray-500 focus:ring-2
          rounded-lg
        "
        :class="[
          { 'bg-red-50': isNewVal, 'text-red-500': isNewVal },
          {
            'bg-green-50 text-green-500 border-green-800 font-bold': !isNewVal,
          },
        ]"
        placeholder="Enter Some text"
      />
    </div>
    <ul
      class="
        autocomplete-results
        border-none
        ring-2 ring-gray-400
        h-auto
        z-40
        absolute
        w-64
        ml-10
        mt-1
        rounded-b-xl
        overflow-auto
      "
      v-show="isOpen"
    >
      <li
        v-for="(item,i) in filteredRes"
        :key="i"
        @click="setResults(item)"
        class="
          autocomplete-result
          p-1
          font-medium
          w-full
          hover:text-white hover:bg-blue-700
          cursor-pointer
        "
        :class="{ 'bg-blue-500 text-white': i === arrowCounter }"
      >
        {{ item }}
      </li>
    </ul>
  </div>
</template>

<script>
import { onMounted, reactive,watch,toRefs, onUnmounted } from 'vue';
import {SearchIcon,BadgeCheckIcon,PlusIcon} from '@heroicons/vue/outline'
export default {
  name: 'AutoSuggest',
  props: {
    items: {
      type: Array,
      required: false,
    },
  },
  components:{
    SearchIcon,
    BadgeCheckIcon,
    PlusIcon,
  },
  setup(props, context) {
    const state = reactive({
        searchText: '',
        selectedText: '',
        filteredRes: [],
        isOpen: false,
        isNewVal:false,
        container:[],
        arrowCounter: -1
      });

      const onTextChange = (val) => {
        // console.log(val);
        state.isOpen =val.trim()!='';
        state.searchText = val;
        console.log('isNew',state.isNewVal);
      };

      const filterResults = () => {
        state.filteredRes = props.items.filter(
          (item) =>
            item.toLowerCase().indexOf(state.searchText.toLowerCase()) > -1
        );
      };

      const handleClickOutside=(event)=>{
         if(!state.container.contains(event.target)){
           state.isOpen=false;
           state.arrowCounter=-1;
         }
      }
      const setResults=(result)=>{
        console.log(result)
        state.searchText=result;
        state.isOpen=false;
      }
      const onArrowDown=()=>{
      if (state.arrowCounter < state.filteredRes.length) {
        state.arrowCounter = state.arrowCounter + 1;
        console.log(state.filteredRes[state.arrowCounter])
      }
      }

    const onArrowUp=()=>{
      if (state.arrowCounter > 0) {
        state.arrowCounter = state.arrowCounter - 1;
        console.log(state.filteredRes[state.arrowCounter])
      }
    }

    const onEnter=()=>{
      if (state.arrowCounter>=0){
      setResults(state.filteredRes[state.arrowCounter]);
      state.arrowCounter = -1;
      }else{
        state.isOpen=false;
      }
    }

      watch(()=>state.searchText,(newVal, oldVal) => {
          filterResults();
          state.isNewVal=state.filteredRes.length==0;

      });

    onMounted(() => {
      console.log(state.container);
      state.filteredRes = props.items;
      console.log("Filtered",state.filteredRes);
      document.addEventListener('click',handleClickOutside)
    });

   onUnmounted(()=>{
    document.removeEventListener('click',handleClickOutside)
   });

    return {
      onArrowDown,
      onArrowUp,
      onEnter,
      setResults,
      onTextChange,
      ...toRefs(state),
    };
  },
};
</script>

<style lang="scss" scoped>
// .autocomplete {
//   position: relative;
// }

// .autocomplete-results {
//   padding: 0;
//   margin: 0;
//   border: 1px solid #eeeeee;
//   height: 120px;
//   min-height: 1em;
//   max-height: 6em;
//   overflow: auto;
// }

// .autocomplete-result {
//   list-style: none;
//   text-align: left;
//   padding: 4px 2px;
//   cursor: pointer;
// }

// .autocomplete-result:hover {
//   background-color: #4aae9b;
//   color: white;
//}
</style>

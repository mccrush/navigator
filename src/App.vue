<template>
  <div class="container p-3">
    <div class="row d-none">
      <div class="col-12">
        <h1 class="h3">Block creator for {{ childrens }} elements</h1>
        <div class="d-flex">
          <div class="form-floating">
            <input
              type="number"
              step="1"
              min="1"
              v-model="levels"
              class="form-control form-control-sm"
              id="levels"
            />
            <label for="levels"><small>Количество уровней</small></label>
          </div>

          <div class="form-floating ms-2">
            <input
              type="number"
              step="1"
              min="1"
              v-model="childrens"
              class="form-control form-control-sm"
              id="childrens"
            />
            <label for="childrens"><small>Количество детей</small></label>
          </div>

          <div class="form-floating ms-2">
            <input
              type="number"
              step="1"
              min="1"
              v-model="currentLevel"
              class="form-control form-control-sm"
              id="currentLevel"
              disabled
            />
            <label for="currentLevel"><small>Текущий уровень</small></label>
          </div>
        </div>
      </div>
    </div>

    <div class="row">
      <div class="col-12">
        <button class="btn btn-light" @click="addChild">+ Add child</button>
        <span class="ms-2">{{ 'ID: ' + currentItemId }}</span>
      </div>
    </div>

    <div v-if="arrayForShow.length" class="row">
      <div class="col-12">
        <div
          v-if="arrayForShow.length < 4"
          class="block border border-warning rounded bg-warning mt-3 p-2"
        >
          <div class="d-flex">
            <button
              v-for="element in arrayForShow"
              :key="element"
              class="btn btn-light border rounded-0 flex-fill"
              @click="currentItemId = element.id"
            >
              {{ element.title }}
            </button>
          </div>
        </div>

        <!-- For > 4  -->
        <!-- <div v-else class="block border rounded mt-3 p-2">
          <h4>
            {{ array[currentLevel - 1].title }}
            <button
              class="btn btn-light border rounded-0 flex-fill"
              @click="currentLevel--"
            >
              -1
            </button>
          </h4>
          <div class="d-flex">
            <button
              v-for="element in array[currentLevel - 1].childrens.slice(
                0,
                array[currentLevel - 1].childrens.length / 2
              )"
              :key="element"
              class="btn btn-light border rounded-0 flex-fill"
              @click="currentLevel++"
            >
              {{ element.title }}
            </button>
          </div>

          <div class="d-flex">
            <button
              v-for="element in array[currentLevel - 1].childrens.slice(
                array[currentLevel - 1].childrens.length / 2
              )"
              :key="element"
              class="btn btn-light border rounded-0 flex-fill"
              @click="currentLevel++"
            >
              {{ element.title }}
            </button>
          </div>
        </div> -->
      </div>
    </div>
    <div class="row">
      <div class="col-12">
        <pre>{{ arrayForShow }}</pre>
        <pre>temp: {{ tempArray }}</pre>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentItemId: '',
      currentLevel: 1,
      tempArray: [],
    };
  },
  computed: {
    // arrayForShow() {
    //   if (this.currentItemId) {
    //     let tempArray = JSON.parse(JSON.stringify(this.arrayForShow));
    //     return this.getChildArray(tempArray, this.currentItemId);
    //   }
    //   return this.array;
    // },
    arrayForShow() {
      if (this.currentItemId) {
        const el = this.tempArray.find(
          (item) => item.id === this.currentItemId
        );
        if (el) {
          console.log('el find = ', el);
          this.tempArray = el.childrens;
          return this.tempArray;
        }
        console.log('el NOT find = ', el);
        return [];
      }
      return this.tempArray;
    },
  },
  methods: {
    // getArrayForShow() {
    //   if (this.currentItemId) {
    //     let tempArray = JSON.parse(JSON.stringify(this.arrayForShow));
    //     this.arrayForShow = this.getChildArray(tempArray, this.currentItemId);
    //     return this.arrayForShow;
    //   }
    //   return this.arrayForShow;
    // },

    addChild() {
      if (this.currentItemId) {
        this.tempArray = [];
        this.tempArray.push(this.createChild());
      }
      this.tempArray.push(this.createChild());
    },

    createChild() {
      return {
        id: String(Date.now()),
        title: `${this.currentLevel} level`,
        childrens: [],
      };
    },

    getChildArray(array, parentId) {
      console.log('array = ', array);
      console.log('parentId = ', parentId);
      return array.find((item) => item.id === parentId).childrens;
    },
  },
};
</script>

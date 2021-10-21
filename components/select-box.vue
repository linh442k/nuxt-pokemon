<template>
  <validation-provider
    v-slot="{ errors }"
    name="select"
    :rules="rules"
  >
    <!--    @click="$emit('fetch-data')"-->
    <v-select
      :value="value"
      @change="$emit('input', choice)"
      v-model="choice"
      :items="list"
      :error-messages="errors"
      :label="label"
      data-vv-name="select"
      outlined
    >
      <template #append-item>
        <v-list-item
          disabled>
          <v-list-item-content
          >
            <v-list-item-title
              v-text="'Loading...'"
              v-intersect="onBottomDetect"
            >
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </template>
    </v-select>
  </validation-provider>
</template>

<script>
import axios from "@nuxtjs/axios";

export default {
  props: {
    value: {},
    label: {
      type: String
    },
    rules: {
      type: [String, Object]
    },
    limit: {
      type: Number,
      default: 10,
    },
    offset: {
      type: Number,
      default: 0,
    },
    defaultApi: {
      type: String,
    },
    itemText: {
      type: String,
    },
    itemValue: {
      type: String,
    }
  },
  data: () => {
    return {
      choice: "",
      list: [],
      start: 0,
    };
  },
  computed: {
    fetchApi: function () {
      return this.defaultApi + "offset=" + this.start.toString() + "&limit=" + this.limit.toString();
    }
  },
  methods: {
    onBottomDetect(entries) {
      if (Array.isArray(this.list) && this.list.length !== 0)
        if (entries[0].isIntersecting) {
          // if(entries[0].intersectionRatio >= 0.5)
          console.log("reach bottom");
          this.start += this.limit;
          this.$axios.$get(this.fetchApi).then(response => {
              this.list = this.list.concat(response.results.map(item => {
                return {
                  text: item[this.itemText],
                  value: item[this.itemValue]
                };
              }));
            }
          );
        }
    }
  },
  mounted() {
    this.start = this.offset
    if (this.list.length === 0) {
      this.$axios.$get(this.fetchApi).then(response => {
          this.list = response.results.map(item => {
            return {
              text: item.name,
              value: item.url
            };
          });
        }
      );
    }
  }
};
</script>

<template>
  <div class="main">
    <div class="color-picker">
      <v-row>
        <v-color-picker
          class="ma-2"
          v-model="color"
          :swatches="swatches"
          show-swatches
        ></v-color-picker>
      </v-row>
      <v-row>
        <v-combobox v-model="colors" multiple small-chips solo>
          <template v-slot:selection="{ attrs, item, parent, selected }">
            <v-chip
              v-if="item === Object(item)"
              v-bind="attrs"
              :color="`${item.color}`"
              :input-value="selected"
              label
              small
            >
              <span class="pr-2">
                {{ item.text }}
              </span>
              <v-icon small @click="parent.selectItem(item)"> $delete </v-icon>
            </v-chip>
          </template>
        </v-combobox>
      </v-row>
      <v-row>
        <v-combobox
          v-model="model"
          :filter="filter"
          :hide-no-data="!search"
          :items="items"
          :search-input.sync="search"
          hide-selected
          label="Search for an option"
          multiple
          small-chips
          solo
        >
          <template v-slot:no-data>
            <v-list-item>
              <span class="subheading">Create</span>
              <v-chip :color="`${colors[nonce - 1]} lighten-3`" label small>
                {{ search }}
              </v-chip>
            </v-list-item>
          </template>
          <template v-slot:selection="{ attrs, item, parent, selected }">
            <v-chip
              v-if="item === Object(item)"
              v-bind="attrs"
              :color="`${item.color} lighten-3`"
              :input-value="selected"
              label
              small
            >
              <span class="pr-2">
                {{ item.text }}
              </span>
              <v-icon small @click="parent.selectItem(item)"> $delete </v-icon>
            </v-chip>
          </template>
          <template v-slot:item="{ index, item }">
            <v-text-field
              v-if="editing === item"
              v-model="editing.text"
              autofocus
              flat
              background-color="transparent"
              hide-details
              solo
              @keyup.enter="edit(index, item)"
            ></v-text-field>
            <v-chip v-else :color="`${item.color} lighten-3`" dark label small>
              {{ item.text }}
            </v-chip>
            <v-spacer></v-spacer>
            <v-list-item-action @click.stop>
              <v-btn icon @click.stop.prevent="edit(index, item)">
                <v-icon>{{
                  editing !== item ? "mdi-pencil" : "mdi-check"
                }}</v-icon>
              </v-btn>
            </v-list-item-action>
          </template>
        </v-combobox>
      </v-row>
      <v-row>
        <v-data-table
          :headers="headers"
          :items="attributes"
          :items-per-page="5"
          class="elevation-1"
        >
          <template v-slot:item.name="{ item }">
            <v-chip :color="item.name" dark>
              {{ item.name }}
            </v-chip>
          </template></v-data-table
        >
      </v-row>
    </div>
  </div>
</template>

<script>
export default {
  name: "ColorPicker",
  data() {
    return {
      color: "",
      colors: [],
      size: "",
      sizes: [],
      activator: null,
      attach: null,
      editing: null,
      editingIndex: -1,
      items: [
        { header: "Select an option or create one" },
        {
          text: "L",
          color: "gray",
        },
        {
          text: "XL",
          color: "gray",
        },
      ],
      nonce: 1,
      menu: false,
      model: [
        {
          text: "XL",
          color: "gray",
        },
      ],
      x: 0,
      search: null,
      y: 0,
      attributes: [],
      headers: [
        {
          text: "color",
          value: "name",
        },
      ],
    };
  },

  watch: {
    color: function () {
      console.log(this.color);
      this.colors.push({
        text: this.color,
        color: this.color,
      });
      let obj = {};
      this.items.forEach((item) => {
        obj[item.text] = 0;
      });

      this.attributes.push({ ...obj, name: this.color });
      console.log(this.attributes);
    },
    model(val, prev) {
      if (val.length === prev.length) return;

      this.model = val.map((v) => {
        if (typeof v === "string") {
          v = {
            text: v,
            color: this.colors[this.nonce - 1],
          };
          this.items.push(v);
          this.headers.push({
            text: v,
            color: this.colors[this.nonce - 1],
          });
          this.nonce++;
        }
        return v;
      });
    },
  },

  methods: {
    edit(index, item) {
      if (!this.editing) {
        this.editing = item;
        this.editingIndex = index;
      } else {
        this.editing = null;
        this.editingIndex = -1;
      }
    },
    filter(item, queryText, itemText) {
      if (item.header) return false;

      const hasValue = (val) => (val != null ? val : "");

      const text = hasValue(itemText);
      const query = hasValue(queryText);

      return (
        text.toString().toLowerCase().indexOf(query.toString().toLowerCase()) >
        -1
      );
    },
  },
};
</script>

<style scoped>
.main {
  height: 100vh;
  display: flex;
  flex: 1;
  justify-content: center;
  align-content: center;
  align-items: center;
}
</style>

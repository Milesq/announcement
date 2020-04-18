<template>
  <v-card>
    <v-card-text>
      <v-form v-model="formValid" @submit.prevent="newUser" class="d-flex align-center mb-1">
        <v-text-field :rules="nameRules" v-model="newName" :label="$t('nameAndSurname')" />
        <v-btn type="submit" class="ma-2" tile outlined color="primary">
          <v-icon left>mdi-check</v-icon>Save
        </v-btn>
      </v-form>

      <v-divider />
      <v-text-field
        :label="$t('search')"
        append-icon="mdi-search"
        hide-details
        v-model="searchText"
      />

      <v-data-table
        class="elevation-1 mt-7"
        :items-per-page="5"
        :headers="headers"
        :items="users"
        :search="searchText"
      >
        <template #item.name="props">
          <v-edit-dialog
            transition="slide-y-transition"
            :return-value.sync="props.item.name"
            @save="update"
          >
            {{ props.item.name }}

            <template #input>
              <v-text-field v-model="props.item.name" single-line />
            </template>
          </v-edit-dialog>
        </template>
      </v-data-table>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      searchText: '',
      newName: '',
      users: ['Miłosz Wiśniewski'].map(name => ({ name })),
      formValid: false,

      nameRules: [
        name => /^[a-zA-Z\s]+$/.test(name) || this.$t('nameErrors[0]'),
        name => /.+\s.+/.test(name) || this.$t('nameErrors[1]'),
      ],
      headers: [
        {
          text: this.$t('name'),
          align: 'start',
          value: 'name',
        },
      ],
    };
  },
  methods: {
    newUser() {
      if (!this.formValid) return;

      this.newName = '';
      this.update();
    },
    update() {
      console.log(this.users);
    },
  },
};
</script>
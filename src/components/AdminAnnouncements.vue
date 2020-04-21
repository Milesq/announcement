<template>
  <v-card>
    <v-card-title>{{ $t('manageAnnouncements') }}</v-card-title>
    <v-card-text>
      <v-list subheader three-line v-if="announcements.length">
        <EditableAnnouncement
          v-for="(el, idx) in announcements"
          :value="announcements[idx]"
          @save="save(idx, $event)"
          :key="el.title + idx"
        >
          <v-list-item class="announcement" v-on:click.prevent>
            <v-list-item-content>
              <v-list-item-title v-html="el.title" />
              <v-list-item-subtitle v-html="el.content" />
            </v-list-item-content>
          </v-list-item>
        </EditableAnnouncement>
      </v-list>

      <v-list v-else>
        <v-skeleton-loader
          loading
          type="list-item"
          v-for="(el, i) in Array(4).fill({ name: '' })"
          :key="i"
        />
      </v-list>
    </v-card-text>

    <v-snackbar left v-model="snackbarSaved">
      {{ $t('save') }}
      <v-btn text @click="snackbarSaved = false">{{ $t('close') }}</v-btn>
    </v-snackbar>
  </v-card>
</template>

<script>
import EditableAnnouncement from './EditableAnnouncement.vue';

export default {
  data: () => ({
    announcements: [],
    snackbarSaved: false,
  }),
  methods: {
    async readData() {
      return (await this.$announcements.get()).docs.map(
        el => el.exists && { ...el.data(), id: el.id }
      );
    },
    save(idx, { title, content }) {
      const { id } = this.announcements[idx];
      this.announcements[idx] = {
        id,
        title,
        content,
      };
      this.snackbarSaved = true;

      this.$announcements.doc(id).update({
        id,
        title,
        content,
      });
    },
  },
  created() {
    this.readData().then(announcements => {
      this.announcements = announcements;
    });
  },
  components: {
    EditableAnnouncement,
  },
};
</script>
<template>
  <div class="c-list">
    <h2 class="subtitle c-subtitle">{{title}}</h2>
    <list hasMargin>
      <list-item v-for="id in list.order"
        tag="router-link"
        variant="solid"
        :icon="getIcon(id)"
        :badgeCount="list.conversations[id].unreadCount"
        :to="buildUrl(id)"
      >
        <div class="c-userAvatarNamed">
          <avatar hasMargin
            size="sm"
            v-if="list.conversations[id].picture"
            :src="list.conversations[id].picture"
          />
          <span class="gi-is-ellipsis">{{list.conversations[id].displayName || list.conversations[id].name}}</span>
        </div>
      </list-item>
    </list>
  </div>
</template>
<style lang="scss" scoped>
@import "../../../assets/sass/theme/index";

.c-list:not(:first-child) {
  margin: $gi-spacer*1.5 0;
}

.c-subtitle {
  padding-left: $gi-spacer-sm;
}

.c-userAvatarNamed {
  display: flex;
  align-items: center;
}
</style>
<script>
import { List, ListItem } from '../../components/Lists/index.js'
import Avatar from '../../components/Avatar.vue'

export default {
  name: 'ConversationsList',
  components: {
    List,
    ListItem,
    Avatar
  },
  props: {
    title: String,
    /** List of conversations - shape: {
      order: [] - see fakeStore.js - individualMessagesSorted,
      conversations: - see fakeStore.js - users or groupChannels
    }
    */
    list: Object,
    routeName: String,
    type: String
  },
  data () {
    return {}
  },
  computed: {},
  methods: {
    getIcon (id) {
      const isPrivate = this.list.conversations[id].private
      return isPrivate === undefined ? '' : (isPrivate ? 'lock' : 'globe')
    },
    buildUrl (id) {
      const { list, routeName, type } = this
      const { name } = list.conversations[id] || {}

      // NOTE - This should be $store responsability
      // ...but for now I've used the $route params just for mocked layout purposes
      return {
        name: routeName,
        params: {
          chatName: name,
          currentConversation: { type, id }
        }
      }

      // ... once $store is implement, we can just pass the following:
      // return { path: `${routePath}${name}` }
    }
  }
}
</script>

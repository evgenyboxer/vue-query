<script lang="ts">
import { defineComponent, PropType, h } from "vue-demi";

import type { Query } from "react-query/types";

import Explorer from "./Explorer.vue";
import InfoPanel from "./InfoPanel.vue";
import QueryActions from "./QueryActions.vue";
import QueryDetails from "./QueryDetails.vue";

export default defineComponent({
  name: "ActiveQueryPanel",
  props: {
    query: {
      type: Object as PropType<Query>,
      required: true,
    },
  },
  render() {
    return h(
      "div",
      {
        class: "active-query-panel",
      },
      [
        h(QueryDetails, {
          // Vue3
          query: this.$props.query,
          // Vue2
          props: {
            query: this.$props.query,
          },
        }),
        h(QueryActions, {
          // Vue3
          query: this.$props.query,
          // Vue2
          props: {
            query: this.$props.query,
          },
        }),
        h(
          InfoPanel,
          {
            // Vue3
            title: "Data Explorer",
            // Vue2
            props: {
              title: "Data Explorer",
            },
          },
          [
            h(Explorer, {
              // Vue3
              label: "Data",
              value: this.$props.query.state.data,
              defaultExpanded: true,
              // Vue2
              props: {
                label: "Data",
                value: this.$props.query.state.data,
                defaultExpanded: true,
              },
            }),
          ]
        ),
        h(
          InfoPanel,
          {
            // Vue3
            title: "Query Explorer",
            // Vue2
            props: {
              title: "Query Explorer",
            },
          },
          [
            h(Explorer, {
              // Vue3
              label: "Query",
              value: this.$props.query,
              defaultExpanded: { queryKey: true },
              // Vue2
              props: {
                label: "Query",
                value: this.$props.query,
                defaultExpanded: { queryKey: true },
              },
            }),
          ]
        ),
      ]
    );
  },
});
</script>

<style>
.active-query-panel {
  display: flex;
  flex-direction: column;
  flex: 1 1 500px;
  height: 100%;
  overflow: auto;
}
</style>

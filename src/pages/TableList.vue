<template>
  <div class="content">
    <div class="md-layout">
      <div
        class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-100"
      >
        <md-card class="md-card-plain">
          <md-card-header data-background-color="green">
            <h4 class="title">لیست شعرها</h4>
            <a href="http://localhost:5969/manage/poem/add">
              <i class="material-icons material-add">add</i>
            </a>
          </md-card-header>
          <md-card-content>
            <poemlist></poemlist>
          </md-card-content>
        </md-card>
      </div>
    </div>
  </div>
</template>

<script>
import { poemlist } from "@/components";
export default {
  components: {
    poemlist
  }
};
</script>
<style scoped>
.md-card .md-card-header .title {
  float: right;
}
</style>

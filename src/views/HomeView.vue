<template>
  <div class="home">
    <Tabs v-model="tabName">
      <TabPane label="首页" name="name1"><div style="background: #fff;">2222首页</div>
      </TabPane>
      <TabPane label="图书详情页" name="name2" ><div style="background: #fff;">55555555555</div></TabPane>    </Tabs>
  </div>
</template>

<script>
// @ is an alias to /src
import { Tabs, TabPane } from "@/views/tabs";

export default {
  name: 'HomeView',
  data(){
    return {
      tabName:'name1'
    }
  },
  components: { Tabs, TabPane },

}
</script>

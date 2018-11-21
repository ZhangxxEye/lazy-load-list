<template>
  <div>
    <div class="greeter"> {{ greeter }}</div>
    <!-- <div style="height: 100px; width: 100%; overflow: scroll"> -->

    <list @load="onLoad" v-model="loading" :finished="finished">
      <ul>
        <li v-for="item in list">{{item}}</li>
      </ul>
    </list>
    <!-- </div> -->

  </div>

</template>

<script>
import list from "@/components/list/index.vue";
export default {
  name: "home",
  data() {
    return {
      greeter: "Hello, Vue!",
      list: [],
      loading: false,
      finished: false
    };
  },
  created() {
    console.log("发送请求4444444444444");
    this.$http
      .getApiXxx()
      .then(response => {
        console.log(response);
      })
      .catch(response => {
        console.log(response);
      });
  },
  components: {
    list
  },
  methods: {
    onLoad() {
      // 异步更新数据
      setTimeout(() => {
        for (let i = 0; i < 10; i++) {
          this.list.push(this.list.length + 1);
        }
        // 加载状态结束
        this.loading = false;

        // 数据全部加载完成
        if (this.list.length >= 40) {
          this.finished = true;
        }
      }, 500);
    }
  }

};
</script>

<style lang="less" src="./index.less"></style>

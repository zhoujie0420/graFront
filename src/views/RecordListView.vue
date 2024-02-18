<template>
  <section>
    <van-nav-bar title="问诊结果编辑"
                 fixed
                 placeholder
                 safe-area-inset-top
    />
  </section>


  <section>
    <van-search placeholder="请输入搜索关键词"/>
  </section>


  <section v-if="records.length">
    <section v-for="info of records" :key="info">
      <record-card @click="showDetail(info.id)">
        <div class="record-info">
          <p class="date">{{ info.consultationDate }}</p>
          <p class="status">
            {{ info.status === 1 ? '已完成' : '未完成' }}
          </p>
        </div>
      </record-card>
      <section v-if="show[info.id] === 1">
        <van-cell-group inset>
            <van-cell v-if="peerStore.role === 1" title="医生姓名" :value="info.otherName"/>
          <van-cell  v-if="peerStore.role === 2" title="患者姓名" :value="info.otherName"/>
          <van-cell title="诊断结果" :label="info.diagnosis"/>
          <van-cell title="处方" :label="info.prescription"/>
          <van-cell title="问诊回放" :value="info.videoUrl"/>
        </van-cell-group>
      </section>
    </section>
  </section>

</template>

<script setup>
import $ from "jquery";
import {apiUrl} from "../../config";
import {showToast} from "vant";
import usePeerStore from "@/store/peer";
import {ref} from "vue";

let peerStore = usePeerStore();
const records = ref([]); // 创建响应式的userInfos
getRecords()
const show = ref({});

function showDetail(id) {
  show.value[id] = show.value[id] === 1 ? 0 : 1;

}

function getRecords() {
  let peerStore = usePeerStore();
  $.ajax({
    url: `${apiUrl}/api/record/getRecords/`,
    type: "post",
    headers: {
      Authorization: "Bearer " + peerStore.token,
    },
    success(resp) {
      if (resp.code === 200) {
        records.value = resp.data; // 更新userInfos的值
      } else {
        showToast(resp.message);
      }
    },
  });
}
</script>

<style scoped lang="scss">
.wide-button {
  width: 200px; /* 根据需要调整按钮宽度 */
  position: fixed;
  left: 50%;
  bottom: 80px; /* 调整底部距离根据需要 */
  transform: translateX(-50%);
}

.center-bottom {
  display: flex;
  justify-content: center;
}

.record-info {
  display: flex;
  justify-content: space-between;
}

.record-info .date {
  text-align: left;
  padding-right: 10px; /* 可根据实际情况调整间距 */
}

.record-info .status {
  text-align: right;
}
</style>
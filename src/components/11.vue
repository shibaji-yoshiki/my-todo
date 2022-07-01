<script>
import { reactive, ref } from 'vue';

// ref関数
const count = ref(0);
count.value++;

// reactive関数
const state = reactive({
  count: 0,
});
state.count++;
</script>

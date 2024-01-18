<template>
  <div class="movementItem">
    <div class="content">
      <h4>{{ title }}</h4>
      <p>{{ description }}</p>
    </div>
    <div class="action">
      <img src="@/assets/trash-icon.svg" alt="borrar" @click="remove" />
      <p :class="[{ red: isNegative, green: !isNegative }]">
        {{ amountCurrency }}
      </p>
    </div>
  </div>
</template>
<script setup>
import { defineProps, toRefs, computed, defineEmits } from "vue";

const formatCurrency = new Intl.NumberFormat("es-MX", {
  style: "currency",
  currency: "MXN",
});

const props = defineProps({
  title: {
    type: String,
  },
  id: {
    type: Number,
  },
  description: {
    type: String,
  },
  amount: {
    type: Number,
  },
});

const { id, title, description, amount } = toRefs(props);

const amountCurrency = computed(() => formatCurrency.format(amount.value));

const isNegative = computed(() => amount.value < 0);

const emit = defineEmits(["remove"]);

const remove = () => {
  emit("remove", id.value);
};
</script>

<style scoped>
.movementItem {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding: 16px;
  background-color: #e6f9ff;
  border-radius: 8px;
  box-sizing: border-box;
}

.movementItem .content {
  width: 100%;
}

.movementItem .action {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  flex-direction: column;
}

h4,
p {
  margin: 0;
  padding: 0;
}

h4 {
  margin-bottom: 8px;
}
.movementItem .action img {
  margin-bottom: 16px;
}

.red {
  color: red;
}

.green {
  color: green;
}
</style>

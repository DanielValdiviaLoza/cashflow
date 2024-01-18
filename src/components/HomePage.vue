<template>
  <LayoutContent>
    <template #header>
      <HeaderContent></HeaderContent>
    </template>
    <template #resume>
      <ResumeContainer
        :label="title"
        :amount="amount"
        :total-amount="totalAmount"
      >
        <template #graphic>
          <GraphicContainer
            :amounts="amounts"
            @selection="select"
          ></GraphicContainer
        ></template>
        <template #action>
          <ActionContainer @create="create"></ActionContainer>
        </template>
      </ResumeContainer>
    </template>
    <template #movements>
      <MovementsContainer
        :movements="movements"
        @remove="remove"
      ></MovementsContainer>
    </template>
  </LayoutContent>
</template>
<script>
import LayoutContent from "./LayoutContent.vue";
import HeaderContent from "./HeaderContent.vue";
import ResumeContainer from "./Resume/ResumeContainer.vue";
import MovementsContainer from "./Movements/MovementsContainer.vue";
import ActionContainer from "./Resume/ActionContainer.vue";
import GraphicContainer from "./Resume/GraphicContainer.vue";

export default {
  components: {
    LayoutContent,
    HeaderContent,
    ResumeContainer,
    MovementsContainer,
    ActionContainer,
    GraphicContainer,
  },
  data() {
    return {
      amount: null,
      title: null,
      movements: [],
    };
  },
  computed: {
    amounts() {
      const lastDays = this.movements
        .filter((m) => {
          const today = new Date();
          const oldDate = today.setDate(today.getDate() - 30);
          return m.time > oldDate;
        })
        .map((m) => m.amount);
      return lastDays.map((m, i) => {
        const lastMovements = lastDays.slice(0, i);
        return lastMovements.reduce((suma, move) => {
          return suma + move;
        }, 0);
      });
    },
    totalAmount() {
      return this.movements.reduce((suma, m) => {
        return suma + m.amount;
      }, 0);
    },
  },
  mounted() {
    const movements = JSON.parse(localStorage.getItem("movements")) || [];
    if (movements) {
      if (Array.isArray(movements)) {
        this.movements = movements.map((m) => {
          return { ...m, time: new Date(m.time) };
        });
      }
    }
  },
  methods: {
    select(e) {
      console.log(e);
      this.amount = e;
    },
    create(move) {
      this.movements.push(move);
      this.save();
    },
    remove(id) {
      const index = this.movements.findIndex((m) => m.id === id);
      this.movements.splice(index, 1);
      this.save();
    },
    save() {
      localStorage.setItem("movements", JSON.stringify(this.movements));
    },
  },
};
</script>

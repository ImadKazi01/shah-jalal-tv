<script setup>
import { onMounted } from "vue";
import TimeTable from "./components/TimeTable.vue";
import Date from "./components/Date.vue";
import News from "./components/News.vue";

const checkForReload = async () => {
  try {
    const response = await fetch("/reload-trigger.txt");
    const timestamp = await response.text();
    const storedTimestamp = localStorage.getItem("reloadTimestamp");

    if (timestamp !== storedTimestamp) {
      localStorage.setItem("reloadTimestamp", timestamp);
      location.reload();
    }
  } catch (error) {
    console.error("Failed to check for reload:", error);
  }
};

onMounted(() => {
  // Check if the current URL matches the reload trigger URL
  if (window.location.pathname === "/") {
    const timestamp = new window.Date();
    fetch("/reload-trigger.txt", {
      method: "PUT",
      body: `timestamp: ${timestamp}`,
    });
  }

  // Check for changes in the reload-trigger.txt file
  checkForReload();
});
</script>

<template>
  <div class="tv-display">
    <Date class="date-component" />
    <section class="tv-display__body">
      <TimeTable class="timetable-component" />
      <News class="news-component" />
    </section>
  </div>
</template>

<style lang="scss">
.tv-display {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: flex-start;
  height: 100vh;

  &__body {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    width: 100%;
    height: 100%;
  }
}
</style>

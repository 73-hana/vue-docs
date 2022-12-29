<template>
  <div>
    <form>
      <p>form without preventDefault()</p>
      <input type="submit" />
    </form>
    <form @submit.prevent="onSubmit">
      <p>form with preventDefault()</p>
      <input type="submit" />
    </form>
  </div>
</template>

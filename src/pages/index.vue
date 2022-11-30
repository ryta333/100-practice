<script setup lang="ts">
defineOptions({
  name: 'IndexPage',
})

const router = useRouter()
const go = (name: string) => {
  if (name)
    router.push(`/days/${name}`)
}
const pratices = ['plum', 'line']

function bg(){
  toggleDark()
};
</script>

<template>
  <div>
    100 days pratice
  </div>
  <div>
    <ul>
      <li v-for="item in pratices" @click="go(item)">
        {{ item }}
      </li>
    </ul>
  </div>
  <button @click="bg">toggleDark</button>
</template>

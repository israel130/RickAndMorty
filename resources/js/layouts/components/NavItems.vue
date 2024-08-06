<script setup>
import VerticalNavSectionTitle from '@/@layouts/components/VerticalNavSectionTitle.vue'
import VerticalNavGroup from '@layouts/components/VerticalNavGroup.vue'
import VerticalNavLink from '@layouts/components/VerticalNavLink.vue'
</script>

<template>
  <VerticalNavSectionTitle
    :item="{
      heading: 'Rick and Morty',
    }"
  />
  <VerticalNavGroup
    :item="{
      title: 'Informacion',
      badgeContent: 'Admin',
      badgeClass: 'bg-error',
      icon: 'bx-home-smile',
    }"
  >
    <VerticalNavLink
      :item="{
        title: 'Api',
        to: '/dashboard',
      }"
    />
  </VerticalNavGroup>
</template>

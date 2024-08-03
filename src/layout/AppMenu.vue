<script setup>
import { ref } from 'vue';

import AppMenuItem from './AppMenuItem.vue';

const model = ref([
    {
        label: '首页',
        items: [{ label: '面板', icon: 'pi pi-fw pi-home', to: '/' }]
    },
    {
        label: '用户管理',
        icon: 'pi pi-fw pi-briefcase',
        to: '/pages',
        items: [
            {
                label: '用户列表',
                icon: 'pi pi-fw pi-pencil',
                to: '/pages/crud'
            }
        ]
    }
]);
</script>

<template>
    <ul class="layout-menu">
        <template v-for="(item, i) in model" :key="item">
            <app-menu-item v-if="!item.separator" :item="item" :index="i"></app-menu-item>
            <li v-if="item.separator" class="menu-separator"></li>
        </template>
    </ul>
</template>

<style lang="scss" scoped></style>

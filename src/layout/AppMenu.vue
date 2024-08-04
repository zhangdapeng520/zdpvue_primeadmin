<script setup>
import { ref } from 'vue';

import AppMenuItem from './AppMenuItem.vue';

const model = ref([
    {
        label: '首页',
        items: [{ label: '面板', icon: 'pi pi-fw pi-home', to: '/' }]
    },
    {
        label: '常用组件',
        items: [
            { label: '表单布局', icon: 'pi pi-fw pi-id-card', to: '/uikit/formlayout' },
            { label: '输入框', icon: 'pi pi-fw pi-check-square', to: '/uikit/input' },
            { label: '按钮', icon: 'pi pi-fw pi-mobile', to: '/uikit/button', class: 'rotated-icon' },
            { label: '表格', icon: 'pi pi-fw pi-table', to: '/uikit/table' },
            { label: '列表', icon: 'pi pi-fw pi-list', to: '/uikit/list' },
            { label: '树形结构', icon: 'pi pi-fw pi-share-alt', to: '/uikit/tree' },
            { label: '面板', icon: 'pi pi-fw pi-tablet', to: '/uikit/panel' },
            { label: '浮层', icon: 'pi pi-fw pi-clone', to: '/uikit/overlay' },
            { label: '媒体', icon: 'pi pi-fw pi-image', to: '/uikit/media' },
            { label: '菜单', icon: 'pi pi-fw pi-bars', to: '/uikit/menu' },
            { label: '消息提示', icon: 'pi pi-fw pi-comment', to: '/uikit/message' },
            { label: '文件', icon: 'pi pi-fw pi-file', to: '/uikit/file' },
            { label: '图表', icon: 'pi pi-fw pi-chart-bar', to: '/uikit/charts' },
            { label: '时间线', icon: 'pi pi-fw pi-calendar', to: '/uikit/timeline' },
            { label: '其他', icon: 'pi pi-fw pi-circle', to: '/uikit/misc' }
        ]
    },
    {
        label: '示例页面',
        icon: 'pi pi-fw pi-briefcase',
        to: '/pages',
        items: [
            { label: '官网', icon: 'pi pi-fw pi-globe', to: '/landing' },
            {
                label: '权限管理', icon: 'pi pi-fw pi-user', items: [
                    { label: '登录', icon: 'pi pi-fw pi-sign-in', to: '/auth/login' },
                    { label: '错误', icon: 'pi pi-fw pi-times-circle', to: '/auth/error' },
                    { label: '没有权限', icon: 'pi pi-fw pi-lock', to: '/auth/access' }
                ]
            },
            { label: '增删改查', icon: 'pi pi-fw pi-pencil', to: '/pages/crud' },
            { label: '404错误', icon: 'pi pi-fw pi-exclamation-circle', to: '/pages/notfound' },
            { label: '空页面', icon: 'pi pi-fw pi-circle-off', to: '/pages/empty' }
        ]
    },
    {
        label: '菜单嵌套',
        items: [
            {
                label: 'Submenu 1',
                icon: 'pi pi-fw pi-bookmark',
                items: [
                    {
                        label: 'Submenu 1.1',
                        icon: 'pi pi-fw pi-bookmark',
                        items: [
                            { label: 'Submenu 1.1.1', icon: 'pi pi-fw pi-bookmark' },
                            { label: 'Submenu 1.1.2', icon: 'pi pi-fw pi-bookmark' },
                            { label: 'Submenu 1.1.3', icon: 'pi pi-fw pi-bookmark' }
                        ]
                    },
                    {
                        label: 'Submenu 1.2',
                        icon: 'pi pi-fw pi-bookmark',
                        items: [{ label: 'Submenu 1.2.1', icon: 'pi pi-fw pi-bookmark' }]
                    }
                ]
            },
            {
                label: 'Submenu 2',
                icon: 'pi pi-fw pi-bookmark',
                items: [
                    {
                        label: 'Submenu 2.1',
                        icon: 'pi pi-fw pi-bookmark',
                        items: [
                            { label: 'Submenu 2.1.1', icon: 'pi pi-fw pi-bookmark' },
                            { label: 'Submenu 2.1.2', icon: 'pi pi-fw pi-bookmark' }
                        ]
                    },
                    {
                        label: 'Submenu 2.2',
                        icon: 'pi pi-fw pi-bookmark',
                        items: [{ label: 'Submenu 2.2.1', icon: 'pi pi-fw pi-bookmark' }]
                    }
                ]
            }
        ]
    },
    {
        label: '参考文档',
        items: [
            {
                label: '普通文档',
                icon: 'pi pi-fw pi-book',
                to: '/documentation'
            },
            {
                label: '链接文档',
                icon: 'pi pi-fw pi-github',
                url: 'https://github.com/zhangdapeng520/zdpvue_primeadmin',
                target: '_blank'
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

<script setup>
import { FilterMatchMode } from '@primevue/core/api';
import { useToast } from 'primevue/usetoast';
import { ref } from 'vue';


const toast = useToast(); // 消息提示
const dt = ref(); // 表格数据
const userTableData = ref([
    {
        id: 1,
        name: '张三',
        age: 23
    },
    {
        id: 2,
        name: '李四',
        age: 23
    },
    {
        id: 3,
        name: '王五',
        age: 23
    }
]); // 用户表格数据
const isShowSaveDialog = ref(false); // 是否显示保存的弹窗
const isShowDeleteDialog = ref(false); // 是否显示修改的弹窗
const isShowDeleteManyDialog = ref(false); // 是否显示批量修改的弹窗
const user = ref({});
const selectedItems = ref(); // 被选中的数据
const filters = ref({ // 数据搜索
    global: { value: null, matchMode: FilterMatchMode.CONTAINS }
});
const isSubmitted = ref(false); // 是否点击了保存按钮

// 点击新增按钮
const onAddClick = () => {
    user.value = {};
    isSubmitted.value = false;
    isShowSaveDialog.value = true;
};

// 隐藏新增的弹窗
const hideSaveDialog = () => {
    isShowSaveDialog.value = false;
    isSubmitted.value = false;
};

// 点击保存按钮
const onSaveClick = () => {
    isSubmitted.value = true;

    if (user?.value.name?.trim()) {
        if (user.value.id) {
            // 修改用户的逻辑
            userTableData.value[findIndexById(user.value.id)] = user.value;
            toast.add({ severity: 'success', summary: '成功', detail: '修改用户成功', life: 3000 });
        } else {
            // 新增用户的逻辑
            user.value.id = createId();
            userTableData.value.push(user.value);
            toast.add({ severity: 'success', summary: '成功', detail: '新增用户成功', life: 3000 });
        }

        isShowSaveDialog.value = false;
        user.value = {};
    }
};

// 点击修改的按钮
const onUpdateClick = (prod) => {
    user.value = { ...prod };
    isShowSaveDialog.value = true;
};

// 点击修改按钮
const onDeleteClick = (prod) => {
    user.value = prod;
    isShowDeleteDialog.value = true;
};

// 根据ID删除
const deleteById = () => {
    userTableData.value = userTableData.value.filter((val) => val.id !== user.value.id);
    isShowDeleteDialog.value = false;
    user.value = {};
    toast.add({ severity: 'success', summary: '成功', detail: '删除用户成功', life: 3000 });
};

// 根据ID查找
const findIndexById = (id) => {
    let index = -1;
    for (let i = 0; i < userTableData.value.length; i++) {
        if (userTableData.value[i].id === id) {
            index = i;
            break;
        }
    }
    return index;
};

// 创建ID
const createId = () => {
    let id = '';
    var chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
    for (var i = 0; i < 5; i++) {
        id += chars.charAt(Math.floor(Math.random() * chars.length));
    }
    return id;
};

// 导出为csv格式
const exportCSV = () => {
    dt.value.exportCSV();
};

// 点击删除选中的用户
const onDeleteManyClick = () => {
    isShowDeleteManyDialog.value = true;
};

// 删除所有被选中的用户
const deleteMany = () => {
    userTableData.value = userTableData.value.filter((val) => !selectedItems.value.includes(val));
    isShowDeleteManyDialog.value = false;
    selectedItems.value = null;
    toast.add({ severity: 'success', summary: '成功', detail: '批量删除成功', life: 3000 });
};
</script>

<template>
    <div>
        <div class="card">
            <Toolbar class="mb-6">
                <template #start>
                    <Button label="新增" icon="pi pi-plus" severity="secondary" class="mr-2" @click="onAddClick" />
                    <Button label="删除" icon="pi pi-trash" severity="secondary" @click="onDeleteManyClick"
                            :disabled="!selectedItems || !selectedItems.length" />
                </template>

                <template #end>
                    <Button label="导出" icon="pi pi-upload" severity="secondary" @click="exportCSV($event)" />
                </template>
            </Toolbar>

            <DataTable
                ref="dt"
                v-model:selection="selectedItems"
                :value="userTableData"
                dataKey="id"
                :paginator="true"
                :rows="5"
                :filters="filters"
                paginatorTemplate="FirstPageLink PrevPageLink PageLinks NextPageLink LastPageLink CurrentPageReport RowsPerPageDropdown"
                :rowsPerPageOptions="[5, 10, 20]"
                currentPageReportTemplate="第 {first} 到 {last} 条数据，共 {totalRecords} 条"
            >
                <template #header>
                    <div class="flex flex-wrap gap-2 items-center justify-between">
                        <h4 class="m-0">用户管理</h4>
                        <IconField>
                            <InputIcon>
                                <i class="pi pi-search" />
                            </InputIcon>
                            <InputText v-model="filters['global'].value" placeholder="搜索..." />
                        </IconField>
                    </div>
                </template>

                <Column selectionMode="multiple" style="width: 3rem" :exportable="false"></Column>
                <Column field="name" header="姓名" style="min-width: 16rem"></Column>
                <Column field="age" header="年龄" sortable style="min-width: 16rem"></Column>

                <!--操作列-->
                <Column :exportable="false" style="min-width: 12rem" header="操作">
                    <template #body="slotProps">
                        <Button icon="pi pi-pencil" outlined rounded class="mr-2"
                                @click="onUpdateClick(slotProps.data)" />
                        <Button icon="pi pi-trash" outlined rounded severity="danger"
                                @click="onDeleteClick(slotProps.data)" />
                    </template>
                </Column>
            </DataTable>
        </div>

        <!--点击新增按钮出现的弹窗-->
        <Dialog v-model:visible="isShowSaveDialog" :style="{ width: '450px' }" header="新增用户" :modal="true">
            <div class="flex flex-col gap-6">
                <div>
                    <label for="name" class="block font-bold mb-3">姓名</label>
                    <InputText id="name" v-model.trim="user.name" required="true" autofocus
                               :invalid="isSubmitted && !user.name" fluid />
                    <small v-if="isSubmitted && !user.name" class="text-red-500">请输入姓名</small>
                </div>
                <div>
                    <label for="age" class="block font-bold mb-3">年龄</label>
                    <InputNumber id="age" v-model.trim="user.age" required="true" autofocus
                                 :invalid="isSubmitted && !user.age" fluid />
                    <small v-if="isSubmitted && !user.age" class="text-red-500">请输入年龄</small>
                </div>
            </div>

            <template #footer>
                <Button label="取消" icon="pi pi-times" text @click="hideSaveDialog" />
                <Button label="保存" icon="pi pi-check" @click="onSaveClick" />
            </template>
        </Dialog>

        <!--当点击删除用户的时候显示的弹窗-->
        <Dialog v-model:visible="isShowDeleteDialog" :style="{ width: '450px' }" header="确认删除" :modal="true">
            <div class="flex items-center gap-4">
                <i class="pi pi-exclamation-triangle !text-3xl" />
                <span v-if="user">您确定要删除用户 <b>{{ user.name }}</b>吗？</span>
            </div>
            <template #footer>
                <Button label="No" icon="pi pi-times" text @click="isShowDeleteDialog = false" />
                <Button label="Yes" icon="pi pi-check" @click="deleteById" />
            </template>
        </Dialog>

        <!--批量删除用户的弹窗-->
        <Dialog v-model:visible="isShowDeleteManyDialog" :style="{ width: '450px' }" header="确认删除"
                :modal="true">
            <div class="flex items-center gap-4">
                <i class="pi pi-exclamation-triangle !text-3xl" />
                <span v-if="user">您确定要删除当前被选中的所有用户吗？</span>
            </div>
            <template #footer>
                <Button label="No" icon="pi pi-times" text @click="isShowDeleteManyDialog = false" />
                <Button label="Yes" icon="pi pi-check" text @click="deleteMany" />
            </template>
        </Dialog>
    </div>
</template>

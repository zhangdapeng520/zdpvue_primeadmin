<script setup>
import { FilterMatchMode } from '@primevue/core/api';
import { useToast } from 'primevue/usetoast';
import { onMounted, ref } from 'vue';


const toast = useToast();
const dt = ref();
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
const isShowUserDialog = ref(false);
const deleteProductDialog = ref(false);
const deleteProductsDialog = ref(false);
const user = ref({});
const selectedProducts = ref();
const filters = ref({
    global: { value: null, matchMode: FilterMatchMode.CONTAINS }
});
const isSubmitted = ref(false); // 是否点击了保存按钮

// 点击新增按钮
const onAddClick = () => {
    user.value = {};
    isSubmitted.value = false;
    isShowUserDialog.value = true;
};

// 隐藏新增用户的弹窗
const hideAddUserDialog = () => {
    isShowUserDialog.value = false;
    isSubmitted.value = false;
};

// 点击保存新增用户新增按钮
const onSubmitAddUserClick = () => {
    isSubmitted.value = true;

    if (user?.value.name?.trim()) {
        if (user.value.id) {
            // 修改用户的逻辑
            userTableData.value[findIndexById(user.value.id)] = user.value;
            toast.add({ severity: 'success', summary: 'Successful', detail: 'user Updated', life: 3000 });
        } else {
            // 新增用户的逻辑
            user.value.id = createId();
            userTableData.value.push(user.value);
            toast.add({ severity: 'success', summary: '成功', detail: '新增用户成功', life: 3000 });
        }

        isShowUserDialog.value = false;
        user.value = {};
    }
};
const editProduct = (prod) => {
    user.value = { ...prod };
    isShowUserDialog.value = true;
};
const confirmDeleteProduct = (prod) => {
    user.value = prod;
    deleteProductDialog.value = true;
};
const deleteProduct = () => {
    userTableData.value = userTableData.value.filter((val) => val.id !== user.value.id);
    deleteProductDialog.value = false;
    user.value = {};
    toast.add({ severity: 'success', summary: 'Successful', detail: 'user Deleted', life: 3000 });
};
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
const createId = () => {
    let id = '';
    var chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
    for (var i = 0; i < 5; i++) {
        id += chars.charAt(Math.floor(Math.random() * chars.length));
    }
    return id;
};
const exportCSV = () => {
    dt.value.exportCSV();
};
const confirmDeleteSelected = () => {
    deleteProductsDialog.value = true;
};
const deleteSelectedProducts = () => {
    userTableData.value = userTableData.value.filter((val) => !selectedProducts.value.includes(val));
    deleteProductsDialog.value = false;
    selectedProducts.value = null;
    toast.add({ severity: 'success', summary: 'Successful', detail: 'userTableData Deleted', life: 3000 });
};
</script>

<template>
    <div>
        <div class="card">
            <Toolbar class="mb-6">
                <template #start>
                    <Button label="新增" icon="pi pi-plus" severity="secondary" class="mr-2" @click="onAddClick" />
                    <Button label="删除" icon="pi pi-trash" severity="secondary" @click="confirmDeleteSelected"
                            :disabled="!selectedProducts || !selectedProducts.length" />
                </template>

                <template #end>
                    <Button label="导出" icon="pi pi-upload" severity="secondary" @click="exportCSV($event)" />
                </template>
            </Toolbar>

            <DataTable
                ref="dt"
                v-model:selection="selectedProducts"
                :value="userTableData"
                dataKey="id"
                :paginator="true"
                :rows="10"
                :filters="filters"
                paginatorTemplate="FirstPageLink PrevPageLink PageLinks NextPageLink LastPageLink CurrentPageReport RowsPerPageDropdown"
                :rowsPerPageOptions="[5, 10, 25]"
                currentPageReportTemplate="Showing {first} to {last} of {totalRecords} userTableData"
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
            </DataTable>
        </div>

        <!--点击新增按钮出现的弹窗-->
        <Dialog v-model:visible="isShowUserDialog" :style="{ width: '450px' }" header="新增用户" :modal="true">
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
                <Button label="取消" icon="pi pi-times" text @click="hideAddUserDialog" />
                <Button label="保存" icon="pi pi-check" @click="onSubmitAddUserClick" />
            </template>
        </Dialog>

        <Dialog v-model:visible="deleteProductDialog" :style="{ width: '450px' }" header="Confirm" :modal="true">
            <div class="flex items-center gap-4">
                <i class="pi pi-exclamation-triangle !text-3xl" />
                <span v-if="user"
                >Are you sure you want to delete <b>{{ user.name }}</b
                >?</span
                >
            </div>
            <template #footer>
                <Button label="No" icon="pi pi-times" text @click="deleteProductDialog = false" />
                <Button label="Yes" icon="pi pi-check" @click="deleteProduct" />
            </template>
        </Dialog>

        <Dialog v-model:visible="deleteProductsDialog" :style="{ width: '450px' }" header="Confirm" :modal="true">
            <div class="flex items-center gap-4">
                <i class="pi pi-exclamation-triangle !text-3xl" />
                <span v-if="user">Are you sure you want to delete the selected userTableData?</span>
            </div>
            <template #footer>
                <Button label="No" icon="pi pi-times" text @click="deleteProductsDialog = false" />
                <Button label="Yes" icon="pi pi-check" text @click="deleteSelectedProducts" />
            </template>
        </Dialog>
    </div>
</template>

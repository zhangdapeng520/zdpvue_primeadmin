<script setup>
import FloatingConfigurator from '@/components/FloatingConfigurator.vue';
import { ref } from 'vue';

const username = ref('');
const password = ref('');
const isRemember = ref(true);
</script>

<template>
    <FloatingConfigurator />
    <div
        class="bg-surface-50 dark:bg-surface-950 flex items-center justify-center min-h-screen min-w-[100vw] overflow-hidden">
        <div class="flex flex-col items-center justify-center">
            <div
                style="border-radius: 56px; padding: 0.3rem; background: linear-gradient(180deg, var(--primary-color) 10%, rgba(33, 150, 243, 0) 30%)">
                <div class="w-full bg-surface-0 dark:bg-surface-900 py-20 px-8 sm:px-20" style="border-radius: 53px">
                    <div class="text-center mb-8">
                        <div class="text-surface-900 dark:text-surface-0 text-3xl font-medium mb-4">
                            欢迎登录本系统
                        </div>
                        <span class="text-muted-color font-medium">
                            请填写您的账号信息
                        </span>
                    </div>

                    <div>
                        <label for="username"
                               class="block text-surface-900 dark:text-surface-0 text-xl font-medium mb-2">
                            账号
                        </label>
                        <InputText
                            id="username"
                            type="text"
                            placeholder="请输入您的账号"
                            class="w-full md:w-[30rem] mb-8"
                            v-model="username" />

                        <label for="password"
                               class="block text-surface-900 dark:text-surface-0 font-medium text-xl mb-2">
                            密码
                        </label>
                        <Password id="password"
                                  v-model="password"
                                  placeholder="请输入您的密码"
                                  :toggleMask="true"
                                  class="mb-4" fluid
                                  :feedback="false" />

                        <div class="flex items-center justify-between mt-2 mb-8 gap-8">
                            <div class="flex items-center">
                                <Checkbox v-model="isRemember" id="isRemember" binary class="mr-2"></Checkbox>
                                <label for="isRemember">记住我</label>
                            </div>
                            <span class="font-medium no-underline ml-2 text-right cursor-pointer text-primary">
                                忘记密码？
                            </span>
                        </div>
                        <Button
                            label="立即登录"
                            class="w-full"
                            as="router-link"
                            to="/"
                        />
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.pi-eye {
    transform: scale(1.6);
    margin-right: 1rem;
}

.pi-eye-slash {
    transform: scale(1.6);
    margin-right: 1rem;
}
</style>

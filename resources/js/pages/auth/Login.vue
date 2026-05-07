<template>
    <!-- Updated AuthBase background to match landing page #F8F9FA -->
    <AuthBase 
        title="Welcome Back" 
        description="Enter your credentials to access the FeelDX Assistant"
        class="bg-[#F8F9FA]"
    >
        <Head title="Log in" />

        <div v-if="status" class="mb-4 text-center text-sm font-medium text-[#2D898B]">
            {{ status }}
        </div>

        <form @submit.prevent="submit" class="flex flex-col gap-6">
            <div class="grid gap-6">
                <!-- Email Field -->
                <div class="grid gap-2">
                    <Label for="email" class="text-xs uppercase tracking-widest text-gray-500 font-bold">Email address</Label>
                    <Input
                        id="email"
                        type="email"
                        required
                        autofocus
                        tabindex="1"
                        autocomplete="email"
                        v-model="form.email"
                        placeholder="designer@example.com"
                        class="rounded-xl border-gray-200 focus-visible:ring-[#2D898B] h-12"
                    />
                    <InputError :message="form.errors.email" />
                </div>

                <!-- Password Field -->
                <div class="grid gap-2">
                    <div class="flex items-center justify-between">
                        <Label for="password" class="text-xs uppercase tracking-widest text-gray-500 font-bold">Password</Label>
                        <TextLink 
                            v-if="canResetPassword" 
                            :href="route('password.request')" 
                            class="text-xs font-semibold text-[#2D898B] hover:text-[#246e70]" 
                            tabindex="5"
                        > 
                            Forgot password? 
                        </TextLink>
                    </div>
                    <Input
                        id="password"
                        type="password"
                        required
                        tabindex="2"
                        autocomplete="current-password"
                        v-model="form.password"
                        placeholder="••••••••"
                        class="rounded-xl border-gray-200 focus-visible:ring-[#2D898B] h-12"
                    />
                    <InputError :message="form.errors.password" />
                </div>

                <!-- Remember Me -->
                <div class="flex items-center justify-between" tabindex="3">
                    <Label for="remember" class="flex items-center space-x-3 cursor-pointer">
                        <Checkbox 
                            id="remember" 
                            v-model:checked="form.remember" 
                            tabindex="4" 
                            class="border-gray-300 data-[state=checked]:bg-[#2D898B] data-[state=checked]:border-[#2D898B]"
                        />
                        <span class="text-sm text-gray-600">Keep me logged in</span>
                    </Label>
                </div>

                <!-- Submit Button -->
                <Button 
                    type="submit" 
                    class="mt-2 w-full h-12 bg-[#2D898B] hover:bg-[#246e70] text-white rounded-xl font-bold shadow-lg shadow-[#2D898B]/10 transition-all active:scale-[0.98]" 
                    tabindex="4" 
                    :disabled="form.processing"
                >
                    <LoaderCircle v-if="form.processing" class="mr-2 h-4 w-4 animate-spin" />
                    Sign In to FeelDX
                </Button>
            </div>

            <div class="text-center text-sm text-gray-500">
                New to the platform?
                <TextLink 
                    :href="route('register')" 
                    class="font-bold text-[#2D898B] hover:underline" 
                    :tabindex="5"
                >
                    Create an account
                </TextLink>
            </div>
        </form>
    </AuthBase>
</template>
<script setup lang="ts">
import InputError from '@/components/InputError.vue';
import TextLink from '@/components/TextLink.vue';
import { Button } from '@/components/ui/button';
import { Checkbox } from '@/components/ui/checkbox';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';
import AuthBase from '@/layouts/AuthLayout.vue';
import { Head, useForm } from '@inertiajs/vue3';
import { LoaderCircle } from 'lucide-vue-next';

defineProps<{
    status?: string;
    canResetPassword: boolean;
}>();

const form = useForm({
    email: '',
    password: '',
    remember: false,
});

const submit = () => {
    form.post(route('login'), {
        onFinish: () => form.reset('password'),
    });
};
</script>

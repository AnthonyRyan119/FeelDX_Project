<template>
    <AuthBase 
        title="Create your Studio" 
        description="Join FeelDX to start curating materials and furniture"
        class="bg-[#F8F9FA]"
    >
        <Head title="Register" />

        <form @submit.prevent="submit" class="flex flex-col gap-6">
            <div class="grid gap-5">
                <!-- Name Field -->
                <div class="grid gap-2">
                    <Label for="name" class="text-xs uppercase tracking-widest text-gray-500 font-bold">Full Name</Label>
                    <Input 
                        id="name" 
                        type="text" 
                        required 
                        autofocus 
                        tabindex="1" 
                        autocomplete="name" 
                        v-model="form.name" 
                        placeholder="e.g. Jane Doe" 
                        class="rounded-xl border-gray-200 focus-visible:ring-[#2D898B] h-12"
                    />
                    <InputError :message="form.errors.name" />
                </div>

                <!-- Email Field -->
                <div class="grid gap-2">
                    <Label for="email" class="text-xs uppercase tracking-widest text-gray-500 font-bold">Email address</Label>
                    <Input 
                        id="email" 
                        type="email" 
                        required 
                        tabindex="2" 
                        autocomplete="email" 
                        v-model="form.email" 
                        placeholder="designer@studio.com" 
                        class="rounded-xl border-gray-200 focus-visible:ring-[#2D898B] h-12"
                    />
                    <InputError :message="form.errors.email" />
                </div>

                <!-- Password Field -->
                <div class="grid gap-2">
                    <Label for="password" class="text-xs uppercase tracking-widest text-gray-500 font-bold">Password</Label>
                    <Input
                        id="password"
                        type="password"
                        required
                        tabindex="3"
                        autocomplete="new-password"
                        v-model="form.password"
                        placeholder="••••••••"
                        class="rounded-xl border-gray-200 focus-visible:ring-[#2D898B] h-12"
                    />
                    <InputError :message="form.errors.password" />
                </div>

                <!-- Confirm Password -->
                <div class="grid gap-2">
                    <Label for="password_confirmation" class="text-xs uppercase tracking-widest text-gray-500 font-bold">Confirm password</Label>
                    <Input
                        id="password_confirmation"
                        type="password"
                        required
                        tabindex="4"
                        autocomplete="new-password"
                        v-model="form.password_confirmation"
                        placeholder="••••••••"
                        class="rounded-xl border-gray-200 focus-visible:ring-[#2D898B] h-12"
                    />
                    <InputError :message="form.errors.password_confirmation" />
                </div>

                <!-- Submit Button -->
                <Button 
                    type="submit" 
                    class="mt-4 w-full h-12 bg-[#2D898B] hover:bg-[#246e70] text-white rounded-xl font-bold shadow-lg shadow-[#2D898B]/10 transition-all active:scale-[0.98]" 
                    tabindex="5" 
                    :disabled="form.processing"
                >
                    <LoaderCircle v-if="form.processing" class="mr-2 h-4 w-4 animate-spin" />
                    Create Account
                </Button>
            </div>

            <!-- Login Link -->
            <div class="text-center text-sm text-gray-500">
                Already have an account?
                <TextLink 
                    :href="route('login')" 
                    class="font-bold text-[#2D898B] hover:underline" 
                    tabindex="6"
                >
                    Log in
                </TextLink>
            </div>
        </form>
    </AuthBase>
</template>
<script setup lang="ts">
import InputError from '@/components/InputError.vue';
import TextLink from '@/components/TextLink.vue';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';
import AuthBase from '@/layouts/AuthLayout.vue';
import { Head, useForm } from '@inertiajs/vue3';
import { LoaderCircle } from 'lucide-vue-next';

const form = useForm({
    name: '',
    email: '',
    password: '',
    password_confirmation: '',
});

const submit = () => {
    form.post(route('register'), {
        onFinish: () => form.reset('password', 'password_confirmation'),
    });
};
</script>

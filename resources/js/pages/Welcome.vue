<template>
    <Head title="FeelDX - Materials & Furniture Assistant" />

    <div class="min-h-screen bg-[#F8F9FA] text-[#1A1A1A] font-sans selection:bg-[#2D898B] selection:text-white overflow-x-hidden">
        <!-- Navigation -->
        <nav class="mx-auto max-w-7xl px-6 py-6 flex items-center justify-between">
            <div class="flex items-center gap-4 md:gap-12">
                <div class="text-xl md:text-2xl font-bold tracking-tighter flex items-center gap-2">
                    <div class="w-6 h-6 md:w-8 md:h-8 bg-[#2D898B] rounded-lg"></div>
                    FeelDX
                </div>
                <div class="hidden md:flex gap-8 text-sm font-medium text-gray-600">
                    <a href="#" class="hover:text-black transition">Solutions</a>
                    <a href="#" class="hover:text-black transition">Materials</a>
                    <a href="#" class="hover:text-black transition">Pricing</a>
                </div>
            </div>
            <div class="flex gap-2 md:gap-4">
                <Link :href="route('login')" class="px-4 md:px-6 py-2.5 text-xs md:text-sm font-semibold hover:bg-gray-100 rounded-xl transition">Login</Link>
                <Link :href="route('register')" class="px-4 md:px-6 py-2.5 bg-[#2D898B] text-white text-xs md:text-sm font-semibold rounded-xl hover:bg-[#246e70] transition">Sign Up</Link>
            </div>
        </nav>

        <!-- Hero Section -->
        <main class="mx-auto max-w-7xl px-6 pt-10 md:pt-16 pb-24 grid lg:grid-cols-2 gap-16 lg:gap-12 items-center">
            
            <!-- Left Content -->
            <div class="max-w-2xl order-2 lg:order-1">
                <h1 class="text-4xl md:text-6xl lg:text-7xl font-bold tracking-tight leading-[1.1] text-[#0F172A]">
                    Source materials <br class="hidden sm:block"/> 
                    <span class="text-[#2D898B]">curate furniture</span> <br class="hidden sm:block"/>
                    effortlessly.
                </h1>
                <p class="mt-6 md:mt-8 text-base md:text-lg text-gray-500 leading-relaxed max-w-lg">
                    The intelligent assistant for interior designers. Streamline your selection process with automated spec sheets and high-fidelity moodboards.
                </p>

                <!-- Email Input Group -->
                <div class="mt-10 flex flex-col sm:flex-row p-1.5 bg-white border border-gray-200 rounded-2xl shadow-sm focus-within:ring-2 focus-within:ring-[#2D898B]/20 max-w-md gap-2 sm:gap-0">
                    <input 
                        type="email" 
                        placeholder="Work email address" 
                        class="flex-1 border-none focus:ring-0 text-sm px-4 py-3 sm:py-0 bg-transparent"
                    >
                    <button class="bg-[#2D898B] text-white px-6 py-3 rounded-xl text-sm font-bold flex items-center justify-center gap-2 hover:bg-[#246e70] transition w-full sm:w-auto">
                        Get Started <span class="text-lg">↗</span>
                    </button>
                </div>

                <!-- Trusted By -->
                <div class="mt-16 flex flex-wrap items-center gap-6 md:gap-8 opacity-40 grayscale">
                    <span class="font-black text-lg md:text-xl italic">ARCH.DIGEST</span>
                    <span class="font-bold text-lg md:text-xl tracking-tighter">STUDIO.OS</span>
                    <span class="font-serif text-lg md:text-xl">INTERIORS.co</span>
                </div>
            </div>

            <!-- Right Visual -->
            <div class="relative flex justify-center lg:justify-end order-1 lg:order-2 mt-12 lg:mt-0">
                <!-- Background Decorative Shape -->
                <div class="absolute -top-10 -right-10 w-[300px] h-[300px] md:w-[500px] md:h-[500px] bg-[#2D898B]/5 rounded-full blur-3xl"></div>
                
                <!-- Main Card -->
                <div class="relative w-full max-w-[280px] sm:max-w-sm bg-white rounded-[2rem] shadow-2xl shadow-gray-200/50 p-6 md:p-8 border border-gray-100">
                    <div class="flex justify-between items-center mb-6 md:mb-8">
                        <div class="flex items-center gap-3">
                            <div class="w-8 h-8 md:w-10 md:h-10 bg-gray-100 rounded-full flex items-center justify-center text-lg md:text-xl">🪵</div>
                            <div>
                                <p class="text-xs md:text-sm font-bold">Oak Veneer</p>
                                <p class="text-[9px] md:text-[10px] text-gray-400">Natural Finish • Grade A</p>
                            </div>
                        </div>
                        <div class="w-4 h-4 md:w-5 md:h-5 rounded-full border-2 border-[#2D898B] flex items-center justify-center">
                            <div class="w-1.5 h-1.5 md:w-2 md:h-2 bg-[#2D898B] rounded-full"></div>
                        </div>
                    </div>

                    <div class="space-y-4">
                        <div class="h-24 md:h-32 bg-gray-50 rounded-xl flex items-center justify-center border border-dashed border-gray-200">
                            <span class="text-[9px] md:text-[10px] uppercase tracking-widest text-gray-400">Texture Preview</span>
                        </div>
                        <div class="flex justify-between items-end pt-2">
                            <div>
                                <p class="text-[9px] md:text-[10px] uppercase text-gray-400 font-bold">Estimated Cost</p>
                                <p class="text-2xl md:text-3xl font-bold">$12,450</p>
                            </div>
                        </div>
                        <button class="w-full py-3 md:py-4 bg-[#0F172A] text-white rounded-xl font-bold text-xs md:text-sm mt-2">
                            Add to Design Sheet
                        </button>
                    </div>
                </div>

                <!-- Floating Card -->
                <div class="absolute -top-12 right-0 sm:-right-4 md:-right-12 w-48 md:w-64 bg-[#2D898B] text-white rounded-3xl shadow-xl p-4 md:p-6 transform rotate-3 scale-90 md:scale-100">
                    <div class="flex justify-between mb-3 md:mb-4">
                        <p class="text-[9px] md:text-[10px] font-bold uppercase tracking-widest opacity-80">Chair Selection</p>
                        <span class="text-base md:text-lg">🪑</span>
                    </div>
                    <p class="text-xl md:text-2xl font-bold mb-1 italic font-serif leading-none">Eames</p>
                    <p class="text-[10px] md:text-xs opacity-80 mb-4 md:mb-6">Lounge & Ottoman</p>
                    
                    <div class="flex items-center gap-2">
                        <div class="flex -space-x-2">
                            <div class="w-5 h-5 md:w-6 md:h-6 rounded-full border-2 border-[#2D898B] bg-[#3A4344]"></div>
                            <div class="w-5 h-5 md:w-6 md:h-6 rounded-full border-2 border-[#2D898B] bg-[#8B4513]"></div>
                            <div class="w-5 h-5 md:w-6 md:h-6 rounded-full border-2 border-[#2D898B] bg-white/20"></div>
                        </div>
                        <span class="text-[8px] md:text-[10px] font-bold">+4 Swatches</span>
                    </div>
                </div>
            </div>
        </main>
    </div>
</template>
<script setup lang="ts">
import { Head, Link } from '@inertiajs/vue3';
</script>

<template>
    <Head title="Selection Assistant" />

    <AppLayout :breadcrumbs="breadcrumbs">
        <div class="max-w-6xl mx-auto p-4 sm:p-6 lg:p-8">
            <!-- Header Section -->
            <div class="flex flex-col sm:flex-row sm:justify-between sm:items-end gap-4 mb-8">
                <div>
                    <h1 class="text-2xl sm:text-3xl font-extrabold tracking-tight text-gray-900">
                        FeelDX Assistant
                    </h1>
                    <p class="text-gray-500 mt-1 text-sm sm:text-base max-w-md">
                        Configure your space and generate instant AI design feedback.
                    </p>
                </div>
                
                <button 
                    @click="openResetModal"
                    class="text-sm font-medium text-gray-500 hover:text-red-600 transition-colors flex items-center justify-center gap-1 w-full sm:w-auto bg-gray-50 sm:bg-transparent px-4 py-3 sm:p-0 rounded-lg border sm:border-0 border-gray-200 shadow-sm sm:shadow-none"
                >
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                        <path d="M3 12a9 9 0 1 0 9-9 9.75 9.75 0 0 0-6.74 2.74L3 8"/>
                        <path d="M3 3v5h5"/>
                    </svg>
                    Reset Selections
                </button>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-12 gap-8">
                
                <!-- ROOM CONFIGURATION -->
                <div class="lg:col-span-7 space-y-6">
                    <div class="bg-white border border-gray-200 rounded-xl p-6 shadow-sm">
                        <h3 class="text-sm font-semibold uppercase tracking-wider text-gray-400 mb-6">1. Room Selection</h3>
                        
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Primary Room Type</label>
                            <v-select 
                                v-model="selections.roomType" 
                                :options="roomOptions" 
                                placeholder="Select a room..."
                                class="design-select"
                            />
                        </div>

                        <div v-if="selections.roomType" class="mt-8 space-y-6 animate-in fade-in slide-in-from-top-4 duration-500">
                            <h3 class="text-sm font-semibold uppercase tracking-wider text-gray-400">2. Materials & Furniture</h3>
                            
                            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                                <div v-for="(options, category) in availableOptions" :key="category">
                                    <label class="block text-xs font-bold text-gray-500 uppercase mb-1 ml-1">{{ category }}</label>
                                    <v-select 
                                        v-model="selections[category]" 
                                        :options="options" 
                                        placeholder="None Selected"
                                        class="design-select"
                                    />
                                </div>
                            </div>

                            <div class="flex flex-col gap-3 mt-4">
                                <button 
                                    @click="generateAiSummary" 
                                    class="w-full bg-[#2D898B] text-white font-bold py-3 px-6 rounded-lg hover:bg-[#246e70] shadow-[#2D898B]/20 transition-all flex items-center justify-center gap-2"
                                >
                                    <span>Generate AI Summary</span>
                                    <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m12 3-1.912 5.813a2 2 0 0 1-1.275 1.275L3 12l5.813 1.912a2 2 0 0 1 1.275 1.275L12 21l1.912-5.813a2 2 0 0 1 1.275-1.275L21 12l-5.813-1.912a2 2 0 0 1-1.275-1.275L12 3Z"/><path d="M5 3v4"/><path d="M19 17v4"/><path d="M3 5h4"/><path d="M17 19h4"/></svg>
                                </button>

                                <button 
                                    @click="saveCurrentSelection" 
                                    class="w-full border-2 border-gray-100 text-black font-bold py-3 px-6 rounded-lg hover:bg-gray-50 transition-all flex items-center justify-center gap-2"
                                >
                                    <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M19 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11l5 5v11a2 2 0 0 1-2 2z"/><polyline points="17 21 17 13 7 13 7 21"/><polyline points="7 3 7 8 15 8"/></svg>
                                    Save Configuration
                                </button>
                            </div>
                        </div>
                        
                        <div v-else class="mt-8 py-12 border-2 border-dashed border-gray-100 rounded-lg text-center">
                            <p class="text-gray-400 italic">Please select a room type to start configuring materials.</p>
                        </div>
                    </div>
                </div>

                <!-- AI RESPONSE & SAVED HISTORY -->
                <div class="lg:col-span-5 space-y-8">
                    <!-- AI RESPONSE -->
                    <div class="bg-gray-900 text-white rounded-xl p-6 shadow-xl sticky top-8">
                        <h2 class="text-lg font-bold mb-6 flex items-center gap-2">
                            <span class="w-2 h-2 bg-green-400 rounded-full animate-pulse"></span>
                            Live AI Feedback
                        </h2>

                        <div v-if="!showAiSummary" class="py-10 text-center">
                            <p class="text-gray-400 text-sm">Configure your room on the left and click generate to see AI insights.</p>
                        </div>

                        <div v-else class="space-y-6 animate-in zoom-in-95 duration-300">
                            <div class="flex flex-wrap gap-2 pb-6 border-b border-gray-800">
                                <span class="px-3 py-1 bg-gray-800 rounded-full text-xs font-medium">Room: {{ selections.roomType }}</span>
                                <span v-for="(val, key) in selections" :key="key" v-show="key !== 'roomType' && val" class="px-3 py-1 bg-gray-800 rounded-full text-xs font-medium">
                                    {{ val }}
                                </span>
                            </div>

                            <div class="space-y-4">
                                <div class="p-3 bg-gray-800/50 rounded-lg">
                                    <span class="text-xs text-gray-400 block mb-1 uppercase font-bold tracking-tighter">Cost Estimate</span>
                                    <p class="text-sm font-semibold">{{ aiResponse.cost }}</p>
                                </div>

                                <div class="p-3 bg-gray-800/50 rounded-lg">
                                    <span class="text-xs text-gray-400 block mb-1 uppercase font-bold tracking-tighter">Design Notes</span>
                                    <p class="text-sm leading-relaxed">{{ aiResponse.compatibility }}</p>
                                </div>

                                <div v-if="aiResponse.missing" class="p-3 bg-orange-900/30 border border-orange-500/30 rounded-lg">
                                    <span class="text-xs text-orange-400 block mb-1 uppercase font-bold tracking-tighter">Missing Items</span>
                                    <p class="text-sm text-orange-200">{{ aiResponse.missing }}</p>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- SAVED HISTORY -->
                    <div v-if="savedConfigs.length > 0" class="space-y-4 animate-in slide-in-from-bottom-4 duration-500">
                        <h3 class="text-sm font-semibold uppercase tracking-wider text-gray-500 flex items-center gap-2">
                            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 3v18h18"/><path d="m7 15 4-4 4 4 5-5"/></svg>
                            Saved History
                        </h3>
                        
                        <div 
                            v-for="item in savedConfigs" 
                            :key="item.id" 
                            @click="loadSavedConfig(item.data)"
                            class="bg-white border border-gray-200 rounded-lg p-4 shadow-sm hover:border-black cursor-pointer transition-all group"
                        >
                            <div class="flex justify-between items-start">
                                <div>
                                    <p class="font-bold text-gray-900 capitalize">{{ item.data.roomType }} Design</p>
                                    <p class="text-xs text-gray-400">{{ item.timestamp }}</p>
                                </div>
                                <button @click.stop="deleteSaved(item.id)" class="text-gray-300 hover:text-red-500 transition-colors p-1">
                                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 6h18"/><path d="M19 6v14c0 1-1 2-2 2H7c-1 0-2-1-2-2V6"/><path d="M8 6V4c0-1 1-2 2-2h4c1 0 2 1 2 2v2"/></svg>
                                </button>
                            </div>
                            
                            <div class="mt-2 flex flex-wrap gap-1">
                                <span v-for="(val, key) in item.data" :key="key" v-show="val && key !== 'roomType'" class="text-[10px] px-2 py-0.5 bg-gray-100 text-gray-600 rounded">
                                    {{ val }}
                                </span>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Reset Confirmation Modal -->
                <div v-if="isResetModalOpen" class="fixed inset-0 z-50 flex items-center justify-center p-4 sm:p-0">
                    <div @click="closeResetModal" class="absolute inset-0 bg-gray-900/50 backdrop-blur-sm animate-in fade-in duration-300"></div>
                    <div class="relative bg-white rounded-xl shadow-2xl max-w-sm w-full p-6 animate-in zoom-in-95 duration-200">
                        <div class="flex items-center justify-center w-12 h-12 mx-auto bg-red-100 rounded-full mb-4">
                            <svg class="w-6 h-6 text-red-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z" />
                            </svg>
                        </div>
                        <h3 class="text-lg font-bold text-center text-gray-900 mb-2">Clear all selections?</h3>
                        <p class="text-sm text-center text-gray-500 mb-6">
                            This will reset your room configuration and AI insights. This action cannot be undone.
                        </p>
                        <div class="flex flex-col sm:flex-row gap-3">
                            <button @click="confirmReset" class="flex-1 bg-red-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-red-700 transition-colors order-2 sm:order-1">
                                Yes, reset
                            </button>
                            <button @click="closeResetModal" class="flex-1 bg-gray-100 text-gray-700 font-bold py-2 px-4 rounded-lg hover:bg-gray-200 transition-colors order-1 sm:order-2">
                                Cancel
                            </button>
                        </div>
                    </div>
                </div>

            </div>
        </div>
    </AppLayout>
</template>
<style>
/* Dropdown styles */
.design-select .vs__dropdown-toggle {
    background: #f9fafb;
    border: 1px solid #e5e7eb; 
    border-radius: 0.5rem; 
    padding: 4px 0;
}
.design-select .vs__selected { color: #111827; font-size: 0.875rem; }
.design-select .vs__search::placeholder { color: #9ca3af; }
.design-select .vs__clear, .design-select .vs__open-indicator { fill: #9ca3af; }
</style>
<script setup lang="ts">
import AppLayout from '@/layouts/AppLayout.vue';
import { type BreadcrumbItem } from '@/types';
import { Head } from '@inertiajs/vue3';
import { ref, reactive, computed, watch, inject } from 'vue';
import Swal from 'sweetalert2';

const breadcrumbs: BreadcrumbItem[] = [
    { title: 'Materials Assistant', href: '/dashboard' },
];

const roomOptions = ['kitchen', 'bathroom', 'living room', 'bedroom', 'laundry'];

const availableOptions = {
    flooring: ['Hardwood', 'Marble Tile', 'Vinyl', 'Carpet'],
    wallFinish: ['White Paint', 'Dark Charcoal Paint', 'Wallpaper', 'Exposed Brick'],
    furniture: ['Sofa', 'Dining Table', 'King Bed', 'Office Desk'],
    lighting: ['Pendant Light', 'Recessed LED', 'Floor Lamp'],
    cabinetry: ['Modern Matte', 'Classic Oak', 'High Gloss White']
};

const initialState = {
    roomType: '',
    flooring: '',
    wallFinish: '',
    furniture: '',
    lighting: '',
    cabinetry: ''
};

const selections = reactive<Record<string, string>>({ ...initialState });
const showAiSummary = ref(false);
const isResetModalOpen = ref(false);
const savedConfigs = ref<any[]>([]);

const swal = inject('$swal') as any;
const Toast = swal.mixin({
    toast: true,
    position: 'top-end',
    showConfirmButton: false,
    timer: 3000,
    timerProgressBar: true,
});

// Computed Logic
const aiResponse = computed(() => {
    if (!selections.roomType) {
        return { cost: '---', compatibility: 'Please select a room.', missing: '', nextAction: '' };
    }

    let cost = 'Standard';
    let compatibility = 'Selections look cohesive and follow modern design principles.';
    let missing = '';
    let nextAction = 'Finalize your material board and check local availability.';

    if (selections.flooring === 'Marble Tile' || selections.cabinetry === 'Modern Matte') cost = 'Premium / High-End';
    if (selections.flooring === 'Vinyl') cost = 'Budget Friendly / Practical';

    if (selections.flooring === 'Hardwood' && selections.wallFinish === 'Dark Charcoal Paint') {
        compatibility = 'Design Alert: The dark wall finish combined with natural wood creates a moody atmosphere. Ensure high-lumen lighting.';
    }

    if (!selections.lighting) missing = 'No lighting source selected.';

    return { cost, compatibility, missing, nextAction };
});

// Watcher
watch(() => selections.roomType, (newVal) => {
    if (!newVal) showAiSummary.value = false;
});

// Functions
function generateAiSummary() { showAiSummary.value = true; };
function openResetModal() { isResetModalOpen.value = true; };
function closeResetModal() { isResetModalOpen.value = false; };

function confirmReset() {
    Object.assign(selections, initialState);
    showAiSummary.value = false;
    closeResetModal();
    Toast.fire({ icon: 'success', title: 'Your workspace has been cleared' });
};

function saveCurrentSelection() {
    const newEntry = {
        id: Date.now(),
        timestamp: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit',hour12: true }),
        data: { ...selections }
    };
    savedConfigs.value.unshift(newEntry);
    Toast.fire({ icon: 'success', title: 'Configuration Saved' });
};

function loadSavedConfig(data: any) {
    Object.assign(selections, data);
    showAiSummary.value = true; // Auto-generate summary for loaded design
    Toast.fire({ icon: 'info', title: 'Design Loaded' });
};

function deleteSaved(id: number) {
    savedConfigs.value = savedConfigs.value.filter(item => item.id !== id);
};
</script>


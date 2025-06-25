<script>

    import { onMount } from 'svelte';
    
    let sessionId = 'exc-0003';
    let chatMessages = [];
    let currentMessage = '';
    let cdnServices = [];
    let executionPlan = [];
    let executionHistory = [];
    let expectedResults = [];
    
    // Reactive state for chat
    $: canExecute = executionPlan.length > 0;
    
    function addMessage(sender, text) {
        chatMessages = [...chatMessages, { sender, text, timestamp: new Date() }];
    }
    
    function sendMessage() {
        if (!currentMessage.trim()) return;
        
        addMessage('user', currentMessage);
        // Simulate AI response processing
        setTimeout(() => {
            addMessage('assistant', 'I will help you create the necessary CDN configuration.');
        }, 500);
        
        currentMessage = '';
    }
    
    function executeActions() {
        if (!canExecute) return;
        
        // Move execution plan to history
        executionHistory = [...executionHistory, ...executionPlan.map(item => ({
            ...item,
            executedAt: new Date(),
            status: 'completed'
        }))];
        
        // Clear execution plan
        executionPlan = [];
        expectedResults = [];
    }
    
    function cancelExecution() {
        executionPlan = [];
        expectedResults = [];
    }
    
    function restoreExecution(historyItem) {
        // Add back to execution plan
        executionPlan = [historyItem];
    }
    
    onMount(() => {
        // Initialize with empty state
        cdnServices = [];
        executionPlan = [];
        expectedResults = [];
    });
</script>

<svelte:head>
    <title>CDNbuddy Assistant - {sessionId}</title>
</svelte:head>

<div class="min-h-screen bg-gradient-to-br from-slate-50 via-blue-50 to-indigo-100">
    <!-- Background decoration -->
    <div class="absolute inset-0 bg-grid-slate-100 [mask-image:linear-gradient(0deg,white,rgba(255,255,255,0.6))] -z-10"></div>
    
    <div class="relative z-10 p-6">
        <div class="max-w-7xl mx-auto">
            <!-- Header -->
            <div class="mb-6">
                <div class="flex items-center justify-between mb-6">
                    <div class="flex items-center gap-4">
                        <div class="w-12 h-12 bg-gradient-to-r from-blue-500 to-purple-600 rounded-2xl flex items-center justify-center shadow-lg shadow-blue-500/25">
                            <span class="text-2xl">🤖</span>
                        </div>
                        <div>
                            <h1 class="text-2xl font-bold bg-gradient-to-r from-gray-900 to-gray-600 bg-clip-text text-transparent">
                                CDN buddy 👋 your comrade here!
                            </h1>
                            <p class="text-gray-500 text-sm">I will handle your infrastructure management</p>
                        </div>
                    </div>
                    <div class="flex items-center gap-3">
                        <div class="px-3 py-1 bg-white/80 backdrop-blur-sm border border-white/20 rounded-full text-sm text-gray-600 shadow-sm">
                            {sessionId}
                        </div>
                        <div class="w-3 h-3 bg-green-400 rounded-full animate-pulse shadow-lg shadow-green-400/50"></div>
                    </div>
                </div>
            </div>

            <!-- Main Content Grid -->
            <div class="grid grid-cols-1 lg:grid-cols-4 gap-4 mb-6">
                
                <!-- Current State -->
                <div class="bg-white/90 backdrop-blur-sm border border-green-200/50 rounded-2xl overflow-hidden shadow-xl shadow-green-500/10">
                    <div class="bg-gradient-to-r from-green-500 to-emerald-500 px-6 py-2">
                        <h2 class="text-white font-semibold flex items-center gap-2 text-sm">
                            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"></path>
                            </svg>
                            Current CDN State
                        </h2>
                    </div>
                    <div class="p-6">
                        {#if cdnServices.length === 0}
                            <div class="text-center text-gray-400 py-8">
                                <svg class="w-16 h-16 mx-auto mb-4 opacity-50" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 11H5m14 0a2 2 0 012 2v6a2 2 0 01-2 2H5a2 2 0 01-2-2v-6a2 2 0 012-2m14 0V9a2 2 0 00-2-2M5 11V9a2 2 0 012-2m0 0V5a2 2 0 012-2h6a2 2 0 012 2v2M7 7h10"></path>
                                </svg>
                                <p class="text-sm font-medium">No CDN services configured</p>
                                <p class="text-xs text-gray-400 mt-1">Your services will appear here</p>
                            </div>
                        {:else}
                            {#each cdnServices as service}
                                <div class="flex items-center gap-3 p-3 bg-green-50 rounded-xl mb-3 last:mb-0">
                                    <div class="w-2 h-2 bg-green-500 rounded-full"></div>
                                    <span class="text-sm font-medium text-green-800">{service.name}</span>
                                </div>
                            {/each}
                        {/if}
                    </div>
                </div>

                <!-- Chat Window -->
                <div class="lg:col-span-2 bg-white/90 backdrop-blur-sm border border-red-200/50 rounded-2xl overflow-hidden shadow-xl shadow-red-500/10">
                    <div class="bg-gradient-to-r from-red-500 to-pink-500 px-6 py-2">
                        <h2 class="text-white font-semibold flex items-center gap-2 text-sm">
                            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z"></path>
                            </svg>
                            Chat Window
                        </h2>
                    </div>
                    <div class="p-6 h-72 overflow-y-auto">
                        {#each chatMessages as message}
                            <div class="mb-6 {message.sender === 'user' ? 'text-right' : ''}">
                                <div class="inline-block max-w-xs">
                                    <div class="text-xs text-gray-500 mb-2 font-medium">
                                        {message.sender === 'user' ? 'You' : '🤖 Assistant'}
                                    </div>
                                    <div class="bg-{message.sender === 'user' ? 'blue-500 text-white' : 'gray-100 text-gray-800'} rounded-2xl px-4 py-3 text-sm shadow-lg">
                                        {message.text}
                                    </div>
                                </div>
                            </div>
                        {/each}
                        {#if chatMessages.length === 0}
                            <div class="text-center text-gray-400 mt-16">
                                <svg class="w-12 h-12 mx-auto mb-4 opacity-50" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z"></path>
                                </svg>
                                <p class="text-sm">Start a conversation</p>
                            </div>
                        {/if}
                    </div>
                    <div class="border-t border-gray-100 p-4 bg-gray-50/50">
                        <div class="flex gap-3">
                            <input
                                type="text"
                                bind:value={currentMessage}
                                on:keydown={(e) => e.key === 'Enter' && sendMessage()}
                                placeholder="Type your message..."
                                class="flex-1 px-4 py-3 border border-gray-200 rounded-xl text-sm focus:outline-none focus:border-blue-500 focus:ring-2 focus:ring-blue-500/20 bg-white/80"
                            />
                            <button
                                on:click={sendMessage}
                                class="px-6 py-3 bg-gradient-to-r from-blue-500 to-purple-600 text-white rounded-xl text-sm font-medium hover:shadow-lg hover:shadow-blue-500/25 transition-all duration-200 transform hover:scale-105"
                            >
                                Send
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Execution Plan -->
                <div class="bg-white/90 backdrop-blur-sm border border-orange-200/50 rounded-2xl overflow-hidden shadow-xl shadow-orange-500/10">
                    <div class="bg-gradient-to-r from-orange-500 to-amber-500 px-6 py-2">
                        <div class="flex items-center justify-between">
                            <h2 class="text-white font-semibold flex items-center gap-2 text-sm">
                                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path>
                                </svg>
                                Execution Plan
                            </h2>
                            <span class="text-xs text-orange-100 bg-white/20 px-2 py-1 rounded-full">
                                updated 1s ago
                            </span>
                        </div>
                    </div>
                    <div class="p-6">
                        {#if executionPlan.length === 0}
                            <div class="text-center text-gray-400 py-8">
                                <svg class="w-16 h-16 mx-auto mb-4 opacity-50" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v10a2 2 0 002 2h8a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2"></path>
                                </svg>
                                <p class="text-sm font-medium">No actions planned</p>
                                <p class="text-xs text-gray-400 mt-1">Actions will appear here</p>
                            </div>
                        {:else}
                            {#each executionPlan as action, index}
                                <div class="bg-gradient-to-r from-gray-50 to-gray-100 rounded-xl p-4 mb-4 last:mb-0 border border-gray-200/50">
                                    <div class="flex items-center justify-between mb-2">
                                        <span class="text-sm font-semibold text-gray-800">
                                            {String(index + 1).padStart(2, '0')}: {action.title}
                                        </span>
                                    </div>
                                    <div class="flex gap-2">
                                        <button class="text-xs bg-green-100 text-green-700 px-3 py-1 rounded-lg hover:bg-green-200 transition-colors font-medium">
                                            Detail
                                        </button>
                                        <button class="text-xs bg-blue-100 text-blue-700 px-3 py-1 rounded-lg hover:bg-blue-200 transition-colors font-medium">
                                            Apply
                                        </button>
                                        <button class="text-xs bg-red-100 text-red-700 px-3 py-1 rounded-lg hover:bg-red-200 transition-colors font-medium">
                                            Delete
                                        </button>
                                    </div>
                                    {#if action.dependency}
                                        <div class="mt-2 text-xs bg-amber-100 text-amber-800 px-3 py-2 rounded-lg border border-amber-200">
                                            ⚠️ Depends on step #{action.dependency}
                                        </div>
                                    {/if}
                                </div>
                            {/each}
                        {/if}
                    </div>
                </div>
            </div>

            <!-- Expected Results -->
            {#if expectedResults.length > 0}
                <div class="bg-white/90 backdrop-blur-sm border border-blue-200/50 rounded-2xl overflow-hidden shadow-xl shadow-blue-500/10 mb-4">
                    <div class="bg-gradient-to-r from-blue-500 to-indigo-500 px-6 py-2">
                        <h2 class="text-white font-semibold flex items-center gap-2 text-sm">
                            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"></path>
                            </svg>
                            Expected Results
                        </h2>
                    </div>
                    <div class="p-6">
                        {#each expectedResults as result}
                            <div class="flex items-center gap-3 mb-2 last:mb-0">
                                <div class="w-2 h-2 bg-blue-500 rounded-full"></div>
                                <p class="text-sm text-gray-700">{result}</p>
                            </div>
                        {/each}
                    </div>
                </div>
            {/if}

            <!-- Action Buttons -->
            <div class="bg-white/80 backdrop-blur-sm border border-white/20 rounded-2xl p-6 shadow-xl shadow-black/5 mb-4">
                <div class="flex justify-center">
                    <div class="flex gap-4 w-96">
                        <button
                            on:click={executeActions}
                            disabled={!canExecute}
                            class="flex-1 bg-gradient-to-r from-green-500 to-emerald-600 text-white py-3 px-5 rounded-xl font-medium hover:shadow-lg hover:shadow-green-500/25 disabled:from-gray-300 disabled:to-gray-400 disabled:cursor-not-allowed disabled:shadow-none flex items-center justify-center gap-2 transition-all duration-200 transform hover:scale-[1.02] disabled:hover:scale-100"
                        >
                            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path>
                            </svg>
                            EXECUTE {executionPlan.length} ACTION{executionPlan.length !== 1 ? 'S' : ''}
                        </button>
                        <button
                            on:click={cancelExecution}
                            class="flex-1 bg-gradient-to-r from-red-500 to-pink-600 text-white py-3 px-5 rounded-xl font-medium hover:shadow-lg hover:shadow-red-500/25 flex items-center justify-center gap-2 transition-all duration-200 transform hover:scale-[1.02]"
                        >
                            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                            </svg>
                            CANCEL
                        </button>
                    </div>
                </div>
            </div>

            <!-- Execution History -->
            <div class="bg-white/90 backdrop-blur-sm border border-gray-200/50 rounded-2xl overflow-hidden shadow-xl shadow-black/5">
                <div class="bg-gradient-to-r from-gray-700 to-gray-800 px-6 py-2">
                    <h2 class="text-white font-semibold flex items-center gap-2 text-sm">
                        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"></path>
                        </svg>
                        Execution History
                        <span class="text-xs text-gray-300 ml-1">(Audit Trail)</span>
                    </h2>
                </div>
                <div class="p-6">
                    {#if executionHistory.length === 0}
                        <div class="text-center text-gray-400 py-8">
                            <svg class="w-16 h-16 mx-auto mb-4 opacity-50" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"></path>
                            </svg>
                            <p class="text-sm font-medium">No execution history</p>
                            <p class="text-xs text-gray-400 mt-1">Completed actions will appear here</p>
                        </div>
                    {:else}
                        {#each executionHistory as item}
                            <div class="flex items-center justify-between py-4 px-4 border border-gray-100 rounded-xl mb-3 last:mb-0 bg-gradient-to-r from-gray-50 to-white">
                                <div class="flex items-center gap-3">
                                    <div class="w-2 h-2 bg-green-500 rounded-full"></div>
                                    <div>
                                        <span class="text-sm font-semibold text-gray-800">{item.id}:</span>
                                        <span class="text-sm text-gray-600 ml-1">{item.title}</span>
                                    </div>
                                </div>
                                <button
                                    on:click={() => restoreExecution(item)}
                                    class="text-xs bg-gradient-to-r from-blue-100 to-indigo-100 text-blue-700 px-4 py-2 rounded-lg hover:from-blue-200 hover:to-indigo-200 transition-all font-medium"
                                >
                                    Restore
                                </button>
                            </div>
                        {/each}
                    {/if}
                </div>
            </div>
        </div>
    </div>
</div>
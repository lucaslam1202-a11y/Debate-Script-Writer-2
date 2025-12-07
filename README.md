<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Free Debate Script Writer | AI-Powered</title>
    <!-- Tailwind CSS (for styling/color schemes) -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Awesome (icons) -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.7.2/css/all.min.css">
    <!-- PDF Export -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
    <!-- Custom Tailwind Config (color schemes) -->
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        primary: '#3b82f6', // Default blue
                        secondary: '#10b981', // Default green
                        dark: '#1f2937',
                        light: '#f3f4f6'
                    },
                }
            }
        }
    </script>
    <style type="text/tailwindcss">
        @layer utilities {
            .content-auto {
                content-visibility: auto;
            }
            .script-container {
                white-space: pre-wrap;
                line-height: 1.6;
            }
            .color-picker-input {
                -webkit-appearance: none;
                width: 50px;
                height: 50px;
                border: none;
                cursor: pointer;
            }
            .color-picker-input::-webkit-color-swatch-wrapper {
                padding: 0;
            }
            .color-picker-input::-webkit-color-swatch {
                border: 2px solid #ddd;
                border-radius: 50%;
            }
        }
    </style>
</head>
<body class="bg-light text-dark transition-colors duration-300">
    <!-- Header -->
    <header class="bg-primary text-white py-4 px-6 shadow-lg">
        <div class="container mx-auto flex justify-between items-center">
            <h1 class="text-2xl font-bold">AI Debate Script Writer</h1>
            <div class="flex gap-4">
                <button id="themeToggle" class="p-2 rounded-full hover:bg-primary/80 transition">
                    <i class="fas fa-moon"></i>
                </button>
                <button id="colorSettingsBtn" class="p-2 rounded-full hover:bg-primary/80 transition">
                    <i class="fas fa-palette"></i>
                </button>
            </div>
        </div>
    </header>

    <!-- Color Settings Modal (Hidden by Default) -->
    <div id="colorSettingsModal" class="fixed inset-0 bg-black/50 flex items-center justify-center z-50 hidden">
        <div class="bg-light rounded-lg p-6 w-full max-w-md shadow-2xl">
            <div class="flex justify-between items-center mb-4">
                <h2 class="text-xl font-bold">Customize Color Scheme</h2>
                <button id="closeColorModal" class="text-dark hover:text-red-500">
                    <i class="fas fa-times text-xl"></i>
                </button>
            </div>
            <div class="space-y-4">
                <div>
                    <label class="block mb-2 font-medium">Primary Color (Header/Buttons)</label>
                    <input type="color" id="primaryColorPicker" class="color-picker-input" value="#3b82f6">
                </div>
                <div>
                    <label class="block mb-2 font-medium">Secondary Color (Accents)</label>
                    <input type="color" id="secondaryColorPicker" class="color-picker-input" value="#10b981">
                </div>
                <div>
                    <label class="block mb-2 font-medium">Theme Mode</label>
                    <div class="flex gap-4">
                        <button id="lightModeBtn" class="px-4 py-2 rounded bg-primary text-white hover:bg-primary/80">Light</button>
                        <button id="darkModeBtn" class="px-4 py-2 rounded bg-dark text-white hover:bg-dark/80">Dark</button>
                    </div>
                </div>
                <button id="saveColorSettings" class="w-full py-2 rounded bg-secondary text-white font-medium hover:bg-secondary/80 transition">
                    Save Settings
                </button>
            </div>
        </div>
    </div>

    <!-- Main Content -->
    <main class="container mx-auto px-6 py-8">
        <div class="grid md:grid-cols-12 gap-8">
            <!-- Input Form (Left Column) -->
            <div class="md:col-span-4 bg-white rounded-lg shadow-md p-6">
                <h2 class="text-xl font-bold mb-6">Script Generator Settings</h2>
                <form id="debateForm" class="space-y-6">
                    <!-- Debate Motion -->
                    <div>
                        <label class="block mb-2 font-medium" for="motion">Debate Motion</label>
                        <input type="text" id="motion" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-primary" placeholder="e.g., This House Supports Universal Basic Income" required>
                    </div>

                    <!-- Role Selection -->
                    <div>
                        <label class="block mb-2 font-medium" for="role">Speaker Role</label>
                        <select id="role" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-primary" required>
                            <option value="">Select Role</option>
                            <option value="captain_government">Captain (Government)</option>
                            <option value="captain_opposition">Captain (Opposition)</option>
                            <option value="prime_minister">Prime Minister</option>
                            <option value="opposition_leader">Opposition Leader</option>
                            <option value="whip_government">Whip (Government)</option>
                            <option value="whip_opposition">Whip (Opposition)</option>
                        </select>
                    </div>

                    <!-- Debate Format -->
                    <div>
                        <label class="block mb-2 font-medium" for="format">Debate Format</label>
                        <select id="format" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-primary" required>
                            <option value="british_parliamentary">British Parliamentary (BP)</option>
                            <option value="public_forum">Public Forum</option>
                            <option value="lincoln_douglas">Lincoln-Douglas</option>
                            <option value="world_schools">World Schools</option>
                        </select>
                    </div>

                    <!-- Speech Length -->
                    <div>
                        <label class="block mb-2 font-medium" for="length">Speech Length (Minutes)</label>
                        <input type="number" id="length" min="3" max="10" value="5" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-primary">
                    </div>

                    <!-- Additional Notes -->
                    <div>
                        <label class="block mb-2 font-medium" for="notes">Additional Notes (e.g., key points/stats to include)</label>
                        <textarea id="notes" rows="3" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-primary" placeholder="e.g., Include 2024 GDP stats for EU countries, focus on youth unemployment"></textarea>
                    </div>

                    <!-- Generate Button -->
                    <button type="submit" id="generateBtn" class="w-full py-3 rounded-lg bg-primary text-white font-bold hover:bg-primary/80 transition flex items-center justify-center gap-2">
                        <i class="fas fa-magic"></i> Generate Script
                    </button>
                </form>
            </div>

            <!-- Output Section (Right Column) -->
            <div class="md:col-span-8">
                <!-- Loading State (Hidden by Default) -->
                <div id="loadingState" class="hidden flex flex-col items-center justify-center h-64">
                    <div class="spinner border-4 border-primary/30 border-t-primary rounded-full w-16 h-16 animate-spin mb-4"></div>
                    <p class="text-lg font-medium">Generating your debate script... (AI is fetching up-to-date stats!)</p>
                </div>

                <!-- Script Output (Hidden by Default) -->
                <div id="scriptOutput" class="hidden bg-white rounded-lg shadow-md p-6">
                    <div class="flex justify-between items-center mb-6">
                        <h2 class="text-xl font-bold">Generated Debate Script</h2>
                        <div class="flex gap-3">
                            <button id="saveDraftBtn" class="px-4 py-2 rounded bg-secondary text-white hover:bg-secondary/80 transition">
                                <i class="fas fa-save mr-2"></i> Save Draft
                            </button>
                            <button id="exportPdfBtn" class="px-4 py-2 rounded bg-primary text-white hover:bg-primary/80 transition">
                                <i class="fas fa-file-pdf mr-2"></i> Export PDF
                            </button>
                            <button id="copyScriptBtn" class="px-4 py-2 rounded bg-dark text-white hover:bg-dark/80 transition">
                                <i class="fas fa-copy mr-2"></i> Copy
                            </button>
                        </div>
                    </div>
                    <div id="scriptContent" class="script-container prose max-w-none"></div>
                </div>

                <!-- Empty State (Default) -->
                <div id="emptyState" class="flex flex-col items-center justify-center h-64 text-center">
                    <i class="fas fa-file-alt text-6xl text-gray-400 mb-4"></i>
                    <h3 class="text-xl font-medium mb-2">No Script Generated Yet</h3>
                    <p class="text-gray-500">Fill out the form on the left and click "Generate Script" to create your AI-powered debate script with up-to-date statistics.</p>
                </div>
            </div>
        </div>
    </main>

    <!-- Footer -->
    <footer class="bg-primary text-white py-4 px-6 mt-8">
        <div class="container mx-auto text-center">
            <p>Free AI Debate Script Writer | Powered by GPT-4o | No Cost, No Ads</p>
            <p class="text-sm mt-2">For educational use only | Stats are updated to 2025</p>
        </div>
    </footer>

    <!-- JavaScript -->
    <script>
        // --------------------------
        // Global Variables
        // --------------------------
        const API_KEY = "YOUR_OPENAI_API_KEY"; // Replace with user's API key (instructions below)
        const API_URL = "https://api.openai.com/v1/chat/completions";

        // DOM Elements
        const debateForm = document.getElementById('debateForm');
        const loadingState = document.getElementById('loadingState');
        const emptyState = document.getElementById('emptyState');
        const scriptOutput = document.getElementById('scriptOutput');
        const scriptContent = document.getElementById('scriptContent');
        const themeToggle = document.getElementById('themeToggle');
        const colorSettingsBtn = document.getElementById('colorSettingsBtn');
        const colorSettingsModal = document.getElementById('colorSettingsModal');
        const closeColorModal = document.getElementById('closeColorModal');
        const primaryColorPicker = document.getElementById('primaryColorPicker');
        const secondaryColorPicker = document.getElementById('secondaryColorPicker');
        const lightModeBtn = document.getElementById('lightModeBtn');
        const darkModeBtn = document.getElementById('darkModeBtn');
        const saveColorSettings = document.getElementById('saveColorSettings');
        const saveDraftBtn = document.getElementById('saveDraftBtn');
        const exportPdfBtn = document.getElementById('exportPdfBtn');
        const copyScriptBtn = document.getElementById('copyScriptBtn');

        // --------------------------
        // Theme & Color Settings
        // --------------------------
        // Load saved color settings from LocalStorage
        function loadColorSettings() {
            const savedPrimary = localStorage.getItem('debatePrimaryColor') || '#3b82f6';
            const savedSecondary = localStorage.getItem('debateSecondaryColor') || '#10b981';
            const savedTheme = localStorage.getItem('debateTheme') || 'light';

            // Apply primary/secondary colors
            document.documentElement.style.setProperty('--tw-primary', savedPrimary);
            document.documentElement.style.setProperty('--tw-secondary', savedSecondary);
            primaryColorPicker.value = savedPrimary;
            secondaryColorPicker.value = savedSecondary;

            // Update Tailwind classes (dynamic CSS)
            document.querySelectorAll('.bg-primary').forEach(el => el.style.backgroundColor = savedPrimary);
            document.querySelectorAll('.text-primary').forEach(el => el.style.color = savedPrimary);
            document.querySelectorAll('.focus\\:ring-primary').forEach(el => el.style.boxShadow = `0 0 0 2px ${savedPrimary}33`);
            document.querySelectorAll('.bg-secondary').forEach(el => el.style.backgroundColor = savedSecondary);
            document.querySelectorAll('.text-secondary').forEach(el => el.style.color = savedSecondary);

            // Apply theme mode
            if (savedTheme === 'dark') {
                document.body.classList.remove('bg-light', 'text-dark');
                document.body.classList.add('bg-dark', 'text-white');
                themeToggle.innerHTML = '<i class="fas fa-sun"></i>';
            } else {
                document.body.classList.remove('bg-dark', 'text-white');
                document.body.classList.add('bg-light', 'text-dark');
                themeToggle.innerHTML = '<i class="fas fa-moon"></i>';
            }
        }

        // Save color settings to LocalStorage
        function saveColorSettingsToStorage() {
            const primary = primaryColorPicker.value;
            const secondary = secondaryColorPicker.value;
            const theme = document.body.classList.contains('bg-dark') ? 'dark' : 'light';

            localStorage.setItem('debatePrimaryColor', primary);
            localStorage.setItem('debateSecondaryColor', secondary);
            localStorage.setItem('debateTheme', theme);

            loadColorSettings();
            colorSettingsModal.classList.add('hidden');
            alert('Color settings saved!');
        }

        // Toggle light/dark theme
        themeToggle.addEventListener('click', () => {
            if (document.body.classList.contains('bg-dark')) {
                document.body.classList.remove('bg-dark', 'text-white');
                document.body.classList.add('bg-light', 'text-dark');
                themeToggle.innerHTML = '<i class="fas fa-moon"></i>';
            } else {
                document.body.classList.remove('bg-light', 'text-dark');
                document.body.classList.add('bg-dark', 'text-white');
                themeToggle.innerHTML = '<i class="fas fa-sun"></i>';
            }
            saveColorSettingsToStorage(); // Auto-save theme change
        });

        // Color modal toggle
        colorSettingsBtn.addEventListener('click', () => colorSettingsModal.classList.remove('hidden'));
        closeColorModal.addEventListener('click', () => colorSettingsModal.classList.add('hidden'));

        // Light/dark mode buttons in modal
        lightModeBtn.addEventListener('click', () => {
            document.body.classList.remove('bg-dark', 'text-white');
            document.body.classList.add('bg-light', 'text-dark');
            themeToggle.innerHTML = '<i class="fas fa-moon"></i>';
        });

        darkModeBtn.addEventListener('click', () => {
            document.body.classList.remove('bg-light', 'text-dark');
            document.body.classList.add('bg-dark', 'text-white');
            themeToggle.innerHTML = '<i class="fas fa-sun"></i>';
        });

        // Save color settings
        saveColorSettings.addEventListener('click', saveColorSettingsToStorage);

        // --------------------------
        // AI Script Generation
        // --------------------------
        debateForm.addEventListener('submit', async (e) => {
            e.preventDefault();

            // Get form values
            const motion = document.getElementById('motion').value;
            const role = document.getElementById('role').value;
            const format = document.getElementById('format').value;
            const length = document.getElementById('length').value;
            const notes = document.getElementById('notes').value;

            // Validate role selection
            if (!role) {
                alert('Please select a speaker role!');
                return;
            }

            // Show loading state
            emptyState.classList.add('hidden');
            scriptOutput.classList.add('hidden');
            loadingState.classList.remove('hidden');

            try {
                // Role mapping for AI prompt
                const roleMap = {
                    'captain_government': 'Captain of the Government team',
                    'captain_opposition': 'Captain of the Opposition team',
                    'prime_minister': 'Prime Minister (Government Opening)',
                    'opposition_leader': 'Leader of the Opposition',
                    'whip_government': 'Government Whip',
                    'whip_opposition': 'Opposition Whip'
                };

                // Format mapping
                const formatMap = {
                    'british_parliamentary': 'British Parliamentary (BP) format',
                    'public_forum': 'Public Forum debate format',
                    'lincoln_douglas': 'Lincoln-Douglas debate format',
                    'world_schools': 'World Schools debate format'
                };

                // AI Prompt (critical for high-quality output)
                const prompt = `
                    You are a professional debate coach with expertise in ${formatMap[format]}. 
                    Generate a ${length}-minute debate script for the ${roleMap[role]} on the motion: "${motion}".
                    Follow these strict guidelines:

                    1. Start with a clear DEFINITION of the motion (context, key terms, scope) – critical for debate framing.
                    2. Include a SPEAKER BREAKDOWN section that outlines what the speaker will cover (main arguments, rebuttals, structure).
                    3. Write the full speech script with:
                       - Up-to-date statistics (2024-2025 data only – no outdated stats)
                       - Logical, persuasive arguments tailored to the role
                       - Rebuttals to potential counterarguments (if applicable to the role)
                       - Clear structure (introduction, body, conclusion)
                       - Formal debate language (appropriate for ${formatMap[format]})
                    4. Ensure stats are specific (e.g., "World Bank 2024 data shows 67% of developing nations...") – no vague claims.
                    5. Tailor the tone to the role:
                       - Captain: Strategic, summarizes team arguments, strong closing
                       - Prime Minister: Establishes government position, sets debate agenda
                       - Opposition Leader: Challenges government, presents alternative framework
                       - Whip: Summarizes key clashes, reinforces team wins
                    6. Add a CONCLUSION section (for captain roles) that reinforces the team's position.
                    
                    Additional notes to include: ${notes || 'None'}.
                    
                    Format the output with clear headings (DEFINITION, SPEAKER BREAKDOWN, SPEECH SCRIPT, CONCLUSION) for readability.
                `;

                // Call OpenAI API
                const response = await fetch(API_URL, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization': `Bearer ${API_KEY}`
                    },
                    body: JSON.stringify({
                        model: 'gpt-4o',
                        messages: [{ role: 'user', content: prompt }],
                        temperature: 0.7, // Balance creativity and accuracy
                        max_tokens: 2000 // Adjust based on speech length
                    })
                });

                if (!response.ok) throw new Error('API Request Failed');

                const data = await response.json();
                const script = data.choices[0].message.content;

                // Update UI with script
                scriptContent.textContent = script;
                loadingState.classList.add('hidden');
                scriptOutput.classList.remove('hidden');

                // Save draft automatically
                saveDraft(script, motion, role);

            } catch (error) {
                loadingState.classList.add('hidden');
                emptyState.classList.remove('hidden');
                alert(`Error generating script: ${error.message}\nMake sure your OpenAI API key is valid!`);
                console.error('AI Error:', error);
            }
        });

        // --------------------------
        // Draft Saving/Export
        // --------------------------
        function saveDraft(script, motion, role) {
            const draft = {
                script,
                motion,
                role,
                timestamp: new Date().toLocaleString()
            };
            localStorage.setItem('debateDraft', JSON.stringify(draft));
        }

        function loadDraft() {
            const draft = JSON.parse(localStorage.getItem('debateDraft'));
            if (draft) {
                scriptContent.textContent = draft.script;
                document.getElementById('motion').value = draft.motion;
                document.getElementById('role').value = draft.role;
                emptyState.classList.add('hidden');
                scriptOutput.classList.remove('hidden');
            }
        }

        // Save draft button
        saveDraftBtn.addEventListener('click', () => {
            const script = scriptContent.textContent;
            const motion = document.getElementById('motion').value;
            const role = document.getElementById('role').value;
            if (script) {
                saveDraft(script, motion, role);
                alert('Draft saved successfully!');
            } else {
                alert('No script to save!');
            }
        });

        // Export to PDF
        exportPdfBtn.addEventListener('click', () => {
            const opt = {
                margin: 10,
                filename: `Debate_Script_${document.getElementById('motion').value.substring(0, 20)}.pdf`,
                image: { type: 'jpeg', quality: 0.98 },
                html2canvas: { scale: 2 },
                jsPDF: { unit: 'mm', format: 'a4', orientation: 'portrait' }
            };
            html2pdf().set(opt).from(scriptContent).save();
        });

        // Copy script to clipboard
        copyScriptBtn.addEventListener('click', async () => {
            try {
                await navigator.clipboard.writeText(scriptContent.textContent);
                alert('Script copied to clipboard!');
            } catch (error) {
                alert('Failed to copy script: ' + error.message);
            }
        });

        // --------------------------
        // Initialization
        // --------------------------
        document.addEventListener('DOMContentLoaded', () => {
            loadColorSettings();
            loadDraft();

            // Close color modal when clicking outside
            window.addEventListener('click', (e) => {
                if (e.target === colorSettingsModal) colorSettingsModal.classList.add('hidden');
            });
        });
    </script>
</body>
</html>

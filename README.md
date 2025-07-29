<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pocafy</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        .fade-in {
            animation: fadeIn 0.5s ease-in-out;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .bg-gradient-custom {
            background: linear-gradient(135deg, #6b21a8 0%, #3b82f6 100%);
        }
        .podcast-player {
            transition: all 0.3s ease;
        }
        .podcast-player.playing {
            box-shadow: 0 10px 25px -5px rgba(59, 130, 246, 0.25);
        }
        .progress-bar {
            appearance: none;
            height: 6px;
            border-radius: 3px;
            background: #e5e7eb;
            outline: none;
        }
        .progress-bar::-webkit-slider-thumb {
            appearance: none;
            width: 16px;
            height: 16px;
            border-radius: 50%;
            background: #3b82f6;
            cursor: pointer;
        }
        .volume-slider {
            appearance: none;
            height: 4px;
            border-radius: 2px;
            background: #e5e7eb;
            outline: none;
        }
        .volume-slider::-webkit-slider-thumb {
            appearance: none;
            width: 12px;
            height: 12px;
            border-radius: 50%;
            background: #3b82f6;
            cursor: pointer;
        }
        .podcast-item:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1);
        }
        .player-controls {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 20px;
        }
        .player-btn {
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 50%;
            background: #e0e7ff;
            color: #3b82f6;
            cursor: pointer;
            transition: all 0.2s;
        }
        .player-btn:hover {
            background: #3b82f6;
            color: white;
        }
        .player-btn.main {
            width: 50px;
            height: 50px;
            background: #3b82f6;
            color: white;
        }
        .player-btn.main:hover {
            background: #2563eb;
            transform: scale(1.05);
        }
        .podcast-image {
            width: 100%;
            height: 200px;
            background: linear-gradient(45deg, #6b21a8, #3b82f6);
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-bottom: 20px;
        }
        .podcast-image i {
            font-size: 4rem;
            color: rgba(255, 255, 255, 0.7);
        }
    </style>
</head>
<body class="bg-gray-100 min-h-screen flex items-center justify-center p-4">
    <div class="container mx-auto max-w-md">
        <!-- Initial Choice Screen -->
        <div id="choiceScreen" class="bg-white rounded-xl shadow-2xl overflow-hidden fade-in">
            <div class="bg-gradient-custom p-6 text-white text-center">
                <h1 class="text-3xl font-bold">Welcome to Pocafy</h1>
                <p class="mt-2 opacity-90">Are you an admin or listener?</p>
            </div>
            
            <div class="p-8">
                <div class="grid grid-cols-1 gap-6">
                    <button onclick="showAdminLogin()" class="flex items-center justify-center gap-3 bg-purple-600 hover:bg-purple-700 text-white font-semibold py-4 px-6 rounded-lg transition-all transform hover:scale-105">
                        <i class="fas fa-user-shield text-xl"></i>
                        <span>I'm an Admin</span>
                    </button>
                    
                    <button onclick="showVisitorOptions()" class="flex items-center justify-center gap-3 bg-blue-600 hover:bg-blue-700 text-white font-semibold py-4 px-6 rounded-lg transition-all transform hover:scale-105">
                        <i class="fas fa-headphones text-xl"></i>
                        <span>I'm a Listener</span>
                    </button>
                </div>
            </div>
        </div>
        
        <!-- Admin Login Screen -->
        <div id="adminLoginScreen" class="hidden bg-white rounded-xl shadow-2xl overflow-hidden">
            <div class="bg-gradient-custom p-6 text-white text-center">
                <h1 class="text-3xl font-bold">Admin Login</h1>
                <p class="mt-2 opacity-90">Enter your password to continue</p>
            </div>
            
            <div class="p-8">
                <form id="adminForm" class="space-y-6">
                    <div>
                        <label for="adminPassword" class="block text-sm font-medium text-gray-700 mb-1">Password</label>
                        <div class="relative">
                            <input type="password" id="adminPassword" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-600 focus:border-transparent" placeholder="Enter password" required>
                            <button type="button" onclick="togglePassword('adminPassword')" class="absolute right-3 top-3 text-gray-500 hover:text-gray-700">
                                <i class="far fa-eye"></i>
                            </button>
                        </div>
                    </div>
                    
                    <div class="flex items-center justify-between">
                        <button type="button" onclick="backToChoice()" class="text-purple-600 hover:text-purple-800 font-medium flex items-center">
                            <i class="fas fa-arrow-left mr-2"></i> Back
                        </button>
                        <button type="submit" class="bg-purple-600 hover:bg-purple-700 text-white font-semibold py-2 px-6 rounded-lg transition-all">
                            Login
                        </button>
                    </div>
                </form>
            </div>
        </div>
        
        <!-- Visitor Options Screen -->
        <div id="visitorOptionsScreen" class="hidden bg-white rounded-xl shadow-2xl overflow-hidden">
            <div class="bg-gradient-custom p-6 text-white text-center">
                <h1 class="text-3xl font-bold">Welcome Listener</h1>
                <p class="mt-2 opacity-90">Login or register to continue</p>
            </div>
            
            <div class="p-8">
                <div class="grid grid-cols-1 gap-6">
                    <button onclick="showVisitorLogin()" class="flex items-center justify-center gap-3 bg-blue-600 hover:bg-blue-700 text-white font-semibold py-4 px-6 rounded-lg transition-all">
                        <i class="fas fa-sign-in-alt text-xl"></i>
                        <span>Login</span>
                    </button>
                    
                    <button onclick="showVisitorRegister()" class="flex items-center justify-center gap-3 bg-green-600 hover:bg-green-700 text-white font-semibold py-4 px-6 rounded-lg transition-all">
                        <i class="fas fa-user-plus text-xl"></i>
                        <span>Register</span>
                    </button>
                    
                    <button onclick="backToChoice()" class="flex items-center justify-center gap-3 bg-gray-600 hover:bg-gray-700 text-white font-semibold py-4 px-6 rounded-lg transition-all">
                        <i class="fas fa-arrow-left text-xl"></i>
                        <span>Back</span>
                    </button>
                </div>
            </div>
        </div>
        
        <!-- Visitor Login Screen -->
        <div id="visitorLoginScreen" class="hidden bg-white rounded-xl shadow-2xl overflow-hidden">
            <div class="bg-gradient-custom p-6 text-white text-center">
                <h1 class="text-3xl font-bold">Listener Login</h1>
                <p class="mt-2 opacity-90">Enter your credentials</p>
            </div>
            
            <div class="p-8">
                <form id="visitorLoginForm" class="space-y-6">
                    <div>
                        <label for="loginEmail" class="block text-sm font-medium text-gray-700 mb-1">Email</label>
                        <input type="email" id="loginEmail" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-600 focus:border-transparent" placeholder="your@email.com" required>
                    </div>
                    
                    <div>
                        <label for="loginPassword" class="block text-sm font-medium text-gray-700 mb-1">Password</label>
                        <div class="relative">
                            <input type="password" id="loginPassword" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-600 focus:border-transparent" placeholder="Enter password" required>
                            <button type="button" onclick="togglePassword('loginPassword')" class="absolute right-3 top-3 text-gray-500 hover:text-gray-700">
                                <i class="far fa-eye"></i>
                            </button>
                        </div>
                    </div>
                    
                    <div class="flex items-center justify-between">
                        <button type="button" onclick="backToVisitorOptions()" class="text-blue-600 hover:text-blue-800 font-medium flex items-center">
                            <i class="fas fa-arrow-left mr-2"></i> Back
                        </button>
                        <button type="submit" class="bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-6 rounded-lg transition-all">
                            Login
                        </button>
                    </div>
                </form>
            </div>
        </div>
        
        <!-- Visitor Register Screen -->
        <div id="visitorRegisterScreen" class="hidden bg-white rounded-xl shadow-2xl overflow-hidden">
            <div class="bg-gradient-custom p-6 text-white text-center">
                <h1 class="text-3xl font-bold">Listener Registration</h1>
                <p class="mt-2 opacity-90">Create your account</p>
            </div>
            
            <div class="p-8">
                <form id="visitorRegisterForm" class="space-y-6">
                    <div>
                        <label for="registerName" class="block text-sm font-medium text-gray-700 mb-1">Full Name</label>
                        <input type="text" id="registerName" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-green-600 focus:border-transparent" placeholder="John Doe" required>
                    </div>
                    
                    <div>
                        <label for="registerEmail" class="block text-sm font-medium text-gray-700 mb-1">Email</label>
                        <input type="email" id="registerEmail" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-green-600 focus:border-transparent" placeholder="your@email.com" required>
                    </div>
                    
                    <div>
                        <label for="registerPassword" class="block text-sm font-medium text-gray-700 mb-1">Password</label>
                        <div class="relative">
                            <input type="password" id="registerPassword" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-green-600 focus:border-transparent" placeholder="Create password" required>
                            <button type="button" onclick="togglePassword('registerPassword')" class="absolute right-3 top-3 text-gray-500 hover:text-gray-700">
                                <i class="far fa-eye"></i>
                            </button>
                        </div>
                    </div>
                    
                    <div>
                        <label for="registerConfirmPassword" class="block text-sm font-medium text-gray-700 mb-1">Confirm Password</label>
                        <div class="relative">
                            <input type="password" id="registerConfirmPassword" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-green-600 focus:border-transparent" placeholder="Confirm password" required>
                            <button type="button" onclick="togglePassword('registerConfirmPassword')" class="absolute right-3 top-3 text-gray-500 hover:text-gray-700">
                                <i class="far fa-eye"></i>
                            </button>
                        </div>
                    </div>
                    
                    <div class="flex items-center justify-between">
                        <button type="button" onclick="backToVisitorOptions()" class="text-green-600 hover:text-green-800 font-medium flex items-center">
                            <i class="fas fa-arrow-left mr-2"></i> Back
                        </button>
                        <button type="submit" class="bg-green-600 hover:bg-green-700 text-white font-semibold py-2 px-6 rounded-lg transition-all">
                            Register
                        </button>
                    </div>
                </form>
            </div>
        </div>
        
        <!-- Admin Upload Screen -->
        <div id="adminUploadScreen" class="hidden bg-white rounded-xl shadow-2xl overflow-hidden">
            <div class="bg-gradient-custom p-6 text-white text-center">
                <h1 class="text-3xl font-bold">Upload Podcast</h1>
                <p class="mt-2 opacity-90">Share your content with listeners</p>
            </div>
            
            <div class="p-8">
                <form id="uploadForm" class="space-y-6">
                    <div>
                        <label for="podcastName" class="block text-sm font-medium text-gray-700 mb-1">Podcast Name</label>
                        <input type="text" id="podcastName" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-600 focus:border-transparent" placeholder="My Awesome Podcast" required>
                    </div>
                    
                    <div>
                        <label for="podcastFile" class="block text-sm font-medium text-gray-700 mb-1">Podcast File</label>
                        <input type="file" id="podcastFile" accept="audio/*" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-600 focus:border-transparent" required>
                    </div>
                    
                    <div>
                        <label for="podcastDescription" class="block text-sm font-medium text-gray-700 mb-1">Description</label>
                        <textarea id="podcastDescription" rows="3" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-600 focus:border-transparent" placeholder="What's this podcast about?"></textarea>
                    </div>
                    
                    <div class="flex items-center justify-between">
                        <button type="button" onclick="resetToInitial()" class="text-purple-600 hover:text-purple-800 font-medium flex items-center">
                            <i class="fas fa-arrow-left mr-2"></i> Back to Home
                        </button>
                        <button type="submit" class="bg-purple-600 hover:bg-purple-700 text-white font-semibold py-2 px-6 rounded-lg transition-all">
                            Upload Podcast
                        </button>
                    </div>
                </form>
            </div>
        </div>
        
        <!-- Visitor Podcast Screen -->
        <div id="visitorPodcastScreen" class="hidden bg-white rounded-xl shadow-2xl overflow-hidden">
            <div class="bg-gradient-custom p-6 text-white">
                <h1 class="text-3xl font-bold">Pocafy Hub</h1>
                <div class="mt-4 relative">
                    <input type="text" id="podcastSearch" class="w-full px-4 py-3 rounded-lg bg-white bg-opacity-20 placeholder-white focus:outline-none focus:ring-2 focus:ring-white" placeholder="Search podcasts...">
                    <i class="fas fa-search absolute right-3 top-3 text-white"></i>
                </div>
            </div>
            
            <div class="p-6">
                <div id="podcastList" class="space-y-4">
                    <!-- Podcasts will be dynamically added here -->
                    <div class="text-center py-8 text-gray-500">
                        <i class="fas fa-podcast text-4xl mb-2"></i>
                        <p>No podcasts available yet</p>
                    </div>
                </div>
            </div>
            
            <div class="p-6 border-t border-gray-200">
                <button onclick="resetToInitial()" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-3 px-6 rounded-lg transition-all">
                    <i class="fas fa-sign-out-alt mr-2"></i> Logout
                </button>
            </div>
        </div>
        
        <!-- Register Success Screen -->
        <div id="registerSuccessScreen" class="hidden bg-white rounded-xl shadow-2xl overflow-hidden text-center">
            <div class="bg-gradient-custom p-6 text-white">
                <h1 class="text-3xl font-bold">Registration Complete!</h1>
            </div>
            <div class="p-8">
                <div class="text-green-500 text-6xl mb-6">
                    <i class="fas fa-check-circle"></i>
                </div>
                <h2 class="text-2xl font-semibold text-gray-800">Account Created</h2>
                <p class="mt-2 text-gray-600">You can now login to access podcasts.</p>
                <button onclick="resetToInitial()" class="mt-6 bg-green-600 hover:bg-green-700 text-white font-semibold py-2 px-6 rounded-lg transition-all">
                    Back to Home
                </button>
            </div>
        </div>
        
        <!-- Podcast Player Modal -->
        <div id="podcastPlayerModal" class="hidden fixed inset-0 bg-black bg-opacity-75 flex items-center justify-center z-50 p-4">
            <div class="bg-white rounded-xl w-full max-w-md">
                <div class="bg-gradient-custom p-6 text-white rounded-t-xl">
                    <div class="flex justify-between items-center">
                        <h2 id="playerPodcastName" class="text-2xl font-bold">Podcast Title</h2>
                        <button onclick="closePlayer()" class="text-white text-2xl">
                            <i class="fas fa-times"></i>
                        </button>
                    </div>
                </div>
                
                <div class="p-6">
                    <div class="podcast-image">
                        <i class="fas fa-podcast"></i>
                    </div>
                    
                    <div class="mb-2">
                        <p id="playerPodcastDescription" class="text-gray-600 text-center">Podcast description will appear here</p>
                    </div>
                    
                    <div class="mb-6">
                        <input type="range" id="progressBar" class="progress-bar w-full" value="0" min="0", max="100">
                        <div class="flex justify-between text-sm text-gray-500 mt-1">
                            <span id="currentTime">0:00</span>
                            <span id="totalTime">0:00</span>
                        </div>
                    </div>
                    
                    <div class="player-controls mb-6">
                        <button id="rewindBtn" class="player-btn">
                            <i class="fas fa-backward"></i>
                        </button>
                        
                        <button id="playPauseBtn" class="player-btn main">
                            <i class="fas fa-play"></i>
                        </button>
                        
                        <button id="forwardBtn" class="player-btn">
                            <i class="fas fa-forward"></i>
                        </button>
                    </div>
                    
                    <div class="flex items-center justify-center gap-3">
                        <i class="fas fa-volume-down text-gray-500"></i>
                        <input type="range" id="volumeControl" class="volume-slider w-32" value="80" min="0", max="100">
                        <i class="fas fa-volume-up text-gray-500"></i>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Initialize data from localStorage or create default data
        function initializeData() {
            // Get users from localStorage or create default users
            let storedUsers = localStorage.getItem('pocafyUsers');
            if (storedUsers) {
                users = JSON.parse(storedUsers);
            } else {
                users = [
                    { email: "user@example.com", password: "password123", name: "John Doe" },
                    { email: "aaron@example.com", password: "Aaron0816", name: "Aaron" }
                ];
                localStorage.setItem('pocafyUsers', JSON.stringify(users));
            }
            
            // Get podcasts from localStorage or create empty array
            let storedPodcasts = localStorage.getItem('pocafyPodcasts');
            if (storedPodcasts) {
                podcasts = JSON.parse(storedPodcasts);
            } else {
                podcasts = [];
                localStorage.setItem('pocafyPodcasts', JSON.stringify(podcasts));
            }
            
            // Check if user is already logged in
            checkRememberedLogin();
        }
        
        // Check if user is remembered from previous session
        function checkRememberedLogin() {
            const rememberedEmail = localStorage.getItem('rememberedEmail');
            if (rememberedEmail) {
                document.getElementById('loginEmail').value = rememberedEmail;
                document.getElementById('visitorOptionsScreen').classList.add('hidden');
                document.getElementById('visitorLoginScreen').classList.remove('hidden');
                document.getElementById('visitorLoginScreen').classList.add('fade-in');
            }
        }
        
        // Store registered users
        let users = [];
        
        // Store uploaded podcasts
        let podcasts = [];
        
        // Audio player variables
        let audio = new Audio();
        let currentPodcast = null;
        let isPlaying = false;
        let progressInterval;
        
        // Initialize data when page loads
        document.addEventListener('DOMContentLoaded', initializeData);
        
        // Toggle password visibility
        function togglePassword(id) {
            const input = document.getElementById(id);
            const icon = input.nextElementSibling.querySelector('i');
            if (input.type === 'password') {
                input.type = 'text';
                icon.classList.remove('fa-eye');
                icon.classList.add('fa-eye-slash');
            } else {
                input.type = 'password';
                icon.classList.remove('fa-eye-slash');
                icon.classList.add('fa-eye');
            }
        }
        
        // Navigation functions
        function showAdminLogin() {
            document.getElementById('choiceScreen').classList.add('hidden');
            document.getElementById('adminLoginScreen').classList.remove('hidden');
            document.getElementById('adminLoginScreen').classList.add('fade-in');
        }
        
        function showVisitorOptions() {
            document.getElementById('choiceScreen').classList.add('hidden');
            document.getElementById('visitorOptionsScreen').classList.remove('hidden');
            document.getElementById('visitorOptionsScreen').classList.add('fade-in');
        }
        
        function showVisitorLogin() {
            document.getElementById('visitorOptionsScreen').classList.add('hidden');
            document.getElementById('visitorLoginScreen').classList.remove('hidden');
            document.getElementById('visitorLoginScreen').classList.add('fade-in');
        }
        
        function showVisitorRegister() {
            document.getElementById('visitorOptionsScreen').classList.add('hidden');
            document.getElementById('visitorRegisterScreen').classList.remove('hidden');
            document.getElementById('visitorRegisterScreen').classList.add('fade-in');
        }
        
        function backToChoice() {
            document.getElementById('adminLoginScreen').classList.add('hidden');
            document.getElementById('visitorOptionsScreen').classList.add('hidden');
            document.getElementById('visitorLoginScreen').classList.add('hidden');
            document.getElementById('visitorRegisterScreen').classList.add('hidden');
            document.getElementById('choiceScreen').classList.remove('hidden');
            document.getElementById('choiceScreen').classList.add('fade-in');
        }
        
        function backToVisitorOptions() {
            document.getElementById('visitorLoginScreen').classList.add('hidden');
            document.getElementById('visitorRegisterScreen').classList.add('hidden');
            document.getElementById('visitorOptionsScreen').classList.remove('hidden');
            document.getElementById('visitorOptionsScreen').classList.add('fade-in');
        }
        
        function resetToInitial() {
            document.getElementById('adminUploadScreen').classList.add('hidden');
            document.getElementById('visitorPodcastScreen').classList.add('hidden');
            document.getElementById('registerSuccessScreen').classList.add('hidden');
            document.getElementById('choiceScreen').classList.remove('hidden');
            document.getElementById('choiceScreen').classList.add('fade-in');
            
            // Reset forms
            document.getElementById('adminForm').reset();
            document.getElementById('visitorLoginForm').reset();
            document.getElementById('visitorRegisterForm').reset();
            
            // Close player if open
            closePlayer();
            
            // Clear remembered login
            localStorage.removeItem('rememberedEmail');
        }
        
        // Form submissions
        document.getElementById('adminForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const password = document.getElementById('adminPassword').value;
            
            if (password === 'Aaron0816') {
                document.getElementById('adminLoginScreen').classList.add('hidden');
                document.getElementById('adminUploadScreen').classList.remove('hidden');
                document.getElementById('adminUploadScreen').classList.add('fade-in');
            } else {
                alert('Incorrect password. Please try again.');
            }
        });
        
        document.getElementById('visitorLoginForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const email = document.getElementById('loginEmail').value;
            const password = document.getElementById('loginPassword').value;
            
            // Check if user exists
            const user = users.find(u => u.email === email && u.password === password);
            
            if (user) {
                // Remember the user's email for next time
                localStorage.setItem('rememberedEmail', email);
                
                document.getElementById('visitorLoginScreen').classList.add('hidden');
                showPodcastScreen();
            } else {
                alert('Invalid email or password. Please try again or register a new account.');
            }
        });
        
        document.getElementById('visitorRegisterForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const name = document.getElementById('registerName').value;
            const email = document.getElementById('registerEmail').value;
            const password = document.getElementById('registerPassword').value;
            const confirmPassword = document.getElementById('registerConfirmPassword').value;
            
            if (!name || !email || !password || !confirmPassword) {
                alert('Please fill in all fields.');
                return;
            }
            
            if (password !== confirmPassword) {
                alert('Passwords do not match.');
                return;
            }
            
            // Check if email already exists
            if (users.some(u => u.email === email)) {
                alert('An account with this email already exists. Please login instead.');
                return;
            }
            
            // Add new user
            users.push({ email, password, name });
            localStorage.setItem('pocafyUsers', JSON.stringify(users));
            
            // Remember the user's email for next time
            localStorage.setItem('rememberedEmail', email);
            
            document.getElementById('visitorRegisterScreen').classList.add('hidden');
            document.getElementById('registerSuccessScreen').classList.remove('hidden');
            document.getElementById('registerSuccessScreen').classList.add('fade-in');
        });
        
        // Show podcast screen with sample data
        function showPodcastScreen() {
            document.getElementById('visitorPodcastScreen').classList.remove('hidden');
            document.getElementById('visitorPodcastScreen').classList.add('fade-in');
            renderPodcastList(podcasts);
        }
        
        // Render podcast list
        function renderPodcastList(podcastArray) {
            const podcastList = document.getElementById('podcastList');
            
            if (podcastArray.length === 0) {
                podcastList.innerHTML = `
                    <div class="text-center py-8 text-gray-500">
                        <i class="fas fa-podcast text-4xl mb-2"></i>
                        <p>No podcasts available yet</p>
                    </div>
                `;
                return;
            }
            
            podcastList.innerHTML = '';
            
            podcastArray.forEach(podcast => {
                const podcastElement = document.createElement('div');
                podcastElement.className = 'podcast-item bg-gray-50 rounded-lg p-4 border border-gray-200 cursor-pointer transition-all';
                podcastElement.innerHTML = `
                    <div class="flex items-center">
                        <div class="bg-gray-200 border-2 border-dashed rounded-xl w-16 h-16 flex items-center justify-center mr-4">
                            <i class="fas fa-podcast text-2xl text-gray-500"></i>
                        </div>
                        <div class="flex-1">
                            <h3 class="font-semibold text-gray-800">${podcast.name}</h3>
                            <p class="text-sm text-gray-600 mt-1">${podcast.description}</p>
                            <div class="flex items-center mt-2 text-sm text-gray-500">
                                <i class="far fa-clock mr-1"></i>
                                <span>${podcast.duration}</span>
                            </div>
                        </div>
                        <div>
                            <button class="text-purple-600 hover:text-purple-800">
                                <i class="fas fa-play-circle text-2xl"></i>
                            </button>
                        </div>
                    </div>
                `;
                
                podcastElement.addEventListener('click', () => openPlayer(podcast));
                podcastList.appendChild(podcastElement);
            });
        }
        
        // Search functionality
        document.getElementById('podcastSearch').addEventListener('input', function(e) {
            const searchTerm = e.target.value.toLowerCase();
            const filteredPodcasts = podcasts.filter(podcast => 
                podcast.name.toLowerCase().includes(searchTerm) ||
                podcast.description.toLowerCase().includes(searchTerm)
            );
            renderPodcastList(filteredPodcasts);
        });
        
        // Podcast player functions
        function openPlayer(podcast) {
            currentPodcast = podcast;
            document.getElementById('playerPodcastName').textContent = podcast.name;
            document.getElementById('playerPodcastDescription').textContent = podcast.description;
            document.getElementById('podcastPlayerModal').classList.remove('hidden');
            
            // Reset player UI
            document.getElementById('playPauseBtn').innerHTML = '<i class="fas fa-play"></i>';
            document.getElementById('progressBar').value = 0;
            document.getElementById('currentTime').textContent = '0:00';
            document.getElementById('totalTime').textContent = podcast.duration;
            
            // Set up audio
            audio.src = podcast.url;
            audio.volume = document.getElementById('volumeControl').value / 100;
            
            isPlaying = false;
        }
        
        function closePlayer() {
            document.getElementById('podcastPlayerModal').classList.add('hidden');
            if (isPlaying) {
                audio.pause();
                isPlaying = false;
                clearInterval(progressInterval);
            }
        }
        
        // Player controls
        document.getElementById('playPauseBtn').addEventListener('click', function() {
            const icon = this.querySelector('i');
            
            if (isPlaying) {
                icon.classList.remove('fa-pause');
                icon.classList.add('fa-play');
                audio.pause();
                clearInterval(progressInterval);
            } else {
                icon.classList.remove('fa-play');
                icon.classList.add('fa-pause');
                audio.play();
                startProgressTracking();
            }
            
            isPlaying = !isPlaying;
        });
        
        document.getElementById('rewindBtn').addEventListener('click', function() {
            audio.currentTime = Math.max(0, audio.currentTime - 15);
        });
        
        document.getElementById('forwardBtn').addEventListener('click', function() {
            audio.currentTime = Math.min(audio.duration, audio.currentTime + 15);
        });
        
        // Volume control
        document.getElementById('volumeControl').addEventListener('input', function() {
            audio.volume = this.value / 100;
        });
        
        // Progress tracking
        function startProgressTracking() {
            clearInterval(progressInterval);
            progressInterval = setInterval(() => {
                if (audio.duration) {
                    const progress = (audio.currentTime / audio.duration) * 100;
                    document.getElementById('progressBar').value = progress;
                    
                    // Update time display
                    const currentMinutes = Math.floor(audio.currentTime / 60);
                    const currentSeconds = Math.floor(audio.currentTime % 60);
                    document.getElementById('currentTime').textContent = 
                        `${currentMinutes}:${currentSeconds < 10 ? '0' : ''}${currentSeconds}`;
                    
                    // Update total time if not set
                    if (document.getElementById('totalTime').textContent === '0:00') {
                        const totalMinutes = Math.floor(audio.duration / 60);
                        const totalSeconds = Math.floor(audio.duration % 60);
                        document.getElementById('totalTime').textContent = 
                            `${totalMinutes}:${totalSeconds < 10 ? '0' : ''}${totalSeconds}`;
                        
                        // Update podcast duration in storage
                        if (currentPodcast) {
                            currentPodcast.duration = `${totalMinutes}:${totalSeconds < 10 ? '0' : ''}${totalSeconds}`;
                            updatePodcastInStorage(currentPodcast);
                        }
                    }
                }
            }, 1000);
        }
        
        // Update podcast in localStorage
        function updatePodcastInStorage(updatedPodcast) {
            const index = podcasts.findIndex(p => p.id === updatedPodcast.id);
            if (index !== -1) {
                podcasts[index] = updatedPodcast;
                localStorage.setItem('pocafyPodcasts', JSON.stringify(podcasts));
            }
        }
        
        // Progress bar click to seek
        document.getElementById('progressBar').addEventListener('input', function() {
            if (audio.duration) {
                const newTime = (this.value / 100) * audio.duration;
                audio.currentTime = newTime;
            }
        });
        
        // Upload form submission
        document.getElementById('uploadForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const name = document.getElementById('podcastName').value;
            const file = document.getElementById('podcastFile').files[0];
            const description = document.getElementById('podcastDescription').value;
            
            if (!name || !file) {
                alert('Please provide both podcast name and file.');
                return;
            }
            
            // Create URL for the file
            const url = URL.createObjectURL(file);
            
            // Create new podcast object
            const newPodcast = {
                id: Date.now(), // Use timestamp as unique ID
                name: name,
                description: description || "No description provided",
                duration: "00:00", // Will be updated when played
                url: url,
                fileName: file.name,
                uploadDate: new Date().toISOString()
            };
            
            // Add to podcasts array
            podcasts.push(newPodcast);
            localStorage.setItem('pocafyPodcasts', JSON.stringify(podcasts));
            
            // Show success message
            alert(`Podcast "${name}" uploaded successfully!`);
            
            // Reset form
            this.reset();
        });
    </script>
</body>
</html>

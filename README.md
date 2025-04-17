<!DOCTYPE html>
<html lang="en" dir="ltr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>تسجيل الحضور</title>
<script src="https://cdn.tailwindcss.com/3.4.16"></script>
<script>tailwind.config={theme:{extend:{colors:{primary:'#4CAF50',secondary:'#2196F3'},borderRadius:{'none':'0px','sm':'4px',DEFAULT:'8px','md':'12px','lg':'16px','xl':'20px','2xl':'24px','3xl':'32px','full':'9999px','button':'8px'}}}}</script>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Pacifico&display=swap" rel="stylesheet">
<link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/remixicon/4.6.0/remixicon.min.css">
<style>
:where([class^="ri-"])::before { content: "\f3c2"; }
body {
font-family: 'Tajawal', sans-serif;
}
.camera-container {
position: relative;
overflow: hidden;
}
#video {
transform: scaleX(-1);
}
#canvas {
display: none;
}
.face-guide {
position: absolute;
top: 50%;
left: 50%;
transform: translate(-50%, -50%);
width: 200px;
height: 200px;
border-radius: 50%;
border: 2px dashed rgba(255, 255, 255, 0.8);
pointer-events: none;
}
.camera-overlay {
position: absolute;
top: 0;
left: 0;
right: 0;
bottom: 0;
background: rgba(0, 0, 0, 0.5);
border-radius: 16px;
display: flex;
align-items: center;
justify-content: center;
color: white;
}
</style>
</head>
<body class="bg-gray-50 text-gray-800">
<!-- Nav Bar -->
<nav class="fixed w-full top-0 bg-primary text-white shadow-md z-10">
<div class="flex flex-col w-full">
<div class="w-full bg-[#3d8c40] py-1 text-center text-sm">
Health Center
</div>
<div class="flex items-center justify-between px-4 py-3">
<button class="w-8 h-8 flex items-center justify-center cursor-pointer" id="settingsBtn">
<i class="ri-settings-3-line ri-lg"></i>
</button>
<h1 class="text-lg font-bold">Check In</h1>
<div class="font-['Pacifico'] text-xl">logo</div>
</div>
</div>
</nav>
<!-- Main Content -->
<main class="pt-24 pb-20 px-4 min-h-screen">
<div class="mt-6 flex flex-col items-center">
<!-- Date and Time Display -->
<div class="mb-6 text-center">
<div class="text-3xl font-bold" id="timeDisplay">00:00:00</div>
<div class="text-lg text-gray-600" id="dateDisplay">Thursday, April 17, 2025</div>
</div>
<!-- Camera Section -->
<div class="w-full max-w-sm mb-6">
<div class="camera-container bg-gray-200 rounded-lg shadow-md aspect-square mb-4">
<div id="cameraPlaceholder" class="w-full h-full flex flex-col items-center justify-center text-gray-500 bg-gray-100 rounded-lg">
<div class="w-16 h-16 flex items-center justify-center bg-gray-200 rounded-full mb-2">
<i class="ri-camera-line ri-2x"></i>
</div>
<p>Click the camera button to start recording</p>
</div>
<video id="video" class="w-full h-full object-cover rounded-lg hidden"></video>
<canvas id="canvas"></canvas>
<div class="face-guide hidden"></div>
<div id="capturedImageContainer" class="w-full h-full hidden">
<img id="capturedImage" class="w-full h-full object-cover rounded-lg" />
</div>
</div>
<!-- Camera Controls -->
<div class="flex justify-center gap-4 mb-6">
<button id="cameraBtn" class="bg-primary text-white py-3 px-6 rounded-button shadow-md flex items-center justify-center cursor-pointer">
<i class="ri-camera-line ri-lg mr-2"></i>
<span>Enable Camera</span>
</button>
<button id="captureBtn" class="bg-green-600 text-white py-3 px-6 rounded-button shadow-md hidden flex items-center justify-center cursor-pointer">
<i class="ri-camera-line ri-lg mr-2"></i>
<span>Take Photo</span>
</button>
<button id="retryBtn" class="bg-orange-500 text-white py-3 px-6 rounded-button shadow-md hidden flex items-center justify-center cursor-pointer">
<i class="ri-refresh-line ri-lg mr-2"></i>
<span>Retry</span>
</button>
<button id="confirmBtn" class="bg-blue-600 text-white py-3 px-6 rounded-button shadow-md hidden flex items-center justify-center cursor-pointer">
<i class="ri-check-line ri-lg mr-2"></i>
<span>Confirm</span>
</button>
</div>
</div>
<!-- Location Section -->
<div class="w-full max-w-sm bg-white rounded-lg shadow-md p-4 mb-6">
<div class="flex items-center mb-2">
<div class="w-8 h-8 flex items-center justify-center text-primary">
<i class="ri-map-pin-line ri-lg"></i>
</div>
<h3 class="text-lg font-bold ml-2">Current Location</h3>
</div>
<div id="locationInfo" class="text-gray-600 text-sm">
Getting location...
</div>
<div class="mt-3 h-32 rounded overflow-hidden bg-gray-100">
<div class="w-full h-full bg-cover bg-center" style="background-image: url('https://public.readdy.ai/gen_page/map_placeholder_1280x720.png')"></div>
</div>
</div>
<!-- Recent Activity -->
<div class="w-full max-w-sm">
<h3 class="text-lg font-bold mb-3">Recent Check-ins</h3>
<div class="bg-white rounded-lg shadow-md overflow-hidden">
<div class="p-3 border-b border-gray-100 flex items-center">
<div class="w-10 h-10 bg-gray-200 rounded-full overflow-hidden mr-3">
<img src="https://readdy.ai/api/search-image?query=professional%20Arabic%20man%20face%20portrait%2C%20business%20attire%2C%20neutral%20expression%2C%20high%20quality&width=100&height=100&seq=1&orientation=squarish" class="w-full h-full object-cover" alt="صورة الموظف">
</div>
<div class="flex-1">
<div class="font-medium">Ahmed Mahmoud</div>
<div class="text-xs text-gray-500">Check-in - April 17, 08:30 AM</div>
</div>
<div class="w-8 h-8 flex items-center justify-center text-green-500">
<i class="ri-check-line ri-lg"></i>
</div>
</div>
<div class="p-3 border-b border-gray-100 flex items-center">
<div class="w-10 h-10 bg-gray-200 rounded-full overflow-hidden mr-3">
<img src="https://readdy.ai/api/search-image?query=professional%20Arabic%20woman%20face%20portrait%2C%20business%20attire%2C%20neutral%20expression%2C%20high%20quality&width=100&height=100&seq=2&orientation=squarish" class="w-full h-full object-cover" alt="صورة الموظف">
</div>
<div class="flex-1">
<div class="font-medium">Sarah Abdullah</div>
<div class="text-xs text-gray-500">Check-in - April 17, 08:15 AM</div>
</div>
<div class="w-8 h-8 flex items-center justify-center text-green-500">
<i class="ri-check-line ri-lg"></i>
</div>
</div>
<div class="p-3 flex items-center">
<div class="w-10 h-10 bg-gray-200 rounded-full overflow-hidden mr-3">
<img src="https://readdy.ai/api/search-image?query=professional%20Arabic%20man%20face%20portrait%2C%20business%20attire%2C%20neutral%20expression%2C%20high%20quality&width=100&height=100&seq=3&orientation=squarish" class="w-full h-full object-cover" alt="صورة الموظف">
</div>
<div class="flex-1">
<div class="font-medium">Khalid Abdulrahman</div>
<div class="text-xs text-gray-500">Check-in - April 17, 08:05 AM</div>
</div>
<div class="w-8 h-8 flex items-center justify-center text-green-500">
<i class="ri-check-line ri-lg"></i>
</div>
</div>
</div>
</div>
</div>
</main>
<!-- Tab Bar -->
<div class="fixed bottom-0 w-full bg-white shadow-[0_-2px_10px_rgba(0,0,0,0.1)] z-10">
<div class="grid grid-cols-4 h-16">
<button class="flex flex-col items-center justify-center cursor-pointer text-primary">
<div class="w-6 h-6 flex items-center justify-center">
<i class="ri-home-line ri-lg"></i>
</div>
<span class="text-xs mt-1">Home</span>
</button>
<button class="flex flex-col items-center justify-center cursor-pointer text-gray-500">
<div class="w-6 h-6 flex items-center justify-center">
<i class="ri-team-line ri-lg"></i>
</div>
<span class="text-xs mt-1">Staff</span>
</button>
<button class="flex flex-col items-center justify-center cursor-pointer text-gray-500">
<div class="w-6 h-6 flex items-center justify-center">
<i class="ri-file-list-line ri-lg"></i>
</div>
<span class="text-xs mt-1">Reports</span>
</button>
<button class="flex flex-col items-center justify-center cursor-pointer text-gray-500">
<div class="w-6 h-6 flex items-center justify-center">
<i class="ri-user-line ri-lg"></i>
</div>
<span class="text-xs mt-1">Profile</span>
</button>
</div>
</div>
<!-- Toast Notification -->
<div id="toast" class="fixed top-16 left-0 right-0 mx-auto w-4/5 max-w-sm bg-green-500 text-white py-3 px-4 rounded-lg shadow-lg flex items-center justify-between transform -translate-y-20 transition-transform duration-300">
<div class="flex items-center">
<i class="ri-check-line ri-lg mr-2"></i>
<span id="toastMessage">Check-in successful!</span>
</div>
<button id="closeToast" class="w-6 h-6 flex items-center justify-center cursor-pointer">
<i class="ri-close-line"></i>
</button>
</div>
<script>
document.addEventListener('DOMContentLoaded', function() {
// Elements
const video = document.getElementById('video');
const canvas = document.getElementById('canvas');
const cameraBtn = document.getElementById('cameraBtn');
const captureBtn = document.getElementById('captureBtn');
const retryBtn = document.getElementById('retryBtn');
const confirmBtn = document.getElementById('confirmBtn');
const cameraPlaceholder = document.getElementById('cameraPlaceholder');
const capturedImageContainer = document.getElementById('capturedImageContainer');
const capturedImage = document.getElementById('capturedImage');
const faceGuide = document.querySelector('.face-guide');
const timeDisplay = document.getElementById('timeDisplay');
const dateDisplay = document.getElementById('dateDisplay');
const locationInfo = document.getElementById('locationInfo');
const toast = document.getElementById('toast');
const closeToast = document.getElementById('closeToast');
const toastMessage = document.getElementById('toastMessage');
let stream = null;
let locationData = null;
// Update time and date
function updateDateTime() {
const now = new Date();
const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
// Format time as HH:MM:SS
const hours = now.getHours().toString().padStart(2, '0');
const minutes = now.getMinutes().toString().padStart(2, '0');
const seconds = now.getSeconds().toString().padStart(2, '0');
timeDisplay.textContent = `${hours}:${minutes}:${seconds}`;
dateDisplay.textContent = now.toLocaleDateString('en-US', options);
}
// Initial update
updateDateTime();
// Update time every second
setInterval(updateDateTime, 1000);
// Get location
function getLocation() {
if (navigator.geolocation) {
navigator.geolocation.getCurrentPosition(
(position) => {
locationData = {
latitude: position.coords.latitude,
longitude: position.coords.longitude
};
// Reverse geocoding would normally go here
// For demo purposes, we'll just display the coordinates
locationInfo.innerHTML = `
<p>خط العرض: ${position.coords.latitude.toFixed(6)}</p>
<p>خط الطول: ${position.coords.longitude.toFixed(6)}</p>
<p>الدقة: ${position.coords.accuracy.toFixed(2)} متر</p>
`;
},
(error) => {
console.error("Error getting location:", error);
locationInfo.textContent = "Unable to access location. Please enable location services.";
showToast("Unable to access location. Please enable location services.", "orange");
}
);
} else {
locationInfo.textContent = "Your browser doesn't support geolocation.";
}
}
// Initialize location
getLocation();
// Camera functions
cameraBtn.addEventListener('click', async () => {
try {
stream = await navigator.mediaDevices.getUserMedia({
video: {
facingMode: "user",
width: { ideal: 1280 },
height: { ideal: 720 }
}
});
video.srcObject = stream;
video.play();
cameraPlaceholder.classList.add('hidden');
video.classList.remove('hidden');
faceGuide.classList.remove('hidden');
cameraBtn.classList.add('hidden');
captureBtn.classList.remove('hidden');
} catch (err) {
console.error("Error accessing camera:", err);
showToast("Unable to access camera. Please check permissions.", "red");
}
});
captureBtn.addEventListener('click', () => {
canvas.width = video.videoWidth;
canvas.height = video.videoHeight;
canvas.getContext('2d').drawImage(video, 0, 0);
// Get the captured image data
const imageData = canvas.toDataURL('image/png');
capturedImage.src = imageData;
// Hide video, show captured image
video.classList.add('hidden');
faceGuide.classList.add('hidden');
capturedImageContainer.classList.remove('hidden');
// Update buttons
captureBtn.classList.add('hidden');
retryBtn.classList.remove('hidden');
confirmBtn.classList.remove('hidden');
});
retryBtn.addEventListener('click', () => {
// Show video again
video.classList.remove('hidden');
faceGuide.classList.remove('hidden');
capturedImageContainer.classList.add('hidden');
// Update buttons
captureBtn.classList.remove('hidden');
retryBtn.classList.add('hidden');
confirmBtn.classList.add('hidden');
});
confirmBtn.addEventListener('click', () => {
// Stop camera stream
if (stream) {
stream.getTracks().forEach(track => track.stop());
}
// Reset UI
video.classList.add('hidden');
capturedImageContainer.classList.add('hidden');
cameraPlaceholder.classList.remove('hidden');
retryBtn.classList.add('hidden');
confirmBtn.classList.add('hidden');
cameraBtn.classList.remove('hidden');
// Show success message
showToast("Check-in successful!", "green");
// In a real app, here you would send the image, timestamp, and location to your server
console.log("Attendance recorded:", {
timestamp: new Date().toISOString(),
location: locationData,
// image would be sent as a file or base64 string
});
});
// Toast functions
function showToast(message, color = "green") {
toastMessage.textContent = message;
// Set color based on message type
toast.className = toast.className.replace(/bg-\w+-500/g, '');
toast.classList.add(`bg-${color}-500`);
// Show toast
toast.style.transform = 'translateY(0)';
// Auto hide after 5 seconds
setTimeout(() => {
hideToast();
}, 5000);
}
function hideToast() {
toast.style.transform = 'translateY(-20rem)';
}
closeToast.addEventListener('click', hideToast);
});
document.addEventListener('DOMContentLoaded', function() {
// Settings button functionality
const settingsBtn = document.getElementById('settingsBtn');
settingsBtn.addEventListener('click', () => {
// In a real app, this would open settings modal
console.log("Settings button clicked");
});
});
</script>
</body>
</html>
# -.

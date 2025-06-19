<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tra cứu vị trí nâng cao</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>
    <style>
        #map { height: 350px; border-radius: 8px; }
        .marker-pulse {
            width: 15px;
            height: 15px;
            border-radius: 50%;
            background: #ff0000;
            box-shadow: 0 0 0 rgba(255, 0, 0, 1);
            animation: pulse 1.5s infinite;
        }
        @keyframes pulse {
            0% { transform: scale(0.9); box-shadow: 0 0 0 0 rgba(255, 0, 0, 0.7); }
            70% { transform: scale(1.1); box-shadow: 0 0 0 10px rgba(255, 0, 0, 0); }
            100% { transform: scale(0.9); box-shadow: 0 0 0 0 rgba(255, 0, 0, 0); }
        }
        .animate-spin {
            animation: spin 1s linear infinite;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
    </style>
</head>
<body class="bg-gray-50 min-h-screen flex items-center justify-center p-4">
    <div class="w-full max-w-lg bg-white rounded-xl shadow-lg overflow-hidden">
        <!-- Header -->
        <div class="bg-gradient-to-r from-blue-700 to-blue-800 p-5 text-white">
            <h1 class="text-2xl font-bold flex items-center">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z" />
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z" />
                </svg>
                Tra cứu vị trí nâng cao
            </h1>
            <p class="text-blue-200 mt-1">Kết hợp đa phương pháp xác thực</p>
        </div>

        <!-- Main Content -->
        <div class="p-6">
            <!-- Phần nhập liệu -->
            <div class="mb-6">
                <label class="block text-gray-700 font-medium mb-2">Nhập URL hồ sơ MXH:</label>
                <div class="flex rounded-lg shadow-sm">
                    <select id="platformSelect" class="bg-gray-100 border border-r-0 border-gray-300 rounded-l-lg px-3 focus:outline-none">
                        <option value="facebook">Facebook</option>
                        <option value="instagram">Instagram</option>
                        <option value="twitter">Twitter</option>
                        <option value="tiktok">TikTok</option>
                    </select>
                    <input type="url" id="profileUrl" placeholder="https://facebook.com/username" 
                           class="flex-1 border-t border-b border-gray-300 px-4 py-2 focus:outline-none">
                    <button onclick="analyzeAdvanced()" 
                            class="bg-blue-600 hover:bg-blue-700 text-white px-4 rounded-r-lg transition">
                        Phân tích
                    </button>
                </div>
                <p class="text-xs text-gray-500 mt-1">Hỗ trợ: Facebook, Instagram, Twitter/X, TikTok</p>
            </div>

            <!-- Kết quả phân tích -->
            <div id="resultContainer" class="hidden">
                <div class="grid md:grid-cols-2 gap-6 mb-6">
                    <!-- Thông tin người dùng -->
                    <div class="bg-gray-50 p-4 rounded-lg">
                        <h2 class="font-bold text-lg mb-3 text-blue-700">Thông tin định danh</h2>
                        <div class="flex items-start mb-3">
                            <div class="bg-blue-100 p-2 rounded-lg mr-3">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-blue-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
                                </svg>
                            </div>
                            <div>
                                <p><span class="font-medium">Tài khoản:</span> <span id="resultUsername">@username</span></p>
                                <p><span class="font-medium">Nền tảng:</span> <span id="resultPlatform">Facebook</span></p>
                                <p><span class="font-medium">Độ xác thực:</span> 
                                    <span id="verificationLevel" class="inline-flex items-center">
                                        <span class="h-2 w-24 bg-gray-200 rounded-full block mr-2">
                                            <span class="block h-2 bg-green-500 rounded-full" style="width: 85%"></span>
                                        </span>
                                        85%
                                    </span>
                                </p>
                            </div>
                        </div>
                    </div>

                    <!-- Phương pháp phân tích -->
                    <div class="bg-gray-50 p-4 rounded-lg">
                        <h2 class="font-bold text-lg mb-3 text-blue-700">Phương pháp sử dụng</h2>
                        <ul class="space-y-2">
                            <li class="flex items-start">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-green-500 mr-2 mt-0.5" viewBox="0 0 20 20" fill="currentColor">
                                    <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
                                </svg>
                                <span>Phân tích thông tin công khai</span>
                            </li>
                            <li class="flex items-start">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-green-500 mr-2 mt-0.5" viewBox="0 0 20 20" fill="currentColor">
                                    <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
                                </svg>
                                <span>Kiểm tra lịch sử check-in</span>
                            </li>
                            <li class="flex items-start">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-green-500 mr-2 mt-0.5" viewBox="0 0 20 20" fill="currentColor">
                                    <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
                                </svg>
                                <span>Đối chiếu IP công khai</span>
                            </li>
                        </ul>
                    </div>
                </div>

                <!-- Bản đồ kết quả -->
                <div class="mb-6">
                    <h2 class="font-bold text-lg mb-2 text-blue-700">Vị trí ước lượng</h2>
                    <div id="map" class="rounded-lg border border-gray-200"></div>
                    <div class="mt-2 flex justify-between items-center">
                        <div>
                            <p><span class="font-medium">Kinh độ:</span> <span id="longitude">106.6297</span></p>
                            <p><span class="font-medium">Vĩ độ:</span> <span id="latitude">10.8231</span></p>
                        </div>
                        <button onclick="zoomToLocation()" class="bg-blue-600 hover:bg-blue-700 text-white px-3 py-1 rounded text-sm">
                            Phóng to
                        </button>
                    </div>
                </div>

                <!-- Nâng cao -->
                <div class="bg-blue-50 border-l-4 border-blue-500 p-4 rounded-md mb-4">
                    <div class="flex">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-blue-500 mt-1 mr-2" viewBox="0 0 20 20" fill="currentColor">
                            <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2v-3a1 1 0 00-1-1H9z" clip-rule="evenodd" />
                        </svg>
                        <p class="text-sm text-blue-700">
                            <strong>Để tăng độ chính xác:</strong> Yêu cầu người dùng cấp quyền truy cập vị trí thực hoặc phân tích hình ảnh metadata (nếu có).
                        </p>
                    </div>
                </div>
            </div>

            <!-- Trạng thái loading -->
            <div id="loading" class="hidden text-center py-8">
                <div class="inline-block animate-spin rounded-full h-10 w-10 border-t-2 border-b-2 border-blue-500 mb-3"></div>
                <p>Đang phân tích nâng cao...</p>
                <p class="text-sm text-gray-500 mt-1">Quá trình có thể mất 10-20 giây</p>
            </div>

            <!-- Thông báo lỗi -->
            <div id="error" class="hidden bg-red-50 border-l-4 border-red-500 p-4 mb-4 rounded-md">
                <div class="flex">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-red-500 mt-1 mr-2" viewBox="0 0 20 20" fill="currentColor">
                        <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zM8.707 7.293a1 1 0 00-1.414 1.414L8.586 10l-1.293 1.293a1 1 0 101.414 1.414L10 11.414l1.293 1.293a1 1 0 001.414-1.414L11.414 10l1.293-1.293a1 1 0 00-1.414-1.414L10 8.586 8.707 7.293z" clip-rule="evenodd" />
                    </svg>
                    <div>
                        <p id="errorMessage" class="font-medium text-red-700"></p>
                        <p class="text-sm text-red-600 mt-1">Vui lòng thử lại hoặc sử dụng phương pháp khác</p>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Thư viện Leaflet JS -->
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

    <script>
        let map;
        let marker;

        // Phân tích nâng cao
        function analyzeAdvanced() {
            const platform = document.getElementById('platformSelect').value;
            const url = document.getElementById('profileUrl').value.trim();

            // Kiểm tra cơ bản
            if (!url) {
                showError("Vui lòng nhập URL hồ sơ");
                return;
            }

            if (!url.includes(platform)) {
                showError(`URL không khớp với nền tảng đã chọn (${platform})`);
                return;
            }

            // Reset UI
            document.getElementById('loading').classList.remove('hidden');
            document.getElementById('error').classList.add('hidden');
            document.getElementById('resultContainer').classList.add('hidden');

            // Giả lập API call
            setTimeout(() => {
                try {
                    const result = processAdvancedAnalysis(platform, url);
                    displayAdvancedResult(result);
                } catch (e) {
                    showError("Lỗi phân tích: " + e.message);
                } finally {
                    document.getElementById('loading').classList.add('hidden');
                }
            }, 2000);
        }

        function processAdvancedAnalysis(platform, url) {
            // Lấy username từ URL
            const username = extractUsername(url, platform);
            
            // Tạo dữ liệu giả lập
            return {
                username: username,
                platform: getPlatformName(platform),
                location: generateLocationData(),
                verification: Math.floor(Math.random() * 20) + 80, // 80-99%
                methods: [
                    "Phân tích hồ sơ công khai",
                    "Kiểm tra lịch sử check-in",
                    "Đối chiếu IP công khai"
                ]
            };
        }

        function extractUsername(url, platform) {
            // Logic trích xuất username đơn giản
            let username = url.split('/').pop();
            if (username.includes('?')) username = username.split('?')[0];
            return username || 'unknown';
        }

        function getPlatformName(platform) {
            const platforms = {
                'facebook': 'Facebook',
                'instagram': 'Instagram',
                'twitter': 'Twitter/X',
                'tiktok': 'TikTok'
            };
            return platforms[platform] || platform;
        }

        function generateLocationData() {
            const locations = [
                { 
                    city: "TP. Hồ Chí Minh", 
                    lat: 10.8231, 
                    lng: 106.6297,
                    details: "Khu vực trung tâm Quận 1, gần chợ Bến Thành"
                },
                {
                    city: "Hà Nội",
                    lat: 21.0278,
                    lng: 105.8342,
                    details: "Khu vực hồ Hoàn Kiếm, phố cổ"
                },
                {
                    city: "Đà Nẵng",
                    lat: 16.0544,
                    lng: 108.2022,
                    details: "Khu vực bãi biển Mỹ Khê"
                }
            ];

            return locations[Math.floor(Math.random() * locations.length)];
        }

        function displayAdvancedResult(result) {
            // Hiển thị thông tin cơ bản
            document.getElementById('resultUsername').textContent = `@${result.username}`;
            document.getElementById('resultPlatform').textContent = result.platform;
            document.getElementById('verificationLevel').querySelector('span:nth-child(2)').style.width = `${result.verification}%`;
            document.getElementById('verificationLevel').querySelector('span:nth-child(3)').textContent = `${result.verification}%`;

            // Hiển thị thông tin vị trí
            document.getElementById('latitude').textContent = result.location.lat.toFixed(4);
            document.getElementById('longitude').textContent = result.location.lng.toFixed(4);

            // Khởi tạo bản đồ
            initMap(result.location.lat, result.location.lng, result.location.city, result.location.details);

            // Hiển thị kết quả
            document.getElementById('resultContainer').classList.remove('hidden');
        }

        function initMap(lat, lng, title, description) {
            // Xóa bản đồ cũ nếu tồn tại
            if (map) map.remove();

            // Tạo bản đồ mới
            map = L.map('map').setView([lat, lng], 14);

            // Thêm layer bản đồ
            L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                attribution: '© <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
            }).addTo(map);

            // Thêm marker
            marker = L.marker([lat, lng], {
                icon: L.divIcon({
                    className: 'marker-pulse',
                    iconSize: [15, 15]
                })
            })
            .addTo(map)
            .bindPopup(`<b>${title}</b><br>${description}`)
            .openPopup();

            // Thêm vòng tròn độ chính xác
            L.circle([lat, lng], {
                color: 'red',
                fillColor: '#f03',
                fillOpacity: 0.2,
                radius: 500 // 500m radius
            }).addTo(map);
        }

        function zoomToLocation() {
            if (map && marker) {
                map.setView(marker.getLatLng(), 16);
            }
        }

        function showError(message) {
            document.getElementById('loading').classList.add('hidden');
            document.getElementById('errorMessage').textContent = message;
            document.getElementById('error').classList.remove('hidden');
        }
    </script>
</body>
</html>

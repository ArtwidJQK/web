
<!DOCTYPE html>
<html lang="vi" x-data="appState()" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mộng Mơ Nơi Anh Đào</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script defer src="https://cdn.jsdelivr.net/npm/alpinejs@3.x/dist/cdn.min.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Quicksand:wght@400;500;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">

    <style>
        /* --- Custom CSS --- */
        :root {
            --pink-light: #fbcfe8; /* Lighter Pink */
            --pink-main: #f472b6; /* Main Pink */
            --pink-dark: #db2777; /* Darker Pink */
            --blue-light: #bfdbfe; /* Lighter Blue */
            --blue-main: #60a5fa; /* Main Blue */
            --blue-dark: #2563eb; /* Darker Blue */
            --text-light: #f8fafc;
            --text-dark: #334155;
            --bg-dark: #2c2a4a; /* Slightly lighter purple/indigo background */
            --bg-glass: rgba(44, 42, 74, 0.6); /* Glass background based on bg-dark */
            --border-glass: rgba(255, 255, 255, 0.1);
        }

        body {
            font-family: 'Quicksand', sans-serif;
            background-color: var(--bg-dark);
            color: var(--text-light);
            overflow: hidden;
            height: 100vh;
            position: relative;
        }

        /* Animated Gradient Background - Faster */
        .animated-gradient-bg {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: -2;
            /* Lighter gradient colors */
            background: linear-gradient(-45deg, #3b3868, #4a4688, #6a63d8, #b8a9f5, #f8a5cd);
            background-size: 400% 400%;
            animation: gradientBG 15s ease infinite; /* Faster: 15s */
        }
        @keyframes gradientBG { 0% { background-position: 0% 50%; } 50% { background-position: 100% 50%; } 100% { background-position: 0% 50%; } }

        /* Cherry Blossom Effect (Same as before) */
        .petal-container { position: fixed; top: 0; left: 0; width: 100%; height: 100%; overflow: hidden; pointer-events: none; z-index: -1; }
        .petal { position: absolute; top: -10%; width: 15px; height: 10px; background-color: rgba(244, 114, 182, 0.7); border-radius: 50% 0; opacity: 0; animation: fall linear infinite, sway 2s ease-in-out infinite alternate; filter: blur(1px); }
        .petal::before { content: ''; position: absolute; top: 0; left: 0; width: 100%; height: 100%; background-color: inherit; border-radius: 0 50%; transform: rotate(30deg); transform-origin: bottom left; }
        @keyframes fall { 0% { top: -10%; opacity: 0; } 10% { opacity: 1; } 100% { top: 110%; opacity: 1; } }
        @keyframes sway { from { transform: translateX(0px) rotate(0deg); } to { transform: translateX(20px) rotate(15deg); } }

        /* Glassmorphism */
        .glass {
            backdrop-filter: blur(12px) saturate(110%); /* Slightly less saturate */
            background: var(--bg-glass);
            border: 1px solid var(--border-glass);
            border-radius: 12px; /* Default radius */
        }

        /* Glowing effect */
        .glow-light { box-shadow: 0 0 8px rgba(244, 114, 182, 0.4), 0 0 15px rgba(96, 165, 250, 0.4); }

        /* Custom Scrollbar */
        .custom-scrollbar::-webkit-scrollbar { width: 6px; }
        .custom-scrollbar::-webkit-scrollbar-track { background: rgba(255, 255, 255, 0.05); border-radius: 10px; }
        .custom-scrollbar::-webkit-scrollbar-thumb { background-color: var(--pink-main); border-radius: 10px; opacity: 0.7; }
        .custom-scrollbar::-webkit-scrollbar-thumb:hover { background-color: var(--pink-dark); opacity: 1; }

        /* Button Styles & Click Effect */
        .btn { padding: 0.6rem 1.2rem; border-radius: 9999px; font-weight: 500; transition: all 0.2s ease-out; cursor: pointer; border: none; outline: none; display: inline-flex; align-items: center; justify-content: center; }
        .btn:active { transform: scale(0.95); filter: brightness(0.9); } /* Click effect */
        .btn-primary { background-color: var(--pink-main); color: var(--text-light); }
        .btn-primary:hover { background-color: var(--pink-dark); transform: translateY(-2px); box-shadow: 0 4px 15px rgba(219, 39, 119, 0.3); }
        .btn-secondary { background-color: transparent; color: var(--pink-light); border: 1px solid var(--pink-light); }
        .btn-secondary:hover { background-color: rgba(244, 114, 182, 0.1); color: var(--pink-main); border-color: var(--pink-main); }
        .btn-nav { background-color: transparent; color: var(--pink-light); padding: 0.5rem 1rem; border-radius: 8px; font-weight: 500; transition: all 0.2s ease-out; }
        .btn-nav.active, .btn-nav:hover { background-color: rgba(244, 114, 182, 0.2); color: var(--pink-main); }
        .btn-icon { background: transparent; border: none; color: var(--pink-light); font-size: 1.3rem; padding: 0.5rem; border-radius: 50%; transition: all 0.2s ease-out; }
        .btn-icon:hover { background-color: rgba(244, 114, 182, 0.2); color: var(--pink-main); }
        .btn-icon:active { transform: scale(0.9); }

        /* Loading Overlay & Spinner */
        .loading-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background-color: rgba(0, 0, 0, 0.6); backdrop-filter: blur(5px); z-index: 9999; display: flex; justify-content: center; align-items: center; }
        .spinner { width: 50px; height: 50px; border: 5px solid rgba(255, 255, 255, 0.3); border-top-color: var(--pink-main); border-radius: 50%; animation: spin 1s linear infinite; }
        @keyframes spin { to { transform: rotate(360deg); } }

        /* Transitions for Views/Chapters */
        .view-transition { transition: opacity 0.5s ease-in-out, transform 0.5s ease-in-out; }
        .view-enter-active { /* Starting state */ opacity: 0; transform: translateY(20px) scale(0.98); }
        .view-enter-to { /* Ending state */ opacity: 1; transform: translateY(0) scale(1); }
        .view-leave-active { /* Starting state */ opacity: 1; transform: translateY(0) scale(1); }
        .view-leave-to { /* Ending state */ opacity: 0; transform: translateY(-20px) scale(0.98); }

        /* Prose adjustments for readability */
        .prose { line-height: 1.75; /* Slightly more spacing */ }
        .prose p { margin-bottom: 1em; } /* Ensure paragraph spacing */
        .prose br { display: none; /* Hide explicit <br> if using <p> tags */ }

        /* Music List Item */
        .music-item:hover { background-color: rgba(255, 255, 255, 0.05); }

    </style>
</head>
<body class="text-sm md:text-base">
    <div class="animated-gradient-bg"></div>
    <div class="petal-container" x-ref="petalContainer"></div>

    <div x-show="isLoading" class="loading-overlay view-transition"
         x-transition:enter="transition ease-out duration-300"
         x-transition:enter-start="opacity-0"
         x-transition:enter-end="opacity-100"
         x-transition:leave="transition ease-in duration-300"
         x-transition:leave-start="opacity-100"
         x-transition:leave-end="opacity-0">
        <div class="spinner"></div>
    </div>

    <div class="flex flex-col h-screen p-4 gap-4">

        <nav class="glass rounded-lg p-3 shadow-md flex justify-center items-center gap-4">
            <button @click="switchView('genres')" class="btn-nav" :class="{ 'active': currentView === 'genres' || currentView === 'reader' }">
                <i class="fas fa-book mr-2"></i>Thể loại
            </button>
            <button @click="switchView('music')" class="btn-nav" :class="{ 'active': currentView === 'music' }">
                <i class="fas fa-music mr-2"></i>Music
            </button>
        </nav>

        <main class="flex-grow flex flex-col items-center justify-center overflow-hidden relative">

            <div x-show="currentView === 'welcome'"
                 class="text-center flex flex-col items-center view-transition"
                 x-transition:enter="view-enter-active" x-transition:enter-start="opacity-0 scale-90" x-transition:enter-end="opacity-100 scale-100"
                 x-transition:leave="view-leave-active" x-transition:leave-start="opacity-100 scale-100" x-transition:leave-end="opacity-0 scale-90">
                <h1 class="text-4xl md:text-5xl font-bold mb-4 text-transparent bg-clip-text bg-gradient-to-r from-pink-300 to-blue-300">
                    Mộng Mơ Nơi Anh Đào
                </h1>
                <p class="text-lg md:text-xl mb-6 text-pink-100 max-w-xl">
                    Chào cậu, người bạn dễ thương đã ghé qua khu vườn nhỏ này! <i class="fas fa-heart text-pink-400"></i><br>
                    Hãy để những cánh hoa anh đào dẫn lối cậu vào thế giới của những câu chuyện, nơi cảm xúc được dệt nên từ con chữ và âm nhạc dịu dàng sẽ xoa dịu tâm hồn nhé...
                </p>
                <button @click="enterWorld()" class="btn btn-primary text-lg shadow-lg">
                    Tiến vào thế giới tiểu thuyết <i class="fas fa-feather-alt ml-2"></i>
                </button>
            </div>

            <div x-show="currentView === 'genres'" class="w-full h-full flex flex-col md:flex-row gap-4 view-transition"
                 x-transition:enter="view-enter-active" x-transition:enter-end="view-enter-to"
                 x-transition:leave="view-leave-active" x-transition:leave-end="view-leave-to">
                <aside class="w-full md:w-1/4 glass rounded-lg p-4 flex flex-col shadow-md h-auto md:h-full">
                    <h3 class="text-lg font-semibold text-center mb-3 text-pink-200 border-b border-pink-400/30 pb-2">Thể loại</h3>
                    <div class="flex-grow space-y-2 mt-2">
                        <button @click="selectGenre('ngon_tinh')"
                                :class="selectedGenre === 'ngon_tinh' ? 'bg-pink-900/50 text-pink-200' : 'hover:bg-pink-900/30 hover:text-pink-300'"
                                class="w-full text-left p-2 rounded-md transition-colors duration-200 font-medium">
                            <i class="fas fa-heartbeat mr-2 w-4 text-center"></i>Ngôn tình
                        </button>
                         <button @click="selectGenre('huyen_huyen')"
                                :class="selectedGenre === 'huyen_huyen' ? 'bg-pink-900/50 text-pink-200' : 'hover:bg-pink-900/30 hover:text-pink-300'"
                                class="w-full text-left p-2 rounded-md transition-colors duration-200 font-medium">
                             <i class="fas fa-magic mr-2 w-4 text-center"></i>Huyền huyễn
                        </button>
                         <button @click="selectGenre('kiem_hiep')"
                                :class="selectedGenre === 'kiem_hiep' ? 'bg-pink-900/50 text-pink-200' : 'hover:bg-pink-900/30 hover:text-pink-300'"
                                class="w-full text-left p-2 rounded-md transition-colors duration-200 font-medium">
                             <i class="fas fa-khanda mr-2 w-4 text-center"></i>Kiếm hiệp
                        </button>
                        </div>
                </aside>
                 <section class="w-full md:w-3/4 glass rounded-lg p-4 flex flex-col shadow-md h-full">
                    <div x-show="selectedGenre === 'ngon_tinh'" class="flex flex-col h-full">
                        <h3 class="text-lg font-semibold text-center mb-3 text-blue-200 border-b border-blue-400/30 pb-2">
                            <i class="fas fa-list-ol mr-2"></i>Danh sách chương (Ngôn tình)
                        </h3>
                        <div class="flex-grow overflow-y-auto custom-scrollbar pr-2 space-y-1 mt-2">
                             <template x-for="(chapter, index) in chapters" :key="index">
                                <button
                                    @click="readChapter(index)"
                                    class="w-full text-left p-3 rounded-md transition-all duration-200 ease-in-out transform text-gray-200 hover:bg-blue-900/30 hover:text-blue-200 hover:translate-x-1 flex justify-between items-center">
                                    <span x-text="`${index + 1}. ${chapter.title}`"></span>
                                    <i class="fas fa-chevron-right text-xs opacity-50"></i>
                                </button>
                            </template>
                        </div>
                    </div>
                     <div x-show="selectedGenre !== 'ngon_tinh'" class="flex flex-col h-full items-center justify-center text-center text-blue-200 p-6">
                         <i class="fas fa-hourglass-half fa-3x mb-4 animate-spin" style="animation-duration: 3s;"></i>
                         <p class="text-lg">Web đang cập nhật truyện mới cho thể loại này ạaa ><</p>
                         <p class="text-sm mt-2">Mong bạn quay lại sau nhé!</p>
                    </div>
                </section>
            </div>

            <div x-show="currentView === 'music'" class="w-full h-full flex flex-col glass rounded-lg p-4 shadow-md view-transition"
                 x-transition:enter="view-enter-active" x-transition:enter-end="view-enter-to"
                 x-transition:leave="view-leave-active" x-transition:leave-end="view-leave-to">
                <h3 class="text-lg font-semibold text-center mb-3 text-pink-200 border-b border-pink-400/30 pb-2">
                    <i class="fas fa-headphones-alt mr-2"></i>Playlist Nhẹ Nhàng
                </h3>
                <div class="flex-grow overflow-y-auto custom-scrollbar pr-2 space-y-2 mt-2">
                    <template x-for="(song, index) in musicPlaylist" :key="index">
                        <div class="flex items-center gap-3 p-2 rounded-md transition-colors duration-200 music-item">
                            <button @click="playSong(index)" class="btn-icon text-lg flex-shrink-0">
                                <i class="fas" :class="(currentSongIndex === index && isPlaying) ? 'fa-pause-circle' : 'fa-play-circle'"></i>
                            </button>
                            <div class="flex-grow overflow-hidden">
                                <p class="font-medium text-pink-100 truncate" x-text="song.title"></p>
                                <p class="text-xs text-gray-400" x-text="song.artist"></p>
                            </div>
                            <span class="text-sm text-gray-400 flex-shrink-0" x-text="formatTime(song.duration || 0)"></span>
                        </div>
                    </template>
                </div>
                 <div class="mt-4 pt-3 border-t border-pink-400/30 flex items-center gap-3" x-show="currentSongIndex !== null">
                     <img :src="musicPlaylist[currentSongIndex]?.cover || 'https://placehold.co/40x40/3b3868/fbcfe8?text=M'" alt="Cover" class="w-10 h-10 rounded flex-shrink-0">
                     <div class="flex-grow overflow-hidden">
                         <p class="text-sm font-medium text-pink-100 truncate" x-text="musicPlaylist[currentSongIndex]?.title"></p>
                         <input type="range" class="w-full h-1.5 bg-gray-700 rounded-lg appearance-none cursor-pointer range-sm dark:bg-gray-600 audio-progress"
                                x-ref="progressBar" x-model="progress" @input="seekAudio($event.target.value)" min="0" :max="duration" step="0.1">
                     </div>
                     <span class="text-xs text-gray-400" x-text="`${formatTime(currentTime)} / ${formatTime(duration)}`"></span>
                     <button @click="togglePlayPause()" class="btn-icon text-lg flex-shrink-0">
                         <i class="fas" :class="isPlaying ? 'fa-pause' : 'fa-play'"></i>
                     </button>
                 </div>
            </div>

            <div x-show="currentView === 'reader'" class="w-full h-full flex flex-col gap-4 view-transition"
                 x-transition:enter="view-enter-active" x-transition:enter-end="view-enter-to"
                 x-transition:leave="view-leave-active" x-transition:leave-end="view-leave-to">
                <div class="glass rounded-lg p-3 text-center shadow-sm flex items-center justify-between">
                     <button @click="switchView('genres')" class="btn-icon text-sm">
                         <i class="fas fa-arrow-left"></i>
                     </button>
                     <h2 class="text-lg md:text-xl font-semibold text-pink-200 truncate px-4" x-text="currentChapter ? currentChapter.title : ''"></h2>
                     <div class="w-8"></div> </div>
                <section id="contentArea" class="flex-grow glass rounded-lg p-4 md:p-6 overflow-y-auto custom-scrollbar glow-light shadow-lg relative prose prose-invert max-w-none text-gray-200 leading-relaxed text-base md:text-lg"
                         x-html="currentChapter ? currentChapter.formattedText : ''">
                </section>
                <div class="glass rounded-lg p-3 flex justify-between items-center shadow-sm">
                    <button @click="prevChapter()" :disabled="currentChapterIndex === 0" class="btn btn-secondary text-sm" :class="{'opacity-50 cursor-not-allowed': currentChapterIndex === 0}">
                        <i class="fas fa-chevron-left mr-1"></i> Chương trước
                    </button>
                    <span class="text-sm text-gray-400" x-text="`Chương ${currentChapterIndex !== null ? currentChapterIndex + 1 : '-'}/${chapters.length}`"></span>
                    <button @click="nextChapter()" :disabled="currentChapterIndex === chapters.length - 1" class="btn btn-secondary text-sm" :class="{'opacity-50 cursor-not-allowed': currentChapterIndex === chapters.length - 1}">
                        Chương sau <i class="fas fa-chevron-right ml-1"></i>
                    </button>
                </div>
            </div>

        </main>

        <audio x-ref="audioPlayer" preload="metadata"></audio>
    </div>

    <script>
        function appState() {
            return {
                // --- State Variables ---
                isLoading: false,
                currentView: 'welcome', // welcome, genres, music, reader
                selectedGenre: 'ngon_tinh', // default genre
                currentChapterIndex: null,
                chapters: [
                    // --- Story Content ---
                    {
                        title: 'Nắng hạ vương vai',
                        text: `Năm ấy, mùa hạ như đổ lửa, nhưng trong lòng An lại trong veo như mặt hồ thu. Lần đầu tiên cô gặp Phong là dưới gốc phượng vĩ đỏ rực góc sân trường. Anh đứng đó, ngược nắng, mái tóc đen mềm khẽ bay trong gió, nụ cười tỏa sáng hơn cả cái nắng gay gắt nhất.\n\nAn khi đó chỉ là cô bé lớp 10 nhút nhát, mái tóc tết hai bên, cặp kính dày cộp che gần nửa khuôn mặt. Còn Phong, anh là hotboy khối 12, đội trưởng đội bóng rổ, niềm mơ ước của bao nữ sinh. Hai thế giới tưởng chừng chẳng bao giờ giao nhau.\n\nVậy mà, định mệnh lại thích trêu đùa. An làm rơi chồng sách vở ngay trước mặt Phong. Anh không những không cười chê mà còn cúi xuống nhặt giúp, nụ cười vẫn ấm áp như nắng. "Cẩn thận nhé, bé con," giọng anh trầm ấm. Khoảnh khắc ấy, trái tim An như lỡ một nhịp. Cả mùa hạ năm đó, hình bóng và nụ cười của Phong cứ quanh quẩn trong tâm trí cô, biến những ngày hè oi ả thành chuỗi ngày mộng mơ, ngọt ngào.`
                    },
                    {
                        title: 'Lời hứa dưới mưa',
                        text: `Họ dần trở nên thân thiết hơn. Phong không hề kiêu ngạo như vẻ ngoài, anh dịu dàng, kiên nhẫn lắng nghe những câu chuyện không đầu không cuối của An. Anh chỉ cô chơi bóng rổ, kiên nhẫn sửa từng động tác vụng về. An thì làm bánh mang cho Phong sau mỗi buổi tập mệt nhoài. Tình cảm cứ thế lớn dần, nhẹ nhàng như cơn mưa rào mùa hạ.\n\nMột buổi chiều mưa tầm tã, Phong đưa An về. Chiếc ô nhỏ chẳng đủ che hết hai người, vai áo cả hai đều ướt sũng. Dưới mái hiên cũ kỹ, Phong bất ngờ nắm lấy tay An. "An này," anh ngập ngừng, "Anh sắp phải đi du học rồi."\n\nTim An như thắt lại. Cô cố nở nụ cười: "Vậy ạ? Chúc mừng anh." Nhưng giọng nói run run đã bán đứng cô. Phong nhìn sâu vào mắt An, ánh mắt chứa đầy lưu luyến và cả sự quyết tâm. "Chờ anh về nhé? Ba năm thôi. Anh về, mình sẽ cùng nhau đi hết những con đường còn lại."\n\nDưới màn mưa trắng xóa, An gật đầu, nước mắt hòa cùng nước mưa lăn dài trên má. Lời hứa ấy, như một sợi dây vô hình, buộc chặt hai trái tim non trẻ, bất chấp khoảng cách và thời gian.`
                    },
                    {
                        title: 'Chỉ còn lại tro tàn',
                        text: `Ba năm dài đằng đẵng trôi qua. An giữ trọn lời hứa, từ chối bao lời tỏ tình, chỉ chờ ngày Phong trở về. Cô đã trở thành một thiếu nữ xinh đẹp, tự tin, không còn là cô bé nhút nhát năm nào. Ngày Phong về nước cũng là ngày hạ cuối cùng của thời sinh viên.\n\nAn mặc chiếc váy trắng tinh khôi Phong thích nhất, đứng đợi anh ở sân bay, bó hoa hướng dương rực rỡ trên tay. Nhưng người bước ra từ cổng đến không phải là Phong của cô. Vẫn là anh, nhưng ánh mắt đã khác, nụ cười đã khác, và bên cạnh anh... là một cô gái khác, xinh đẹp và rạng rỡ.\n\nHọ tay trong tay, cười nói vui vẻ. Phong lướt qua An như lướt qua một người xa lạ. Bó hoa trên tay An rơi xuống, những cánh hoa vàng úa tả tơi như trái tim cô lúc này. Lời hứa năm xưa dưới mưa, tình yêu ba năm chờ đợi, tất cả vụn vỡ tan tành chỉ trong một khoảnh khắc.\n\nAn đứng đó, chết lặng giữa dòng người đông đúc. Nắng hạ vẫn rực rỡ, nhưng thế giới của cô chỉ còn lại một màu xám xịt. Mối tình đầu đẹp như mơ, khởi đầu bằng nụ cười ấm áp dưới nắng hạ, cuối cùng lại kết thúc bằng sự thờ ơ lạnh lẽo, để lại trong lòng cô một vết sẹo không bao giờ lành, chỉ còn lại tro tàn của những kỷ niệm ngọt ngào.`
                    }
                ].map(chapter => ({
                    ...chapter,
                    // Format text with <p> tags for paragraphs
                    formattedText: chapter.text.split('\n\n')
                                        .map(p => `<p>${p.replace(/\n/g, '<br>')}</p>`)
                                        .join('')
                })),
                musicPlaylist: [
                    { title: "Đi qua mùa hạ", artist: "Thái Đinh", duration: 245, src: "https://cdn.pixabay.com/download/audio/2022/11/23/audio_9a0f6fce4b.mp3?filename=lofi-chill-medium-version-159456.mp3", cover: "https://placehold.co/40x40/f472b6/ffffff?text=H" },
                    { title: "Chỉ là không cùng nhau", artist: "Tăng Phúc ft. Trương Thảo Nhi", duration: 310, src: "https://cdn.pixabay.com/download/audio/2022/03/15/audio_f239d9823a.mp3?filename=sunny-morning-110050.mp3", cover: "https://placehold.co/40x40/60a5fa/ffffff?text=C" },
                    { title: "Lofi Buồn", artist: "Unknown Artist", duration: 180, src: "https://cdn.pixabay.com/download/audio/2021/11/23/audio_804be29ead.mp3?filename=empty-mind-118973.mp3", cover: "https://placehold.co/40x40/a78bfa/ffffff?text=L" },
                    // Add more songs here
                ],
                currentSongIndex: null, // Index of the currently loaded/playing song in the playlist
                isPlaying: false,
                currentTime: 0,
                duration: 0,
                progress: 0,
                audioContextResumed: false,

                // --- Computed Properties ---
                get currentChapter() {
                    return this.currentChapterIndex !== null ? this.chapters[this.currentChapterIndex] : null;
                },
                get currentSong() {
                    return this.currentSongIndex !== null ? this.musicPlaylist[this.currentSongIndex] : null;
                },

                // --- Methods ---
                init() {
                    console.log("App Initializing...");
                    this.createPetals(30);
                    this.$nextTick(() => this.setupAudioPlayer());
                },

                setupAudioPlayer() {
                     const player = this.$refs.audioPlayer;
                     if (player) {
                         console.log("Audio player ref found.");
                         player.addEventListener('loadedmetadata', this.loadMetadata.bind(this));
                         player.addEventListener('timeupdate', this.updateProgress.bind(this));
                         player.addEventListener('ended', this.handleSongEnd.bind(this));
                         player.addEventListener('error', (e) => { console.error("Audio Error:", e); this.isPlaying = false; });
                         console.log("Audio event listeners attached.");
                     } else {
                         console.error("Audio player ref not found!");
                     }
                },

                // --- View Switching ---
                switchView(view) {
                    if (this.currentView === view) return; // Do nothing if already in the view

                    this.isLoading = true;
                    // Simulate loading time for effect
                    setTimeout(() => {
                        this.currentView = view;
                        this.isLoading = false;
                        console.log("Switched to view:", view);
                         // Reset chapter if moving away from reader
                         if (view !== 'reader') {
                             this.currentChapterIndex = null;
                         }
                         // Pause music if moving away from music view? (Optional)
                         // if (view !== 'music' && this.isPlaying) {
                         //     this.togglePlayPause();
                         // }
                    }, 500); // Loading duration
                },

                enterWorld() {
                    this.isLoading = true;
                    setTimeout(() => {
                        this.currentView = 'genres'; // Default to genres after welcome
                        this.isLoading = false;
                        console.log("Entered world, view set to genres");
                        this.resumeAudioContext(); // Try resuming context on first interaction
                    }, 800); // Loading duration after welcome
                },

                // --- Genre & Chapter Logic ---
                selectGenre(genre) {
                    this.selectedGenre = genre;
                    console.log("Selected genre:", genre);
                    // In a real app, you might fetch chapters for this genre here
                },

                readChapter(index) {
                    if (index >= 0 && index < this.chapters.length) {
                        this.isLoading = true;
                        setTimeout(() => {
                            this.currentChapterIndex = index;
                            this.currentView = 'reader';
                            this.scrollToTop();
                            this.isLoading = false;
                            console.log(`Reading chapter ${index}`);
                        }, 300); // Short transition for chapter load
                    }
                },

                prevChapter() {
                    if (this.currentChapterIndex > 0) {
                        this.readChapter(this.currentChapterIndex - 1);
                    }
                },

                nextChapter() {
                    if (this.currentChapterIndex < this.chapters.length - 1) {
                        this.readChapter(this.currentChapterIndex + 1);
                    }
                },

                scrollToTop() {
                    this.$nextTick(() => {
                        const contentArea = document.getElementById('contentArea');
                        if (contentArea) contentArea.scrollTop = 0;
                    });
                },

                // --- Audio Player Logic ---
                resumeAudioContext() {
                    if (!this.audioContextResumed && window.AudioContext) {
                        const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                        if (audioCtx.state === 'suspended') {
                            console.log("Audio context suspended, attempting resume...");
                            audioCtx.resume().then(() => {
                                console.log("Audio context resumed.");
                                this.audioContextResumed = true;
                            }).catch(e => console.error("Context resume error:", e));
                        } else {
                             this.audioContextResumed = true;
                        }
                    }
                },

                playSong(index) {
                    console.log(`Attempting to play song index: ${index}`);
                    if (index === this.currentSongIndex) {
                        // If it's the same song, just toggle play/pause
                        this.togglePlayPause();
                    } else {
                        // Load and play the new song
                        this.currentSongIndex = index;
                        const song = this.musicPlaylist[index];
                        const player = this.$refs.audioPlayer;
                        if (player && song && song.src) {
                             console.log(`Loading new source: ${song.src}`);
                            player.src = song.src;
                            player.load(); // Important: load the new source
                            // Play is called via promise after load or directly
                            const playPromise = player.play();
                            if (playPromise !== undefined) {
                                playPromise.then(() => {
                                    console.log("Playback started for new song.");
                                    this.isPlaying = true;
                                }).catch(error => {
                                    console.error("New song playback failed:", error);
                                    this.isPlaying = false;
                                    // Attempt to resume context if it failed
                                    this.resumeAudioContext();
                                });
                            } else {
                                 this.isPlaying = !player.paused;
                            }
                        } else {
                             console.error("Player or song source not found for index:", index);
                        }
                    }
                },

                togglePlayPause() {
                    const player = this.$refs.audioPlayer;
                    if (!player || this.currentSongIndex === null) {
                         console.warn("No song loaded to play/pause.");
                         return;
                    }
                    console.log(`Toggling Play/Pause. Currently Playing: ${this.isPlaying}`);

                    if (this.isPlaying) {
                        player.pause();
                        this.isPlaying = false;
                        console.log("Audio paused.");
                    } else {
                         this.resumeAudioContext(); // Ensure context is active
                         const playPromise = player.play();
                         if (playPromise !== undefined) {
                             playPromise.then(() => {
                                 console.log("Audio resumed/started.");
                                 this.isPlaying = true;
                             }).catch(error => {
                                 console.error("Playback failed:", error);
                                 this.isPlaying = false;
                             });
                         } else {
                              this.isPlaying = !player.paused;
                         }
                    }
                },

                handleSongEnd() {
                    console.log("Song ended.");
                    this.isPlaying = false;
                    this.currentTime = 0;
                    this.progress = 0;
                    // Optional: Play next song?
                    // if (this.currentSongIndex < this.musicPlaylist.length - 1) {
                    //     this.playSong(this.currentSongIndex + 1);
                    // } else {
                    //     this.currentSongIndex = null; // End of playlist
                    // }
                },

                loadMetadata() {
                    const player = this.$refs.audioPlayer;
                    if (player) {
                        this.duration = player.duration || 0;
                        console.log(`Metadata loaded. Duration: ${this.formatTime(this.duration)}`);
                         // Update duration for the specific song in the playlist if needed (useful if not pre-defined)
                         if (this.currentSongIndex !== null && !this.musicPlaylist[this.currentSongIndex].duration) {
                              this.musicPlaylist[this.currentSongIndex].duration = this.duration;
                         }
                    }
                },

                updateProgress() {
                    const player = this.$refs.audioPlayer;
                    if (player && isFinite(player.currentTime)) {
                        this.currentTime = player.currentTime;
                        this.progress = this.currentTime;
                    }
                },

                seekAudio(value) {
                    const player = this.$refs.audioPlayer;
                    const seekTime = parseFloat(value);
                    if (player && isFinite(seekTime) && seekTime >= 0 && seekTime <= this.duration) {
                        player.currentTime = seekTime;
                        this.currentTime = seekTime;
                        console.log(`Seeked to: ${this.formatTime(seekTime)}`);
                    }
                },

                formatTime(seconds) {
                    if (isNaN(seconds) || !isFinite(seconds)) return '0:00';
                    const minutes = Math.floor(seconds / 60);
                    const secs = Math.floor(seconds % 60);
                    return `${minutes}:${secs < 10 ? '0' : ''}${secs}`;
                },

                // --- Cherry Blossom Effect ---
                createPetals(count) {
                    // Debounce or ensure this only runs once effectively
                    if (this.petalsCreated) return;
                    this.petalsCreated = true; // Simple flag

                    const container = this.$refs.petalContainer;
                    if (!container) return;
                    console.log(`Creating ${count} petals...`);
                    for (let i = 0; i < count; i++) {
                        const petal = document.createElement('div');
                        petal.classList.add('petal');
                        petal.style.left = `${Math.random() * 100}%`;
                        const duration = Math.random() * 6 + 7; // 7-13 seconds fall time
                        const delay = Math.random() * 12;
                        const swayDuration = Math.random() * 2 + 1.5;
                        petal.style.animationDuration = `${duration}s, ${swayDuration}s`;
                        petal.style.animationDelay = `${delay}s, ${delay}s`;
                        petal.style.width = `${Math.random() * 5 + 10}px`;
                        petal.style.height = `${Math.random() * 3 + 7}px`;
                        petal.style.opacity = `${Math.random() * 0.3 + 0.4}`; // Vary opacity
                        container.appendChild(petal);
                    }
                },
                petalsCreated: false // Flag to prevent multiple creations
            }
        }
    </script>
</body>
</html>

<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Robot Humanoid - Suara AI Asli</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }

    body {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      background: radial-gradient(circle at center, #0a192f 0%, #020813 100%);
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      overflow: hidden;
    }

    .robot-wrapper {
      position: relative;
      width: 320px;
      height: 550px;
      display: flex;
      flex-direction: column;
      align-items: center;
      animation: float 4s ease-in-out infinite;
      filter: drop-shadow(0 0 20px rgba(0, 255, 255, 0.15));
      transition: filter 0.5s;
    }

    .robot-wrapper.talking {
      filter: drop-shadow(0 0 50px rgba(0, 255, 255, 0.7));
    }

    @keyframes float {
      0%, 100% { transform: translateY(0); }
      50% { transform: translateY(-12px); }
    }

    /* --- KEPALA --- */
    .head {
      position: relative;
      width: 90px;
      height: 120px;
      background: linear-gradient(135deg, #ffffff 0%, #a0b4d0 40%, #e0f0ff 100%);
      border-radius: 45px 45px 30px 30px / 60px 60px 40px 40px;
      box-shadow: inset -8px -8px 15px rgba(0,0,0,0.15), inset 5px 5px 15px rgba(255,255,255,0.9);
      z-index: 10;
    }

    .ear {
      position: absolute;
      top: 30px;
      width: 25px;
      height: 50px;
      background: linear-gradient(90deg, #111, #333, #111);
      border-radius: 8px;
    }
    .ear.left { left: -18px; }
    .ear.right { right: -18px; }
    .ear::after {
      content: '';
      position: absolute;
      top: 50%; left: 50%;
      transform: translate(-50%, -50%);
      width: 12px; height: 20px;
      border: 2px solid #555;
      border-radius: 50%;
      box-shadow: 0 0 10px rgba(0, 255, 255, 0.5);
    }

    .eyes {
      position: absolute;
      top: 45px;
      width: 100%;
      display: flex;
      justify-content: space-evenly;
      padding: 0 15px;
    }
    .eye {
      width: 12px; height: 6px;
      background: #00ffff;
      border-radius: 5px;
      box-shadow: 0 0 15px #00ffff;
      animation: blink 4s infinite;
    }
    @keyframes blink { 0%, 95%, 100% { transform: scaleY(1); } 97% { transform: scaleY(0.1); } }

    /* Mulut */
    .mouth {
      position: absolute;
      bottom: 20px; left: 50%;
      transform: translateX(-50%);
      width: 20px; height: 4px;
      background: #111;
      border-radius: 2px;
      transition: all 0.1s;
    }
    .talking .mouth {
      animation: talkAnim 0.2s infinite alternate;
    }
    @keyframes talkAnim {
      0% { height: 4px; width: 20px; background: #111; }
      100% { height: 16px; width: 12px; border-radius: 8px; background: #333; }
    }

    .neck {
      width: 35px; height: 35px;
      background: repeating-linear-gradient(90deg, #111, #111 3px, #444 3px, #444 6px);
      border-left: 2px solid #555; border-right: 2px solid #555;
      margin-top: -8px; z-index: 5;
    }

    .torso {
      position: relative;
      width: 140px; height: 170px;
      background: linear-gradient(135deg, #ffffff 0%, #a0b4d0 40%, #e0f0ff 100%);
      clip-path: polygon(15% 0, 85% 0, 100% 30%, 80% 100%, 20% 100%, 0% 30%);
      box-shadow: inset -15px -15px 25px rgba(0,0,0,0.2);
      z-index: 8; margin-top: -5px;
      display: flex; flex-direction: column; align-items: center;
    }

    .torso::before {
      content: ''; position: absolute;
      top: 20px; width: 80px; height: 60px;
      border-top: 2px solid rgba(255,255,255,0.8); border-radius: 50%;
    }

    .abdomen {
      position: absolute; bottom: -25px;
      width: 70px; height: 50px;
      background: #0a0a0a; border-radius: 5px 5px 15px 15px; border: 2px solid #333;
      display: flex; flex-direction: column; justify-content: space-evenly; align-items: center;
    }
    .ring {
      width: 50px; height: 6px; background: #222; border-radius: 50%; border-top: 2px solid #555;
    }

    .arm {
      position: absolute; top: 5px; width: 35px; height: 200px;
      display: flex; flex-direction: column; align-items: center; z-index: 4;
    }
    .arm.left { left: -35px; transform-origin: top center; animation: swingL 4s ease-in-out infinite alternate; }
    .arm.right { right: -35px; transform-origin: top center; animation: swingR 4s ease-in-out infinite alternate; }

    @keyframes swingL { 0% { transform: rotate(3deg); } 100% { transform: rotate(8deg); } }
    @keyframes swingR { 0% { transform: rotate(-3deg); } 100% { transform: rotate(-8deg); } }

    .shoulder {
      width: 45px; height: 45px;
      background: radial-gradient(circle at 30% 30%, #555, #000);
      border-radius: 50%; border: 2px solid #444;
    }

    .upper-arm, .forearm {
      width: 22px; height: 70px;
      background: linear-gradient(90deg, #fff, #a0b4d0, #fff); border-radius: 12px;
    }
    .upper-arm { margin-top: -8px; }
    .forearm { margin-top: -5px; }

    .elbow {
      width: 22px; height: 22px; background: #111;
      border-radius: 50%; border: 2px solid #444; margin-top: -5px;
    }
    .hand {
      width: 18px; height: 35px; background: #111;
      border-radius: 8px; margin-top: -5px;
    }

    .pelvis {
      position: relative; width: 130px; height: 55px;
      background: linear-gradient(135deg, #fff, #a0b4d0);
      clip-path: polygon(10% 0, 90% 0, 100% 100%, 0% 100%);
      margin-top: -10px; z-index: 7;
    }

    .start-btn {
      position: absolute; bottom: 30px; padding: 15px 40px;
      font-size: 16px; font-weight: bold; color: #00ffff;
      background: rgba(0, 0, 0, 0.8); border: 2px solid #00ffff; border-radius: 30px;
      cursor: pointer; text-transform: uppercase; letter-spacing: 2px;
      box-shadow: 0 0 20px rgba(0, 255, 255, 0.4); transition: all 0.3s ease; z-index: 10;
    }
    .start-btn:hover { background: rgba(0, 255, 255, 0.2); }
    .start-btn.hidden { display: none; }
  </style>
</head>
<body>

  <div class="robot-wrapper" id="robotWrapper">
    <div class="head">
      <div class="ear left"></div>
      <div class="ear right"></div>
      <div class="eyes">
        <div class="eye"></div>
        <div class="eye"></div>
      </div>
      <div class="mouth"></div>
    </div>
    <div class="neck"></div>
    <div class="torso">
      <div class="arm left">
        <div class="shoulder"></div><div class="upper-arm"></div><div class="elbow"></div><div class="forearm"></div><div class="hand"></div>
      </div>
      <div class="arm right">
        <div class="shoulder"></div><div class="upper-arm"></div><div class="elbow"></div><div class="forearm"></div><div class="hand"></div>
      </div>
      <div class="abdomen">
        <div class="ring"></div><div class="ring"></div><div class="ring"></div>
      </div>
    </div>
    <div class="pelvis"></div>
  </div>

  <button class="start-btn" id="startBtn">Aktifkan Robot AI</button>

  <script>
    const startBtn = document.getElementById('startBtn');
    const robotWrapper = document.getElementById('robotWrapper');

    // Teks yang akan diucapkan (Durasi ±10 detik)
    const teksRobot = "Halo manusia. Saya adalah unit intelijen buatan humanoid. Sistem sensor dan saraf mekanik saya telah aktif sepenuhnya. Silakan berikan saya instruksi.";

    // --- FUNGSI MEMBANGUNKAN SUARA AI DI HP ---
    function pancingSuaraAI() {
      if ('speechSynthesis' in window) {
        // Trik: Ucapkan teks kosong dengan volume 0 untuk membangunkan mesin TTS HP
        const wakeUp = new SpeechSynthesisUtterance('');
        wakeUp.volume = 0;
        window.speechSynthesis.speak(wakeUp);
      }
    }

    // Pancing suara segera setelah halaman dimuat
    window.addEventListener('load', pancingSuaraAI);

    startBtn.addEventListener('click', () => {
      startBtn.classList.add('hidden');
      pancingSuaraAI(); // Pancing sekali lagi saat tombol diklik

      if ('speechSynthesis' in window) {
        // Bersihkan antrean suara sebelumnya
        window.speechSynthesis.cancel();

        setTimeout(() => {
          const ucapan = new SpeechSynthesisUtterance(teksRobot);

          // --- PENGATURAN SUARA AI MANUSIA ROBOTIK ---
          ucapan.pitch = 0.1; // Sangat rendah (Berat/Seperti mesin)
          ucapan.rate = 0.9;  // Sedikit lebih lambat
          ucapan.volume = 1;

          // Cari suara yang tersedia
          const daftarSuara = window.speechSynthesis.getVoices();
          let suaraPilihan = null;

          // 1. Coba cari suara Bahasa Indonesia
          suaraPilihan = daftarSuara.find(v => v.lang.includes('id') || v.lang.includes('ID'));

          // 2. Jika tidak ada, cari suara Bahasa Inggris (Paling umum di HP)
          if (!suaraPilihan) {
            ucapan.lang = 'en-US';
            suaraPilihan = daftarSuara.find(v => v.lang.includes('en')) || daftarSuara[0];
          } else {
            ucapan.lang = 'id-ID';
          }

          if (suaraPilihan) {
            ucapan.voice = suaraPilihan;
          }

          // --- ANIMASI ---
          ucapan.onstart = () => {
            robotWrapper.classList.add('talking');
          };

          ucapan.onend = () => {
            robotWrapper.classList.remove('talking');
            startBtn.classList.remove('hidden'); // Munculkan tombol lagi
          };
          
          ucapan.onerror = (e) => {
            console.error("Error TTS:", e);
            robotWrapper.classList.remove('talking');
            alert("HP Anda menolak fitur suara AI. Pastikan Anda menggunakan Google Chrome dan volume media menyala.");
            startBtn.classList.remove('hidden');
          };

          // Mulai bicara
          window.speechSynthesis.speak(ucapan);
          
        }, 100); // Jeda sedikit agar HP siap

      } else {
        alert("Browser Anda tidak mendukung suara AI. Gunakan Google Chrome.");
        startBtn.classList.remove('hidden');
      }
    });

    // Memuat daftar suara secara asinkron (kadang browser butuh waktu)
    if ('speechSynthesis' in window) {
      window.speechSynthesis.onvoiceschanged = () => {
        window.speechSynthesis.getVoices();
      };
    }
  </script>
</body>
</html>

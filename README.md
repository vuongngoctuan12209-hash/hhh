<div class="sensi-item"><div class="sensi-info"><div class="sensi-name">Mắt nhìn</div><div class="sensi-bar-bg"><div class="sensi-bar" id="b6"></div></div></div><div class="sensi-val" id="v6">0</div></div>

    <div class="label" style="margin-top:20px;">CHỨC NĂNG BOOST</div>
    <div class="func-grid">
      <button class="func-btn" id="f1" onclick="toggleFunc('f1','Aim Lock')">AIM LOCK</button>
      <button class="func-btn" id="f2" onclick="toggleFunc('f2','Tăng CPU')">TĂNG CPU</button>
      <button class="func-btn" id="f3" onclick="toggleFunc('f3','DPI Boost')">DPI BOOST</button>
      <button class="func-btn" id="f4" onclick="toggleFunc('f4','Nhạy Tâm')">NHẠY TÂM</button>
    </div>

    <button class="btn btn-blue" onclick="activeAll()">KÍCH HOẠT TẤT CẢ CHỨC NĂNG</button>
    <button class="btn btn-red" onclick="logout()">THOÁT MENU</button>
    
    <div class="tip">
      Lưu ý: ĐÂY KHÔNG PHẢI PHẦN MỀM THỨ 3.
    </div>
  </div>

  <div class="footer">TOOL OB54 • Visual Only • Scroll OK</div>
</div>

<div class="toast" id="toast"></div>

<script>
const MENU_KEY = "ALLPLISONLAP";

function showToast(msg) {
  let t = document.getElementById('toast');
  t.innerText = msg;
  t.style.display = 'block';
  setTimeout(() => t.style.display = 'none', 1500);
}

function checkKey() {
  if(document.getElementById('keyInput').value === MENU_KEY) {
    document.getElementById('lockScreen').style.display = 'none';
    document.getElementById('app').style.display = 'block';
    gen();
  } else {
    showToast('Key không đúng!');
  }
}

function logout() {
  document.getElementById('app').style.display = 'none';
  document.getElementById('lockScreen').style.display = 'flex';
  document.getElementById('keyInput').value = '';
}

function hashSeed(str, salt) {
  let h = 2166136261;
  for (let i = 0; i < str.length; i++) {
    h ^= str.charCodeAt(i);
    h += (h << 1) + (h << 4) + (h << 7) + (h << 8) + (h << 24);
  }
  h += salt * 16777619;
  return Math.abs(h);
}

function gen() {
  let name = document.getElementById('device').value.trim();
  if(!name) return showToast('Nhập tên máy đi');
  
  document.getElementById('deviceTag').innerText = name;
  let key = name.toLowerCase();
  
  for(let i = 1; i <= 6; i++) {
    let val = 100 + (hashSeed(key, i * 31) % 101);
    document.getElementById('v' + i).innerText = val;
    document.getElementById('b' + i).style.width = (val/2) + '%';
  }
  
  document.getElementById('result').style.display = 'block';
  document.querySelector('.app').scrollTo({top: 300, behavior: 'smooth'});
  showToast('Đã tạo độ nhạy cho ' + name);
}

function toggleFunc(id, name) {
  let btn = document.getElementById(id);
  btn.classList.toggle('active');
  if(btn.classList.contains('active')) {
    showToast(name + ' ON');
  } else {
    showToast(name + ' OFF');
  }
}

function activeAll() {
  ['f1','f2','f3','f4'].forEach(id => {

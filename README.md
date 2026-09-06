!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Input Data</title>
<style>
body{font-family:-apple-system,BlinkMacSystemFont,Arial;margin:0;background:#f5f5f7;padding:24px;color:#111}
main{max-width:560px;margin:auto}.card{background:white;border-radius:20px;padding:18px;margin:15px 0;box-shadow:0 4px 20px #0001}
h1{margin-bottom:4px}p{color:#666}label{font-weight:bold;font-size:14px}
input{width:100%;padding:15px;margin:8px 0;border:1px solid #ccc;border-radius:13px;font-size:18px;box-sizing:border-box}
button{padding:13px 16px;border:0;border-radius:13px;font-size:16px;font-weight:bold}
.primary{background:#07f;color:#fff;flex:1}.gray{background:#e9e9ed}.red{background:#f33;color:#fff}
.row{display:flex;gap:10px;margin-top:8px}.status{text-align:center;padding:12px;margin-top:12px;border-radius:12px;font-weight:bold}
.ok{background:#dff7e7;color:#176b38}.err{background:#ffe5e3;color:#a21b14}
.item{padding:11px 0;border-bottom:1px solid #eee;word-break:break-word}.count{color:#666;font-size:14px}
</style>
</head>
<body>
<main>
<h1>Input Data</h1>
<p>Scan barcode/QR atau ketik nama secara manual.</p>
<div class="card">
<label>Nama / Barcode / QR</label>
<input id="input" placeholder="Scan atau ketik nama..." autocomplete="off">
<div class="row"><button class="primary" onclick="save()">SCAN / INPUT</button></div>
<div id="status"></div>
</div>
<div class="card">
<div id="count" class="count"></div><div id="list"></div>
<div class="row"><button class="gray" onclick="exportCSV()">Export CSV</button><button class="red" onclick="clearAll()">Hapus Semua</button></div>
</div>
</main>
<script>
const KEY='mupti_data';let data=JSON.parse(localStorage.getItem(KEY)||'[]');
const input=document.getElementById('input');
function render(){count.textContent=data.length+' data tersimpan';list.innerHTML=data.map((x,i)=>'<div class="item">'+(i+1)+'. '+x+'</div>').join('')||'<small>Belum ada data.</small>'}
function message(t,c){status.textContent=t;status.className=c;setTimeout(()=>status.className='',2200)}
function save(){let v=input.value.trim();if(!v)return message('❌ Isi data dulu.','err');if(data.some(x=>x.toLowerCase()===v.toLowerCase())){input.value='';input.focus();return message('❌ Namine Duplikat, Ngapuntene!','err')}data.push(v);localStorage.setItem(KEY,JSON.stringify(data));render();input.value='';input.focus();message('✓ Data Sampun Masuk Geh','ok')}
input.addEventListener('keydown',e=>{if(e.key==='Enter')save()});
function clearAll(){if(confirm('Hapus semua data?')){data=[];localStorage.setItem(KEY,'[]');render()}}
function exportCSV(){let csv='No,Nama\n'+data.map((x,i)=>(i+1)+',"'+x.replaceAll('"','""')+'"').join('\n');let a=document.createElement('a');a.href=URL.createObjectURL(new Blob(['\ufeff'+csv],{type:'text/csv'}));a.download='data-mupti.csv';a.click()}
render();input.focus();
</script>
</body>
</html>
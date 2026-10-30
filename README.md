<!DOCTYPE html>
<html lang="ur" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Attiq Utility Store</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:Arial,sans-serif;background:#f3f7f3;color:#222}
header{background:#126b3b;color:white;text-align:center;padding:25px 10px}
header h1{margin:0 0 10px}
header p{margin:0}
.notice{text-align:center;padding:12px;background:#e2f2e6}
h2{text-align:center}
.products{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:12px;padding:12px;max-width:850px;margin:auto}

/* پروڈکٹ کارڈ اور کراپنگ کا خاص کوڈ */
.card{background:white;padding:10px;border-radius:12px;text-align:center;box-shadow:0 2px 7px #0001;display:flex;flex-direction:column;align-items:center}
.img-container{width:125px;height:125px;overflow:hidden;position:relative;border-radius:8px;margin-bottom:8px}
.img-container img{position:absolute;width:300%;height:200%;object-fit:cover}

/* تصویر کے 6 حصوں کی پوزیشنز */
.img-flour img { top: 0; left: 0; }
.img-sugar img { top: 0; left: -100%; }
.img-rice img { top: 0; left: -200%; }
.img-ghee img { top: -100%; left: 0; }
.img-lentils img { top: -100%; left: -100%; }
.img-milk img { top: -100%; left: -200%; }

.card h3{margin:8px}
.price{color:#126b3b;font-weight:bold}
input{width:65px;padding:8px}
button{border:0;border-radius:8px;padding:11px;background:#126b3b;color:white;font-size:15px;cursor:pointer}
.add{width:100%;margin-top:8px}
.order{background:white;margin:12px;padding:16px;border-radius:12px}
#cart{line-height:1.8}
.whatsapp{display:block;background:#168c48;color:white;text-align:center;padding:14px;border-radius:8px;text-decoration:none;font-weight:bold}
footer{text-align:center;padding:20px}
@media(min-width:650px){.products{grid-template-columns:repeat(3,minmax(0,1fr))}}
</style>
</head>
<body>

<header>
<h1>🛒 Attiq Utility Store</h1>
<p>آپ کی روزمرہ ضرورت کی اشیاء ایک ہی جگہ</p>
</header>

<div class="notice">خوش آمدید! اپنی پسند کی اشیاء منتخب کریں۔</div>
<h2>ہماری مصنوعات</h2>
<div class="products" id="products"></div>

<section class="order">
<h2>🧺 آپ کا آرڈر</h2>
<div id="cart">ابھی کوئی چیز منتخب نہیں کی۔</div>
<p><b>کل رقم: Rs. <span id="total">0</span></b></p>
<a class="whatsapp" id="send" href="#" target="_blank" rel="noopener">
WhatsApp پر آرڈر بھیجیں
</a>
<p>WhatsApp کھلنے پر پیغام بھیجنے کے لیے Send دبائیں۔</p>
</section>

<footer>Attiq Utility Store ❤️<br>آپ کے اعتماد کا شکریہ</footer>

<script>
// یہاں پر تصویر کا نام "image_fvqZ7x.png" استعمال کیا گیا ہے جو آپ کی مین گریڈ تصویر ہے۔
const products=[
{name:"آٹا",price:120,class:"img-flour",image:"image_fvqZ7x.png"},
{name:"چینی",price:160,class:"img-sugar",image:"image_fvqZ7x.png"},
{name:"چاول",price:150,class:"img-rice",image:"image_fvqZ7x.png"},
{name:"گھی",price:570,class:"img-ghee",image:"image_fvqZ7x.png"},
{name:"دال",price:300,class:"img-lentils",image:"image_fvqZ7x.png"},
{name:"دودھ",price:180,class:"img-milk",image:"image_fvqZ7x.png"}
];

const phone="923476972215";
const cart=[];
const area=document.getElementById("products");

products.forEach((p,i)=>{
const card=document.createElement("div");
card.className="card";

// کراپ کنٹینر بنانا تاکہ صرف مخصوص حصہ نظر آئے
const imgContainer=document.createElement("div");
imgContainer.className="img-container "+p.class;

const img=document.createElement("img");
img.src=p.image;
img.alt=p.name;
imgContainer.appendChild(img);

const name=document.createElement("h3");
name.textContent=p.name;
const price=document.createElement("p");
price.className="price";
price.textContent=p.price>0?"Rs. "+p.price:"قیمت درج کرنا باقی ہے";
const label=document.createElement("label");
label.textContent="تعداد: ";
const qty=document.createElement("input");
qty.type="number";qty.min="1";qty.value="1";qty.id="qty"+i;
const btn=document.createElement("button");
btn.className="add";
btn.textContent="آرڈر میں شامل کریں";
btn.onclick=()=>add(i);
label.appendChild(qty);
card.append(imgContainer,name,price,label,btn);
area.appendChild(card);
});

function add(i){
const p=products[i];
const qty=Number(document.getElementById("qty"+i).value);
if(!Number.isInteger(qty)||qty<1){alert("درست تعداد لکھیں");return}
if(p.price<=0){alert("اس چیز کی اصل قیمت ابھی درج نہیں۔");return}
const old=cart.find(x=>x.i===i);
if(old)old.qty+=qty;
else cart.push({i,qty});
render();
}

function render(){
const box=document.getElementById("cart");
box.replaceChildren();
let total=0;
const lines=["السلام علیکم! مجھے یہ چیزیں آرڈر کرنی ہیں:"];
cart.forEach(x=>{
const p=products[x.i],sum=p.price*x.qty;
total+=sum;
const row=document.createElement("p");
row.textContent=p.name+" — تعداد "+x.qty+" — Rs. "+sum;
box.appendChild(row);
lines.push(p.name+": "+x.qty+" × Rs. "+p.price+" = Rs. "+sum);
});
if(!cart.length)box.textContent="ابھی کوئی چیز منتخب نہیں کی۔";
document.getElementById("total").textContent=total;
lines.push("کل رقم: Rs. "+total);
lines.push("نام اور پتہ: ");
document.getElementById("send").href=
"https://wa.me"+phone+"?text="+encodeURIComponent(lines.join("\n"));
}
</script>
</body>
</html>
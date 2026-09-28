# Footbal-bet

<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Football Prediction</title>

<style>
*{box-sizing:border-box}

body{
    margin:0;
    font-family:Tahoma,Arial,sans-serif;
    background:#071b12;
    color:#fff;
}

header{
    background:#0b2c1d;
    padding:18px;
    text-align:center;
    border-bottom:2px solid #20df7a;
}

header h1{
    margin:0;
    color:#42ff98;
}

.container{
    width:95%;
    max-width:1000px;
    margin:20px auto;
}

.card{
    background:#0d3020;
    border:1px solid #21794b;
    border-radius:16px;
    padding:18px;
    margin-bottom:18px;
    box-shadow:0 7px 25px #0005;
}

input,select,button{
    width:100%;
    padding:13px;
    margin:6px 0;
    border-radius:10px;
    font-size:15px;
}

input,select{
    background:#081f15;
    color:white;
    border:1px solid #27764b;
}

button{
    border:0;
    background:#20df7a;
    color:#032012;
    font-weight:bold;
    cursor:pointer;
}

button:hover{
    background:#4aff9b;
}

.red{
    background:#d83c4b;
    color:white;
}

.gold{
    color:#ffd84a;
}

.hidden{
    display:none;
}

.balance{
    text-align:center;
    font-size:28px;
    color:#ffd84a;
    padding:15px;
}

.nav{
    display:grid;
    grid-template-columns:repeat(3,1fr);
    gap:8px;
    margin-bottom:18px;
}

.match{
    background:#092519;
    border:1px solid #236d47;
    border-radius:14px;
    padding:16px;
    margin:12px 0;
}

.teams{
    display:flex;
    justify-content:space-between;
    align-items:center;
    text-align:center;
    gap:10px;
    font-size:17px;
    font-weight:bold;
}

.team{
    width:40%;
}

.vs{
    color:#42ff98;
}

.options{
    display:grid;
    grid-template-columns:repeat(3,1fr);
    gap:7px;
    margin-top:12px;
}

.options button{
    background:#12472e;
    color:white;
    border:1px solid #288353;
}

.options button.selected{
    background:#20df7a;
    color:#032012;
}

.small{
    color:#aaa;
    font-size:12px;
}

table{
    width:100%;
    border-collapse:collapse;
}

th,td{
    border:1px solid #286b48;
    padding:9px;
    text-align:center;
}

th{
    background:#12482e;
}

#message{
    text-align:center;
    color:#ffd84a;
    min-height:25px;
}

@media(max-width:600px){
    .nav{
        grid-template-columns:1fr;
    }

    .teams{
        font-size:14px;
    }

    .options{
        grid-template-columns:1fr;
    }

    table{
        font-size:12px;
    }
}
</style>
</head>

<body>

<header>
    <h1>⚽ FOOTBALL PREDICTION</h1>
    <div class="small">
        سیستم پیش‌بینی با اعتبار مجازی
    </div>
</header>

<div class="container">

<!-- ================= AUTH ================= -->

<div id="auth">

    <div class="card">
        <h2>📝 ثبت‌نام</h2>

        <input id="regUser"
               placeholder="نام کاربری">

        <input id="regPass"
               type="password"
               placeholder="رمز عبور">

        <button onclick="register()">
            ثبت‌نام
        </button>
    </div>


    <div class="card">
        <h2>🔐 ورود</h2>

        <input id="loginUser"
               placeholder="نام کاربری">

        <input id="loginPass"
               type="password"
               placeholder="رمز عبور">

        <button onclick="login()">
            ورود
        </button>
    </div>


    <div class="card">
        <h2>🛠 ورود مدیر</h2>

        <input id="adminPass"
               type="password"
               placeholder="رمز مدیر">

        <button onclick="adminLogin()">
            ورود به پنل مدیریت
        </button>

        <div class="small">
            رمز پیش‌فرض دمو:
            <b>admin123</b>
        </div>
    </div>

</div>


<!-- ================= USER ================= -->

<div id="userPanel" class="hidden">

    <div class="nav">
        <button onclick="showUserSection('matches')">
            ⚽ مسابقات
        </button>

        <button onclick="showUserSection('history')">
            📋 پیش‌بینی‌های من
        </button>

        <button class="red" onclick="logout()">
            خروج
        </button>
    </div>


    <div class="card">
        <h2>
            سلام
            <span id="username"></span>
            👋
        </h2>

        <div class="balance">
            اعتبار مجازی:
            <span id="balance">20.00</span>
            $
        </div>

        <div class="small" style="text-align:center">
            اعتبار اولیه هر کاربر: ۲۰ دلار مجازی
        </div>
    </div>


<!-- ================= MATCHES ================= -->

<div id="matches">

    <div class="card">

        <h2>⚽ مسابقات امروز</h2>

        <div id="matchesList"></div>

    </div>

</div>


<!-- ================= HISTORY ================= -->

<div id="history" class="hidden">

    <div class="card">

        <h2>📋 تاریخچه پیش‌بینی‌ها</h2>

        <div id="historyList"></div>

    </div>

</div>

</div>


<!-- ================= ADMIN ================= -->

<div id="adminPanel" class="hidden">

    <div class="nav">

        <button onclick="showAdmin('adminUsers')">
            👥 کاربران
        </button>

        <button onclick="showAdmin('adminBets')">
            🎯 پیش‌بینی‌ها
        </button>

        <button onclick="showAdmin('adminMatches')">
            ⚽ مسابقات
        </button>

    </div>


    <!-- USERS -->

    <div id="adminUsers" class="card">

        <h2>👥 کاربران</h2>

        <table>

            <thead>
                <tr>
                    <th>کاربر</th>
                    <th>اعتبار</th>
                    <th>تاریخ ثبت‌نام</th>
                </tr>
            </thead>

            <tbody id="usersTable"></tbody>

        </table>

    </div>


    <!-- BETS -->

    <div id="adminBets" class="card hidden">

        <h2>🎯 پیش‌بینی‌های کاربران</h2>

        <table>

            <thead>
                <tr>
                    <th>کاربر</th>
                    <th>مسابقه</th>
                    <th>انتخاب</th>
                    <th>مبلغ مجازی</th>
                </tr>
            </thead>

            <tbody id="betsTable"></tbody>

        </table>

    </div>


    <!-- MATCHES -->

    <div id="adminMatches" class="card hidden">

        <h2>⚽ افزودن مسابقه</h2>

        <input id="homeTeam"
               placeholder="تیم میزبان">

        <input id="awayTeam"
               placeholder="تیم مهمان">

        <input id="matchTime"
               placeholder="زمان مسابقه">

        <button onclick="addMatch()">
            افزودن مسابقه
        </button>


        <h3>مسابقات موجود</h3>

        <div id="adminMatchList"></div>

    </div>

</div>


<div id="message"></div>

</div>


<script>

/* ==================================================
   CONFIG
================================================== */

const START_BALANCE = 20;

const MIN_PREDICTION = 0.5;

const ADMIN_PASSWORD = "admin123";


/* ==================================================
   DATABASE - DEMO LOCAL STORAGE
================================================== */

function getUsers(){

    return JSON.parse(
        localStorage.getItem("football_users") || "[]"
    );

}

function saveUsers(users){

    localStorage.setItem(
        "football_users",
        JSON.stringify(users)
    );

}


function getBets(){

    return JSON.parse(
        localStorage.getItem("football_bets") || "[]"
    );

}

function saveBets(bets){

    localStorage.setItem(
        "football_bets",
        JSON.stringify(bets)
    );

}


function getMatches(){

    let matches =
        JSON.parse(
            localStorage.getItem("football_matches") || "null"
        );

    if(!matches){

        matches = [

            {
                id:1,
                home:"Barcelona",
                away:"Real Madrid",
                time:"20:30"
            },

            {
                id:2,
                home:"Manchester City",
                away:"Arsenal",
                time:"22:00"
            },

            {
                id:3,
                home:"Liverpool",
                away:"Chelsea",
                time:"23:30"
            }

        ];

        saveMatches(matches);

    }

    return matches;

}


function saveMatches(matches){

    localStorage.setItem(
        "football_matches",
        JSON.stringify(matches)
    );

}


/* ==================================================
   MESSAGE
================================================== */

function message(text){

    document.getElementById("message")
        .innerText = text;

    setTimeout(()=>{

        document.getElementById("message")
            .innerText = "";

    },3000);

}


/* ==================================================
   REGISTER
================================================== */

function register(){

    const username =
        document.getElementById("regUser")
        .value.trim();

    const password =
        document.getElementById("regPass")
        .value;

    if(!username || !password){

        message("نام کاربری و رمز را وارد کنید.");

        return;

    }


    const users = getUsers();


    if(
        users.some(
            user => user.username === username
        )
    ){

        message("این نام کاربری قبلاً وجود دارد.");

        return;

    }


    users.push({

        username:username,

        password:password,

        balance:START_BALANCE,

        created:new Date()
            .toLocaleString("fa-IR")

    });


    saveUsers(users);


    message(
        "ثبت‌نام انجام شد؛ ۲۰ دلار اعتبار مجازی دریافت کردید."
    );


    document.getElementById("regUser").value="";
    document.getElementById("regPass").value="";

}


/* ==================================================
   LOGIN
================================================== */

function login(){

    const username =
        document.getElementById("loginUser")
        .value.trim();

    const password =
        document.getElementById("loginPass")
        .value;


    const users = getUsers();


    const user = users.find(

        u =>
        u.username === username &&
        u.password === password

    );


    if(!user){

        message("اطلاعات ورود صحیح نیست.");

        return;

    }


    localStorage.setItem(
        "football_current_user",
        username
    );


    openUser();

}


/* ==================================================
   OPEN USER
================================================== */

function openUser(){

    const username =
        localStorage.getItem(
            "football_current_user"
        );

    if(!username)return;


    const users = getUsers();


    const user =
        users.find(
            u => u.username === username
        );


    if(!user)return;


    document.getElementById("auth")
        .classList.add("hidden");


    document.getElementById("adminPanel")
        .classList.add("hidden");


    document.getElementById("userPanel")
        .classList.remove("hidden");


    document.getElementById("username")
        .innerText = user.username;


    document.getElementById("balance")
        .innerText =
        Number(user.balance).toFixed(2);


    renderMatches();

    renderHistory();

}


/* ==================================================
   USER SECTIONS
================================================== */

function showUserSection(section){

    document.getElementById("matches")
        .classList.add("hidden");

    document.getElementById("history")
        .classList.add("hidden");


    document.getElementById(section)
        .classList.remove("hidden");

}


/* ==================================================
   RENDER MATCHES
================================================== */

function renderMatches(){

    const matches = getMatches();

    const container =
        document.getElementById("matchesList");

    container.innerHTML="";


    matches.forEach(match=>{

        container.innerHTML += `

        <div class="match">

            <div class="small"
                 style="text-align:center">
                ⏰ ${escapeHTML(match.time)}
            </div>

            <div class="teams">

                <div class="team">
                    ${escapeHTML(match.home)}
                </div>

                <div class="vs">
                    VS
                </div>

                <div class="team">
                    ${escapeHTML(match.away)}
                </div>

            </div>


            <div class="options">

                <button
                    onclick="selectPrediction(
                        ${match.id},
                        'میزبان',
                        this
                    )">
                    برد ${escapeHTML(match.home)}
                </button>

                <button
                    onclick="selectPrediction(
                        ${match.id},
                        'مساوی',
                        this
                    )">
                    مساوی
                </button>

                <button
                    onclick="selectPrediction(
                        ${match.id},
                        'مهمان',
                        this
                    )">
                    برد ${escapeHTML(match.away)}
                </button>

            </div>

        </div>

        `;

    });

}


/* ==================================================
   SELECT PREDICTION
================================================== */

function selectPrediction(
    matchId,
    prediction,
    button
){

    const match =
        getMatches().find(
            m => m.id === matchId
        );


    const amount =
        prompt(
            "مبلغ پیش‌بینی مجازی را وارد کنید:"
        );


    if(amount === null)return;


    const value =
        Number(amount);


    if(
        !Number.isFinite(value) ||
        value < MIN_PREDICTION
    ){

        message(
            "حداقل مبلغ پیش‌بینی ۰٫۵ دلار مجازی است."
        );

        return;

    }


    const username =
        localStorage.getItem(
            "football_current_user"
        );


    const users = getUsers();


    const user =
        users.find(
            u => u.username === username
        );


    if(!user)return;


    if(user.balance < value){

        message("اعتبار مجازی کافی نیست.");

        return;

    }


    const bets = getBets();


    /*
       جلوگیری از چند پیش‌بینی
       برای یک مسابقه توسط یک کاربر
    */

    const already =
        bets.some(
            b =>
            b.username === username &&
            b.matchId === matchId
        );


    if(already){

        message(
            "برای این مسابقه قبلاً پیش‌بینی ثبت کرده‌اید."
        );

        return;

    }


    user.balance -= value;


    bets.push({

        id:Date.now(),

        username:username,

        matchId:matchId,

        home:match.home,

        away:match.away,

        prediction:prediction,

        amount:value,

        created:new Date()
            .toLocaleString("fa-IR")

    });


    saveUsers(users);

    saveBets(bets);


    message(
        "پیش‌بینی شما با اعتبار مجازی ثبت شد."
    );


    openUser();

}


/* ==================================================
   HISTORY
================================================== */

function renderHistory(){

    const username =
        localStorage.getItem(
            "football_current_user"
        );


    const bets =
        getBets().filter(
            b => b.username === username
        );


    const container =
        document.getElementById("historyList");


    if(!bets.length){

        container.innerHTML =
            "<p>هنوز پیش‌بینی‌ای ثبت نکرده‌اید.</p>";

        return;

    }


    container.innerHTML = "";


    bets.forEach(bet=>{

        container.innerHTML += `

        <div class="match">

            <b>
                ${escapeHTML(bet.home)}
                -
                ${escapeHTML(bet.away)}
            </b>

            <p>
                انتخاب:
                <span class="gold">
                    ${escapeHTML(bet.prediction)}
                </span>
            </p>

            <p>
                مبلغ مجازی:
                ${Number(bet.amount).toFixed(2)} $
            </p>

            <div class="small">
                ${escapeHTML(bet.created)}
            </div>

        </div>

        `;

    });

}


/* ==================================================
   ADMIN LOGIN
================================================== */

function adminLogin(){

    const password =
        document.getElementById("adminPass")
        .value;


    if(password !== ADMIN_PASSWORD){

        message("رمز مدیر اشتباه است.");

        return;

    }


    document.getElementById("auth")
        .classList.add("hidden");


    document.getElementById("userPanel")
        .classList.add("hidden");


    document.getElementById("adminPanel")
        .classList.remove("hidden");


    renderAdmin();

}


/* ==================================================
   ADMIN SECTIONS
================================================== */

function showAdmin(id){

    document.getElementById("adminUsers")
        .classList.add("hidden");

    document.getElementById("adminBets")
        .classList.add("hidden");

    document.getElementById("adminMatches")
        .classList.add("hidden");


    document.getElementById(id)
        .classList.remove("hidden");


    renderAdmin();

}


/* ==================================================
   ADMIN RENDER
================================================== */

function renderAdmin(){

    renderUsers();

    renderBets();

    renderAdminMatches();

}


/* ==================================================
   USERS
================================================== */

function renderUsers(){

    const users=getUsers();

    const tbody =
        document.getElementById("usersTable");


    tbody.innerHTML="";


    users.forEach(user=>{

        tbody.innerHTML += `

        <tr>

            <td>
                ${escapeHTML(user.username)}
            </td>

            <td>
                ${Number(user.balance).toFixed(2)} $
            </td>

            <td>
                ${escapeHTML(user.created)}
            </td>

        </tr>

        `;

    });

}


/* ==================================================
   BETS
================================================== */

function renderBets(){

    const bets=getBets();

    const tbody =
        document.getElementById("betsTable");


    tbody.innerHTML="";


    bets.forEach(bet=>{

        tbody.innerHTML += `

        <tr>

            <td>
                ${escapeHTML(bet.username)}
            </td>

            <td>
                ${escapeHTML(bet.home)}
                -
                ${escapeHTML(bet.away)}
            </td>

            <td>
                ${escapeHTML(bet.prediction)}
            </td>

            <td>
                ${Number(bet.amount).toFixed(2)} $
            </td>

        </tr>

        `;

    });

}


/* ==================================================
   ADD MATCH
================================================== */

function addMatch(){

    const home =
        document.getElementById("homeTeam")
        .value.trim();

    const away =
        document.getElementById("awayTeam")
        .value.trim();

    const time =
        document.getElementById("matchTime")
        .value.trim();


    if(!home || !away || !time){

        message(
            "نام دو تیم و زمان مسابقه را وارد کنید."
        );

        return;

    }


    const matches=getMatches();


    matches.push({

        id:Date.now(),

        home:home,

        away:away,

        time:time

    });


    saveMatches(matches);


    document.getElementById("homeTeam").value="";
    document.getElementById("awayTeam").value="";
    document.getElementById("matchTime").value="";


    renderAdminMatches();

    message("مسابقه اضافه شد.");

}


/* ==================================================
   ADMIN MATCHES
================================================== */

function renderAdminMatches(){

    const matches=getMatches();

    const box =
        document.getElementById("adminMatchList");


    box.innerHTML="";


    matches.forEach((match,index)=>{

        box.innerHTML += `

        <div class="match">

            <b>
                ${escapeHTML(match.home)}
                -
                ${escapeHTML(match.away)}
            </b>

            <div class="small">
                زمان: ${escapeHTML(match.time)}
            </div>

            <button
                class="red"
                onclick="deleteMatch(${index})">
                حذف مسابقه
            </button>

        </div>

        `;

    });

}


/* ==================================================
   DELETE MATCH
================================================== */

function deleteMatch(index){

    const matches=getMatches();

    matches.splice(index,1);

    saveMatches(matches);

    renderAdminMatches();

    message("مسابقه حذف شد.");

}


/* ==================================================
   LOGOUT
================================================== */

function logout(){

    localStorage.removeItem(
        "football_current_user"
    );


    document.getElementById("userPanel")
        .classList.add("hidden");


    document.getElementById("adminPanel")
        .classList.add("hidden");


    document.getElementById("auth")
        .classList.remove("hidden");

}


/* ==================================================
   ESCAPE HTML
================================================== */

function escapeHTML(value){

    return String(value)

        .replaceAll("&","&amp;")

        .replaceAll("<","&lt;")

        .replaceAll(">","&gt;")

        .replaceAll('"',"&quot;")

        .replaceAll("'","&#039;");

}


/* ==================================================
   START
================================================== */

if(
    localStorage.getItem(
        "football_current_user"
    )
){

    openUser();

}

</script>
<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Football Prediction Demo</title>

<style>
*{box-sizing:border-box}
body{
 margin:0;
 font-family:Tahoma,Arial,sans-serif;
 background:#071b12;
 color:#fff;
}
header{
 background:#0b2c1d;
 padding:18px;
 text-align:center;
 border-bottom:2px solid #20df7a;
}
header h1{margin:0;color:#42ff98}
.container{width:95%;max-width:1000px;margin:20px auto}
.card{
 background:#0d3020;
 border:1px solid #21794b;
 border-radius:16px;
 padding:18px;
 margin-bottom:18px;
 box-shadow:0 7px 25px #0005;
}
input,button{
 width:100%;
 padding:13px;
 margin:6px 0;
 border-radius:10px;
 font-size:15px;
}
input{
 background:#081f15;
 color:#fff;
 border:1px solid #27764b;
}
button{
 border:0;
 background:#20df7a;
 color:#032012;
 font-weight:bold;
 cursor:pointer;
}
button:hover{background:#4aff9b}
.red{background:#d83c4b;color:white}
.gold{color:#ffd84a}
.hidden{display:none}
.nav{
 display:grid;
 grid-template-columns:repeat(4,1fr);
 gap:8px;
 margin-bottom:18px;
}
.balance{
 text-align:center;
 font-size:27px;
 color:#ffd84a;
 padding:15px;
}
.match{
 background:#092519;
 border:1px solid #236d47;
 border-radius:14px;
 padding:16px;
 margin:12px 0;
}
.teams{
 display:flex;
 justify-content:space-between;
 align-items:center;
 text-align:center;
 gap:10px;
 font-weight:bold;
}
.team{width:40%}
.vs{color:#42ff98}
.options{
 display:grid;
 grid-template-columns:repeat(3,1fr);
 gap:7px;
 margin-top:12px;
}
.options button{
 background:#12472e;
 color:white;
 border:1px solid #288353;
}
table{
 width:100%;
 border-collapse:collapse;
}
th,td{
 border:1px solid #286b48;
 padding:9px;
 text-align:center;
}
th{background:#12482e}
.small{font-size:12px;color:#aaa}
.copybox{
 display:flex;
 gap:7px;
}
.copybox input{margin:0}
.copybox button{width:110px;margin:0}
#message{
 text-align:center;
 color:#ffd84a;
 min-height:25px;
}
@media(max-width:650px){
 .nav{grid-template-columns:1fr 1fr}
 .options{grid-template-columns:1fr}
 .copybox{display:block}
 .copybox button{width:100%;margin-top:6px}
 table{font-size:11px}
}
</style>
</head>

<body>

<header>
<h1>⚽ FOOTBALL PREDICTION</h1>
<div class="small">سیستم دمو با اعتبار مجازی</div>
</header>

<div class="container">

<!-- AUTH -->
<div id="auth">

<div class="card">
<h2>📝 ثبت‌نام</h2>
<input id="regUser" placeholder="نام کاربری">
<input id="regPass" type="password" placeholder="رمز عبور">
<button onclick="register()">ثبت‌نام</button>
</div>

<div class="card">
<h2>🔐 ورود</h2>
<input id="loginUser" placeholder="نام کاربری">
<input id="loginPass" type="password" placeholder="رمز عبور">
<button onclick="login()">ورود</button>
</div>

<div class="card">
<h2>🛠 ورود مدیریت</h2>
<input id="adminPass" type="password" placeholder="رمز مدیر">
<button onclick="adminLogin()">ورود مدیر</button>
</div>

</div>


<!-- USER PANEL -->
<div id="userPanel" class="hidden">

<div class="nav">
<button onclick="showUser('matches')">⚽ مسابقات</button>
<button onclick="showUser('wallet')">💰 کیف پول</button>
<button onclick="showUser('history')">📋 تاریخچه</button>
<button class="red" onclick="logout()">خروج</button>
</div>

<div class="card">
<h2>سلام <span id="username"></span> 👋</h2>

<div class="balance">
اعتبار مجازی:
<span id="balance">20.00</span> $
</div>

<div class="small" style="text-align:center">
این موجودی صرفاً مجازی و آزمایشی است.
</div>
</div>


<!-- MATCHES -->
<div id="matches">

<div class="card">
<h2>⚽ مسابقات</h2>
<div id="matchesList"></div>
</div>

</div>


<!-- WALLET -->
<div id="wallet" class="hidden">

<div class="card">

<h2>📥 واریز آزمایشی USDT</h2>

<p>
شبکه:
<b>BNB Smart Chain (BEP-20)</b>
</p>

<p>
حداقل واریز:
<b class="gold">5 USDT</b>
</p>

<p class="small">
آدرس USDT برای واریز:
</p>

<div class="copybox">
<input
id="depositAddress"
readonly
value="0x3765C083F36B7D874d3a6249436a84C9e9bDAbA6">

<button onclick="copyAddress()">
کپی
</button>
</div>

<input
id="depositAmount"
type="number"
step="0.01"
placeholder="مقدار USDT">

<input
id="depositTx"
placeholder="TXID تراکنش">

<button onclick="submitDeposit()">
ثبت درخواست واریز
</button>

<p class="small">
این نسخه تراکنش را به‌صورت خودکار تأیید نمی‌کند.
</p>

</div>


<div class="card">

<h2>📤 برداشت آزمایشی USDT</h2>

<p>
شبکه:
<b>BNB Smart Chain (BEP-20)</b>
</p>

<p>
حداقل برداشت:
<b class="gold">50 USDT</b>
</p>

<input
id="withdrawAddress"
placeholder="آدرس BSC برای دریافت USDT">

<input
id="withdrawAmount"
type="number"
step="0.01"
placeholder="مقدار USDT">

<button onclick="submitWithdraw()">
ایجاد درخواست برداشت
</button>

<p class="small">
درخواست پس از ثبت در پنل مدیریت نمایش داده می‌شود.
</p>

</div>

</div>


<!-- HISTORY -->
<div id="history" class="hidden">

<div class="card">
<h2>📋 پیش‌بینی‌های من</h2>
<div id="historyList"></div>
</div>

</div>

</div>


<!-- ADMIN -->
<div id="adminPanel" class="hidden">

<div class="nav">
<button onclick="showAdmin('adminUsers')">👥 کاربران</button>
<button onclick="showAdmin('adminBets')">🎯 پیش‌بینی‌ها</button>
<button onclick="showAdmin('adminWithdraws')">📤 برداشت‌ها</button>
<button onclick="showAdmin('adminDeposits')">📥 واریزها</button>
</div>


<!-- USERS -->
<div id="adminUsers" class="card">

<h2>👥 کاربران</h2>

<table>
<thead>
<tr>
<th>کاربر</th>
<th>اعتبار مجازی</th>
<th>تاریخ</th>
</tr>
</thead>
<tbody id="usersTable"></tbody>
</table>

</div>


<!-- BETS -->
<div id="adminBets" class="card hidden">

<h2>🎯 پیش‌بینی‌های کاربران</h2>

<table>
<thead>
<tr>
<th>کاربر</th>
<th>مسابقه</th>
<th>انتخاب</th>
<th>مبلغ</th>
</tr>
</thead>
<tbody id="betsTable"></tbody>
</table>

</div>


<!-- WITHDRAWS -->
<div id="adminWithdraws" class="card hidden">

<h2>📤 درخواست‌های برداشت USDT</h2>

<p class="small">
فقط مدیر این بخش را در پنل مدیریت می‌بیند.
</p>

<table>
<thead>
<tr>
<th>کاربر</th>
<th>مقدار</th>
<th>آدرس BSC</th>
<th>وضعیت</th>
<th>TXID پرداخت</th>
<th>عملیات</th>
</tr>
</thead>

<tbody id="withdrawTable"></tbody>
</table>

</div>


<!-- DEPOSITS -->
<div id="adminDeposits" class="card hidden">

<h2>📥 درخواست‌های واریز USDT</h2>

<table>
<thead>
<tr>
<th>کاربر</th>
<th>مقدار</th>
<th>TXID</th>
<th>وضعیت</th>
<th>عملیات</th>
</tr>
</thead>

<tbody id="depositTable"></tbody>
</table>

</div>

</div>

<div id="message"></div>

</div>


<script>

/* ==============================
CONFIG
============================== */

const START_BALANCE = 20;

const MIN_PREDICTION = 0.5;

const MIN_DEPOSIT = 5;

const MIN_WITHDRAW = 50;

const ADMIN_PASSWORD = "admin123";

const USDT_ADDRESS =
"0x3765C083F36B7D874d3a6249436a84C9e9bDAbA6";


/* ==============================
STORAGE
============================== */

function getUsers(){
return JSON.parse(
localStorage.getItem("fp_users") || "[]"
);
}

function saveUsers(x){
localStorage.setItem(
"fp_users",
JSON.stringify(x)
);
}

function getBets(){
return JSON.parse(
localStorage.getItem("fp_bets") || "[]"
);
}

function saveBets(x){
localStorage.setItem(
"fp_bets",
JSON.stringify(x)
);
}

function getDeposits(){
return JSON.parse(
localStorage.getItem("fp_deposits") || "[]"
);
}

function saveDeposits(x){
localStorage.setItem(
"fp_deposits",
JSON.stringify(x)
);
}

function getWithdraws(){
return JSON.parse(
localStorage.getItem("fp_withdraws") || "[]"
);
}

function saveWithdraws(x){
localStorage.setItem(
"fp_withdraws",
JSON.stringify(x)
);
}

function getMatches(){

let x=JSON.parse(
localStorage.getItem("fp_matches") || "null"
);

if(!x){

x=[
{
id:1,
home:"Barcelona",
away:"Real Madrid",
time:"20:30"
},
{
id:2,
home:"Manchester City",
away:"Arsenal",
time:"22:00"
},
{
id:3,
home:"Liverpool",
away:"Chelsea",
time:"23:30"
}
];

saveMatches(x);

}

return x;

}

function saveMatches(x){
localStorage.setItem(
"fp_matches",
JSON.stringify(x)
);
}


/* ==============================
MESSAGE
============================== */

function msg(t){

document.getElementById("message")
.innerText=t;

setTimeout(()=>{
document.getElementById("message")
.innerText="";
},3000);

}


/* ==============================
REGISTER
============================== */

function register(){

let username=
document.getElementById("regUser").value.trim();

let password=
document.getElementById("regPass").value;

if(!username || !password){

msg("نام کاربری و رمز را وارد کنید.");

return;
}

let users=getUsers();

if(users.some(x=>x.username===username)){

msg("این نام کاربری قبلاً ثبت شده است.");

return;
}

users.push({

username,
password,

balance:START_BALANCE,

created:new Date()
.toLocaleString("fa-IR")

});

saveUsers(users);

msg("ثبت‌نام انجام شد؛ ۲۰ دلار اعتبار مجازی اضافه شد.");

}


/* ==============================
LOGIN
============================== */

function login(){

let username=
document.getElementById("loginUser")
.value.trim();

let password=
document.getElementById("loginPass")
.value;

let user=getUsers().find(
x=>x.username===username &&
x.password===password
);

if(!user){

msg("نام کاربری یا رمز اشتباه است.");

return;
}

localStorage.setItem(
"fp_current",
username
);

openUser();

}


/* ==============================
OPEN USER
============================== */

function openUser(){

let username=
localStorage.getItem("fp_current");

if(!username)return;

let user=getUsers().find(
x=>x.username===username
);

if(!user)return;

document.getElementById("auth")
.classList.add("hidden");

document.getElementById("adminPanel")
.classList.add("hidden");

document.getElementById("userPanel")
.classList.remove("hidden");

document.getElementById("username")
.innerText=user.username;

document.getElementById("balance")
.innerText=
Number(user.balance).toFixed(2);

renderMatches();

renderHistory();

}


/* ==============================
USER NAV
============================== */

function showUser(id){

["matches","wallet","history"]
.forEach(x=>{
document.getElementById(x)
.classList.add("hidden");
});

document.getElementById(id)
.classList.remove("hidden");

}


/* ==============================
MATCHES
============================== */

function renderMatches(){

let matches=getMatches();

let box=
document.getElementById("matchesList");

box.innerHTML="";

matches.forEach(m=>{

box.innerHTML+=`

<div class="match">

<div class="small"
style="text-align:center">
⏰ ${esc(m.time)}
</div>

<div class="teams">

<div class="team">
${esc(m.home)}
</div>

<div class="vs">
VS
</div>

<div class="team">
${esc(m.away)}
</div>

</div>

<div class="options">

<button onclick="makePrediction(
${m.id},
'میزبان'
)">
برد ${esc(m.home)}
</button>

<button onclick="makePrediction(
${m.id},
'مساوی'
)">
مساوی
</button>

<button onclick="makePrediction(
${m.id},
'مهمان'
)">
برد ${esc(m.away)}
</button>

</div>

</div>

`;

});

}


/* ==============================
PREDICTION
============================== */

function makePrediction(matchId,prediction){

let amount=Number(
prompt(
"مبلغ پیش‌بینی مجازی را وارد کنید:"
)
);

if(!Number.isFinite(amount) ||
amount<MIN_PREDICTION){

msg("حداقل پیش‌بینی ۰٫۵ دلار مجازی است.");

return;
}

let username=
localStorage.getItem("fp_current");

let users=getUsers();

let user=users.find(
x=>x.username===username
);

if(!user)return;

if(user.balance<amount){

msg("اعتبار مجازی کافی نیست.");

return;
}

let bets=getBets();

if(
bets.some(
x=>x.username===username &&
x.matchId===matchId
)
){

msg("برای این مسابقه قبلاً پیش‌بینی کرده‌اید.");

return;
}

let match=getMatches().find(
x=>x.id===matchId
);

user.balance-=amount;

bets.push({

id:Date.now(),

username,

matchId,

home:match.home,

away:match.away,

prediction,

amount,

created:new Date()
.toLocaleString("fa-IR")

});

saveUsers(users);
saveBets(bets);

msg("پیش‌بینی با اعتبار مجازی ثبت شد.");

openUser();

}


/* ==============================
HISTORY
============================== */

function renderHistory(){

let username=
localStorage.getItem("fp_current");

let bets=getBets()
.filter(x=>x.username===username);

let box=
document.getElementById("historyList");

if(!bets.length){

box.innerHTML=
"<p>هنوز پیش‌بینی‌ای ثبت نشده است.</p>";

return;
}

box.innerHTML="";

bets.forEach(b=>{

box.innerHTML+=`

<div class="match">

<b>
${esc(b.home)} -
${esc(b.away)}
</b>

<p>
انتخاب:
<span class="gold">
${esc(b.prediction)}
</span>
</p>

<p>
مبلغ:
${Number(b.amount).toFixed(2)} $
</p>

<div class="small">
${esc(b.created)}
</div>

</div>

`;

});

}


/* ==============================
COPY USDT ADDRESS
============================== */

function copyAddress(){

navigator.clipboard
.writeText(USDT_ADDRESS)
.then(()=>{
msg("آدرس USDT کپی شد.");
})
.catch(()=>{
msg("کپی خودکار انجام نشد؛ آدرس را دستی کپی کنید.");
});

}


/* ==============================
DEPOSIT REQUEST
============================== */

function submitDeposit(){

let amount=Number(
document.getElementById("depositAmount")
.value
);

let txid=
document.getElementById("depositTx")
.value.trim();

let username=
localStorage.getItem("fp_current");

if(amount<MIN_DEPOSIT){

msg("حداقل واریز ۵ USDT است.");

return;
}

if(!txid){

msg("TXID را وارد کنید.");

return;
}

let deposits=getDeposits();

deposits.push({

id:Date.now(),

username,

amount,

txid,

network:"BNB Smart Chain",

status:"pending",

created:new Date()
.toLocaleString("fa-IR")

});

saveDeposits(deposits);

msg("درخواست واریز ثبت شد.");

document.getElementById("depositAmount").value="";
document.getElementById("depositTx").value="";

}


/* ==============================
WITHDRAW REQUEST
============================== */

function submitWithdraw(){

let address=
document.getElementById("withdrawAddress")
.value.trim();

let amount=Number(
document.getElementById("withdrawAmount")
.value
);

let username=
localStorage.getItem("fp_current");

if(!address){

msg("آدرس BSC را وارد کنید.");

return;
}

if(amount<MIN_WITHDRAW){

msg("حداقل برداشت ۵۰ USDT است.");

return;
}

let withdraws=getWithdraws();

withdraws.push({

id:Date.now(),

username,

address,

amount,

network:"BNB Smart Chain",

status:"pending",

txid:"",

created:new Date()
.toLocaleString("fa-IR")

});

saveWithdraws(withdraws);

msg("درخواست برداشت ثبت شد.");

document.getElementById("withdrawAddress").value="";
document.getElementById("withdrawAmount").value="";

}


/* ==============================
ADMIN LOGIN
============================== */

function adminLogin(){

let pass=
document.getElementById("adminPass").value;

if(pass!==ADMIN_PASSWORD){

msg("رمز مدیر اشتباه است.");

return;
}

document.getElementById("auth")
.classList.add("hidden");

document.getElementById("userPanel")
.classList.add("hidden");

document.getElementById("adminPanel")
.classList.remove("hidden");

renderAdmin();

}


/* ==============================
ADMIN NAV
============================== */

function showAdmin(id){

[
"adminUsers",
"adminBets",
"adminWithdraws",
"adminDeposits"
].forEach(x=>{
document.getElementById(x)
.classList.add("hidden");
});

document.getElementById(id)
.classList.remove("hidden");

renderAdmin();

}


/* ==============================
ADMIN RENDER
============================== */

function renderAdmin(){

renderAdminUsers();
renderAdminBets();
renderAdminWithdraws();
renderAdminDeposits();

}


/* ==============================
ADMIN USERS
============================== */

function renderAdminUsers(){

let users=getUsers();

let box=
document.getElementById("usersTable");

box.innerHTML="";

users.forEach(u=>{

box.innerHTML+=`

<tr>

<td>${esc(u.username)}</td>

<td>
${Number(u.balance).toFixed(2)} $
</td>

<td>
${esc(u.created)}
</td>

</tr>

`;

});

}


/* ==============================
ADMIN BETS
============================== */

function renderAdminBets(){

let bets=getBets();

let box=
document.getElementById("betsTable");

box.innerHTML="";

bets.forEach(b=>{

box.innerHTML+=`

<tr>

<td>${esc(b.username)}</td>

<td>
${esc(b.home)} -
${esc(b.away)}
</td>

<td>${esc(b.prediction)}</td>

<td>
${Number(b.amount).toFixed(2)} $
</td>

</tr>

`;

});

}


/* ==============================
ADMIN WITHDRAWS
============================== */

function renderAdminWithdraws(){

let data=getWithdraws();

let box=
document.getElementById("withdrawTable");

box.innerHTML="";

data.forEach((w,i)=>{

let action="";

if(w.status==="pending"){

action=`

<button onclick="markPaid(${i})">
ثبت پرداخت
</button>

<button class="red"
onclick="cancelWithdraw(${i})">
لغو
</button>

`;

}

box.innerHTML+=`

<tr>

<td>${esc(w.username)}</td>

<td>${Number(w.amount).toFixed(2)} USDT</td>

<td style="word-break:break-all">
${esc(w.address)}
</td>

<td>${esc(w.status)}</td>

<td style="word-break:break-all">
${esc(w.txid || "-")}
</td>

<td>
${action}
</td>

</tr>

`;

});

}


/* ==============================
MARK PAID
============================== */

function markPaid(index){

let txid=prompt(
"TXID پرداخت را وارد کنید:"
);

if(!txid)return;

let data=getWithdraws();

data[index].status="paid";

data[index].txid=txid;

saveWithdraws(data);

renderAdmin();

}


/* ==============================
CANCEL WITHDRAW
============================== */

function cancelWithdraw(index){

let data=getWithdraws();

if(!data[index])return;

data[index].status="cancelled";

saveWithdraws(data);

renderAdmin();

}


/* ==============================
ADMIN DEPOSITS
============================== */

function renderAdminDeposits(){

let data=getDeposits();

let box=
document.getElementById("depositTable");

box.innerHTML="";

data.forEach((d,i)=>{

let action="";

if(d.status==="pending"){

action=`

<button onclick="approveDeposit(${i})">
تأیید
</button>

<button class="red"
onclick="rejectDeposit(${i})">
رد
</button>

`;

}

box.innerHTML+=`

<tr>

<td>${esc(d.username)}</td>

<td>${Number(d.amount).toFixed(2)} USDT</td>

<td style="word-break:break-all">
${esc(d.txid)}
</td>

<td>${esc(d.status)}</td>

<td>${action}</td>

</tr>

`;

});

}


/* ==============================
DEPOSIT APPROVE
============================== */

function approveDeposit(index){

let data=getDeposits();

data[index].status="approved";

saveDeposits(data);

renderAdmin();

}


/* ==============================
DEPOSIT REJECT
============================== */

function rejectDeposit(index){

let data=getDeposits();

data[index].status="rejected";

saveDeposits(data);

renderAdmin();

}


/* ==============================
ESCAPE
============================== */

function esc(v){

return String(v)
.replaceAll("&","&amp;")
.replaceAll("<","&lt;")
.replaceAll(">","&gt;")
.replaceAll('"',"&quot;")
.replaceAll("'","&#039;");

}


/* ==============================
LOGOUT
============================== */

function logout(){

localStorage.removeItem("fp_current");

document.getElementById("userPanel")
.classList.add("hidden");

document.getElementById("adminPanel")
.classList.add("hidden");

document.getElementById("auth")
.classList.remove("hidden");

}


/* ==============================
START
============================== */

if(
localStorage.getItem("fp_current")
){
openUser();
}

</script>

</body>
</html>
</body>
</html>
پیشبینی بازیهای فوتبال

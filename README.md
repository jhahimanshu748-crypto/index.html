# index.html<!DOCTYPE html>
<html>
<head>
<title>Guroo-Chelaa</title>

<style>
body{
    background:#9b0000;
    font-family:Arial;
    margin:0;
    padding:0;
    color:white;
}

header{
    background:#7a0000;
    padding:15px;
    text-align:center;
}

.container{
    width:90%;
    max-width:600px;
    margin:auto;
    margin-top:20px;
}

.card{
    background:white;
    color:black;
    padding:15px;
    border-radius:12px;
    margin-bottom:15px;
}

textarea{
    width:100%;
    height:80px;
    border-radius:8px;
    border:1px solid #ccc;
    padding:10px;
}

button{
    background:#9b0000;
    color:white;
    border:none;
    padding:8px 12px;
    border-radius:6px;
    cursor:pointer;
}

.comment{
    background:#f1f1f1;
    margin-top:5px;
    padding:5px;
    border-radius:5px;
    font-size:14px;
}

.admin{
    color:red;
    cursor:pointer;
    float:right;
}
</style>
</head>

<body>

<header>
<h1>Guroo-Chelaa</h1>
<p>Founded by Himanshu Vishal</p>
<p>Share your thoughts with the world 🌍</p>
</header>

<div class="container">

<div class="card">
<h3>Write your thought</h3>
<textarea id="postText" placeholder="Write something..."></textarea><br><br>
<button onclick="addPost()">Post</button>
</div>

<div id="posts"></div>

</div>

<script>
let adminPassword="guru123";   // You can change this

function addPost(){
    let text=document.getElementById("postText").value;
    if(text=="") return;

    let post={
        text:text,
        likes:0,
        comments:[]
    };

    let data=JSON.parse(localStorage.getItem("guroo"))||[];
    data.unshift(post);
    localStorage.setItem("guroo",JSON.stringify(data));
    document.getElementById("postText").value="";
    showPosts();
}

function showPosts(){
    let data=JSON.parse(localStorage.getItem("guroo"))||[];
    let html="";
    data.forEach((p,i)=>{
        html+=
        <div class="card">
            ${p.text}
            <br><br>
            ❤️ ${p.likes}
            <button onclick="likePost(${i})">Like</button>
            <span class="admin" onclick="deletePost(${i})">Delete</span>
            <br><br>
            <input id="c${i}" placeholder="Comment..." />
            <button onclick="addComment(${i})">Send</button>
        ;

        p.comments.forEach(c=>{
            html+=<div class="comment">${c}</div>;
        });

        html+="</div>";
    });
    document.getElementById("posts").innerHTML=html;
}

function likePost(i){
    let data=JSON.parse(localStorage.getItem("guroo"));
    data[i].likes++;
    localStorage.setItem("guroo",JSON.stringify(data));
    showPosts();
}

function addComment(i){
    let box=document.getElementById("c"+i);
    if(box.value=="") return;
    let data=JSON.parse(localStorage.getItem("guroo"));
    data[i].comments.push(box.value);
    localStorage.setItem("guroo",JSON.stringify(data));
    showPosts();
}

function deletePost(i){
    let pass=prompt("Admin password:");
    if(pass==adminPassword){
        let data=JSON.parse(localStorage.getItem("guroo"));
        data.splice(i,1);
        localStorage.setItem("guroo",JSON.stringify(data));
        showPosts();
    } else {
        alert("Wrong password!");
    }
}

showPosts();
</script>

</body>
</html>

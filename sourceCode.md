# FallDetectionSystem
Basic fall detection system using runlinc

Html
<h1 style="font-size=100px; font-weight: normal;"> Fall Detecting System</h1>
<h3>Client tracking and sensing page </h3>
<div class="regBox" id="regBox" >
<div class="regBox-content" id="content">
<h2 style="font-size=30px; font-weight=bold;">Client's information</h2>
<br>
<form name="myForm" action="#"  method="post">
  <label for="fname">First name:</label>
  <input type="text" id="fname" name="fname"><br>
  <label for="lname">Last name:</label>
  <input type="text" id="lname" name="lname"><br>
<label for="lname">Emergency email</label>
  <input type="eemail" id="eemail" name="email">
</form>
  <input type="button" value="Start device" onClick=
"starting()">
</form> 
</div>  
<div class="sensor" id="sensor">Detection system is off</div> 
<div class="decison" id="decs">
<span id="dec"></span>
<span id="dec1"></span>
<span id="dec2"></span>
</div>
<br/><br/>
</div>      
<script src="https://falldetector00.pagekite.me/mainScript.js" type="text/javascript"></script>
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/emailjs-com@2/dist/email.min.js"></script>
    <script type="text/javascript">
    (function() {
    emailjs.init("user_BsGNckOP56zVgEIHsrDMu");
    })();
    </script>

CSS
body{
background-image: url("https://www.linkpicture.com/q/backgroung.jpg");
background-color: maroon;
  height: 500px;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  position: relative; 
text-align: center;
color: white;
font-family:Copperplate, Papyrus, fantasy	
;
}



.regBox {
  background-color: white;
  width: 300px;
  border: 5px solid white;
  padding: 100px;
  margin: 20px;
margin-left: auto;
    margin-right: auto 
}
.regBox-content{

  text-align:justify;
color:black;
}
.sensor{
color:Red;

}
.decision{

Font-size:30px
color: white;
}
#dec{
display:block;
font-size:20px;}
#dec1{
display:inline;
}
#dec2{
display:inline;
}
Javascript
var Anet= 5;
var start=0;
var fullName= "";
var clicked=false;
var emergencyEmail="";
function starting(){
var fname= document.getElementById("fname").value;
var eemail= document.getElementById("eemail").value;
var lname= document.getElementById("lname").value;
emergencyEmail=eemail;
fullName=fname + " " + lname;
if(fname=="" || lname=="" || eemail==""){alert(' information missing');}
else{
document.getElementById("content").remove();
document.getElementById("sensor").style.color="White";
document.getElementById("regBox").style.backgroundImage = "url(https://media.giphy.com/media/2i7jspnRBYgg6v4Oki/giphy.gif)";
document.getElementById("sensor").innerHTML=`<h2>`+"Detection system Activated"+`</h2><br>`+"Name: "+fullName+`<Br>`+"Started on:"  + new Date();;
 getvalue();
}}
function getvalue(){ start=1;}
function getLocation() {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(showPosition);
            }
        }

        function showPosition(position) {
      maplink(position.coords.latitude,position.coords.longitude)
        }
function maplink(x,y){
    var map="<a href=https://www.google.com/maps/search/?api=1&query=" + x + "," + y +
                "</a>"+"is fall detected area";
    var templateParams = {
                user_Email:emergencyEmail ,
                 message: map
            };
            emailjs.send("service_7mzj1ej", "template_jkz3xru", templateParams).then(function(response) {
                alert('Email sent');
            }, function(error) {
                alert('Connection error, fail to send email. Please refresh page and try again');
            });
        }
   function fallDetected(){
clicked=true;
start=0;
turnOn(Buzzer);
                 const speech = new SpeechSynthesisUtterance("Dont panic.."+fullName +" you had a fall, Emergency is on the way.Hold on!");
             window.speechSynthesis.speak(speech);
document.getElementById("dec").innerHTML="Fall recorded on:"+ new Date()
document.getElementById("dec2").style.display="none";
document.getElementById("dec1").style.display="none";
getLocation();
            }
function checkFall(){

document.getElementById("dec").innerHTML="Did you fall?";
document.getElementById("dec1").innerHTML='<button onClick="fallDetected()">YES</button>';
document.getElementById("dec2").innerHTML='<button onClick="restart()">N0</button>';
var timer = setTimeout(function() {
if(clicked){
 clearTimeout(timer);
}else{
  fallDetected()
start=0;
}
}, 10000);

}

function restart(){
document.getElementById("dec2").style.display="none";
document.getElementById("dec1").style.display="none";
document.getElementById("dec").style.display="none"
clicked=true;
start=1;
}


JS Loop
if (start==1){
window.ondevicemotion = function (event) {

var Ax = event.accelerationIncludingGravity.x
var Ay= event.accelerationIncludingGravity.y
var Az = event.accelerationIncludingGravity.z
var Anet= Math.sqrt(Math.abs(Ax*Ax+Ay*Ay+Az*Az))

	

if(Anet==0){
    start=1;
}else if (Anet<1){
checkFall();
}
}

            }
